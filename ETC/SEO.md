## SEO 란 무엇인가?
- Search Engine Optimization

- 구글, 네이버 같은 검색엔진에 친화적인 사이트를 구축하여 광고가 아닌 자연 검색 결과를 통해 트래픽의 양과 질을 극대화하는 작업을 의미
  - 친화적이란 올바른 타이틀, 내용 등으로 사이트를 구성한 것을 말한다.
  - 유료 광고를 사용하지 않는다.
<br>

- 웹사이트의 가시성을 높이고 온라인 비즈니스의 성공에 기여할 수 있는 중요한 마케팅 전략이다.

- 사용자들에게 유용한 양질의 콘텐츠를 많이 제공함으로써 여러 관련 키워드로 검색 결과 페이지에 노출이 되어<br>
웹사이트의 온라인 가시성을 개선하는 마케팅 작업이다.

- 고객을 유지하려면 고객의 니즈를 가장 잘 충족시켜주는 답을 제공해야한다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/6fef15e3-0d04-48b4-bcb3-81d95ff45905)
<br>
<hr>
<br>

### ✔ 목적
- 앞서 말했듯 친화적인 사이트를 구축하여 자연 검색 결과를 통해 트래픽을 얻는 것을 말한다.

- 자연 검색 결과란 아래 사진과 같다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/d6ad743a-5b4a-471a-9073-fbe5b167cf69)
<br>
<br>

- 결국 콘텐츠, 웹페이지를 제작함에있어 궁극적인 목적은 사용자에게 양질의 정보를 올바르게 전달하는 것이다.
  - 자연 검색 결과를 통해 트래픽을 유도
<br>
<hr>
<br>

### ✔ 검색엔진 최적화에 영향을 주는 요인
**페이지 타이틀(title)**

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/c9848310-95ce-4593-8dca-4aecf20ebf2a)

- title 태그는 html 문서 내 head 태그의 자식요소 중 하나이다

- 브라우저 탭에 표시되며 본문 콘텐츠 내에서는 표시되지 않는다.

- 본문 콘텐츠를 가장 잘 표현하는 키워드를 고른다.

- 각 페이지마다 구체적이고 적절한 단어를 사용하여 타이틀을 정한다.
<br>
<br>

**올바른 meta태그 사용**
- meta 태그는 html 문서 내 head 태그에 포함한다.

- UTF-8, viewport, description, keywords 등 많은 것이 있다.

- 특히 og 태그의 경우 웹페이지 미리보기 기능인데, 카톡에서 링크를 보내면 딸려오는 이미지를 말한다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/b0c39375-913f-4ead-af7f-ff46ada29801)
<br>

- 페이스북, 네이버, 카카오톡에 링크 공유 시 보여질 미리보기를 설정할 수 있다.

```javascript
<!-- 웹 페이지 URL -->
<meta property="og:url" content="www.youtube.com"> 

<!-- 콘텐츠 제목 -->
<meta property="og:title" content="YouTube">

<!-- 콘텐츠 설명 -->
<meta property="og:description" content="여기를 눌러 링크를 확인하세요."> 

<!-- 웹페이지,콘텐츠 타입 (ex: blog,article,website..) -->
<meta property="og:type" content="video"> 

<!-- 미리보기 썸네일 이미지 -->
<meta property="og:image" content="../images/thumb.png"> 

<!-- 사이트 이름 -->
<meta property="og:site_name" content="YouTube"> 
```
<br>
<br>

**캐노니컬(Canonical) 태그 사용**
- 캐노니컬 태그(canonical tag)란 한 화면 내에서 여러개의 URL이 중복되어 사용될 때 검색엔진에 대표 URL을 알려주는 태그이다.

- ex) website라는 사이트의 main이라는 페이지 내에서 아래의 URL들이 사용될 경우 html의 head내에 캐노니컬 태그로 대표되는 URL을 지정해주는 것이다.
```javascript
https://www.website.com/main?id=1
https://www.website.com/main?id=as200-21d5-1e401ad123
...

<link rel="canonical" href="https://www.website.com/main">
```
<br>
<br>

**모바일 친화적인 웹**
- `meta name="viewport"`태그를 통해 검색엔진에 해당 사이트가 모바일 친화적이라는 것을 알려주는 것이 최적화 점수를 많이 받을 수 있는 방법

- [여기](https://medium.com/search-engine-optimization-marketing/%EA%B5%AC%EA%B8%80%EC%9D%B4-%EC%95%8C%EB%A0%A4%EC%A3%BC%EB%8A%94-seo-%EB%B3%B4%EB%8B%A4-%EB%8D%94-%EC%A4%91%EC%9A%94%ED%95%9C-%EB%AA%A8%EB%B0%94%EC%9D%BC-%EA%B2%80%EC%83%89%EC%97%94%EC%A7%84%EC%B5%9C%EC%A0%81%ED%99%94-a6a6f73f47b7) 에서 자세한 설명 확인
<br>
<br>

**시맨틱 마크업, 의미있는 문서 작성과 링크(a), 이미지(img) 등 올바른 태그와 그에 맞는 속성 사용**
- `heading` 태그(h1,h2,h3 ...)의 적절한 사용과 `img`태그 `a`태그의 정확한 사용은 접근성 측면에서도 중요하다.

- `title`과 `alt` 속성을 사용해서 어떠한 링크인지, 어떻게 이동하는지(ex.새창열림), 어떤 이미지인지 설명되어야한다.
<br>
<br>

**SSL(https) 사용 여부 ( http❌ | https✅ )**
<br>
<br>

**핵심적인 웹 지표(LCP,FID,CLS)와 로딩속도**
- 핵심적인 웹 지표(Core web vitals)란 실제 사용 데이터에 따른 페이지의 성능을 보여주는 지표이다.

- 페이지의 성능이 중요한 이유는 사용자의 이탈률 때문이다. 당연하게도 로드 시간이 길수록 이탈률이 높아진다.
```
페이지 로드 시간이 1초에서 3초로 늘어나면 이탈률은 32% 증가합니다.
페이지 로드 시간이 1초에서 6초로 늘어나면 이탈률은 106% 증가합니다.

[출처] https://support.google.com/webmasters/answer/9205520?hl=ko#about_data
```

- LCP, FID, CLS 세 가지 항목으로 구성되어있다.
  - LCP : 사용자가 URL을 요청한 시점부터 페이지 내에서 시각적으로 가장 큰 콘텐츠를 그리는데에 걸리는 시간이다.
  - FID : 웹 페이지가 사용자의 동작(링크 클릭, 버튼 탭 등)에 반응할 때까지 걸리는 시간이다.
  - CLS : 누적 레이아웃 변경. 페이지가 덜컥거리면서 로딩되면 사용자는 원하는 곳에 빠르게 접근할 수 없다.
<br>
<br>

**백링크**
- 검색엔진이 인식 가능한 백링크는 a태그와 같은 링크 태그로 구성된 '클릭 및 이동'이 가능한 하이퍼링크이다.
```
www.google.com ❌
www.google.com ✅
```
<br>
<br>

**연관성있는 양질의 콘텐츠를 주기적으로 제공**
- 높은 성능과 접근성으로 좋은 사용자 경험을 제공하여 웹 페이지의 클릭률과 체류시간을 높히고, 이탈률은 낮춘 콘텐츠
<br>
<br>

**노출 대비 클릭률**
- 1~5번째 검색결과에서 65% 이상의 클릭률이 발생하고, 1페이지 내에서 70% 이상의 클릭률이 발생한다고 한다.

- 순위를 높히기 위해서는 앞서 나열한 모든 요소들, 양질의 콘텐츠, 마케팅 요소 등 많은 요인들이 조화를 이루어야 한다.

- SEO CTR(Click Through Rate) = 클릭수(Click) / 검색량(Search Volume)

- 이외에도 키워드 연구, 웹사이트 내부 최적화, 외부 링크 구축, 속도 최적화, 사용자 경험 개선 등 다양한 요소가 포함될 수 있다.
<br>
<hr>
<br>

**Reference**<br>

[데이터랩 : SEO (검색엔진최적화) 개념 이해하기 – AtoZ](https://seo.tbwakorea.com/blog/complete-guide-to-seo/)<br>
[hsecode.log : [최적화] 검색엔진 최적화(SEO) 이유와 방법 10가지](https://velog.io/@hsecode/%EC%B5%9C%EC%A0%81%ED%99%94-%EA%B2%80%EC%83%89%EC%97%94%EC%A7%84-%EC%B5%9C%EC%A0%81%ED%99%94SEO-%EC%9D%B4%EC%9C%A0%EC%99%80-%EB%B0%A9%EB%B2%95-10%EA%B0%80%EC%A7%80)
