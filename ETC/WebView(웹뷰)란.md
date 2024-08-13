# WebView란 무엇인가?
- 프레임워크에 내장된 웹 브라우저 컴포넌트를 통해 뷰(View)의 형태로 앱에 임베딩하는 것을 말한다.

- 네이티브(또는 크로스 플랫폼) 앱에 내재되어 있는 웹 브라우저이다.

- 쉽게 말해, App에서 웹브라우저를 이용해 화면을 보여주는 방식이다.

- 이는 Native App위에 WebView가 보여지는 방식이며, 아래 그림과 같다.

![image](https://github.com/user-attachments/assets/260f7372-c6d9-4532-a0ad-0f2e26dc83c5)
<br>
<hr>
<br>

## ✔️ 예시
- 카카오톡에서 대화 상대와 링크를 주고 받으면 카카오톡 앱에서 웹뷰로 해당 링크에 접속하는 것을 확인할 수 있다.

![GIF 2024-08-13 오후 11-29-16](https://github.com/user-attachments/assets/d98014de-b484-4d20-9276-b1f81dcf8f62)
> 카카오톡에서 받은 링크를 클릭 시 앱 내에 브라우저를 통해 해당 URL로 접근하는 것을 확인 가능
<br>
<hr>
<br>

## ✔️ 쓰는 이유
- 간단하게 말하면, 기존에 만들어진 모듈을 그대로 이용하기 위함이 크다.

- 새로운 기술 개발은 돈과 시간이 많이 소요되지만, 기존 모듈을 App에서 쉽게 보여줄 수 있다면, 굳이 추가 개발없이
진행할 수 있기 때문이다.

- 예를 들어, App 에서 결제를 하는 경우가 굉장히 많은데, 이때 결제 모듈 자체를 App 전용으로 개발하기에는 쉽지 않다.
이를 웹뷰를 사용해 App 내에서 결제하는 화면을 보여주는 것이다.

- 아래는 PG사에서 구현해놓은 모듈이다.

![image](https://github.com/user-attachments/assets/8864d6cf-ace8-4fc9-9674-13d4794513c6)
<br>

- 이를 그대로 사용하기 위해 웹뷰를 활용한다면, 굳이 App 개발을 하지 않고도 API 사용이 가능하다.
<br>
<hr>
<br>

## ✔️ 일반 브라우저와의 차이
![image](https://github.com/user-attachments/assets/97da5f30-792c-4624-97c1-56d9bbfe3256)
<br>

- 네이티브 앱은 웹뷰를 포함하기 때문에, 웹뷰를 사용하면 웹 콘텐츠를 네이티브 앱 뷰와 같이 사용자에게 보여준다.

- 일반 웹 브라우저와 달리 웹뷰에는 주소창, 새로고침, 즐겨찾기와 같은 기능은 없고 단순히 웹페이지만 보여줌

- 웹뷰와 네이티브 앱의 소통은 브릿지 진행


**Reference**<br>

[뚜까망치 루크 : App에서 웹뷰(WebView)는 왜 쓰는 것일까?](https://hammerbrother.tistory.com/entry/App%EC%97%90%EC%84%9C-%EC%9B%B9%EB%B7%B0WebView%EB%8A%94-%EC%99%9C-%EC%93%B0%EB%8A%94-%EA%B2%83%EC%9D%BC%EA%B9%8C-%EC%9B%B9%EB%B7%B0%EC%9D%98-%EC%9E%A5%EC%A0%90%EA%B3%BC-%EB%8B%A8%EC%A0%90-%EC%9B%B9%EB%B7%B0%EB%9E%80)<br>
[뚜까망치 루크 : 웹뷰(Webview)의 장점과 단점](https://hammerbrother.tistory.com/entry/App%EC%97%90%EC%84%9C-%EC%9B%B9%EB%B7%B0Webview%EC%9D%98-%EC%9E%A5%EC%A0%90%EA%B3%BC-%EB%8B%A8%EC%A0%90%EC%9D%80-%EC%9B%B9%EB%B7%B0-%EB%8B%A8%EC%A0%90-%EC%9B%B9%EB%B7%B0-%EC%9E%A5%EC%A0%90-%EC%9B%B9%EB%B7%B0%EB%A5%BC-%EC%93%B0%EB%8A%94-%EC%9D%B4%EC%9C%A0-%EB%AC%B4%EC%8B%A0%EC%82%AC-App%EC%9D%84-%ED%86%B5%ED%95%9C-%EC%9B%B9%EB%B7%B0%EC%9D%98-%EC%9E%A5%EC%A0%90%EA%B3%BC-%EB%8B%A8%EC%A0%90-%ED%8C%8C%EC%95%85)<br>
[]



