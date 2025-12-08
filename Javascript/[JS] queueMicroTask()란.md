# queueMicroTask란 무엇인가?
- `queueMicroTask()` 메서드는 브라우저의 이벤트 루프로 통제권이 넘어가기전, 안전한 시점에 실행할 마이크로태스크를 큐에 추가하는 기능

- 현재 태스크가 종료된 후, 그리고 실행 맥락의 통제권이 브라우저 이벤트 루프로 넘어가기 전에,<br>
실행해야하는 다른 코드가 없으면 처리되는 짧은 함수
<br>

### 이벤트 루프 구조
```
┌─────────────────────────────────────────────────────────┐
│                      Call Stack                         │
│                   (현재 실행 중인 코드)                    │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼ 스택이 비면
┌─────────────────────────────────────────────────────────┐
│               Microtask Queue (우선순위 높음)             │
│  • queueMicrotask() 콜백                                 │
│  • Promise.then/catch/finally 콜백                       │
│  • MutationObserver 콜백                                 │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼ 마이크로태스크가 모두 비면
┌─────────────────────────────────────────────────────────┐
│               Macrotask Queue (우선순위 낮음)             │
│  • setTimeout / setInterval                             │
│  • I/O 이벤트, 클릭 이벤트 등                              │
│  • requestAnimationFrame                                │
└─────────────────────────────────────────────────────────┘
```
<br>
<hr>
<br>

## ✔️ 사용법
### 기본 사용법
```js
queueMicrotask(() => {
  console.log('마이크로태스크에서 실행됨');
});
```
<br>
<br>

### 실행 순서 이해하기
- 예제 1: 기본 실행 순서

```js
console.log('1. 동기 코드 시작');

setTimeout(() => {
  console.log('4. setTimeout (매크로태스크)');
}, 0);

queueMicrotask(() => {
  console.log('3. queueMicrotask (마이크로태스크)');
});

console.log('2. 동기 코드 끝');

// 출력 순서:
// 1. 동기 코드 시작
// 2. 동기 코드 끝
// 3. queueMicrotask (마이크로태스크)
// 4. setTimeout (매크로태스크)
```
> 왜 이 순서인가?<br>

> 동기 코드가 먼저 모두 실행됨<br>
> 콜 스택이 비면 마이크로태스크 큐 확인 → queueMicrotask 실행<br>
> 마이크로태스크 큐가 비면 매크로태스크 큐 확인 → setTimeout 실행
<br>
<br>

- 예제 2: Promise와의 관계
```js
console.log('1. 시작');

Promise.resolve().then(() => {
  console.log('3. Promise.then');
});

queueMicrotask(() => {
  console.log('4. queueMicrotask');
});

Promise.resolve().then(() => {
  console.log('5. 또 다른 Promise.then');
});

console.log('2. 끝');

// 출력:
// 1. 시작
// 2. 끝
// 3. Promise.then
// 4. queueMicrotask
// 5. 또 다른 Promise.then
```
> Promise.then()과 queueMicrotask()는 같은 마이크로태스크 큐를 사용<br>
등록된 순서대로 실행
<br>
<hr>
<br>

## ✔️ 주의 사항
### 무한 루프 주의
```js
// ❌ 이러면 안 됨 - 브라우저가 멈춤
function badIdea() {
  queueMicrotask(() => {
    badIdea(); // 마이크로태스크가 계속 추가됨
  });
}
// 매크로태스크(렌더링 포함)까지 절대 도달 못함!
```
<br>
<br>

### 에러 처리
```js
queueMicrotask(() => {
  throw new Error('마이크로태스크에서 에러!');
});
// 이 에러는 잡히지 않고 전역 에러로 처리됨

// 안전하게 하려면:
queueMicrotask(() => {
  try {
    // 위험한 작업
  } catch (e) {
    console.error('처리됨:', e);
  }
});
```
<br>
<hr>
<br>

## ✔️ 정리
### 언제 쓰면 좋을까?
- 현재 동기 코드가 끝난 직후에 뭔가를 실행하고 싶을 때

- 여러 작업을 일괄 처리(batching)하고 싶을 때

- Promise 없이 일관된 비동기 패턴을 만들고 싶을 때

- `setTimeout(fn, 0)`보다 더 빨리 실행하고 싶을 때
<br>
<br>

### 핵심 기억 포인트
- 마이크로태스크는 매크로태스크보다 우선순위가 높다.

- `Promise.then()`과 같은 큐를 사용한다.

- 현재 콜 스택이 비면 즉시 실행된다.

- 렌더링 전에 실행된다.
