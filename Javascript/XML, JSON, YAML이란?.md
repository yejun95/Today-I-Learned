## XML, JSON, YAML 파일 형식은 무엇인가?
- 세 가지 모두 다 data를 표현하는 방식 중에 하나이다.

- 다만 그 형태와 문법은 서로 상이하기에 알맞은 방법을 선택해야 한다.
<br>
<hr>
<br>

### ✔ XML
- 데이터를 표현하기 위하여 많이 사용되어 온 방식으로 HTML과 흡사한 구조를 가지고 있다.

- XML이 가지는 고유한 문법이 있다는 점에서 소프트웨어 및 하드웨어에 대하여 독립적으로 데이터를 처리할 수 있다.
  - 특징 : 꺽쇠(<>)를 사용하여 트리(tree)계층 구조를 만든다.
 
- 가장 중요한 문법으로는 XML 요소들은 시작 태그와 종료 태그로 구성된다는 점이다.
  - 이러한 문법 때문에 HTML과 흡사한 구조를 가진 것처럼 보인다. 
<br>

**XML 예시**
```
<?xml version="1.0" encoding="UTF-8"?>
<users>
  <user>
    <name>홍길동</name>
    <score>95</score>
    <hobby>
      <element>Soccer</element>
      <element>Ninza</element>
    </hobby>
  </user>
  <user>
    <name>이순신</name>
    <score>100</score>
    <hobby>
      <element>Sing</element>
      <element>Dancing</element>
    </hobby>
  </user>
  <user>
    <name>나동빈</name>
    <score>97</score>
    <hobby>
      <element>Coding</element>
      <element>Hiding</element>
    </hobby>
  </user>
</users>
```
<br>
<hr>
<br>

### ✔ JSON
- . 일반적으로 서버와의 통신 규약인 REST API를 사용할 때 가장 많이 사용되어, 최근에는 XML보다는 JSON 형식이 채택되고 있다.

- 사실상 모든 프로그래밍 언어에서 JSON을 지원한다는 점에서 XML과 YAML에 비해서 채택률이 높아지고 있다.

- JSON은 주석을 사용할 수 없다는 특징이 있다.
<br>

**JSON 예시 변환 전**
```
<?xml version="1.0" encoding="UTF-8"?>
<users>
  <user>
    <name>홍길동</name>
    <score>95</score>
    <hobby>
      <element>Soccer</element>
      <element>Ninza</element>
    </hobby>
  </user>
  <user>
    <name>이순신</name>
    <score>100</score>
    <hobby>
      <element>Sing</element>
      <element>Dancing</element>
    </hobby>
  </user>
  <user>
    <name>나동빈</name>
    <score>97</score>
    <hobby>
      <element>Coding</element>
      <element>Hiding</element>
    </hobby>
  </user>
</users>
```
<br>

**JSON 예시 변환 후**
```
{
	"users": {
		"1": {
			"name": "홍길동",
			"score": 95,
			"hobby": ["Soccer", "Ninza"]
		},
		"2": {
			"name": "이순신",
			"score": 100,
			"hobby": ["Sing", "Dancing"]
		},
		"3": {
			"name": "나동빈",
			"score": 97,
			"hobby": ["Coding", "Hiding"]
		}
	}
}
```
<br>

- 기본적으로 JSON 형식에서는 키(Key) 값이 서로 다른 형태로 사용된다.

- JSON은 XML과 다르게 꺽쇠가 사용되지 않고 대괄호({})와 큰 따옴표를 이용해 계층형 구조를 형성한다는 특징이 있습니다.
<br>
<hr>
<br>

### ✔ YAML
- YAML 또한 JSON과 비슷하게 사람이 읽기 쉬운 형태의 데이터 표현 형식이다.

- 태그를 사용하지 않고 공백 위주로 데이터를 구분하므로 한 줄로 작성할 수 없다는 특징이 있다.
  - 💡 XML과 JSON은 모두 한줄로 써서 전송해도 상관이 없다.

- 일반적으로 API를 만들 때에는 JSON을 사용하며, YAML은 설정 파일을 작성할 때 가장 많이 사용된다는 특징이 있다.
  - 대표적으로 docker의 compose 파일을 만들 때, YAML 파일을 사용한다.
 
**YAML 예시**
```
  1:
    name: 홍길동
    score: 95
    hobby:
      - Soccer
      - Ninza
  2:
    name: 이순신
    score: 100
    hobby:
      - Sing
      - Dancing
  3:
    name: 나동빈
    score: 97
    hobby:
      - Coding
      - Hiding
```
<br>

- 이와 같이 YAML 문서는 위에서 아래로 직렬적으로 작성되어 있다는 점에서 **데이터 직렬화 형식**이라고도 부른다.

- 일반적으로 Swagger API, Spring Boot, Docker 등의 굉장히 많은 환경에서 설정(Conf) 파일 작성을 목적으로 YAML을 사용한다.
<br>
<hr>
<br>

### ✔ summary
- XML은 장황하지만 오류가 조금 있어도 무방하며, 주석이 가능하기 때문에 안정성을 중요시 할 때 사용한다.

- JSON은 간결하지만 문법 오류에 취약하여 `,` `{}` 등이 하나라도 빠지면 오류가 난다.

- YAML은 한줄 데이터가 아니기 때문에 사람이 보기 쉽지만 줄바꿈과 태그가 필수요소이다. (minify 하지 않는다.)
  - minify : 압축, 데이터를 한줄로 나열하는 것.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/6d60f7e5-b6c2-4b1c-8c2d-b8ed50f7b472)
<br>
<hr>
<br>

**Reference**<br>

[안경잡이개발자 : XML, JSON, YAML 형식 내용 정리 및 비교 분석](https://ndb796.tistory.com/251)<br>

