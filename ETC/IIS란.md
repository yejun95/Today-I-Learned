## IIS란 무엇인가?
- Internet Information Service

- 마이크로소프트의 윈도우에서 무료로 지원되는 웹 서버이다.

- Microsoft의 제품이므로 Microsoft Windows OS에서만 실행된다.

- ASPX는 IIS에서만 실행된다.

- 윈도우를 사용하여 쉽게 설치가 가능하며, 시각적으로 창에서 작업하는 경우가 많아서 용이한 작업이 가능하다.

- 웹 프로그램을 쉽게 설치 및 관리가 가능하고 설정 및 확인이 쉽게 가능하다.

- 서버는 현재 FTP, SMTP, NNTP, HTTP/HTTPS를 포함하고 있다.

- ASP 스크립트 언어를 사용할 수 있다.
  - [ASP란??]()
<br>
<hr>
<br>

### ✔ 설치 방법
- 이미 윈도우 안에 IIS가 내장되어 있으며, Windows 기능 켜기/끄기를 통해서 바로 설치할 수 있다.

- 반드시 .Net Framework도 설치해야 한다.

- 프로그램 및 기능 선택

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/fe0286bd-a9bb-44b7-a37e-8b3392a627de)
<br>
<br>

**아래와 같이 선택한다.**
- 인터넷 정보 서비스
  - World Wide Web 서비스(전부 다)
  - 웹 관리 도구
    - IIS 관리 콘솔

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/bedd0c35-2bb6-4aa1-bb03-382e504c1cba)
<>br
<br>

- 인터넷 정보 서비스에서 World Wide Web 서비스: 응용프로그램 개발 기능을 확장하여 상세목록을 띄워준다.

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/4d8e758c-2a31-42eb-92cd-6c8616e3144c)
<br>
<br>

- 다음 마지막으로 맨위에  .NET Framework 3.5 설치

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/da272462-2340-4130-8a4e-e4645743bffc)
<br>
<hr>
<br>

### ✔ 구동확인
- Windows 관리도구 -> IIS 관리자 선택

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/9fb05a6a-a283-45a4-9e24-5fcbc419f0f9)
<br>
<br>

- 사이트 -> Default Web Site

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/e98b1cc5-8310-4c85-be37-4168a5feed91)
<br>
<br>

- 실제 웹 서버가 구동되었는지 확인하기 위해서 웹 브라우저에 접속한다.

- 아래 주소에서 포트 번호 80은 입력하지 않아도 된다.

- http 기본 프로토콜은 80이기 때문이다.

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/efdcb7a9-8442-4ea3-89b0-1d2f3b1c550b)
<br>
<hr>
<br>

**Reference**<br>

[-o-k's story : [Windows] IIS(Internet Information Sevices) 웹서버 설치](https://t-okk.tistory.com/155)<br>
[Seek constantly : IIS(인터넷 정보 서비스)란?](https://deukyu.tistory.com/61)
