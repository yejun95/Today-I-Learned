# Server-Sent Events(SSE)란?
- 서버에서 클라이언트로 단방향 실시간 데이터 스트리밍을 가능하게 하는 웹 표준 기술이다.

- HTTP 연결을 통해 서버가 클라이언트에게 지속적으로 푸시할 수 있다.

<img width="872" height="687" alt="image" src="https://github.com/user-attachments/assets/1c18dd62-b42b-4d49-b123-198e41848cdc" /><br>
> 연결이 성공하면 서버는 클라이언트에게 지속적으로 이벤트를 푸시할 수 있다.
<br>
<hr>
<br>

## ✔️ 어떨 때 사용해야 하는가?
- 실시간 알림 (뉴스, 주식 가격, 채팅 메시지)

- 서버 상태 모니터링
<br>
<hr>
<br>

## ✔️ Polling vs Websocker vs SSE
- 실시간 웹 애플리케이션 개발 시 사용되는 몇 가지 방법이 있다.

- 각 통신 방식에는 장단점이 존재하여 어떤게 정답이라고는 할 수 없으니, 아래 글을 보고 적합한 것을 고르도록 하자.

### Polling (풀링)
<img width="602" height="296" alt="image" src="https://github.com/user-attachments/assets/ad0d74c5-2a76-419a-8bdb-c080c0f0e855" />
<br>

```
클라이언트 → 서버: "새 데이터 있나요?"
서버 → 클라이언트: "없어요"
클라이언트 → 서버: "새 데이터 있나요?" (몇 초 후)
서버 → 클라이언트: "있어요! 여기 데이터"
```
> 클라이언트 요청에 대한 응답을 보낸다.
<br>

**장점**
- 간단한 구현 : 가장 기본적인 HTTP 요청/응답 패턴 사용

- 범용성 : 모든 브라우저와 서버에서 지원

- 레거시 호환 : 오래된 시스템과도 호환 가능

- 무상태 : 서버에서 연결 상태를 유지할 필요 없음
<br>

**단점**
- 비효율성 : 데이터가 없어도 계속 클라이언트에서 요청을 보냄

- 지연 시간 : 폴링 간격만큼 실시간성이 떨어짐

- 서버 부하 : 불필요한 요청으로 인한 리소스 낭비

- 배터리 소모 : 모바일 기기에서 배터리 소모 증가
<br>
<br>

### Websocket (웹소켓)
<img width="590" height="293" alt="image" src="https://github.com/user-attachments/assets/79ef31eb-cb1e-46df-98e3-43869e60d8fc" />
<br>

```
클라이언트 ↔ 서버: 양방향 실시간 통신
클라이언트 → 서버: "메시지 전송"
서버 → 클라이언트: "응답 메시지"
```
> 한 번 연결이 되면 양방향으로 실시간 통신이 가능하다.
<br>

**장점**
- 양방향 실시간 통신

- 낮은 지연시간
<br>

**단점**
- 구현이 복잡 : 클라이언트와 서버 양측의 실시간 처리 로직이 들어가야함

- 프록시/방화벽 문제 가능

- 연결 관리 필요
<br>
<br>

### Server-Sent Events (SSE)
<img width="582" height="269" alt="image" src="https://github.com/user-attachments/assets/d198559c-6057-4140-b2c4-77b61495e6ce" />

