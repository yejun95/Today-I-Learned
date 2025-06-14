# static에 Autowird 사용이 불가능한 이유
- static 클래스나 필드는 객체가 아닌 클래스 수준에서 존재한다.

- Spring은 객체 인스턴스를 기준으로 DI를 수행하므로 static 대상에는 DI가 불가능하다.

- `@Autowired`는 Spring 컨테이너가 관리하는 빈 인스턴스의 필드에 의존성을 주입하는 반면, static 변수나 메서드는 클래스 레벨에서 존재하며, 특정 인스턴스와 연결되지 않는다.

- Spring의 빈은 애플리케이션 컨텍스트에 의해 관리되며, 그 생명주기는 애플리케이션 컨텍스트에 의해 제어되는 반면 static 필드는 클래스 로딩 시점에 초기화되며, 애플리케이션 컨텍스트와는 별개로 존재한다.

- 때문에 @Autowired된 빈을 static 클래스나 필드에 직접 사용할 수 없다.
<br>
<hr>
<br>

## ✔️ 테스트 코드
```java
@Service
public class MemberService {

    @Autowired
    private static MemberRepository memberRepository;

    public void testStaticClassAccess() {
        memberRepository.save();
    }

}
```
<br>

- 위 코드는 흔하게 볼 수 있는 Service에서의 Repository를 주입받는 형태이다.

- 그러나 해당 코드는 컴파일 단계에서부터 에러가 발생하게 되는데, static으로 선언된 필드를 통해 의존성을 주입받으려 했기 때문이다.

- 해당 코드를 테스트 코드에서 실행하여 보면 아래와 같은 결과가 나온다.
<br>

```java
@SpringBootTest
class MemberServiceTest {

    @Autowired
    MemberService memberService;

    @Test
    void staticInnerClassAutowiredTest() {
        memberService.testStaticClassAccess();
    }
}

-------------------------------------------------------------

Cannot invoke "org.zerock.springboot_learn.repository.MemberRepository.save()" because "org.zerock.springboot_learn.service.MemberService.memberRepository" is null
java.lang.NullPointerException: Cannot invoke "org.zerock.springboot_learn.repository.MemberRepository.save()" because "org.zerock.springboot_learn.service.MemberService.memberRepository" is null
```
> static으로 선언하면, Spring은 그 필드에 의존성 주입을 하지 않아 null이 된다.
<br>
<hr>
<br>

## ✔️ JVM과 Spring 관점에서의 설명
| 항목               | JVM                                | Spring Framework               |
| ---------------- | ---------------------------------- | ------------------------------ |
| 정적(static) 변수 관리 | 클래스 로딩 시 `static` 메모리 영역에 로드       | 관리 안 함 (빈 주입 불가)               |
| 객체 생성            | `new` 키워드, 클래스 로딩 시 `static` 블록 실행 | @Component, @Service 등으로 객체 관리 |
| @Autowired 처리    | ❌ 인식 못 함                           | ✅ 생성된 Bean에 의존성 주입 수행          |
| 메서드 호출 시점        | 클래스/인스턴스 로딩 순서에 따라                 | 컨테이너에서 Bean으로 꺼내 실행            |
<br>

### JVM 동작 과정
- 클래스 로딩 시점
  - MemberService 클래스가 로드되면, static 필드인 memberRepository는 메서드 영역(Method Area) 에 메모리 공간이 할당
  - 이 시점에서 Spring은 관여하지 않음.
  - 그냥 JVM이 클래스를 읽고 static 필드를 준비하는 단계
<br>

- Spring ApplicationContext 초기화
  - @Service로 선언된 MemberService가 Bean으로 등록
  - Spring은 Bean 인스턴스를 생성하고, 인스턴스 필드에만 @Autowired를 통해 의존성 주입
  - 하지만 static 필드는 클래스 레벨이므로, Spring은 주입하지 않음
<br>

- 실행 시점
  - memberRepository.save()를 호출하면, memberRepository는 여전히 null인 상태이므로 → NPE 발생
<br>

### JVM 메모리 구조
| 메모리 영역                     | 설명                       | 관련 대상           |
| -------------------------- | ------------------------ | --------------- |
| Heap                       | 인스턴스 객체 저장 공간            | Spring Bean 객체들 |
| Method Area (or MetaSpace) | 클래스 정보, static 변수 저장 공간  | static 필드       |
| Stack                      | 메서드 호출 정보 저장             | 로컬 변수 등         |
| PC Register                | 현재 실행 중인 명령어 주소          | 쓰레드당 하나         |
| Native Method Stack        | C/C++ 등 네이티브 메서드 호출 시 사용 | JNI 등           |
<br>

![image](https://github.com/user-attachments/assets/5e159491-e841-41c9-b883-2fab2eb56836)
> 출처 : https://inpa.tistory.com/entry/JAVA-%E2%98%95-JVM-%EB%82%B4%EB%B6%80-%EA%B5%AC%EC%A1%B0-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%98%81%EC%97%AD-%EC%8B%AC%ED%99%94%ED%8E%B8
<br>
<hr>
<br>

## ✔️ 왜 Spring은 static을 관리하지 않는가?
- Spring의 IoC 컨테이너는 객체의 생명주기(Bean Lifecycle) 를 관리

- 하지만 static은 클래스 레벨이라 객체와 무관하게 존재

- 그래서 @Autowired가 달려 있어도 Spring은 무시
<br>
<hr>
<br>

## ✔️ 해결 방법
### setter 사용
```java
@Service
public class CommonService {
	public static MyBean MyBean;

    // MyBean의 setter 메서드를 사용
    @Autowired
    public void setMyBean(MyBean Object) {
        this.MyBean = Object;
    }
    
    public static void commonMethod() {
        myBean.doSomething(); 
    }
}
```
<br>

### 생성자 사용
```java
@Service
public class CommonService {
	public static MyBean MyBean;

    // MyBean의 생성자를 이용
    @Autowired
    private CommonService(MyBean Object) {
        this.MyBean = Object;
    }
    
    public static void commonMethod() {
        myBean.doSomething(); 
    }
}
```
<br>

### @Postconstruct 사용
```java
@Service
public class CommonService {
	public static MyBean MyBean;
    @Autowired
    private MyBean Object;
   
    @PostConstruct
    private void initialize() {
        this.MyBean = Object;
    }
    
    public static void commonMethod() {
        myBean.doSomething(); 
    }
}
```
<br>

**➡️ @Postconstruct란?**
- 스프링 빈 생명주기(Bean LifeCycle) 사이클은 아래와 같다.
  - 스프링 컨테이너 생성  ->  스프링 빈 생성  ->  의존 관계 주입  ->  초기화 콜백  ->  사용  ->  소멸 콜백  ->  스프링 종료
  - 초기화 콜백(init) : Bean이 생성되고, Bean의 의존성 주입이 완료된 뒤 호출된다.
  - 소멸 콜백(destroy) : 스프링이 종료되기 전, Bean이 소멸되기 직전에 호출된다.
<br>

- 스프링 빈으로 등록되는 객체를 사용하기 위해서는 객체가 생성된 후 의존성 주입까지(Dependency Injection) 끝나야 해당 객체를 사용할 수 있게 된다. 

- `@PostConstruct`는 빈이 생성되고 의존 관계 주입이 완료된 후 실행되는 초기화 콜백을 적용할 수 있는 어노테이션이다.

- 즉, 빈의 모든 의존성이 주입된 후에 추가적인 초기화 작업을 수행할 수 있다.

- 또한 `@PostConstruct`는 bean lifecycle에서 오직 한 번만 수행된다는 것을 보장할 수 있다. 

- 그래서 `@PostConstruct`를 사용하면 bean이 여러 번 초기화되는 것을 방지할 수 있다. 
<br>
<hr>
<br>

## ✔️ 요약
- static 필드는 JVM의 Method Area에 로드됨.

- `@Autowired`는 Spring의 IoC 컨테이너가 객체를 만들고 주입하는 과정에서 동작.

- JVM이 관리하는 static 필드는 Spring 컨테이너가 주입 대상에서 제외함.

- 그 결과, static 필드는 주입되지 않아 null, 따라서 NPE 발생.
<br>
<hr>
<br>

**Reference**<br>

[Inpa Dev: JVM 내부 구조 & 메모리 영역 💯 총정리](https://inpa.tistory.com/entry/JAVA-%E2%98%95-JVM-%EB%82%B4%EB%B6%80-%EA%B5%AC%EC%A1%B0-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%98%81%EC%97%AD-%EC%8B%AC%ED%99%94%ED%8E%B8)<br>
[MadPlay!: static 변수에 autowired 설정하려면 어떻게 해야 할까?](https://madplay.github.io/post/spring-framework-static-field-injection)<br>
[곱마2: static 변수에 autowired 설정하는 방법](https://minsu092274.tistory.com/73)
