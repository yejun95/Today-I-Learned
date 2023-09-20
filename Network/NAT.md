## NAT란 무엇인가?
- Network Address Translation

- 사설 IP가 공용 IP로 통신 할 수 있도록 주소를 변환해 주는 방법

- 라우터를 통해 네트워크 트래픽을 주고 받는 기술
<br>
<hr>
<br>

### ✔ 사용 목적
- 대개 사설 네트워크에 속한 여러 개의 호스트가 하나의 공인 IP 주소를 사용하여 인터넷에 접속하기 위함
  - IPv4주소는 한계가 있기 때문에 모든 기기에 대해 공인 IP를 사용 할 수 없다.
  - 때문에 사설망을 만들어 외부와의 연결은 1개의 IP로만 진행한다. 

- 인터넷의 공인 IP주소를 절약할 수 있다.
  - 위에서 말한바와 동일하지만 공인 IP는 한정이 되어 있으며, 고정 IP구매 시 매우 비싸다.
 
- 외부와 사설망 사이에 방화벽을 설치하여 통신망을 보호한다.
  - 라우터에 NAT 설정 시 외부에는 할당된 공인 IP만 알려지게하고, 사설 IP는 숨긴다.
<br>
<hr>
<br>

### ✔ 종류
**Dynamic NAT**
- 1개의 사설 IP를 가용 가능한 공인 IP로 연결

- 공인 IP 그룹(NAT Pool)에서 현재 사용 가능한 IP를 가져와서 연결

- 아래와 같이 사설 IP 요청이 있으면 Pool에서 사용가능한 IP를 통해 연결한다.

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/5b364ffd-1501-4d92-a621-141da26a6bf0)
<br>
<br>

**Static NAT**
- 하나의 사설 IP를 고정된 하나의 공인 IP로 연결

- AWS Internet Gateway가 사용하는 방식

- 아래와 같이 각 사설 IP에 고정된 공인 IP가 할당된다.

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/9cf148fa-8af3-460f-8a87-746f087a2a67)
<br>
<br>

**PAT**
- Port Address Translation

- 많은 사설 IP를 하나의 공인 IP로 연결

- 아래와 같이 사설 IP들이 하나의 공인 IP를 통해 연결된다.

- 가장 많이 사용되고 있는 방식

- 💡 연결 시 SRC port를 통해 port + '1', port + '2' 의 형태로 사설 IP의 구분이 이루어진다. 

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/1dabb5cc-fe05-4ca9-bddc-e46731078315)
<br>
<hr>
<br>

**Reference**<br>

[AWS강의실 : 쉽게 설명하는 AWS 기초 강좌 15:사설IP & NAT & CIDR](https://www.youtube.com/watch?v=3VXLD0-Iq8A)<br>
[개발자를 꿈꾸는 프로그래머 : NAT란?](https://jwprogramming.tistory.com/30#)
