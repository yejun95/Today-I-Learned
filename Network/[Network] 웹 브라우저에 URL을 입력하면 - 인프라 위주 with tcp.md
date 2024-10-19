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

![image](https://github.com/user-attachments/assets/72572d0b-bc45-4422-baad-e1e07da0c9e7)
> PC > Naver의 인프라 구조
<br>

### 1. PC에서 url에 www.naver.com을 입력
<br>

### 2. DNS가 네이버의 IP 주소를 PC에게 알려줌
- DNS에게 질의 전, PC는 3가지의 확인 작업을 거친다.
<br>

1. hosts 파일에 입력된 도메인에 대한 IP주소가 있는지 확인한다.

2. hosts 파일에 등록된 도메인이 없다면 DNS Cache를 통해 캐싱 결과를 확인하고, 캐싱된 주소가 있으면 이를 응답한다.

3. 캐싱된 주소도 없다면 직접 DNS에게 직접 질의를 한다.
    - 내 PC의 공유기가 DNS 서버일 경우, 내 공유기에 질의
    - ISP에서 전달해준 DNS 서버에게 질의
<br>

### 3. PC가 네이버의 IP 주소를 획득
<br>

### 4. PC에서 네이버의 IP주소로 TCP 연결을 진행
<br>

### 5. PC가 HTTP Request를 보냄
<br>

### 6. 네이버가 HTTP Response를 보냄
<br>
<hr>
<br>

## ✔️ CDN과 GSLB

<br>
<hr>
<br>

**Reference**<br>

[널널한 개발자 : 웹 브라우저에 URL 입력하면 일어나는 일 - 인프라 위주](https://www.youtube.com/watch?v=GAyZ_QgYYYo)

