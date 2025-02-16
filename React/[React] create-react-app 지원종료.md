# create-react-app 지원종료

- https://react.dev/blog/2025/02/14/sunsetting-create-react-app

비공식적으로 Deprecation 라고 여겼던 개발자들이 많아서 별 충격은 없으시겠지만.. 
아무튼 고생했구려..

—————————————————-

### Create React App 종료 안내 (한글 번역 & 요약)

**2025년 2월 14일**
글 작성자: Matt Carroll, Ricky Hanlon

오늘부로 Create React App(CRA)은 새로운 애플리케이션에서 더 이상 지원되지 않으며, 기존 앱은 프레임워크로 마이그레이션할 것을 권장합니다.<br>
또한, 프레임워크가 적합하지 않은 경우나 직접 프레임워크를 만들고 싶은 경우를 위한 문서도 제공하고 있습니다.
<br>
<hr>
<br>

### Create React App의 탄생 배경
2016년, React 애플리케이션을 새로 만들기 위한 명확한 방법이 없었습니다.<br>
JSX, 린팅(linting), 핫 리로딩(hot reloading)과 같은 기본 기능을 지원하려면 여러 도구를 설치하고 직접 구성해야 했습니다.<br>
이 과정은 매우 까다로웠고, 이를 해결하기 위해 다양한 보일러플레이트(boilerplate)들이 등장했습니다.<br>
그러나 보일러플레이트는 업데이트가 어렵고, 여러 가지 버전이 난립하면서 React의 새로운 기능을 쉽게 배포하기 어려운 문제가 있었습니다.<br>
이러한 문제를 해결하기 위해 Create React App이 등장했습니다. CRA는 여러 도구들을 하나의 추천 설정으로 묶어 관리할 수 있도록 했으며,<br>
이를 통해 React 팀이 중요한 기능(예: Fast Refresh, React Hooks의 린트 규칙)을 쉽게 배포할 수 있도록 만들었습니다.<br>
그 결과, CRA는 매우 인기 있는 방식이 되었고, 오늘날에는 CRA와 비슷한 방식으로 동작하는 다양한 프레임워크가 존재합니다.
<br>
<hr>
<br>

### Create React App이 종료되는 이유
CRA는 시작하기에는 편리하지만, 다음과 같은 여러 가지 한계가 있습니다.

1. 고성능 프로덕션 앱을 만들기 어려움
2. 라우팅, 데이터 페칭, 코드 스플리팅 등의 기능이 기본적으로 포함되지 않음
3. 활성 유지 보수자가 없음

이러한 문제를 해결하려면 CRA를 프레임워크 수준으로 발전시켜야 하지만, 이미 존재하는 여러 프레임워크들이 이러한 문제를 해결하고 있습니다.<br>
따라서 React 팀은 CRA를 공식적으로 종료(Deprecation)하기로 결정했습니다.
<br>
<hr>
<br>

### Create React App 종료 일정 및 영향
📢 2025년 2월 14일부터<br>
새로운 CRA 프로젝트를 생성할 경우, 터미널에서 다음과 같은 경고 메시지가 표시됩니다.<br>

```
create-react-app is deprecated.

You can find a list of up-to-date React frameworks on react.dev
For more info see: react.dev/link/cra

This error message will only be shown once per install.
```

이 메시지는 설치 시 한 번만 표시되며, 기존 프로젝트에서는 계속 사용할 수 있습니다.<br>
그러나 새로운 프로젝트에서는 CRA 대신 공식적으로 권장하는 프레임워크를 사용하는 것이 좋습니다.
<br>
<hr>
<br>

### ✅ 새로운 React 프로젝트를 만드는 방법
새로운 React 프로젝트를 만들려면 아래 프레임워크 중 하나를 사용하는 것이 좋습니다.

- Next.js (App Router)

```
npx create-next-app@latest
```
> Next.js는 Vercel에서 유지보수하며, 서버리스 배포 및 정적 내보내기(Static Export)를 지원합니다.
<br>

- React Router (v7)

```
npx create-react-router@latest
```
> React Router는 Shopify에서 유지보수하며, Vite 기반의 빠른 개발 환경을 제공합니다.
<br>

- Expo (React Native 앱)

```
npx create-expo-app@latest
```
> Expo는 React Native를 기반으로 iOS, Android, 웹을 모두 지원하는 프레임워크입니다.
<br>

- 기타 옵션
  - TanStack Start (Beta): TanStack Router 기반의 풀스택 React 프레임워크
  - RedwoodJS: 풀스택 React 프레임워크로, 사전 설정된 패키지가 포함됨
<br>
<hr>
<br>

### 기존 Create React App 프로젝트 마이그레이션 방법
- 기존 프로젝트를 최신 프레임워크로 전환하려면 아래 마이그레이션 가이드를 참고하세요.

1. Next.js의 CRA 마이그레이션 가이드
2. React Router의 프레임워크 전환 가이드
3. Expo Webpack → Expo Router 마이그레이션 가이드
<br>
<hr>
<br>

### Vite를 사용한 React 프로젝트 설정
Vite는 React 프로젝트를 설정할 때 가장 많이 사용되는 도구 중 하나입니다.<br>
새로운 프로젝트를 만들거나 기존 프로젝트를 변환할 때 Vite를 사용할 수 있습니다.
<br>

```
npm create vite@latest my-app --template react cd my-app npm install npm run dev
```
<br>

- 빠른 개발 서버 제공
- 빌드 속도 향상
- React 19 및 최신 기능 지원
<br>

- Vite를 사용하면 기존 CRA 프로젝트를 쉽게 변환할 수 있으며, 추가적인 설정 없이 더 나은 성능을 제공할 수 있습니다.
<br>
<hr>
<br>

### Create React App의 주요 한계
Create React App과 같은 빌드 도구를 사용하면 React 애플리케이션을 쉽게 시작할 수 있습니다.<br>
npx create-react-app my-app 명령을 실행하면 개발 서버, 린팅(Linting),<br>
그리고 프로덕션 빌드를 포함한 완전히 구성된 React 애플리케이션이 생성됩니다.<br>

예를 들어, 내부 관리자(Admin) 도구를 만든다고 가정하면, 다음과 같이 랜딩 페이지부터 시작할 수 있습니다:
<br>

```js
export default function App() {
  return (
    <div>
      <h1>Welcome to the Admin Tool!</h1>
    </div>
  );
}
```
<br>

이렇게 하면 JSX, 기본 린팅 규칙, 개발 및 프로덕션 환경에서 실행할 수 있는 번들러와 같은 기능을 즉시 사용할 수 있습니다.<br>
그러나 이 기본 설정만으로는 실제 프로덕션 애플리케이션을 구축하는 데 필요한 도구가 부족합니다.<br>

대부분의 프로덕션 애플리케이션은 라우팅, 데이터 페칭, 코드 스플리팅과 같은 문제를 해결할 수 있는 솔루션이 필요합니다.<br>
<br>

**라우팅**

Create React App에는 기본적으로 특정한 라우팅 솔루션이 포함되어 있지 않습니다.<br>
처음 시작할 때는 useState를 사용하여 간단히 라우트를 변경할 수도 있습니다.<bR>
하지만 이렇게 하면 앱의 링크를 공유할 수 없고, 모든 링크가 동일한 페이지로 연결되며,<br>
시간이 지남에 따라 애플리케이션 구조를 관리하기 어려워집니다.

```js
import { useState } from 'react';

import Home from './Home';
import Dashboard from './Dashboard';

export default function App() {
  // ❌ 상태를 이용한 라우팅은 URL을 생성하지 않음
  const [route, setRoute] = useState('home');
  return (
    <div>
      {route === 'home' && <Home />}
      {route === 'dashboard' && <Dashboard />}
    </div>
  );
}
```
<br>

이 때문에 대부분의 Create React App 사용자들은 React Router 또는 Tanstack Router와 같은<br>
라우팅 라이브러리를 추가하여 문제를 해결합니다.<br>
라우팅 라이브러리를 사용하면 앱에 추가적인 라우트를 정의할 수 있고,<br>
애플리케이션 구조를 더 체계적으로 만들 수 있으며, 특정 경로를 공유할 수도 있습니다.<br>

예를 들어, React Router를 사용하면 다음과 같이 라우트를 설정할 수 있습니다:
<br>

```js
import { RouterProvider, createBrowserRouter } from 'react-router-dom';

import Home from './Home';
import Dashboard from './Dashboard';

// ✅ 각 라우트가 고유한 URL을 가짐
const router = createBrowserRouter([
  { path: '/', element: <Home /> },
  { path: '/dashboard', element: <Dashboard /> }
]);

export default function App() {
  return <RouterProvider router={router} />;
}
```
<br>

이렇게 변경하면 /dashboard 링크를 공유하면 해당 페이지로 직접 이동할 수 있습니다.<br>
라우팅 라이브러리를 도입하면 중첩 라우트(nested routes), 라우트 가드(route guards),<br>
라우트 전환(route transitions) 같은 기능도 추가할 수 있습니다.<br>
이러한 기능들은 라우팅 라이브러리 없이 구현하기 어렵습니다.<br>

물론 라우팅 라이브러리를 사용하면 애플리케이션이 조금 더 복잡해질 수 있습니다.<br>
하지만 이를 통해 관리가 쉬운 구조와 다양한 기능을 제공받을 수 있기 때문에,<br>
대부분의 프로젝트에서 라우팅 라이브러리를 도입하는 것이 일반적입니다.
<br>

**데이터 페칭**
Create React App에서 흔히 겪는 또 다른 문제는 데이터 페칭(data fetching)입니다.<br>
Create React App은 기본적으로 특정한 데이터 페칭 솔루션을 포함하고 있지 않습니다.<br>
처음 시작할 때는 fetch를 useEffect 내부에서 호출하여 데이터를 불러오는 것이 일반적인 방법입니다.<br>

하지만 이렇게 하면 컴포넌트가 렌더링된 후에 데이터를 가져오게 되므로 네트워크 워터폴(network waterfall) 문제가 발생할 수 있습니다.
네트워크 워터폴은 애플리케이션이 렌더링될 때마다 데이터를 순차적으로 불러오는 방식 때문에 로딩 시간이 길어지는 문제입니다.

export default function Dashboard() {
  const [data, setData] = useState(null);

  // ❌ 컴포넌트에서 데이터를 가져오면 네트워크 워터폴 발생
  useEffect(() => {
    fetch('/api/data')
      .then(response => response.json())
      .then(data => setData(data));
  }, []);

  return (
    <div>
      {data.map(item => <div key={item.id}>{item.name}</div>)}
    </div>
  );
}


위와 같은 방식으로 데이터를 가져오면, 데이터를 더 일찍 가져올 수 있었음에도 불구하고, 사용자는 데이터를 기다려야 하는 상황이 발생합니다.
이 문제를 해결하기 위해서는 React Query, SWR, Apollo, Relay 같은 데이터 페칭 라이브러리를 사용하면 좋습니다.
이러한 라이브러리는 데이터를 사전 로드(prefetching) 하는 기능을 제공하여, 컴포넌트가 렌더링되기 전에 데이터를 미리 요청할 수 있습니다.

또한, 데이터를 라우트 로더(loader) 패턴과 통합하면 라우트가 로드될 때 데이터를 동시에 가져올 수 있어 최적화된 데이터 페칭이 가능합니다.

export async function loader() {
  const response = await fetch(`/api/data`);
  const data = await response.json();
  return data;
}

// ✅ 코드가 다운로드되는 동안 데이터를 병렬로 가져옴
export default function Dashboard({ loaderData }) {
  return (
    <div>
      {loaderData.map(item => <div key={item.id}>{item.name}</div>)}
    </div>
  );
}


최초 로드 시, 라우터가 데이터를 미리 가져온 후에 라우트를 렌더링할 수 있습니다.
사용자가 앱을 탐색할 때도 라우트와 데이터를 동시에 병렬로 가져올 수 있어 로딩 시간을 줄이고 사용자 경험을 개선할 수 있습니다.

그러나, 이를 올바르게 설정하려면 라우트의 로더(loader) 를 적절히 구성해야 하며, 이는 성능 향상을 위해 코드의 복잡도를 증가시키는 트레이드오프가 존재합니다.

Code Splitting


Create React App의 또 다른 일반적인 문제는 코드 스플리팅(Code Splitting)입니다. Create React App은 특정한 코드 스플리팅 솔루션을 포함하지 않습니다. 처음 시작할 때는 코드 스플리팅을 고려하지 않을 수도 있습니다.

이 경우, 애플리케이션은 단일 번들로 제공됩니다:





bundle.js 75KB

그러나 이상적인 성능을 위해서는 코드를 별도의 번들로 "분할"하여 사용자가 필요한 부분만 다운로드하도록 해야 합니다. 이렇게 하면 사용자가 현재 보고 있는 페이지에 필요한 코드만 다운로드하므로 애플리케이션 로딩 시간이 단축됩니다.





core.js 25KB



home.js 25KB



dashboard.js 25KB

코드 스플리팅을 수행하는 한 가지 방법은 React.lazy를 사용하는 것입니다. 하지만 이 방법은 컴포넌트가 렌더링될 때까지 코드를 가져오지 않으므로 네트워크 워터폴(Network Waterfall) 문제가 발생할 수 있습니다. 보다 최적화된 솔루션은 라우터 기능을 활용하여 코드가 다운로드되는 동안 병렬로 가져오는 것입니다. 예를 들어, React Router는 lazy 옵션을 제공하여 코드 스플리팅을 적용하고 로딩 시점을 최적화할 수 있습니다.

import Home from './Home';
import Dashboard from './Dashboard';

// ✅ Routes are downloaded before rendering
const router = createBrowserRouter([
  {path: '/', lazy: () => import('./Home')},
  {path: '/dashboard', lazy: () => import('Dashboard')}
]);

최적화된 코드 스플리팅을 올바르게 구현하는 것은 까다로우며, 사용자가 불필요한 코드를 다운로드하게 만드는 실수를 하기 쉽습니다. 따라서 코드 스플리팅은 라우터 및 데이터 로딩 솔루션과 함께 통합될 때 가장 효과적으로 작동하며, 캐싱을 최적화하고 병렬 요청을 수행하며 "사용자 상호작용 시 로드(Import on Interaction)" 패턴을 지원할 수 있습니다.





결론: 이제는 프레임워크를 사용하세요!

Create React App은 더 이상 유지보수되지 않으며, React 프로젝트를 만들 때는 공식적으로 지원하는 프레임워크를 사용하는 것이 가장 좋은 방법입니다.

✅ 추천 대안





npx create-next-app@latest → Next.js



npx create-react-router@latest → React Router



npx create-expo-app@latest → Expo (React Native)



npm create vite@latest my-app --template react → Vite (커스텀 설정 가능)

CRA에서 마이그레이션해야 한다면 Next.js, React Router, Expo 등의 가이드를 참고하세요! 🚀







부록1) Vite와 Next.js의 코드 스플리팅(Code Splitting) 전략 

1. Vite의 코드 스플리팅 전략

Vite는 ESM 기반의 네이티브 브라우저 모듈 로딩을 활용하여 코드 스플리팅을 최적화합니다.
이를 통해 불필요한 코드 다운로드를 방지하고, 초기 로딩 속도를 개선할 수 있습니다.

✅ Vite의 주요 코드 스플리팅 방식





ESM 기반의 자동 코드 스플리팅





Vite는 import()를 감지하여 자동으로 번들을 분리합니다.



예제:

import { lazy, Suspense } from 'react';

const Home = lazy(() => import('./Home'));
const Dashboard = lazy(() => import('./Dashboard'));

export default function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Home />
    </Suspense>
  );
}




위 코드에서 import('./Home') 및 import('./Dashboard')가 자동으로 별도 청크(bundle) 로 분리됩니다.



라우팅과 함께 최적화





Vite는 react-router-dom 같은 라우팅 라이브러리와 함께 사용하면 라우트 단위 코드 스플리팅을 쉽게 구현할 수 있습니다.



예제 (React Router 사용):

import { createBrowserRouter, RouterProvider } from 'react-router-dom';

const router = createBrowserRouter([
  { path: '/', lazy: () => import('./Home') },
  { path: '/dashboard', lazy: () => import('./Dashboard') }
]);

export default function App() {
  return <RouterProvider router={router} />;
}




위 방식은 Home과 Dashboard가 필요한 시점에만 다운로드되도록 최적화됩니다.



Preloading & Prefetching 최적화





Vite는 vite-plugin-legacy, @vite/plugin-legacy 등을 활용하여 브라우저의 rel="preload" 및 rel="prefetch"을 자동 적용할 수 있습니다.



이를 통해 사용자가 다음에 방문할 가능성이 높은 페이지의 코드를 미리 다운로드하여 로딩 속도를 개선합니다.



2. Next.js의 코드 스플리팅 전략

Next.js는 기본적으로 자동 코드 스플리팅을 지원하며, 서버 사이드 렌더링(SSR), 정적 사이트 생성(SSG), 온디맨드 로딩을 조합하여 최적화합니다.

✅ Next.js의 주요 코드 스플리팅 방식





페이지 단위 코드 스플리팅





Next.js는 페이지 단위(pages/ 또는 app/ 디렉토리)로 자동 코드 스플리팅을 수행합니다.



즉, 사용자가 특정 페이지를 방문할 때만 해당 코드가 로드됩니다.



예제:

pages/
  index.js  → "/" (홈 페이지)
  dashboard.js  → "/dashboard"




사용자가 /dashboard 페이지로 이동해야 dashboard.js가 다운로드됩니다.



next/dynamic을 활용한 동적 로딩





import()를 기반으로 특정 컴포넌트를 필요할 때만 불러올 수 있습니다.



예제:

import dynamic from 'next/dynamic';

const Dashboard = dynamic(() => import('../components/Dashboard'), {
  loading: () => <p>Loading...</p>, // 로딩 중 UI
  ssr: false, // 서버에서 렌더링하지 않음 (클라이언트 사이드에서만 로드)
});

export default function Home() {
  return (
    <div>
      <h1>Welcome</h1>
      <Dashboard />
    </div>
  );
}


ssr: false 설정을 통해 클라이언트에서만 동적 로딩하도록 조정할 수도 있습니다.



데이터 로딩과 함께 코드 스플리팅 최적화 (getServerSideProps, getStaticProps)





Next.js는 데이터 로딩과 코드 스플리팅을 결합하여 최적화할 수 있습니다.



예제 (서버 측 데이터 패칭 + 코드 스플리팅):

export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();
  return { props: { data } };
}

export default function Dashboard({ data }) {
  return (
    <div>
      {data.map(item => (
        <div key={item.id}>{item.name}</div>
      ))}
    </div>
  );
}




getServerSideProps를 활용하면 서버에서 미리 데이터를 가져와서 코드와 함께 렌더링할 수 있습니다.



자동 Preloading & Prefetching





Next.js는 next/link를 사용할 때 자동으로 해당 링크의 코드 및 데이터를 미리 불러옵니다.



예제:

import Link from 'next/link';

export default function Home() {
  return (
    <div>
      <Link href="/dashboard">
        <a>Go to Dashboard</a>
      </Link>
    </div>
  );
}




사용자가 <a> 태그에 마우스를 올리면 dashboard.js가 백그라운드에서 자동 다운로드됩니다.





부록2) 네트워크 워터폴은 react query 사용하면 왜 안생기지?


🚨 네트워크 워터폴(Network Waterfall)이란?

네트워크 워터폴은 여러 개의 비동기 요청이 직렬(순차적)로 실행되어 불필요한 대기 시간이 증가하는 현상입니다.
예를 들어, 다음과 같은 상황에서 발생할 수 있습니다.

❌ 기본 useEffect를 사용한 데이터 요청 (네트워크 워터폴 발생)

import { useState, useEffect } from "react";

export default function Dashboard() {
  const [user, setUser] = useState(null);
  const [posts, setPosts] = useState(null);

  // 첫 번째 요청 (유저 데이터)
  useEffect(() => {
    fetch("/api/user")
      .then((res) => res.json())
      .then((data) => setUser(data));
  }, []);

  // 두 번째 요청 (게시물 데이터)
  useEffect(() => {
    if (!user) return; // user 데이터가 로드된 후에만 실행됨 (직렬 요청)
    fetch(`/api/posts?userId=${user.id}`)
      .then((res) => res.json())
      .then((data) => setPosts(data));
  }, [user]);

  if (!user || !posts) return <p>Loading...</p>;

  return (
    <div>
      <h1>{user.name}'s Dashboard</h1>
      {posts.map((post) => (
        <div key={post.id}>{post.title}</div>
      ))}
    </div>
  );
}


✅ useEffect를 사용한 기본 데이터 패칭 방식에서는 user 데이터를 먼저 가져온 후(posts 요청은 user가 로드된 이후 실행)
✅ posts 요청은 user.id가 필요하므로 두 번째 요청이 첫 번째 요청이 끝난 후에야 시작됨 (직렬 실행)
✅ 워터폴 발생 → user를 가져오는 동안 posts 요청이 대기하는 시간 낭비



🚀 React Query를 사용하면 네트워크 워터폴이 발생하지 않는 이유

React Query는 쿼리 키(Query Key) 기반으로 데이터를 캐싱하고, 병렬(parallel)로 데이터를 가져오는 최적화 기능을 제공합니다.

✅ React Query를 사용한 코드 (병렬 요청)

import { useQuery } from "@tanstack/react-query";

export default function Dashboard() {
  // 유저 데이터 요청
  const { data: user } = useQuery({
    queryKey: ["user"],
    queryFn: () => fetch("/api/user").then((res) => res.json()),
  });

  // 게시물 데이터 요청 (동시에 실행됨)
  const { data: posts } = useQuery({
    queryKey: ["posts", user?.id], // user.id가 필요하지만, React Query는 자동으로 처리
    queryFn: () => fetch(`/api/posts?userId=${user.id}`).then((res) => res.json()),
    enabled: !!user, // user가 있을 때만 실행됨
  });

  if (!user || !posts) return <p>Loading...</p>;

  return (
    <div>
      <h1>{user.name}'s Dashboard</h1>
      {posts.map((post) => (
        <div key={post.id}>{post.title}</div>
      ))}
    </div>
  );
}




✨ React Query가 네트워크 워터폴을 방지하는 이유

✅ 1. 데이터 요청을 병렬 실행 (Parallel Fetching)





useQuery를 사용하면 React Query가 최대한 빠르게 데이터를 가져옴



user와 posts 요청이 서로 의존하지 않는다면 동시에 실행됨

✅ 2. 자동 캐싱 및 중복 요청 방지





동일한 쿼리 키(["user"], ["posts", user?.id])가 사용되면 중복 요청을 방지



만약 user 데이터가 이미 캐시에 있다면, 다시 요청하지 않고 바로 posts 요청을 실행

✅ 3. 의존성이 있는 데이터도 최적화 (enabled 옵션 사용)





enabled: !!user를 설정하면 user 데이터가 로드된 후 posts 요청이 실행됨



하지만 React Query 내부에서 비동기 상태를 관리하므로, 최대한 빠르게 posts 요청을 시작



useEffect를 사용한 방식보다 불필요한 렌더링과 대기 시간이 줄어듦
<br>
<hr>
<br>

**Refernece**<br>

[OKKY : 김포포 : create-react-app 지원종료](https://okky.kr/articles/1527414)
