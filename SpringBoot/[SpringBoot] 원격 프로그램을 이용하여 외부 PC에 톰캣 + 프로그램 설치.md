## 외부 PC에 톰캣 + 프로그램 설치
- 나의 데스크탑과 노트북을 원격으로 연결한다.
  - 노트북은 핸드폰 핫스팟으로 인터넷 연결
<br>

- 테스크탑에서 노트북으로 파일 이동 후 노트북에서 설치를 진행한다.
  - 톰캣 설치
  - 톰캣에 war 파일 넣기
  - 톰캣 실행
  - 프로그램 구동 확인
<br>
<hr>
<br>

### ✔ 세션 만들기
- 파일을 가지고 있는 데스크탑에서 세션을 만든다.
  - 세션을 만드는 당사자가 참가하는 쪽의 컴퓨터를 제어함
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/39124d32-62b1-4e29-9b43-ee9e6e05e365)
<br>
<br>

- 세션을 만들었으면 왼쪽에 위치한 `167 153 217` 숫자를 노트북에서 입력하여 세션 참가를 한다.

- 이후 노트북에서 수락을 누르면 데스크탑에서 노트북의 화면이 보이게 된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/602bd466-26f9-4466-b300-9ae5de7719b2)
<br>
<hr>
<br>

### ✔ 파일 전송
- 상단 3번째 그림을 눌러 파일 전송을 한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/c17d418d-8131-4292-be57-6ead42a7db5a)
<br>
<br>

- 원하는 경로와 파일을 선택하여 전송 버튼을 클릭한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/fb7d67da-47eb-40f0-88b0-ba8b42f49c37)
<br>
<hr>
<br>

### ✔ 노트북에서 설치 진행
- 데스크탑에서 3개의 파일이 넘어 왔다.
  - 톰캣 압축 파일
  - ROOT.war
  - springBoot.war
<br>

- 톰캣 압축을 풀고 webapps 폴더에 war 파일을 넣는다.
  - webapps 폴더에 있는 기존 ROOT 폴더는 지운다. (톰캣 디폴트 HTML이 들어있음)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a14ab536-adb8-46c5-811b-190283ad0a81)
<br>
<br>

- 톰캣을 실행하여 war 파일의 압축이 풀리게 한다.
  - bin 경로로 이동하여 cmd 창을 연 후 `./catalina.bat run` 명령어 입력
  - JDK 버전 차이로 인한 에러 발생
  - 데스크탑 1.8 / 노트북 11

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/1c7f4a86-2e59-4bec-8ee8-ffcad3f4f416)
<br>
<br>

- 노트북의 JDK 11 삭제 후 1.8로 재설치

- 명령어 재실행
  - springBoot.war는 정상 실행
  - bootAndvue.war는 이전에 설정해놓은 LogBack 에러로 인해 정상 실행 불가능
<br>  

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/9ffc8b0a-cece-4ba0-8bf8-12806c96b7c0)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/890a0981-d9e0-4b96-a102-c7b5be9f8e1d)

- `logback-local.xml`에서 정의해놓은 로그가 남겨지는 경로가 노트북엡는 없기 때문에 발생하는 것으로 보인다.
