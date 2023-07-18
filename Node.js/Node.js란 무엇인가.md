# Node.js란 무엇인가?

Chrome V8 Javascript 엔진으로 빌드된 Javascript 런타임이다.<br>
(런타임 : 프로그래밍 언어가 구동되는 환경)<br>

즉 Node.js는 Javascript를 구동시키는 환경이기 때문에 웹 브라우저 환경에서 서버로도 사용이 가능하다.
<br><br><br>
💡 핵심 키워드

- 구글 V8 자바스크립트 엔진
- 싱글 스레드 : 비동기 I/O 처리로 인하여 논 블로킹 방식으로 작업을 수행 한다.
- 비동기 I/O 처리 : 비동기 방식으로 태스크가 병렬적으로 수행되게 하는 것.
- 방대한 모듈 제공(NPM)
- 이벤트 루프 : 여러 이벤트 발생 시 어떤 순서로 호출 할지를 판단 (호출 스택, 백그라운드, 태스크 큐)
<br>

❓ **싱글 스레드, 이벤트 루프, 비동기 대체 무슨 말들일까.??** ❓<br>
<br>
**싱글 스레드**

컴퓨터가 어떠한 프로그램을 수행할 때, 1개 작업당 1개의 스레드가 필요하게 되는데,<br>

즉, 무엇인가의 작업을 진행하기 위한 일꾼이라고 생각하면 된다.

그러면 여러 작업이 동시에 들어 왔을 때, 일꾼이 한명이기 때문에 속도가 느려질 것이다.

하지만 이를 위해 none-blocking I/O 방식을 취하게 되는데, 하나의 요청을 처리하는 동안

다른 요청을 block 하지 않는다는 뜻이다.
<br>
<br>
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/41b79c6e-8bd1-4d25-afa5-be5f31d013e5)
<br>
사진과 같이 작업을 1개씩 처리하는 것이 아니고 여러 작업을 동시에 진행하게 된다.
<br>
<br>
<br>
**<h3>💡 그런데 앞서 node.js는 분명 싱글 스레드라고 했는데,</h3>**
<br>
어떻게 멀티 스레드처럼 일꾼들을 데리고 동작할 수 있는걸까? 

먼저 node.js의 구조부터 살펴보자. 

node.js는 v8이라는 자바스크립트 엔진과 비동기 작업을 처리하는 libuv라는 라이브러리로 이루어져있다.

<br>
<br>

먼저 v8에는 memory heap과 하나의 call stack이 있다. [V8 memory heap, call stack](https://velog.io/@wjdwl002/V8-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%97%94%EC%A7%84%EC%9D%98-%EB%8F%99%EC%9E%91-%EB%B0%A9%EC%8B%9DMemory-Heap-Call-Stack)

(여기서 하나의 call stack이 있다는 것과 싱글 스레드는 같은 의미로 이해할 수 있다)

memory heap은 메모리 할당이 일어나는 곳이고, call stack은 코드 실행에 따라 호출 스택이 쌓인다.

call stack은 차례대로 실행되기 때문에 비동기 처리를 할 수 없다.

<br>
<br>

따라서, 비동기 작업을 가능하게 하는 것은 바로 libuv라는 라이브러리에서 non-blocking IO라는

기능을 가능하게 하는 이벤트 루프를 제공하기 때문이다. 

**<h3>libuv는 c언어로 생성되었고, 시스템 커넬을 이용하는데, 커넬은 멀티 스레드를 이용한다.</h3>**

**<h3>그렇기에 node.js는 libuv가 멀티 스레드로 동작하기 때문에 비동기 처리를 할 수 있다.</h3>**

아래 이미지의 internal C++ Thread Pool이 멀티 스레드이다.

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/91102e94-6e58-4ff2-9752-e3f762271d44)



[참고 : Node.js는 싱글스레드](https://velog.io/@daeseongkim/Node.js-Node.js%EB%8A%94-%EC%8B%B1%EA%B8%80-%EC%8A%A4%EB%A0%88%EB%93%9C)
