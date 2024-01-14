## 보일러 플레이트(Boiler Plate)란?
- BoilerPlate 코드란 모든 코드를 작성하기 위해 항상 필요한 부분을 의미

- Vue와 react등에서 npm 명령어를 통해 골격를 자동으로 생성하는 것도 포함한다.

- 만약 기업에서 Vue3를 사용하는데, 일정한 코드 패턴을 따라야 한다면 이를 자동화 하는 것도 보일러 플레이트인 것이다.
<br>
<hr>
<br>

### ✔ 구성요소
- Import : 필요한 코드를 불러들이는 부분

- Component : 현 페이지를 구현하는 코드

- StyleSheet : 페이지의 객체를 꾸미기 위한 style

- Export : 현 Javascript 코드를 타 Javascript에서 접근하기 위한 부분
<br>
<hr>
<br>

### ✔ 유래
- 코딩을 통해 기능을 구현하기위한 가장 빠른 방법은 '모방'이다.

- 우리가 코드를 이해하기 전, 따라 치는 것을 통해 기능을 구현한 적이 있을 것이다.

- 이를 통해, '묻지도 따지지도 않고 따라 적는 코드'를 우리는 보일러 플레이트(Boiler Plate) 코드라고 한다.
  - 이 용어는 미국 신문 업계 초창기에 매일 바뀌지 않고 동일한 내용(신문의 제목, 형태 등 변하지 않는 부분)을<br>
  효율적으로 출력하기 위해 '박아 놓고 똑같이 사용하기 위해' 작성된 철판 모형을 의미한다.
<br>

- 이런 용어가 프로그래밍으로 넘어와서 '별 수정 없이 반복적으로 사용되는 코드'를 보일러 플레이트라고 한다.

- 묻지도 따지지도 말고 그냥 복사한 다음, 필요 부분만 수정하면 된다는 말이다.

- Vue3를 기준으로 봐보자
```Vue
// Component
<template>
  <v-app>
    <v-main class="w-75 mx-auto">
      <TheHeader/>
      <TheView/>
      <TheFooter/>
    </v-main>
  </v-app>
</template>

<script>
// Import
import TheHeader from './layouts/TheHeader.vue'
import TheView from './layouts/TheView.vue'
import TheFooter from './layouts/TheFooter.vue'
import useAuth from '@/composables/useAuth';

// Export
export default {
  name: 'App',

  components: {
    TheHeader,
    TheView,
    TheFooter,
  },

  setup() {
    // 새로고침해도 세션 및 쿠키 체크를 통해 로그인을 유지한다.
    const { check } = useAuth();
    check()
  }
}
</script>

// StyleSheet
<style>

</style>

```
<br>

- 안에 내용물은 달라질 수 있지만, 위 코드에서 보면 4가지의 태그는 어느 component에서든지 유지된다.

- 이렇게 반복적으로 나타나는 코드를 보일러 플레이트라고 한다.
<br>
<hr>
<br>

**Reference**<br>

[개발자로 취직하기 : 보일러 플레이트(Boiler Plate) 이해하기](https://coding-grandpa.tistory.com/2)<br>
[꿈많은 DEVELOPER : 보일러플레이트(Boilerplate code)란? - 개념, 필요한 이유](https://cometruedream.tistory.com/216)
