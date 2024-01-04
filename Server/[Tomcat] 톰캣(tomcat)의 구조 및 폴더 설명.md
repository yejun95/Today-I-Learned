## 톰캣(tomcat)의 내부 구조
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/0d294f8c-aa39-4607-b33d-f858a15e179b)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/9744a1ed-2859-4ff1-a58b-f9c8db450963)

- 기본적으로 `server.xml`에 의해 위에와 같은 톰캣 구조를 확인할 수 있다.

- 태그의 순서 자체가 내부 구조를 만들고 있는 것을 알 수 있다.
<br>
<hr>
<br>

### ✔ Server
- 톰캣 자체를 가르키며, 서버의 인스턴스이다.
<br>
<hr>
<br>

### ✔ Service
- Engine에 Connector를 연결하는 중간 구성 요소이며, `HTTP`와 `AJP` 연결 설정이 포함된 `Catalina` 서비스가 존재한다.

- Service 태그 안에 Connector와 Engine이 존재한다.
 
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/79aa5ec3-15c6-4a2a-8437-9d70a07b9352)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/de6b02c2-5249-4c21-9373-662ee179804f)
<br>
<hr>
<br>

### ✔ Connector 
- 특정 요청들을 Engine에게 전달하는 역할, Service는 1개 이상의 Connector를 가져야한다.

- HTTP 또는 HTTPS, AJP 등 다양한 방법으로 요청을 보내면 그에 맞는 Connector들이 응답하여 Engine으로 전달하는 역할을 수행한다.

- `server.xml`의 기본 설정을 보면 설정된 `port`로 요청이 들어온 `http`를 처리한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/191d41c3-dc33-4d41-8c45-711b9aec877c)
<br>
<hr>
<br>

### ✔ Engine
- 하나의 컨테이너로 Catalina 서블릿 엔진을 나타낸다.

- HTTP 헤더를 체크하여 특정 요청이 어느 가상 Host 또는 어느 context로 연결되야 하는지 판단한다.

- 직접 설정한 context에 보면 Engine > Host 내부에서 설정이 되어 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/ebe3ed4f-30a6-48e9-bfd1-ff5c5f7da1ab)
<br>
<hr>
<br>

### ✔ Host
- 네트워크 이름을 Tomcat 서버에 연결시키기 위해 사용하며, 응용 프로그램에 대한 기본 디렉터리를 지정할 수 있다. 

- 지정된 디렉터리 안에 Context를 배치하여 실질적인 처리가 이루어지도록 연결한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/0bdf19cb-5d7d-4385-b12e-a32662dd9ac4)
<br>
<hr>
<br>

### ✔ Context
- 단일 웹 응용 프로그램을 의미한다.

- 톰캣 설정의 최하단에 위치하며, 설정된 Context Path를 기반으로 하여 지정된 Context가 실행된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/3ccb3fa7-c1c8-40b5-93fa-ee27de9a2369)
<br>
<hr>
<br>

### ✔ 정리
- 결국 톰캣의 내부 구조란 `server.xml`에서 설정된 태그 Layer에 따라 만들어지는 것이다.

- Server부터 Context에 이르기까지의 일련의 과정들이 하나로 동작한다.

- 내부 구조 == `server.xml`
<br>
<hr>
<br>

## 톰캣(tomcat) 파일 구조
- bin : 톰캣실행에 필요한 실행,종료시키는 스크립트 파일들이 위치

- conf : server.xml 및 서버 전체 설정과 관련한 톰캣 설정파일들이 위치
  - context.xml : 세션,쿠키 저장 경로 등을 지정하는 설정 파일이다.
  - server.xml : Tomcat의 주 설정 파일로 접근/접속에 관한 설정이 주를 이룬다.
  - web.xml : Tomcat의 환경설정 파일이며 서블릿, 필터, 인코딩 등을 설정할 수 있다. 
  가장 먼저 읽는 파일 DefaultServlet 지정 및 Servlet-mapping

- lib : 아파치와 같은 다른 웹서버와 톰캣을 연결해주는 바이너리 모듈들이 포함되어있고, 톰캣구동하는데<br>
필요한 (jar)라이브러리들이 위치

- logs : 톰캣실행 로그파일들이 위치

- temp : 톰캣이 실행되는 동안 임시파일이 위치

- webapps : 웹어플리케이션이 위치

- work : jsp파일을 서블릿형태로 변환한 java파일과 class파일을 저장하는 위치
