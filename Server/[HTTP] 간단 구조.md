## HTTP의 구조?
- 하이퍼텍스트(HTML) 문서를 교환하기 위해 만들어진 protocol(통신 규약).

-  TCP/IP 기반으로 되어있다.

- 기본적으로 request(요청)/response(응답) 구조로 되어있다.
  - 클라이언트가 요청하고 서버가 응답한다.
  - 모든 통신은 요청과 응답으로 진행된다.
<br>
<hr>
<br>

### ✔ Request
- 공백을 제외하고 3가지 부분으로 나뉜다.
  - Start Line
  - Headers
  - Body (get 요청의 경우 없음)
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/3670d3f9-bd7b-4394-8823-b5372cccf39c)
<br>
<br>

**Start Line**
- 3가지로 구성
  - HTTP method
  - Request target
  - HTTP version
<br>

- HTTP method : 요청의 의도를 담고 있는 GET, POST, PUT, DELETE 등이 있다.

- Request target : HTTP Request가 전송되는 목표 주소이며, API 주소라고 보면 된다.

- HTTP version :  version에 따라 Request 메시지 구조나 데이터가 다를 수 있어서 version을 명시한다.

- 관리자 모드를 봤을 때 요청 헤더의 원본을 보면 Start Line에서 3가지 구조를 확인할 수 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/edcddab7-3fea-4721-ae55-dbd5784549f9)
<br>
<br>

**Headers**
-  request에 대한 추가 정보(addtional information)를 담고 있는 부분이다.

- 아래 정보는 바로 위에 있는 사진의 get 요청 header 원본이다.

```
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
Cache-Control: max-age=0
Connection: keep-alive
Host: localhost:8080
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36
sec-ch-ua: "Not_A Brand";v="8", "Chromium";v="120", "Google Chrome";v="120"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
```

- Accept : 클라이언트가 처리 가능한 미디어 타입 종류 나열

- Accept-Encoding : 클라이언트가 서버로부터 응답을 받을 때 어떤 인코딩 방식을 이해할 수 있는지 나타낸다.

- Accept-Language : 클라이언트가 어떤 언어를 이해할 수 있는지를 서버에 알려준다.
  - ko-KR: 한국어 (대한민국)를 가장 선호
  - ko;q=0.9: 한국어를 90% 정도 선호
  - en-US;q=0.8: 미국 영어를 80% 정도 선호
  - en;q=0.7: 어떤 영어 버전이든 70% 정도 선호
<br>

- Cache-Control : 캐시를 제어하는 헤더이다. 
  - max-age=0 : 리소스의 캐시 유효 시간을 0으로 설정<br>
  - "해당 리소스는 항상 최신 버전이 필요하며 캐시를 사용하지 말아주세요"라고 서버에게 알리는 중이다.
<br>

- Connection : 연결 유지에 대하여 서버 또는 클라이언트에게 지시하는 역할이다.
  - keep-alive : HTTP 요청이나 응답이 완료된 후에도 해당 연결을 열어두고 유지
<br>

- Host : 요청하려는 서버 호스트 이름과 포트번호

- Sec-Fetch-Dest : 브라우저가 리소스를 가져오는 목적지를 나타내는 보안 관련 헤더
  - document : HTML 문서를 가져오기 위한 것
<br>

- Sec-Fetch-Mode : 브라우저가 리소스를 가져오는 모드를 나타냄
  - navigate : 사용자가 주소 표시줄에 URL을 직접 입력하거나 링크를 클릭하여 새로운 문서를 탐색할 때 발생
  - same-origin : 같은 출처(Origin)에서 리소스를 가져오는 경우
  - no-cors : Cross-Origin 요청인데, CORS (Cross-Origin Resource Sharing) 헤더가 없는 경우 사용
  - cors : Cross-Origin 요청이며, CORS 헤더가 있는 경우 사용
<br>

- Sec-Fetch-Site :  브라우저가 리소스를 가져오는 위치 또는 사이트의 유형을 나타내는 보안 관련 헤더
  - none : 해당 리소스의 출처(Origin)가 사용자의 현재 페이지와 다른 출처에 속함
<br>

- Sec-Fetch-User: 브라우저가 현재 사용자의 상태를 나타내는 헤더
  - ?1 : 사용자가 로그인한 상태
<br>

- Upgrade-Insecure-Requests: 브라우저에게 현재 페이지의 리소스들을 안전한(secure) 버전으로 업그레이드하도록 요청
  - 1 : 현재 페이지의 모든 HTTP 리소스를 HTTPS로 업그레이드하도록 지시
<br>

- User-Agent : 클라이언트의 사용 환경 및 브라우저 정보를 서버에게 전달할 때 사용
  - Mozilla/5.0 : 대부분의 브라우저들이 호환성을 위해 사용하는 모질라(Mozilla) 포맷의 정보
  - Windows NT 10.0; Win64; x64 : 라이언트가 Windows 10 운영 체제를 사용하며, 64비트(x64) 아키텍처를 사용 중
  - AppleWebKit/537.36 (KHTML, like Gecko) : 렌더링 엔진으로 WebKit을 기반으로 하며, Gecko 엔진처럼 동작한다는 것을 나타냄
  - Chrome/120.0.0.0 : 브라우저가 Google Chrome이며, 버전은 120.0.0.0임을 나타냄
  - Safari/537.36 : Chrome과 호환성을 유지하기 위해 Safari의 버전 정보가 추가됨
<br>

- sec-ch-ua : 브라우저의 사용자 에이전트 (User Agent)에 대한 정보를 나타냄
  - Not_A Brand : 브라우저가 특정한 브랜드가 아님을 나타낸다.
  - Chromium";v="120": 브라우저는 Chromium 엔진을 기반으로 하며, 버전은 120이다.
  - Google Chrome";v="120" : 브라우저는 Google Chrome이며, 버전은 120이다.
<br>

- Sec-CH-UA-Mobile : 브라우저가 모바일 장치에서 동작하는 것인지에 대한 정보를 제공
  - 0 : 모바일 장치가 아님
  - ?1 or 1 : 모바일 장치임
<br>

-  Sec-CH-UA-Platform : 브라우저가 실행 중인 플랫폼(운영체제)을 나타낸다.
  - Windows : 윈도우 운영체제에서 실행되었다.
<br>
<br>

**body**
- HTTP Request가 전송하는 데이터를 담고 있는 부분
 
- 전송하는 데이터가 없다면 body 부분은 비어있다.

- 보통 post 요청일 경우, HTML 폼 데이터가 포함되어 있다.

- 관리자모드에서 페이로드를 보면 post 요청의 데이터를 볼 수 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/41596d1c-56f1-48b8-a237-6e16e8ac3b2f)
<br>
<hr>
<br>

### ✔ Response
- Request와 동일하게 공백(blank line)을 제외하고 3가지 부분으로 나누어진다.
  - Status Line
  - Headers
  - Body

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/d21b42c0-d130-428b-b302-91f3073dce4e)
<br>
<br>

**Start Line**
- HTTP Response의 상태를 간략하게 나타내주는 부분

- 3가지 구조로 구성
  - HTTP version
  - Status Code
  - Status Text
<br>

```
HTTP/1.1 200 OK
[HTTP version] [Status Code] [Status Text]
```
<br>

- 관리자 모드에서 원본을 보면 응답 헤더를 자세하게 볼 수 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/1e775ecc-5269-4705-b869-9186dede78e3)
<br>
<br>

**headers**
- Request의 headers와 동일하다.

- 다만 response에서만 사용되는 header 값들이 있다.
  - 예를 들어, User-Agent 대신에 Server 헤더가 사용된다.
  - 밑 사진은 뽐뿌에서 참조한 POST 요청이다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a4b4f3c7-d4fc-45bd-875c-aa43a0bc5a25)

- Server: Finatra : 해당 서버가 Finatra라는 웹 프레임워크를 기반으로 한 서버임을 나타낸다.
  - User-Agent 헤더가 클라이언트의 사용환경을 나타냈다면 Server 헤더는 서버의 환경을 나타낸다.
  - nginx 와 같은 웹서버 속성이 보일 수도 있다.
<br>
<br>

**body**
- Response의 body와 일반적으로 동일하다.

- Request와 마찬가지로 모든 response가 body가 있지는 않다.

- 데이터를 전송할 필요가 없을경우 body가 비어있게 된다.
<br>
<hr>
<br>

**Reference**<br>

[99C0RN : HTTP Request/Response 구조](https://hahahoho5915.tistory.com/62)<br>
[tasty : HTTP 구조 및 핵심 요소](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)
