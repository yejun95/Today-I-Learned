## Warning: A component is changing an uncontrolled input to be controlled.
- 리액트에서 input에 입력하는 순간 아래와 같은 에러가 뜰 때가 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/dbe5c674-40ca-45c2-a485-d93f0da70092)
<br>
<br>

### ✔ 발생 원인
- input 의 value에 undefined가 들어갔을 경우에 대한 처리가 없어서 생기는 에러이다.

- uncontrolled input 이였다가 controlled input으로 바뀌면서 발생한다.

- 즉, 한마디로 초기값이 undefined였다가 렌더링 후에 값이 들어와 바뀌면서 발생하는 에러이다.

- 에러가 발생하는 예시 코드는 아래와 같다.

```javascript
import React, { useState } from 'react'

function Test() {
  const [value, setValue] = useState()

  return (
    <>
      <input onChange={(e) => setValue(e.target.value)} value={value}/>
    </>
  )
}


export default Test
```

- 현재 `value={value}` 해당 부분에서 undefined 처리가 없기 때문에 에러가 나므로 `value={value || ''}` 이렇게만 고쳐주면
더 이상 에러가 발생하지 않는다.
<br>
<hr>
<br>

### ✔ 다른 해결 방법
- `value={value || ''}` 해당 문법을 쓰면 상당히 어색해 보이는데 더 간단하게 해결하는 방법이 있다.

- 바로 useState()에 초기값을 설정해주면 끝이다.
  - `useState('')`
<br>

- 그러니 웬만하면 useState() 에다가 초기값을 설정해주는 방법으로 사용하는 것이 좋다.

**Reference**<br>

[코딩병원 : A Component is changing an uncontrolled input to be controlled 해결 방법](https://itprogramming119.tistory.com/entry/A-component-is-changing-an-uncontrolled-input-to-be-controlled-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95)<br>
