# CircuitBreaker란 무엇인가?
## ✔️ 먼저 Resilience4j 부터 알아보기
- Resilience4j는 함수형 프로그래밍을 위해 설계된 가벼운 fault tolerance 라이브러리이다.

- Circuit Breaker, Rate Limiter, Retry, Bulkhead 등을 사용

- 스프링에서 활용할 수 있는 서킷브레이커는 지금 설명하는 Resilience4j 라이브러리를 통해 사용이 가능하다.
<br>
<hr>
<br>

## ✔️ Circuit Breaker(서킷 브레이커)란 무엇인가?
- 서로 다른 시스템 간의 연동 시 장애전파 차단을 목적으로 하며, 연동 시 이상을 감지하고 이상이 발생하면 연동을 차단하고, 이후 이상이 회복되면 자동으로 다시 연동하기 위한 기술이다.

- 연결의 성공/실패를 카운트하여 실패율이 임계치를 넘었섰을 때 자동적으로 접속을 차단하는 시스템

- 다량의 오류가 발생하여 실패율이 임계치를 넘어가게 되면, 해당 시점에 발생하는 모든 호출들은 fallback method를 타게한다.

- 서킷 브레이커는 슬라이딩 윈도우를 통해 호출 결과를 저장하고 집계한다.
  - Count-based 방식 : 마지막 N개의 호출 결과를 집계
  - Time-based 방식 : 마지막 N초간 일어난 호출 결과를 집계
<br>
<hr>
<br>

## ✔️ State Transition(상태 전이)
- Resilience4j에서 지원하는 Circuit breaker는 기본 3가지 상태`(CLOSED, OPNE, HALF_OPEN)`와<br>
특별한 2가지 상태`(DISABLED, FORCED_OPEN)를 통해 성공/실패 상태를 관리한다.

![image](https://github.com/user-attachments/assets/82311756-8ed8-448b-977d-f7e3b8959bfd)
<br>

- CLOSED : 보통 상태로써 정상적으로 호출되고 응답을 주는 상태이다.

- OPEN, HALF_OPEN : 문제 발생이 감지된 상태

**➡️ CLOSED -> OPEN**
- 실패율이 설정된 임계값(threshold)보다 크거나 같으면 서킷브레이커는 상태를 CLOSED에서 OPEN으로 변경한다.

- 기본적으로 모든 Exception이 실패로 측정되지만, 설정에 따라 특정 Exception만 실패로 측정되도록 설정할 수 있다.

- 느린 호출의 비율이 설정된 임계값보다 크거나 같은 경우에도 서킷브레이커는 상태를 CLOSED에서 OPEN으로 변경한다.

- 실패율 & 느린 호출 비율의 값은 지정된 최소 호출 횟수 이후부터 측정된다.
<br>

**➡️ OPEN -> HALF_OPEN**
- 서킷브레이커의 상태가 OPEN이 된 후 설정된 시간이 경과하면 상태를 HALF_OPEN으로 변경한다.

- HALF_OPEN 상태에서는 지정된 횟수의 호출을 통해 여전히 호출이 불가능한지, 아니면 호출이 가능한 지 판단한다.

- 만약 실패율이 설정된 임계값보다 크거나 같은 경우, 다시 OPEN으로 돌아가고, 임계값보다 작다면 CLOSED 상태로 변경된다.
<br>

**➡️ DISABLED * FORCED_OPEN**
- DISABLED는 항상 접근을 허용한다.

- FORCED_OPEN은 항상 거부하는 상태를 말한다.

- 2가지 특별 상태에서는 어떤 값도 측정되지 않는다.

- 해당 상태에서 상태를 변경하고자 한다면, 상태 전환을 유도하거나 서킷브레이커를 초기화해야 한다.
<br>
<hr>
<br>

**Reference**<br>

[댕댕's Dev Note : [Java/Spring] Resilience4j - Circuit Breaker (1). 정의](https://mein-figur.tistory.com/entry/resilience4j-circuit-breaker-definition)<br>
[우아한기술블로그 : 개발자 의식의 흐름대로 적용해보는 서킷브레이커](https://techblog.woowahan.com/15694/)
