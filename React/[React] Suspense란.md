# Suspense란 무엇인가?
- 웹 어플리케이션을 개발하다 보면 데이터 로딩 속도로 인해 사용자에게 부정적인 경험을 주는 문제에 직면하게 된다.

- 이런 상황에서 React Suspense를 사용하면 데이터의 로딩 속도를 보다 세련되게 처리하고, 사용자에게<br>
더 나은 경험을 제공해준다.

<img class="image_resized" style="aspect-ratio:1152/720;width:1152px;" src="https://ems.elancer.co.kr/99_upload/Append/T_Blog/editor/2024053011185335672.gif" alt="react-suspense" width="1052" height="520"><br>
> 즉, 컴포넌트의 렌더링을 일시 중지하고 데이터 로딩을 기다릴 수 있게 해주는 기능이다.
<br>

- React 16.6부터 도입된 기능으로, 컴포넌트가 렌더링 되기 전에 데이터 로딩을 기다릴 수 있게 해줌으로써 데이터 로딩 중에도 자연스러운 사용자 경험을 유도할 수 있다.

- 또한 코드 스플리팅(Code Splitting)과 함께 사용하면 초기 로딩 속도를 개선하는 데에도 도움이 됩니다.
<br>
<hr>
<br>

## ✔️ 핵심 개념
- Suspense의 핵심 개념으로는 ‘lazy’와 ‘fallback’이 있다.
  - [lazy란](https://github.com/yejun95/Today-I-Learned/edit/master/React/%5BReact%5D%20router%20-%20lazy%EB%9E%80.md)
<br>

- lazy는 동적 임포트를 통해 컴포넌트를 필요한 시점에 로드하는 기능이며, 이를 통해 초기 번들 크기를 줄이고 초기 로딩 속도를 개선할 수 있다.

- fallback은 컴포넌트가 로딩 중일 때 보여줄 ‘UI를 설정하는 prop’ 이다.
  - 로딩 스피너, 스켈레톤 UI 등을 활용
  - 이를 통해 사용자에게 콘텐츠가 로딩 중임을 알리고, 현재 애플리케이션이 멈춘 게 아니라 원활하게 동작하고 있음을 인식시킨다.
<br>
<hr>
<br>

## ✔️ 사용 이유
### 데이터 로딩 상태를 선언적으로 사용

**Suspense 사용 전**
```js
function App() {
  const [data, setData] = useState(null);

  useEffetct(() => {
    fetchData().then((result) => setData(result))
  }, []);

  if (!data) {
    return <div>로딩 중...</div>
  }

  return <div>{data}</div>
}
```
<br>

**Supense 사용 후**
```js
function App() {
  return (
    <Suspense fallback={<div>로딩 중...</div>}>
      <DataComponent />
    </Suspense>
  )
}
```
<br>
<br>

### 사용자 경험 개선
- Suspense를 활용하면 데이터 로딩을 통한 딜레이 동안에 자연스러운 사용자 경험을 제공할 수 있다.

<img class="image_resized" style="aspect-ratio:1152/720;width:1152px;" src="https://ems.elancer.co.kr/99_upload/Append/T_Blog/editor/2024053011255830448.gif" alt="lazy-suspense-react" width="1052" height="520">
<br>
<br>

### 초기 로딩 속도 개선
- lazy를 사용하여 초기에 필요한 컴포넌트만 로드하고, 나머지는 요구되었을 때 로드할 수 있다.
<br>
<br>

### 에러 처리 용이
- Error Boundary와 함께 사용하면 에러가 발생했을 때 사용자에게 적절한 피드백을 제공하고, 애플리케이션의 다른 부분은 정상적으로 동작하도록 할 수 있다.
<br>
<hr>
<br>

**Reference**<br>

[Elancer Logo : React Suspense: 리액트 서스펜스를 사용하는 이유와 사용법 총정리](https://www.elancer.co.kr/blog/detail/267)<br>
[React 공식문서 : <Suspense>](https://ko.react.dev/reference/react/Suspense)
