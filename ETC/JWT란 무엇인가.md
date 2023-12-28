## JWT란 무엇인가?
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/75722fcb-2542-4727-aa28-cfbbf4b46122)
<br>
<br>

- Json Web Token

- 클라이언트와 서버 사이에서 통신할 때, 권한을 위해 사용하는 토큰이다.

- 클라이언트가 서버에 접속을 하면 서버에서 해당 클라이언트에게 인증되었다는 의미로 '토큰'을 부여한다

- 이 토큰은 유일하며 토큰을 발급받은 클라이언트는 또 다시 서버에 요청을 보낼 때 요청 헤더에 토큰을 심어서 보낸다.

- 주로 앱과 서버 통신을 위해 많이 사용한다.
  - 웹에는 세션, 쿠키가 있지만 앱에서는 없기 때문이다.
<br>

- JSON 데이터를 Base64 URL-safe Encode 를 통해 인코딩하여 직렬화한 것

- 토큰 내부에는 위변조 방지를 위해 개인키를 통한 전자서명도 들어있다
<br>
<hr>
<br>

### ✔ 서버 기반 vs 토큰 기반
**서버 (세션) 기반 인증 시스템**
- 서버의 세션을 사용한다.

- 서버 측(램 or DB)에서 사용자의 인증 정보를 관리한다.
  - 조회하는 과정에서 많은 오버헤드가 발생
  - 트래픽이 증가할 경우, 많은 비용 발생
<br>

- 클라이언트의 요청을 받으면 클라이언트의 상태를 계속 유지한다. -> Stateful
  - 성능 문제 발생
<br>
<br>

**토큰 기반**
- 세션의 단점 극복

- 클라이언트에게 토큰을 발급하고, 클라이언트가 관리하기 때문에 서버에 부담이 없다.

- 클라이언트는 서버에 요청을 보낼 때, 헤더에 토큰을 함께 보낸다.

- 서버가 클라이언트의 상태를 유지하지 않음 -> Stateless

- 단점
  - 쿠키/세션과 다르게 토큰 자체의 데이터 길이가 길어, 인증 요청이 많아질수록 네트워크 부하가 심해질수 있다.
  - Payload 자체는 암호화되지 않기 때문에 유저의 중요한 정보는 담을 수 없다.
  - 토큰을 탈취당하면 대처하기 어렵다. (따라서 사용 기간 제한을 설정하는 식으로 극복한다)
<br>

**장단점 정리**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/c1921c2f-e6f0-48e8-b839-56ebeeb3ae23)
<br>
<hr>
<br>

### ✔ 토큰 인증 방식
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/96017329-6baf-4e1f-9385-f7553d16254c)

- 사용자가 아이디와 비밀번호로 로그인을 한다.

- 서버 측에서 사용자(클라이언트)에게 유일한 토큰을 발급한다.

- 클라이언트는 서버 측에서 전달받은 토큰을 쿠키나 스토리지에 저장해 두고,<br>
서버에 요청을 할 때마다 해당 토큰을 HTTP 요청 헤더에 포함시켜 전달한다.

- 서버는 전달받은 토큰을 검증하고 요청에 응답한다. 
토큰에는 요청한 사람의 정보가 담겨있기에 서버는 DB를 조회하지 않고 누가 요청하는지 알 수 있다.
<br>
<hr>
<br>

### ✔ JWT 구조
- `.` 을 구분자로 나누어지는 세 가지 문자열의 조합이다.

- `.` 을 기준으로 좌측부터 Header, Payload, Signature를 의미한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/2133ae8c-6e13-4516-ac46-8585c1004406)

- Header : JWT 에서 사용할 타입과 해시 알고리즘의 종류

- Payload : 서버에서 첨부한 사용자 권한 정보와 데이터

- Signature : Base64 URL-safe Encode 를 한 이후 Header 에 명시된 해시함수를 적용하고, 
개인키(Private Key)로 서명한 전자서명이 담겨있다.
<br>
<br>

**Header**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/b20c7bd1-5b8f-4bf9-ab61-0deb87fe2006)

- alg : 서명 암호화 알고리즘 (ex : HMAC, SHA256, RSA)

- typ : 토큰 유형
<br>
<br>

**Payload**
- 토큰에서 사용할 정보의 조각들인 Claim 이 담겨있다.
  - Claim : key-value 형식으로 이루어진 한 쌍의 정보
<br>

- 서버와 클라이언트가 주고받는 시스템에서 실제로 사용될 정보에 대한 내용을 담고 있는 섹션

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/9684e6a1-46d1-4c3e-a258-cdfa6e32c2eb)
<br>
<br>

- Payload는 정해진 데이터 타입은 없지만, 대표적으로 Registered claims, Public claims, Private claims 이렇게 세 가지로 나뉜다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/ef560660-518c-4ae5-af0d-54538d14bc71)
<br>

- Registed claims : 미리 정의된 클레임.
  - iss(issuer; 발행자), 
  - exp(expireation time; 만료 시간), 
  - sub(subject; 제목), 
  - iat(issued At; 발행 시간), 
  - jti(JWI ID)
<br>

- Public claims : 사용자가 정의할 수 있는 클레임 공개용 정보 전달을 위해 사용.

- Private claims : 해당하는 당사자들 간에 정보를 공유하기 위해 만들어진 사용자 지정 클레임. <br>
외부에 공개되도 상관없지만 해당 유저를 특정할 수 있는 정보들을 담는다
<br>
<br>

**Signature**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/13158e76-1c77-4b64-8be9-05e8eee4feef)
<br>

- 시그니처에서 사용하는 알고리즘은 헤더에서 정의한 알고리즘 방식(alg)을 활용한다.

- 시그니처의 구조는 (헤더 + 페이로드)와 서버가 갖고 있는 유일한 key 값을 합친 것을 헤더에서 정의한 알고리즘으로 암호화를 한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/d61f972a-09ba-419e-9d38-2f58fbe5e265)
<br>
<hr>
<br>

### ✔ JWT 인증 과정
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/e59ecf08-0f37-4792-9686-ebb1cbfd2464)
<br>
<br>

- 사용자가 ID, PW를 입력하여 서버에 로그인 인증을 요청한다.

- 서버에서 클라이언트로부터 인증 요청을 받으면, Header, PayLoad, Signature를 정의한다.
  - Hedaer, PayLoad, Signature를 각각 Base64로 한 번 더 암호화하여 JWT를 생성하고 이를 쿠키에 담아 클라이언트에게 발급한다.
<br>

- 클라이언트는 서버로부터 받은 JWT를 로컬 스토리지에 저장한다. (쿠키나 다른 곳에 저장할 수도 있음)
  - API를 서버에 요청할때 Authorization header에 Access Token을 담아서 보낸다.
<br>

- 서버가 할 일은 클라이언트가 Header에 담아서 보낸 JWT가 내 서버에서 발행한 토큰인지 일치 여부를 확인하여<br>
일치한다면 인증을 통과시켜주고 아니라면 통과시키지 않으면 된다.
  - 인증이 통과되었으므로 페이로드에 들어있는 유저의 정보들을 select해서 클라이언트에 돌려준다.
<br>

- 클라이언트가 서버에 요청을 했는데, 만일 액세스 토큰의 시간이 만료되면 클라이언트는 리프래시 토큰을 이용해서<br>
서버로부터 새로운 엑세스 토큰을 발급 받는다
<br>
<hr>
<br>

### ✔ 토큰이 신뢰성을 가지는 이유
- 유저 JWT: A(Header) + B(Payload) + C(Signature) 일 때 (만일 임의의 유저가 B를 수정했다고 하면 B'로 표시한다.)
 
- 다른 유저가 B를 임의로 수정 -> 유저 JWT: A + B' + C 수정한 토큰을 서버에 요청을 보내면 서버는 유효성 검사 시행
  - 유저 JWT: A + B' + C
  - 서버에서 검증 후 생성한 JWT: A + B' + C' => (signature) 불일치
<br>

 - 대조 결과가 일치하지 않아 유저의 정보가 임의로 조작되었음을 알 수 있다.
<br>

**토큰의 목적은 정보 보호가 아닌 위조 방지**
- 위에 사례처럼 토큰은 서버에 있는 데이터와 일치하는지 확인하는 역할을 한다.

- 디버거를 복호화가 쉽기 때문에 Payload에는 민감 정보를 넣지 말아야 한다.

- 시그니처에 사용된 비밀키가 노출되지 않는 이상 데이터 위조 시 바로 걸러진다.
<br>
<hr>
<br>

**Reference**<br>

[Inpa Dev : JWT 토큰 인증 이란? (쿠키 vs 세션 vs 토큰)](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-JWTjson-web-token-%EB%9E%80-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC#token_%EC%9D%B8%EC%A6%9D)<br>
[hahan : JWT란 무엇인가?](https://velog.io/@hahan/JWT%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)<br>
