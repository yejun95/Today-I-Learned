## 서블릿과 서블릿 컨테이너
### ✔ Servlet (서블릿)
- 동적 웹을 구현하기 위해 만들어진 것으로, 클라이언트의 요청을 처리할 수 있는 클래스이다. 

- 자바로 HTTP 요청을 처리하는 프로그램을 만들 때 사용한다.

- 독자적으로 실행되지는 못하고 톰캣과 같은 Servlet 컨테이너에서 실행된다.

- HTTP 프로토콜 서비스를 지원하는 javax.servlet.http.HttpServlet 클래스를 상속받는다.

- 자바를 사용하여 웹을 만들 때, 무조건 사용되기 때문에 Spring에서 당연하게 사용한다.

- 클라이언트 요청마다 쓰레드가 생성되며, 더 이상 처리할게 없으면 쓰레드는 삭제된다.
  - 멀티쓰레드 기능
<br>
<hr>
<br>

### ✔ Servlet Container (서블릿 컨테이너)
- 서블릿을 실행해주는 역할이며, 말 그대로 서블릿을 담는 그릇이다.

- 익히 알고 있는 Tomcat이 이에 해당된다.

- WAS == 서블릿 컨테이너 라고 볼 수 있지만, WAS는 웹서버까지 포함하기 때문에<br>
WAS안에 서블릿 컨테이너가 존재한다고 보는게 바람직하다.

- 서블릿의 생명주기를 관리한다.
  - init() : 서버가 실행되면 처음에 한번만 실행된다.
  - service() : 클라이언트의 요청을 처리할 때 실행된다. -> 비즈니스 로직 처리
  - destroy() : 요청에 대한 응답 처리가 끝나면 실행된다. -> 할당된 쓰레드 삭제
<br>
<hr>
<br>

### ✔ 클라이언트 요청 처리 과정
- 클라이언트가 HTTP 요청을 보낸다.
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/2540b156-fce0-4d0a-9dd0-275e0a8bc886)
<br>
<br>

- 서블릿 컨테이너는 해당 요청 처리를 위해 `HttpServletRequest`, `HttpServletResponse` 객체를 생성한다.
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/fe61745d-86d3-42e9-b37f-614aabe98953)
<br>
<br>

- 클라이언트가 보낸 요청이 어떤 API에 대한 요청인지 확인한다.
  - 이후 서블릿 쓰레드를 생성하여 req, res 객체를 보낸다.
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/155ca49f-8a4c-4e83-b4f7-d25020b84dc6)
<br>
<br>

- 컨테이너는 API에 맵핑된 비즈니스 로직 처리를 위해서 서블릿의 `service()` 메서드를 호출한다.
  - 요청 메서드 방식에 따라 `doGet`, `doPost` 라고 불린다.
  - 현재는 get이 들어왔다고 가정한다.
<br>
<br>

- `doGet` 메서드는 동적 페이지 생성 후 Res 객체에 실어 보낸다.
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/32521f02-b1a2-4ca9-a9ab-68ba9589ffd6)
<br>
<br>

- 스레드 작업이 끝나면 컨테이너는 Res 객체를 HTTP Response 객체로 변환하여 클라이언트로 보낸다. (HTTP 응답)
  - 이후 Req, Res 객체를 소멸시킴
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/d408ecde-ead2-40b2-b13e-78ac48c90e15)
<br>
<hr>
<br>

### ✔ Web Service Architecture
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/53556dec-df90-4fe9-829c-98e39f75371d)
<br>
<hr>
<br>

**Reference**<br>

[응애. 나 애기 개발자 : 이름이 귀여운 WAS와 Web Server에 대해 알아보자](https://7357.tistory.com/179)<br>
[finerss's world! : 서블릿 컨테이너(Servlet Container](https://finerss.tistory.com/11)<br>
[박재성 : Servlet Container와 Servlet의 관계](https://www.youtube.com/watch?v=aP4Lw3SfffQ)
