# Context API
- 기존 부모 컴포넌트와 자식 컴포넌트끼리 데이터를 주고 받기 위해서는 props를 이용한다.

- 그러나 컴포넌트 구조가 복잡해지면 props가 중첩되기 때문에 사용하기 매우 힘들다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/b01ee8ad-c62c-41db-a70d-57c72bb7b704)
> 기존 방식 : props가 중첩되어 구분하기 힘들다.
<br>

- Context 방식은 부모 컴포넌트의 데이터를 하위 컴포넌트 어디서든 사용이 가능하다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/dbdec203-3fd9-4cc9-b787-8025c8ba7afe)
> Context 사용
<br>
<hr>
<br>

## ✔ 사용법
- `React.createContext(value)` 함수를 사용하며, value에는 기본값을 설정할 수 있다.

```javascript
import React from 'react';

const ThemeContext = React.createContext('light');

function App(props) {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar/>
    </ThemeContext.Provider>
  )
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton/>
    </div>
  )
}

function ThemedButton(props) {
  return (
    <ThemeContext.Consumer>
      {value => <button theme={value}/>}
    </ThemeContext.Consumer>
  )
}

export default App
```
> 상위 컴포넌트인 App에서 Provider를 설정했기 때문에, ToolBar 또는 ThemeButton에서 Consumer로 사용가능
<br>

- App에서 선언된 Context의 값을 'light' -> 'dark'로 변경

- 현재 ToolBar에는 Consumer가 없지만 사용하고 싶으면 써도 무방하다.

- Consumer 태그 안에서 `{value => 적용할 곳}` 방식으로 쓰면 된다.

- 만약 상위 레벨에 매칭되는 Provider가 없다면 기본값이 사용된다.
  - 현재 Context의 기본 값은 'light'
  - 기본값으로 undefined를 넣으면 기본값이 사용되지 않음!!!!
<br>
<hr>
<br>

## ✔ 문제점
- Context Provider가 적용된 컴포넌트가 재렌더링될 때마다 모든 하위 Consumer 컴포넌트도 렌더링된다.
  - 이유 : Provider의 value를 위한 새로운 객체가 매번 새롭게 생성되기 때문 
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/6246b527-2e17-49dd-8103-cfd4f92f3c44)
> value 객체가 새로 만들어지기 때문에 재렌더링 발생
<br>

- 현재 코드에서는 App이 Provider 컴포넌트인데, 해당 컴포넌트가 재렌더링이 된다면<br>
하위 컴포넌트중에서 Consumer를 사용한 ThemedButton 컴포넌트가 재렌더링 될 것이고<br>
그로 인해 해당 컴포넌트를 적용하고 있는 ToolBar 컴포넌트도 재렌더링 됨
<br>

**해결법**
- Provider의 value를 state를 사용하면 재렌더링을 막을 수 있다.

```javascript
function App(props) {
  const [value, setValue] = useState('dark');
  return (
    <ThemeContext.Provider value={value}>
      <Toolbar/>
    </ThemeContext.Provider>
  )
}
```
> useState를 사용하여 value를 할당
<br>
<hr>
<br>

## ✔ useContext
- Context API를 하위 컴포넌트에서 사용하려면 Consumer 태그를 무조건 사용해야 됐다.

- 그러나 useContext hook을 사용하면 편하게 값을 불러올 수 있다.

- 적용 전
```javascript
function ThemedButton(props) {
  return (
    <ThemeContext.Consumer>
      {value => <h1>{value}</h1>}
    </ThemeContext.Consumer>
  )
}
```
<br>

- 적용 후

```javascript
function ThemedButton(props) {
  const value = useContext(ThemeContext);
  return (
    <h1>
      {value}
    </h1>
  )
}
```
<br>

- 부모 컴포넌트에서 선언한 `const ThemeContext = React.createContext('light');`의 변수명을 useContext로 사용

- 해당 변수를 그대로 사용하면 된다.
<br>
<hr>
<br>

## ✔ 실습 예제
**ThemeContext.jsx**
```javascript
import React from 'react';

const ThemeContext = React.createContext();
ThemeContext.displayName = "크롬에서 확인 ThemeContext";

export default ThemeContext;
```
<br>

- Context를 생성

- displayName은 크롬 개발자 도구에서 React devtool 사용시 component 탭에서 Context의 이름 설정 가능

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/482b4b5b-96ea-4ff3-ba6e-c5ab9828c0ae)
<br>
<br>

**MainContext.jsx**
```javascript
import { useContext } from "react";
import ThemeContext from "./ThemeContext";

function MainContext(props) {
    const { theme, toggleTheme } = useContext(ThemeContext);

    return (
      <div
          style={{
            width: "100vw",
            height: "100vh",
            padding: "1.5rem",
            backgroundColor: theme == "light" ? "white" : "black",
            color: theme == "light" ? "black" : "white",
          }}
      >
          <p>Context API 테마 변경 테스트</p>
          <button onClick={toggleTheme}>테마 변경!</button>
      </div>
    )
}

export default MainContext;
```
<br>

- toggleTheme를 클릭할 때 마다 theme의 값을 변경되어 적용되는 css가 달라진다.

- toggleTheme는 부모 컴포넌트에 존재하는 함수로써, 클릭 시 부모 컴포넌트 쪽에서 실행이 된다.
  - 그로 인해 부모에서 theme 값이 변경되기 때문에 해당 컴포넌트에서도 바뀌는 것
<br>
<br>

**DarkOrLight.jsx**
```javascript
import { useState, useCallback } from "react";
import ThemeContext from "./ThemeContext";
import MainContext from "./MainContext";

function DarkOrLight(props) {
    const [theme, setTheme] = useState("light");

    const toggleTheme = useCallback(() => {
      if (theme == "light") {
          setTheme("dark");
      } else if (theme == "dark") {
          setTheme("light");
      }
    }, [theme]);

    return (
        <ThemeContext.Provider value={{ theme, toggleTheme }}>
            <MainContext/>
        </ThemeContext.Provider>
    )
}

export default DarkOrLight;
```
<br>

- theme의 state 값에 따라서 Provider의 값을 변경한다.

- toggleTheme는 함수로 사용되며, 하위 컴포넌트에서 해당 함수를 사용해서 callback을 받음
<br>
<br>

- 결과 확인

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/3edff004-588e-4c23-bd70-a78b2899a542)
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/9571df5a-3713-4d95-a5e0-69ba00daa3f7)
> 클릭 시 테마 변경됨
<br>
<br>

- MainContext.jsx에서 useContext를 Consumer로 변경 시 아래와 같이 사용 가능

```javascript
import { useContext } from "react";
import ThemeContext from "./ThemeContext";

function MainContext(props) {
    // const { theme, toggleTheme } = useContext(ThemeContext);

    return (
      <ThemeContext.Consumer>
        {({ theme, toggleTheme }) => 
        <div
            style={{
              width: "100vw",
              height: "100vh",
              padding: "1.5rem",
              backgroundColor: theme == "light" ? "white" : "black",
              color: theme == "light" ? "black" : "white",
            }}
        >
            <p>Context API 테마 변경 테스트</p>
            <button onClick={toggleTheme}>테마 변경!</button>
        </div>

        }
      </ThemeContext.Consumer>
    )
}

export default MainContext;
```
> value가 2개 이기 때문에 {({value, value}) => ///// } 형태로 사용
<br>
<hr>
<br>

**Reference**

[인프런 Inje Lee : 처음 만난 리액트(React)](https://www.inflearn.com/course/%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
