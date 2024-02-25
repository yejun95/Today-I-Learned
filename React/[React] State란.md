## State란?
- 리액트 Component의 변경 가능한 데이터를 말한다.

- state는 사전에 정의되있는 것이 아니고, 필요에 따라 개발자가 정의한다.

- 렌더링이나 데이터 흐름에 사용되는 값만 state에 포함시켜야 한다.
  - 무분별한 데이터를 state에 포함하면 쓸데없이 렌더링이 될 수 있다.
<br>

- state는 단순히 자바스크립트 객체이다.
  - Ex : `const [test, setTest] = useState()`
<br>

- state는 직접 수정할 수 없으며, 배열의 두번째 함수인 `setTest`를 통해 수정해야 한다.
<br>
<hr>
<br>

### ✔ 예제
- useState에 초기값 10000을 넣어서 화면에 출력

```javascript
import React, { useState } from 'react'

function Example() {
    const [test, setTest] = useState(10000);  
    return (
      <>
          <div>{test}</div>
      </>
    )
}

export default Example
```

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/61bddc23-564f-43a8-8d84-98c703dd7aac)
<br>
<br>

- 이제 `setTest`를 이용하여 `test`의 값을 변경해보자.

```javascript
import React, { useState } from 'react'

function Example() {
    const [test, setTest] = useState(10000);  
    return (
      <>
          <div>{test}</div>
          <button onClick={() => setTest(2222)}>버튼</button>
      </>
    )
}

export default Example
```

- 초기에 10000 이였던 값이 버튼을 클릭하면 2222 값으로 변한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/aea7d6fe-6e12-49d0-b9ca-2a5404ae2223)
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/1ca5a1fa-b581-4b42-8456-040513a4c646)
<br>
<hr>
<br>

**Reference**<br>

[인프런 Inje Lee : 처음 만난 리액트(React)](https://www.inflearn.com/course/%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
