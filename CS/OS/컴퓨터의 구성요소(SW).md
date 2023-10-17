## 컴퓨터의 구성요소는 무엇이 있는가?
- 컴퓨터는 H/W와 S/W로 구성된다.

- 이중 S/W는 유저 경험으로 이루어지는 Application과 System S/W로 구분된다.

- 💡 가장 대표적인 System S/W는 OS(Operation System)이다.
<br>
<hr>
<br>

### ✔ 프로그램, 프로세스, 스레드
- `프로그램`은 설치 하는 것.

- 설치된 프로그램을 실행하면 `프로세스`가 생성된다.

- `쓰레드`는 프로세스 속에 존재하는 실행단위이다.
  - 프로세스에세 할당된 자원을 공유한다. (자원 == 메모리)
<br>

- 생성 순서 : 프로그램  ->  프로세스  ->  스레드

- ex) 엑셀을 설치하면 프로그램이 설치되는 것이다.
  - 설치한 엑셀을 실행하면 프로세스가 생성이 된다.
  - 엑셀을 사용하게 되면 프로세스 안에서 쓰레드가 실행된다. (실행 == 연산)
 <br>
 
**프로세스 사진**
- 컴퓨터에서 작업관리자를 들어가면 보이는 것이 프로세스이다.
  - 현재 실행중인 프로그램을 보여주는 것
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/4d1fd513-669d-422b-9911-59a93971b825)
<br>
<hr>
<br>

### ✔ 용도에 따른 기억공간의 구분
- 프로세스에는 `Heap`이라는 영역이 존재한다.

- 쓰레드에는 `Stack`이라는 영역이 존재한다.

- `Heap`은 공용으로 사용하는 영역이지만 `Stack`은 각 용도마다 생성이 된다.
<br>

**집 평면도를 활용한 예시**
- 주방, 거실등은 공용 공간으로 사용되고 있다.

- 침실, 드레스룸, 욕실 등은 각 공간마다 쓰임세가 확정되어있다. (자는곳, 갈아입는곳, 씻는곳)

- 즉, 각 쓰레드는 용도에 따라 구분되어 실행된다.
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/e90377bb-bfe2-4c2f-a482-f91ccd12b0d9)
<br>

- 아래 그림을 보면 한 프로세스 안에 여러 쓰레드가 존재한다.
  - 쓰레드는 Stack에 쌓인 일들을 처리한다.
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/5cc4ea5c-f8a8-49d8-bb9a-48c861700ae1)
<br>
<hr>
<br>

**Reference**<br>

[널널한 개발자 : 넓고 얕게 외워서 컴공 전공자 되기](https://www.inflearn.com/course/lecture?courseSlug=%EB%84%93%EA%B3%A0%EC%96%95%EA%B2%8C-%EC%BB%B4%EA%B3%B5-%EC%A0%84%EA%B3%B5%EC%9E%90&unitId=128254)<br>
[찰스의 안드로이드: 운영체제의 Process와 Thread 이야기](https://www.charlezz.com/?p=44590)
