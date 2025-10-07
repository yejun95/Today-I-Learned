# ConcurrentHashMap이란?
- ConcurrentHashMap은 Java 1.5 버전에서 HashTable의 대안으로 처음 소개된 Collection이다.

- ConcurrentHashMap이 나오기 전까지는 multi-thread에서 map을 thread-safe하게 운용하기 위해 method/block lock을 통해 감싸주거나 HashTable 또는 synchronized map을 사용해야 했다.

- 하지만, HashTable/synchronized map의 경우 Map 전체에 lock을 걸어 운용하고 있기 때문에 thread-safe하지만, 성능 오버헤드를 발생시킨다.<br>
하나의 Thread가 lock을 유지하는 동안 다른 모든 Thread는 해당 Map Collection을 사용할 수 없기 때문이다.

- 그래서 Map의 일부에만 lock을 걸어 운용하는 ConcurrentHashMap이 등장했다. 
  - Java 1.5~7: 세그먼트 단위로 락을 적용하는 방식 사용
  - Java 8 이후: 각 버킷 단위로 락을 적용하는 방식으로 변경
  - 읽기 작업은 락 없이 수행되어 성능 최적화
  - 일부 작업에서는 CAS 연산을 활용한 무락 동시성 제공

- 이러한 방식으로 ConcurrentHashMap은 Thread-safe한 다른 Map Collection들보다 성능 우위를 가질 수 있게 되었다.
<br>
<br>

### Thread-Safe란?
- 멀티스레드 환경에서 여러 스레드가 동시에 접근해도 데이터의 일관성이 보장되는 것
<br>
<br>

### 왜 Thread-Safe가 필요한가?
```java
public class Counter {
    private int count = 0;
    
    public void increment() {
        count++; // ❌ 멀티스레드에서 문제 발생!
    }
}

// 문제 상황:
// Thread 1: count 읽기 (0)
// Thread 2: count 읽기 (0) 
// Thread 1: count++ (1)
// Thread 2: count++ (1)
// 결과: 2가 되어야 하는데 1이 됨!
```
<br>
<hr>
<br>

## ✔️ 기존 해결책들의 문제점
### HashTable 방식
```java
public class HashTable<K,V> {
    // 모든 메서드에 synchronized 적용
    public synchronized V put(K key, V value) { ... }
    public synchronized V get(Object key) { ... }
    public synchronized V remove(Object key) { ... }
}

// 문제점: 전체 Map에 락을 걸어서 성능 저하
// 하나의 스레드가 작업 중이면 다른 모든 스레드 대기
```
<br>
<br>

### Collections.synchronizedMap 방식
```java
Map<String, String> syncMap = Collections.synchronizedMap(new HashMap<>());

// 내부적으로 모든 메서드를 synchronized로 감쌈
// HashTable과 동일한 문제점
```
<br>
<br>

### 성능 문제 시각화
```
시간축: |----|----|----|----|----|
Thread1: [락획득][작업][락해제]
Thread2:        [대기][대기][대기]
Thread3:        [대기][대기][대기]
Thread4:        [대기][대기][대기]

결과: 한 번에 하나의 스레드만 작업 가능 = 성능 저하
```
> 동기화된 실행 방식
<br>
<hr>
<br>

## ✔️ ConcurrentHashMap의 핵심 아이디어
### 부분적 락킹 개념 적용
- 전체 Map에 락을 걸지 말고, 필요한 부분에만 락을 걸자!

```java
전체 Map
├── 영역 A (락 A)
├── 영역 B (락 B)  
├── 영역 C (락 C)
└── 영역 D (락 D)

// Thread 1이 영역 A에서 작업 중이어도
// Thread 2는 영역 B에서 동시에 작업 가능!
```
<br>

<img width="860" height="419" alt="image" src="https://github.com/user-attachments/assets/bfef70fa-48af-4549-9a19-c44354088f23" />
<br>
<br>

### 동시성 향상 시각화
```
시간축: |----|----|----|----|----|
Thread1: [락A획득][작업A][락A해제]
Thread2:        [락B획득][작업B][락B해제]
Thread3:        [락C획득][작업C][락C해제]

결과: 여러 스레드가 동시에 다른 영역에서 작업 가능 = 성능 향상
```
<br>
<br>

### 동시에 작업 하는데 데이터의 정합성은 어떻게 체크할 수 있을까?
- 동일한 변수를 증감할 때는 기본적인 `put/get`메서드로는 불가능하다.

- `get()` 은 동시성 문제를 일으키지는 않지만, 최신 데이터를 반드시 보장하지 않는다.<br>
예를 들어, 한 스레드가 데이터를 쓰는 도중에 다른 스레드가 그 데이터를 읽으면 과거의 값을 읽을 수 있다. 

- 이러한 문제는 목적에 따라 달라지게 되는데 데이터 일관성 보다는 성능 최적화에 중점을 두고 있기 때문인거 같다.

- 그럼에도 get이 동시성을 보장하는 이유는 내부적으로 volatile 변수를 사용해 메모리 가시성을 보장한다.
  - volatile 키워드는 특정 필드에 대해 스레드 간의 메모리 가시성을 제공한다.
  - ConcurrentHashMap 버킷들은 노드 배열을 사용하고, 이 배열에 대한 참조는 volatile 로 선언되어 있다.
  - 이를 통해서 여러 스레드가 동시에 읽기를 할 때 최신 값을 읽을 수 있도록 보장된다. 

<img width="439" height="296" alt="image" src="https://github.com/user-attachments/assets/1f98c2ec-b4f1-4bd2-9bfe-4bcaffc626ad" /><br>
> volatile 메모리 가시성 보장
<br>
<br>

1. 기본 put/get 메서드
```java
// ❌ 이렇게 하면 정합성 보장 안됨
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
map.put("counter", 0);

// Race Condition 발생!
int current = map.get("counter");  // 읽기
map.put("counter", current + 1);  // 쓰기 (사이에 다른 스레드가 끼어들 수 있음)
```
<br>

2. 원자적 연산 메서드
```java
// ✅ 이렇게 하면 정합성 보장됨!
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
map.put("counter", 0);

// 원자적으로 처리됨!
map.compute("counter", (k, v) -> v + 1);
```
<br>

- 실제 실행 흐름
```
시간축: |----|----|----|----|----|
Thread1: [락획득][get(0)][계산(1)][put(1)][락해제]
Thread2: [대기][대기][대기][락획득][get(1)][계산(2)][put(2)][락해제]
Thread3: [대기][대기][대기][대기][대기][락획득][get(2)][계산(3)][put(3)][락해제]

결과: 순차적으로 처리되어 정확한 값 보장!
```
<br>
<br>

### 실제 테스트 코드
```java
public class ConcurrentHashMapConsistencyTest {
    public static void main(String[] args) throws InterruptedException {
        testBasicOperations();      // ❌ 정합성 보장 안됨
        testAtomicOperations();     // ✅ 정합성 보장됨
        testAtomicInteger();        // ✅ 정합성 보장됨
    }
    
    // ❌ 기본 연산 - 정합성 보장 안됨
    public static void testBasicOperations() throws InterruptedException {
        ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
        map.put("counter", 0);
        
        int threadCount = 1000;
        CountDownLatch latch = new CountDownLatch(threadCount);
        
        for (int i = 0; i < threadCount; i++) {
            new Thread(() -> {
                try {
                    // Race Condition 발생!
                    int current = map.get("counter");
                    map.put("counter", current + 1);
                } finally {
                    latch.countDown();
                }
            }).start();
        }
        
        latch.await();
        System.out.println("기본 연산 결과: " + map.get("counter")); // 1000보다 작을 수 있음
    }
    
    // ✅ 원자적 연산 - 정합성 보장됨
    public static void testAtomicOperations() throws InterruptedException {
        ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
        map.put("counter", 0);
        
        int threadCount = 1000;
        CountDownLatch latch = new CountDownLatch(threadCount);
        
        for (int i = 0; i < threadCount; i++) {
            new Thread(() -> {
                try {
                    // 원자적으로 처리됨!
                    map.compute("counter", (k, v) -> v + 1);
                } finally {
                    latch.countDown();
                }
            }).start();
        }
        
        latch.await();
        System.out.println("원자적 연산 결과: " + map.get("counter")); // 정확히 1000!
    }
    
    // ✅ AtomicInteger 사용 - 정합성 보장됨
    public static void testAtomicInteger() throws InterruptedException {
        ConcurrentHashMap<String, AtomicInteger> map = new ConcurrentHashMap<>();
        map.put("counter", new AtomicInteger(0));
        
        int threadCount = 1000;
        CountDownLatch latch = new CountDownLatch(threadCount);
        
        for (int i = 0; i < threadCount; i++) {
            new Thread(() -> {
                try {
                    // AtomicInteger의 원자적 연산
                    map.get("counter").incrementAndGet();
                } finally {
                    latch.countDown();
                }
            }).start();
        }
        
        latch.await();
        System.out.println("AtomicInteger 결과: " + map.get("counter").get()); // 정확히 1000!
    }
}
```
> 데이터 정합성을 위해서는 compute를 사용하거나 CAS 알고리즘을 사용하는 Atomic 타입을 같이 써야한다.
<br>
<hr>
<br>

## ✔️ 언제 사용해야 할까?
### 사용 권장
- 높은 동시성이 필요한 상황

- 읽기 작업이 쓰기 작업보다 많은 경우

- 캐시 구현

- 세션 관리

- 카운터 구현

- 실시간 데이터 처리
<br>
<br>

### 사용 비권장
- 단일 스레드 환경

- Map 크기가 매우 작은 경우 (10개 미만)

- 복잡한 동기화 로직이 필요한 경우

- null 값을 저장해야 하는 경우
<br>
<hr>
<br>

## ✔️ 정리
- 부분적 락킹 : 전체가 아닌 필요한 부분에만 락 적용

- Lock-Free 읽기 : 읽기 작업은 락 없이 수행

- 원자적 연산 : 특별한 메서드들로 안전한 업데이트

- 높은 동시성 : 여러 스데르가 동시에 다른 영역 작업 가능
<br>
<hr>
<br>

**Reference**<br>

[mhyun, Park : ConcurrentHashMap는 어떻게 Thread-safe 한가?](https://velog.io/@alsgus92/ConcurrentHashMap%EC%9D%98-Thread-safe-%EC%9B%90%EB%A6%AC)<br>
[꽥블로그 : ConcurrentHashmap 동시성 처리 방법](https://thewayitwas.tistory.com/608)<br>
