## docker란 무엇인가?

- 어플리케이션을 패키징 할 수 있는 tool

- 컨테이너 기술을 기반으로 한 일종의 가상화 플랫폼
  - 컨테이너 : 컨테이너가 실행되고 있는 호스트 os의 기능을 그대로 사용하면서 프로세스를 격리해 독립된 환경을 만드는 기술
  - 가상화 : 물리적 자원인 하드웨어를 효율적으로 활용하기 위해서 하드웨어 공간 위에 가상의 머신을 만드는 기술

- 한 컴퓨터 안에서 여러개의 시스템과 환경 설정 등이 충돌되지 않고 동시에 사용 할 수 있도록 한다.

- 독립된 환경을 만들어서 하드웨어를 효율적으로 활용하는 기술

- 컨테이너라고 불리는 하나의 작은 소프트웨어 유닛안에 우리의 어플리케이션과<br>
그에 필요한 시스템 툴, 환경설정, 모든 디펜던시를 하나에 묶어서<br>
다른서버, 다른피씨 그 어떤곳에도 쉽게 배포하고 안정적으로 구동할 수 있게 도와주는 툴이다.

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/75a5d333-39ac-4ea6-9c17-5303b5698fd7)
<br>
<br>
- 어플리케이션과 어플리케이션이 구동하는데 필요한 모든 것들을 도커 컨테이너에 담아 저장

- 어플리케이션을 구동하고 싶은 서버에 해당 도커 컨테이너를 다운받는다면, 어떤 PC에서도 동일하게 구동할 수 있다.


![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/3e65841b-95d0-46f7-ab1a-69ae70b52b9b)
<br>
<hr>
<br>

### ✔ 등장 배경
- 기존 로컬에서 개발해 실 서버 배포 시 소스파일만 올리는 것으로는 문제 발생

- ex) js소스 파일을 돌리기 위해서는 node.js, npm, 각종 Dependencies등 설정을 해야함

- 개발자의 PC마다 환경 설정이 다르기 때문에 오류의 발생 원인이 된다.
<br>

내 서버에 node.js가 있고 서버에도 node.js가 있으니 내 서버에서 개발한 js 파일을 서버에 배포하면 자동으로 동작하겠지?<br>

-> 배포 -> 에러가 발생!! -> 내 PC에선 잘되는데 서버에선 왜 안돼!!! -> **node.js의 버전이 맞지 않기 때문에 발생**
<br>


![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/2d5794c8-cc79-42ca-9721-cf7b18ef4d50)
<br>
<hr>
<br>

### ✔ VM이랑 똑같은 것 아닌가?
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/89d3d8a2-aa54-4111-9878-8dbce5c40725)
<br>

**Virtual Machine**
- 기존의 가상화 기술로, 하이퍼바이저(Hypervisor)를 이용해 여러개의 운영체제를 하나의 호스트에서 생성해서 사용하는 방식
  - ex) vmware, VirtualBox
 
- 각각에 VM마다 OS가 올라가기 때문에 배포를 위한 이미지의 크기가 커지게 된다.
<br>

💡 정리 
- 가상머신은 Hypervisor를 통해 여러 개의 운영체제를 생성하고 관리 (Guest OS)

- 시스템 자원을 가상화하고 독립된 공간을 생성하는 작업은 HyperVisor를 거치므로 -> 성능 손실이 큼 ↑

- 가상머신은 Guest OS를 사용하기 위한 라이브러리, 커널 등을 포함하므로 -> 배포할 때 용량이 큼 ↑
<br>
<br>
<br>

**docker**
- 하드웨어에 설치된 운영체제에 Container Engine이라는 소프트웨어를 설치해 개별적인 Container를 만들어 각각의 어플리케이션을 고립된 환경에서 구동할 수 있게 해준다.

- 여기서 가장 많이 사용되는 Container Engine이 바로 Docker! VM의 경량화 버전이라고 생각하면 됨.
<br>

💡 정리
- 도커 컨테이너는 가상화된 공간을 생성할 때 리눅스 자체 기능을 사용하여 프로세스 단위의 격리 환경을 만드므로 -> 성능 손실 없음 無

- 가상머신과 달리 커널을 공유해서 사용하므로, 컨테이너에는 라이브러리 및 실행파일만 있으므로 -> 용량이 작음 ↓

- 위의 이유로 -> 컨테이너를 이미지로 만들었을 때, 배포하는 시간이 가상 머신에 비해 빠르며 ↑, 사용할 때의 성능 손실 또한 거의 없음 ↓
<br>
<hr>
<br>

### ✔ docker 준비물
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/4190e477-ad6a-49fe-954e-ba1b0a7e79fa)
<br>

**dockerfile** 
- 어플리케이션을 구동하기 위한 파일은 무엇이 있는가?

- 어떤 dependencies를 다운받아야 하는가?

- 필요한 환경변수

- 어떻게 구동해야하는지에 대한 script
<br>

**Image**
- 어플리케이션을 실행하는데 필요한 코드, 런타임, 환경, 시스템 툴, 시스템 라이브러리등이 포함

- 실행되고 있는 어플리케이션의 상태를 찰칵- 해서 이미지로 만들어둔다고 생각하면 된다!

- 💡 !!!!! 한 번 만들어지면 변경이 불가능 !!!!!
<br>

**container**
- Image를 고립된 환경에서 개별적인 시스템 안에서 실행할 수 있는 공간

- container 안에서 image를 이용해 우리의 어플리케이션이 구동한다.
<br>
<hr>
<br>

### ✔ 어떻게 Container에 배포가 가능한가? (어떻게 Image를 공유 하는가?)
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/2b8fe719-a987-4868-960b-cc58edbdcd54)
<br>

1. 내 로컬에서 만든 이미지를 Container Registry에 Push

2. 서버는 Container Registry에서 이미지를 Pull 로 당겨와서 사용한다.

3. 서버에는 Docker 설치 필수!
<br>
<hr>
<br>

### ✔ docker의 종류
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/184075dc-69f3-4d49-abee-e923ef24274f)
<br>

- public, private

- 개인 사용자의 경우 dockerhub를 주요 사용

- 회사에서는 private한 docker를 사용하는데, aws나 google Cloud등에서 docker 서비스를 제공하고 있다.
<br>
<hr>
<br>

### ✔ 총정리
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/11e6ab7b-eaeb-4857-bd1e-30caf874bb51)
<br>

1. 사용자는 Local에 Docker를 설치한 후 Dockerfile을 만든다.

2. Dockerfile을 build 시켜 Image로 만든다.

3. 만든 Image를 Container Registry에 Push

4. 실서버에 Docker를 설치 한 후 Container Registry에서 Image를 Pull 한다.

5. Docker에서 Image를 run 한다.
<br>
<hr>
<br>

**Reference**<br>

[youtube 드림코딩 : 도커 한방에 정리](https://www.youtube.com/watch?v=LXJhA3VWXFA&t=836s)<br>
[SH's Devlog : 도커란? - 도커 개념 정리](https://seosh817.tistory.com/345)<br>
[디토20 : 도커란 무엇인가? 도커 한방 정리!](https://be-developer.tistory.com/18)<br>
