# tick이란 무엇인가?
- 콜백 큐에서 콜 스택으로 이벤트를 밀어 넣는 한번의 작업을 tick이라고 한다.

- 때문에 tick을 이해하기 위해서는 js의 이벤트 과정을 이해해야 한다.
<br>
<hr>
<br>

## ✔️ javascript 동작 원리
- javascript 엔진은 실행시간에 메모리 관리를 위해 메모리 힙과 콜 스택을 사용한다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/26ffb42a-bf25-4c06-840e-bd167a9f7664)
> Memory Heap : 메모리 할당이 일어나는 곳<br>
Call Stack : 코드 실행에 따라 호출 스택이 쌓이는 곳
<br>

- 그리고 브라우저에서 제공하는 API인 webAPI와 비동기 작업을 처리하기 위한 이벤트 루프가 존재한다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/de5fe9ff-bda3-40c2-b250-4aa1e28b87e2)
<br>

- javascript는 싱글 스레드 기반이기 때문에 비동기적인 특성을 가지고 있다.
때문에 현재 줄의 코드를 처리하지 못하더라도 다음 코드를 바로 실행해버린다.

- 이런 비동기적 특성을 해결하기 위해 Promise, async/await 등이 존재한다.
  - API를 통하여 데이터를 받아오기도 전에 종속적인 코드를 실행해버리면 데이터가 없어서 에러 발생
<br>

- 이때 javascript가 비동기 처리를 위해 이벤트를 만나면 콜백 큐에 순서대로 쌓는다.

- 그리고 콜 스택이 비면, 콜백 큐에서 첫 번째 이벤트를 가지고 와서 밀어 넣는데, 이 과정을 이벤트 루프에서 Loop(반복)한다.
<br>
<hr>
<br>

## ✔️ 정리
- 결국 tick이란 콜백 큐에 있는 이벤트를 콜 스택에 다시 밀어넣는 작업이다.

- 해당 기능을 쓰는 이유는 javascript의 특성 때문에 UI가 갱신되기도 전, DOM을 탐색하는 상황일 때,
DOM을 찾지 못하는 경우 강제로 콜 스택에 밀어넣어 찾을 수 있게 하는 것이다.

- `nexttick`이라는 함수로써 사용되고, 특히 테스트 코드 작성 시 DOM이 그려지지 않은 상태에서 엘리먼트를 찾으려고 하면
에러가 발생한다.
  - 이때 tick을 사용하여 강제로 이벤트를 콜 스택에 밀어넣어 렌더링을 진행시킴
<br>
<hr>
<br>

**Reference**<br>

[쬬리 : javascript engine 이벤트 루프(Event Loop) 와 틱(tick)](https://joyhong-91.tistory.com/43)<br>

