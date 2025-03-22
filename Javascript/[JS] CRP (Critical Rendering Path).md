# CRP
- Critical Rendering Path

- 브라우저가 서버로부터 HTML 응답을 받아 화면을 그리기 위해 실행하는 과정이다.
<br>
<hr>
<br>

## ✔ 실행 과정
**1. HTML 파싱**
- 브라우저 주소창에 url을 입력하여 요청을 보내면 서버로부터 HTML 문서를 받아오게 된다.

- 이걸 파싱하기 시작하면서 브라우저가 데이터를 화면에 그리는 과정이 시작

**1. DOM 트리 만들기**
- HTML 페이지의 Object 표현

- html로부터 시작되어, 페이지의 각 element, text에 대한 노드가 만들어진다.

- 아래 코드를 통해 실제 DOM트리를 확인해보자.

```html
<html>
<head>
  <title>Understanding the Critical Rendering Path</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>Understanding the Critical Rendering Path</h1>
  </header>
  <main>
    <h2>Introduction</h2>
    <p>Lorem ipsum dolor sit amet</p>
  </main>
  <footer>
    <small>Copyright 2017</small>
  </footer>
</body>
</html>
```
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/0b4fcef8-7b96-4cbc-8457-2cd10f3704a1)
> HTML은 부분적으로 실행될 수 있으며, 페이지에 내용이 표시되기 위해 문서 전체를 로드할 필요가 없다.
<br>

**2. CSSOM 트리 만들기**
- DOM과 관련된 스타일의 Object 표현

- HTML을 파싱하다가 CSS 링크를 만나면 CSS 파일을 요청해서 받아오게 된다.

- 받아온 CSS 파일은 HTML을 파싱한 것과 유사한 과정을 거쳐서 역시 트리 형태의 CSSOM으로 만들어진다

```css
body { font-size: 18px; }

header { color: plum; }
h1 { font-size: 28px; }

main { color: firebrick; }
h2 { font-size: 20px; }

footer { display: none; }
```

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/ac7c654c-a980-4b3f-854c-55b259af78b1)
<br>

- CSS는 렌더링을 차단하는 리소스로, 완전히 파싱되지 않으면 Render 트리를 구성할 수 없다.

- HTML과 달리 CSS는 상속된 계단식 특성때문에 부분적으로 실행될 수 없으며, 문서의 뒷부분에 정의된 스타일은 이전에 정의된 스타일을 덮어쓰게된다.
  - 이 때문에 CSS가 전역으로 설정되어있는 것을 지역에서 덮어쓸 수 있는 것
  - CSS전체가 파싱되기 전에 먼저 스타일 시트에 정의한 CSS스타일을 사용하면, 잘못된 CSS가 적용되는 상황이 발생할 수 있다.
  - 하지만, 완전히 파싱되기 전까지 렌더링을 차단함으로써 이와 같은 일이 일어나지 않는다.
<br>

**3. JavaScript 실행**
- JavaScript는 파서 차단 리소스로 간주된다.

- 이것은 HTML 문서 자체의 파싱이 JavaScript에 의해 차단된다는 뜻이다.

- 파서가 내부 태그이든 외부 태그이든 <script> 태그에 도달하면, (외부 태그라면, 외부 스크립트를 가져오고) 스크립트를 실행한다.
<br>

**4. Render 트리 만들기**
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/4c552aa5-921d-4d13-803f-53f0028f8f75)
<br>

- DOM 과 CSSOM은 문서의 각기 다른 측면을 표현하는 서로 독립적인 객체이다.

- 하나는 콘텐츠를 설명하고 하나는 문서에 적용되어야 하는 스타일 규칙을 설명한다.

- 이 두가지를 병합하여 브라우저가 어떻게 화면에 픽셀을 렌더링하는지에 대한 정보를 가진 Render 트리를 만든다.

**5. 레이아웃 생성하기**
- 지금까지 표시할 노드와 해당 노드의 스타일을 계산했다.

- 하지만 기기의 뷰포트 내에서 이러한 노드의 정확한 위치와 크기를 계산하지 않았다.

- 이를 계산하는 것이 레이아웃 단계
<br>

**6.페인팅**
- 렌더링 트리의 각 노드를 화면의 실제 픽셀로 변환한다.

- 텍스트, 색, 이미지, 그림자 등 요소의 모든 시각적 부분을 그리는 작업을 포함한다.
<br>
<hr>
<br>

**Reference**<br>

[Minsu Kang : Critical Rendering Path](https://velog.io/@mu1616/Critical-Rendering-Path)<br>
[Front-end developer WONISM : Critical Rendering Path란?](https://wonism.github.io/critical-rendering-path/)
