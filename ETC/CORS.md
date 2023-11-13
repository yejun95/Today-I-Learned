## CORS
- CORS(Cross-Origin Resource Sharing)는 출처가 다른 자원들을 공유한다는 뜻으로, 한 출처에 있는 자원에서 다른 출처에 있는 자원에 접근하도록 하는 개념이다.

- 출처(origin) : URL의 프로토콜, 호스트(도메인), 포트를 의미하며,  만약 두 객체의 Protocol, Host, Port가 모두 일치하는<br>
경우 같은 출처를 가졌다고 말한다.

![image](https://github.com/bjsystems/rnd/assets/121341413/a3d72ead-9a9a-4221-806b-38bf4c792f3c)
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/5a036893-5e49-4946-b954-a255347e6dbd)
![image](https://github.com/bjsystems/rnd/assets/121341413/047ca7c9-b832-4417-893d-8375b77bcfc6)
<br>


![image](https://github.com/bjsystems/rnd/assets/121341413/05622dad-667c-47ad-9328-6e18ea027e1e)
<br>

- 기본적으로 동일 출처 요청만 자유롭게 요청이 가능하며 동일 출처 정책(Same-Origin Policy) 이라고 한다.

- 하지만 기준을 완화하여 다른 출처 요청도 할 수 있도록 기준을 만든 체제가 다른 출처 정책(Cross-Origin Policy)이다.

- 이렇듯 다른 출처인 경우에 요청하는 곳과 응답하는 곳이 일치하지 않으면 CORS 정책에 준수하여 요청해야만 
응답을 얻을 수가 있다.
<br>

**Cross-Origin을 지원하는 태그**
```javascript
<link rel="stylesheet" href="…" />
<script src="…"></script>  // type="module" 속성은 제외
<img src="…" />
```
<br>
<hr>
<br>

### ✔ 사용 이유
- 불법 사이트나 카피 사이트를 통해 브라우저에 저장된 개인정보의 탈취를 방지하기 위함이다.

- 제약이 없다면, 해커가 CSRF(Cross-Site Request Forgery)나 XSS(Cross-Site Scripting) 등의 방법을 이용해서<br>
우리가 만든 어플리케이션에서 해커가 심어놓은 코드가 실행하여 개인 정보를 가로챌 수 있다.

- 아래 사진은 SOP 정책이 없는 상황에서 악의적인 홈페이지에 접속하는 상황을 가정 한 것이다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/94e7fc25-ee1f-4aea-8cf7-da7fbfc1de61)

1. 사용자가 악성 사이트에 접속한다.

2. 이때 해커가 몰래 심어놓은 악의적인 자바스크립트가 실행되어, 사용자가 모르는 사이에 어느 포털 사이트에 요청을 보낸다.

3. 그럼 포털 사이트에서 해당 브라우저의 쿠키를 이용하여 로그인을 하거나 등 상호작용에 따른 개인 정보를 응답 값을 받은뒤,<br>
사이트에서 해커 서버(hacker.example.com)로 재차 보낸다.

4. 이외에도 사용자가 접속중인 내부망의 아이피와 포트를 가져오거나, 해커가 사용자 브라우저를 프록시처럼 악용할 수도 있다.

- 따라서 이러한 악의적인 경우를 방지하기 위해, SOP 정책으로 동일하지 않는 다른 출처의 스크립트가 실행되지 않도록 브라우저에서 사전에 방지하는 것이다.
<br>
<hr>
<br>

### ✔ 브라우저에서 CORS 기본동작
1. 클라이언트에서 HTTP요청의 헤더에 Origin을 담아 전달
 - 기본적으로 웹은 HTTP 프로토콜을 이용하여 서버에 요청을 보내게 되는데,
 - 이때 브라우저는 요청 헤더에 Origin 이라는 필드에 출처를 함께 담아 보내게 된다.
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/09a4a89b-2e94-4bfb-a932-617d6e0cc1ce)
<br>

2. 서버는 응답헤더에 Access-Control-Allow-Origin을 담아 클라이언트로 전달한다.
- 이후 서버가 이 요청에 대한 응답을 할 때 응답 헤더에 `Access-Control-Allow-Origin`이라는 필드를 추가하고 값으로<br>
'이 리소스를 접근하는 것이 허용된 출처 url'을 내려보낸다.
<br>

3. 클라이언트에서 Origin과 서버가 보내준 Access-Control-Allow-Origin을 비교한다.
- 이후 응답을 받은 브라우저는 자신이 보냈던 요청의 Origin과 서버가 보내준 응답의 Access-Control-Allow-Origin을 비교해본 후 차단할지 말지를 결정한다.

- 만약 유효하지 않다면 그 응답을 사용하지 않고 버린다. (CORS 에러 !!)

- 위의 경우에는 둘다 http://localhost:3000이기 때문에 유효하니 다른 출처의 리소스를 문제없이 가져오게 된다.

- 결국 서버에서 `Access-Control-Allow-Origin` 헤더에 허용할 출처를 기재해서 클라이언트에 응답하면 되는 것이었다.
  - 즉, 백엔드 개발자가 고쳐야될 부분인 것이다.
<br>
<hr>
<br>

**Reference**<br>

[Inpa Dev : 악명 높은 CORS 개념 & 해결법 - 정리 끝판왕](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F)<br>
[돈 받고 일하면 프로 : CORS, HTTP Only](https://shirohoo.github.io/backend/server-side/2021-09-28-cors/)
