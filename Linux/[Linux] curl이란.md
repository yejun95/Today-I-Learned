## curl 이란?
- Client Url 이란 의미로 클라이언트에서 url을 사용해서 서버와 데이터를 송수신하는 명령어이다.

- 클라이언트에서 커맨드 라인이나 소스코드로 손 쉽게 웹 브라우저 처럼 활동할 수 있도록 해주는 기술

- Linux, MacOS, Window 등 다양한 환경에서 HTTP, HTTPS, SMTP, TELNET, FTP, LDAP 등 다양한 프로토콜 지원

- url을 가지고 할 수 있는 것들은 전부 다 가능하다.
  - HTTP 프로토콜 이용해 웹 페이지의 소스를 가져온다거나 파일 다운 가능
  - FTP 프로토콜 이용해 파일 받기 or 올리기 가능
  - SMTP 프로토콜 이용해 메일 전송 가능
<br>
<hr>
<br>

### ✔ 사용법
```
curl [options] [url]
```
- curl의 option은 short 형인 "-"와 long 형인 "--"를 제공한다.
<br>

| short | long | 설명 |
|---|---|---|
| -k | --insecure | https URL 접속 시 SSL 인증서 검사 없이 연결 |
| -i | --head | HTTP 응답 헤더를 표시 |
| -d | --data | POST 요청이나 JSON 방식과 같이 reqeust body에 데이터를 담을 때 사용 |
| -o | --output | -o [파일명] 을 사용하면 출력 결과를 파일로 저장 |
| -O | --remote-name | 파일 저장 시 remote의 file 이름으로 저장 |
| -s | --silent | 진행 내역이나 메시지 등을 출력하지 않음 |
| -X | --request | Request에 사용할 메서드(GET, POST, PUT 등)를 지정 |
| -v | --verbose | 동작하는 과정을 출력 |
| -A | --user-agent | 특정 브라우저인 것처럼 동작하기위한 설정 |
| -H | --header | 요청할 헤더 설정 |
| -L | --location | 서버에서 HTTP (301, 302 -리다이렉트) 응답이 오면 리다이렉트 URL로 따라감 (--max-redirs 횟수)로 지정 가능  |
| -D | --dump-header<file> | 파일에 응답 헤더를 기록 |
| -u | --user | 사용자 아이디 / 비밀번호 입력 |
| -f | --fail | 오류 발생 시 출력 없이 실패 |
| -T | --upload-file | 로컬 파일을 서버로 전송 |
| -C | --continue-at | 중지된 다운로드를 재시작 |
| -J | --remote-header-name | 응답 헤더에 있는 파일 이름으로 파일 저장 (curl 7.20 이상) |
| -I | --head | 응답 헤더만 출력 |
<br>
<hr>
<br>

### ✔ 사용 예제
- url 요청에 대한 응답 값 출력

```
curl https://www.naver.com
```

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/b120d686-e228-4ac7-b656-c2349339c7fa)
<br>

- url 요청에 대한 응답을 dummy.txt라는 파일에 저장

```
curl -o dummy.txt naver.com
```
> 명령어를 입력한 경로에 dummy.txt 파일이 생성됨

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/ca621449-de84-4fbd-81b4-af39b4cca21b)

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/2d5736a9-1795-468c-97b0-8938e5a2ae3b)
<br>
<hr>
<br>

**Reference**<br>

[제육's 휘발성 코딩 : [Linux] curl 명령어 사용법 정리 (HTTP, FTP, SMTP 등)](https://sasca37.tistory.com/279)<br>
[닥치고개돌 : CURL 이란? CURL사용법](https://shutcoding.tistory.com/entry/CURL-%EC%9D%B4%EB%9E%80-CURL%EC%82%AC%EC%9A%A9%EB%B2%95)
