## SSH란 무엇인가?
- Secure Shell, 즉 네트워크 프로토콜 중 하나로 컴퓨터와 컴퓨터가 인터넷과 같은 Public Network를 통해<br>
서로 통신을 할 때 보안적으로 안전하게 통신을 하기 위해 사용하는 프로토콜이다.

- 기존의 유닉스 시스템 셸에 원격 접속하기 위해 사용하던 텔넷은 암호화가 이루어지지 않아 계정 정보가 탈취될 위험이 높으므로,<br>
여기에 암호화 기능을 추가하여 1995년에 나온 프로토콜이다.

- 데이터 전송, 원격 제어 등으로 쓰인다.

- 기본 포트 : 22

- SSH의 가장 핵심 키워드는 ‘KEY’ 다.<br>
사용자(클라이언트)와 서버(호스트)는 각각의 키를 보유하고 있으며, 이 키를 이용해 서로가 맞는지 인증하고 안전하게 데이터를 주고 받게 된다.<br>
키를 생성하는 방식은 두 가지가 있는데, ‘대칭키’와 ‘비대칭키(또는 공개 키)’ 방식이다.

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/4647abf6-dd93-40cb-ba6c-9e8474cd7768)
<br>

**비대칭키 방식**
- 비대칭키 방식에서는 서버 또는 사용자가 Key Pair(키 페어)를 생성한다.
- Key Pair는 공개 키 + 개인 키 로 이루어진 한 쌍이며, 보통 공개 키의 경우 .pub, 개인 키의 경우 .pem의 파일 형식이다.
<br>

**대칭키 방식**
- 서로가 누군지를 알았으니 이제 정보를 주고받을 차례다.

- 주고받는 과정에서 정보를 암호화해서 주고받는데, 여기서 사용되는 과정이 대칭키 방식이다.

- 대칭키 방식에서는 비대칭키 방식과 달리 한 개의 키(대칭 키) 만을 사용한다.

- ex) 사용자 또는 서버는 하나의 대칭 키를 서로 공유한다.<br>
한쪽이 대칭 키로 정보를 암호화하면, 다른쪽은 동일한 대칭 키로 암호를 풀고 정보를 받는다.<br>
정보 교환이 끝나면 사용된 대칭 키는 폐기되고 나중에 다시 접속할 때마다 새로운 대칭 키를 생성하여 사용한다.
<br>
<br>

- OS별 SSH

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/2d6f151a-ec29-4e80-b572-008da58750d5)


**Reference**<br>

[군옥수수수 : SSH란?](https://baked-corn.tistory.com/52)<br>
[hyeseong-dev.log : SSH란?](https://velog.io/@hyeseong-dev/%EB%A6%AC%EB%88%85%EC%8A%A4-ssh%EB%9E%80)<br>

