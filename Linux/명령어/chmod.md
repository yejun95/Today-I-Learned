## chmod
- 특정 파일, 폴더에 접근 및 사용하기 위해서는 권한이라는 것이 꼭 필요한데
이에 대한 권한을 부여하는 명령어가 `chmod` 이다.
  -  각 사용자가 디렉토리 / 파일에 대한 접근 권한을 관리하는 명령어

- 보통 root는 모두 접근이 가능하지만, 각 개별 사용자가 필요할 때 사용한다.

- 사용법
```
chmod [권한값] [파일명]
```
<br>

- 파일권한 형식 : [디렉토리 1][소유자 권한 3][그룹 권한 3][전체 권한 3]
  - r : 읽기 권한 - 4
  - w : 쓰기 권한 - 2
  - x : 실행 권한 - 1
  - chmod의 권한값은 숫자의 합으로 표기
  <br>
  <hr>
  <br>

### ✔ 사용이유
- 사용자 별로 권한을 다르게하여 파일이나 디렉토리의 접근성을 허용 or 차단하기 위함이다.

- 예를 들어 중요 key인 `.pem`파일의 경우 아무나 읽을 수 있으면 해킹의 위험이 있다.
  - 때문에 `ssh -i`로 접속 시 `.pem`파일의 권한을 설정해야 정상 접속이 가능하다.

- 아래 그림과 같이 같은 디렉토리라도 허용과 차단이 나뉜다.

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/a33670af-5c32-4741-882f-8afd772417a7)
<br>
<br>

- 또한 같은 디렉토리라도 모두에게 허용할 수가 있다

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/17ed9a2a-2ebd-41a4-9a16-7ef9f1fee341)
<br>
<hr>
<br>

### ✔ 예시

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/ef802e5a-4fe3-4676-8a68-3d3fb939694f)
<br>

- 앞에 있는 `rwxr-x---` : [디렉토리 1][소유자 권한 3][그룹 권한 3][전체 권한 3]
  - 총 10자리로 되어 있는 것들이 권한이다.
  - 1번째 : 디렉토리 구분, 파일은 `-` / 디렉토리 `d` 로 표시
  - 2~4번째 : 소유자 권한
  - 5~7번째 : 그룹 권한
 
- sample_1 파일은 그룹이 읽기, 실행만 가능

- sample_2 파일은 그룹이 읽기, 쓰기, 실행이 모두 가능
<br>
<hr>

- 아래와 같이 3개의 명령이 실행 시 변경되는 결과 
```
chmod 777 sample_1

chmod 750 sample_2

chmod 444 sample
```
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/f62c5536-0700-48fe-a8a0-55c698fe8850)
<br>
<hr>
<br>

**Reference**<br>

[배워가는블로거 : chmod 설정](https://zamezzz.tistory.com/28)<br>
[서버이야기 : Linux 명령어 - chmod 명령어 사용법 알아보기(파일 권한 바꾸기)](https://server-talk.tistory.com/419)
