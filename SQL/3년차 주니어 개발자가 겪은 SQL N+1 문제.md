# 3년차 주니어 개발자의 N+1 문제 삽질 기록
- 오늘 개발 과정 중 Mybatis 환경에서의 N+1문제를 만나 기록을 해본다.
<br>

### 개발 과정
1. 대량 주문건(1,000 ~ 10,000)에 대한 상세 정보를 전부 조회해야한다.
2. 미리 셋팅된 사은품을 조회한다.
3. 사은품 적용 요건에 맞는 주문에는 각각에 맞는 사은품을 적용해야한다.<br>
이 때, 바로 적용하는 것이 아니고 1차로 미리보기로 적용될 사은품을 보여준다.<br>
이후 적용 버튼을 통해 각 주문건에 대해 사은품을 적용한다.
<br>
<br>

### 문제 상황
- 평소 나의 개발 습관은 쿼리를 단순하게

- 복잡한 JOIN은 피하고

- 서비스 레이어에서 조합

- 위 3개 습관을 통해 개발하다가 대량 주문건에 대해 사은품을 붙이려니 N+1 문제가 발생해 주문 1,000건 * N 이라는 어마어마한 쿼리가 실행되면서
수십, 수백만개의 주문건이 아님에도 1~2분이 넘어가는 사태가 발생하였다.
<br>
<hr>
<br>

## ✔️ 서비스 로직
- 실제 서비스 로직은 더 복잡하나, 예시를 들기위해 간단하게 작성하였다.

```java
@Service
@RequiredArgsConstructor
public class OrderService {
    
    private final OrderDao orderDao;
    private final CustomerDao customerDao;
    
    public List getOrdersWithCustomers() {
        
        // 1번 쿼리
        List orders = orderDao.findAll();
        
        List result = new ArrayList<>();
        
        for (OrderDto order : orders) {
            // 여기가 문제
            CustomerDto customer = customerDao.findById(order.getCustomerId());
            
            OrderResponseDto response = new OrderResponseDto();
            response.setOrderId(order.getId());
            response.setAmount(order.getAmount());
            response.setCustomerName(customer.getName());
            result.add(response);
        }
        
        return result;
    }
}
```
<br>

- 1번 쿼리에 대한 각각의 정보를 확인하기 위하여 루프안에서 쿼리를 또 실행하니 N+1 문제가 발생한 것이다.

- 데이터가 적을 때는 몰랐지만 많으면 많을 수록 로딩 시간이 길어졌다.
<br>
<hr>
<br>

## ✔️ 해결
### 방법 1: JOIN
```java
public List getOrdersWithCustomers() {
    return orderDao.findOrdersWithCustomers();  // JOIN으로 1번에 끝
}
```

### 방법 2: IN 절
```java
public List getOrdersWithCustomers() {
    
    // 1번째 쿼리
    List orders = orderDao.findAll();
    
    if (orders.isEmpty()) {
        return Collections.emptyList();
    }
    
    // ID 모으기
    List customerIds = orders.stream()
        .map(OrderDto::getCustomerId)
        .distinct()
        .toList();
    
    // 2번째 쿼리: 한 방에 조회
    Map customerMap = customerDao.findByIds(customerIds)
        .stream()
        .collect(Collectors.toMap(CustomerDto::getId, c -> c));
    
    // 조합
    return orders.stream()
        .map(order -> {
            CustomerDto customer = customerMap.get(order.getCustomerId());
            
            OrderResponseDto response = new OrderResponseDto();
            response.setOrderId(order.getId());
            response.setAmount(order.getAmount());
            response.setCustomerName(customer.getName());
            return response;
        })
        .toList();
}
```
<br>
<hr>
<br>

## ✔️ 오늘 배운 것
### 쿼리 분리 자체가 나쁜 게 아니었다
```java
// 이게 문제
for (Order order : orders) {
    customerDao.findById(order.getCustomerId());  // N번 호출
}

// 이건 괜찮음
List ids = orders.stream().map(Order::getCustomerId).distinct().toList();
customerDao.findByIds(ids);  // 1번 호출
```

- 루프 안에서 쿼리 치는 게 문제였다.
<br>
<br>

### JOIN도 나쁜 게 아니었다
- 무조건 쿼리 쪼개는 게 좋은 줄 알았는데, 상황에 따라 다르다.

| 상황 | 추천 |
|------|------|
| 항상 같이 쓰는 데이터 | JOIN |
| 가끔 필요한 데이터 | IN 절 분리 |
| 캐싱 쓸 수 있을 때 | IN 절 분리 |
<br>
<hr>
<br>

## ✔️ 결론
- N+1 문제 = 루프 안에서 쿼리 호출

- 이 패턴만 기억하자.
```java
List ids = entities.stream()
    .map(Entity::getForeignKeyId)
    .distinct()
    .toList();

Map map = relatedDao.findByIds(ids)
    .stream()
    .collect(Collectors.toMap(Related::getId, e -> e));
```
<br>

- 3년 동안 몰랐던 걸 오늘 알았다. 부끄럽지만 성장했으니 됐다.
  
