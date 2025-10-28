# @Async 비동기 처리의 실패 전략
- 자바의 `@Async` 어노테이션은 메서드를 비동기로 처리할 수 있게 한다.

- 하지만 비동기로 처리된 내용을 클라이언트로 리턴해야할 때, 해당 로직이 실패하게 된다면 어떻게 처리해야 할까?
  - ex) 비동기 처리 후 클라이언트로 메세지 전송
  - ex) 주문 로직에 대한 비동기 처리 후 클라이언트에서 완료 처리 필요
<br>

- 서버 내에서만 자체적으로 돌아가는 로직이라면 실패하여도 티가 나지 않겠지만,<br>
만약 클라이언트로 응답해야되는 값이 실패한다면 유저 경험이 현저하게 낮아질 것이다.
```java
@Async
public void processOrder(Long orderId) {
    // 이 메서드에서 예외가 발생해도 호출자는 알 수 없음
    // 비동기로 돌아가기 때문에 메인 스레드와 분리됨
}
```

- 때문에 비동기 처리에 대한 실패 전략을 어떻게 수립해야 하는지 알아보자.
<br>
<hr>
<br>

## ✔️ 비동기 처리 실패 전략
### 실패 전략 1: 작업 상태 추적 + 폴링
- 비동기 작업을 시작하고 상태를 DB에 저장

- 클라이언트는 폴링으로 확인

**백엔드 서버**
```java
@Service
@RequiredArgsConstructor
public class OrderService {
    
    private final OrderRepository orderRepository;
    private final AsyncOrderProcessor asyncOrderProcessor;
    
    public OrderResponse createOrder(OrderRequest request) {
        Order order = Order.builder()
            .status(OrderStatus.PENDING)
            .amount(request.getAmount())
            .build();
        
        Order savedOrder = orderRepository.save(order);
        
        // 비동기 처리 시작
        asyncOrderProcessor.processOrderAsync(savedOrder.getId());
        
        // 바로 클라이언트에게 반환 (상태만 반환)
        return OrderResponse.builder()
            .orderId(savedOrder.getId())
            .status("PROCESSING")
            .message("주문이 처리 중입니다. 잠시 후 결과를 확인해주세요.")
            .build();
    }
}

@Service
@RequiredArgsConstructor
public class AsyncOrderProcessor {
    
    private final OrderRepository orderRepository;
    private final AsyncUncaughtExceptionHandler asyncExceptionHandler;
    
    @Async("taskExecutor")
    @Override
    public CompletableFuture<Order> processOrderAsync(Long orderId) {
        try {
            // 실제 처리 로직
            Order order = orderRepository.findById(orderId)
                .orElseThrow(() -> new OrderNotFoundException("주문을 찾을 수 없습니다."));
            
            // 주문 처리 시뮬레이션 (예: 결제 처리, 재고 차감)
            Thread.sleep(2000);
            
            if (shouldFail()) { // 예시: 랜덤 실패
                throw new RuntimeException("주문 처리 중 오류 발생");
            }
            
            order.setStatus(OrderStatus.COMPLETED);
            return CompletableFuture.completedFuture(orderRepository.save(order));
            
        } catch (Exception e) {
            // 실패 전략: 상태를 FAILED로 저장
            Order order = orderRepository.findById(orderId).orElse(null);
            if (order != null) {
                order.setStatus(OrderStatus.FAILED);
                order.setFailureReason(e.getMessage());
                orderRepository.save(order);
            }
            
            // 로깅
            asyncExceptionHandler.handleUncaughtException(e);
            
            return CompletableFuture.failedFuture(e);
        }
    }
    
    private boolean shouldFail() {
        return Math.random() < 0.3; // 30% 확률로 실패
    }
}

// 클라이언트가 상태를 확인하는 엔드포인트
@RestController
@RequiredArgsConstructor
public class OrderStatusController {
    
    private final OrderRepository orderRepository;
    
    @GetMapping("/orders/{orderId}/status")
    public ResponseEntity<OrderStatusResponse> getOrderStatus(@PathVariable Long orderId) {
        Order order = orderRepository.findById(orderId)
            .orElseThrow(() -> new OrderNotFoundException("주문을 찾을 수 없습니다."));
        
        return ResponseEntity.ok(OrderStatusResponse.builder()
            .status(order.getStatus())
            .failureReason(order.getFailureReason())
            .build());
    }
}
```
<br>
<br>

**클라이언트**
```java
// 1. 주문 생성
const order = await createOrder(orderData);
const orderId = order.orderId;

// 2. 상태 폴링
const checkStatus = async () => {
  const status = await getOrderStatus(orderId);
  
  if (status === 'COMPLETED') {
    console.log('주문 완료!');
  } else if (status === 'FAILED') {
    console.log('주문 실패: ', status.failureReason);
  } else if (status === 'PROCESSING') {
    // 3초 후 다시 확인
    setTimeout(checkStatus, 3000);
  }
};

checkStatus();
```
<br>
<br>

### 실패 전략 2: Callback 인터페이스
- 작업 완료/실패 시 콜백을 수행

```java
public interface OrderCallback {
    void onSuccess(Order order);
    void onFailure(Long orderId, Exception e);
}

@Service
@RequiredArgsConstructor
public class OrderCallbackProcessor {
    
    private final OrderRepository orderRepository;
    private final AsyncOrderCallback asyncOrderCallback;
    
    public void processOrderWithCallback(OrderRequest request, OrderCallback callback) {
        Order order = Order.builder()
            .status(OrderStatus.PENDING)
            .amount(request.getAmount())
            .build();
        
        Order savedOrder = orderRepository.save(order);
        
        // 비동기 처리 + 콜백 등록
        asyncOrderCallback.processOrderAsync(savedOrder.getId(), callback);
    }
}

@Service
public class AsyncOrderCallback {
    
    private final OrderRepository orderRepository;
    
    @Async("taskExecutor")
    public void processOrderAsync(Long orderId, OrderCallback callback) {
        try {
            // 실제 처리
            Order order = orderRepository.findById(orderId)
                .orElseThrow(() -> new OrderNotFoundException("주문을 찾을 수 없습니다."));
            
            Thread.sleep(2000);
            
            if (Math.random() < 0.3) {
                throw new RuntimeException("주문 처리 실패");
            }
            
            order.setStatus(OrderStatus.COMPLETED);
            orderRepository.save(order);
            
            // 성공 콜백 실행
            callback.onSuccess(order);
            
        } catch (Exception e) {
            // 실패 콜백 실행
            callback.onFailure(orderId, e);
        }
    }
}

// 사용 예시
@Service
@RequiredArgsConstructor
public class WebSocketNotificationService implements OrderCallback {
    
    private final SimpMessagingTemplate messagingTemplate;
    
    @Override
    public void onSuccess(Order order) {
        // WebSocket으로 클라이언트에게 성공 메시지 전송
        messagingTemplate.convertAndSend("/topic/orders/" + order.getId(), 
            Map.of("status", "SUCCESS", "order", order));
    }
    
    @Override
    public void onFailure(Long orderId, Exception e) {
        // WebSocket으로 실패 메시지 전송
        messagingTemplate.convertAndSend("/topic/orders/" + orderId,
            Map.of("status", "FAILED", "error", e.getMessage()));
    }
}
```
<br>
<br>

### 실패 처리 전략 3: Spring Retry
- 재시도 로직을 추가해 일시적 오류를 회피

```java
@Service
@RequiredArgsConstructor
public class ResilientOrderProcessor {
    
    private final OrderRepository orderRepository;
    
    @Async("taskExecutor")
    @Retryable(
        value = {Exception.class},
        maxAttempts = 3,  // 최대 3번 재시도
        backoff = @Backoff(delay = 1000, multiplier = 2)  // 1초, 2초, 4초 간격
    )
    public CompletableFuture<Order> processOrderWithRetry(Long orderId) {
        Order order = orderRepository.findById(orderId)
            .orElseThrow(() -> new OrderNotFoundException("주문을 찾을 수 없습니다."));
        
        // 처리 로직
        order.setStatus(OrderStatus.COMPLETED);
        return CompletableFuture.completedFuture(orderRepository.save(order));
    }
    
    // 모든 재시도 실패 후 실행되는 메서드
    @Recover
    public CompletableFuture<Order> recover(Exception e, Long orderId) {
        Order order = orderRepository.findById(orderId).orElse(null);
        if (order != null) {
            order.setStatus(OrderStatus.FAILED);
            order.setFailureReason("최대 재시도 횟수 초과: " + e.getMessage());
            orderRepository.save(order);
        }
        return CompletableFuture.failedFuture(e);
    }
}
```
<br>
<br>

### 실패 전략 4: AsyncUncaughtExceptionHandler
- 비동기 예외를 전역 처리

```java
@Configuration
@EnableAsync
public class AsyncConfig implements AsyncConfigurer {
    
    @Bean(name = "taskExecutor")
    public Executor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(100);
        executor.setThreadNamePrefix("async-task-");
        executor.initialize();
        return executor;
    }
    
    @Override
    public AsyncUncaughtExceptionHandler getAsyncUncaughtExceptionHandler() {
        return new CustomAsyncExceptionHandler();
    }
}

@Component
@Slf4j
public class CustomAsyncExceptionHandler implements AsyncUncaughtExceptionHandler {
    
    @Override
    public void handleUncaughtException(Throwable ex, Method method, Object... params) {
        log.error("비동기 메서드 실행 중 예외 발생: method={}, params={}", 
            method.getName(), params, ex);
        
        // 알림 서비스로 실패 정보 전송
        sendFailureNotification(method.getName(), params, ex);
        
        // 실패 로그를 DB에 저장 (옵션)
        saveFailureLog(method.getName(), params, ex);
    }
    
    private void sendFailureNotification(String method, Object[] params, Throwable ex) {
        // Slack, 이메일 등의 알림 전송
        log.info("관리자에게 알림 전송: {} 메서드 실패", method);
    }
    
    private void saveFailureLog(String method, Object[] params, Throwable ex) {
        // 실패 로그를 DB에 저장하는 로직
    }
}
```
<br>
<hr>
<br>

## ✔️ 정리
| 전략 | 사용 시기 | 장점 | 단점 |
| --- | --- | --- | --- |
| 상태 추적 + 폴링 | 장시간 처리 작업 | 구현 단순, 상태 추적 용이 | 폴링 부담, 실시간 반영 지연 |
| Callback | 클라이언트 알림 필요 | 실시간 피드백 | 복잡도 증가 |
| Retry | 일시적 오류 회피 | 자동 복구 가능 | 재시도 설정 부담 |
| Exception Handler | 전역 예외 관리 | 중앙화된 처리 | 로직 위치 고민 필요 |
