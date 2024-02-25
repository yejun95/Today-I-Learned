## Props란?
- Props의 prop은 property를 뜻하며, 이는 자바스크립트의 속성을 말한다.

- 또한 컴포넌트에 전달할 다양한 정보를 담고 있는 자바스크립트 객체라고 보면 된다.

- 이렇게 데이터가 담긴 props를 통해서 원하는 데이터를 전달하는 것이다.
<br>
<hr>
<br>

### ✔ 특징
- readonly 값이므로 변경할 수 없다.

- 리액트 공식문서에서는 아래와 같이 설명하고 있다.
  - 모든 리액트 컴포넌트는 그들의 Props에 관해서는 Pure 함수 같은 역할을 해야 한다.
  - 모든 리액트 컴포넌트는 Props를 직접 바꿀 수 없고, 같은 Props에 대해서는 항상 같은 결과를 보여줘야 한다.
  - Pure 함수란 동일한 값을 입력할 때, 항상 같은 출력 값이 나온다는 뜻
<br>
<hr>
<br>

### ✔ 사용법
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/4da1c532-9df4-4f10-8cf9-8819220b70ad)

- Profile이라는 컴포넌트에 속성으로 Props를 보낸다.

- 문자열은 `""`를 사용하지만, 그 외 자바스크립트나 정수 같은 경우 `{}` 중괄호를 사용해야한다.
<br>
<br>

**jsx 문법을 사용하지 않는 경우**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/29d60a80-0a74-456a-a7e2-1ddfaae4d3ac)

- 위와 같은 순서대로 정의하면 된다.

- 아래 그림은 위에서 보여주었던 코드에서 jsx 문법을 제거한 것 
  - type, props, children 순서로 정의되었다.
  - 하위 컴포넌트가 없기 때문에 childrun은 null 로 정의한 것

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/c62eb8ac-b28d-4b42-9c9c-b64d6f9a7af0)
<br>
<hr>
<br>

**Reference**<br>

[인프런 Inje Lee : 처음 만난 리액트(React)](https://www.inflearn.com/course/%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
