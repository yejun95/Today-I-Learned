# JPA Entity의 id를 Long 타입으로 주는 이유
- JPA의 Entity 구성 시 id 컬럼을 보통 `int, long, Integer`로 주지 않고 `Long` 타입으로 많이 주고는 한다.

- 왜 그런 것인지 알아보자.
<br>
<hr>
<br>

## ✔️ int (기본형) 분석
```java
@Entity
public class User {
    @Id
    private int id;  // 32비트: -2,147,483,648 ~ 2,147,483,647
}
```
<br>

### Entity로 쓰지 못하는 이유
- 범위 제한 : 약 21억 개만 표현 가능
  - 데이터 양이 많지 않으면 상관없으나, 대규모 시스템의 경우 21억개로는 부족하다.
<br>

- NULL 불가 : 값이 없으면 0으로 초기화하기 때문에 새 엔티티 상태 표현이 어렵다.
```java
public void saveUser(User user) {
    if (user.getId() == 0) {
        // 새 엔티티인지, ID가 0인 기존 엔티티인지 구분 불가
        userRepository.save(user);
    }
}
```
<br>
<hr>
<br>

## ✔️ Integer (래퍼 클래스) 분석
```java
@Entity
public class User {
    @Id
    private Integer id;  // 32비트 + 객체 특성
}
```
<br>

### Entity로 쓰지 못하는 이유
- NULL 허용이 돼서 새 엔티티 상태 표현이 가능하지만 `int`와 동일한 21억 개 제한
```java
@Entity
public class User {
    @Id
    private Integer id;
    
    public boolean isNew() {
        return id == null;  // ✅ NULL 체크 가능
    }
}
```
<br>
<hr>
<br>

## ✔️ long (기본형) 분석
```java
@Entity
public class User {
    @Id
    private long id;  // 64비트: 약 9경 개 표현 가능
}
```
<br>

### Entity로 쓰지 못하는 이유
- 64비트로 거의 무제한으로 ID 생성이 가능하지만 기본형이기 때문에 `NULL`이 불가능하다.
  - 즉, 새 엔티티 상태가 0으로 초기화돼서 사용 불가능
<br>

```java
@Entity
public class User {
    @Id
    private long id;  // 기본값 0
    
    // 문제: ID가 0인 경우와 새 엔티티 구분 불가
    public boolean isNew() {
        return id == 0;  // ❌ 부정확한 판단
    }
}
```
<br>
<hr>
<br>

## ✔️ Long (래퍼 클래스) 분석
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;  // 64비트 + 객체 특성 + NULL 허용
}
```
<br>

### Entity로 사용하는 이유
- 넓은 저장 범위

- NULL 허용

- 명확한 상태 구분
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    // ✅ 명확한 상태 구분
    public boolean isNew() {
        return id == null;  // Transient 상태
    }
    
    public boolean isPersistent() {
        return id != null;  // Persistent 상태
    }
}

// ✅ 올바른 사용
public void saveUser(User user) {
    if (user.getId() == null) {
        // 명확히 새 엔티티
        userRepository.save(user);
    } else {
        // 기존 엔티티 업데이트
        userRepository.save(user);
    }
}
```
> 새 엔티티인지 기존 엔티티 인지 구분이 가능
<br>
<hr>
<br>

## ✔️ 종합 비교표
| 특성 | int | Integer | long | Long |
|------|-----|---------|------|------|
| 범위 | 21억 | 21억 | 9경 | 9경 |
| NULL 허용 | ❌ | ✅ | ❌ | ✅ |
| 메모리 사용 | 4바이트 | 24바이트 | 8바이트 | 24바이트 |
| 연산 속도 | 빠름 | 느림 | 빠름 | 느림 |
| JPA 생명주기 | 문제 | 가능 | 문제 | 가능 |
| 확장성 | 부족 | 부족 | 충분 | 충분 |
| 실무 적합성 | ❌ | ❌ | ❌ | ✅ |
