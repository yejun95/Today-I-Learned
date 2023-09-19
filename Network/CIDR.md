## CIDR
- Classless Inter-Domain Routing 으로 클래스 없는 도메인간 라우팅 기법
<br>
<hr>
<br>

### ✔ 사전 지식
- IPv4 는 총 32비트의 숫자로 구성되어있다.(4,294,967,296개)
  - 588,514,304 개는 특정한 목적으로 선점되어 있다.
  - 따라서 가용한 IP 는 3,706,452,992개이다.
  - 이미 충분하지 않다는 것을 알 수 있는데 이를 해결하기 위해 사설 네트워크(Private Network) 를 사용한다.
 
- 사설망
  - 하나의 Public IP 를 여러 기기가 공유할 수 있는 방법
  - 하나의 망에는 Private ip 를 부여받은 기기들과 gateway로 구성
  - 각 기기는 인터넷과 통신시 Gateway를 통해 통신
  - private IP는 지정된 대역의 IP만 사용가능

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/3c8e9b03-aae8-463f-8a00-f58fc47909b6)
<br>

- 즉 해당 범위의 IP 를 부여받은 다면 이는 무조건 사설망이다. (ex. 내 IP 를 확인해봤는데 10.0.0.1이다. => 사설망이다)

**통신 방법**
- 192.168.0.2 가 61.123.44.1 에 통신을 하고 싶다면 NAT에 기록을 하고 통신을 하는 것이다.
  - [NAT Gateway란 무엇인가?](https://github.com/yejun95/Today-I-Learn/tree/master/Network)

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/14fd0b49-29e8-42e7-bfaf-9791c0b4a9f6)
<br>
<hr>
<br>

### ✔ CIDR 이란?
- 주소의 영역을 여러 네트워크 영역으로 나누기 위해 IP를 묶는 방식, 즉 IP 주소 범위를 정의하는 방식

- 여러개의 사설망을 구축하기 위해 망을 나누는 방법
<br>

**CIDR 표기법**

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/1171c38c-39cc-4be1-8f23-3a6d4067a653)
<br>

- CIDR를 정의하는 설명중에, "cidr은 네트워크 정보를 여러개로 나누어진 Sub-Network들을 모두 나타낼 수 있는<br>
하나의 Network로 통합해서 보여주는 방법이다." 라고 하는데<br>
이 말은 아이피와 서브넷 마스크를 아래와 같이 표기한 것을 말한다.

- 10.88.135.144/28

- 단 한줄만으로 네트워크 범위를 추측 또는 측정 할 수 있다.

- /28이니 4비트만 사용가능 하므로 15번까지 가능 (144 ~ 159)

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/419ab423-5d7a-40fe-a6b7-95f547542d78)
<br>

- 여기서 10.88.135.144/28 의 의미는 아래와 같다. 즉 10.88.135.144 는 사설망의 범위의 시작점을 얘기한다.
  - 따라서 10.88.135.144 부터 10.88.135.159 까지 의 대역을 의미하는 것이다
  - 즉, /28 이 커지면 커질 수록 해당 사설망에서 사용할 수 있는 호스트 수는 작아짐
 
- 첫번째/ 마지막 IP 는 예약이 되어있기 때문에 사용이 불가능하다.
  - 첫번째 IP 는 네트워크 자체를 가르키는 IP
  - 마지막 IP 는 Broadcast IP
  - 따라서 사용할 수 있는 주소는 총 가용한 사설망 갯수 -2 를 해야함
  - 예를 들어 10.0.0.0/28 에서 사용할 수 있는 사설망 갯수는? 이라는 문제가 나오면 마지막 4비트를 사용할 수 있으니<br>
  2^4 해서 16개가 아닌 IP 네트워크 자체를 가르키는 10.0.0.0 을 가르키는 IP 와 10.0.0.15 를 가르키는 IP는 빼야해서 14개가 된다
<br>

- 아래 그림을 참고해서 다시 한번 설명하자면

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/406911a4-fa94-4a3f-84d9-4fc63d4d0ce5)
<br>

- 왼쪽 상단의 네트워크 CIDR notation을 보면 192.168.0.0/16이다.

- 그러면 /16이므로 앞에 16비트 만큼은 Network Address 이기 때문에 0.0 부분만 host로 사용할 수 있다.

- 이 중 네트워크 자체를 가르키는 192.168.0.0과 boradcast를 나타내는 192.168.255.255 제외하면<br>
총 65,534 개를 사설망 host로 사용할 수 있다는 것이다.
<br>
<hr>
<br>

**Reference**<br>

[알고풀자 : CIDR이란?](https://algopoolja.tistory.com/97)<br>
[INPA : CIDR 개념 쉽게 이해해보자](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-CIDR-%EC%9D%B4-%EB%AC%B4%EC%96%BC-%EB%A7%90%ED%95%98%EB%8A%94%EA%B1%B0%EC%95%BC-%E2%87%9B-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC-%EA%B3%84%EC%82%B0%EB%B2%95)<br>
