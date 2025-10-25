# @Async 어노테이션은 무엇인가?
- 스프링 프레임워크에서 제공하는 어노테이션으로, 메서드를 비동기적으로 실행할 수 있게 해주는 기능

- 즉, 메서드 호출 후 다음 코드로 진행하며, 결과는 나중에 받는다.
<br>
<hr>
<br>

## ✔️ 사용 목적
- 시간이 오래걸리는 로직에서 유저 경험을 높일 수 있도록 핵심 로직만 동기 처리를 하고, 그 외 나머지는 비동기 처리를 함으로써<br>
서버의 응답 시간을 줄일 수 있다.

- 즉, 유저에게 응답을 필수로 줘야하는 부분을 제외하고 비동기로 처리하는 것이다.
<br>

### 문제 상황 (동기 처리)
```java
@Service
public class OrderService {
    
    public void processOrder(Order order) {
        // 1. 주문 검증 (1초 소요)
        validateOrder(order);
        
        // 2. 결제 처리 (3초 소요) - 외부 API 호출
        processPayment(order);
        
        // 3. 이메일 발송 (2초 소요) - 외부 서비스 호출
        sendEmail(order);
        
        // 총 6초 소요! 사용자는 6초간 대기
    }
}
```
<br>
<br>

### 해결책 (비동기 처리)
```java
@Service
public class OrderService {
    
    public void processOrder(Order order) {
        // 1. 주문 검증 (1초 소요) - 동기 처리
        validateOrder(order);
        
        // 2. 결제 처리 - 비동기 처리
        processPaymentAsync(order);
        
        // 3. 이메일 발송 - 비동기 처리
        sendEmailAsync(order);
        
        // 총 1초 소요! 사용자는 즉시 응답 받음
    }
    
    @Async
    public void processPaymentAsync(Order order) {
        // 결제 처리 로직
        paymentService.process(order);
    }
    
    @Async
    public void sendEmailAsync(Order order) {
        // 이메일 발송 로직
        emailService.send(order);
    }
}
```
<br>
<hr>
<br>

## ✔️ 문제점 분석
- 위 사례 코드의 문제점을 분석해보자.
<br>

### 현 코드 문제점
```java
public void processOrder(Order order) {
    validateOrder(order);           // ✅ 검증만 완료
    processPaymentAsync(order);     // ❌ 결제 실패해도 모름
    sendEmailAsync(order);          // ❌ 이메일 실패해도 모름
    
    // 프론트엔드로 "성공" 응답 전송
    // 하지만 실제로는 결제나 이메일이 실패했을 수 있음!
}
```
<br>
<br>

### 실제 시나리오
- 사용자: "주문 완료됐나요?"

- 시스템: "네, 완료되었습니다!" 

- 실제상황: 결제 실패, 이메일 발송 실패... 😱
<br>
<br>

### 해결법 1 : 핵심 작업은 동기, 부가 작업만 비동기
- 비동기로 처리된 작업에 대하여 재시도 로직이 포함되어야 한다.

```java
@Service
public class OrderService {
    
    public OrderResult processOrder(Order order) {
        // 1. 주문 검증 (필수)
        validateOrder(order);
        
        // 2. 결제 처리 (핵심 비즈니스 로직 - 동기)
        PaymentResult paymentResult = paymentService.process(order);
        if (!paymentResult.isSuccess()) {
            throw new PaymentException("결제 실패: " + paymentResult.getMessage());
        }
        
        // 3. 주문 상태 업데이트 (핵심)
        order.setStatus(OrderStatus.PAID);
        orderRepository.save(order);
        
        // 4. 부가 작업들 - 비동기 처리
        sendEmailAsync(order);
        updateInventoryAsync(order);
        sendNotificationAsync(order);
        
        return new OrderResult(order.getId(), "주문이 성공적으로 처리되었습니다.");
    }
    
    @Async
    public void sendEmailAsync(Order order) {
        try {
            emailService.send(order);
            log.info("이메일 발송 완료: {}", order.getId());
        } catch (Exception e) {
            log.error("이메일 발송 실패: {}", order.getId(), e);
            // 실패 시 재시도 로직이나 알림 시스템에 등록
            retryEmailService.scheduleRetry(order);
        }
    }
    
    @Async
    public void updateInventoryAsync(Order order) {
        try {
            inventoryService.updateStock(order);
            log.info("재고 업데이트 완료: {}", order.getId());
        } catch (Exception e) {
            log.error("재고 업데이트 실패: {}", order.getId(), e);
            // 재고 업데이트 실패 시 관리자 알림
            alertService.notifyAdmin("재고 업데이트 실패", order.getId());
        }
    }
}
```
<br>
<br>

### 해결법 2 : 상태 기반 처리 (Saga 패턴)
- 우선 유저에게는 "처리 중" 이라는 응답을 보여주고, 이후에 서버에서 로직을 동작시킨다.

- 만약 최종 실패 시, 유저에게 실패 응답 출력

```java
@Service
public class OrderService {
    
    public OrderResult processOrder(Order order) {
        // 1. 주문 생성 (검증 포함)
        validateOrder(order);
        order.setStatus(OrderStatus.PENDING);
        orderRepository.save(order);
        
        // 2. 비동기로 전체 프로세스 시작
        processOrderAsync(order);
        
        return new OrderResult(order.getId(), "주문이 접수되었습니다. 처리 중입니다.");
    }
    
    @Async
    public void processOrderAsync(Order order) {
        try {
            // 결제 처리
            PaymentResult paymentResult = paymentService.process(order);
            if (paymentResult.isSuccess()) {
                order.setStatus(OrderStatus.PAID);
                orderRepository.save(order);
                
                // 이메일 발송
                sendEmailAsync(order);
                
                // 재고 업데이트
                updateInventoryAsync(order);
                
            } else {
                // 결제 실패
                order.setStatus(OrderStatus.PAYMENT_FAILED);
                orderRepository.save(order);
                
                // 실패 알림
                notifyPaymentFailure(order, paymentResult.getMessage());
            }
            
        } catch (Exception e) {
            // 전체 프로세스 실패
            order.setStatus(OrderStatus.FAILED);
            orderRepository.save(order);
            
            log.error("주문 처리 실패: {}", order.getId(), e);
            notifyOrderFailure(order, e.getMessage());
        }
    }
    
    @Async
    public void sendEmailAsync(Order order) {
        try {
            emailService.send(order);
            log.info("이메일 발송 완료: {}", order.getId());
        } catch (Exception e) {
            log.error("이메일 발송 실패: {}", order.getId(), e);
            // 이메일 실패는 주문 상태에 영향 없음 (재시도만)
            retryEmailService.scheduleRetry(order);
        }
    }
}
```
<br>
<br>

### 실무에서 권장파는 패턴
- 핵심 비즈니스 로직은 동기 처리
```java
public OrderResult processOrder(Order order) {
    // 필수 검증
    validateOrder(order);
    
    // 핵심 비즈니스 로직 (결제)
    PaymentResult paymentResult = paymentService.process(order);
    if (!paymentResult.isSuccess()) {
        throw new PaymentException("결제 실패");
    }
    
    // 주문 상태 업데이트
    order.setStatus(OrderStatus.PAID);
    orderRepository.save(order);
    
    // 부가 서비스만 비동기
    sendEmailAsync(order);
    updateInventoryAsync(order);
    
    return new OrderResult(order.getId(), "주문 완료");
}
```
<br>
<br>

- 비동기 메서드 실패 처리 전략
```java
@Async
public void sendEmailAsync(Order order) {
    try {
        emailService.send(order);
    } catch (Exception e) {
        // 1. 로그 기록
        log.error("이메일 발송 실패: {}", order.getId(), e);
        
        // 2. 재시도 큐에 등록
        retryQueue.add(new EmailRetryTask(order));
        
        // 3. 관리자 알림
        alertService.notifyAdmin("이메일 발송 실패", order.getId());
        
        // 4. 대체 수단 (SMS 등)
        smsService.send(order.getPhone(), "주문 완료 알림");
    }
}
```
<br>
<br>

## ✔️ 정리
- 비동기 처리는 부가 기능에만 사용하고, 핵심 비즈니스 로직은 동기 처리해야 한다.

- ✅ 결제, 주문 생성 → 동기 처리

- ✅ 이메일, 알림, 로그 → 비동기 처리

- ✅ 실패 시 적절한 복구 전략 필요
