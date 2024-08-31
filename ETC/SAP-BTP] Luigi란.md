# Luigi가 무엇인가?
- Micro Frontend framework로써 기존 frontend를 독립적인 팀으로써 개발 가능하게 만들기 위해서
더 작고 단순한 덩어리로 만든다.
  - 즉, Micro Frontend간 통신이 가능하게 하고, 원하는 곳 어디서든 호출하여 사용 가능
<br>

- 또한 통신이 원활하게 실행되도록 하기 위해 라우팅, 네비게이션과 같은 설정이 가능하다.
  - luigi.config.js
<br>

- 가장 중요한 것은 Luigi가 기술에 구애받지 않는다는 것인데, 프론트엔드를 만들기 위해 
사실상 모든 툴킷(Angular, React, Vue 또는 UI5 포함)을 사용할 수 있다는 것을 의미

- Micro Frontend의 이점
  - 미래를 대비한 확장성 -> 새로운 컴포넌트 구성이 쉬움
  - 동일한 앱에서 여러 기술이 공존 -> 여러 Frontend 사용 가능
  - 독립된 팀에서 관리 가능 -> 동일 Frontend를 사용하는 사람끼리 Team 구성이 가능
  - 새로운 기능 및 버그 수정을 신속하게 구축 -> 변경된 부분만 신속 배포
  - 유지보수 비용 절감 -> 모듈 간 결합도가 낮아 코드 수정 용이
<br>

### Luigi terminology를 통한 Micro Frontend의 구조

![image](https://github.com/bjsystems/rnd/assets/121341413/cc08f0b2-2e72-48ed-bd67-1f1ac8a41f41)
<br>

- Luigi Core : Micro Frontend(일명 View)가 내장되는 "Main App"을 의미 -> 튜토리얼 에서는 React가 내장됨
  - luigi.config.js : luigi core api를 사용하기 위한 설정 파일
<br>

- Luigi Client : Micro Frontend와 관련된 루이지 옵션을 나타내며, Micro Frontend간 통신을 가능하게 함
  - LifeCycle, Listener, uxManager 등과 같은 api 제공
  - 즉 Micro Frontend와 메인 애플리케이션(Luigi Core 모듈) 간의 메시지 전송을 담당
  - 메시지 전송은 `sendCustomMessage` api를 통해 id 값을 지정하여 소통 가능 - Luigi Client > Luigi Core 메시지 전송

![image](https://github.com/bjsystems/rnd/assets/121341413/b1aee431-d600-4b59-8f2d-6c7d7f1a9450)
> home.js
set-language라는 id값을 이용하여 콜백으로 받은 값을 넘겨준다.
<br>

- 그러면 luigi-config.js에서 Listerners를 통해 콜백으로 데이터를 받음

![image](https://github.com/bjsystems/rnd/assets/121341413/bb71c207-5984-4156-9935-94cc1da1c5be)
<br>

- 반대로 Luigi Core > Luigi Client로 메시지 전송 방법
  - luigi-config.js에서 Luigi.customMessages().send or Luigi.customMessages().sendToAll 사용
  - ref : https://docs.luigi-project.io/docs/luigi-core-api?section=custommessages
<br>

- React의 home.js에서 해당 메시지 받아보기
  - luigi-config.js 에서 customMessage 설정
  - 이후 home.js에서 addCustommessageListener를 통해 받음
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/c2b728e1-29ac-4639-9ad2-df5bd349d63b)
> microfrontend id 설정

![image](https://github.com/bjsystems/rnd/assets/121341413/f14e8764-cf61-4f91-b02c-91055d61d516)
> 1번 microfrontend로 message 전송
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/ab902dc1-0b8f-4f0a-aa5d-73c6f9b96fac)
> 메시지를 변수에 담고 출력
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/c0f98f82-2ce7-44a6-b209-2b5c58ff5ce5)
> 이상한 랜덤값이 등장 - 추가 확인 필요
<br>

- 💡 즉, luigi란 Core와 Client 각각의 API를 사용하여 Micro Frontend 간의 설정을 손쉽게 가능하게 하는 것
<br>
<hr>
<br>

**Reference**<br>

[Luigi 공식문서](https://docs.luigi-project.io/docs/getting-started)<br>
[Luigi github](https://github.com/SAP/luigi)<br>
<br>
<hr>
<br>

# 설정 파일
## ✔ public
![image](https://github.com/bjsystems/rnd/assets/121341413/87b31e23-c3b5-45fd-ba74-8b0c6d789d5d)
<br>

➡ **index.html**
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Luigi</title>
    <meta
      name="viewport"
      content="width=device-width, user-scalable=no, initial-scale=1, maximum-scale=1, minimum-scale=1"
    />
    <link rel="stylesheet" href="/luigi-core/luigi.css" />
  </head>

  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <script src="/luigi-core/luigi.js"></script>
    <script src="/luigi-config.js"></script>
  </body>
</html>

```

- 앱의 최초 시작점이 되는 파일이며, 이곳에 luigi core를 주입한다.
  - 이후 sampleapp.html로 실행시킴

- Micro Frontend 중 하나는 Core가 주입되어 Micro Frontend간 통신을 가능하게 해야한다.
<br>
<br>

➡ **luigi-config.js**
- Micro Frontend 간 통신 및 관련 설정이 가능한 곳

- Luigi terminology 그림에서 Micro Frontend 사이에 위치한 luigi client에 해당된다.

![image](https://github.com/bjsystems/rnd/assets/121341413/fd0c4bc4-9c1b-4c0c-a907-204942701f50)
> pathSegment : localhost:3000/{pathSegment}
단, 앱 진입점이 되는 Micro Frontend의 경우 index.js의 basename과 Route path를 따름
sappleapp.html#/{basename}/{path}

- index.js

```javascript
...

  return (
    <ThemeProvider>
      <React.StrictMode>
        <Router basename="microfrontend">
          <Routes>
            <Route path="/home" element={<Home localeDict={dict[currentLocale]} currentLocale={currentLocale} />} />
            <Route path="/products" element={<Products localeDict={dict[currentLocale]} />} />
            <Route path="/products/:id" element={<ProductDetail localeDict={dict[currentLocale]} />} />
          </Routes>
        </Router>
      </React.StrictMode>
    </ThemeProvider>
  );
```
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/21d0f53f-36c7-479d-9d6a-fc8bead2be73)
<br>

- `myTranslationProvider`

![image](https://github.com/bjsystems/rnd/assets/121341413/51a64ea6-eef3-4df2-8a75-22b35ba2e4d0)
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/856e48a7-f08c-49d6-a17c-eb45b6b4b41d)
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/5397e2a6-8e41-48bf-88ba-2707a5337bea)
<br>
<br>

➡ **sampleapp.html**
```html
<!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="utf-8" />
      <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
      <meta name="viewport" content="width=device-width, initial-scale=1" />
      <title>React App</title>
    </head>
    <body>
      <noscript>You need to enable JavaScript to run this app.</noscript>
      <div id="root"></div>
    </body>
  </html>
```
<br>

- index.html의 설정을 토대로 해당 파일을 앱의 시작점으로 로드한다.

- index.js와 luigi-core의 설정을 바탕으로 앱의 진입점도 Micro Frontend로 만들어줘야하기 때문
<br>
<hr>
<br>

## ✔ src
- mock data 및 page 파일

![image](https://github.com/bjsystems/rnd/assets/121341413/f47faca4-c5f4-42da-b591-9512f9ecc3cc)
<br>

➡ **index.js**
- 웹팩에서 entry point 확인하는 파일

- luigi가 주입된 Micro Frontend에 대한 라우팅 설정 및 language 설정 진행

![image](https://github.com/bjsystems/rnd/assets/121341413/f143024c-2dc6-4656-930d-e4841fd13711)
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/656c3db5-6276-4fae-b76b-16d6c5ea3bff)
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/5840839b-96f4-4f6f-b46a-12da586e9b53)
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/dc11819b-1b67-445d-87c6-1f816cd36c7b)
<br>
<hr>
<br>

## ✔ webpack.config.js
- webpack이란 브라우저에서 js, css, jpg등의 파일들을 묶어서 사용하기 위함
  - 해당 행위를 번들링이라고 한다.
<br>

- 결과물에 대한 설정

![image](https://github.com/bjsystems/rnd/assets/121341413/c2f8a0ee-89e5-4145-83de-225e46fb5501)
<br>

- filename : production에서는 캐시 무효화를 위해 콘텐츠 해시를 사용
  - 빌드때 마다 해시값이 들어간 파일명으로 생성됨
  - Ex) 빌드시마다 css파일의 내용이 수정되어도 항상 동일한 파일명(main.css)으로 생성되기 때문에 브라우저에서 동일한 파일로 인식하여 캐싱된 css파일을 불러오기 때문
<br>

- chunkFilename : 웹팩이 코드 스플리팅을 통해 생성한 청크 파일의 이름을 설정할 수 있다. chunkFilename 옵션을 설정하지 않으면, filename 옵션을 사용
  - 코드 스플리팅 : 싱글페이지 앱이더라도 해당 라우트를 방문했을 때만 관련된 모듈들을 로딩
  - 청크 : 하나의 덩어리라는 뜻으로, 코드 스플리팅 시 생성되는 자바스크립트 파일 조각을 의미
  - 즉, 코드 스플리팅을 하면 관련 모듈만 로딩해야 하는데, 이때 파일 조각이 생기므로 해당 네이밍을 결정하기 위한 것
<br>

- globalObejct : Node.js의 전역 객체, this를 통해 전역 객체 접근 가능
<br>

- 플러그인 설정

![image](https://github.com/bjsystems/rnd/assets/121341413/c9b77f3a-a90b-47ac-8ab2-c4a44833fa7d)
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/557acae8-23fe-4f82-ba2c-29bbab41c729)
<br>

- React를 웹펙에서 인식시키기 위한 작업

![image](https://github.com/bjsystems/rnd/assets/121341413/a449f913-1b5d-4378-a557-55ceba4a8237)
> loader : 웹팩이 웹 애플리케이션을 해석할 때, javascript 파일이 아닌 웹 자원(HTML, CSS, Images, 폰트 등)들을
변환할 수 있도록 도와주는 속성

- 코드 예시

```javascript
// app.js

import './common.css';

console.log('css loaded');
```
<br>

```css
/* common.css */

p {
  color: blue;
}
```
<br>

```javascript
// webpack.config.js

module.exports = {
  entry: './app.js',
  output: {
    filename: 'bundle.js'
  }
}
```
<br>

- 위와 같이 css 모듈을 import하여 loader없이 사용하면 아래와 같은 에러 발생

![image](https://github.com/bjsystems/rnd/assets/121341413/bae09d2a-9c34-4858-9a23-ea2f79808829)
> app.js에서 import한 common.css 파일을 해석하기 위해 적절한 코드를 추가해달라는 뜻
<br>

- devServer

![image](https://github.com/bjsystems/rnd/assets/121341413/08b50072-d8e6-4f09-859b-70886f8cff8d)
<br>

- devtool : source map 지정

![image](https://github.com/bjsystems/rnd/assets/121341413/c6813747-71a0-41c1-8c52-b7a5fd7665f8)
<br>

- 웹팩을 실행하여 여러 모듈을 하나의 번들링 파일로 만들 때 자바스크립트에서 에러가 발생하면 
브라우저의 콘솔에는 번들링된 하나의 파일 첫째줄에서 에러가 떴다고 알려주기 때문에 
어떤 모듈의 몇 번째 줄에서 에러가 떴는지 정확히 알기 쉽지 않다.

- 즉, 에러를 트래킹하기 위해 웹팩에서는 source map이라는 모듈을 사용한다.

- `inline-source-map` : 에러코드가 표시될 때 브라우저 콘솔에서 원본 코드의 구체적인 줄번호를 알려줌
<br>

- performance

![image](https://github.com/bjsystems/rnd/assets/121341413/4e6221ad-dd0b-4e51-a883-fee4840e53ff)
