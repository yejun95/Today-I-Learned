## CORS
- CORS(Cross-Origin Resource Sharing)는 출처가 다른 자원들을 공유한다는 뜻으로, 한 출처에 있는 자원에서 다른 출처에 있는 자원에 접근하도록 하는 개념이다.

- 출처(origin) : URL의 프로토콜, 호스트(도메인), 포트를 의미하며,  만약 두 객체의 Protocol, Host, Port가 모두 일치하는 
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

**💡 사용 이유**
- 불법 사이트나 카피 사이트를 통해 브라우저에 저장된 개인정보의 탈취를 방지하기 위함
```
ex) 클라이언트가 인스타그램 사용 중 해커의 스크립트가 심어진 http://hacking.com/ URL을 눌러 페이지를 열었다. 
해커의 페이지에는 클라이언트의 토큰을 사용해 개인정보를 탈취하는 스크립트가 심어져있었고 클라이언트의 
개인정보는 그대로 해커에게 넘어갔다.
```
