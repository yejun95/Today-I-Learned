## composables에서 데이터를 담은 ref 객체가 component에서 .value로 데이터 출력이 되지 않는 경우
- composables에서 axios를 통해 ref 객체에 동적으로 데이터를 담고 component에서 composables를 호출해<br>
`.value`로 값을 찍으면 데이터가 출력되지 않는다.

- 하지만 `.value`를 빼고 출력하면 데이터가 정상적으로 출력이 된다. 왜 그런 것일까?

- 아래 예시 코드부터 봐보자.

```javascript
// useTest.js

import axios from 'axios'
import { ref } from 'vue';

export default function useTestData() {
  const testArr = ref([]);

  axios.get('http://localhost:3000/posts')
    .then(res => {
      testArr.value = res.data
    }).catch(err => {
      console.log(err)
    })

  return {
    loadTest,
    testArr,
  }
};
```

```javascript
// HomeView.vue

<template>
  <h1>HomeView</h1>
</template>

<script setup>
import useTestData from '@/composables/useTest.js'

const { testArr } = useTestData();

console.log(testArr.value)

</script>

```

- 위 코드와 같이 composables에서 axios를 사용해 `testArr`에 데이터를 담았다.

- 그리고 사용할 component에서 import를 하고 `console.log`로 `console.log(testArr.value)`로 찍어보면 값이 나오지 않는다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/10aa306a-09a3-4da8-8b00-4b026cb95ae2)
<br>
<br>

- 그러나 `.value`를 제거하고 `console.log(testArr)`만 찍어보면 데이터가 정상적으로 출력된다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/5303665b-67f7-4200-91ac-016d6be28f92)
<br>
<br>

- 왜 이러한 차이를 보이는 것일까?
<br>

**💡 이러한 문제는 동기/비동기의 차이로 인해 발생하는 것이다.**
- 페이지가 실행되자마자 동기적으로 composables 함수가 실행되어 데이터가 제대로 담기지 않는 것이다.
<br>

- 그러면 같은 코드를 버튼을 통해 비동기로 호출하도록 만들어 보자.

```javascript
// useTest.js

import axios from 'axios'
import { ref } from 'vue';

export default function useTestData() {
  const testArr = ref([]);

  const loadTest = async () => {
    await axios
      .get('http://localhost:3000/posts')
      .then(res => {
        testArr.value = res.data
      })
      .catch(err => {
        console.log(err)
      })
  }

  return {
    loadTest,
    testArr,
  }
};
```

```javascript
<template>
  <h1>HomeView</h1>
  <button @click="fetchData">버튼</button>
</template>

<script setup>
import useTestData from '@/composables/useTest.js'

const { testArr, loadTest  } = useTestData();

const fetchData = () => {
  loadTest();
  console.log('testArr', testArr.value)
}
</script>
```

- 해당 코드에서는 composables에서 axios를 `async/await`를 사용하고 함수로 사용하였다.

- 이를 component에서 버튼을 클릭해 호출하도록 변경, 아래의 그림을 통해 `console.log` 데이터의 변화를 봐보자.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/3c912a83-4ad9-48af-85b9-9d529752b757)

- `console.log(testArr.value)`로 입력했음에도 이전과 다르게 정상적으로 데이터가 출력되었다.

- `console.log(testArr)` 로 출력하여도 당연하게 정상적으로 잘 나온다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/daebec48-66ac-4cbc-9757-677f65358702)
<br>
<hr>
<br>

### ✔ Summary
- 동기/비동기의 차이로 인해 composables에서 ref 객체에 값이 제대로 맵핑되지 않는 경우가 발생한다.

- axios를 즉시 실행하지말고 비동기적으로 실행해야 `.value`로 출력을 할 수 있다.
  - 물론 프로그램이 복잡하지 않다면 `async/await`를 제거하여도 무방하다.
<br>

- 만약 composables로 넘긴 ref 객체를 `.value`를 사용하지않고 배열 그대로 `v-for`에 사용한다고 하면<br>
axios를 즉시 실행하여도 무방하다.
