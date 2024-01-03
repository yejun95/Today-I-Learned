## Servlet과 DispatcherServlet의 차이
- 기존 Servlet은 각 경로마다 전부 맵핑을 해줘야하는 번거로움이 있었다.

- 때문에 클라이언트 요청에 의한 중복 코드가 지속적으로 발생
  - 서버로 들어오는 Request는 url에 맞는 서블릿과 매핑된다. 
  - Request와 서블릿이 일대일로 매핑되니 서블릿마다 View를 생성하는 로직이 필요해진다. 
  - 이런 이유로 중복코드가 발생한다.  -> 모든 서블릿을 URL매핑을 위해 web.xml에 모두 등록
  - 이런 문제를 해결하는 디자인 패턴이 '프론트 컨트롤 패턴'이다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/05711140-bbd1-4571-a492-cb7c1da78e34)
<br>
<br>

- 상단 그림과 같은 문제로 인하여 프론트 컨트롤 패턴을 적용하는 DispatcherServlet이 등장하게 된다

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/0e6d9625-c179-4315-897d-7d1309beada7)
<br>
<br>

- 서버로 들어오는 Request는 모두 하나의 서블릿을 거친다. 그것이 프론트 서블릿이다. 

- 프론트 서블릿은 요청이 들어온 url과 매핑되는 Controller에게 요청데이터를 넘긴다. 

- Controller는 비즈니스 로직을 처리하고 그 결과를 프론트 서블릿에게 전달한다. 

- 결과를 전달받은 프론트 서블릿은 View에게 데이터를 넘겨 화면을 동적으로 생성한다. 

- 프론트 컨트롤러와 컨트롤러 사이에는 핸들러 맵핑, 핸들러 어댑터 등 중간 과정이 별개로 존재한다.
<br>
<hr>
<br>

### ✔ 정리
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/9378415a-3be9-459e-a956-2fb75519aaa8)

- 클라이언트 요청에 의해 HttpServlet 객체가 생성되며, 쓰레드에 할당되어 `service()` 메서드가 실행된다.

- 이후 `service()` 메서드에 의한 모든 요청은 DispatcherServlet 으로 전해진다.

- 요청에 맞는 Controller와 맵핑한다.

- 내부 로직
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/99709dfc-bd61-4da6-93fe-170eeee233dd)
<br>
<hr>
<br>

**Reference**<br>

[L.O.K : [[SpringMVC] MVC 패턴 구현하기(2) - Controller]](https://lordofkangs.tistory.com/482)<br>
[키크니개발자 : [Spring] Spring Web Mvc의 Dispatcher Servlet(디스패처 서블릿)의 알아보기](https://tall-developer.tistory.com/17)<br>
[로키의 개발 블로그 : [Spring] DispatcherServlet이란?](https://yejun-the-developer.tistory.com/4)<br>
[응애. 나 애기 개발자. : 꾸준히 듣게되는 Servlet과 Dispatcher Servlet에 대해 알아보자](https://7357.tistory.com/180)
