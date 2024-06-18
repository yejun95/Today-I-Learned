# port와 expose의 차이
- docker.compose.yml 내용을 보면 ports와 expose가 나와있다.
  - wbs 프로젝트의 내용임
<br>

- 아래는 frontend 블록 코드만 가져왔음
```yml
frontend:
    container_name: jilli-wbs-frontend
    build:
      context: .
      dockerfile: Dockerfile.frontend
    ports:
      - "8086:8086"
    environment:
      # ES_JAVA_OPTS: "-Xms512m -Xmx512m"
      - CHOKIDAR_USEPOLLING=true
    networks:
      - wbs-net
    # depends_on:
      # - elasticsearch
    volumes:
      - ./frontend/src:/home/wbs/src
    expose:
      - "8086"
```
<br>

- expose는 "8086"로 1개이고, port도 "8086"으로 1개가 존재한다.
<br>

## ✔️ 차이점
- "port"는 외부 호스트와 컨테이너 내부 포트를 연결하는 역할을 한다.
  - `외부 호스트에서 접근할 포트 : 내부 컨테이너 포트`
  - `8086 : 8086`
<br>

- "expose"는 내부 컨테이너 포트를 설정하고, 노출시키는 것
  - 연결된 컨테이너끼리 접근 가능한 포트를 설정하는 것
  - 내부 네트워크에서만 유효하며, 호스트와의 포트 매핑을 설정하지 않음
  - 외부에서는 접근할 수 없고, 같은 Docker 네트워크에 있는 다른 컨테이너들만 접근 가능
<br>

- ports 와 expose는 모두 컨테이너 포트를 노출시키는 역할을 한다는 점에서는 동일하다.
  - 단, expose로 노출시키는 경우, 호스트 내부에서만 접근이 가능
  - 반면, ports로 노출시키는 경우에는 호스트 외부의 다른 호스트들도 ports 에 설정한 호스트 포트를 통해 접근이 가능
  - 내부적으로 컨테이너 연결만이 목적이라면 expose로 충분. 
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/d982db16-c0f3-4e13-b3f9-524913ea577c)
> Client 요청에 따라 알맞은 port로 연결
같은 브릿지 안에 컨테이너끼리는 expose 포트를 통해 연결 가능
<br>
<hr>
<br>

## ✔ 컨테이너간 통신 확인
- 동일 네트워크에 있는 컨테이너끼리 통신이 가능하다고 하였다.

- 이를 직접 확인해보자.
  - ping 사용
<br>

- 현재 도커 네트워크 확인

```
$ docker network ls
NETWORK ID     NAME                         DRIVER    SCOPE
c401a4476f0c   bridge                       bridge    local
bd2f9de398db   contarct_contract-net        bridge    local
951d202170e0   contarct_contract-test-net   bridge    local
087ee35ce21e   host                         host      local
442e8d94372e   none                         null      local
ae0f98ed5370   pillar2_tax-net              bridge    local
f6c3144723ef   wbss_wbs-net                 bridge    local
```
> host, bridge, null 3개 존재
bridge가 위쪽 그림에서 보여준 'docker0'와 같은 중간 네트워크를 말함
전부 각기 다른 이름은 가진 bridge network임
<br>

- 실행중인 컨테이너 확인

```
$ docker container ls
CONTAINER ID   IMAGE           COMMAND                   CREATED             STATUS         PORTS                                                      NAMES
7debc8e21c82   kimchang-frontend   "docker-entrypoint.s…"   14 minutes ago      Up 10 seconds   0.0.0.0:9001->9001/tcp                                     pillar2-frontend
7161c0bb98a6   wbss-backend        "docker-entrypoint.s…"   About an hour ago   Up 13 minutes   0.0.0.0:8085->8085/tcp, 0.0.0.0:9228-9229->9228-9229/tcp   jilli-wbs-backend
7ff90bfeca82   wbss-frontend       "docker-entrypoint.s…"   About an hour ago   Up 13 minutes   0.0.0.0:8086->8086/tcp                                     jilli-wbs-frontend
```
> wbs와 pillar2로 통신 확인 예정
둘다 bridge 네트워크를 가지고 있지만, subnet이 상이해 동일 네트워크는 아님
<br>

## ✔️ ping 테스트
- jilli-wbs-backend > jilli-wbs-frontend로 ping 확인

![image](https://github.com/bjsystems/rnd/assets/121341413/084f70a0-958d-4d3f-9290-90f51bac2fed)
> 정상 전송
<br>

- jilli-wbs-frontend > jilli-wbs-backend로 ping 확인

![image](https://github.com/bjsystems/rnd/assets/121341413/e2041e39-0b20-4473-940d-41828e200ddc)
> 정상 전송
<br>

- jilli-wbs-backend > pillar2-frtonend ping 확인

![image](https://github.com/bjsystems/rnd/assets/121341413/1894046f-8135-4595-b0be-12e0d70c79c6)
> 전송 실패
<br>

- pillar2-frontend 컨테이너를 jilli-wbs와 동일 네트워크로 할당 진행

**wbs의 network**
```yml
networks:
  wbs-net:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 192.168.253.0/24
```
<br>

**pillar2의 network를 이에 맞게 이름 및 subnet 변경**
```yml
version: '2.2'

services:
  frontend:
    image: kimchang-frontend
    container_name: pillar2-frontend
    build:
      context: .
      dockerfile: Dockerfile.frontend
    ports:
      - "9001:9001"
    networks:
      - wbss_wbs-net
    volumes:
      - ./frontend/src:/home/tax/src
      - ./frontend/cypress:/home/tax/cypress
      - ./frontend/public:/home/tax/public
      - ./frontend/vite.config.ts:/home/tax/vite.config.ts
    expose:
      - "9001"
    extra_hosts:
      - "backend:192.168.0.2"
  backend:
    image: kimchang-backend
    container_name: pillar2-backend
    build:
      context: .
      dockerfile: Dockerfile.backend
    ports:
      - "9000:8080"     # service port
      - "9229:9229"     # debug port
      - "9228:9228"     # jest debug port
    # environment:
      # ES_JAVA_OPTS: "-Xms512m -Xmx512m"
    networks:
      - wbss_wbs-net
    # depends_on:
      # - elasticsearch
    volumes:
      - ./backend/src:/home/tax/src
      - ./backend/test:/home/tax/test
      - ./backend/public:/home/tax/public
      - ./backend/data:/home/tax/data
networks:
  wbss_wbs-net:
    external: true
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 192.168.253.0/24
```
> 외부(wbs)에서 생성된 network이기 때문에 `external: true` 추가
<br>

- 기존 컨테이너 삭제 후 `docker compose up frontend`를 통해 컨테이너 재생성
```
$ docker compose up frontend
[+] Running 2/1
 ✔ Network pillar2_wbss_wbs-net  Created                                                                                                                                                                                       0.7s 
 ✔ Container pillar2-frontend    Created                                                                                                                                                                                       0.0s 
Attaching to pillar2-frontend
pillar2-frontend  | 
pillar2-frontend  | > kimchang-pillar2-frontend@1.0.0 dev
pillar2-frontend  | > vite --host 0.0.0.0
pillar2-frontend  | 
pillar2-frontend  | 
pillar2-frontend  | 
pillar2-frontend  |   VITE v4.5.2  ready in 667 ms
pillar2-frontend  | 
pillar2-frontend  | 
pillar2-frontend  |   ➜  Local:   http://localhost:9001/
pillar2-frontend  |   ➜  Network: http://192.168.251.2:9001/
```
<br>

- ping test 진행

- jilli-wbs -> pillar2

![image](https://github.com/bjsystems/rnd/assets/121341413/219a0dfd-8464-41a2-ab83-2a5bd487fc7f)
> 정상 전송
