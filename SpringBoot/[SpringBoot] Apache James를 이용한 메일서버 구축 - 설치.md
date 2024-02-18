- [[SpringBoot] Apache James를 이용한 메일서버 구축 - 설정]()
- [[SpringBoot] Apache James를 이용한 메일서버 구축 - 메일 클라이언트]()
<br>
<hr>
<br>

## 1. Apache James를 이용한 메일서버 구축 - 설치
- Apache James는 Java로 만든 메일 서버로 오픈 소스로 되어있다.

- 안정적인 기능과 Apache의 지원을 받는 메일 서버기 때문에 많이 애용된다.
<br>
<hr>
<br>

### ✔ Apache James 설치
- [사이트](https://james.apache.org/download.cgi#Apache_James_Server)를 방문하여 설치 파일을 다운 받는다.
  - 3.8.0 버전에서 Binary 파일을 받는다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/4b0c331d-e51d-4a06-a5c1-cc6c19953293)
<br>
<br>

- 압축을 푼 후 폴더를 열어보면 아래와 같은 형태가 나온다.
  - bin : James를 실행하는 파일
  - conf : James 설정 파일
  - lib : James 실행에 필요한 Java 라이브러리
  - log : 실행 결과에 대한 다양한 정보를 제공하는 로그 파일
  - var : activemq, 메일 오류 파일 등이 저장
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/7df60391-ea6d-4379-a8ba-f75ad4270456)
<br>
<br>

- conf 폴더에는 20 여개의 설정 파일들이 있다.

- James는 다양한 기능을 제공하는데, 이 설정 파일들을 이용하여 조절할 수 있다.
  - [설정 파일 설명 사이트](https://james.apache.org/server/config.html)
<br>

- 하지만 지금은 필요한 xml 파일만 수정하여 설정을 이어나갈 것이다.
<br>
<hr>
<br>

### ✔ Apache James를 실행하기위한 설정
**domainlist.xml**
- 사용할 도메인 설정
  - test.com 으로 설정한다.
<br>

- 이 도메인은 실제 등록된 도메인을 지정해도 되고, 테스트를 위해서 여기서는 test.com을 사용한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/e76e3478-01f2-40f2-af33-92cb34e1b6ef)
<br>
<br>

- 이 상태에서 서버를 바로 실행해도 되지만 `mailetcontainer.xml`에서 우편 관리자 계정에 대한 도메인을
위와 같이 동일하게 지정한다.
  - 우편 관리자 계정은 반송 메일 등에 사용된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/97af0f79-62de-4214-abc9-063bb460a34d)
<br>
<br>

- James는 Java로 제작되었기 때문에 다운로드 받은 zip 파일 하나로 윈도우와 리눅스에서 실행할 수 있다.

- 다만, James가 여러 가지 이유로 갑자기 종료 되었거나 멈춰진 상태로 있을 경우, 자동으로 감지하여 재시작하기 위해[Wrapper를](https://www.tanukisoftware.com/en/wrapper.php) 사용하여 제작되었다.
  - 기업에서 메일 서버는 중지되면 안되기 때문이다.
  - taunki의 Wrapper가 JVM 모니터링 및 restart 기능을 가지고 있기 때문에 사용한다.
  - [Wrapper 한글 설명](https://m.blog.naver.com/shin7688/120098447977)
<br>

- 이 Wrapper가 윈도우에서는 윈도우 서비스로 등록해서 사용하고, 리눅스에서는 쉘(Shell)로 백그라운드에서 실행한다.
<br>
<hr>
<br>

### ✔ Apache James 실행
- 윈도우 시작 메뉴를 눌러 cmd 를 입력해서 콘솔(cmd) 창을 관리자 권한으로 실행한다.

- 리눅스도 root 권한으로 실행해야 한다.

- 관리자 권한으로 실행하는 이유 : 1024보다 작은 포트는 관리자 권한으로만 실행 할 수 있다.

- 메일 서버의 포트
  - STMP : 25, 587
  - IMAP : 143, 993
<br>

- 콘솔 창에서 James 압축을 해제한 폴더 중 bin 폴더로 이동하여 `james.bat install` 를 실행한다.
  - 이것은 James를 윈도우 서비스로 등록하는 명령어로, 다음과 같이 서비스가 등록된다.
  - 윈도우 버튼 -> 서비스를 검색하면 열 수 있는 창임.
  - `james.bat remove`로 제거 할 수 있다.
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/65d71c78-c0a0-4d36-9e41-a7682eb07b3f)
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a7daae75-52d6-4931-9c47-35eb194aa0bd)
<br>
<br>

- 이후 윈도우 서비스 창에서 실행 버튼을 눌러 실행해도 되고, 콘솔 창에서 `james.bat start`로 실행해도 된다.

- 에러 발생
  - `wrapper  | The Apache James :: Server :: Spring :: App service was launched, but failed to start.`
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/76784215-b843-4902-b0ba-b44afc72e6fd)
<br>
<hr>
<br>

### ✔ wrapper  | The Apache James :: Server :: Spring :: App service was launched, but failed to start.
- log 파일을 확인하여 상세한 에러를 확인한다.

```
STATUS | wrapper  | 2024/01/21 23:14:26 | Starting the Apache James :: Server :: Spring :: App service...
STATUS | wrapper  | 2024/01/21 23:14:26 | --> Wrapper Started as Service
STATUS | wrapper  | 2024/01/21 23:14:26 | Launching a JVM...
INFO   | jvm 1    | 2024/01/21 23:14:27 | Wrapper (Version 3.2.3) http://wrapper.tanukisoftware.org
INFO   | jvm 1    | 2024/01/21 23:14:27 |   Copyright 1999-2006 Tanuki Software, Inc.  All Rights Reserved.
INFO   | jvm 1    | 2024/01/21 23:14:27 | 
INFO   | jvm 1    | 2024/01/21 23:14:27 | WrapperSimpleApp: Unable to locate the class org.apache.james.app.spring.JamesAppSpringMain: java.lang.UnsupportedClassVersionError: org/apache/james/app/spring/JamesAppSpringMain has been compiled by a more recent version of the Java Runtime (class file version 55.0), this version of the Java Runtime only recognizes class file versions up to 52.0
INFO   | jvm 1    | 2024/01/21 23:14:27 | 
INFO   | jvm 1    | 2024/01/21 23:14:27 | WrapperSimpleApp Usage:
INFO   | jvm 1    | 2024/01/21 23:14:27 |   java org.tanukisoftware.wrapper.WrapperSimpleApp {app_class} [app_arguments]
INFO   | jvm 1    | 2024/01/21 23:14:27 | 
INFO   | jvm 1    | 2024/01/21 23:14:27 | Where:
INFO   | jvm 1    | 2024/01/21 23:14:27 |   app_class:      The fully qualified class name of the application to run.
INFO   | jvm 1    | 2024/01/21 23:14:27 |   app_arguments:  The arguments that would normally be passed to the
INFO   | jvm 1    | 2024/01/21 23:14:27 |                   application.
STATUS | wrapper  | 2024/01/21 23:14:30 | <-- Wrapper Stopped
ERROR  | wrapper  | 2024/01/21 23:14:31 | The Apache James :: Server :: Spring :: App service was launched, but failed to start.
```

- 결정적인 에러 사항은 아래 부분이다.
```
org/apache/james/app/spring/JamesAppSpringMain has been compiled by a more recent version of the Java Runtime (class file version 55.0), this version of the Java Runtime only recognizes class file versions up to 52.0
```
<br>
<br>

- 이 에러는 컴파일된 클래스 파일이 Java 11 (class file version 55.0) 이상의 버전으로 컴파일되었는데, 현재 실행 중인 Java Runtime이 이를 지원하지 않는다는 것을 의미한다.

- 나의 자바 버전 확인
  - 1.8이기 때문에 11이상부터 지원되는 Apache James를 실행할 수 없는것이다.
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/27345fe9-3edc-4803-b680-11ec22527fe6)
<br>
<br>

- Apache James를 다운그레이드하여 1.8이 가능한 버전으로 다시 진행한다.

- 공식 홈페이지를 참조해보면 3.5x 버전까지는 자바 1.8이 가능하고, 3.6x 버전 부터는 자바 11이상부터 가능하다.
  - 때문에 [3.5x버전](https://archive.apache.org/dist/james/server/)으로 설치를 진행한다.
  - [공식문서](https://james.apache.org/james/update/2020/07/16/james-3.5.0.html)
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/6ff83705-1c42-4fd9-8342-4d9bc2b09f19)
<br>
<hr>
<br>

### ✔ Apache James 재실행
- 3.5버전으로 재실행한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/dfa75324-87d1-4e24-8417-36695c436ec0)
<br>
<br>

- 포트를 확인하여 실행되는지 체크가 가능하다.
  - `netstat -ano`
  - TCP에 25와 143포트가 있다면 정상적으로 실행중인 것
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/0a4c515e-4d91-4ab3-9c2a-e0b822c0ce10)
<br>
<br>

### ✔ 등록 도메인 확인
- `james-cli.bat listdomains` 명령어를 통해 등록된 도메인 확인이 가능하다.

- 내가 등록한 `test.com`이 제대로 등록되있는 것을 볼 수 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/e213c618-3fb6-4bec-9c5a-147f808868ad)
<br>
<br>

- 💡 명령어를 통해 도메인 등록 가능
  - 윈도우: james-cli.bat adddomain test.com
  - 리눅스: ./james-cli.sh AddDomain test.com
<br>

- 하지만 현재 `domainlist.xml`에서 등록을 했기 때문에 따로 명령어로 등록을 하지 않은 것이다.
<br>
<hr>
<br>

### ✔ 계정 등록
- 다음 명령어를 통해 메일 사용자(계정@도메인)을 생성하고 ListUsers로 확인한다.
  - `  james-cli AddUser xodlf@test.com test1234`
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/40b35e06-308d-4ded-b2da-f228cc545712)
<br>
<hr>
<br>

**Reference**<br>

[SW 개발이 좋은 사람 1. Apache James 메일 서버 - 설치](https://forest71.tistory.com/)
