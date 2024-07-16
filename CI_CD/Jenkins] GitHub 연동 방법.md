# Jenkins와 gitHub 연동 방법
- 현재 Jenkins에 토큰이 등록되어 있고, pipeline이 설정되어 있다는 가정하에 진행
<br>
<hr>
<br>

## ✔ 1. 깃헙 토큰 발급
- settings -> Developer settings -> Personal access tokens

![image](https://github.com/bjsystems/rnd/assets/121341413/701c0684-3e95-400d-b0a4-ea3d0ed65223)
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/2a3ae829-3b05-4940-a6a6-86a9717008cc)
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/df543965-c156-445e-92be-6d684e6bd77c)
<br>

- token이름 / repo / admin:repo_hook 체크

![image](https://github.com/bjsystems/rnd/assets/121341413/149297a7-3c76-4ac8-8357-9289ccac1dd1)
<br>

- repo : 리포지토리와 관련된 다양한 작업을 수행할 수 있는 권한을 부여
  - 읽기, 쓰기 및 삭제 권한
  - 공개 및 개인 리포지토리에 대한 액세스
  - 파일 읽기/쓰기
  - 커밋 생성
  - 브랜치 관리
  - 기타 등
<br>

- admin:repo_hook :  리포지토리 webhook에 대한 관리 권한을 제공
  - webhook : 특정 이벤트가 발생할 때 외부 서버에 API 요청을 보내기 위한 메커니즘
  - webhook 생성, 수정, 삭제
  - webhook 테스트 및 재전송
<br>

- 요약
  - repo: 리포지토리의 모든 작업을 수행할 수 있는 권한 (읽기, 쓰기, 삭제 등)
  - admin:repo_hook: 리포지토리 webhook을 관리할 수 있는 권한 (생성, 수정, 삭제 등)
<br>

- 그 외 필요한 기능들은 필요에 따라 추가한다.

- 이후 하단에서 Generate token 버튼 클릭

![image](https://github.com/bjsystems/rnd/assets/121341413/0439169d-d08c-4c34-9d28-ce0678b75cda)
<br>

- token 값이 아래와 같이 빨간색으로 칠한 부분에 나타난다.
  - 💡 발행받은 토큰 값은 이후 확인이 불가능

![image](https://github.com/bjsystems/rnd/assets/121341413/7503aee7-d68a-4e8f-9bd6-a75b9c83a1ab)
<br>
<hr>
<br>

## ✔ Jenkins에 발급된 GitHub 토큰 등록
- Jenkins에 접속하여 메인화면의 왼쪽 메뉴에서 JenKins 관리 클릭

![image](https://github.com/bjsystems/rnd/assets/121341413/f3c55bbe-7adb-4677-a562-80f6ee88ec26)
<br>

- Credentials 클릭

![image](https://github.com/bjsystems/rnd/assets/121341413/1e05ee47-48c9-4e9f-8ef1-3e1c3b47524b)
<br>

- global 클릭

![image](https://github.com/bjsystems/rnd/assets/121341413/1a90828e-c2ef-4f77-a62a-818e5458402d)
<br>

- 우측 상단의 Add Creentials 클릭

![image](https://github.com/bjsystems/rnd/assets/121341413/8b1f7b8b-4e3f-45a7-bbbf-213c393f36ed)
<br>

- 아래와 같이 입력 후 Create

![image](https://github.com/bjsystems/rnd/assets/121341413/a429de93-90bd-4f76-a006-66cd52106f71)
<br>

- 차후 pipeline 설정 시 Credentials에 등록된 ID 값을 통해서 접근한다.

![image](https://github.com/bjsystems/rnd/assets/121341413/373ce628-0ffe-43a7-95c8-f847c808c100)
<br>

- wbs의 pipeline을 보면 github clone을 하기 위해 'bjsystems' credential 사용

![image](https://github.com/bjsystems/rnd/assets/121341413/1fcae929-0cc8-4915-b771-20f547969213)

- 현재는 기존에 만들어진 item을 기준으로 설명했기 때문에 Jenkins의 자세한 배포 방법은 아래 이슈 참조
  - [Jenkin 배포 구성](https://github.com/bjsystems/bj.pms/issues/65)
<br>
<hr>
<br>

## ✔ GitHub Repository Webhooks 설정
- 해당 섹션은 github에서 발생하는 이벤트(pull, push 등)를 감지하여 Jenkins에서 자동으로 빌드를
트리거하기 위해 설정하는 부분이다.

- 현재 bjsystems는 Jenkins에 접속해 수동으로 배포를 하고 있기에 해당 부분 설정이 필요없다.
<hr>

- 지금까지 Jenkins에서 GitHub와 연동을 위해 설정을 했다면,
GitHub에서도 Jenkins와 연동하기위한 webhook을 설정해야한다.

- 연동할 GitHub Repo -> Settings -> Webhooks -> Add webhook

![image](https://github.com/bjsystems/rnd/assets/121341413/9c1f1cf8-0deb-4765-9668-84656b85a0e7)
<br>

- Payload URL과 Content type 설정 후 Add webhook 버튼 클릭
  - Payload URL : http://Jenkins주소/github-webhook/
  - Content type : application/json

![image](https://github.com/bjsystems/rnd/assets/121341413/04b8a531-8e55-445f-a573-da1b584a1de7)
<br>

- 추가로 github 이벤트를 Jenkins에서 감지하기 위해서는 Jenkins의 프로젝트 설정에서 Trigger 체크를 해줘야한다.

![image](https://github.com/bjsystems/rnd/assets/121341413/d2e83168-a0a8-4c15-ae9a-7121ea22c37e)
<br>
<hr>
<br>

## ✔ webhook
- 앞서 말했지만 특정 이벤트가 발생 시 다른 URL로 API 호출을 할 수 있게 해주는 역할을 한다.

- 즉, github에 변화가 생기면 Jenkins로 API 호출을 하는 것이다.

![image](https://github.com/bjsystems/rnd/assets/121341413/f1c13eab-76eb-415a-863a-fb44d7c32321)
<br>

- 로컬에서 소스 수정 후 Github에 push한다.

- Github Webhook이 동작해 jenkins에게 API 요청을 날린다.

- jenkins는 Github로부터 온 API 요청을 받아서 저장소의 소스코드를 다운받고 프로젝트를 빌드하여 JAR파일로 만든다.

- jenkins가 JAR파일을 지정해놓은 서버에 배포하고 실행시킨다.
<br>
<hr>
<br>

**Reference**<br>

[Inpa Dev : Github 토근 발급 받기](https://inpa.tistory.com/475)<br>
[Inpa Dev : [Jenkins] Github(Git) 연동 하는 방법](https://inpa.tistory.com/entry/Jenkins-%F0%9F%A4%B5-GithubGit-%EC%97%B0%EB%8F%99-%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)<br>
[RUNGOAT : Jenkins와 Github 연동하기](https://velog.io/@rungoat/CICD-Jenkins%EC%99%80-GitHub-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0)<br>
[LJH : Github Webhook과 jenkins로 배포 자동화하기](https://velog.io/@znftm97/Github-Webhook%EA%B3%BC-jenkins%EB%A1%9C-%EB%B0%B0%ED%8F%AC-%EC%9E%90%EB%8F%99%ED%99%94%ED%95%98%EA%B8%B0)<br>
