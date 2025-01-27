# 웹 브라우저에 URL을 입력하면 발생하는 과정
- 우리가 인터넷을 이용할 때, 브라우저 주소창에 `www.naver.com` 이라고 입력을 하면<br>
네이버 화면이 출력이 된다.

- 우리는 네이버의 IP 주소를 모르는데, URL을 입력하는 것만으로 어떻게 네이버에서 콘텐츠를 나에게 보여주는 것일까?

- 해당 글을 통해 URL 입력을 통해 일어나는 과정을 인프라 위주와 TCP/IP, CDN, GSLB 등을 묶어서 설명을 할 것이다.
<br>
<hr>
<br>

## ✔️ 클라이언트(PC) 관점에서 설명
- 설명 전 네이버의 라우터 구조를 모르기 때문에 해당 부분은 생략하고 설명함.

![image](https://github.com/user-attachments/assets/3cc27fe9-3bfb-4806-b489-c5f79458bfcc)
> PC > Naver의 인프라 구조
<br>

### 1. PC에서 url에 www.naver.com을 입력
![image](https://github.com/user-attachments/assets/8aa0d9cd-0d5c-437c-b304-969dd3b6d8e8)
<br>

### 2. DNS가 네이버의 IP 주소를 PC에게 알려줌
![image](https://github.com/user-attachments/assets/77e3bb4e-697c-4cb6-b439-27a14ec851d4)
> 공유기가 DNS인 경우, 공유기에게 질의<br>
ISP를 통해 DNS 서버에 접근할 경우, 해당 DNS 에게 질의

- DNS에게 질의 전, PC는 3가지의 확인 작업을 거친다.
<br>

1. hosts 파일에 입력된 도메인에 대한 IP주소가 있는지 확인한다.

2. hosts 파일에 등록된 도메인이 없다면 DNS Cache를 통해 캐싱 결과를 확인하고, 캐싱된 주소가 있으면 이를 응답한다.

3. 캐싱된 주소도 없다면 그때 DNS에게 직접 질의를 한다.
    - 내 PC의 공유기가 DNS 서버일 경우, 내 공유기에 질의
    - ISP상에 있는 DNS 서버에게 질의
<br>

### 3. PC가 네이버의 IP 주소를 획득
![image](https://github.com/user-attachments/assets/0d5f445a-3971-401a-951d-e7a55a8e766c)
> 2번 과정에서 hosts 파일과 캐싱된 주소가 없다면, DNS에게 직접 질의하여 IP 주소를 응답받는다.
<br>

### 4. PC에서 네이버의 IP주소로 TCP 연결을 진행
![image](https://github.com/user-attachments/assets/212986c8-38fc-42b8-805f-baf4e984fdf7)
> Naver의 IP 주소를 획득한 뒤에 TCP 연결(3way handshake)를 진행한다.
<br>

- TCP 연결 이후 Naver 서버가 PC에게 컨텐츠를 제공하게 된다.

- 여기서 컨텐츠를 제공하기 위한 데이터의 흐름을 추가로 설명하는 글을 참조 글로 넣었으니 필히 읽어 볼 것.
  - 정확히는 5번, 6번 과정에서 HTTP Request, Response를 하였을 때, 데이터의 이동 흐름이다.
<br>

- [TCP 송수신 원리](https://github.com/yejun95/Today-I-Learned/edit/master/Network/%5BNetwork%5D%20TCP%20%EC%86%A1%EC%88%98%EC%8B%A0%EC%9B%90%EB%A6%AC.md)

<br>

### 5. PC가 HTTP Request를 보냄
![image](https://github.com/user-attachments/assets/b51f78bb-b049-4afd-8e8d-797a097cbd86)
<br>

### 6. 네이버가 HTTP Response를 보냄
![image](https://github.com/user-attachments/assets/1151b1a2-05cd-4deb-8a9b-e013e6c63606)
<br>
<hr>
<br>

## ✔️ CDN과 GSLB
- 앞서 설명했던 부분들은 url 입력 후에 일어나는 과정을 단순하게 설명했을 뿐이다.

- 서버의 규모가 커지고 트래픽이 많아 질수록 부하 분산, 장애 대응 등 여러 부분에 걸쳐 고민을 해야 되는데,
이때 추가되야 할 것이 CDN과 GSLB다.
<br>
<br>

### CDN : Content Delivery Network
![image](https://github.com/user-attachments/assets/c2c2074a-88ed-4904-90f6-e4a3b0faa1aa)
<br>

![image](https://github.com/user-attachments/assets/d0d28589-d319-437d-b1f1-2215ec38327b)
> 각 지역에 분산되어 있는 CDN 서버 예시
<br>

![image](https://github.com/user-attachments/assets/3228a643-d505-42a2-80a8-d4aea66be15c)
> 좌 : 데이터 접근을 위해 서버에 직접 접근, 우 : 데이터 접근을 위해 사용자로부터 가까운 서버로 접근
<br>

- 지역적으로 분산되어 있는 프록시 서버들의 클러스터 데이터 센터
  - 보통 상용 사업자들이다. (Ex. Akamai, KT, SK 등)
<br>

- 유저 관점에서 빠르게 컨텐츠를 받아볼 수 있다.
  - Ex) 한국에서 Naver에 접속한다면, 가장 빠른 속도가 보장가능한 CDN 엣지 서버에서 컨텐츠를 제공한다.
  - 즉, 물리적 거리에 따라 그때 그때 다른 CDN 엣지 서버가 컨텐츠를 제공해줌
<br>

- 원본 서버의 부하 경감(오프로드)
  - CDN 서버에는 Naver의 기타 정적 요소 등이 상당 수 캐싱되어 있다.
  - 이에 따라 유저의 요청이 캐싱되어 있는 데이터라면 원본 서버까지 요청이 오지 않아도 된다.
  - 정적 캐싱 : 미리 CDN 서버에 캐싱해둠
  - 동적 캐싱 : 유저 요청에 따라 보낼 컨텐츠가 엣지에 있는지 확인
    - cache hit - 요청에 맞는 캐싱 데이터가 있으면 바로 응답
    - cache miss - 요청에 맞는 캐싱 데이터가 없으면 그때 서버에게 요청
<br>

- 유저의 요청을 먼저 받기 때문에 원본 서버를 숨길 수 있어 보안적으로 유리
<br>

**➡️ 글로벌 업체인 LINE CDN 흐름**

![image](https://github.com/user-attachments/assets/a05fd32b-9f69-42b7-bd4b-049cc3abbbe8)
<br>

**➡️ 처리 과정**

![image](https://github.com/user-attachments/assets/e6a147ef-9d5d-4103-a658-b6cf2c4e5aee)
<br>
<br>

### GSLB : Global Server Load Balancing
- IP 주소와 Port를 기반으로 트래픽을 분산시키는 로드밸런서와는 달리 GSLB는 기존의 DNS에서 한 단계 더 발전한 개념이다.(DNS 기반 로드밸런싱 서비스)  

- 기존 DNS는 라운드 로빈 방식을 사용함으로써 정교하진 않지만, GSLB의 경우 주기적으로 HealthCheck를 통해 원활한 서버로 접근할 수 있도록 한다.
<br>

**➡️ DNS를 통한 기존 로드밸런싱**

![image](https://github.com/user-attachments/assets/38b05caf-a68b-425e-8bcb-be5f7cf4ddef)
> 적절한 배분이 되지 않는다.
<br>

**➡️ GSLB를 활용한 로드밸런싱**

![image](https://github.com/user-attachments/assets/67bf5722-4186-4bd0-87e2-815ee70bb7e6)
> 서버 부하율에 따라 적절히 배분한다.
<br>

**➡️ 재해 복구 관점에서의 GSLB**

![image](https://github.com/user-attachments/assets/a3b6569b-430a-43e8-a009-732c563cd2be)
> health check를 통해 서버가 죽으면 GSLB가 유저 위치에 따라 최적의 서버로 다시 연결시켜준다.
<br>

**➡️ 동작 흐름**
![image](https://github.com/user-attachments/assets/7241605f-6410-482a-a735-dad67a784890)
<br>
<hr>
<br>

**Reference**<br>

[널널한 개발자 : 웹 브라우저에 URL 입력하면 일어나는 일 - 인프라 위주](https://www.youtube.com/watch?v=GAyZ_QgYYYo)
[널널한 개발자 : ]
[Myster리 : CDN이란 무엇일까?](https://mysterlee.tistory.com/72)
