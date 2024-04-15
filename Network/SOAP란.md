## SOAP란 무엇인가?
- Simple Object Access Protocol

- 흔히 알고 있는 HTTP, HTTPS, SMTP 등을 통해 XML 기반의 메시지를 컴퓨터 네트워크 상에서 교환하는 프로토콜이다.

- 보안이나 메시지 전송 등에 있어서 REST보다 더 많은 표준들이 정해져있기 때문에 조금 더 복잡하다.

- SOAP는 보안 수준이 엄격하다.
  - SOAP에서는 SSL도 지원하고 WS-Security라는 자체 표준의 보안 기능도 가지고 있다.
  - 따라서 은행용 모바일 앱처럼 보안 수준이 높아야 하거나, 신뢰할 수 있는 메시징 앱, 또는 ACID를 준수해야 하는 경우라면 SOAP 방식이 더욱 선호된다.
<br>

- REST에서는 표준화된 메시징 시스템이 갖춰져 있지 않으며, 통신 장애가 있을 경우 재시도를 통해서만 조치할 수 있다.
  - 반면 SOAP 표준에는 성공/반복 실행 로직이 규정되어 있기 때문에, SOAP API를 통해서 통신을 할 때 처음부터 끝까지 신뢰성을 제공한다.
<br>
<hr>
<br>

### ✔ 장점
- 기존 원격 기술들에 비해서 프록시와 방화벽에 구애받지 않고 쉽게 통신 가능하다.

- 플랫폼과 프로그래밍 언어에 독립적이다.

- 웹 서비스를 제공하기 위한 표준(WSDL, UDDI, WS-*)이 잘 정립되어 있다.

- 에러 처리에 대한 내용이 기본으로 내장되어 있다.

- 분산 환경에 적합하다.
<br>
<hr>
<br>

### ✔ 단점
- 복잡한 구조로 인해 오버헤드가 있으며, 이는 SOAP의 확장을 저해하고 있다.

- REST에 비해 상대적으로 무겁고 속도도 느리다.

- 개발 난이도가 높아 개발 환경의 지원이 필요하다.
<br>
<hr>
<br>

### ✔ 동작 원리
- 서비스 요청자가 웹 서비스 요청을 SOAP로 인코딩

- 인코딩한 값을 서비스 제공자에게 전달

- 서비스 제공자는 2.값을 디코딩 후 적절한 서비스 로직을 수행

- 나온 결과값을 다시 SOPA로 인코딩하여 서비스 요청자에게 반환
<br>
<hr>
<br>

### ✔ SOAP의 메시지 구조
- SOAP 메시지는 선택적 Header와 필수 Body를 포함하는 Envelope로 구성된 XML 문서로 인코딩되고, Body에 포함된 Fault는 오류 보고에 사용된다.
  - SOAP Envelope
<br>

- Envelope는 모든 SOAP 메시지의 루트 요소이며 두 개의 하위 요소인 선택적 Header 요소 및 필수 Body 요소를 포함한다.
  - SOAP Header
<br>

- Header는 Envelope의 선택적 하위 요소이며 메시지 경로를 따라 SOAP 노드로만 처리될 애플리케이션 관련 정보를 전달하는 데 사용된다.
  - SOAP Body
<br>

- Body는 Envelope의 필수 하위 요소이며 메시지의 최종 수신인을 대상으로 하는 정보를 포함한다.\
  - SOAP Fault

- Fault는 Body의 하위 요소이며 오류 보고에 사용된다.
<br>
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/36b71935-d46e-4952-84c2-31c75e6d5172)
<br>
<hr>
<br>

### ✔ SOAP와 REST의 차이
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/d321ea1c-21ff-4742-ac96-79f1d53a0be0)


**Reference**<br>

[wishket blog : SOAP REST 차이, 두 방식의 가장 큰 차이점은?](https://blog.wishket.com/soap-api-vs-rest-api-%EB%91%90-%EB%B0%A9%EC%8B%9D%EC%9D%98-%EA%B0%80%EC%9E%A5-%ED%81%B0-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%80/)<br>
[UsefulToKnow : SOAP란-무엇일까 ](https://usefultoknow.tistory.com/entry/SOAP%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)<br>
