## Kernel 이란?
- 컴퓨터라는 국가의 법은 Kernel로 구현이 된다.
  - H/W와 사용자 영역 중간에 위치하여 제어를 한다.

- 핵심 기능
  - I/O 입출력 제어
  - 자원 관리 (CPU + Memory)
  - 접근 통제
<br>

- Kernel 영역과 사용자 영역은 완전히 다른 세상이다.
  - 인간계와 신계로 비교할 만큼 두 영역의 경계선은 뚜렷하다.
<br>

- S/W는 OS와 App으로 나뉘게 된다.
  - 계층 구조에 의해서 App(사용자영역)은 OS(Kernel 영역)에 의존적이지만<br>
  App을 만들기 위해 OS를 설치한다는 관점에서도 이해해야한다.
<br>
<hr>
<br>

### ✔ 계층별 기능
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/2206ed07-511d-4ba7-a51b-aa7f92ac3b8f)
<br>

**H/W**
- Physical 영역이다.

- CPU와 같은 하드웨어가 존재하는 공간이다.

- 주변기기와 같은 Device도 존재한다.
<br>

**S/W**
- 커널과 유저가 존재하는 공간이다.

- Kernel mode는 Logical이라고 부르며 이는 Virtual과 동일한 개념이다.
  - 즉, 가상화라는 것은 H/W를 S/W를 통해 만드는 것이다.
<br>

- H/W 계층에 의존적이기 때문에 하드웨어 없이는 존재할 수 없다.
  - 당연하게도 컴퓨터 부품이 없으면 실행 조차 불가능하기 때문이다.
<br>

- Kernel mode는 `OS`가 존재하는 곳으로 보통 CPU와 OS를 통해 platform이라는 단어가 등장한다.
  - ex) CPU 64비트, OS 64비트  ->  64비트 platform 이라고 한다.
<br>

- H/W Device를 움직이기 위한 전용 소프트웨어가 존재하며, 이를 'Driver'라고 부른다.
  - 이는 kernel의 어떠한 요소를 위한 부분이다.
<br>

- 컴퓨터 세계에서 User는 Process(task)의 형태로 존재한다.
  - 동시에 여러 개가 존재한다.  ->  멀티 태스킹 환경
<br>
<hr>
<br>

### ✔ Kernel의 상호 작용
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/8ac74771-7953-4ed8-963d-0d022fe1e2c2)
<br>

- 만약 모니터에 무언가를 출력하려고 할 때  ->  kernel은 모니터 화면을 제어해야한다.
  - H/W : Device는 video card가 될 것이고, HDMI를 통해 모니터와 연결한다.
  - kernel : H/W에서 사용하는 것이 video card일 때, kernel의 요소는 G.E (Graphic Engine)이 된다.
  - user : 무엇을 출력하는지를 담은 어떠한 매개체를 kernel과 주고 받는다. (일방향 or 양방향)
<br>

**user와 kernel의 매개체**
- user와 kernel의 경계는 인간계와 신계만큼 경계선이 뚜렷하다.
  - 이 때문에 어떠한 매개체를 이용하여 정보를 주고 받아야한다.
<br>

- 이러한 인터페이스의 역할을 하는것이 **'file'**이다.
  - Interface : 상호 연결을 해주는 것
  - 즉, **file의 형태로** 인터페이스 역할을 통해 user와 kernel을 이어주는 것이다.
<br>

- 일반 mp3, 영상 파일이 아니고 'Device File' 이라고 부른다.
  - **kernel의 구성요소에 대한 추상화된 인터페이스를 user에 제공할 떄 파일의 형태를 띈다.**

- user는 file에 대한 I/O를 진행하는데, 셋 중에 하나  ->  RWX (Read, Write, Excute) 를 한다.
  - read는 kernel로 부터 읽어 오는 것
  - write는 kernel로 보내는 것
<br>

- 입출력 행위의 주체인 Process가 인터페이스(file)에 대한 접근 권한을 얻어야 한다.
  - 해당 권한은 OS가 부여
  - OS가 허락해준 file에 대해서만 주체가 무언가를 할 수가 있다.
  - 이 것이 kernel의 핵심 기능 중 하나인 접근 통제에 해당된다.
<br>

**User가 Hello World 화면에 출력할 때**
1. Process 1번에서 write를 해서 정보를 Kernel로 내려보낸다.
2. kernel의 요소가 해당 내용을 처리하고 장치에 전달한다. -> Driver -> H/W Device
3. H/W에서 video card를 통해 화면에 출력한다.
* 단반향 전송이기 때문에 정보가 올라올 것이 없다.

- 이때 프로세스 2번이 나도 똑같은 실행 할래. 라고 하게 되고 해당 실행이 프로세스 1개 씩만<br>
진행이 가능하다면 OS가 너 기다려. 하고 통제를 한다.
<br>
<hr>
<br>

### ✔ kernel의 필터
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/339d9f8d-041e-4924-9f37-73def98a04e8)
<br>

- H/W와 S/W가 정보를 주고 받을 때, 인터페이스라는 device file의 매개체로 진행된다.

- 이때 매개체를 받는 요소는 File System이다.

- 이후 user에서 정보가 내려올 때, 필터링을 거치게 된다.
  - I/O 모니터링을 통해 감시  ->  실시간 감시 엔진
  - 보통 filter를 통해 user쪽 Anti Virus 프로그램에 검사를 의뢰한다.  ->  ex) V3, 백신 프로그램 등
  - 검사 후 실행가능한 안전한 프로그램이면 필터를 통과하여 driver -> H/W로 내려간다.
<br>
<hr>
<br>

**Reference**<br>

[널널한 개발자 : 넓고 얕게 컴공 전공자 되기](https://www.inflearn.com/course/%EB%84%93%EA%B3%A0%EC%96%95%EA%B2%8C-%EC%BB%B4%EA%B3%B5-%EC%A0%84%EA%B3%B5%EC%9E%90/dashboard)
