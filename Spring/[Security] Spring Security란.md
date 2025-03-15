# Spring Security란 무엇인가?
- 인증, 권한 관리 그리고 데이터 보호 기능을 포함하여 웹 개발 과정에서 필수적인 사용자 관리 기능을 구현하는데
도움을 주는 Spring의 강력한 프레임워크이다.

- 일반적으로 개발 시 가장 먼저 작업하는 부분이 회원가입, 로그인, 로그아웃, 세션, 권한 등인데, 이러한 기능의 개발은
개발자에게 많은 시간을 요구한다.

- 이 때, Spring Security는 개발자들이 보안 관련 기능을 효육적이고 신속하게 구현할 수 있도록 도와준다.

- 또한 Filter 방식으로 동작하기 때문에 스프링 MVC와 분리되어 관리 및 동작한다.
  - Filter는 DispatcherServlet 전에 적용
<br>
<hr>
<br>

## ✔️ 사용 이유
- Spring의 생태계에서 보안에 필요한 기능을 제공

- Spring Security를 사용하지 않고 코드를 직접 작성할 경우 Spring에서 추구하는 IoC/DI 패턴과 같은 확장 패턴을
염두해서 인증/인가 부분을 직접 개발하기가 쉽지 않은데, Spring Security에서는 이와 같은 기능을 제공하고 있어
개발 작업 효율을 높일 수 있다.
<br>
<hr>
<br>

## ✔️ Spring Security 아키텍처

![image](https://github.com/user-attachments/assets/164306ef-0a28-4c8e-a01f-e350b223614a)
> 붉은색 박스 부분이 Spring 프레임워크에서 Spring Security가 적용되는 부분
<br>

### 적용 순서
1. 사용자의 요청이 서버로 들어온다.
2. Authentication Filter가 요청을 가로채고 Authentication Manager로 요청을 위임한다.
3. Authentication Manager는 등록된 Authentication Provider를 조회하며 인증을 요구한다.
4. Authentication Provider가 실제 데이터를 조회하여 UserDetails 결과를 돌려준다.
5. 결과는 SecurityContextHolder에 저장이 되어 저장된 유저정보를 Spring Controller에서 사용할 수 있게 된다.
<br>

### Spring Security가 작동하는 내부 구조
![image](https://github.com/user-attachments/assets/e9821ddb-03b1-486e-ada4-8b549ae69693)
<br>

1. 사용자가 자격 증명 정보를 제출하면, AbstractAuthenticationProcessingFilter가 Authentication 객체를 생성한다.
2. Authentication 객체가 AuthenticationManager에게 전달한다.
3. 인증에 실패하면, 로그인 된 유저정보가 저장된 SecurityContextHolder의 값이 지워지고 `RememberMeService.joinFail()`이 실행된다.
그리고 AuthenticationFailureHandler가 실행된다.
4. 인증에 성공하면, SessionAuthenticationStrategy가 새로운 로그인이 되었음을 알리고, Authentication 이 SecurityContextHolder에 저장된다.
5. 이후에 SecurityContextPersistenceFilter가 SecurityContext를 HttpSession에 저장하면서 로그인 세션 정보가 저장된다.
6. 그 뒤로 `RememberMeServices.loginSuccess()`가 실행된다. 
7. ApplicationEventPublisher가 InteractiveAuthenticationSuccessEvent를 발생시키고 AuthenticationSuccessHandler 가 실행됩니다.
<br>
<hr>
<br>

**Reference**<br>

[ELANCER : Spring Security란? 사용하는 이유부터 설정 방법까지 알려드립니다!](https://www.elancer.co.kr/blog/detail/235)<br>
[HelloJudy : Spring Security 개념과 처리 과정](https://hello-judy-world.tistory.com/216)<br>
