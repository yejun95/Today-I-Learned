## VPC란 무엇인가?
- Virtual Private Cloud

- 말 그대로 가상의 사설망이며, 다른 고객과 완벽하게 논리적으로 격리된 네트워크 공간이다.

- 사용자 입맛대로 서버를 구성할 수 있다.
  - IP 주소 범위 선택
  - 서브넷 생성
  - 라우팅 테이블 및 네트워크 게이트웨이 구성 등
 <br>
 
- 나만의 개인 네트워크 망 데이터센터라고 봐도 무방하다.

- 아래와 같이 VPC라는 박스 안에서 원하는 요소를 추가하여 생성하는 것이다.

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/542afc0f-f21d-4c16-bf03-f813a7d4f212)
<br>
<hr>
<br>

### ✔ 관련 단어
- 리전 : 데이터 센터의 물리적 위치

- 가용영역 : 각 리전에서 여러 개의 IDC센터가 있는데 그 중 선택하는 것

- 인스턴스 : 서브넷 안에 생성되는 것으로 하나의 가상 컴퓨터라고 생각하면 된다.
  - 인스턴스 1개 1개가 전부 서버가 되는 것
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/819163ce-3ee4-4c67-8ea6-890b55215fec)
<br>

- [CIDR](https://github.com/yejun95/Today-I-Learn/blob/master/Network/CIDR.md)

- [Subnet](https://github.com/yejun95/Today-I-Learn/blob/master/Network/Subnet.md)
  - 라우팅 테이블, 인터넷 게이트웨이 내용 포함

- [NAT](https://github.com/yejun95/Today-I-Learn/blob/master/Network/NAT.md)
<br>
<hr>
<br>


**Reference**<br>

[끄적끄적 기억저장소 : AWS VPC란](https://memory-hub.tistory.com/11)<br>
[Inpa Dev : VPC 개념 & 사용 - 인프라구축](https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-VPC-%EC%82%AC%EC%9A%A9-%EC%84%9C%EB%B8%8C%EB%84%B7-%EC%9D%B8%ED%84%B0%EB%84%B7-%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9B%A8%EC%9D%B4-NAT-%EB%B3%B4%EC%95%88%EA%B7%B8%EB%A3%B9-NACL-Bastion-Host)
