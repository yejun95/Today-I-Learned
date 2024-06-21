# Dockerfile 이란?
- docker에서 이미지를 생성하기 위한 용도로 작성하는 파일이다.

- 만들 이미지에 대한 정보를 기술해 둔 템플릿이라고 보면 된다.

- 도커 이미지 생성 스크립트
  - `docker build [옵션] [작성한 dockerfile 경로]`
<br>

- Dockerfile 예시

**wbs의 Dockerfile.frontend**
```yml
FROM node:18.16.0-alpine

# install simple http server for serving static content
RUN npm install -g http-server

# make the 'app' folder the current working directory
WORKDIR /home/wbs

# copy both 'package.json' and 'package-lock.json' (if available)
COPY ./frontend/package*.json ./
COPY ./frontend/quasar.config.js ./
COPY ./frontend/tsconfig.json ./
COPY ./frontend/.eslintrc.js ./
COPY ./frontend/index.html ./

# install project dependencies
RUN npm install

CMD npm run dev

EXPOSE 8086
```
<br>
<br>

## ✔️내부 코드 설명

➡ **FROM**
- 도커파일에서 베이스 이미지를 지정하는 지시어
 -
- FROM에서 베이스 이미지와 태그를 지정하면 registry에서 해당 이미지를 pull 받아옴

- `node:18.16.0-alpine` : 해당 node 버전 이미지를 다운로드하여 새로 만들 이미지의 기초가 되도록 구성함
<br>
<br>

➡ **RUN**
- command 를 실행(run)하여 새 이미지에 포함시키는 역할

- 컨테이너에 필요한 라이브러리가 존재하거나, 특정 파일 혹은 디렉토리가 필요할 때가 있다.

- 이때, RUN 뒤에 라이브러리 설치 명령어 또는 파일/디렉토리 생성 명령어를 작성하면 이미지에 반영된다.

- `RUN npm install -g http-server` : Node.js를 기반으로 동작하는 정적 웹 서버
<br>
<br>

➡ **WORKDIR**
- 작업 디렉토리를 설정

- 이동하려는 디렉토리가 존재하지 않을 경우 해당 디렉토리를 생성하여 이동

- `WORKDIR /home/wbs` : /home/wbs 경로를 작업 디렉토리로 설정
<br>
<br>

➡ **COPY**
- Host 내에 있는 파일 또는 디렉토리를 컨테이너의 파일시스템으로 복사

- COPY ./frontend/package*.json ./ : frontend폴더 내 package.json 파일을 컨테이너의 './' 디렉토리로 복사
  - 작업 디렉토리인 WORKDIR(/home/wbs)로 복사가 된다.
<br>
<br>

➡ RUN npm install
- package.json을 먼저 COPY하고 모듈 다운로드를 해야하기 때문에 스크립트가 COPY 아래에 위치
<br>
<br>

➡ **CMD**
- 컨테이너가 시작될 때 실행할 커맨드를 지정

- 언뜻보면 RUN과 유사하지만 다르다.
  - RUN : 이미지를 빌드할 때 실행
  - CMD : 이미 만들어진 이미지로부터 도커 컨테이너를 시작할 때 실행
<br>

- `CMD npm run dev` : 컨테이너가 실행될 때, frontend app을 실행
<br>
<br>

➡ **EXPOSE**
- 컨테이너가 외부에 노출할 포트 번호를 의미

- 쉽게 말해 컨테이너에 할당하는 포트이다.
<br>
<hr>
<br>

**Reference**<br>

[토람이 : docker :: 도커파일(Dockerfile) 의 개념, 작성 방법/문법, 작성 예시](https://toramko.tistory.com/entry/docker-%EB%8F%84%EC%BB%A4%ED%8C%8C%EC%9D%BCDockerfile-%EC%9D%98-%EA%B0%9C%EB%85%90-%EC%9E%91%EC%84%B1-%EB%B0%A9%EB%B2%95%EB%AC%B8%EB%B2%95-%EC%9E%91%EC%84%B1-%EC%98%88%EC%8B%9C)
