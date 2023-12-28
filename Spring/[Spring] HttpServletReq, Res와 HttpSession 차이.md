## HttpServletReq, Res와 HttpSession 차이
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/01f8a774-66fc-482f-bc70-571b5b39c9e6)
<br>

### ✔ HttpServlet
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/46ab1110-b960-46aa-8893-351da453472e)
<br>

- 웹브라우저가 WAS에게 Servlet request -> WAS는 HttpServletRequest 객체를 생성하여 저장

- 응답을 보낼 때 사용하기 위해 HttpServletResponse 객체 생성
  - Servlet에게 두 객체 전달
  - doGet, doPost, Service 등과 같은 메서드에 파라미터로 전달되어 사용됨
<br>
<hr>
<br>

### ✔ HttpServletRequest
- http프로토콜의 request정보를 서블릿에게 전달하기 위해 사용

- 헤더정보, 파라미터, 쿠키, URI, URL 등의 정보를 읽어 들이는 메소드 포함
  - `HttpServletRequest의 변수명.getSession`으로 세션 정보를 확인할 수 있는 이유
<br>

- Body의 Stream을 읽어 들이는 메소드 포함
<br>
<hr>
<br>

### ✔ HttpServletResponse
- 요청을 보낸 클라이언트에게 응답을 보내기 위해 WAS에서 생성되어 서블릿에게 전달됨

- 서블릿은 이 객체를 이용하여 content type, 응답코드, 응답 메시지등을 전송
<br>
<hr>
<br>

### ✔ HttpSession
- 클라이언트와 서버 간의 상태를 유지하기 위한 세션 관리를 제공하는 인터페이스

- 아래의 코드로 세션 정보 확인 및 세션 부여가 가능하다.
```java
HttpSession session = request.getSession();
session.setAttribute("key", "value");
```
<br>
<br>

**로그인, 로그아웃 참고 소스**
```java
@RequestMapping(value="/login",method=RequestMethod.POST)
	public String login(MemberVO vo, HttpServletRequest req, RedirectAttributes rttr) throws Exception{
		logger.info("post login");
		
		HttpSession session = req.getSession();  //값을 받고
		MemberVO login = service.login(vo);
		
		if(login == null) {
			session.setAttribute("memeber", null);
			rttr.addFlashAttribute("msg", false);
		}else {
			session.setAttribute("member", login); //set으로 세션생성
		}
		return "redirect:/";
	}
	
	@RequestMapping(value="logout", method=RequestMethod.GET)
	public String logout(HttpSession session) throws Exception{
		session.invalidate();
		return "redirect:/";
	}
```
<br>
<hr>
<br>

### ✔ 정리
- 두 클래스는 역할 자체가 다르며, 비교 대상이 아니지만 같이 쓰인다는 점에서 공부를 해야한다.

- HttpServlet을 통해 요청, 응답을 하고, 알맞는 대상에게 HttpSession을 통해 세션을 부여하는 것이다.

- 함께 쓰이지만 완전히 다른 클래스라고 기억하자.
<br>
<br>

**주요 세션 메서드**
|메소드 이름|리턴 타입|설명|
|---|---|---|
|getAttribute(String name)| java.lang.Object|세션 속성명이 name인 속성의 값을 Object 타입으로 리턴한다. 해당 되는 속성명이 없을 경우에는 null 값을 리턴한다.|
|getAttributeNames()|java.util.Enumeration |세션 속성의 이름들을 Enumeration 객체 타입으로 리턴한다.|
|getCreationTime()|long |1970년 1월 1일 0시 0초를 기준으로 하여 현재 세션이 생성된 시간까지 경과한 시간을 계산하여 1/1000초 값으로 리턴한다.|
|getId() |java.lang.String|세션에 할당된 고유 식별자를 String 타입으로 리턴한다. |
|getMaxInactiveInterval()|int|현재 생성된 세션을 유지하기 위해 설정된 세션 유지시간을 int형으로 리턴한다.|
|invalidate()|void|현재 생성된 세션을 무효화 시킨다.|
|removeAttribute(String.name)|void|세션 속성명이 name인 속성을 제거한다.|
|setAttribute(String name, Object value)|void|세션 속성명이 name인 속성에 속성값으로 value를 할당한다. |
|setMaxInactiveInterval(int interval)|void|세션을 유지하기 위한 세션 유지시간을 초 단위로 설정한다.|
<br>
<hr>
<br>

**Reference**<br>

[oliviarla : HttpServletRequest, HttpServletResponse 객체란 ](https://velog.io/@oliviarla/HttpServletRequest-HttpServletResponse-%EA%B0%9D%EC%B2%B4%EB%9E%80)<br>
[간펴니 : HttpserveltRequest와 HttpSession,세션과 로그인 로그아웃](https://kimfk567.tistory.com/19)
