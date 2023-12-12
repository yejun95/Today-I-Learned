## 톰캣(tomcat) 개별 xml 파일 설정
- 기존에는 `server.xml`에서 프로그램별 context 설정을 진행하였다.

- 지금은 context 설정만 하기 때문에 코드가 괜찮지만, 여러 프로그램에서 context, DB, 각종 환경 설정이<br>
많아질수록 보기가 힘들어질 것이다.

- 이 때문에 각 프로그램별 xml 파일을 만들어서 관리할 수가 있다.

- 또한 톰캣에서도 `server.xml`은 수정되었을 때, 서버가 무조건 재시작이 되어야 하므로 유지 보수성에 안좋기 때문에<br>
개별 xml 파일을 생성하여 관리하는 것을 적극 권장하고 있다.
<br>
<hr>
<br>

### ✔ 개별 xml 파일 설정 방법
- `conf/Catalina/localhost` 해당 경로에서 war 파일명과 일치한 xml 파일을 생성한다.
  - Ex) boot.war -> boot.xml로 생성
<br>

- 현재 두 가지 프로그램을 실행하고 있으므로 2개의 xml을 생성하였다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/61cef7ee-7393-44b2-8d30-5e6e05e5ce99)
<br>
<br>

- 기존에 `server.xml`에서 설정하였던 context를 주석처리한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/1c0d58bf-e013-4bf3-bd5c-f019e4c3a4a7)
<br>
<br>

- `springBoot.xml`에서 해당 설정을 그대로 적용한다.
  - ROOT.xml은 따로 설명하지 않음

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/711a2411-135c-4c19-8783-753e12f68ed9)
<br>
<br>

- 정상적으로 실행이 된다.
  - 또한 설정 변경 시 서버를 끄지 않아도 자동 reload 된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/fa206e3c-39d9-4571-9596-6caf1bd16512)
<br>
<hr>
<br>

**Reference**<br>

[빛과 소금 : tomcat context ROOT 변경하기](https://searcher.tistory.com/entry/tomcat-context-ROOT-%EB%B3%80%EA%B2%BD%ED%95%98%EA%B8%B0)
