## TestApplication.java와 TestApplicationTests.java의 차이
### ✔ TestApplication.java
- `@SpringBootApplication` 어노테이션이 적용된 클래스로, 애플리케이션을 실행할 때 애플리케이션의 시작점이 된다.

- `@SpringBootApplication` 어노테이션으로 인해 스프링 부트의 자동 설정, 스프링 Bean 읽기와 생성이 모두 자동으로 설정된다.

- `@SpringBootAplication` 어노테이션이 있는 위치부터 설정을 읽어가기 때문에 이 어노테이션을 포함한 클래스는 항상 프로젝트의 최상단에 위치해야만 한다.
  - 자바 main 메서드가 포함되어 있기 때문에 어플리케이션이 시작되면 가장 먼저 실행된다.

- 아래와 같이 코드를 보면 TestApplication.java는 Springboot와 관련된 속성을 가지고 있는 어노테이션이 적용되어 있다.

```java
@SpringBootApplication
public class TestApplication {

	public static void main(String[] args) {
		SpringApplication.run(TestApplication.class, args);
	}

}
```
<br>

**- 즉, 스프링부트를 실행시키기 위한 java 파일인 것이다.**

- 내부 어노테이션

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/fa11b5c1-b751-4df0-822c-9de34d0227a6)
<br>
<hr>
<br>

### ✔ TestApplicationTests.java
- 이 파일은 주로 테스트 코드를 작성하는 데 사용되는 파일이다. 

- java의 대표적인 테스트 프레임워크인 JUnit을 사용할 수 있다.

- `@SpringBootTest` 어노테이션을 사용하여 테스트 환경을 제공한다.

- 내부 어노테이션

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/94c42fb7-cab3-4607-ab9f-a5ef86501808)
<br>
<hr>
<br>

- 💡 이렇게 두 파일은 각각 애플리케이션의 실행과 테스트에 관련된 역할을 담당한다.
