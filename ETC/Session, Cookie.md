## 세션과 쿠키
- 공통점 : 웹 통신간 유지하려는 정보를 저장하기 위해 사용하는 것
  - ex) 로그인 유지
<br>

- 차이점 : 저장되는 장소의 주체가 어디인가가 제일 큰 차이점이다.
  - 쿠키 : 개인 PC 브라우저에 저장
  - 세션 : 웹 서버에 저장
<br>

### ✔ 사용 이유
- HTTP 프로토콜의 특징이자 약점을 보완하고자 사용하게 되었다.

- HTTP는 stateless, connectionless 하기 때문에 이전 정보를 저장하지 않고, 연결을 끊어버린다.
  - ex) 손님 : A 물건 주세요 -> HTTP : 네~ 응답<br>
손님 : 결제해주세요 -> HTTP : ...? 무슨 물건?
  - 이런식으로 HTTP는 이전 정보를 저장하지 않는다.
<br>

- 그러나 웹을 이용하면 HTTP 통신을 사용할 수밖에 없고 정보를 유지해야 하는 경우가 많다.

- 이 때문에 세션과 쿠키라는 개념이 등장하여 정보를 유지할 수 있게 해주는 것이다.
<br>
<hr>
<br>

### ✔ 세션 대신 쿠키를 사용하는 이유
- 세션은 웹 서버에 저장되고 쿠키는 브라우저에 저장된다.

- 이는 당연하게도 서버보다 브라우저에 저장되는 쿠키가 보안성 측면에서 안좋을 수 밖에 없는데 왜 사용하는 것일까?

- 세션은 서버 사용자가 많을 경우 소모되는 자원이 상당하다.

- 이 때문에 클라이언트 요청에 대한 리소스에 대해서 세션과 쿠키를 적절하게 병행하여 사용해야 한다.
<br>
<hr>
<br>

### ✔ 쿠키 Cookie
- HTTP의 일종으로 사용자가 어떠한 웹 사이트를 방문할 경우, 그 사이트가 사용하고 있는 서버에서 사용자의 컴퓨터에 저장하는 작은 기록 정보 파일이다.

- HTTP에서 클라이언트의 상태 정보를 클라이언트의 PC에 저장하였다가 필요시 정보를 참조하거나 재사용할 수 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/d253cec5-689e-45f2-a7b3-49c0d28d5dbb)
<br>

**동작 순서**
1. 클라이언트가 페이지를 요청한다. (사용자가 웹사이트 접근)
2. 웹 서버는 쿠키를 생성한다.
3. 생성한 쿠키는 response header에 쿠키 정보를 담아 HTTP 화면을 돌려줄 때,  같이 클라이언트에게 돌려준다.
4. 넘겨 받은 쿠키는 클라이언트가 가지고 있다가(로컬 PC에 저장), 다시 서버에 요청할 때 요청과 함께 쿠키를 항상 전송한다.
    - 서버에 request 할 때, 자동으로 cookie를 같이 전송하니, 이 부분도 비효율 적이라고 할 수 있다.
5. 동일 사이트 재방문시 클라이언트의 PC에 해당 쿠키가 있는 경우, 요청 페이지와 함께 쿠키를 전송한다.
<br>
<br>

**사용 예시**
- 방문했던 사이트에 다시 방문 하였을 때 아이디와 비밀번호 자동 입력

- 팝업창을 통해 "오늘 이 창을 다시 보지 않기" 체크
<br>
<br>

**특징**
- 웹 서버에서도 쿠키가 저장될 수 있는데, 이를 세션 쿠키라고 한다.
  - 세션이 유지되는 동안에만 유지되는 쿠키이다.
  - 브라우저를 닫으므로 세션이 만료된다면, 세션 쿠키도 함께 사라진다.
<br>

- 유효기간이 있는 쿠키 -> permanent cookie
<br>
<hr>
<br>

### ✔ 세션 Session
- 일정 시간동안 같은 사용자(브라우저)로부터 들어오는 일련의 요구를 하나의 상태로 보고, 그 상태를 일정하게 유지시키는 기술이다.

- 즉, 방문자가 웹 서버에 접속해 있는 상태를 하나의 단위로 보고 그것을 세션이라고 한다.

- 쿠키와는 다르게 웹 서버에 정보를 저장한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/cd9f38e6-ec58-4297-bc82-3aea0414ea14)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/1adeabbe-eb7c-4df0-87f5-84fd7d4d329e)
<br>

**주요 특징**
- 서버에 저장되기 때문에 브라우저에 저장하는 쿠키보다 보안성 측면에서 좋다.

- 저장 데이터에 제한이 없다. (서버 용량이 관건)

- 각 클라이언트 고유 Session ID를 부여한다.

- 웹 서버에서 요청을 보낸 웹 클라이언트를 구분(식별)하기 위해 사용
<br>
<br>

**동작 순서**
1. 클라이언트가 페이지를 요청한다. (사용자가 웹사이트 접근)
2. 서버는 접근한 클라이언트의 Request-Header 필드인 Cookie를 확인하여, 클라이언트가 해당 session-id를 보냈는지 확인한다.
3. session-id가 존재하지 않는다면, 서버는 session-id를 생성해 cookie에 담아서 클라이언트에게 돌려준다.
  - 브라우저에서 request를 할 때 마다, cookie가 자동으로 전송되기 때문에 cookie안에 담긴 session 또한 자동으로 전송되는 것
<br>

4. 서버에서 클라이언트로 돌려준 session-id를 쿠키를 사용해 서버에 저장한다. -> 쿠키 이름 : JSESSIONID5.
  - tomcat이기 때문에 JSESSIONID 이다. 다른 서버를 쓰면 session ID 도 바뀐다.
<br>

5. 클라이언트는 재접속 시, 이 쿠키(JSESSIONID)를 이용하여 session-id 값을 서버에 전달
<br>
<br>

**사용 예시**
- 화면이 이동해도 로그인 정보가 풀리지 않는다. -> 로그아웃 전까지 유지
<br>
<hr>
<br>

**Reference**<br>

[99C0RN : 쿠키, 세션 특징 및 차이](https://hahahoho5915.tistory.com/32)<br>
[IT핥기 : [JSP] Cookie(쿠키) 이해하기](https://www.youtube.com/watch?v=a2tvTi9qgaE&list=PLpzDq-W37heSMxWj0XEVfM1rUcHBDjhm3&index=16)<br>
[IT핥기 : [JSP] Session(세션) 이해하기](https://www.youtube.com/watch?v=VrWK1VPW5Qg&t=287s)
