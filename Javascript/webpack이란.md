## webpack이란 무엇인가?

- 브라우저에서 자바스크립트 파일들을 묶어서(**번들링**) 사용하기 위함이며,<br>
어떠한 자원(js, css, png, jpg 등)이나 자산 등을 전송, 구축, 패키징이 가능하게 만드는 도구이다.

- 번들링 : 애플리케이션을 구성하는 자원(HTML, CSS, JS, Image)등을 모두 각각의 모듈로 보고<br>
이를 조합해서 하나의 결과물을 만드는 도구
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/c3a6e60b-2399-4c47-bd00-22e790a8ff4b)
<br>
<hr>
<br>

### ✔ 탄생 배경

**자바스크립트 변수의 유효 범위로 인한 중첩 가능성 증대**
<br>

```javascript
<!-- index.html -->
<html>
  <head>
    <!-- ... -->
  </head>
  <body>
    <!-- ... -->
    <script src="./app.js"></script>
    <script src="./main.js"></script>
    <script>
      getNum(); // 20
    </script>
  </body>
</html>
```
<br>

```javascript
// app.js
var num = 10;
function getNum() {
  console.log(num);
}
```

```javascript
// main.js
var num = 20;
function getNum() {
  console.log(num);
}
```
<br>

- 이 경우 `main.js`에서 선언된 num을 사용하게 되고, 변수가 서로 겹치게 되는 상황이 발생한다.

- 개별 파일로는 문제가 없겠지만 html파일에 모아놓으면은 스코프 단위에서 발생하는 이슈가 생기는 것이다.

- 이를 웹팩의 모듈 번들링을 통해서 해결. 
<br>

**사용하지 않는 코드 관리**
- 미사용 코드를 번들 파일에서 제외하는 tree shaking이라는 개념이 rollup 모듈 번들러를 통해 대중화<br>
[tree shaking이란?](https://github.com/yejun95/Today-I-Learn/blob/master/Javascript/TreeShaking.md)
<br>

**Dynamic Loading & Lazy Loading 미지원을 해결** 
- 성능 향상을 위해 필요한 모듈만 동적으로 로딩해준다. (웹페이지의 전체 코드를 로딩하지 않음)<br>
[Dynamic Loading과 Lazy Loading의 차이]()
<br>
<hr>
<br>

**참고 자료**<br>
[Jaedeok KIm : What is Webpack?](https://medium.com/atant/what-is-webpack-252513b3c1ad)<br>
[백산2 : 웹팩(Webpack)이란?](https://demain18-blog.tistory.com/72)<br>
