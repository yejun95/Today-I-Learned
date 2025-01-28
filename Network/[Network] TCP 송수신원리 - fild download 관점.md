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

![image](https://github.com/user-attachments/assets/929a9c79-e48b-4dc1-a887-1902dab302e4)
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
<hr>
<br>

## ✔️ 클라이언트 관점 - Decapsulation
- 클라이언트는 서버로 부터 수신받은 프레임을 분해하여 Read 하는 작업을 진행해야 한다.

![image](https://github.com/user-attachments/assets/aa029717-9e2c-47d5-9b1f-2b7bfb0205f5)
<br>

## 과정
- 서버로 부터 수신받은 프레임을 Decapsulation 진행하여 프레임에서 패킷을 꺼낸 후 안에 있는 세그먼트를 TCP 버퍼에 쌓는다.

- 만약 서버로 부터 1개의 세그먼트를 받았다면 다음 2번을 보내달라고 요청을 해야 하므로 ACK의 번호를 2번으로 하여 서버에 전송을 한다.
  - 서버는 클라이언트의 ACK가 올 때 까지 다음 세그먼트를 전송하지 않고 **❗Wait❗**한다.
  - 이 때 서버는 ACK를 기다리다가 속도 지연이 발생한다.
  - 또한 TCP의 Window Size에 여유가 없다면 서버는 새로운 세그먼트를 전송하지 않는다.
  - 즉, 서버는 수신측의 Window Size가 MSS보다 큰지 확인 후 Send를 결정한다. -> Window Size보다 MSS가 작으면 **❗Wait❗**를 한다.
<br>

- TCP 버퍼에 쌓인 세그먼트를 File 버퍼에서 Read한다.
  - 이 때 Read속도가 네트워크 속도보다 느리다면 TCP 버퍼가 가득차게 된다.
  - 서버는 TCP 버퍼가 가득차면 Window Size가 MSS 보다 작기 때문에 Send를 멈추고 **❗Wait❗**한다.
<br>
<hr>
<br>

## ✔️ 정리
![image](https://github.com/user-attachments/assets/b8879bd8-1dfc-449d-96e0-f1c388921223)
<br>

1. 서버 프로세스가 소켓에 I/O를 함

2. Driver를 통해 하드에 저장되어 있는 파일을 읽어옴

3. 할당된 메모리에 파일을 64KB로 쪼개서 읽음

4. 메모리에 쌓인 버퍼를 TCP에서  분해하여 TCP 버퍼에 Copy한다.

5. L4 -> L3 -> L2로의 Encapsulation을 통해 세그먼트를 담은 패킷이 프레임에 캡슐화된다.

6. 캡슐화된 Frame이 클라이언트 쪽으로 전송된다.

7. 서버로부터 전송받은 프레임을 Decapsulation 하여 세그먼트를 꺼낸다.

8. Decapsulation을 통해 분해된 Segment가 TCP 버퍼에 쌓인다.

9. TCP 버퍼에 쌓인 세그먼트를 Read한다.
<br>

**➡️ 키 포인트**
- 서버는 클라이언트의 ACK가 올 때 까지 다음 세그먼트를 전송하지 않고 **❗Wait❗**한다. (속도 지연 발생 가능)

- 서버는 수신측의 Window Size가 MSS보다 큰지 확인 후 Send를 결정한다. -> Window Size보다 MSS가 작으면 **❗Wait❗**를 한다.

- 네트워크 전송 속도보다 Read 속도가 느리면 TCP 버퍼에 세그먼트가 쌓이고 이로 인해 서버는 Window Size가 MSS보다 작기 때문에 Send를 멈추고 **❗Wait❗**을 한다.
<br>
<hr>
<br>

**Reference**

[널널한 개발자TV : 이해하면 인생이 바뀌는 TCP 송/수신 원리](https://www.youtube.com/watch?v=K9L9YZhEjC0)
