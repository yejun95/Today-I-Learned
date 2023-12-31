### 이클립스로 진행

- spring starter로 spring 프로젝트 생성

- src 경로에 frontend 폴더 만들고 vue 프로젝트 생성

- build하여 spring static 폴더에 index.html 생성 (Outputdir 설정)

- localhost:8080에서 vue 페이지 확인
<br>
<hr>
<br>

### ✔ 이클립스에서 vue를 지원하는가?
- 이클립스에서는 vue를 지원하지 않는다.

- 때문에 이클립스 안에서 .vue 파일에 접근하려 하면 이클립스에서 켜지는게 아니라
외부에서 실행이 된다.

**plugin**
- 마켓플레이스에 vue.js CodeMix를 다운받으면 vue 파일의 실행이 가능하지만
약 2주 무료 이후 유료로 전환되는 플러그인이다.
<br>

### ✔ 그렇다면 어떻게 이클립스로 vue를 실행할 수 있는가?
- 정답은 번들러에 있었다.

- 번들링을 통해 파일을 압축하는 과정에서 .vue 파일이 사라지고 html, js, css만 남게 되므로
이클립스에서도 vue를 보여줄 수 있는 것이다.
<br>
<br>

### ✔ 1개 포트로 개발
- 하나의 포트로 개발하면 vue js 개발 환경을 운영 환경과 똑같이 가져갈 수 있다는 장점이 있다.
  - ex) 프론트에서 로그인을 요청하고 로그인이 성공한다면 서버사이드에서 홈으로 리다이렉트를 시킬것이다.
  - ex) 두개의 포트로 띄운다면 요청한 프론트의 포트번호가 리다이렉트 될때는
   서버사이드의 포트번호로 바뀌어 리다이렉트 될것이다.

- 그러나 각각 개발하는 것이 back과 front 서로의 의존성을 분리할 수 있기 때문에
분리하는 것이 더 나아보인다.
