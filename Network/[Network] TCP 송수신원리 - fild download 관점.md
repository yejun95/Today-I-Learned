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

### 과정
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

![image](https://github.com/user-attachments/assets/1a6b4be2-d175-4b63-ae43-bcf20933e975)

<br>

### 과정
- 메모리에 쌓인 버퍼가 소켓 통신을 통해 TCP에서 분해가 이루어지며, 이 때 TCP에 존재하는 버퍼에 그대로 카피된다.
  - 쪼개진 버퍼가 그대로 쌓이는 것
  - 64KB -> 64KB
<br>

- 클라이언트로의 전송을 위해 L4 -> L3 -> L2 로의 Encapsulation이 이루어진다.
  - L4 : 세그먼트 (MSS 결정)
  - L3 : 패킷 (MSS를 감싼 MTU 결정)
  - L2 : 프레임
  - 세그먼트를 패킷이 감싸고 다시 패킷을 프레임이 감싼다.
<br>

- 캡슐화된 프레임이 클라이언트 쪽으로 전송된다.
<br>

**➡️ MSS**
- Meximum Segment Size

- TCP 상에서 전송할 수 있는 사용자 데이터의 최대 크기

- MSS값은 기본적으로 설정된 MTU 값에 의해 결정된다. 

- 예를 들어 Ethernet일 경우 MTU 1500에 IP 헤더크기 20byte TCP 헤더 크기 20byte를 제외한 1460이 MSS 값이다.

- MSS = MTU – IP Header의 크기(최소 20byte) – TCP Header의 크기(최소 20byte)
<br>

**➡️ MTU**
- Meximum Transmission Unit

- 데이터링크에서 하나의 프레임 또는 패킷에 담아 운반 가능한 최대 크기

- TCP/IP 네트워크 등과 같은 패킷 또는 프레임 기반의 네트워크에서 전송될 수 있는 최대 크기의 패킷 또는 프레임

- 한 번에 전송할 수 있는 최대 전송량(Byte)인 MTU 값은 매체에 따라 달라진다.

- 예를 들어 Ethernet환경이라면 MTU 기본값은 1500, FDDI 인 경우 4000, X.25는 576, Gigabit MTU는 9000 정도 등 매체 특성에 따라 한 번에 전송량이 결정된다.

- 기본적인 MTU 1500을 초과하는 것은 "Jumbo Frame"이라고 불린다.

- Offical Maxtimun MTU 값은 65535이다.

- MTU는 Ethernet프레임을 제외한 IP datagram의 최대 크기를 의미한다. 

- 즉, MTU가 1500이라고 할 때 IP Header의 크기 20byte 와 TCP Header의 크기 20byte를 제외하면 실제 사용자 data는 최대 1460까지 하나의 패킷으로 전송될 수 있다.
<br>



**Reference**

[널널한 개발자TV : 이해하면 인생이 바뀌는 TCP 송/수신 원리](https://www.youtube.com/watch?v=K9L9YZhEjC0)
