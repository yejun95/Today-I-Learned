## 소스 작성을 위한 프로젝트 구조
### ✔ 패키지구조 : 계층형 vs 도메인
**계층형 패키지 구조**
- 계층형으로 패키지를 설계하는 방식

- 전체적인 구조를 빠르게 파악할 수 있지만 디렉토리에 클래스들이 너무 많이 모이는 단점이 있다.
<br>
<br>

**도메인 패키지 구조**
- 도메인 단위로 디렉토리를 구성하는 방식

- 관련된 코드들이 응집해있는 장점이 있지만, 프로젝트에 대한 이해도가 낮을 경우 전체 구조를 파악하기 어려움
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/ff188d2e-296f-4053-9adf-19cccda388fb)

- 어떠한 패키지가 더 낫냐라는 정답은 없다.

- 프로젝트 성격에 맞게 알맞는 패키지 구조를 선택하는 것이 좋다.

- 클래스들이 많아지면 많아질 수록 도메인 구조로 설계하는 것이 훨씬 좋다.

-  또한 최근 마이크로서비스의 영향으로 패키지를 도메인으로 구분하는 경우가 더 많아짐.
<br>
<hr>
<br>

### ✔ 대표적인  패키지 구조
- 크게 5가지로 나눌 수 있다.
  - controller
  - repository
  - dto
  - domain (entity)
  - service
    - serviceImpl
<br>
<br>

**controller**
- HTTP 요청과 응답을 위한 클래스

- @Controller 어노테이션을 붙여주면 스프링 빈에 등록되고 스프링에서 관리하는 객체가 된다.

- @GetMapping(“주소”) 와 같이 http 메서드 명과 함께 주소를 작성해주게 되면 해당 주소로 요청을 받을 수 있게 된다.

- Service를 사용하여 db에 접근하게 된다.
<br>
<br>

**repository**
- repository는 db에 접근하는 코드를 모아둔 Interface이다.

- Mapper를 포함하고 있는 개념이며, DAO이다.
  - DAO(Data Access Object): DB에 접근해서 데이터를 조회 및 조작하는 객체
<br>

- repository(dao)는 비즈니스로직에서 db의 데이터를 조회 및 조작하는 것을 비즈니스 로직과 분리하기 위한 것으로
db랑 연결하는 것이 강한 mapper와 달리 db의 데이터를 조회 및 조작하는 것에 중점을 뒀다는 것이 제일 큰 개념이다.
<br>
<br>

**dto**
- DTO : Data Transfer Object로 말 그대로 데이터 전송 객체이다.

- 특수한 로직 없이 getter, setter만 가지고 있다.

- Service나 Controller에서 DB에 접근할 때 사용하는 클래스다.

- 위에서 설명한 Domain 클래스와 다른 게 무엇인지 의문을 품을 수 있다.
  - domain은 DB 테이블에 대한 정보를 가지고 있는 클래스
  - dto는 해당 테이블에서 실제로 CRUD를 할 필드를 정의해둔 것이라고 보면 된다.
  - domain과 dto를 나누어서 사용하는 이유는 코드 작성 중에 db에 접근할 필드의 변경이 생겼을 때,<br>
  domain을 변경하여 db에 접근하는 경우, db 테이블 설정을 직접 접근하는 상황이 발생하여 문제를 일으킬 수 있다.
<br>
<br>

**demain (entity)**
- 실제 db의 테이블과 직접적으로 mapping 되는 클래스

- setter가 없으며, getter로만 정보를 얻어야 한다.

- 가장 core한 클래스이기 때문에 변경이 되어서는 안된다.
<br>
<br>

**service**
- Repository와 DTO를 통해 db에 접근하여 CRUD의 각각의 프로세스 관리와 에러 처리 등을 담당하는 역할을 한다.

- @Service 어노테이션을 붙여주게 되면 스프링 빈에 등록되고 스프링에서 관리하는 객체가 된다.

- db와 실제적인 접근을 명령하는 소스코드를 작성하는 클래스이다.

- DTO에 작성된 메서드를 기반으로 소스코드를 작성하게 됩니다.
<br>
<hr>
<br>

### ✔ service, serviceImpl
- service를 인터페이스와 구현체를 분리하는 것은 OCP에 입각한다.

- OCP : Open Closed Principle 개방 폐쇄 원칙
  - 확장에 대해서는 개방적(open)이고, 수정에 대해서는 폐쇄적(closed)이어야 한다는 의미
  - 인터페이스와 구현체를 분리하면, 특정 기술을 추가하거나 변경하더라도, 클라이언트 코드(호출 영역)는<br>
수정하지 않아도 된다.
  - 실제 Spring MVC에서 구현 시 호출 영역(controller)에서 코드 변경없이 기능을 추가하거나 변경할 수 있다.
<br>
<br>

**예제 코드**
- interface
```java
// Interface

public interface IService {

    String test();

}
```
<br>

- 구현체
```java
@Service
public class AServiceImpl implements IService {

    @Override
    public String test() {
        return "AServiceImpl";
    }

}
```
<br>

- controller
```java
// Controller 

@RequiredArgsConstructor // 생성자 주입
@RestController
public class SampleController {

    private final IService service;

    @GetMapping("/test")
    public String test(){
        return service.test();
    }

}
```
<br>

- 위 예시 코드를 보면 컨트롤러는 특정 구현체가 아닌 IService 타입을 의존 주입받고 있다.
  - (물론 특정 구현체인 AServiceImpl 을 의존주입 받아도 노상관)

- 이렇게 구현체가 아닌 인터페이스를 의존받으면 OCP 원칙을 지킬 수 있다.
  - 구체적인 이해를 위해 임의로 BServiceImpl 구현체를 추가해보겠다
<br>
<br>

- 또 다른 구현체
```java
public class BServiceImpl implements IService {

    @Override
    public String test() {
        return "BServiceImpl";
    }

}
```
<br>

- AServiceImpl 와 마찬가지로 IService에 선언된 test() 메서드를 구현하고 있다.

-  이제 이 클래스에 @Service 어노테이션을 붙이면 스프링 컨테이너에 의해 Controller 로 의존주입이 된다.
  - ( AServiceImpl 의 @Service 어노테이션은 제거해주어야 한다.)
<br>
<hr>
<br>

**Reference**<br>

[m1naworld.log : [Spring Boot] 패키지 구조](https://velog.io/@m1naworld/Spring-Boot-%ED%8C%A8%ED%82%A4%EC%A7%80-%EA%B5%AC%EC%A1%B0)<br>
[우기의 코딩 노트 : [Spring Boot] 패키지 구조 및 정리](https://woogienote.tistory.com/87)<br>
[파미패럿 : Mapper와 Repository의 차이](https://pamyferret.tistory.com/69)<br>
[Hoo, I am : [Spring] Service, ServiceImpl 의 관계 (feat. OCP)](https://junior-datalist.tistory.com/243)<br>
