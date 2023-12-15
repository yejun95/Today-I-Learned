## CGI란 무엇인가?
- Common Gateway Interface

- 서버와 애플리케이션 간에 데이터를 주고 받는 방식 또는 컨벤션이다.

- 별도로 제작된 웹서버와 프로그램간의 교환방식이다.

- C, Perl, Python 등 으로 구현 가능

- 서버의 cgi-bin이라는 폴더를 만들어놓고, 그 내부의 스크립트 파일을 만들어놓는다.
  - 옛날 페이지의 경우 /cgi-bin/ 이라는 경로가 있으면 이는 CGI를 활용한 경우이다. 
<br>

- 웹서버가 CGI를 통해 cgi bin에 접속해서 그 내부의 파일을 실행시키고, 그 결과를 클라이언트에 보낸다.
<br>
<hr>
<br>

### ✔ 등장 배경
- 웹이 처음 등장했을 때는 HTML과 이미지를 전달해주는 웹서버밖에 없었다. 

- 하지만 웹에 대한 수요가 증가함에 따라서 정적인 HTML만을 가지고 정보를 제공하는 것에 한계가 생겼고, 이를 극복하기 위해서 등장한 기술이 CGI다. 

- 웹서버로 요청이 들어왔을 때 그것이 웹서버가 처리할 수 없는 정보일 때 그 정보를 처리할 수 있는 외부 프로그램을 호출해서<br>
외부 프로그램이 처리한 결과를 웹서버가 받아서 브라우저로 전송하는 것이다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/01ac1def-dbef-41d1-9d8e-7147726997a7)

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/77f96f98-27f8-4712-aa48-b2cb4a95d7a1)
<br>
<hr>
<br>

### ✔ 문제점
- 클라이언트 요청마다 프로세스를 생성한다.

- 파이썬으로 CGI 프로그램이 작성되었으면 요청이 있을 때마다 파이썬 인터프리터를 각각 새로 구동하게 되고<br>
요청이 많아질수록 프로세스가 많아져 시스템에 부하를 주게 된다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/ea07c817-616a-43d0-b1a7-d6b7f79b6e50)
<br>
<hr>
<br>

### ✔ WAS의 등장
- 클라이언트의 요청마다 프로세스를 생성한다는 단점과 정적, 동적 서버를 나누기 위해 등장하였다.

- 웹서버 -> CGI의 경로에서 웹서버 -> WAS로 변경
  - 결국 근래에는 CGI의 역할을 WAS가 하는 것이다.
<br>

- 더 이상 웹서버에서 프로그램을 호출하지 않고 WAS에서 프로그램 실행 후 실행 결과를 웹서버에 전달한다.
  - ex) tomcat
<br>
<hr>
<br>

**Reference**<br>

[탐p슨 : Common Gateway Interface(CGI)란 무엇인가](https://live-everyday.tistory.com/197)<br>
[ben_DS : CGI 기술의 등장 배경과 WAS로의 발전](https://bentist.tistory.com/40)<br>
