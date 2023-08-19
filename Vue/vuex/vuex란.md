## Vuex란 무엇인가?

- vue.js에 대한 상태(state) 관리 패턴 + 라이브러리 이다.
  - state == data
  - getters == computed
  - mutations == method
  - actions == 비동기 method 
 <br>

 여기서 상태란 무엇을 말하는가?
 <br>

 ```javascript
new Vue({
  // 상태
  data () {
    return {
      count: 0
    }
  },
  // 뷰
  template: `
    <div>{{ count }}</div>
  `,
  // 액션
  methods: {
    increment () {
      this.count++
    }
  }
})
```
<br>

data()를 상태(state)라고 부르며, 바로 이 상태를 관리하는 것이 vuex이다.
<br>
<hr>
<br>

**✔ 등장 배경**

- 지나치게 중첩된 `props`나 `ref`등은 유지보수를 힘들게 하며, 난해한 코드로 바뀌게 된다.

- 특히 Vue는 단방향으로 데이터가 흐르기 때문에, 여러 컴포넌트가 한 상태를 공유하는 경우<br>
형제 컴포넌트간의 상태공유/관리가 복잡해진다.
<br>

**💡 이벤트 버스를 쓰면 되는거 아닌가?**
- 코드가 복잡해지면 어디서 이벤트를 보냈는지 혹은 어디서 이벤트를 받았는지 알기 어렵다.
<br>

예시
```javascript
// Login.vue
eventBus.$emit('fetch', loginInfo);
​
// List.vue
eventBus.$on('display', data => this.displayOnScreen(data));
​
// Chard.vue
eventBus.$emit('refreshData', chartData);
```
<br>

이러한 문제로 인하여 vuex store기능을 통해 값을 저장하면서 state를 변경시켜 components에 반영하게 된다.<br>
기존 : props나 이벤트 버스를 통한 state 변경
[state 변경 과정 참조](https://devscb.tistory.com/63)
<br>
<hr>
<br>

**✔ 해결 가능한 문제**

**MVC 패턴에서 발생하는 구조적 오류**
- 뷰와 모델의 양방향 통신으로 인해 하나의 뷰가 모델을 변경하였을 때, 다시 그 모듈이 연관된 뷰들을 갱신하고<br>
업데이트 된 뷰들이 연관된 모델을 다시 갱신하므로 엮이고 엮이는 관계를 추적하기 힘들다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/e4a33be0-dd66-46ac-8184-343f3dd93166)
<br>

- 단뱡항 통신의 Flux 패턴을 사용
[Flux 패턴이란?]()
<br>

**컴포넌트 간 데이터 전달 명시**
<br>

**여러 개의 컴포넌트에서 같은 데이터를 업데이트 할 때 동기화 문제(mutations, actions)**
<br>
<hr>
<br>

**✔ Vuex 구조**

- 뷰 컴포넌트 -> 비동기 로직 -> 동기 로직 -> 상태
  - 시작점은 Vue Components이다.
  - 컴포넌트에서 비동기 로직(Method를 선언해서 API 콜 하는 부분 등)인 Actions를 콜하고,
  - Actions는 비동기 로직만 처리할 뿐 State(Data)를 직접 변경하진 않는다.
  - Actions가 동기 로직인 Mutations를 호출해서 State(Data)를 변경한다.
  - Mutations에서만 State(Data)를 변경할 수 있다.

- 참고 자료
  - [자바스크립트 비동기 처리와 콜백 함수](https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/)<br>
  - [자바스크립트 Promise 쉽게 이해하기](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/cbe9ab16-78f6-4208-b168-9def0c1bfcff)
<br>
<hr>
<br>

**Reference**

[doozi : Vuex란? 개념과 예제](https://doozi0316.tistory.com/entry/Vuex-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%98%88%EC%A0%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)<br>
[nroo : Vuex란 Vuex의 컨셉과 구조](https://ict-nroo.tistory.com/106)<br>
[devscb : vuex란, vuex 사용이유, vuex 구조 등등...](https://devscb.tistory.com/63)<br>
[캡틴판교 : Vue.js 중급 강좌](https://www.inflearn.com/course/vue-pwa-vue-js-%EC%A4%91%EA%B8%89/dashboard)<br>

