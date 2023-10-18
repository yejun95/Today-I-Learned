## 컴퓨터의 계층 구조
- 컴퓨터는 H/W, S/W, App 이렇게 3계층을 나뉜다고 볼 수 있다.
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/62984ccb-bd2c-451e-8063-07cb66fa93d5)
<br>

- 아래서부터 순차적으로 H/W, S/W, App의 계층이다.
  - 이를 국가와 국민으로 예를 들어보자.
  - 영토(H/W)가 없으면 정부(S/W가) 만들어질 수 없고, 정부(S/W)가 없으면 국민(App)이 존재할 수 없다.
<br>

- 이러한 계층 구조에서 위의 계층은 아래 계층에 대한 의존성을 가진다.
  - 즉, 아래 계층이 존재하지 않으면 자신은 존재할 수 없다.
  - ex) H/W는 S/W의 존립 기반이다.  S/W는 App의 존립 기반이다.
<br>
<hr>
<br>

### ✔ 각 영역의 컴퓨터 구성요소
- 위에 그림을 세부적으로 설명해보자.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/2414ff6f-df21-475a-bbd4-adb9570138fd)
<br>

- S/W와 App는 인간계과 신계로 생각할 정도로 각 계층이 매우 독립적이다.
  - 완전히 다른 세상이기 때문에 경계선이 극명하다.
<br>

- 다시 국가와 국민을 통해서 설명해보자
  - 민간 영역에서 각 국민은 자신만의 집, 공간이 존재한다. <br>
  ->  프로세스의 메모리 공간<br>
    
  - 다른 사람의 집은 정부에서 법적으로 들어가지 못하게 하였다.<br>
  ->  OS가 각 프로세스의 독립성과 원자성을 유지시킨다.<br>
  
  - 그러나 검찰의 경우 법적으로 조사를 위해 다른 집에 들어갈 수 있다.<br>
  ->  OS가 Debugger를 통해 다른 프로세스 메모리에 접근하게 해준다.<br> (검찰 == Debugger)
<br>

- OS의 핵심 알맹이 : Kernel  ->  접근/통제를 관리하는 것이 키포인트다.
<br>
<hr>
<br>

**Reference**<br>

[널널한 개발자 : 넓고 얕게 배워서 컴공 전공자 되기](https://www.inflearn.com/course/lecture?courseSlug=%EB%84%93%EA%B3%A0%EC%96%95%EA%B2%8C-%EC%BB%B4%EA%B3%B5-%EC%A0%84%EA%B3%B5%EC%9E%90&unitId=128255)
****
