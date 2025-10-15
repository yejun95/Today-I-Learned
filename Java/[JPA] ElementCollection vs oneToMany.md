# ElementCollection vs oneToMany
- 핵심 차이점은 아래와 같다.

- `@ElementCollection` : 값 타입 컬렉션 (독립적인 생명주기 없음)
  - 엔티티와 기본 타입 또는 임베더블 클래스 사이의 일대다 관계를 나타냄
  - 컬렉션은 엔티티의 속성 중 하나로 취급되며, 컬렉션의 요소는 별도의 엔티티가 아니라 단순한 값
  - 데이터베이스에서는 컬렉션의 각 요소가 별도의 테이블이 아닌, 부모 엔티티 테이블에 포함되는 방식으로 저장
<br>

- `@OneToMany` : 엔티티 컬렉션 (독립적인 생명주기 있음)
  - 엔티티와 다른 엔티티 간의 일대다 관계를 나타냄
  - 부모 엔티티는 여러 자식 엔티티를 가질 수 있으며, 자식 엔티티는 부모 엔티티에 대한 참조를 가지고 있음
  - 데이터베이스에서는 보통 외래 키를 사용하여 부모 엔티티와 자식 엔티티 간의 관계를 유지
<br>
<hr>
<br>

## ✔️ ElementCollection
```java
@Entity
public class User {
    @Id @GeneratedValue
    private Long id;
    
    private String name;
    
    @ElementCollection
    @CollectionTable(name = "user_phones")
    private List<String> phoneNumbers = new ArrayList<>();
    
    @ElementCollection
    @CollectionTable(name = "user_addresses")
    private List<Address> addresses = new ArrayList<>();
}

@Embeddable
public class Address {
    private String city;
    private String street;
    private String zipCode;
}
```
<br>

### 특징
- 별도의 ID가 없는 값 타입(value Type)

- 부모 엔티티에 완전히 종속

- 부모가 삭제되면 자동으로 삭제

- 독립적으로 조회/수정 불가
<br>
<hr>
<br>

## ✔️ OneToMany
```java
@Entity
public class Team {
    @Id @GeneratedValue
    private Long id;
    
    private String name;
    
    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<>();
}

@Entity
public class Member {
    @Id @GeneratedValue
    private Long id;
    
    private String name;
    
    @ManyToOne
    @JoinColumn(name = "team_id")
    private Team team;
}
```
<br>

- 독립적인 엔티티

- 각자 고유한 ID 보유

- 독립적인 생명주기

- 독립적으로 조회/수정 가능
<br>
<hr>
<br>

## ✔️ 실제 사례로 이해하기
### 사례 1 : 사용자 전화번호 (ElementCollection 적합)
```java
@Entity
public class User {
    @Id @GeneratedValue
    private Long id;
    
    private String name;
    
    @ElementCollection
    @CollectionTable(
        name = "user_phone_numbers",
        joinColumns = @JoinColumn(name = "user_id")
    )
    @Column(name = "phone_number")
    private Set<String> phoneNumbers = new HashSet<>();
}

// 사용 예시
User user = new User("홍길동");
user.getPhoneNumbers().add("010-1234-5678");
user.getPhoneNumbers().add("02-123-4567");
userRepository.save(user);
```
<br>
<br>

**DB 구조**
```
-- users 테이블
id | name
1  | 홍길동

-- user_phone_numbers 테이블
user_id | phone_number
1       | 010-1234-5678
1       | 02-123-4567
```
<br>

**왜 ElementCollection?**
- 전화번호는 독립적인 의미가 없음

- 사용자 없이 전화번호만 조회할 일이 없음

- 단순한 값의 집합
<br>
<br>

### 사례 2: 주문과 주문상품 (OneToMany 적합)
```java
@Entity
public class Order {
    @Id @GeneratedValue
    private Long id;
    
    private LocalDateTime orderDate;
    
    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL)
    private List<OrderItem> orderItems = new ArrayList<>();
    
    // 연관관계 편의 메서드
    public void addOrderItem(OrderItem orderItem) {
        orderItems.add(orderItem);
        orderItem.setOrder(this);
    }
}

@Entity
public class OrderItem {
    @Id @GeneratedValue
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "order_id")
    private Order order;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "product_id")
    private Product product;
    
    private int orderPrice;
    private int count;
}

// 사용 예시
Order order = new Order();
OrderItem item1 = new OrderItem(product1, 10000, 2);
OrderItem item2 = new OrderItem(product2, 20000, 1);

order.addOrderItem(item1);
order.addOrderItem(item2);
orderRepository.save(order);

// 주문상품을 독립적으로 조회
OrderItem foundItem = orderItemRepository.findById(item1.getId());
```
<br>

**왜 OneToMany?**
- OrderItem은 독립적인 엔티티

- 고유한 ID 필요 (환불, 교환 등 개별 처리)

- 주문 없이도 주문상품 통계 조회 가능

- 다른 엔티티(Product)와도 연관관계 보유
<br>
<br>

### 선택 기준표
| 기준 | @ElementCollection | @OneToMany |
|:---:|:---:|:---:|
| **독립적인 ID 필요?** | ❌ 불필요 | ✅ 필요 |
| **독립적인 조회/수정?** | ❌ 불가 | ✅ 가능 |
| **다른 엔티티와 관계?** | ❌ 불가 | ✅ 가능 |
| **생명주기** | 부모에 종속 | 독립적 |
| **테이블** | 컬렉션 테이블 | 별도 엔티티 테이블 |
| **성능** | 전체 삭제 후 재생성 | 개별 수정 가능 |
<br>
<hr>
<br>

## ✔️ 실제 시나리오별 권장사항
### @ElementCollection이 적합한 경우
```java
// 1. 단순한 값의 집합
@Entity
public class Product {
    @ElementCollection
    @CollectionTable(name = "product_images")
    @Column(name = "image_url")
    private List<String> imageUrls = new ArrayList<>();
    
    // 2. 설정값들
    @ElementCollection
    @CollectionTable(name = "user_preferences")
    @MapKeyColumn(name = "preference_key")
    @Column(name = "preference_value")
    private Map<String, String> preferences = new HashMap<>();
    
    // 3. 태그나 카테고리
    @ElementCollection
    @Enumerated(EnumType.STRING)
    @CollectionTable(name = "product_categories")
    @Column(name = "category")
    private Set<Category> categories = new HashSet<>();
}
```
<br>
<br>

### @OneToMany가 적합한 경우
```java
// 1. 비즈니스 로직이 있는 관계
@Entity
public class Post {
    @OneToMany(mappedBy = "post", cascade = CascadeType.ALL)
    private List<Comment> comments = new ArrayList<>();
    
    public void addComment(Comment comment) {
        comments.add(comment);
        comment.setPost(this);
        // 비즈니스 로직: 댓글 수 제한 등
        if (comments.size() > 100) {
            throw new IllegalStateException("댓글 수 초과");
        }
    }
}

// 2. 독립적인 생명주기가 필요한 경우
@Entity
public class Order {
    @OneToMany(mappedBy = "order")
    private List<OrderItem> orderItems = new ArrayList<>();
    
    // 주문 취소 시 일부 상품만 환불
    public void refundOrderItems(List<Long> itemIds) {
        orderItems.stream()
            .filter(item -> itemIds.contains(item.getId()))
            .forEach(OrderItem::refund);
    }
}
```
<br>
<hr>
<br>

**Reference**<br>

[한결 : @ElementCollection vs @OneToMany](https://velog.io/@eoveol/JPA-ElementCollection-vs-OneToMany)<br>
[passionfruit200 : 회원과 회원권한관리에서 OneToMany 대신 ElementCollection을 사용하는 경우](https://passionfruit200.tistory.com/346)
