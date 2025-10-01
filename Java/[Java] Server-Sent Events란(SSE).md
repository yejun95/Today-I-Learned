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
<br>

```
클라이언트 → 서버: "연결 요청"
서버 → 클라이언트: "데이터 스트림 시작"
서버 → 클라이언트: "새 데이터 전송"
서버 → 클라이언트: "새 데이터 전송"
```
> 서버에서 클라이언트로 단방향 실시간 데이터 스트림을 제공
<br>

**장점**
- 실시간성 : 서버에서 즉시 데이터 푸시 가능

- 효율성 : 불필요한 풀링 요청 없이 서버에서 필요한 시점에만 데이터 전송

- 서버 부하 감소 : 클라이언트의 반복 요청 없이 서버 주도로 데이터 전송
<br>

**단점**
- 단뱡항 통신 : 서버에서 클라이언트로 데이터 전송 가능 (클라이언트 -> 서버 통신 불가)

- 연결 수 제한 : 브라우저당 동시 연결 수 제한 (보통 6개)

- 메모리 사용 : 장시간 연결로 인한 서버 메모리 점유
  - 자동으로 끊어지는 기준 확립 필요 (ex. 일정시간 지나면 서버 타임아웃)
  - 대규모 시스템의 경우 다른 기술을 고려하는 것이 나을수도...있다.
  - 만명 연결 시 예상 메모리 사용량 -> 10,000명 × 평균 1MB = 약 10GB 메모리 사용
<br>
<hr>
<br>

## ✔️ 동작 과정
### 1. 클라이언트 연결 요청
- 클라이언트가 서버와의 연결을 요청하는 것을 구독이라는 행위로 나타낼 수 있다.

- 서버의 변화를 구독하여 실시간으로 관찰하고 싶다는 의미

```
// HTTP  요청 헤더

GET /events HTTP/1.1
Host: example.com
Accept: text/event-stream  // 중요 포인트
Cache-Control: no-cache
Connection: keep-alive
```
> text/event-stream : SSE 프로토콜임을 명시
<br>
<br>

### 2. 서버 연결 수락 및 응답 설정
- Spring Boot 기준 `SseEmitter`를 사용하여 SSE 연결을 쉽게 구현 가능
```java
@RestController
@RequestMapping("/api")
public class SSEController {
    
    // 동시성 안전한 연결 관리
    private final Set<SseEmitter> emitters = ConcurrentHashMap.newKeySet();
    
    @GetMapping(value = "/events", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public SseEmitter streamEvents() {
        // 무제한 타임아웃으로 연결 생성
        SseEmitter emitter = new SseEmitter(Long.MAX_VALUE);
        
        // 연결 성공 시 emitter 추가
        emitters.add(emitter);
        
        // 연결 종료 시 자동 정리
        emitter.onCompletion(() -> emitters.remove(emitter));
        emitter.onTimeout(() -> emitters.remove(emitter));
        emitter.onError((ex) -> emitters.remove(emitter));
        
        // 초기 연결 확인 메시지 전송
        try {
            emitter.send(SseEmitter.event()
                .name("connection")
                .data("연결 성공"));
        } catch (IOException e) {
            emitter.completeWithError(e);
        }
        
        return emitter;
    }
}
```
<br>
<br>

### 3. Transfer-Encoding: chunked 상세 분석
- 일반 HTTP vs SSE 응답 비교
  - 일반적인 HTTP 응답에서는 전체 데이터 크기를 미리 계산하여 `Content-Length`헤더에 설정해야 한다.
  - 하지만 SSE에서는 데이터가 준비되는 대로 즉시 전송할 수 있어 효율적
<br>

```java
// 일반적인 HTTP 응답 (Content-Length 방식)
@GetMapping("/normal")
public ResponseEntity<String> normalResponse() {
    String data = "Hello World";
    HttpHeaders headers = new HttpHeaders();
    headers.setContentLength(data.length()); // 길이 계산 필요
    return ResponseEntity.ok()
        .headers(headers)
        .body(data);
}

// SSE 응답 (chunked 방식)
@GetMapping(value = "/events", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
public SseEmitter streamEvents() {
    SseEmitter emitter = new SseEmitter();
    
    // 즉시 전송, 길이 계산 불필요
    try {
        emitter.send("data: Hello World\n\n");
        emitter.send("data: Another message\n\n");
    } catch (IOException e) {
        emitter.completeWithError(e);
    }
    
    return emitter;
}
```
<br>
<br>

### 4. 연결 유지 및 이벤트 전송
- Spring Boot 이벤트 브로드캐스팅
  - 여러 클라이언트에게 동시에 이벤트를 전송하는 브로드캐스팅 기능을 구현
  - 메모리 효율성과 실시간성 보장
<br>

```java
@Service
public class SSEService {
    
    // 동시성 안전한 연결 관리
    private final Set<SseEmitter> emitters = ConcurrentHashMap.newKeySet();
    
    public void addEmitter(SseEmitter emitter) {
        emitters.add(emitter);
        
        // 연결 종료 시 자동 정리
        emitter.onCompletion(() -> emitters.remove(emitter));
        emitter.onTimeout(() -> emitters.remove(emitter));
        emitter.onError((ex) -> emitters.remove(emitter));
    }
    
    public void broadcastEvent(String eventName, Object data) {
        List<SseEmitter> deadEmitters = new ArrayList<>();
        
        // 모든 연결된 클라이언트에게 이벤트 전송
        for (SseEmitter emitter : emitters) {
            try {
                emitter.send(SseEmitter.event()
                    .name(eventName)
                    .data(data));
            } catch (IOException e) {
                // 전송 실패한 연결은 정리 대상에 추가
                deadEmitters.add(emitter);
            }
        }
        
        // 죽은 연결 정리
        emitters.removeAll(deadEmitters);
    }
    
    // 주기적 하트비트로 연결 상태 확인
    @Scheduled(fixedRate = 30000)
    public void sendHeartbeat() {
        broadcastEvent("heartbeat", "ping");
    }
}
```
<br>
<hr>
<br>

## ✔️ 실제 사용 예시
### 이벤트 데이터 전송 유틸리티
```java
public class SSEEventBuilder {
    
    // 단순 메시지 전송
    public static void sendSimpleMessage(SseEmitter emitter, String message) 
            throws IOException {
        emitter.send("data: " + message + "\n\n");
    }
    
    // JSON 데이터 전송
    public static void sendJsonData(SseEmitter emitter, Object data) 
            throws IOException {
        ObjectMapper mapper = new ObjectMapper();
        String jsonData = mapper.writeValueAsString(data);
        emitter.send("data: " + jsonData + "\n\n");
    }
    
    // 이벤트 타입을 지정한 데이터 전송
    public static void sendNamedEvent(SseEmitter emitter, String eventName, String data) 
            throws IOException {
        emitter.send("event: " + eventName + "\n");
        emitter.send("data: " + data + "\n\n");
    }
    
    // ID를 포함한 데이터 전송
    public static void sendEventWithId(SseEmitter emitter, String id, String data) 
            throws IOException {
        emitter.send("id: " + id + "\n");
        emitter.send("data: " + data + "\n\n");
    }
    
    // 완전한 이벤트 정보를 포함한 전송
    public static void sendCompleteEvent(SseEmitter emitter, String id, String eventName, 
                                       String data, int retryMs) throws IOException {
        emitter.send("id: " + id + "\n");
        emitter.send("event: " + eventName + "\n");
        emitter.send("retry: " + retryMs + "\n");
        emitter.send("data: " + data + "\n\n");
    }
}
```
<br>
<br>

### 실제 사용 예시 - 전송 후 연결 종료
```java
@RestController
public class EventController {
    
    @GetMapping(value = "/events", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public SseEmitter streamEvents() {
        SseEmitter emitter = new SseEmitter();
        
        try {
            // 단순 메시지 전송
            SSEEventBuilder.sendSimpleMessage(emitter, "안녕하세요");
            
            // JSON 형태의 복잡한 데이터 전송
            Map<String, Object> notification = new HashMap<>();
            notification.put("type", "notification");
            notification.put("message", "새 메시지");
            notification.put("timestamp", System.currentTimeMillis());
            SSEEventBuilder.sendJsonData(emitter, notification);
            
            // 특정 이벤트 타입으로 전송
            SSEEventBuilder.sendNamedEvent(emitter, "user-message", "사용자 메시지");
            
            // ID와 재연결 설정을 포함한 전송
            SSEEventBuilder.sendCompleteEvent(emitter, "12345", "reconnect", "재연결 설정", 5000);
            
        } catch (IOException e) {
            emitter.completeWithError(e);
        }
        
        return emitter;
    }
}
```
<br>
<br>

### 실제 사용 예시 - 로그인 시 알림 전송 (연결된 사용자에게만)
```java
@RestController
public class ScalableNotificationController {
    
    @Autowired
    private OptimizedSSEManager sseManager;
    
    @Autowired
    private HybridNotificationService notificationService;
    
    // 실시간 알림이 필요한 사용자만 연결
    @GetMapping(value = "/events", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public ResponseEntity<SseEmitter> streamEvents(HttpServletRequest request) {
        String userId = getCurrentUserId(request);
        
        // 연결 가능 여부 확인
        if (!sseManager.canConnect(userId)) {
            return ResponseEntity.status(HttpStatus.SERVICE_UNAVAILABLE)
                .body(null);
        }
        
        try {
            SseEmitter emitter = sseManager.createConnection(userId);
            return ResponseEntity.ok(emitter);
        } catch (Exception e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body(null);
        }
    }
    
    // 로그인 시 알림 전송 (연결된 사용자에게만)
    @PostMapping("/login")
    public ResponseEntity<String> login(@RequestBody LoginRequest request) {
        // 로그인 처리...
        
        // 연결된 사용자에게만 실시간 알림
        notificationService.sendNotification("all-connected-users", 
            "사용자 " + request.getUsername() + "님이 로그인했습니다.");
        
        return ResponseEntity.ok("로그인 성공");
    }
}
```
<br>
<hr>
<br>

**Reference**<br>

[brad.min : SSE(Server-Sent-Event) 구현하기](https://techbrad.tistory.com/82)<br>
[Junseo Kim : Server-Sent Events를 이용한 실시간 알림](https://velog.io/@max9106/Spring-SSE-Server-Sent-Events%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%8B%A4%EC%8B%9C%EA%B0%84-%EC%95%8C%EB%A6%BC)
