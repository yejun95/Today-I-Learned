# 멀티쓰레드는 언제 써야 하는가?
- 멀티쓰레드는 하나의 프로그램에서 여러 개의 작업 흐름(쓰레드)을 동시에 실행할 수 있게 해주는 프로그래밍 기법이다.
<br>

❗ **TIP**
```
프로세스: 독립적인 메모리 공간을 가진 실행 단위
쓰레드: 프로세스 내에서 실행되는 작업의 최소 단위
멀티쓰레드: 하나의 프로세스에서 여러 쓰레드가 동시에 실행
```
<br>

## ✔️ 사용하기 좋을 때
### I/O 작업이 많은 경우
- 파일 읽기/쓰기

- 네트워크 통신

- 데이터베이스 쿼리

```
싱글쓰레드: 요청1 → DB쿼리(2초) → 응답1 → 요청2 → DB쿼리(2초) → 응답2
총 시간: 4초

멀티쓰레드: 요청1, 요청2 → 동시 DB쿼리(2초) → 응답1, 응답2
총 시간: 2초
```
<br>
<br>

### CPU 집약적 작업을 병렬로 처리
- 대용량 데이터 처리

- 이미지 처리

- 계산 작업
<br>
<br>

### 사용자 응답성을 높여야 하는 경우
- UI가 멈추지 않게 하면서 백그라운드 작업 수행
<br>
<br>

## ✔️ 실제 사용 사례와 예제
### 사례 1: 웹 서버 요청 처리
```java
public class WebServer {
    private final ExecutorService executor = Executors.newFixedThreadPool(10);
    
    public void handleRequest(Socket clientSocket) {
        executor.submit(() -> {
            try {
                // 각 클라이언트 요청을 별도 쓰레드에서 처리
                processRequest(clientSocket);
            } catch (IOException e) {
                e.printStackTrace();
            }
        });
    }
    
    private void processRequest(Socket socket) throws IOException {
        // 실제 요청 처리 로직
        BufferedReader reader = new BufferedReader(
            new InputStreamReader(socket.getInputStream())
        );
        
        // DB 쿼리나 파일 I/O 같은 시간이 걸리는 작업
        String result = performDatabaseQuery();
        
        // 응답 보내기
        PrintWriter writer = new PrintWriter(socket.getOutputStream());
        writer.println(result);
        writer.close();
    }
}
```
<br>
<br>

### 사례 2: 대용량 데이터 처리
```java
public class DataProcessor {
    
    public void processLargeDataSet(List<String> dataList) {
        int numberOfThreads = Runtime.getRuntime().availableProcessors();
        ExecutorService executor = Executors.newFixedThreadPool(numberOfThreads);
        
        // 데이터를 청크 단위로 분할
        int chunkSize = dataList.size() / numberOfThreads;
        
        List<Future<Integer>> futures = new ArrayList<>();
        
        for (int i = 0; i < numberOfThreads; i++) {
            int start = i * chunkSize;
            int end = (i == numberOfThreads - 1) ? dataList.size() : (i + 1) * chunkSize;
            
            List<String> chunk = dataList.subList(start, end);
            
            Future<Integer> future = executor.submit(() -> {
                return processChunk(chunk); // 각 청크를 병렬 처리
            });
            futures.add(future);
        }
        
        // 결과 수집
        int totalProcessed = 0;
        for (Future<Integer> future : futures) {
            try {
                totalProcessed += future.get();
            } catch (InterruptedException | ExecutionException e) {
                e.printStackTrace();
            }
        }
        
        executor.shutdown();
        System.out.println("총 처리된 항목: " + totalProcessed);
    }
    
    private int processChunk(List<String> chunk) {
        int processed = 0;
        for (String data : chunk) {
            // 각 데이터 항목 처리 로직
            performComplexCalculation(data);
            processed++;
        }
        return processed;
    }
}
```
<br>
<br>

### 사례 3: 비동기 작업 처리
```java
public class AsyncTaskManager {
    
    public CompletableFuture<String> fetchUserDataAsync(String userId) {
        return CompletableFuture.supplyAsync(() -> {
            // 시간이 걸리는 데이터베이스 쿼리
            return databaseService.getUserById(userId);
        }).thenApply(user -> {
            // 추가 데이터 처리
            return processUserData(user);
        });
    }
    
    public void handleUserRequest(String userId) {
        // 메인 쓰레드는 블록되지 않음
        CompletableFuture<String> userDataFuture = fetchUserDataAsync(userId);
        
        // 다른 작업들을 계속 수행 가능
        
        // 필요할 때 결과를 받음
        userDataFuture.thenAccept(userData -> {
            System.out.println("사용자 데이터: " + userData);
        });
    }
}
```
<br>
<hr>
<br>

## ✔️ 멀티쓰레드 사용 시 주의사항
### 동기화 문제
```java
public class Counter {
    private int count = 0;
    
    // 잘못된 예시 - 동기화 없음
    public void increment() {
        count++; // 여러 쓰레드에서 동시 호출 시 문제 발생
    }
    
    // 올바른 예시 - 동기화 추가
    public synchronized void incrementSync() {
        count++;
    }
}
```
<br>
<br>

### 데드락 방지
```java
public class DeadlockExample {
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();
    
    // 데드락을 피하려면 항상 같은 순서로 락을 획득
    public void method1() {
        synchronized (lock1) {
            synchronized (lock2) {
                // 작업 수행
            }
        }
    }
    
    public void method2() {
        synchronized (lock1) { // lock1을 먼저 획득
            synchronized (lock2) {
                // 작업 수행
            }
        }
    }
}
```
<br>
<hr>
<br>

## ✔️ 언제 멀티쓰레드를 사용하지 말아야 할까?
- 간단한 작업: 오버헤드가 비용보다 큰 경우

- CPU 코어 수보다 많은 쓰레드: 문맥 전환 비용 증가

- 동기화 복잡도: 코드가 복잡해져서 버그 발생 가능성 증가
<br>
<hr>
<br>

## ✔️ 정리
- 멀티쓰레드는 성능 향상을 가져다주지만, 복잡성도 함께 증가시킨다.

- 항상 성능 측정과 함께 신중하게 적용해야 한다.
