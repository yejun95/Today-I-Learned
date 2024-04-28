# kubectl 이란
- 쿠버네티스 클러스터에 명령을 내리는 CLI(Command Lint Interface)이다.

- 즉, 쿠버네티스에게 "웹서버 n개 실행해줘!" 라고 요청을 하는 것.
<br>
<hr>
<br>

### ✔ 기본 구조
- kubectl [command] [TYPE] [NAME] [flags]
  - command : 자원에 실행할 명령(create, get, delete, edit...)
  - TYPE : 자원의 타입(node, pod, service...)
  - name : 자원의 이름
  - flags : 부가적으로 설정할 옵션(--help, - o options...)
<br>
<hr>
<br>

### ✔ command 명령어
- apply : 원하는 상태를 적용, 보통 -f 옵션으로 파일과 함께 사용

- get : 리소스 목록 출력

- describe : 리소스 상태를 상세하게 출력

- delete : 리소스 제거

- logs : 컨테이너의 로그 출력

- exec : 컨테이너에 명령어 전달, 컨테이너에 접근할 때 주로 사용

- config : kubectl 설정을 관리
<br>
<hr>
<br>

**Reference**<br>

[on_cloud : [Kubernetes] kubectl command](https://velog.io/@on_cloud/Kubernetes-kubectl-command)<br>
[it-zero : kubectl 명령어 구조](https://btcd.tistory.com/105)
  
