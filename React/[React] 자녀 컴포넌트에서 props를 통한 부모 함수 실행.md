## 자녀 컴포넌트에서 props를 통한 부모 함수 실행
- 리액트에서 기본적으로 부모 -> 자녀로 props를 보내서 값을 받는 경우가 있다.

- 또한 부모 -> 자녀로 보낸 props를 다시 자녀 -> 부모로 보낼 수도 있다.
  - 이때는 `setState`로 값을 설정해서 보냄
<br>

- 하지만 부모 -> 자녀로 보낸 props가 값이 아닌 함수를 보낼 수가 있다.

- 즉, 자녀 컴포넌트에서 해당 props를 사용하면 해당하는 부모 컴포넌트의 함수가 실행되는 것이다.
<br>
<hr>
<br>

### ✔ 부모 컴포넌트
```javascript
// LandingPage.jsx

import React, {useState} from 'react'
import Toolbar from './Toolbar'

function LandingPage() {
    const [isLoggedIn, setIsLoggedIn] = useState(false)

    const onClickLogin = () => {
        setIsLoggedIn(true)
    }

    const onC<lickLogout = () => {
        setIsLoggedIn(false)
    }
    return (
        <div>
            <Toolbar
                isLoggedIn={isLoggedIn}
                onClickLogin={onClickLogin}
                onClickLogout={onClickLogout}
            />
            <div style={{ padding: 16 }}>소통과 함께하는 리액트 공부!</div>
        </div>
    )
}

export default LandingPage
```
<br>

- `isLoggedIn` 이라는 state를 사용해 boolean 타입을 다루고 있다.

- 또한 `onClickLogin`, `onClickLogout` 함수를 실행할 때 마다 `isLoggedIn` State의 값이 변경된다.

- 하지만 state 값의 변경을 자식 컴포넌트에서 진행하기 위하여 props로 내려주고 있는 상황이다.

- 즉, 순수한 값이 아닌 함수 자체를 props로 보내 자식 컴포넌트에서 함수를 실행하게 만드는 것이다.
<br>
<hr>
<br>

### ✔ 자식 컴포넌트
```javascript
import React from 'react'

function Toolbar(props) {
    const { isLoggedIn, onClickLogin, onClickLogout } = props;
    
    return (
        <div>
            {isLoggedIn && <span>환영합니다.</span>}
            {isLoggedIn ? (
                <button onClick={onClickLogout}>로그아웃</button>
            ) : (
                <button onClick={onClickLogin}>로그인</button>
            )}
        </div>
    )
}

export default Toolbar
```
<br>

- 자식 컴포넌트에서는 받은 props를 구조분해할당으로 변수에 맵핑하고 있다.

- 이때 로그아웃, 로그인 버튼을 클릭하면은 props를 실행한다.

- 그러면 부모 컴포넌트의 함수가 실행되면서 boolean 타입이 변경된다.
<br>
<br>

- 처음에는 boolean이 false 이기 때문에 로그인 버튼으로 출력된다.

- 클릭하면 props로 내려온 `onClickLogin` 함수를 실행시킨다.

- 이때, 부모 컴포넌트에서 동일한 이름인 `onClickLogin` 함수가 실행되면서 `isLoggedIn` state의 boolean 값이 변경된다.
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/20cebf7f-b9d6-404b-a7f4-a4835f3dc190)
<br>
<br>

- 로그아웃 과정도 로그인 과정과 동일하다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/77c12485-9de7-4945-9bab-27f6a22b9c25)
<br>
<hr>
<br>

**Reference**<br>

[인프런 Inje Lee : 처음 만난 리액트(React)](https://www.inflearn.com/course/%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
