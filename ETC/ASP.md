## ASP란 무엇인가?
- Active Server Page, 동적으로 서버에서 작동하는 페이지를 의미 (= Server Side Script) 

- Client Side Script인 자바스크립트와 반대되는 개념 (서버 측에서 실행)

- IIS에 설치하여 사용이 가능 (= IISRoot 폴더가 존재하는 이유)
  - [IIS : Internet Information Service](https://github.com/yejun95/Today-I-Learn/blob/master/ETC/IIS%EB%9E%80.md)
<br>
<hr>
<br>

### ✔ 사용 이유
- 사용자와의 동적인 상호작용 (HTML 은 정적)

- 서버 측 자원을 사용해야 하는 경우 (DB)

- 스크립트의 안정적 실행 (서버 측에서 HTML로 먼저 번역 진행 후 브라우저에 보여줌)

- 스크립트 소스를 감추기 위함
<br>

**사용 코드 : `<% %>`**
  ```java
  <HTML>
  <HEAD>
  <TITLE> Welcome To Dukyoung.net </TITLE>
  </HEAD>
  <BODY>
  <CENTER> 
  <% IF Hour(Now) < 12 THEN %>
  지금은 오전입니다.
  <% ELSE %>
  지금은 오후입니다.
  <% END IF%>
  </CENTER>
  </BODY>
  </HTML>
  ```
<br>
<hr>
<br>

**Reference**<br>

[재미있는 코딩나라 : ASP란 무엇인가](https://coding-fun.tistory.com/36)
