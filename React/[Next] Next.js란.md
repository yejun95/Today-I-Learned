# Next.js란 무엇인가?
- Next.js는 리액트를 위해 만든 오픈소스 자바스크립트 웹 프레임워크이다.

- 리액트에 존재하지 않는 서버 사이드 렌더링(SSR), 정적 사이트 생성(SSG), 증분 정적 재생성(ISR) 등과 같은 기능을 제공

- 프론트엔드는 리액트로 구성하고 Next.js로 서버 사이드 렌더링을 하는 것
<br>
<hr>
<br>

## ✔️ 탄생 배경
- 리액트는 클라이언드 사이드 렌더링(CSR)이기 때문
  - 서버에서 완성된 HTML파일을 보내주는 것이 아님
  - 처음에는 빈 HTML파일을 보여주고, 그 이후에 resource들을 다운 받아 보여줌
<br>

- 위와 같은 단점으로 인하여 초기 로딩 속도가 느리고, 검색 엔진 최적화 측면에서 불리

- 이러한 문제점을 보완하기 위하여 Next.js가 탄생
<br>
<hr>
<br>

## ✔️ 사용 이유
- 탄생 배경과 같은 맥락이지만 다시 한 번 사용하는 이유는 아래와 같다.

- Next.js는 React를 기반으로 한 프레임워크로, 서버 사이드 렌더링(SSR)을 지원하고 SEO(검색 엔진 최적화)에 유리하기 때문에 많이 사용
 
- Next.js는 사용자가 페이지에 접속할 때 서버에서 SSR 방식으로 HTML을 먼저 렌더링한 후, 브라우저가 JavaScript를 다운로드하고 React를 실행하는 방식으로 작동

- 이를 통해 SEO에 유리한 구조를 만들 수 있다.

- 또한, 사용자가 페이지 간 이동할 때는 클라이언트 사이드 렌더링(CSR) 방식으로 브라우저에서 처리되므로, 단일 페이지 애플리케이션(SPA)의 장점도 유지 가능
<br>
<hr>
<br>

## ✔️ 작동 방식
- 사용자가 초기에 Server에 페이지 접속을 요청한 경우 SSR방식으로 렌더링 될 HTML을 보낸다.

- 브라우저에서 JavaScript를 다운로드 받고 React를 실행한다.

- 사용자가 페이지와 상호작용을 하며 다른 페이지로 이동할 경우 CSR 방식으로 Server가 아닌 브라우저에서 처리한다.
<br>
<hr>
<br>

## ✔️ Next.js 준비하기
- Node.js와 npm 설치 필요

- 프로젝트 기본 구조 설정

```
npx create-next-app <app-name>
```
<br>

- npx 명령어를 통해 설치하면 패키지 구조는 아래와 같다.

![image](https://github.com/user-attachments/assets/235be010-14cc-4cbe-93db-40d2bd9eeb42)
> typescript를 적용하여 init을 함
<br>

- 이후 프로젝트를 실행하면 아래와 같은 화면이 등장

```
npm run dev
```

![image](https://github.com/user-attachments/assets/e47a7302-c014-4c43-a422-dbad5cb11006)
<br>
<hr>
<br>

**Reference**<br>

[한빛미디어 : 리액트에서 Next.js로, 넥스트JS의 특장점과 빠르게 시작하는 법 알아보기](https://www.hanbit.co.kr/channel/category/category_view.html?cms_code=CMS7641364152)<br>
[woony.log : Next.js란?](https://velog.io/@codns1223/Nextjs-Next.js%EB%9E%80)<br>
[그래도 해야지 : Next.js란 무엇이고 왜 사용하는가](https://subtlething.tistory.com/115)<br>
