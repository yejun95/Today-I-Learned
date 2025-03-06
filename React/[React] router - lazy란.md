# router - lazy란
- lazy Loading이란 필요한 코드를 비동기적으로 로딩하는 방법을 말한다.

- 즉, 라우터에 사용 시 그때 그때 필요한 부분만 로딩하여 렌더링하게 만드는 방법이다.

- 이는 코드 스플리팅(Code Splitting)이라고도 한다.
  - 하나로 크게 뭉쳐진 코드를 조각 조각 나누는 작업
  - 나누어진 부분을 필요한 시점에 동적으로 로딩하는 기술
  - 즉, 사용자가 필요한 코드만 비동기적으로 로딩하는 방법
<br>
<hr>
<br>

## ✔ React에서 Lazy Loading이 필요한 이유
- 리액트는 SPA(Single Page Application)으로써 빌드가 되면 하나의 JS 파일로 번들링이 된다.

- 이렇게 하나의 JS로 번들링된 페이지로 사용자가 진입하게 된다면 최초에 모든 페이지에 대한<br>
정보를 불러오게 되는데, 이는 초기 로딩을 느리게 만든다.

- 페이지의 느린 로딩은 사용자에게 부정적인 경험을 줄 수 있으므로 당연하게 피해야 한다.
<br>
<hr>
<br>

## ✔ Lazy Loading 사용법
- React.lazy의 사용법을 보기 이전에 위에서 언급한 '모든 페이지에 대한 정보를 불러온다'는 구문을 다시 살펴보자.
 
- 만일 내 프로젝트의 페이지가 `/, /profile, /search` 와 같이 나뉘어져있다면?<<br>
사용자가 `/profile` 페이지에 들어가있는 동안 `/, /search` 페이지들의 정보는 사용자에게 필요하지 않을 확률이 높다.

- 그렇다면? 이러한 `/, /profile, /search` 를 분리하여 지금 사용자가 필요한 페이지에 대한 정보만 불러올 수 있도록 하면 성능 개선을 기대할 수 있다.
<br>

### 코드 스플리팅 적용 전 Router
```js
import Home from './routes/home.tsx';
 
const router = createBrowserRouter([
  {
    path: '/',
    element: <Home />,
  },
]);
  
function App() {
  return (
    <RouterProvider router={router} />
  );
}
```
> 코드 스플리팅 전의 import는 별 다를 것이 없다.
<br>

### 코드 스플리팅 적용 후 Router의 import
```js
import { lazy } from 'react';

const Home = lazy(() => import('./routes/home.tsx'));
```
> lazy가 적용된 import 형식
<br>

**💡 그리고 React.lazy가 적용된 컴포넌트는 React.Suspense 컴포넌트 안에 렌더링해야 한다.**
- [Suspense란](https://github.com/yejun95/Today-I-Learned/blob/master/React/%5BReact%5D%20Suspense%EB%9E%80.md)
<br>

```js
import { lazy, Suspense } from 'react';
 
const Home = lazy(() => import('./routes/home.tsx'));
 
function App() {
  return (
    <Suspense>
      <RouterProvider router={router} />
    </Suspense>
  );
}
```
<br>

- 그리고 React.Suspense는 fallback이라는 속성을 사용할 수 있는데<br>
Suspense로 감싼 lazy 컴포넌트들이 로딩되는 동안 대신해 보여줄 콘텐츠를 설정할 수 있다.

```js
import LoadingSpinner from './components/loading-spinner.tsx';
 
...
...

function App() {
  return (
    <Suspense fallback={<LoadingSpinner />}>
      <RouterProvider router={router} />
    </Suspense>
  );
}
```
> 현재는 Spinner 컴포넌트를 fallback으로 사용하고 있다.
<br>

- 여기에서 LoadingSpinner도 import되는 컴포넌트이지만<br>
가볍고 fallback으로 이용되기 때문에 굳이 코드 스플리팅을 적용해주지 않고 직접 불러와서 사용하는 편이 낫다.
<br>
<hr>
<br>

## ✔ Lazy와 Suspense를 적용한 예시 코드 
```js
import { useEffect, useState, lazy, Suspense } from 'react';
import { RouterProvider, createBrowserRouter } from 'react-router-dom';
import { auth } from './firebase.ts';
import LoadingSpinner from './components/loading-spinner.tsx';
import * as S from './styles/global.ts';
 
const ProtectedRoute = lazy(() => import('./components/protected-route.tsx'));
const Home = lazy(() => import('./routes/home.tsx'));
const Profile = lazy(() => import('./routes/profile.tsx'));
const SearchResult = lazy(() => import('./routes/search-result.tsx'));
const Auth = lazy(() => import('./routes/auth.tsx'));
const Layout = lazy(() => import('./components/layout.tsx'));
 
const router = createBrowserRouter([
  {
    path: '/',
    element: (
      <ProtectedRoute>
        <Layout />
      </ProtectedRoute>
    ),
    children: [
      {
        path: '',
        element: <Home />,
      },
      {
        path: '/profile',
        element: <Profile />,
      },
      {
        path: `/search/:searchKeyword`,
        element: <SearchResult />,
      },
    ],
  },
  {
    path: '/auth',
    element: <Auth />,
  },
]);
 
function App() {
  const [isLoading, setIsLoading] = useState(true);
  const init = async () => {
    await auth.authStateReady();
    setIsLoading(false);
  };
  useEffect(() => {
    init();
  }, []);
  return (
    <>
      <S.GlobalStyles />
      <Suspense fallback={<LoadingSpinner />}>
        {isLoading ? <LoadingSpinner /> : <RouterProvider router={router} />}
      </Suspense>
    </>
  );
}
 
export default App;
```
<br>

- 위 코드는 최상위 컴포넌트 내에서 직접 Suspense를 사용한 예시이다.

- 그러나 router 자체에서 Suspense를 사용 할 수 있으며, 아래 URL을 참고하면 된다.

- [React - Suspense란](https://github.com/yejun95/Today-I-Learned/blob/master/React/%5BReact%5D%20Suspence%EB%9E%80.md)
<br>
<hr>
<br>

**Reference**<br>

[youg.dev : 리액트 lazy loading 적용](https://velog.io/@fkszm3/%EB%A6%AC%EC%95%A1%ED%8A%B8-lazy-loading-%EC%A0%81%EC%9A%A9)<br>
[leekoby : React.lazy()와 Suspense](https://velog.io/@abc2752/React.lazy%EC%99%80-Suspense)<br>
[Ryung Log : [REACT] 리액트 앱 성능 개선! React.lazy를 이용한 코드 스플리팅](https://s-ryung.tistory.com/74)<br>
