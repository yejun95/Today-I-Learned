## [SpringBoot] Apache James를 이용한 메일서버 구축 - 메일 클라이언트
### ✔ 메일 서비스 확인
- 기존에 등록했던 도메인과 사용자 계정이 있는지 확인한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a2194346-a43d-4b08-a0d7-68bb034fab05)
<br>
<br>

- 진행 전 hosts 파일에 다음과 같이 넣어주고 진행
  - 파일 경로 : C:\Windows\System32\drivers\etc/hosts
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/97eda70d-9692-49cb-9c51-a8fcee4cbfbd)
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/ae4f796a-6fbe-4b07-b0ed-a4d356726da6)
<br>
<br>

- 127.0.0.1은 현재 사용 중인 PC를 의미하고, 임의로 지정한 test.com 도메인은 127.0.0.1를 가르키도록 지정했다

- 다른 PC에 제임스를 실행한 경우에는 해당 PC의 IP를 지정하면 된다.

- 네트워크 상태를 점검하는 명령어인 ping으로 test.com의 IP를 확인할 수 있다.
  -`ping test.com`
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/16a210f1-0ca3-4495-9d1d-e76c39c9ac37)
<br>
<br>

- 메일 서비스를 확인하기 위해 모질라에서 제공하는 오픈소스 이메일 클라이언트인 Thunderbird를 사용한다.
  - [다운로드](https://www.thunderbird.net/ko/)
<br>

- james 에서 설정했던 메일주소를 사용하여 입력

- 입력 후 Configure manually를 클릭하여 수동 설정 진행
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/2ef0dce5-6ca0-4be1-adbb-56104d41bb50)
<br>
<br>

- [수동으로 구성]을 선택하면, 다음 그림과 같이 SMTP와 IMAP 서버 주소를 입력하는 창에 자동으로 도메인 앞에 점(.)이 붙어 생성된다.

- SMTP와 IMAP 프로토콜 별로 서버 도메인을 등록하기 때문에 점이 붙는데,<br>
smtp.test.com, imap.test.com 같이 하위 도메인을 프로토콜별 서버 주소로 입력해야 한다.

- 또는 합쳐서 mail.test.com과 같은 도메인을 등록해서 사용한다.
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/576c1705-1ab1-4cb3-bbce-e9c97a7bf5b0)
<br>
<br>

- 여기에서는 임의의 도메인을 만들어서 사용하는 것이기 때문에 하위 도메인 없이 구현하였다.

- 따라서 점(.)을 제거하고 test.com만 서버 주소로 입력한다.

- 단, 썬더버드를 설치한 PC의 hosts 파일에 test.com을 등록해야 한다.

- 아니면 James를 설치한 서버 주소 IP를 직접 입력해서 사용해도 된다.

- 정보 입력 후 Re-test 버튼을 클릭하면 포트 번호가 자동으로 입력되고 Done을 클릭할 수 있게 된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/c481990c-6388-4b1f-95ea-234b2ba616e3)
<br>
<br>

- Done을 클릭하면 위험성을 인지한다 확인 후 넘어간다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/7d2a6f0b-56d8-4fc6-8ccb-76b4d777af03)
<br>
<hr>
<br>

### ✔ 메일 전송
- 아래와 같이 메일을 전송하였으나 error 발생으로 전송되지 못하였다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/c84be3ea-e888-4bc8-ba32-c73e5be074bf)
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a93dae6c-f890-4178-a870-6205c89a4a8c)
<br>
<br>

- gmail로 보낼 때는 가지 않았지만 네이버로 전송하니 정상적으로 메일이 도착하였다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/43aa0453-d910-4ed8-951c-ceba672eba41)
<br>
<br>
