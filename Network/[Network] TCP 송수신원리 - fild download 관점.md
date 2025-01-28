# TCP 송/수신 원리의 이해 - file download 관점
### 전제조건
- TCP/IP 연결을 통해 파일을 1개 다운로드 한다는 가정으로 설명을 진행한다.

- 클라이언트에서 서버에 파일 다운로드를 요청

![image](https://github.com/user-attachments/assets/42469673-5dd4-4b37-9d38-5d09a9e3ba16)
<br>
<hr>
<br>

## ✔️ 서버 관점 - 다운로드 할 파일을 메모리에 적재
### 전제조건
- 파일의 크기는 1.4MB

- 메모리에 할당된 크기는 64KB

![image](https://github.com/user-attachments/assets/655ab30c-df0d-4340-85ab-53f9fbe42aae)
<br>

- 서버 프로세스가 파일 입출력을 위해 소켓에 I/O를 한다.

- 커널 영역에서 Driver통해 HDD에 저장되어 있는 파일을 메모리로 읽는다.
  - 이 때 읽을 수 있는 메모리에 할당 크기는 프로그램마다 상이하다.
  - 파일의 크기는 항상 상이하기 때문에 메모리 사이즈는 파일의 사이즈보다 작게 설정하고 쪼개서 읽어야 한다.
<br>

- 할당된 메모리 크기 만큼 파일을 Read한다.
<br>
<hr>
<br>

## ✔️ 서버 관점 - Encapsulation
- 파일이 메모리에 적재 됐다면 TCP/IP 통신을 통해 클라이언트로 전송하기 위한 일련의 과정이 필요하다.

