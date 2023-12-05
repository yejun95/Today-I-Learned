## plyload란 무엇인가?
- 전송되는 데이터를 의미하며, 데이터 자체이다.

- 서버로 데이터를 전송한 경우 브라우저에서 payload 탭이 활성화된다.

- HTTP 요청을 보낼 때 전달되는 데이터이다. 

- JSON 형태를 가진다.
  - 응답 받을 때도 JSON 형태
<br>

- Ex) 택배를 보낼 때, 택배 물건이 payload이고 송장, 보충제 같은 것은 부가적인 것이지 택배 자체가 아니다.

- 아래 이미지와 같이 body에 포함되어서 전송된다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/14c32d8b-b55c-455c-befa-367caa52e793)
<br>
<hr>
<br>

### ✔ 실제 데이터에서의 페이로드
- 프로토콜의 오버헤드와 원하는 데이터를 구별 할 때 사용된다.

- Ex) 웹 서비스의 응답 데이터가 JSON 데이터 형식이라면 JSON내의 `data`가 페이로드 데이터가 되고<br>
나머지 데이터는 통신을 하는데 용이하게 해주는 부차적인 정보를 가진 프로토콜 오버헤드라고 보면된다.
```
{
	"status" : 
	"from": "localhost",
	"to": "http://melonicedlatte.com/chatroom/1",
	"method": "GET",
	"data":{ "message" : "There is a cutty dog!" }
}
```
<br>
<hr>
<br>

**Reference**<br>

[Easy is Perfect : 페이로드란?](https://melonicedlatte.com/web/2020/01/14/205200.html)<br>
[리노92의 블랙박스 : payload란?](https://rino92.tistory.com/45)<br>
[생활코딩 : Network 탭 사용법](https://www.youtube.com/shorts/KT2lKab--kQ)
