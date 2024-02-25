## JSX란 무엇인가?
- JS는 JavaScript를 나타낸다.

- JSX : A syntax extenstion to JavaScript
  - javascript의 확장 문법이라는 뜻이다.
<br>

- 어떻게 확장 시킨 것인가?
  - Javascript + XML / HTML
  - ex) `const element = <h1>Hello, world!</h1>;
  - javascript와 HTML이 모두 표현되어 있는 것을 볼 수 있다.
<br>
<hr>
<br>

### ✔ JSX의 역할
- JSX의 코드를 JavaScript 코드로 변환한다.

- 이 때 React의 createElement라는 함수가 이용된다.
  - 즉, JSX 코드를 사용하면 createElement 함수가 자동 적용된다.
  Javascript 코드로 변환해야 되기 때문
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/34a120cc-59ca-4b86-b849-9d6ed3634055)
<br>
<br>

**JSX 사용한 코드**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/f265285d-9aad-4ae6-b4d7-f43af607f19e)
<br>
<br>

**JSX를 사용하지 않는 코드**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/f648dcb2-9433-4715-9e34-b95f70b463d0)
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/4bfabedd-904c-44be-a9ea-99e16d70f866)
<br>
<br>

- JSX를 굳이 사용하지 않고 전부 React.createElement 함수를 사용하여 만들어도 되지만 JSX를 사용하는 것이 좀 더 간결하다는 것을 위에 예시로 볼 수 있다.
<br>
<hr>
<br>

### ✔JSX 장점
**코드가 간결해진다.**
- 가독성이 향상된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/4314a6b3-c196-49b7-b596-5a252fb55857)
<br>
<br>

**Injection Attacks 방어**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/f068ba57-d23a-4057-9d56-f9f0ea8fcece)

- `title` 이라는 변수에 잠재적으로 보안 위협이 되는 코드가 삽입됐다고 하자.

- 그 아래 JSX 코드에서는 괄호를 사용하여 `title` 변수를 사용한다.

- 기본적으로 리액트 돔은 렌더링 전에 임베딩된 값을 모두 문자열로 변환한다.
  - 즉, 명시적으로 선언되지 않는 값은 괄호 사이에 들어가지 않는다.
<br>

- 💡 XSS 공격을 방어할 수 있다.
<br>
<hr>
<br>

### ✔ 사용법
- `{}` 괄호에 JavaScript 변수나 함수를 넣어 사용한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/eeee5b8c-4a1f-4bf5-8a5e-d80148675728)
<br>
<br>

- JSX에서 중괄호{}를 사용하면 무조건 JavaScript 코드가 들어간다고 생각하자.
<br>
<br>

**태그 속성에 값을 넣는 법**

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/671bf952-ff17-462a-b97a-bbe39d6c1173)
<br>
<br>

**자식(children) 정의하는 법**

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/741a4e98-becd-4fb5-9acb-03b7915983d0)
<br>
<hr>
<br>

### ✔ JSX 코드 실습
- chapter_03 폴더안에 `Book.jsx`, `LIbrary.jsx` 생성

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/783cbac3-3149-4478-abd8-b1f8136a94b8)
<br>
<br>

```jsx
// Book

import React from 'react';

function Book(props) {
  return (
    <div>
      <h1>{`이 책의 이름은 ${props.name}입니다.`}</h1>
      <h2>{`이 책은 총 ${props.numOfPage}페이지로 이뤄져 있습니다.`}</h2>
    </div>
  );
}

export default Book;
```
<br>
<br>

```jsx
// Library

import React from 'react';
import Book from './Book';

function Library(props) {
  return (
    <div>
      <Book name="처음 만난 파이썬" numOfPage={300}/>
      <Book name="처음 만난 AWS" numOfPage={400}/>
      <Book name="처음 만난 리액트" numOfPage={500}/>
    </div>
  )
}

export default Library;
```
<br>

- 이후 렌더링을 위해 `index.js` 수정
  - <App/> 태그를 지우고 <Library/> 태그 추가
```javascript
// index.js

import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

import Library from './chapter_03/Library';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <Library/>
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```
<br>
<br>

- localhost:3000 에서 화면 확인

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/e868765b-bede-48e4-ba1c-7d2343938da0)
<br>
<br>

- 만약 JSX를 사용하지 않고 Book 컴포넌트를 만들면 아래와 같이 된다.
  - 기존 것이랑 비교하면 매우 가독성이 좋지 않은 것을 볼 수 있다.
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/fbb619b3-8b94-47d4-b0b0-3eb8d5b3d413)
<br>
<br>

- 💡 주의 사항 : jsx에서 `return`문을 사용할 때 소괄호() 로 사용해야 한다.
<br>
<hr>
<br>

**Reference**<br>

[인프런 Inje Lee : 처음 만난 리액트(React)](https://www.inflearn.com/course/lecture?courseSlug=%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8&unitId=113264&tab=curriculum)

