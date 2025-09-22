# CAS 알고리즘이란
- Compare-And-Swap 알고리즘은 동시성 프로그래밍에서 사용되는 락-프리 알고리즘의 일종

- 변수의 값을 비교한 후 예상하는 값이면 새로운 값으로 교체하는 원자적 연산을 수행

- 자바에서는 `Atomic Class`를 통해 CAS 연산을 지원
  - 락을 사용하지 않기 때문에 데드락과 같은 문제를 방지하면서도 높은 성능 유지
 
- 따라서 CAS는 멀티 스레드 환경에서 성능 저하 없이 데이터의 일관성을 유지할 수 있는 효과적인 방법
<br>
<hr>
<br>

## ✔️ CAS 알고리즘 동작 원리
<img width="538" height="433" alt="image" src="https://github.com/user-attachments/assets/1d6416a6-291b-4acb-ad69-c1ae15cfe532" />
<br>

- 위 그림을 토대로 동작 원리를 알아보자.
  - 인자로 기존 값(Compared Value)과 변경할 값(Exchanged Value)을 전달
  - 기존 값(Compared Value)이 현재 메모리가 가지고 있는 값(Destination)과 같다면 변경할 값(Exchanged Value)을 반영하며 true 반환
  - 반대로 기존 값(Compared Value)이 현재 메모리가 가지고 있는 값(Destination)과 다르다면 값을 반영하지 않고 false 반환

```
CAS(메모리주소, 예상값, 새값):
    if (메모리주소의 현재값 == 예상값):
        메모리주소 = 새값
        return 성공
    else:
        return 실패
```
<br>

### 은행 계좌 예시로 이해하기
- 상황 설정
  - 계좌 잔액: 100만원
  - 스레드 A: 50만원 출금 시도
  - 스레드 B: 30만원 출금 시도
<br>

- CAS 동작 과정
```
시간: 09:00:01 → 09:00:02 → 09:00:03 → 09:00:04

스레드 A: 잔액 확인(100만원) → 계산(50만원 출금) → CAS(100만원, 50만원)
스레드 B:                     잔액 확인(100만원) → 계산(30만원 출금) → CAS(100만원, 70만원) → 성공!
```

- `09:00:04` 시점:
  - 스레드 A의 CAS: "100만원이면 50만원으로 바꿔줘" → 실패! (실제로는 70만원)
  - 스레드 B의 CAS: "100만원이면 70만원으로 바꿔줘" → 성공!
<br>

- 이렇게 CAS는 "값이 예상한 대로면 바꿔주고, 아니면 실패"라는 간단한 원리로 동작한다.

- 마치 "이 값이 맞으면 바꿔줘"라고 물어보는 것과 같다고 보면 된다.
<br>
<hr>
<br>

## ✔️ synchronized vs CAS
### synchronized 방식
```java
private int counter = 0;
private Object lock = new Object();

public void increment() {
    synchronized(lock) {
        counter++;  // 다른 스레드는 기다려야 함
    }
}
```
<br>

- 특징 : 
  - 다른 스레드가 기다림(블로킹)
  - 한 번에 하나의 스레드만 실행
  - 느리지만 안전
<br>

### CAS 방식
```java
private AtomicInteger counter = new AtomicInteger(0);

public void increment() {
    int current;
    do {
        current = counter.get();
    } while (!counter.compareAndSet(current, current + 1));
}
```
<br>

- 특징 : 
  - 다른 스레드가 기다리지 않음 (논블로킹)
  - 여러 스레드가 동시에 시도
  - 빠르지만 CPU 많이 사용
<br>
<hr>
<br>

**Reference**<br>

[환이s : 동시성 제어를 위한 세 가지 키워드 / CAS 알고리즘 개념 알아가기](https://drg2524.tistory.com/195)<br>
