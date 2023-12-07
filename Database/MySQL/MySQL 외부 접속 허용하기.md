## MySQL 포트 포워딩을 통한 외부 접속 허용하기
### ✔ 외부 접속 허용 아이디 생성 및 권한 부여
- mysql 콘솔창 실행
  - Ex) `MySQL 8.0 Command Line Client
<br>

- root 비번 입력 후 로그인 처리

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/8d548e64-0e60-4849-bef2-2b42da4fba87)
<br>
<br>

- 외부 접속을 허용할 유저를 만든다.
  - 사용자명 : MySQL 서버에 로그인할 사용자의 이름을 정의
  - 호스트 : 접속을 허용할 호스트 정의
  - 비밀번호 : MySQL 서버에 로그인할 사용자의 비밀번호 정의
<br>

```
<!-- 유저 생성 기본 구문 -->
CREATE USER '사용자명'@'호스트' IDENTIFIED BY '비밀번호';
```
<br>

- 각각 'level' / '1234' 라는 이름과 비밀번호를 가진 유저 생성
  - 호스트의 '*' 표시는 모든 곳에서 접속을 허용한다는 의미
  - 특정 아이피만 접속을 허용 하고 싶을 경우, 호스트 부분에 특정 아이피를 적어주면 된다.
<br>

```
CREATE USER 'level'@'*' IDENTIFIED BY '1234';
```
<br>

- 외부 접속한 사용자의 권한을 설정한다.
  - GRANT ALL PRIVILEGES : 데이터베이스의 모든 권한 부여
  - 데이터베이스명 : 권한을 부여할 데이터베이스명
  - 사용자명 : 권한을 부여받을 사용자명
<br>

```
<!-- DB 권한 설정 기본 구문 -->
GRANT ALL PRIVILEGES ON 데이터베이스명 TO 사용자명;
```

- 'level' 사용자에게 'my_db'라는 데이터베이스에 모든 권한주기

```
GRANT ALL PRIVILEGES ON my_db.* TO 'level'@'%' IDENTIFIED BY '1234';
```
<br>

- 'level' 사용자에게 모든 데이터베이스에 대한 권한 주기

```
GRANT ALL PRIVILEGES ON *.* TO 'level'@'%' IDENTIFIED BY '1234';
```
<br>

- 변경된 권한 설정을 적용하기 위해 다음 명령어를 실행

```
FLUSH PRIVILEGES;
```
<br>
<hr>
<br>

### ✔ 내부 포트 열기
- 방화벽 > 고급 설정
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/74901e6b-56da-48fc-90b0-0408c2952099)
<br>
<br>

- 좌측 인바운드 규칙 클릭 > 우측 새 규칙 클릭
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/3079e86f-46fe-4795-a918-03d5f8c7a0f7)
<br>
<br>

- 프로그램 선택
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/e45a9783-d21a-4dde-8d8d-b36e5d132bee)
<br>
<br>

- 모든 프로그램 선택
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/9bd03141-54b7-438e-976f-7517b037eacf)
<br>
<br>

- 연결 허용
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/55c6bcc5-2e47-4e49-850d-85b1187654d5)
<br>
<br>

- 규칙 적용 시기 모두 체크
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/309bd178-db61-4e7a-ac1a-0fb1be58fb98)
<br>
<br>

- 식별 가능한 이름과 설명 입력
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/6a994c60-fb06-4337-b117-0b8f97852a7b)
<br>
<hr>
<br>

### ✔ 포트 포워딩
- ipconfig에서 기본게이트웨이 주소를 브라우저에 입력하여 공유기 진입
 - 제조사별 ID/PW 상이 (본인은 TP-LINK 사용)
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/88be7391-6770-4400-8da8-2284ede8dec6)
<br>
<br>

- 3306 포트에 대하여 포트 포워딩 설정
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/5d6603cd-8586-4604-a599-e37070cb7539)
<br>
<hr>
<br>

### ✔ 외부에서 MySQL DB 접속하기
- 노트북을 핸드폰 핫스팟으로 인터넷을 연결하여 접속해본다.

- 서버가 되는 PC의 IP와 미리 만들어 놓았던 ID/PW를 입력하여 접속한다.
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/14750f9f-ba29-4403-898a-c41926033104)
<br>

- 정상 접속 성공!
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/07b0ee4a-05bd-4d5b-a440-0c8891469b30)

