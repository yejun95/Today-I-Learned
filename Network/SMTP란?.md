## SMTP란 무엇인가?
- Simple Mail Transfer Protocol

- 전자 메일 전송을 위한 표준 프로토콜이다.

- 이때, 이메일을 송수신 하는 서버를 SMTP라고 한다.

- TCP 포트 번호 : 25
  - 새로운 SMTP 서버의 경우는 587번을 사용
<br>

- 우리가 메일을 보낼때는 바로 상대편의 컴퓨터로 메일을 송신하는것이 아니라,<br>
중간에 메일서버라는 곳을 몇군데 거치게 된다. 

- 메일서버에 메일이 보관되고 그것을 다시 다른 메일서버에 보내면서 결국 보내고자하는 end-user 에게 전해진다.

- SMTP외에도 POP3, IMAP이라는 다른 프로토콜이 존재

- SMTP가 메일서버 간 전송 규약이라면 POP3/IMAP는 **유저가 메일서버에서 메일을 받기 위한** 프로토콜이다.
  - POP3 : 메일을 서버에서 다운로드
  - IMAP : 중앙서버에서 동기화
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/e47bc780-b97d-4878-85d4-0a2da4bdaa53)
<br>
<hr>
<br>

### ✔ POP3
- Post Office Protocol

- 이메일을 받아오는 표준 프로토콜

- TCP 포트번호 : 110

- 사용자의 기기로 메시지를 다운로드하도록 하며, 다운로드 한 이후에는 서버에서 delete하는 stateless한 프로토콜이다.

- 메일 서버에서 이메일을 로컬 PC로 수신받을 수 있는 client / server 프로토콜이다.

- 메일 서버에 저장되어있는 메일을 로컬 pc로 가져오는 역할
<br>
<hr>
<br>

### ✔ IMAP
- Internet Message Access Protocol

- POP와 같이 메일 서버 종류 중 하나

- TCP 포트번호 : 143

- 서버에 모든 메시지 데이터를 저장해두고 변경사항을 동기화하는 stateful한 프로토콜

- POP와는 달리 중앙 서버에서 동기화가 이뤄지기 때문에 모든 장치에서 동일한 이메일 폴더를 확인 가능

- 스마트폰, 태블릿, PC모두 동일한 받은메일/보낸메일/기타폴더 등 모든 이메일 메시지를 볼 수 있다.

- 서버에 이메일이 남겨진 상태로 사용자에게 이메일을 보여준다.
  - 사용자는 언제 어디서나 원하는 메일을 열람
<br>

- 메일이 서버에 저장되어있기 때문에 로컬pc에 문제가 생겨도 이메일에는 아무 영향을 미치지 않는다. 
<br>
<hr>
<br>

### ✔ POP3 - IMAP 차이
|POP3|IMAP|
|------|---|
|메일 서버에서 로컬장치로 이메일을 다운로드 받음.|메일 서버에서 동기화가 이루어짐.|
|수신함 즉, 받은 메일만을 다운로드 하여 조회 가능.|수신함 뿐만 아니라 모든 메일함을 조회 가능.|
|로컬장치로 다운로드 시, 서버에서는 이메일 삭제|서버에 이메일 실시간 존재.|
|오프라인 지원 |온/오프 모두 지원|
<br>
<br>

- 아래 그림에서 보이듯이 양방향과 단방향 차이점도 존재한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/67204b32-79be-40e9-9a87-7dd83c4def4f)
<br>
<hr>
<br>

### ✔ 메일 주고 받기 예시
- 내 메일 : abc@naver.com
- 상대방 메일 : def@daum.net

- 내가 상대방에게 메일을 보낸다고 가정했을 때, naver와 daum에는 각각의 SMTP 서버가 존재한다.

- 나는 naver 메일서버에 daum으로 보낼 데이터를 전송한다. (SMTP)

- naver 메일 서버가 daum 메일 서버로 메일을 보낸다. (SMTP)

- daum 메일보관함에 데이터가 도착한다.

- 상대방은 daum 메일 서버 보관함에서 메일을 확인한다.
  - POP
  - IMAP
  - HTTP  
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/b8361c7c-3bda-4dcf-8a04-e3b942cde4ef)
<br>
<hr>
<br>

### ✔ 왜 SMTP로 메일을 받지 않고 POP, IMAP 등을 사용하는 것인가?
- SMTP는 이메일 송신에 특화된 프로토콜이기 때문에 수신받기 위한 프로토콜을 따로 사용해야한다.

- 맞춰 여러기기를 사용하거나, 오프라인에서만 사용하는 등 수신자가 원하는 상황이 다를 수 있기 때문에<br>
적절한 수신 프로토콜을 사용해야한다.
<br>
<hr>
<br>

### ✔ SMTP Format
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/7365517c-f4f0-492a-aa31-1d468deb351c)
<br>
<br>

- Header :엄청 많은 정보들이 들어있다. From, To, Body Format 등등
 
- Blank line : 공백
 
- Body : 전달할 메일의 내용들이 포함
<br>
<hr>
<br>

**Reference**<br>

[hwaya2828.log : SMTP](https://velog.io/@hwaya2828/SMTP)<br>
[완벽하기 쉽지 않지만 완벽해지려고 노력해야 한다 : 메일서버 개념 SMTP /POP3 / IMAP 란?](https://sangbeomkim.tistory.com/143)<br>
[개발자 아저씨들 힘을모아 : [네트워크📶] SMTP란 / SMTP Format / SMTP 명령어](https://programming119.tistory.com/152)<br>
[my devlog; : e-mail protocol: SMTP, POP3, IMAP이란?](https://hyunah-home.tistory.com/entry/e-mail-protocol-SMTP-POP3-IMAP%EC%9D%B4%EB%9E%80)<br>
