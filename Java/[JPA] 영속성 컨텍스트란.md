# 영속성 컨텍스트란?
- 엔티티를 영구적으로 저장하는 환경을 뜻한다.

- 애플리케이션과 데이터베이스 사이에서 객체를 보관하는 가상의 데이터베이스 역할이라고 보면 이해가 쉽다.

- EntityManager를 통해 엔티티를 저장하거나 조회하면 EntityManager는 영속성 컨텍스트에 엔티티를 보관하고 관리한다.

![image](https://github.com/user-attachments/assets/32398159-8b05-47c2-a311-2d8374c0bded)
> EntityManager는 EntityManagerFactory라는 인터페이스를 구현한다.
<br>

- Ex) `em.persist(member)`와 같이 EntityManager를 사용해 회원 엔티티를 저장하면<br>
바로 DB에 반영하지 않고 영속성 컨텍스트에 저장한다.
  - 이후 Flush 작업을 진행해서 DB에 반영한다.
  - `@Transactionl` 어노테이션을 필수로 선언해야 하는 이유
<br>
<hr>
<br>

## ✔️ 엔티티의 생명주기
![image](https://github.com/user-attachments/assets/13098d5a-74ce-43cf-8c17-81bdb9fbf5ec)
<br>

**➡️ 비영속(new/transient): 영속성 컨텍스트와 전혀 관계가 없는 상태**
- 엔티티 객체를 생성했지만 아직 영속성 컨텍스트에 저장하지 않은 상태를 비영속(new/transient)라 한다.

```java
Member member = new Member();
```
<br>

**➡️ 영속(managed): 영속성 컨텍스트에 저장된 상태**
- 엔티티 매니저를 통해서 엔티티를 영속성 컨텍스트에 저장한 상태를 말하며, 영속성 컨텍스트에 의해 관리된다는 뜻이다.

```java
em.persist(member);
```
<br>

**➡️ 준영속(detached): 영속성 컨텍스트에 저장되었다가 분리된 상태**
- 영속성 컨텍스트가 관리하던 영속 상태의 엔티티 더이상 관리하지 않으면 준영속 상태가 된다.

- 특정 엔티티를 준영속 상태로 만드려면 em.datach()를 호출하면 된다.

```java
// 엔티티를 영속성 컨텍스트에서 분리해 준영속 상태로 만든다.
em.detach(member);

// 영속성 콘텍스트를 비워도 관리되던 엔티티는 준영속 상태가 된다.
em.claer();

// 영속성 콘텍스트를 종료해도 관리되던 엔티티는 준영속 상태가 된다.
em.close();
```
<br>

**➡️ 삭제(removed): 삭제된 상태**
- 엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제한다.

```java
em.remove(member);
```
<br>
<hr>
<br>

## ✔️ 영속성 컨텍스트의 특징
![image](https://github.com/user-attachments/assets/d1ce9f8d-17ea-40bc-82fa-09aedbc5b70a)
> 그림으로 보는 영속성 컨텍스트 내부
<br>

**➡️ 영속성 컨텍스트의 식별자 값**
- 영속성 컨텍스트는 엔티티를 식별자 값으로 구분한다. 

- 따라서 영속 상태는 식별자 값이 반드시 있어야 한다.
<br>

**➡️ 영속성 컨텍스트와 데이터베이스 저장**
- JPA는 보통 트랜잭션을 커밋하는 순간 영속성 컨텍스트에 새로 저장된 엔티티를 데이터 베이스에 반영하는데 이를 flush라 한다.
<br>

**➡️ 영속성 컨텍스트가 엔티티를 관리하면 다음과 같은 장점이 있다.**
- 1차 캐시
  - 영속성 컨텍스트 내부에는 캐시가 있는데 이를 1차 캐시라고 한다.
  - 영속 상태의 엔티티를 이곳에 저장한다.
  - 1차 캐시의 키는 식별자 값(데이터베이스의 기본 키)이고 값은 엔티티 인스턴스이다.
  - 조회하는 방법은 아래와 같다.

```java
// em.find(엔티티 클래스 타입, 식별자 값);
Member member = em.find(Member.class, "member1");
```

- 1차 캐시 조회의 흐름
  - 1차 캐시에서 엔티티를 찾는다
  - 있으면 메모리에 있는 1차 캐시에서 엔티티를 조회한다.
  - 없으면 데이터베이스에서 조회한다.
  - 조회한 데이터로 엔티티를 생성해 1차 캐시에 저장한다. (엔티티를 영속상태로 만든다)
  - 조회한 엔티티를 반환한다.

```java
Member member = new Member();
member.setId("member1")
member.setUsername("회원1");

//1차 캐시에 저장됨
em.persist(member);

//1차 캐시에서 조회
Member member1 = em.find(Member.class, "member1");
```

![image](https://github.com/user-attachments/assets/06e3c155-44ba-4a27-8429-0e72f55090e0)
> 1차 캐시에서 찾음
<br>

```java
Member member2 = em.find(Member.class, "member2");
```

![image](https://github.com/user-attachments/assets/12c73bc3-60ca-40bc-8fc3-b1d4935db4d8)
> 1차 캐시에 없다면 db를 조회하고, 해당 데이터를 1차 캐시에 저장한다.
<br>

> 사실 1차 캐시는 큰 도움이 되지 않는다.<br>
이유1. 1차 캐시가 있는 EntityManager는 트랜잭션 단위로 만들고 사라진다. 즉, 1차 캐시가 살아있는 시간은 매우 짧아 성능에 큰 효과는 없다.<br>
이유2. 트랜잭션마다 각자 EntityManger를 사용한다. 즉, 각자 다른 영속성 컨텍스트와 1차 캐시를 가진다.<br>
<br>

- 동일성 보장
  - 영속성 컨텍스트는 엔티티의 동일성을 보장한다.
 
```java
Member a = em.find(Member.class, "member1");
Member b = em.find(Member.class, "member1");
System.out.print(a==b) // true
```
> 동일성 비교 : 실제 인스턴스가 같다. ==을 사용해 비교한다.<br>
동등성 비교 : 실제 인스턴스는 다를 수 있지만 인스턴스가 가지고 있는 값이 같다. equals()메소드를 구현해서 비교한다.
<br>

- 트랙잭션을 지원하는 쓰기 지연
  - em.find(member)를 사용해 member를 저장해도 바로 INSERT SQL이 DB에 보내지는 것이 아니다.
  - 엔티티 매니저는 트랜잭션을 커밋하기 직전까지 내부 쿼리 저장소에 INSERT SQL을 모아둔다.
  - 그리고 트랜잭션을 커밋할 때 모아둔 쿼리를 DB에 보낸다.
  - 이것을 트랜잭션을 지원하는 쓰기 지연이라 한다.
<br>

- 변경 감지
  - JPA로 엔티티를 수정할 때는 단순히 엔티티를 조회해서 데이터를 변경하면 된다.

- 변경감지의 흐름
  - 트랙잭션을 커밋하면 엔티티 매니저 내부에서 먼저 플러시가 호출된다.
  - 엔티티와 스냅샷을 비교하여 변경된 엔티티를 찾는다.
  - 변경된 엔티티가 있으면 수정 쿼리를 생성해서 쓰기 지연 SQL 저장소에 저장한다.
  - 쓰기 지연 저장소의 SQL을 플러시한다.
  - 데이터베이스 트랜잭션을 커밋한다.
> 변경 감지는 영속성 컨텍스트가 관리하는 영속 상태의 엔티티만 적용된다.
<br>

- 지연 로딩
<br>
<hr>
<br>

## ✔️ Flush
- 플러시는 영속성 컨텍스트의 변경 내용을 데이터베이스에 반영한다.

- 영속성 컨텍스트의 엔티티를 지우는게 아니라 변경 내용을 데이터베이스에 동기화하는 것이다.
<br>

**➡️ 플러시 흐름**
- 변경 감지가 동작해서 스냅샷과 비교해서 수정된 엔티티를 찾는다.

- 수정된 엔티티에 대해서 수정 쿼리를 만들고 SQL 저장소에 등록한다.

- 쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송한다.
<br>

**➡️ 플러시하는 방법**
- em.flush()

- 트랙잭션 커밋시 자동 호출

- JPQL 쿼리 실행시 자동 호출
<br>
<hr>
<br>

**Reference**<br>

[neptunes032.log : JPA 영속성 컨텍스트란?](https://velog.io/@neptunes032/JPA-%EC%98%81%EC%86%8D%EC%84%B1-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EB%9E%80)<br>
[잇트루 : [JPA] 영속성 컨텍스트(Persistence Context)란? - 개넘 정리 및 사용법](https://ittrue.tistory.com/254)<br>
[go_go_ : [JPA] 영속성 컨텍스트의 전반적인 이해(개념, 장점, 동작 방식)](https://velog.io/@suk13574/JPA-%EC%98%81%EC%86%8D%EC%84%B1-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EC%9D%98-%EC%A0%84%EB%B0%98%EC%A0%81%EC%9D%B8-%EC%9D%B4%ED%95%B4%EA%B0%9C%EB%85%90-%EC%9E%A5%EC%A0%90-%EB%8F%99%EC%9E%91-%EB%B0%A9)
