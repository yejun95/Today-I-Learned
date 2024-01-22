## 인터럽트(Interrupt)란?
- 프로세스 실행 도중 예기치 않은 상황이 발생할 때, 발생한 상황을 처리한 후 실행 중인 작업으로 복귀하는 것을 말한다.

- 주로 입출력 장치의 signal, data가 발생할 때까지 원래의 작업을 수행하다가 해당 기능을 처리하는 것이다. (외부 인터럽트)
<br>
<hr>
<br>

### ✔ 종류
**비동기적 인터럽트 / 하드웨어 인터럽트**
- 전원 이상, 기계 착오, 외부 신호, 입출력 등 프로세스 외부에서 발생하는 인터럽트
<br>
<br>

**동기적 인터럽트 / 소프트웨어 인터럽트**
- 프로세스 내부에서 잘못된 명령어, 데이터로 exception이 발생하는 것이며 trap이라고 불림
<br>
<hr>
<br>

### ✔ 인터럽트 서비스 루틴(ISR : Interrupt Service Routine)
- 인터럽트가 발생하면 처리하기 위한 루틴인 ISR이 실행된다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/df86c6ad-5713-45ad-842d-e87417fc12e2)
<br>
<br>

- 주 프로그램 작업 수행 중 인터럽트 발생
- 주 프로그램 상태 레지스터와 PC 등을 스택에 잠시 저장
- 인터럽트 서비스 루틴으로 점프, 처리
- 다시 주 프로그램 작업 복귀


<br>
<hr>
<br>

**Reference**<br>

[wannabeking : 인터럽트(Interrupt)란?](https://velog.io/@pppp0722/%EC%9D%B8%ED%84%B0%EB%9F%BD%ED%8A%B8Interrupt%EB%9E%80)
