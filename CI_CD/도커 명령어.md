## 도커 명령어
- exec : 가동중인 컨테이너에 명령어 전달
  - `docker exec {컨테이너 아이디} {명령어 키워드}`
<br>

- it : 컨테이너 내부에 들어갈 수 있게 해주는 키워드
  - `docker exec -it {컨테이너 아이디} {명령어 키워드}`
<br>

- cp : 파일 복사 (호스트 파일 -> 컨테이너 or 컨테이너 파일 -> 호스트)
  - `docker cp {복사할 파일 경로} {컨테이너 이름}:{컨테이너 내부 파일 경로}`
  - Ex) 호스트에 있는 test.txt 파일을 컨테이너의 /test 경로로 복사
  - `docker cp test.txt alpine-container:/test`
 <br>
