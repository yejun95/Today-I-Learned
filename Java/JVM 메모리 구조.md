![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/1ae630d7-ef2f-4126-8f54-3368cd31a9d3)## JVM(Java Virtual Machine)
- JVM은 자바 프로그램 실행환경을 만들어주는 소프트웨어이며, 메모리 관리(GC)를 수행하며 스택 기반의 가상머신이다.
  - GC : Garbage Collector
<br>

- 자바 코드를 컴파일하여 .class의 바이트 코드로 만들면 이코드가 자바 가상 머신 환경에서 실행된다.

- 바이트코드
  - JVM이 이해할 수 있는 언어로 변환된 Java 소스 코드를 의미
  - Java 컴파일러에 의해 변환되는 코드의 명령어의 크기가 [ 1 byte ]라서 Java 바이트 코드라고 불림
  - Java 바이트 코드는 자바 가상 머신(JVM)만 설치되어 있다면 어느 OS에서도 실행 가능
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/fe4f52b5-82c9-45a3-a987-f5f4089b506e)
<br>
<hr>
<br>

### ✔ 메모리 구조
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/24b3eaea-c8b8-42a8-952e-dbd8df1998ba)
<br>

- 메서드 영역 : JVM이 클래스(.class)파일을 읽어서 클래스 변수에 대한 정보를 저장하는 곳
  - `static`으로 선언한 공통의 변수
<br>

- 콜 스택 : 메서드 작업에 필요한 메모리 공간
  - 지역 변수와 함수 호출 시 생성되는 변수들을 저장하는 영역
  - 메서드 호출 시 해당 공간에 올라간다.  ->  메서드의 지역변수 포함
  - 메서드가 작업을 마치면 할당되었던 메모리 공간은 비워진다.
  - 스택 : LIFO(후입선출)  ->  나중에 들어온게 먼저 나가게 된다.  ->  제일 상단 메소드 부터 실행
<br>

- 힙 : 인스턴스가 생성되는 공간
  - 자바 프로그램에서 사용되는 모든 인스턴스 변수(객체)들이 저장
  - 자바에서는 new를 사용하여 객체를 생성하면 힙 영역에 저장
  - Heap 영역은 Stack 영역과 다르게 보관되는 메모리가 호출이 끝나더라도 삭제되지 않고 유지된다.
  - 그러다 어떤 참조 변수도 Heap 영역에 있는 인스턴스를 참조하지 않게 된다면, GC에 의해 메모리에서 청소된다.
<br>

> 메서드 호출 시 메모리 스택에 할당<br>
> 수행 후 메모리 반환하고 스택에서 제거<br>
> 스택 제일 위에 있는 메서드가 현재 실행 중인 메서드 (LIFO)<br>
> 아래에 있는 메서드가 바로 위에 있는 메서드를 호출한 메서드
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/01e6cb61-1fb7-44dc-8c93-c97f701f439a)
<br>
<hr>
<br>

## 내용 추가 2023-11-19
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/fb5b28dc-c488-468d-9642-a360efbfd9e3)

### ✔ .java파일 실행 시 
- `.java` 파일을 실행시키면 JAVAC으로 인하여서 .class 파일(바이트코드)의 형태로 변한다.

- `.class`의 파일을 이제 Class Loader로 넘긴 후 Class Loader가 이제 JVM의 메모리 영역(run time date area)에 적재시킨다.

- 적재시키는 과정 속 JVM의 메모리 영역은 5가지의 형태로 구분이 되어져있는데 다음과 같이 적재를 시킨다.
  - Method Area: 클래스와 관련된 정보를 저장한다.
  - Heap Area: 객체와 관련된 정보들에 대해 저장한다.
  - Stack Area: 메서드 내의 관련된 정보들에 대해 저장한다.(지역안에 존재하는 변수들 등등..)
  - PC Register: Thread(쓰레드)가 생성될 때마다 생성되는 영역, 현재 쓰레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역이다. 이것을 이용해서 쓰레드를 돌아가면서 수행할 수 있게 한다.
  - Native Method Area: JAVA가 아닌 다른 언어에 대한 정보를 저장한다.
<br>

**실행 엔진(Execution Engine)**
- Class Loader에 의해 `.class`파일들은 Runtime Data Area의 Method Area에 배치된다.

- JVM은 Method Area의 바이트 코드를 Execution Engine에 제공하여, Class에 정의된 내용대로 바이트 코드를 실행시킨다.
  - 이 때, 실행 방식 2가지가 존재한다.
  - 1. Interpreter: 바이트코드를 한 줄씩 해석, 실행하는 방식이다. 초기 방식으로, 속도가 느리다는 단점이 있다.
  - 2. JIT(Just In Time): 그래서 나온 것이 JIT(Just In Time) 컴파일 방식이다.<br>
  바이트코드를 JIT 컴파일러를 이용해 프로그램을 실제 실행하는 시점(바이트코드를 실행하는 시점)에 각 OS에 맞는 Native Code로 변환하여 실행 속도를 개선하였다.
<br>

- 💡 그러나, 바이트코드를 Native Code로 변환하는 데에도 비용이 소요되므로, JVM은 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고,<br>
인터프리터 방식을 사용하다 일정 기준이 넘어가면 JIT 컴파일 방식으로 명령어를 실행한다.
<br>
<hr>
<br>

**Reference**<br>

[khyojun : [Java를 실행하면 어떻게 진행이 되나요?] 최종: 그래서 자바를 실행하면 어떻게 되는데요?](https://velog.io/@nandong1104/Java%EB%A5%BC-%EC%8B%A4%ED%96%89%ED%95%98%EB%A9%B4-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%A7%84%ED%96%89%EC%9D%B4-%EB%90%98%EB%82%98%EC%9A%94-%EC%B5%9C%EC%A2%85-%EA%B7%B8%EB%9E%98%EC%84%9C-%EC%9E%90%EB%B0%94%EB%A5%BC-%EC%8B%A4%ED%96%89%ED%95%98%EB%A9%B4-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%90%98%EB%8A%94%EB%8D%B0%EC%9A%94)<br>
[수피치의 발자취 : JVM의 내부 구성 및 동작 원리](https://soopeach.tistory.com/247)
