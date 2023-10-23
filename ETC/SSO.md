## SSO (Single-Sign-On)이란?
- 한 번의 인증 과정(로그인)으로 여러 컴퓨터 상의 자원을<br>
이용가능하게 하는 인증 기능이다.

- 아래 그림처럼 하나의 ID로 한 번의 인증을 거쳐 어플리케이션을 사용한다.


![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/fe85a15b-6803-44cc-9e38-4333a7e85834)
<br>
<br>

- 가장 직관적으로 이해하기 쉬운 예로 구글의 서비스를 들 수 있다.

- 구글의 경우 아이디 하나로 구글 드라이브, gmail, google 포토 등을 이용할 수 있다.

- SP(Service Provider): 서비스, 애플리케이션 등 다양하게 불리고 있으며, 인증을 거쳐야만 사용할 수 있는 서비스를 의미
- IdP(Identity Provider): SP를 사용하기 위해 인증을 할 수 있도록 공통 인증 관련 부분만 모아서 구성한 서비스<br>
보통 하나의 회사가 다양한 인증이 필요한 서비스를 이용할 수 있도록 하기 위한 그룹이며,<br>
타 회사 인증 부분까지 포함하는 IdP는 거의 없다고 한다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/6350d9a0-ad9c-4a69-93c7-52b0b853bc0b)
<br>
<hr>
<br>

### ✔ SSO 유형
**인증 위임 모델(Delegation)**
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/b200801f-c235-4a5b-a848-1eb3570d5c86)
<br>

- SSO 에이전트가 인증을 대행하는 방식

- 대상 애플리케이션의 인증 방식을 변경하기 어려울 때 사용

- 사용자의 인증 정보를 SSO 에이전트가 관리하며 로그인 대신 수행

- 동작 방식
  - User가 Target Server에 로그인할 때 ID, PW가 필요할 때, Agent가 이 정보를 가지고 있으며<br>
  Target Server에 User가 접근할 때 Agent가 User를 대신하여 ID/PW 정보를 전달하여 로그인 대행

<br>
<br>

**인증 정보 전달 모델(Propagation)**
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/588b4e0a-1a6a-4131-8d23-4e1aabf4e9e8)
<br>

- SSO 시스템과 신뢰관계를 토대로 사용자를 인증한 사실을 전달받아 SSO를 구현하는 방식

- 통합 인증을 수행하는 '곳'에서 인증을 받아, 대상 어플리케이션으로 전달할 토근(Tokem)을 발급 받고<br>
대상 어플리케이션에 사용자가 접글할 때 토큰을 자동으로 전달하여 사용자를 확인하게 하는 방식

- 웹 환경에서는 Cookie를 이용하여 토큰을 자동으로 대상 어플리케이션에 전달 가능

- 보통 웹 기반에서는 해당 방식을 주로 사용한다.


**Reference**<br>
[JuHyeong.dev : SSO(Single Sign-On) 통합인증 이란?](https://dkswnkk.tistory.com/581)<br>
[코딩하는 주노 이야기 : 서버 공통 SSO(Single Sign-On 이란](https://co-no.tistory.com/36)

