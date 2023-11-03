## 파싱(Parsing)이란 무엇인가?
- 원하는 데이터를 추출하여 가공하기 쉬운 형태로 바꾸는 것.

- 데이터의 형태는 무궁무진하며, 정형화된 형식(list, array)등을 제외하면 보통 다루기가 쉽지 않다.

- 그렇기 때문에 파싱을 해서 내가 사용하기 편하게 번역하는 작업을 거쳐야 한다.
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/5a4e0100-cd01-4a81-9226-9842ab9df6dc)

- 데이터를 파싱(번역)하기 위한 함수나 프로그램을 parser라고 한다.
  - XML parser
  - cookie parser
  - body parser
 
- parser에서 번역을 한 후 parse tree까지 만드는 것이 parsing이다.
  - Compiler나 Interpreter에서 원시 프로그램을 읽어 들여 그 문장의 구조를 알아내는 구문분석(parsing)을 행한다.
  - 즉 parser가 parsing을 하는 것이다.
<br>
<hr>
<br>

### ✔ What Is The Parsing?
- 문서의 내용을 토큰(token)으로 분석하고, 구조를 반영한 파스트리(parse tree)를 생성하는 과정이다.
<br>

**토큰이란?**
- 언어가 사용하는 기본 단어를 말한다.

- 토큰은 구문적으로 의미를 갖는 최소의 단위이며 우리가 작성하는 프로그램은 모두 이러한 토큰으로 이루어진다.

- 공백문자는 문자열 내에서 사용된 경우가 아니면 아무런 의미를 가지지 않는다.

```
ex) java안에서 String name = request.getParameter("name");의 토큰은 

String, name, =, request, ., getParameter, (, "name" ,) , ; -> 이게 하나의 토큰들이라고 생각하면 된다. 
```
<br>
<br>

**파스트리란?** 
- 파스트리란 어떤 문장을 트리구조로 나타낸 것을 의미하는데 파스트리, 파싱트리, 어원트리는 모두 같은 말이다.
  
- 여기서 트리란 자료구조의 하나로서 일종의 그래프이다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/5f314f00-9556-4163-a0df-d90e076458ec)

**왜 트리구조로 만들까?**
- 계층적 구조로 표현하여 코드간 연결을 확인한다.
  - token으로 분석하기 때문
  - 쪼개진 데이터를 원하는 경로로 원하는 만큼 번역
<br>

- 코드 최적화를 통한 중복 계산 제거, 불필요 연산을 하지 않기 위함이다.
  - ex) 비선형자료, 2진트리
<br>
<hr>
<br>

**Reference**<br>

[na27 : Parsing (파싱) 이란? Parser (파서) 란?](https://na27.tistory.com/230)<br>
[곽구 : 파싱이란? Parsing 의 뜻은 무엇일까?](https://solog4something.tistory.com/13)<br>
[전봇대파괴자의 IT 베이스캠프 : 파싱(Parsing)이란 무엇인가?](https://hengbokhan.tistory.com/134)
