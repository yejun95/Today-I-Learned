## Subnet이란?
- VPC의 하위 단위로 VPC에 할당된 IP를 더 작은 단위로 분할한 개념

- 하나의 서브넷은 하나의 가용역역(AZ)안에 위치 (AWS 기준)
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/af952fe9-906d-4f6e-9c9f-b9883d4bb699)
<br>

- CIDR을 통해 IP 주소 범위를 지정할 수 있다.<br>
[CIDR이란?](https://github.com/yejun95/Today-I-Learn/blob/master/Network/CIDR.md)

- 일반적으로 첫 1개는 네트워크 어드레스, 마지막 1개는 브로드캐스트 어드레스이다.
  - 브로드캐스트(Broadcast) : 로컬 LAN에 있는 모든 네트워크 단말기에 데이터를 보내는 방식

- AWS 기준 서브넷의 IP 갯수 : 총 5개
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/a3630e9e-a9fc-410b-97a4-45cd79c315fb)
<br>
<hr>
<br>

### ✔ 서브넷의 종류
**public subnet**
- 인터넷 게이트웨이(IGW)를 통해 외부의 인터넷과 연결되어 있다.

- 안에 위치한 인스턴스에 public IP 부여 가능

- 웹서버, 애플리케이션 서버 등 유저에게 노출되어야 하는 인프라
<br>

**private subnet**
- 외부 인터넷과의 연결 경로가 없다.

- public IP 부여 불가능

- 데이터베이스, 로직 서버 등 외부에 노출 될 필요가 없는 인프라

- 💡 인터넷 연결이 필요할 때, NAT을 통해 IP 주소를 변환하여 연결이 가능하다.
  - private subnet -> router -> NAT -> Gateway
<br>
<hr>
<br>

### ✔ 라우터 테이블
- 트래픽이 어디로 가야 할지 알려주는 이정표

- VPC 생성시 기본으로 하나 제공

**예시**
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/d19dda9f-12a7-4805-9df7-fe7f9d32ec2b)
<br>

- subney이 `10.0.1.231`, `10.0.2.18`, `10.0.3.148`로 트래픽을 보내고자 할 때
해당 IP와 가장 근접한 라우터 테이블을 찾아서 Target에 보내준다.

- 해당 예시에서 3개의 IP는 `10.0.0.0/16`에도 부합하지만 `0.0.0.0/0`도 가능하다.
  - `0.0.0.0/16`은 모든 IP를 수용하겠다는 것
<br>

**예시2**
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/8142d86f-775b-46f4-af80-c2b5fa08d1b6)
<br>

- 다음과 같은 아이피 요청이 들어 왔을 경우

- 제일 근접한 라우팅 테이블인 `172.31.0.0/16`으로 확인하고 Target에 트래픽을 보내게 된다.

- `pcx-11223344556677889`는 VPC Peering 주소이다.
<br>

**예시 3**
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/90daa8b4-87af-4c08-b9d2-243453eb5afa)
<br>

- `1.23.5.123`, `8.8.8.8` IP로 요청이 들어오면 Destination 상단 2개는 부합하지 않는다.

- 그러므로 `0.0.0.0/0`으로 보내게 된다.
  - Target이 igw이기 때문에 Gateway를 통한 외부 인터넷 통신인 것을 알 수 있다.
<br>

**예시 4**
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/3fb2db06-8e85-46e9-b54f-42005cb603c4)
<br>

- 가용영역A EC2가 `10.0.1.31`로 트래픽을 보낸다.

- Route table은 Destination을 확인하여 Target에 맞게 보낸다.

- 최종 목적지는 가용영역 B에 있는 EC2

- 이처럼 서브넷 끼리도 연결이 가능하다.
<br>
<hr>
<br>

### ✔ 인터넷 게이트웨이
- VPC가 외부의 인터넷과 통신할 수 있도록 경로를 만들어주는 리소스

- 기본적으로 확장성과 고가용성이 확보되어 있다.

- IPv4, Ipv6 지원
  - IPv4의 경우 NAT 역할
 
- Route Table에서 경로 설정 후에 접근 가능
<br>

**예시**
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/abd5f9dd-b3d4-4d40-b118-708a4690db0c)

- 가용영역 A의 서브넷에서 `8.8.8.8`로 트래픽을 보낸다.

- Route table에서 해당 IP주소에 맞는 Destination을 확인한다.
  - `10.0.0.0/16`은 부합하지 않기에 `0.0.0.0/0`으로 보낸다.
 
- Target이 igw이기 때문에 게이트웨이를 통해 외부 인터넷으로 트래픽 보내기가 성공한다.
<br>
<hr>
<br>

**Reference**<br>

[AWS 강의실 : 쉽게 설명하는 AWS 기초 강좌 16:VPC 와 Subnet](https://www.youtube.com/watch?v=WY2xoIClOFA)
