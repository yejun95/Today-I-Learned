## Custom Hook
- 반복적인 로직을 Custom Hook으로 만들어 사용한다.

- 이름이 use로 시작하고 내부에서 다른 Hook을 호출하는 하나의 자바스크립트 함수이다.

- 단순한 함수이기 때문에 리액트 컴포넌트처럼 제약이 없다.
  - 파라미터 등을 마음대로 받을 수 있다.
<br>
<hr>
<br>

### ✔ 예시
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/fe284e25-9cbc-4982-997b-83b35e4b96ba)
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/5d386a7a-7642-45fd-bc28-3b90e464422a)
<br>
<br>

- 위에 두 코드는 전부 동일하지만 마지막 return 부분만 다르다.

- 이러면 공통 로직을 Custom Hook으로 추출하여 진행할 수 있다.
<br>
<br>

**공통 로직 추출**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/0a0d9b70-da73-4155-9ff8-2087edcead78)
<br>
<br>

- 공통 로직 추출 후 각각 적용

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/9ecb6344-06b4-40fa-a03d-04f6d566f471)
<br>
<br>

- 해당 코드는 Custom Hook을 추출하기 전과 동일하게 작동한다.
<br>
<hr>
<br>

### ✔ React Hook 끼리 데이터를 공유하는 방법
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/0356f280-9bb7-4922-bb28-472c3c3bb278)
<br>
<br>

- useState로 설정한 값을 Custom Hook에 인자값으로 넣어 현재 컴포넌트와 Custom Hook 과의 데이터를 공유한다.
<br>
<hr>
<br>

### ✔ 실습 예제
```javascript
// useCounter.jsx

import React, { useState } from 'react'

function useCounter(initialValue) {
    const [count, setCount] = useState(initialValue);

    const increaseCount = () => setCount((count) => count + 1);
    const decreaseCount = () => setCount((count) => Math.max(count - 1, 0));

    return [count, increaseCount, decreaseCount];
}

export default useCounter;
```
<br>

- Custom Hook으로써 매개변수로 value를 받을 수 있다.

- 해당 value는 useState의 count에 값이 맵핑된다.

- 두가지 함수는 count 증가와 감소를 나타낸다.
<br>
<br>

```javascript
// Accommodate.jsx

import React, { useState, useEffect } from 'react'
import useCounter from './useCounter'

const MAX_CAPACITY = 10;

function Accommodate(props) {
    const [isFull, setIsFull] = useState(null);
    const [count, increaseCount, decreaseCount] = useCounter(0);

    useEffect(() => {
        console.log("=======================")
        console.log("useEffect() is called.")
        console.log(`isFUll: ${isFull}`)
    })

    useEffect(() => {
        setIsFull(count >= MAX_CAPACITY);
        console.log(`Current count value : ${count}`)
    }, [count])

    return (
        <div style={{ padding: 16 }}>
            <p>{`총 ${count}명 수용했습니다.`}</p>

            <button onClick={increaseCount} disabled={isFull}>
                입장
            </button>
            <button onClick={decreaseCount}>퇴장</button>

            {isFull && <p style={{ color: "red" }}>정원이 가득찼습니다.</p>}
        </div>
    )
}

export default Accommodate;
```
<br>

- 각 버튼에 Custom Hook에서 만들어놓은 함수를 맵핑한다.

- 버튼이 클릭될 때 마다 Custom Hook에서 정의해놓은 count에 값이 변경된다.

- count값이 계속 올라가서 `MAX_CAPACITY`에 정의해놓은 값에 다다르면 더 이상 증가 시킬 수 없다.
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/65ae5a05-765d-44b9-a36c-b131dcb5bad8)
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a80c1269-7ef3-497b-8e19-a48da09da548)
<br>
<hr>
<br>

**Reference**<br>

[인프런 Inje Lee : 처음 만난 리액트(React)](https://www.inflearn.com/course/%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
