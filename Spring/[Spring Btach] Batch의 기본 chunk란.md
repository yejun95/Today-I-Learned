# Chunk 방식이란?
- Spring Batch 처리에서 데이터를 작은 단위로 나누어 처리하는 방식
  - HTTP 전송시에도 동일한 개념으로 사용한다.
<br>

- 작업 할 때 각 커밋 사이에 처리되는 row 수를 얘기한다.

- 한 번에 하나씩 아이템을 입력 받아 Chunk 단위의 덩어리로 만든 후 Chunk 단위로 트랜잭션을 처리
  - !! Chunk 단위로 트랜잭션을 수행하기 때문에 실패할 경우엔 해당 Chunk 만큼만 롤백이 되고, 이전에 커밋된 트랜잭션 범위까지는 반영이 됨
<br>

- Chunk 지향 처리가 결국 Chunk 단위로 데이터를 처리한다는 의미이기 때문에 그림으로 표현하면 아래와 같다.
<img width="750" height="460" alt="image" src="https://github.com/user-attachments/assets/c0d7ea78-f9f9-49ea-be4f-8f329a9e6474" /><br>
> Reader에서 데이터를 하나 읽어온다.<br>
> 읽어온 데이터를 Processor에서 가공한다.<br>
> 가공된 데이터들을 별도의 공간에 모은 뒤, Chunk 단위만큼 쌓이게 되면 Writer에 전달하고 Writer는 일괄 저장한다.
<br>

- **Reader와 Processor에서는 1건씩 다뤄지고, Writer에선 Chunk 단위로 처리된다는 것이다.**
<br>
<hr>
<br>

## ✔️ 기본개념
- Chunk : 처리 단위

- Chunk Size (Batch Size) : 한 번에 처리할 레코드 수

- Transaction Boundary : Chunk 단위로 트랜잭션 관리

- 아래는 Chunk 지향 처리를 Java 코드로 표현한 것
```java
// Chunk 단위 트랜잭션 처리 과정
@Transactional
public void processChunk(List<Order> orders) {
    // Chunk 시작 (트랜잭션 시작)
    try {
        for (Order order : orders) {
            // 1. Reader: 1건씩 읽기
            Order processedOrder = processor.process(order);
            // 2. Processor: 1건씩 가공
            processedOrders.add(processedOrder);
        }
        // 3. Writer: Chunk 단위 일괄 저장
        writer.write(processedOrders);
        
        // Chunk 완료 (커밋)
        // ✅ 이전 Chunk들은 이미 커밋됨
        // ✅ 실패 시 현재 Chunk만 롤백
        
    } catch (Exception e) {
        // 현재 Chunk만 롤백, 이전 Chunk들은 유지
        throw e;
    }
}
```
<br>

### 실패 시나리오 예시
```java
// 10,000건 데이터를 Chunk Size 1000으로 처리하는 경우
// Chunk 1: 1-1000번째 데이터 → ✅ 성공 (커밋됨)
// Chunk 2: 1001-2000번째 데이터 → ✅ 성공 (커밋됨)  
// Chunk 3: 2001-3000번째 데이터 → ❌ 실패 (롤백됨)
// Chunk 4: 3001-4000번째 데이터 → 처리되지 않음

// 재시작 시: Chunk 3부터 다시 시작 (1-2000번째는 이미 처리됨)
```
<br>
<hr>
<br>

## ✔️ Spring Batch 내부 처리 흐름
```java
// Spring Batch 내부에서 실제로 수행되는 로직
public class ChunkProcessor {
    
    public void process(ChunkContext chunkContext) {
        List<Object> items = new ArrayList<>();
        int chunkSize = chunkContext.getStepContext()
            .getStepExecution().getStepContribution().getStepExecution()
            .getJobParameters().getLong("chunk.size", 1000L);
            
        // 1. Reader 단계: 1건씩 읽기
        for (int i = 0; i < chunkSize; i++) {
            Object item = itemReader.read();
            if (item == null) break; // 데이터 끝
            
            // 2. Processor 단계: 1건씩 가공
            Object processedItem = itemProcessor.process(item);
            items.add(processedItem);
        }
        
        // 3. Writer 단계: Chunk 단위로 일괄 처리
        if (!items.isEmpty()) {
            itemWriter.write(items);
            // 트랜잭션 커밋 발생
        }
    }
}
```

## ✔️ 왜 Chunk 방식을 사용할까?
### 메모리 효율성
```java
// 메모리 사용량 비교
public class MemoryUsageExample {
    
    // ❌ 전체 데이터 로딩 방식
    public void badApproach() {
        List<Order> allOrders = orderRepository.findAll(); // 100만 건
        // 메모리 사용량: 100만 건 × 평균 1KB = 1GB
        // OutOfMemoryError 위험!
    }
    
    // ✅ Chunk 방식
    public void goodApproach() {
        int chunkSize = 1000;
        int totalProcessed = 0;
        
        while (true) {
            List<Order> chunk = orderRepository.findWithPaging(
                totalProcessed, chunkSize);
            if (chunk.isEmpty()) break;
            
            // 메모리 사용량: 1000건 × 평균 1KB = 1MB
            processChunk(chunk);
            totalProcessed += chunk.size();
        }
    }
}
```
<br>
<hr>
<br>

## ✔️ 주의사항
### 오버헤드 문제
```java
// ❌ Chunk Size가 너무 작은 경우
@Bean
public Step smallChunkStep() {
    return stepBuilderFactory.get("smallChunkStep")
        .<Order, Order>chunk(10) // 너무 작음
        .reader(orderReader)
        .processor(orderProcessor)
        .writer(orderWriter)
        .build();
}
// 문제점:
// - 트랜잭션 시작/커밋 오버헤드가 큼
// - 네트워크 호출 횟수 증가
// - 전체 처리 시간 증가
```
<br>
<br>

### 메모리 사용량 증가
```java
// ❌ Chunk Size가 너무 큰 경우
@Bean
public Step largeChunkStep() {
    return stepBuilderFactory.get("largeChunkStep")
        .<Order, Order>chunk(50000) // 너무 큼
        .reader(orderReader)
        .processor(orderProcessor)
        .writer(orderWriter)
        .build();
}
// 문제점:
// - OutOfMemoryError 위험
// - GC 압박 증가
// - 처리 시간 불균등
```
<br>
<br>

### 트랜잭션 경계 불일치
```java
// ❌ 잘못된 트랜잭션 관리
@Service
public class OrderService {
    
    @Transactional // Chunk 트랜잭션과 충돌
    public void processOrder(Order order) {
        orderRepository.save(order);
        // 별도 트랜잭션으로 인한 일관성 문제
    }
}

@Component
public class OrderProcessor implements ItemProcessor<Order, ProcessedOrder> {
    
    @Autowired
    private OrderService orderService;
    
    @Override
    public ProcessedOrder process(Order order) throws Exception {
        // ❌ 별도 트랜잭션에서 처리
        orderService.processOrder(order);
        return processedOrder;
    }
}
```
<br>
<br>

### 동시성 문제
```java
// ❌ 동시 실행 시 데이터 충돌
@Bean
public Job concurrentJob() {
    return jobBuilderFactory.get("concurrentJob")
        .start(step1()) // 동시 실행
        .start(step2()) // 동시 실행
        .build();
}

@Bean
public Step step1() {
    return stepBuilderFactory.get("step1")
        .<Order, Order>chunk(1000)
        .reader(orderReader) // 같은 테이블 조회
        .processor(orderProcessor)
        .writer(orderWriter)
        .build();
}

@Bean
public Step step2() {
    return stepBuilderFactory.get("step2")
        .<Order, Order>chunk(1000)
        .reader(orderReader) // 같은 테이블 조회
        .processor(orderProcessor)
        .writer(orderWriter)
        .build();
}
// 문제점:
// - 같은 데이터 중복 처리
// - 데드락 발생 가능성
// - 데이터 무결성 위반
```
<br>
<hr>
<br>

## ✔️ 정리
### 주의사항
- Chunk Size 최적화 필수
- 메모리 누수 주의
- 트랜잭션 타임아웃 방지
- 부분 실패 시 일관성 보장
- 성능 지표 수집
<br>
<br>

### 권장사항
- 정기적인 성능 모니터링
- 환경별 최적화
- 포괄적 에러 처리
- 상세 로깅 체계
- 재시작 안전성 확보
<br>
<hr>
<br>

**Reference**<br>

[Kevin : Batch 기본 Chunk 방식](https://velog.io/@kevin_/Batch-%EA%B8%B0%EB%B3%B8-Chunk-%EB%B0%A9%EC%8B%9D)<br>
[향로 : Spring Batch 가이드 - Chunk 지향 처리](https://jojoldu.tistory.com/331)<br>
