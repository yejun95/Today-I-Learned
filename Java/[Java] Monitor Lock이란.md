# Monitor Lock(모니터 락)이란
- 멀티스레드 환경에서 여러 스레드가 공유 자원에 동시에 접근하는 것을 방지하기 위한 동기화 메커니즘이다.

- 자바에서는 모든 객체가 고유한 모니터락을 가지고 있다.
  - 한 번에 오직 하나의 스레드만 특정 객체의 모니터락을 획득할 수 있다.
  - 다른 스레드들은 락이 해제될 때까지 대기(blocked) 상태가 된다.

- `synchronized` 키워드를 통해 특정 객체의 모니터락을 획득하고,<br>
락이 있는 동안에는 해당 자원에 대한 다른 스레드의 접근을 막아 데이터의 일관성을 유지한다.
<br>
<hr>
<br>

## ✔️ 왜 필요한가?
- 멀티스레드 환경에서 여러 스레드가 동시에 같은 데이터를 수정하면 경쟁 조건(Race Condition)이 발생

```java
// 문제가 되는 상황
class Counter {
    private int count = 0;
    
    public void increment() {
        count++;  // 실제로는 3단계: read → modify → write
    }
}
```
<br>

- `count++`은 원자적이지 않다.
1. Thread A: count 읽기 (0)
2. Thread B: count 읽기 (0)
3. Thread A: 1 증가 → 1 저장
4. Thread B: 1 증가 → 1 저장

- 결과: 2가 되어야 하는데 1이 됨!
<br>
<hr>
<br>

## ✔️ 모니터락의 작동 원리
### synchronized 키워드 사용
```java
class Counter {
    private int count = 0;
    
    // 방법 1: 메서드 전체 동기화
    public synchronized void increment() {
        count++;
    }
    
    // 방법 2: 블록 동기화
    public void decrement() {
        synchronized(this) {
            count--;
        }
    }
}
```
<br>

### 내부적으로 일어나는 일
- 1. :
  - Thread A가 `increment()` 호출
  - `Counter` 객체의 모니터락 획득
  - `count++` 실행
  - 메서드 종료 시 락 자동 해제
<br>

- 2. :
  - Thread B가 `increment()` 호출 (Thread A가 실행 중)
  - `Counter` 객체의 모니터락 획득 시도
  - 이미 Thread A가 보유 중
  - Thread B는 BLOCKED 상태로 대기
  - Thread A가 락 해제하면 Thread B가 획득
<br>
<hr>
<br>

## ✔️ 실제 예시 코드
### 예시 1: 은행 계좌
```java
public class BankAccount {
    private int balance = 1000;
    
    // synchronized 없으면 문제 발생!
    public synchronized void withdraw(int amount) {
        if (balance >= amount) {
            System.out.println(Thread.currentThread().getName() + 
                " 출금 시작: " + amount);
            
            // 잔액 확인 후 출금 전 딜레이 (실제 상황 시뮬레이션)
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            
            balance -= amount;
            System.out.println(Thread.currentThread().getName() + 
                " 출금 완료. 잔액: " + balance);
        } else {
            System.out.println(Thread.currentThread().getName() + 
                " 출금 실패: 잔액 부족");
        }
    }
    
    public synchronized int getBalance() {
        return balance;
    }
    
    public static void main(String[] args) {
        BankAccount account = new BankAccount();
        
        // 3개의 스레드가 동시에 출금 시도
        Thread t1 = new Thread(() -> account.withdraw(600), "고객1");
        Thread t2 = new Thread(() -> account.withdraw(600), "고객2");
        Thread t3 = new Thread(() -> account.withdraw(600), "고객3");
        
        t1.start();
        t2.start();
        t3.start();
    }
}
```
<br>

- synchronized가 있을 때 출력
```
고객1 출금 시작: 600
고객1 출금 완료. 잔액: 400
고객2 출금 실패: 잔액 부족
고객3 출금 실패: 잔액 부족
```
<br>
<hr>
<br>

## ✔️ 중요 개념
### 재진입 가능성(Reentrant)
- 모니터락은 재진입이 가능하다.

```java
public class ReentrantExample {
    public synchronized void method1() {
        System.out.println("method1 실행");
        method2();  // 같은 객체의 synchronized 메서드 호출 가능!
    }
    
    public synchronized void method2() {
        System.out.println("method2 실행");
    }
}
```
<br>
<br>

### 락의 범위
- 각 객체는 고유한 모니터락을 가지고 있다.

```java
public class LockScope {
    private Object lock1 = new Object();
    private Object lock2 = new Object();
    
    public void method1() {
        synchronized(lock1) {
            // lock1의 모니터락 획득
        }
    }
    
    public void method2() {
        synchronized(lock2) {
            // lock2의 모니터락 획득 (독립적!)
        }
    }
}
```
<br>
<br>

### 주의 사항
- 데드락 발생 가능성 존재

```java
public class DeadlockExample {
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();
    
    public void method1() {
        synchronized(lock1) {
            System.out.println("method1: lock1 획득");
            
            synchronized(lock2) {
                System.out.println("method1: lock2 획득");
            }
        }
    }
    
    public void method2() {
        synchronized(lock2) {  // 순서가 반대!
            System.out.println("method2: lock2 획득");
            
            synchronized(lock1) {
                System.out.println("method2: lock1 획득");
            }
        }
    }
}
// Thread A: method1() 실행 → lock1 획득 → lock2 대기
// Thread B: method2() 실행 → lock2 획득 → lock1 대기
// → 데드락!
```
<br>
<hr>
<br>

## 정리
- 모니터락: 자바 객체의 내장 동기화 메커니즘

- synchronized: 모니터락을 사용하는 키워드

- 용도: 멀티스레드 환경에서 공유 자원 보호

- 주의: 데드락, 성능 저하 가능성

- 발전: `java.util.concurrent` 패키지가 더 나은 대안 제공
