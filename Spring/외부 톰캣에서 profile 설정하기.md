## IDE없이 tomcat에 profile 설정
- 이전에는 개발, 운영 서버를 전환하기 위해서 이클립스 환경의 `application.properties`를 수정하여 진행하였다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/2712bf47-82eb-4938-a9ca-afa9db7f1ccf)
<br>

- 이를 `application.properties` 설정 없이 외부에서 변경시켜 볼 것이다.
  - #6 이클립스 환경에서의 VM arguments 설정
<br>

- 💡 내장 톰캣으로 실행하였을 때는, dev, prod 등 각각의 포트를 지정하고 profile을 설정하였다.
  - 그러나 외장 톰캣일 경우 `server.xml`에 설정된 포트 (ex. 8080) 를 고정으로 사용하고
  profile 설정 시 `application-{profile}.properties` 파일에 따른 환경 설정을 다르게 할 수 있다.
<br>
<hr>
<br>

### ✔ setenv.bat 에서 profile 설정
- window 기준 tomcat/bin 경로에서 `setenv.bat` 파일을 vi 편집기로 생성한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/bcf067e3-51cc-48b1-b27e-eb91bf1bad6e)
<br>
<br>

- 해당 문구 기입 `set JAVA_OPTS=%JAVA_OPTS% -Dspring.profiles.active={profile}`
  - 현재 dev로 profile을 설정하였다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/5a9a2f41-1bfd-4d6d-ab16-f5ea9847ea41)
<br>
<br>

- 이후 `./catalina.bat run` 명령어를 통해 서버 실행 시 `dev`로 profile이 실행된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/f051b1bc-93b8-467f-b859-8e8b1515ddf5)
