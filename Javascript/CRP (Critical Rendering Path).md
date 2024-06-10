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
<br>

**2. CSSOM 트리 만들기**
- DOM과 관련된 스타일의 Object 표현

- HTML을 파싱하다가 CSS 링크를 만나면 CSS 파일을 요청해서 받아오게 된다.

- 받아온 CSS 파일은 HTML을 파싱한 것과 유사한 과정을 거쳐서 역시 트리 형태의 CSSOM으로 만들어진다
<br>

**3. JavaScript 실행**
- JavaScript 파일을 만나면 해당 파일을 받아와서 실행할 때까지 파싱이 멈춘다.
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
