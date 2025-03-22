# BOM이란 무엇인가?
- Browser Object Model

- 웹 브라우저를 객체화한 것.
  - 브라우저를 스크립트 언어로 제어하기 위함
<br>

- JavaScript가 브라우저와 소통하기 위해서 만들어진 모델

- 모든 개별 객체들은 최상위 객체인 window 아래 존재

- 웹 페이지 내용을 제외한 웹 브라우저 창에 포함된 모든 객체 요소들을 의미

- window 속성에 속하며 document 문서가 아닌, window를 제어
<br>
<hr>
<br>

## ✔ 종류
- window : 최상위 객체로 각 프레임별로 하나씩 존재

- location : url 주소에 대한 정보를 제공

- document : 현재 문서에 대한 정보

- navigator : 브라우저의 정보를 제공, 주로 호환성 문제를 위해 사용

- history : 브라우저의 방문 기록 정보를 제공

- screen : 브라우저의 외부 환경에 대한 정보를 제공
<br>
<hr>
<br>

## ✔ 실습
**window**
- window 객체는 브라우저 자체를 컨트롤 하는 것으로, 우리에게 친숙한 `alert`, `close` 등의 함수를 가지고 있다.

- 브라우저에서 window 객체를 사용하여 alert을 사용해보자.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/40a589bf-65a4-411e-83e8-226292736889)
> alert 출력
<br>

- 하지만 평소 alert을 사용할 때, window를 입력하지 않고 사용했을 것인데, 이는 window 객체가 생략 가능하기 때문

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/60c1f03f-7c16-4b10-bcae-0808b0a0a8cf)
> window를 적지 않아도 동일하게 출력
<br>
<br>

**location**
- window 객체인 동시에 document의 프로퍼티이다.
  - 즉, `window.locaion`, `document,locaion` 둘 다 가능
<br>

- 웹 문서에 대한 정보를 쉽게 가져올 수 있다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/1221689c-54af-442d-96b6-8996786262e7)
> host 함수를 사용해 현재 url 출력
<br>
<br>

**document**
- 현재 문서에 대한 정보를 갖는 객체이다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/3f24701f-ba36-43f2-9acd-9ec152cd6efb)
> 현재 내가 보고 있는 화면을 제어 가능
<br>

- `document.querySelector`, `document.getElementById` 등을 통해 id나 class 값을 이용하여 접근 가능
<br>
<Br>

**navigator**
- 실행중인 애플리케이션에 대한 다양한 정보를 가져올 수 있다.

- ex) 위치정보 : `navigator.geolocation.getCurrentPosition()`

- google에서 navigator.geolocation.getCurrentPosition 검색하여 MDN 접속

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/6fe980e1-de2c-4674-a606-f97c096019f4)

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/ca64cb36-a206-498d-8b90-c7b6e28a5cf6)
> 사용법 확인
<br>

- 본 실습에서는 success 콜백만 이용

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/71747aef-649c-422e-96bb-353f2ee7b53b)
> 위치 권한 허용 진행
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/ebd4ad8c-d366-494f-afd7-59fde42155fe)
> 위도, 경도 등 확인
<br>
<br>

**history**
- 현재 브라우저가 접근했던 url을 제어 가능

- 브라우저에서 볼 수 있는 앞, 뒤로가기 버튼을 제어 한다고 보면 됨

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/b9eb2ed0-932a-4bf4-b08f-ddd3481eed14)
<br>

- `history.back()`, `history.forward()` 등등...
<br>
<br>

**screen**
- 사용자의 디스플레이 화면에 대한 다양한 정보 가짐

- screen 객체 출력 진행

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/bce9956c-1463-4a51-9dd5-61c893c11d4c)
> console.dir을 통해 객체 출력<br>
> 현재 보고 있는 브라우저의 정보가 나온다.
<br>
<hr>
<br>

**Reference**<br>

[짐코딩 : 프론트엔드 날개달기](https://www.inflearn.com/course/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EB%82%A0%EA%B0%9C%EB%8B%AC%EA%B8%B0/dashboard)
