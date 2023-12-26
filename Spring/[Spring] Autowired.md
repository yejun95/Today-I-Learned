## Autowired란 무엇인가?
- 의존관계 주입(DI)을 할 때 사용하는 어노테이션(Annotation)이다.
  - 주로 service나 Repository에 대한 의존성을 주입하기 위해 사용
  - 객체 의존관계를 외부에서 넣어주는 것을 '의존성 주입(DI)'라 한다. 

- 의존 객체의 타입에 해당하는 빈(Bean)을 찾아 주입하는 역할을 한다.
  - 빈 : 스프링에서는 스프링이 제어권(IoC)을 가져서 직접 생성하고 의존관계를 부여하는 오브젝트를 빈이라고 부른다.
<br>
<hr>
<br>

### ✔사용 가능 위치
- @Autowired는 기본적으로 아래의 위치에서 사용할 수 있다.

- 생성자 (스프링 4.3부터는 생략 가능)

- Setter

- 필드
<br>
<hr>
<br>

### ✔ 사용 주의점
- 스프링이 직접 생성하고 의존관계를 부여하는 오브젝트를 Bean이라고 하였는데,<br>
Bean으로 미리 등록되어 있지 않다면은 스프링은 이를 직접 생성할 수 없다.

**생성자에 @Autowired 명시**
```java
@Service
public class TestService {

    TestRepository testRepository;
    
    @Autowired
    public TestService(TestRepository testRepository) {
        this.testRepository = testRepository;
    }
}
```
<br>

```
public class TestRepository {
	....
}
```

- `TestRepository` 클래스를 가져와서 의존성 주입을 시도한다.

- 그러나 해당 클래스는 Bean이 등록되어 있지 않기 때문에 실행 에러가 발생한다.

- 💡 `@Component` 애노테이션이 있으면 스프링 빈으로 자동 등록된다.
  - @Component를 포함하는 @Controller, @Service, @Repository 애노테이션도 스프링 빈으로 자동 등록된다.
  - 때문에 해당 클래스에 @Repository 애노테이션이 있었다면 Bean으로 등록됐을 것이다.
  - 아래 코드 처럼...
<br>

```java
@Repository
public class TestRepository {
	....
}
```
<br>
<br>

**Setter에 @Autowired 명시**
```java
@Service
public class TestService {

    TestRepository testRepository;
	
    @Autowired
    //@Autowired(required = false)
    public void setTestRepository(TestRepository testRepository) {
        this.testRepository = testRepository;
    }
}
```
<br>

```java
public class TestRepository {
	....
}
```

- 자체 인스턴스를 생성했음에도 작동하지 않는다.

- @Autowired가 명시되어 있기 때문에 스프링은 의존성을 주입하려고 시도한다.
  - setter를 이용해 인스턴스를 생성할 수 있지만, 의존성 주입에는 실패하는 것이다.  ->  실행 에러
<br>

- @Autowired(required = false) 옵션을 주면 의존성 주입을 받지 않고 인스턴스를 만들어서 bean으로 등록할 수 있다. 
  - 즉, TestRepository는 의존성 주입이 되지 않은 상태로 bean이 등록된다.
<br>
<br>

**필드에 @Autowired 명시**
```java
@Service
public class TestService {
	
    @Autowired
    //@Autowired(required = false)
    TestRepository testRepository;
}
```
<br>

```java
public class TestRepository {
	....
}
```
<br>

- `TestRepository`에 대한 Bean이 등록되어 있지 않다.

- setter나 필드 Injection을 사용할 때는 Optional 설정(required = false)을 통해<br>
TestService가 해당하는 의존성 없이도 bean으로 등록할 수 있다.
<br>
<hr>
<br>

**Reference**<br>

[짱호 : 스프링 핵심기술 - @Autowired](https://jjingho.tistory.com/6)
