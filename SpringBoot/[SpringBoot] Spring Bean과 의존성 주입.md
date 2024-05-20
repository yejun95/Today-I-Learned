![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/a5bc24e5-aaf4-48cc-b420-e8d70494cc19)## Sprign Bean이란 무엇인가?
- 스프링 컨테이너가 관리하는 자바 객체

- 빈은 인스턴스화된 객체를 의미하며, 스프링 컨테이너에 등록된 객체를 스프링 빈이라고 한다. 

- 쉽게 이해하자면 new 키워드 대신 사용한다고 보면 된다.
<br>
<hr>
<br>

### ✔ Spring Bean이 있는 이유
- 우리가 Spring을 사용할 때, annotation을 거의 대부분 붙여서 사용한다.
  - @Controller
  - @Service
  - @Repository
  - 등등...
<br>

- 이러한 annotation들이 붙은 객체를 Spring Bean에 등록하고, Spring 컨테이너에서 사용하게 만드는 것이다.

- 즉, Spring project 안에서 자바 객체들을 자유롭게 사용하기 위함

- 이렇게 Bean에 자바 객체가 등록되어 있지 않다면 Spring은 해당 객체들을 사용할 수 없다.
<br>
<hr>
<br>

### ✔ Bean에 등록되는 범위
- 동일한 패키지

- 하위 패키지에만 Spring Bean에 등록된다.

- 만약 아래와 같은 패키지 구조가 있다고 가정하자.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/7af488f9-e9b6-4dbf-80d5-429e9fd8d326)
<br>

- hello.hellospring 패키지에는 HelloSpringApplication이 있기 때문에 해당 패키지는 Spring Bean에 등록된다.

- 그러나 test 패키지에 경우 SpringApplication 객체가 없기 때문에 Spring Bean에 등록될 수 없다.

- 즉, Spring을 실행 할 수 있는 Application 객체가 없는 패키지는 Spring Bean에 등록될 수 없는 것이다.
<br>
<hr>
<br>

### ✔ Bean을 활용한 의존성 주입
- 의존성 주입(Dependency Injection), 통칭 DI란 말을 들어본 적이 있을 것이다.

- 이는 스프링 컨테이너가 뜰 때, `@Autowired`라는 annotation을 활용하여 Bean에 등록된 객체를 다른 객체에 넣어줄 때 쓰는 말이다.

- 말이 어려우니 코드로 확인해보자

```java
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```
<br>

- 위 코드를 보면 `@Controller`와 `@Autowird`라는 annotation을 사용하고 있다.

- `@Controller`를 사용함으로써 스프링 컨테이너가 뜰 때, Bean에 등록됐을 것이다.

- 그리고 Controller에서는 MemberService에서 필요한 코드를 가져와 사용해야한다.

- 즉, Controller가 MemberService를 의존해야 되기 때문에 `@Autowired`를 사용하여 의존성을 주입받는 것이다.
  - 의존성 주입은 필드, 생성자, setter 3가지 방법이 있으며, 현 코드에서는 생성자를 통한 의존성 주입을 하고있다.
<br>

- DI를 설명하면 길어지지만, 현재 생성자를 통한 의존성 주입이 가능한 이유는 스프링 컨테이너가 뜰 때, 각 객체가 Bean에 등록되고<br>
생성자를 가진 객체는 해당 생성자가 실행이 된다.
  - 즉, 생성자 매개변수로 MemberService가 있기 때문에 해당 객체에서 의존성 주입을 받을 수 있는 것이다.
<br>
<br>

- `@Controller`를 제외한 나머지 `@Service`, `@Repository` 등, 전부 annotation이 붙여있고 스프링을 실행하는 순간 Bean에 등록된다.

- 💡 만약 MemberService 객체에 아래와 같이 `@Service`가 붙지 않는다면 스프링 실행 시 에러가 난다.
  - Controller가 MemberService에서 의존성을 주입받아야 하는데, Bean에 등록되있지 않기 때문

```java
public class MemberService {

    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```
> 해당 상태로 스프링 실행

```
2024-05-20T23:43:37.630+09:00  INFO 17624 --- [hello-spring] [  restartedMain] h.hellospring.HelloSpringApplication     : Starting HelloSpringApplication using Java 17.0.6 with PID 17624 (C:\Users\yogo\Desktop\yejun\coding\intelliJ_Project\hello-spring\out\production\classes started by yogo in C:\Users\yogo\Desktop\yejun\coding\intelliJ_Project\hello-spring)
2024-05-20T23:43:37.633+09:00  INFO 17624 --- [hello-spring] [  restartedMain] h.hellospring.HelloSpringApplication     : No active profile set, falling back to 1 default profile: "default"
2024-05-20T23:43:37.674+09:00  INFO 17624 --- [hello-spring] [  restartedMain] .e.DevToolsPropertyDefaultsPostProcessor : Devtools property defaults active! Set 'spring.devtools.add-properties' to 'false' to disable
2024-05-20T23:43:37.674+09:00  INFO 17624 --- [hello-spring] [  restartedMain] .e.DevToolsPropertyDefaultsPostProcessor : For additional web related logging consider setting the 'logging.level.web' property to 'DEBUG'
2024-05-20T23:43:38.552+09:00  INFO 17624 --- [hello-spring] [  restartedMain] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port 8080 (http)
2024-05-20T23:43:38.564+09:00  INFO 17624 --- [hello-spring] [  restartedMain] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2024-05-20T23:43:38.564+09:00  INFO 17624 --- [hello-spring] [  restartedMain] o.apache.catalina.core.StandardEngine    : Starting Servlet engine: [Apache Tomcat/10.1.20]
2024-05-20T23:43:38.613+09:00  INFO 17624 --- [hello-spring] [  restartedMain] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2024-05-20T23:43:38.613+09:00  INFO 17624 --- [hello-spring] [  restartedMain] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 939 ms
2024-05-20T23:43:38.673+09:00  WARN 17624 --- [hello-spring] [  restartedMain] ConfigServletWebServerApplicationContext : Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'memberController' defined in file [C:\Users\yogo\Desktop\yejun\coding\intelliJ_Project\hello-spring\out\production\classes\hello\hellospring\controller\MemberController.class]: Unsatisfied dependency expressed through constructor parameter 0: No qualifying bean of type 'hello.hellospring.service.MemberService' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {}
2024-05-20T23:43:38.676+09:00  INFO 17624 --- [hello-spring] [  restartedMain] o.apache.catalina.core.StandardService   : Stopping service [Tomcat]
2024-05-20T23:43:38.688+09:00  INFO 17624 --- [hello-spring] [  restartedMain] .s.b.a.l.ConditionEvaluationReportLogger : 

Error starting ApplicationContext. To display the condition evaluation report re-run your application with 'debug' enabled.
2024-05-20T23:43:38.713+09:00 ERROR 17624 --- [hello-spring] [  restartedMain] o.s.b.d.LoggingFailureAnalysisReporter   : 

***************************
APPLICATION FAILED TO START
***************************

Description:

Parameter 0 of constructor in hello.hellospring.controller.MemberController required a bean of type 'hello.hellospring.service.MemberService' that could not be found.


Action:

Consider defining a bean of type 'hello.hellospring.service.MemberService' in your configuration.


Process finished with exit code 0\
```
> 'hello.hellospring.service.MemberService' that could not be found. => MemberService를 찾을 수 없다고 나온다.<br>
앞서 말했듯이 Controller가 MemberService에게 의존성을 주입받아야 하는데, annotation이 빠져 Bean에 등록되지 않았기 때문
<br>
<hr>
<br>

### ✔ ComponentScan
- 사실 모든 annotation들은 `@Component`라는 것을 붙여서 사용할 수 있다.
<br>

**예시**

```java
@Component
public class MemberService {

    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```
> @Service 대신 @Component 사용
<br>

- 이것이 가능한 이유는 `@Controller`, `@Service`, `@Repository` 같은 annotation을 타고 들어가보면 `@Component`라는 annotation이 달려있다.

- 아래는 `@Service`를 들어갔을 때 코드

```java
// 주석 및 import 생략

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Service {

	@AliasFor(annotation = Component.class)
	String value() default "";
}
```
> @Component annotation이 달려있음
<br>

- 이 때문에 `@Component`를 붙여놔도 되지만, 각 객체의 활용도를 알기 위해서는 해당하는 annotation을 붙여주는 것이 좋다.

- 또한 해당 annotation을 읽을 수 있는 이유는 SpringApplication에 Component를 스캔할 수 있는 객체가 내장되어 있기 때문이다.


```java
@SpringBootApplication
public class HelloSpringApplication {

	public static void main(String[] args) {
		SpringApplication.run(HelloSpringApplication.class, args);
	}

}
```
<br>

- 위는 SpringApplication 객체인데, 최상단에 있는 annotation을 타고 들어가보자.

```java
// 주석 및 import 생략
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {

}
```
> ComponentScan 이라는 annotation 덕분에 @Component 라고 붙여도 스프링이 알 수 있는 것
<br>
<hr>
<br>

### ✔ 자바 코드로 직접 Bean을 등록하는 방법
- 지금까지 썼던 annotation들은 자바 객체에 직접 적용하였다.

- 하지만 `@Configuration`이라는 annotation을 사용한다면 별도의 파일로 Bean 관리가 가능하다.
<br>

**예시**

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/64b7595f-140e-4bbc-848d-4c90a9a0f085)
<br>

- SpringConfig 라는 파일을 만들고, 해당 클래스 안에서 필요한 객체 자신을 직접 리턴한다.

- 이때 SpringConfig 객체에는 `@Configuration`을 Bean에 등록시켜야할 객체는 `@Bean`라는 annotation을 사용한다.
<br>
<hr>
<br>

**Reference**<br>

[인프런 김영한 : 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
