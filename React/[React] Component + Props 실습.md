## 간단한 Component와 Props 실습
### ✔ 컴포넌트 구조
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/1d1f8390-f316-40f1-9d39-6209e2e49d8f)
<br>

- chapter_03은 무시해도 됨
<br>
<hr>
<br>

### ✔ index.js
- CommentList만 렌더링함

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

import Library from './chapter_03/Library';
import CommentList from './chapter_05/CommentList'

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <CommentList/>
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```
<br>
<hr>
<br>

### ✔ CommentList
- 화면에 출력될 컴포넌트

```javascript
import React from 'react'
import Comment from './Comment'

const comments = [
  {
    name: "이인제",
    comment: "안녕하세요 소플입니다."
  },
  {
    name: "r황현우",
    comment: "안녕하세요 현우니다."
  },
  {
    name: "유건의",
    comment: "안녕하세요 건의입니다."
  },
]

function CommentList() {
  return (
    <>
      <div>
        {comments.map(d => {
          return (
            <Comment name={d.name} comment={d.comment}/>
          )
        })}
      </div>
    </>
  )
}

export default CommentList
```
<br>

- Vue.js처럼 v-for과 같은 디렉티브가 없기 때문에 `{}` 중괄호 안에서 map 함수를 직접 사용한다.

- Comment라는 컴포넌트에 Props로 name과 comment를 보낸다.
 
- api를 사용하지 않고 변수에 배열 객체 3개를 선언한다.
<br>
<hr>
<br>

### ✔ Comment
- 자식으로 사용될 컴포넌트이다.

- CommentList에서 Props로 보낸 name과 comment를 적용하는 컴포넌트이다.

- CSS는 styles 변수로 따로 선언하였다.

```javascript
import React from 'react'

const styles = {
  wrapper: {
    margin: 8,
    padding: 8,
    display: "flex",
    flexDirection: "row",
    border: "1px solid grey",
    borderRadius: 16,
  },
  imageContainer: {},
  image: {
    width: 50,
    heihgt: 50,
    borderRadius: 25,
  },
  contentContainer: {
    marginLeft: 8,
    display: "flex",
    flexDirection: "column",
    justifyContent: "center",
  },
  nameText: {
    color: "black",
    fonrtSize: 16,
    fontWight: "bold",
  },
  commentText: {
    color: "black",
    fontSize: 16,
  }
}

function Comment(props) {
  return (
    <div style={styles.wrapper}>
      <div style={styles.imageContainer}>
        <img
          src="https://upload.wikimedia.org/wikipedia/commons/8/89/Portrait_Placeholder.png"
          style={styles.image}
        />
      </div>

      <div style={styles.contentContainer}>
        <span style={styles.nameText}>{props.name}</span>
        <span style={styles.commentText}>{props.comment}</span>
      </div>
    </div>
  )
}

export default Comment;
```

- CSS로 레이아웃을 잡는다.

- 부모 컴포넌트인 CommentList에서 넘긴 Props를 `props.name`, `props.comment`로 받아서 사용한다.
<br>
<hr>
<br>

### ✔ 결과 화면
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/6a70cdf8-be7c-4713-8dc2-5636e794de73)
<br>
<hr>
<br>

**Reference**<br>

[인프런 Inje Lee : 처음 만난 리액트(React)](https://www.inflearn.com/course/%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
