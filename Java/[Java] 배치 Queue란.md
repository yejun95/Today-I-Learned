# 배치 큐란
- 배치 큐는 기본적으로 자바의 `Queue` 인터페이스를 통한 배치를 구현하는 것을 말한다.

- 개별 작업들을 일정한 크기나 시간 간격으로 묶어서 한 번에 처리하는 큐 시스템이다.

- 실시간 처리의 반대 개념으로, 지연을 감수하고 처리량을 극대화하는 패턴
<br>
<hr>
<br>

## ✔️ 언제 사용해야 할까?
### 1. 처리량이 응답시간보다 중요한 경우
```java
// ❌ 개별 처리 - 느림
public void processIndividual(List<Data> dataList) {
    for (Data data : dataList) {
        database.insert(data); // 1000개 = 1000번 DB 호출
    }
}

// ✅ 배치 처리 - 빠름
public void processBatch(List<Data> dataList) {
    database.insertBatch(dataList); // 1000개 = 1번 DB 호출
}
```
<br>

- 위에서 실행한 `insert`, `insertBatch`에 대한 상세 예시
```java
// 개별 처리 구현
public class IndividualDatabaseProcessor {
    private Connection connection;
    
    public void insert(Data data) throws SQLException {
        String sql = "INSERT INTO data_table (id, name, value, created_at) VALUES (?, ?, ?, ?)";
        
        try (PreparedStatement stmt = connection.prepareStatement(sql)) {
            stmt.setInt(1, data.getId());
            stmt.setString(2, data.getName());
            stmt.setString(3, data.getValue());
            stmt.setTimestamp(4, new Timestamp(data.getCreatedAt()));
            
            stmt.executeUpdate(); // 매번 실행
        }
    }
    
    // 1000개 데이터 처리 시:
    // - 1000번의 prepareStatement() 호출
    // - 1000번의 executeUpdate() 호출
    // - 1000번의 자원 해제
}

// 배치 처리 구현
public class BatchDatabaseProcessor {
    private Connection connection;
    
    public void insertBatch(List<Data> dataList) throws SQLException {
        String sql = "INSERT INTO data_table (id, name, value, created_at) VALUES (?, ?, ?, ?)";
        
        try (PreparedStatement stmt = connection.prepareStatement(sql)) {
            connection.setAutoCommit(false); // 트랜잭션 시작
            
            for (Data data : dataList) {
                stmt.setInt(1, data.getId());
                stmt.setString(2, data.getName());
                stmt.setString(3, data.getValue());
                stmt.setTimestamp(4, new Timestamp(data.getCreatedAt()));
                
                stmt.addBatch(); // 배치에 추가 (실행하지 않음)
            }
            
            stmt.executeBatch(); // 한 번에 실행
            connection.commit(); // 트랜잭션 커밋
        }
    }
    
    // 1000개 데이터 처리 시:
    // - 1번의 prepareStatement() 호출
    // - 1000번의 addBatch() 호출 (메모리 작업)
    // - 1번의 executeBatch() 호출
    // - 1번의 자원 해제
}
```
<br>
<br>

### 2. 시스템 리소스가 제한적인 환경
```java
// 메모리 부족한 환경에서의 로그 처리
public class LogBatchProcessor {
    private final int BATCH_SIZE = 1000;
    private final long BATCH_TIMEOUT = 5000; // 5초
    
    private Queue<LogEntry> logQueue = new LinkedList<>();
    
    public void addLog(LogEntry log) {
        synchronized (logQueue) {
            logQueue.offer(log);
            
            // 배치 크기 도달 시 즉시 처리
            if (logQueue.size() >= BATCH_SIZE) {
                processBatch();
            }
        }
    }
    
    private void processBatch() {
        List<LogEntry> batch = new ArrayList<>();
        synchronized (logQueue) {
            int count = Math.min(BATCH_SIZE, logQueue.size());
            for (int i = 0; i < count; i++) {
                batch.add(logQueue.poll());
            }
        }
        
        if (!batch.isEmpty()) {
            writeToFile(batch); // 한 번에 파일 쓰기
        }
    }
}
```
<br>
<br>

### 3. 외부 API 호출 비용이 높은 경우
```java
// 개별 API 호출 vs 배치 API 호출
public class ExternalApiProcessor {
    private final int BATCH_SIZE = 100;
    private Queue<ApiRequest> requestQueue = new LinkedList<>();
    
    // ❌ 개별 호출 - 비용 높음
    public void callApiIndividually(List<ApiRequest> requests) {
        for (ApiRequest request : requests) {
            externalApi.call(request); // 100번 호출 = 100 * $0.01 = $1.00
        }
    }
    
    // ✅ 배치 호출 - 비용 절약
    public void callApiBatch(List<ApiRequest> requests) {
        List<List<ApiRequest>> batches = partition(requests, BATCH_SIZE);
        for (List<ApiRequest> batch : batches) {
            externalApi.callBatch(batch); // 1번 호출 = $0.10
        }
    }
}
```
<br>
<hr>
<br>

## ✔️ 큐 vs 배열
- 앞선 글을 보면 단순히 일정 사이즈 이상이 되면 배치가 실행되는 소스이다.

- 그렇다면 단순히 배열에 담고 배열이 일정 사이즈 이상이 돼서 배치를 돌리면 똑같은 게 아닐까 생각할 수 있다.

- 그러나 순서 보장에 있어서 큐는 보장이 되지만, 배열은 순서 보장이 어렵다.

- 밑에 예시를 보자

### FIFO 보장
```java
// ❌ 배열 사용 - 순서 보장 어려움
public class ArrayBasedProcessor {
    private List<Data> dataList = new ArrayList<>();
    private int batchSize = 100;
    
    public void addData(Data data) {
        dataList.add(data); // 뒤에 추가
        if (dataList.size() >= batchSize) {
            processBatch();
        }
    }
    
    private void processBatch() {
        // 문제: 어떤 데이터부터 처리할지 명확하지 않음
        List<Data> batch = new ArrayList<>(dataList);
        dataList.clear();
        // 처리 순서가 보장되지 않음!
    }
}

// ✅ 큐 사용 - 순서 보장
public class QueueBasedProcessor {
    private Queue<Data> dataQueue = new LinkedList<>();
    private int batchSize = 100;
    
    public void addData(Data data) {
        dataQueue.offer(data); // 뒤에 추가
        if (dataQueue.size() >= batchSize) {
            processBatch();
        }
    }
    
    private void processBatch() {
        List<Data> batch = new ArrayList<>();
        for (int i = 0; i < batchSize && !dataQueue.isEmpty(); i++) {
            batch.add(dataQueue.poll()); // 앞에서부터 순서대로 제거
        }
        // 처리 순서가 보장됨!
    }
}
```
<br>
<br>

### 동시성 안전성
```java
// ❌ 배열 - 문제 발생

import java.util.*;

public class ArrayTest {
    private List<String> list = new ArrayList<>();
    
    public void add(String data) {
        list.add(data); // 여러 스레드가 동시에 접근하면 문제!
    }
    
    public int size() {
        return list.size();
    }
}

// 테스트
public static void main(String[] args) throws InterruptedException {
    ArrayTest test = new ArrayTest();
    
    // 10개 스레드가 동시에 데이터 추가
    Thread[] threads = new Thread[10];
    for (int i = 0; i < 10; i++) {
        threads[i] = new Thread(() -> {
            for (int j = 0; j < 100; j++) {
                test.add("data-" + j);
            }
        });
    }
    
    for (Thread t : threads) t.start();
    for (Thread t : threads) t.join();
    
    System.out.println("예상: 1000개, 실제: " + test.size() + "개");
    // 결과: 1000개 미만 (데이터 손실!)
}

// ✅ 큐 - 안전함 : ConcurrentLinkedQueue를 사용해야함
import java.util.concurrent.*;

public class QueueTest {
    private Queue<String> queue = new ConcurrentLinkedQueue<>();
    
    public void add(String data) {
        queue.offer(data); // 스레드 안전!
    }
    
    public int size() {
        return queue.size();
    }
}

// 테스트
public static void main(String[] args) throws InterruptedException {
    QueueTest test = new QueueTest();
    
    // 10개 스레드가 동시에 데이터 추가
    Thread[] threads = new Thread[10];
    for (int i = 0; i < 10; i++) {
        threads[i] = new Thread(() -> {
            for (int j = 0; j < 100; j++) {
                test.add("data-" + j);
            }
        });
    }
    
    for (Thread t : threads) t.start();
    for (Thread t : threads) t.join();
    
    System.out.println("예상: 1000개, 실제: " + test.size() + "개");
    // 결과: 정확히 1000개 (데이터 손실 없음!)
}
```
> 일반 큐로 동시성 구현 시 syncronized나 lock이 필요
<br>

| | 일반 Queue | ConcurrentLinkedQueue |
|---|---|---|
| **스레드 안전** | ❌ 아님 | ✅ 맞음 |
| **성능** | ❌ 느림 (락 필요) | ✅ 빠름 (락 없음) |
| **데이터 손실** | ❌ 발생 가능 | ✅ 없음 |
| **사용법** | ❌ 복잡 (동기화 필요) | ✅ 간단 (그냥 사용) |
<br>
<hr>
<br>

## ✔️ 배치큐 사용 예시
- 10초마다 배치가 실행되어 외부 api를 `restTemplate`으로 호출하는 멀티쓰레딩 배치큐

```java
import java.util.*;
import java.util.concurrent.*;

public class SimpleOrderBatchProcessor {
    private Queue<String> orderQueue = new ConcurrentLinkedQueue<>();
    private ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);
    private final int BATCH_SIZE = 5; // 5개씩 묶어서 처리
    private final long BATCH_TIMEOUT = 10000; // 10초
    
    public SimpleOrderBatchProcessor() {
        // 10초마다 배치 처리
        scheduler.scheduleAtFixedRate(this::processBatch, 
            BATCH_TIMEOUT, BATCH_TIMEOUT, TimeUnit.MILLISECONDS);
    }
    
    public void addOrder(String orderId) {
        orderQueue.offer(orderId);
        System.out.println("주문 추가: " + orderId + " (큐 크기: " + orderQueue.size() + ")");
        
        // 5개 모이면 즉시 처리
        if (orderQueue.size() >= BATCH_SIZE) {
            processBatch();
        }
    }
    
    private void processBatch() {
        List<String> batch = new ArrayList<>();
        String orderId;
        
        // 5개씩 주문 수집
        while ((orderId = orderQueue.poll()) != null && batch.size() < BATCH_SIZE) {
            batch.add(orderId);
        }
        
        if (!batch.isEmpty()) {
            sendToAPI(batch);
        }
    }
    
    private void sendToAPI(List<String> orders) {
        System.out.println("🚀 API 호출: " + orders.size() + "개 주문 전송");
        System.out.println("   주문들: " + orders);
        
        // 실제로는 RestTemplate으로 외부 API 호출
        // restTemplate.postForObject("http://api.com/orders", orders, String.class);
        
        System.out.println("✅ API 호출 완료");
    }
    
    public void shutdown() {
        scheduler.shutdown();
        processBatch(); // 남은 주문 처리
    }
}
```
<br>

```java
public class OrderSystem {
    public static void main(String[] args) throws InterruptedException {
        SimpleOrderBatchProcessor processor = new SimpleOrderBatchProcessor();
        
        // 주문 20개 생성
        for (int i = 1; i <= 20; i++) {
            processor.addOrder("ORDER-" + i);
            Thread.sleep(1000); // 1초 간격
        }
        
        Thread.sleep(15000); // 15초 대기
        processor.shutdown();
    }
}
```
