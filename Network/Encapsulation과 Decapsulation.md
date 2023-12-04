## Encapsulation
- 러시아의 전통 목각 인형 마트료시카 인형을 떠올리면 된다.

- 캡슐화라고도 불리며, 단위화를 하는 것이다.

- 패키징을 진행한다고도 부른다.

- 자바에서 접근제어자가 캡슐화를 구현하는 부분이다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/d2cd4ae3-704b-4bd6-8e84-ca145b27cd6f)
<br>
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/f9cc2fd2-51d3-4169-a2d9-e37f9446ff47)

- 제일 바깥쪽은 L2 계층이며 Frame이라고 부른다.

- Frame 안에는 L3가 오며, 즉 IP Packet 통째로 L2 Frame의 payload가 된다.
  - 마트료시카 예시를 든 이유가 payload를 뜯으면 header가 또 나오고 해당 헤더에서 또 payload를 뜯을 수 있기 때문이다.
<br>

- L3의 payload에는 L4의 TCP 헤더와 데이터가 들어가 있다.
  - TCP의 데이터 단위는 segment
<br>

- TCP 이후부터는 또 헤더, payload가 나오는게 아니고 Stream 형식으로 변하게 된다.

- 결국 Header, payload에 header를 붙이고 또 header를 붙이는 것이 `Encapsulation`이다.
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/1efd546a-2c6b-4ce4-8892-e099435183d3)
<br>
<hr>
<br>

### ✔ Decapsulation
- 반대로 header를 하나씩 지워가면서 안의 알맹이를 꺼내는 것이 `Decapsulation`이다.
<br>
<hr>
<br>

[payload란 무엇인가?]()
<br>
<br>

**Reference**<br>

[널널한 개발자 : 외워서 끝내는 네트워크 핵심이론- 기초](https://www.inflearn.com/course/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%ED%95%B5%EC%8B%AC%EC%9D%B4%EB%A1%A0-%EA%B8%B0%EC%B4%88/dashboard)<br>
