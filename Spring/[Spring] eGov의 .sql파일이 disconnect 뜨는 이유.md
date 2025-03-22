## 전자정부프레임워크에서 .sql 파일이 disconnect가 뜨는 현상
- 전정부에서 .sql파일을 만들어서 db를 연결해보면 처음에 disconnect로 되있을 것이다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/f3837e0b-5a57-40c2-83ea-b0a17b530052)
<br>

- 이처럼 디폴트 값이 disconnect로 되어 있는데, mysql start.bat 파일로 실행하다 보면 실제적으로
db에는 연결되어 접근할 수 있지만 해당 .sql파일은 사용하지 못해 쿼리를 쓰지 못하는 경우가 있다.

- 이는 Data Souce Explorer 탭에서 스키마를 더블클릭하여 connection을 해주면 손쉽게 해결된다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/920689bb-60ed-4fbb-9eac-1fa0de3903a2)
<br>

- 사용할 스키마를 더블클릭

- 아래와 같이 스키마에 작은 표시가 생기고, connect로 표시가 변경된다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/23dc6b5c-828f-4b1d-b77e-b721998ae420)




