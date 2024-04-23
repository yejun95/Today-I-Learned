## URL과 URI의 차이
- 인터넷을 사용하다보면 URL이라는 말은 많이 들어봤지만, 그와 비슷한 URI는 생소할 것이다.

- 이러한 차이점이 무엇인지 확인하고자 한다.
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/3f39e604-c1f6-4410-90fe-ecec92fe975b)
<br>
<br>

- 위 그림에서 보이듯이 URI는 URL과 URN을 포함하고 있다.
  - URI : 자원 식별자
  - URL : 위치(Location)
  - URN - 이름 (Name)
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/d9f6fbad-52af-4143-b719-77954723cc18)
<br>
<hr>
<br>

### ✔ URI (Uniform Resource Identifier, 통합자원식별자)
- 인터넷에 있는 자원을 나타내는 **유일한** 주소
  - 자원 자체를 식별하는 방법
  - Uniform : 리소스를 식별하는 통일된 방식
  - 자원, URI로 식별할 수 잇는 모든 것
  - 다른 항목과 구분하는데 필요한 정보
<br>

- 인터넷에 존재하는 각종 정보들의 유일한 이름이나 위치를 표시하는 식별자
<br>
<hr>
<br>

### ✔ URL (Uniform Resource Locator)
- 네트워크 상에서 리소스가 어디있는지 알려주기 위한 규약
  - 즉, 자원의 위치를 알려주는 규약
<br>

- 때문에 URL을 웹 사이트 주소로만 알고 있는 것은 잘못된 것이다.
  - 컴퓨터 네트워크상의 자원을 모두 나타내는 표기법이다.
<br>

- [URL 구성 요소 & 요청 흐름 정리](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-%EA%B5%AC%EC%84%B1-%EC%9A%94%EC%86%8C-%EC%9A%94%EC%B2%AD-%ED%9D%90%EB%A6%84-%EC%A0%95%EB%A6%AC)
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/265ac29d-54ba-43f0-afae-28aba7e81e81)
<br>
<hr>
<br>

### ✔ URN (Uniform Resource Name)
- URL이 리소스가 있는 위치를 지정한다면, URN은 리소스에 이름을 부여

- 하지만 리소스가 이름에 매핑되어 있어야 하기 때문에 이름으로 부여하면 찾기가 힘들다.
  - 그래서 대부분 URL만 쓴다.
<br>
<hr>
<br>

### ✔ URI / URL / URN 구분하기
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/0ee8b2d7-1934-4314-853c-b859f8547c46)
<br>
<br>

- URI / URL / URN 정의에 대해 알아보았지만, 항상 애매모호한 부분이 있다.

- 자원의 위치(URL)과 자원의 식별자(URI)는 비슷해 보이지만 '자원의 위치'라는것은 결국 **'하나의 파일 위치'**를 나타낸다.
<br>

**예시**
- 아래와 같은 홈페이지 링크가 있다고 해보자
  - http://www.naver.com/index.html?page=1232950&id=776
  - http://www.naver.com/ 서버에 위치한 index.html 페이지는 query string인 page의 값에 따라 결과가 달라진다
<br>

- 이때 여기서 URL은 index.html의 위치를 표기한 http://www.naver.com/index.html 까지다.

- 하지만 사용자가 원하는 정보에 도달하기 위해서는 ?page=1232950&id=776이라는 식별자(Identifier)가 필요

- 따라서 같은 주소라도 아래와 같이 분리가 가능하다.
  - URI : http://www.naver.com/index.html?page=1232950&id=776
  - URL : 식별자가 빠진 http://www.naver.com/index.html
<br>

- 💡 통상적으로 대충 URL이라고 얘기하지만 정확히는 URI라고 보는 것이 맞다.
<br>

**또 다른 예시**
- 조금 억지스러운 예를 들면, 아래의 두 주소는 같은 URL이고 다른 URI라고 할 수 있다.
  - http://www.naver.com/index.html?page=1232950&id=776
  - http://www.naver.com/index.html?page=9923145&id=122
<br>
<hr>
<br>

**Reference**<br>

[Inpa Dev : URL / URI / URN 차이점 - 한방 이해하기](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-URI-%EC%B0%A8%EC%9D%B4)<br>
[h log : URI URL 구조, 차이점](https://velog.io/@h220101/URI-URL-%EA%B5%AC%EC%A1%B0-%EC%B0%A8%EC%9D%B4%EC%A0%90)


