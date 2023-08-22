## provide/inject란?
- vue 2.2버전부터 추가된 것으로 부모/자식 간 데이터 교환의 기능을 한다.
<br>
<hr>
<br>

### ✔ 사용 이유
- 부모/자식간 1단계씩 거쳐가는 props와는 달리 단계를 거치지 않고 데이터 교환이 가능하다.
<br>

- props 드릴링<br>
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/5e8762e9-f066-4d53-af14-722cd0148f09)
<br>

- provide / inject 드릴링<br>
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/d78d66b7-4963-43b2-b95c-24b5fb492654)
<br>
<hr>
<br>

### ✔ 실습 코드

**parents**
```vue
<template>
  <div class="home">
    <input type="text" v-model="test2" />
    <TestComponent />
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import TestComponent from "@/components/test.vue";
@Options({
  provide() {
    return {
      test: this.test2,
    };
  },
  data() {
    return {
      test2: "etst2",
    };
  },
  components: {
    TestComponent,
  },
})
export default class Home extends Vue {}
</script>
```
<br>

**child**
```vue
<template>
  <div>{{ test }}</div>
</template>
<script>
import { Vue, Options } from "vue-class-component";

@Options({
  inject: ["test"],
})
export default class Test extends Vue {}
</script>
```
<br>

💡<br>
- props와 달리 간편하게 사용할 수는 있지만 구조가 조금만 복잡해지더라도 provide가 어디서 시작 됐는지 확인하기가 쉽지 않다.<br>

- 자식 컴포넌트만이 provide를 가져올 수 있다.
<br>
<hr>
<br>

**Reference**<br>

[Vue.js: Provide / Inject](https://ko.vuejs.org/guide/components/provide-inject.html#prop-drilling)<br>
[allblack의 개발 새발: Vue3 Provide / Inject 정리](https://allblack0811.tistory.com/39)


