# proxy란?

- 클라이언트와 서버 사이에 존재하며, 중계기로서 대리로 통신을 수행하는 것을 Proxy라고 하며,<br> 그 중계 기능을 하는 주체를 Proxy Server라고 한다.

- 보통 웹은 클라이언트에서 서버로, 서버에서 클라이언트로 통신하며 데이터를 전달한다.<br>
이때 필연적으로 중복되는 데이터를 반복하여 전달하는 상황이 발생하는데, 이렇게 동일한 요청 매번 처리하는 것은 곧 리소스 낭비 와 서버의 부하 로 이어지게 된다.<br>
때문에 본 서버에 도달하기 전에 새로운 서버(proxy server)를 미리 배치하여 중복 요청에 대해 (연산이 필요없는) 동일한 응답을 할 수 있다면, 클라이언트에겐 빠른 속도의 서비스를, 서버에게는 불필요한 부하를 줄이는 효과를 낼 수 있게 된다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/3306c1a2-8826-4ecc-849f-42cf81368d0e)
<br>
<hr>



### ✔ 사용 이유
- 보안 : 프록시 서버 없이 클라이언트가 서버에 요청 시 본인의 IP 주소가 노출되는데,<br>
프록시 서버를 사용 시 서버측에서나의 IP가 아닌 프록시 서버의 IP를 보게 된다. 즉, IP를 숨길 수 있다.<br>
또는 특정 사이트 접근 제한이 가능

- 캐시 : 같은 요청이 들어왔을 때, Cache 자원을 반환하여 서비스의 속도를 높여준다.

- 우회 : IP 접속 차단의 경우 프록시 서버를 통해 속여 접근이 가능하다.
<br>
<hr>

### ✔ proxy 종류
- 프록시 서버는 네트워크 상 어디에 위치하느냐, 혹은 어느 방향으로 데이터를 제공하느냐에 따라<br>forward Proxy 와 Reverse Proxy 로 나뉘게 된다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/35efef4b-ea8a-460a-8e5e-a8bdfea56fa6)
<br>
<br>

### 포워드 프록시(forward proxy)
- 클라이언트 바로 뒤에 있으며, 클라이언트의 요청을 받아 인터넷을 통해 서버에서<br>
데이터를 가져온 후, 클라이언트에게 응답을 한다.

- 즉, 클라이언트가 서버에 접근하고자 할때, 클라이언트는 타겟 서버의 주소를 포워드 프록시에 전달하여,<br>
포워드 프록시가 인터넷으로 요청된 내용을 가져오는 방식이다.

- ex) 우리가 naver.com을 요청하면 프록시 서버가 naver.com의 리소스를 받아와 클라이언트에게 forward 하는 것이다.<br>
**💡 우리가 흔히 알고 있는 프록시는 이러한 포워드 프록시에 해당한다.**
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/e43fe8b0-4f41-4aca-8150-52e1dbc2f93c)
<br>
<br>

### 리버스 프록시(reverse proxy)
- 리버스 프록시는 아래 그림 처럼 웹서버/WAS 앞에 놓여 있는 것을 말한다.

- 클라이언트는 웹서비스에 접근할때 웹서버에 요청하는 것이 아닌, 프록시로 요청하게 되고,<br>
프록시가 배후(reverse)의 서버로부터 데이터를 가져오는 방식이다.

- 내부 서버가 직접 서비스를 제공해도 되지만 이렇게 구성하는 이유는 보안 때문이다.<br>
보통 기업의 네트워크환경에서는 DMZ라고 부르는 내부네트워크/외부네트워크 사이에 위치하는 구간이 존재한다.<br>
이 구간에는 보통 메일 서버, 웹 서버, FTP 서버 등 외부 서비스를 제공하는 서버가 위치하게 된다.<br>
[DMZ란?](https://github.com/yejun95/Today-I-Learn/blob/master/ETC/DMZ.md)

- WAS를 DMZ에 놓고 서비스해도 되지만 보안상문제가 있기 때문에 그렇게 하진 않는다.<br>
WAS는 DB서버와 연결되어 있으므로, WAS가 해킹당할 경우 DB서버까지 해킹당할 수 있는 문제가 발생할 수 있기 때문이다.<br>
따라서 리버스 프록시 서버를 DMZ에 두고 실제 서비스 서버는 내부망에 위치시킨 후 서비스 하는 것이 일반적이다.

- **우리가 구성하는 일반적인 WEB(Apache, nginx) - WAS(Tomcat) 분리 형태를 Reverse 프록시라고 보면 된다.<br>**
여기서 WEB(Apache, nginx)가 reverse proxy가 된다.<br>
물론 아파치 톰캣 같이 물리적인 한서버에 web, was가 존재한다면 reverse proxy라고 볼 수 없다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/7fdcfc7c-fe73-4e45-93e3-2b0eb3aa5b9f)
<br>
<br>

**리버스 프록시의 사용 이점**
- 로드 밸런싱<br>
  - 대량의 트래픽을 하나의 서버가 관리하기 힘들기 때문에 중간에서 프록시 서버가<br>
여러 대의 서버로 분산을 해준다. => [Nginx](https://github.com/yejun95/Today-I-Learn/blob/master/ETC/Nginx.md)

- 서버 보안<br>
  - 본래 서버의 IP 주소를 노출시키지 않을 수 있다.
  - 위의 그림 에서 보면, 클라이언트는 인터넷을 통해 리버스 프록시 서버 url에게 요청을 한다.<br>
그리고 리버스 프록시는 본서버에게 요청을 경유해서 보내게 된다. 이렇게 되면 클라이언트는 본 서버의 url을 모른채<br>
리버스 프록시 url을 통해 서비스를 이용하게 되고, 이는 즉 본서버의 정보를 숨기는 효과가 된다.
<br>

### ✔포워드 프록시 vs 리버스 프록시 차이점
**프록시 서버 위치**
- Forward Proxy 서버는 클라이언트 앞에 놓여져 있는 반면,
- Reverse Proxy 서버는 웹서버/WAS 앞에 놓여 있다는 차이점이 있다.
<br>

**프록시 서버 통신 대상**
- Forward Proxy는 내부망에서 클라이언트와 Proxy 서버가 통신하여 인터넷을 통해 외부에서 데이터를 가져온다.
- Reverse Proxy는 내부망에서 Proxy 서버와 내부망서버가 통신하여 인터넷을 통해 요청이 들어오면 Proxy 서버가 받아 응답해준다.
<br>

**감춰지는 대상**
- Forward Proxy는 직접 서버 url로 요청을 보내지만, Reverse Proxy는 프록시 서버 url로만 접근이 가능하다.<br>
이로서 Reverse Proxy는 본서버의 IP 정보를 숨길수 있는 효과를 얻게 된다.

- Forward Proxy는 내부망에서 인터넷 상에 있는 서버에 요청할때 먼저 포워드 프록시 서버를 호출하고<br>
프록시가 서버에게 요청을 보내게 되는데, 이로서 서버에게 클라이언트가 누구인지 감출수 있다.<br>
즉, 서버 입장에서 응답받은 IP는 포워드 Forward Proxy의 IP이기 때문에 클라이언트가 누군지 알 수 없다.
<br>

<hr>

**참고 자료**<br>
[younghyun : proxy란?](https://velog.io/@younghyun/%ED%94%84%EB%A1%9D%EC%8B%9CProxy%EB%9E%80)<br>
[코딩각 : 프록시 서버란 무엇인가?](https://digiconfactory.tistory.com/entry/%ED%94%84%EB%A1%9D%EC%8B%9C-%EC%84%9C%EB%B2%84-Proxy-Server-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)<br>
[inpa : Reverse Proxy / Forward Prox](https://inpa.tistory.com/entry/NETWORK-%F0%9F%93%A1-Reverse-Proxy-Forward-Proxy-%EC%A0%95%EC%9D%98-%EC%B0%A8%EC%9D%B4-%EC%A0%95%EB%A6%AC)
