## DB 스네이크 케이스 -> 자바 카멜 케이스 변경
- 기본적으로 DB는 `_` 언더바를 이용한 스네이크 케이스를 사용한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/fb1a180c-f2fd-4cb7-93fe-a3ffdd404c8b)
<br>
<br>

- 그러나 자바 VO 객체는 카멜케이스를 사용한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/458714f4-ac80-47c4-9f70-08a28c69a102)
<br>
<br>

- 이 때문에 둘을 동기화 시키기 위하여 `application.properties`에서 Mybatis 설정을 해줘야 한다.
<br>
<hr>
<br>

### ✔ application.properties
- `mybatis.configuration.map-underscore-to-camel-case=true`

- 해당 코드를 true 설정해서 넣으면 자동으로 DB의 `_` 언더바가 카멜케이스로 변한다
