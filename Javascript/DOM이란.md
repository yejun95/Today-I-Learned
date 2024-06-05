# DOM이란 무엇인가?
- Document Object Model

- 스크립트 언어로 HTML 문서를 제어하기 위해 문서를 객체화 한 것이다.

- 애초에 Javascript는 HTML 문서를 제어하기 위해 개발된 언어
<br>
<hr>
<br>

## ✔ 제어 방법
- 브라우저 안에는 웹문서를 해석하는 렌더링 엔진이 있다.

- 브라우저로 HTML 파일을 열면 이 렌더링 엔진이 HTML로 만든 문서를 한줄씩 해석

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/cafb1046-9777-4d31-b757-e6daf6ddf1c2)
<br>

- 해석이 끝나면 이러한 문서를 객체화하여 Javascript로 접근할 수 있게 만드는 것이다.
  - 이때 문서가 객체화 된 것을 DOM으로 부르는 것이다.
<br>

- 이때 DOM은 트리 구조를 가지고 있다.
  - html > head > body > div 등 타고 내려옴

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/c749bd78-04a7-4943-9b19-0b41d55cea79)
> DOM 트리, 각각 요소는 node라고함
<br>

- 이제 각각에 node에 접근해서 제어가 가능해진다.
  - 해당 부분은 [BOM]() 에서 다룬다.
<br>

- 또한 [웹페이지 빌드 과정 : CRP]()도 알아두면 좋을 것이다.
<br>
<hr>
<br>

**Reference**<br>

[짐코딩 : 프론트엔드 날개달기](https://www.inflearn.com/course/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EB%82%A0%EA%B0%9C%EB%8B%AC%EA%B8%B0/dashboard)
