# Compose란 무엇인가?
- 컨테이너 여럿을 띄우는 도커 애플리케이션을 정의하고 실행하는 도구이다.

- 컨테이너 실행에 필요한 옵션을 docker-compose.yml이라는 파일에 적어둘 수 있고, 컨테이너 간 의존성도 관리할 수 있어서 좋다.

- 웹 서비스는 일반적으로 프론트엔드 서버, 벡엔드 서버, 데이터베이스 서버로 구성되기 때문에<br>
각 서버를 docker container로 연결하여 동작시키고 docker compose를 사용하여 해당 컨테이너들을 관리하는 것이다.

- 즉, 여러 개의 Docker 컨테이너들을 하나의 서비스로 정의하고 구성해 하나의 애플리케이션으로 만드는 것이다.
<br>
<hr>
<br>

## ✔️ compose를 사용해야 하는 이유
- 여러 컨테이너를 하나의 앱으로 동작시킬 때, compose를 사용하지 않는다면 각 컨테이너를 실행해야한다.

- 만약 backend와 frontend를 compose로 구성하였다면 아래와 같이 개별로 실행해야한다.
  - Jilli-WBS의 docker-compose.yml 기준으로 작성하였음
<br>

**docker-compose.yml**
```yml
version: '2.2'

services:
  frontend:
    container_name: jilli-wbs-frontend
    build:
      context: .
      dockerfile: Dockerfile.frontend
    ports:
      - "8086:8086"
    # environment:
      # ES_JAVA_OPTS: "-Xms512m -Xmx512m"
    networks:
      - wbs-net
    # depends_on:
      # - elasticsearch
    volumes:
      - ./frontend/src:/home/wbs/src
    expose:
      - "8086"
  backend:
    container_name: jilli-wbs-backend
    build:
      context: .
      dockerfile: Dockerfile.backend
    ports:
      - "8085:8085"     # service port
      - "9229:9229"     # debug port
      - "9228:9228"     # jest debug port
    # environment:
      # ES_JAVA_OPTS: "-Xms512m -Xmx512m"
    networks:
      - wbs-net
    # depends_on:
      # - elasticsearch
    volumes:
      - ./backend/src:/home/wbs/src
      - ./backend/test:/home/wbs/test
      - ./backend/public:/home/wbs/public
      - ./backend/data:/home/wbs/data
    expose:
      - "8085"
networks:
  wbs-net:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 192.168.253.0/24
          ip_range: 192.168.253.0/24
```
<br>


**frontend 개별 실행 시작**
```
docker build -t jilli-wbs-frontend -f Dockerfile.frontend .
```
> -t jilli-wbs-frontend : 빌드된 이미지에 태그를 지정<br>
-f Dockerfile.frontend : 사용할 Dockerfile을 지정<br>
. : 현재 디렉토리를 컨텍스트로 사용
<br>

- jilli-wbs-frontend 라는 이름으로 이미지 생성 완료

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/2993fbb6-35ae-4f06-bc93-2f2870cc1803)
<br>

```
docker run -d --name jilli-wbs-frontend -p 8086:8086 -v $(pwd)/frontend/src:/home/wbs/src jilli-wbs-frontend
```
> -d : 컨테이너를 백그라운드에서 실행<br>
--name jilli-wbs-frontend : 컨테이너의 이름을 지정<br>
-p 8086:8086 : 호스트의 포트 8086을 컨테이너의 포트 8086에 매핑<br>
-v $(pwd)/frontend/src:/home/wbs/src : 호스트의 frontend/src 디렉토리를 컨테이너의 /home/wbs/src 디렉토리에 마운트<br>
jilli-wbs-frontend : 실행할 이미지 이름을 지정
<br>

- frontend 컨테이너 생성 및 실행 완료

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/72064d6c-b941-4e20-bd57-75894075fba2)
<br>
<br>

**backend 개별 실행 시작**
```
docker build -t jilli-wbs-backend -f Dockerfile.backend .
```
> -t jilli-wbs-backend : 빌드된 이미지에 태그를 지정<br>
-f Dockerfile.backend : 사용할 Dockerfile을 지정<br>
. : 현재 디렉토리를 컨텍스트로 사용
<br>

- jilliy-wbs-backend 라는 이름으로 이미지 생성 완료

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/66b9c83e-9c15-499c-81fa-d4c616e3321f)
<br>

```
docker run -d --name jilli-wbs-backend -p 8085:8085 -p 9229:9229 -p 9228:9228 \
  -v $(pwd)/backend/src:/home/wbs/src \
  -v $(pwd)/backend/test:/home/wbs/test \
  -v $(pwd)/backend/public:/home/wbs/public \
  -v $(pwd)/backend/data:/home/wbs/data \
  jilli-wbs-backend
```
> -d : 컨테이너를 백그라운드에서 실행
--name jilli-wbs-backend : 컨테이너의 이름을 지정
-p 8085:8085: 호스트의 포트 8085를 컨테이너의 포트 8085에 매핑<br>
-p 9229:9229: 호스트의 포트 9229를 컨테이너의 포트 9229에 매핑<br>
-p 9228:9228: 호스트의 포트 9228을 컨테이너의 포트 9228에 매핑
<br>

- backend 컨테이너 생셩 및 실행 완료

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/d710f7f4-3bfc-46e2-8853-cd82fc0ce1fc)
<br>
<br>

- 💡 이처럼 컨테이너를 일일히 개별 실행해줘야하는 번거로움을 줄이고 관리를 편하기 하기 위해 compose 사용
<br>
<hr>
<br>

## ✔️ compose 파일의 옵션
**wbs의 docker-compose.yml**

```yml
version: '2.2'

services:
  frontend:
    container_name: jilli-wbs-frontend
    build:
      context: .
      dockerfile: Dockerfile.frontend
    ports:
      - "8086:8086"
    # environment:
      # ES_JAVA_OPTS: "-Xms512m -Xmx512m"
    networks:
      - wbs-net
    # depends_on:
      # - elasticsearch
    volumes:
      - ./frontend/src:/home/wbs/src
    expose:
      - "8086"
  backend:
    container_name: jilli-wbs-backend
    build:
      context: .
      dockerfile: Dockerfile.backend
    ports:
      - "8085:8085"     # service port
      - "9229:9229"     # debug port
      - "9228:9228"     # jest debug port
    # environment:
      # ES_JAVA_OPTS: "-Xms512m -Xmx512m"
    networks:
      - wbs-net
    # depends_on:
      # - elasticsearch
    volumes:
      - ./backend/src:/home/wbs/src
      - ./backend/test:/home/wbs/test
      - ./backend/public:/home/wbs/public
      - ./backend/data:/home/wbs/data
    expose:
      - "8085"
networks:
  wbs-net:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 192.168.253.0/24
          ip_range: 192.168.253.0/24
```
<br>

➡️ **version**
- yaml 파일 포맷의 버전을 나타냄
<br>
<br>

➡️ **services**
- 서비스는 도커 컴포즈로 생성할 컨테이너 옵션을 정의

- 하나의 프로젝트로서 도커 컴포즈에 의해 관리

```
services:
  container_1:
    image: ...
  container_2:
    image: ...
```
<br>

```yml
services:
  frontend:
    container_name: jilli-wbs-frontend
```
> frontend 서비스를 정의<br>
컨테이너의 이름은 jilli-wbs-frontend으로 설정
<br>

```yml
build:
      context: .
      dockerfile: Dockerfile.frontend
```
> frontend 서비스의 Docker 이미지를 빌드할 때 사용할 정보<br>
context: Dockerfile이 위치한 경로를 지정, 여기서는 현재 디렉토리(.)를 사용
dockerfile: 사용할 Dockerfile의 이름을 명시, Dockerfile.frontend 파일을 사용하여 이미지를 빌드
<br>

```yml
ports:
      - "8086:8086"
```
> 호스트와 컨테이너 간의 포트 매핑을 설정<br>
호스트의 포트 8086을 컨테이너의 포트 8086으로 매핑
<br>

```yml
networks:
      - wbs-net
```
> wbs-net이라는 이름의 Docker 네트워크에 frontend 서비스를 연결
<br>

```yml
volumes:
      - ./frontend/src:/home/wbs/src
```
> 호스트의 ./frontend/src 디렉토리를 컨테이너 내의 /home/wbs/src 디렉토리로 마운트<br>
이를 통해 소스 코드를 컨테이너 내부에서 접근할 수 있습니다.
<br>

```yml
expose:
      - "8086"
```
> 컨테이너가 사용하는 포트 8086을 외부에 노출
<br>

- backend 또한 동일한 설정을 가짐
<br>
<br>

➡️ **networks**
- 도커 네트워크에 연결하는 속성을 설정

- 네트워크 종류, 명칭, subnet 등

```yml
networks:
  wbs-net:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 192.168.253.0/24
          ip_range: 192.168.253.0/24
```
> wbs-net이라는 이름의 Docker 네트워크를 정의<br>
bridge 네트워크 드라이버를 사용하여 브릿지 네트워크를 생성<br>
192.168.253.0/24 서브넷과 IP 범위를 지정
<br>

- [subnet 설정하는 이유](https://github.com/yejun95/Today-I-Learned/blob/master/CI_CD/Docker%5D%20conflicts%20with%20network%EC%97%90%20%EC%9D%98%ED%95%9C%20failed%20to%20create%20network%20%ED%98%84%EC%83%81%20%EB%B6%84%EC%84%9D.md)

<br>
<hr>
<br>

**Reference**<br>

[snooby : Docker-compose 란](https://velog.io/@baeyuna97/Docker-compose-%EB%9E%80)<br>
[seunghwaan : 도커 컴포즈(Docker compose) - 개념 정리 및 사용법](https://seosh817.tistory.com/387)<br>
