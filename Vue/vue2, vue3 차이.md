## Vue2와 Vue3의 차이
### ✔ Fragmenst
- vue2에서는 컴포넌트의 루트 요소가 1개일 필요가 있었다.

```javascript
<template>
  <div>
    <header>...</header>
    <main>...</main>
    <footer>...</footer>
  </div>
</template>
```
<br>

-  Vu3에서는 여러 개의 요소를 루트 요소로 지정할 수 있다. 

```javascript
<template>
  <header>...</header>
  <main>...</main>
  <footer>...</footer>
</template>
```
<br>
<hr>
<br>

### ✔ 글로벌 API
- Vue 인스턴스의 생성 방법이 글로벌 API를 사용하는 방법으로 변한다.

- 위가 Vue2, 아래가 Vue3이다.

```javascript
new Vue({
  el: '#app',
  ...
})
```

```javascript
Vue.createApp({
  ...
}).mount('#app')
```
<br>
<hr>
<br>

###  composition API
- 다음 항목들을 setup 함수 안에서 전부 포함한다.
  - methods
  - data
  - lifecycle hooks
  - watch
<br>

- Vue2에서는 option API를 통해서 data나 methods등 역할마다 정리해서 기재한다.

```javascript
<script>
export default {
  data: () => ({
    count: 0,
  })
  methods: {
    increment() {
      this.count++;
    },
    decrement() {
      this.count--;
    }
  }
}
</script>
```
<br>

- 그러나 Composition API를 사용하면 아래와 같이 바뀐다.
  - 기존의 data, method 등의 선언이 setup() 메서드 안에서 모두 처리가 가능하다.
  - `ref` 함수는 `count` 변수를 반응적인 상태로 만든다. (초기값 0 설정한 것).
  - 💡 `setup()`안에 API를 넣는 이유는 컴포넌트보다 먼저 생기기 때문이다.
  가장 먼저 생성되기 때문에 `setup()` 안에서 `this`는 사용할 수 없다.

```javascript
<script>
import { ref } from 'vue';
export default {
  setup() {
    const count = ref(0); 
    const increment = () => { count.value++; }
    const decrement = () =>{ count.value--; },
  return {
     // 여기서 필요한 데이터를 리턴한다.
   }
  }  
}
</script>
```
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/9b1879e3-0a71-4db3-8fbc-d2d61027a405)
<br>
<hr>
<br>

### ✔ LifeCycle hook 호출 변화
- vue2는 data, method와 같은 함수 안에서 선언하였지만,
vue3에서는 setup내부에서 선언 하도록 만들었다.

```javascript
import { onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted, onActivated, onDeactivated, onErrorCaptured } from 'vue'
    
    export default {
      setup() {
        onBeforeMount(() => {
          // ... 
        })
        onMounted(() => {
          // ... 
        })
        onBeforeUpdate(() => {
          // ... 
        })
        onUpdated(() => {
          // ... 
        })
        onBeforeUnmount(() => {
          // ... 
        })
        onUnmounted(() => {
          // ... 
        })
        onActivated(() => {
          // ... 
        })
        onDeactivated(() => {
          // ... 
        })
        onErrorCaptured(() => {
          // ... 
        })
      }
    }
```
<br>
<br>

### ✔ props와 this의 분리
- 기존 vue2 코드는 title이 props의 값인지 data인지 method인지 구별하기 힘들다.
  - 밑에 this가 만약 광범위한 소스코드에서 등장했다면 정체성을 알기 힘들 것이다.

```javascript
 mounted () {
        console.log('title: ' + this.title)
    }
```
<br>

- vue3에서는 setup은 props를 사용하기 위해 attribute로 받아야하고 다음과 같이 사용해 구분하기 쉽다.
  - this는 사용할 수 없지만 정체성을 파악할 수는 있다.
```javascript
   setup (props) {
        // ...
        onMounted(() => {
          console.log('title: ' + props.title)
        })
        // ...
    }
```
<br>
<hr>
<br>

**Reference**<br>

[dev-junku.log : Vue 2 → Vue 3 정리](https://velog.io/@dev-junku/Vue-2-Vue-3-%EC%A0%95%EB%A6%AC)<br>
[Moz1e's log : vue2 & vue3 차이점](https://moz1e.tistory.com/540)<br>
[두더지 개발자 : Vue2와 Vue3의 차이](https://engineer-mole.tistory.com/419)<br>
[lire_eruel.log : [TIL] Vue 2와 Vue 3의 차이](https://velog.io/@lire_eruel/TIL-Vue-2%EC%99%80-Vue-3%EC%9D%98-%EC%B0%A8%EC%9D%B4)
