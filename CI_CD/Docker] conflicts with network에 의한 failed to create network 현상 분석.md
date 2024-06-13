# conflicts with network에 의한 failed to create network 현상 분석
- 신규 앱(o2.admin)을을 로컬에서 docker로 실행하던 도중 에러가 발생하여 해당 현상을 파악하기 위해 이슈 등록
<br>
<hr>
<br>

## ✔ Error
```
bjsystems@DESKTOP-S70PNEV MINGW64 ~/Desktop/service/admin (main)
$ docker compose up frontend
[+] Building 69.6s (16/16) FINISHED
 => [frontend internal] load build definition from Dockerfile.frontend                                                                                                                                                                                                                                         0.0s 
 => => transferring dockerfile: 954B                                                                                                                                                                                                                                                                           0.0s 
 => [frontend internal] load .dockerignore                                                                                                                                                                                                                                                                     0.0s 
 => => transferring context: 2B                                                                                                                                                                                                                                                                                0.0s 
 => [frontend internal] load metadata for docker.io/library/node:12.22-alpine                                                                                                                                                                                                                                  1.7s 
 => [frontend auth] library/node:pull token for registry-1.docker.io                                                                                                                                                                                                                                           0.0s 
 => [frontend  1/10] FROM docker.io/library/node:12.22-alpine@sha256:d4b15b3d48f42059a15bd659be60afe21762aae9d6cbea6f124440895c27db68                                                                                                                                                                          0.0s 
 => [frontend internal] load build context                                                                                                                                                                                                                                                                     4.8s 
 => => transferring context: 645.12kB                                                                                                                                                                                                                                                                          4.8s 
 => CACHED [frontend  2/10] RUN apk add --update --no-cache curl py-pip                                                                                                                                                                                                                                        0.0s 
 => CACHED [frontend  3/10] RUN npm install -g http-server && npm install -g node-gyp                                                                                                                                                                                                                          0.0s 
 => CACHED [frontend  4/10] WORKDIR /admin/frontend                                                                                                                                                                                                                                                            0.0s 
 => [frontend  5/10] COPY ./frontend/package*.json ./                                                                                                                                                                                                                                                          0.0s 
 => [frontend  6/10] COPY ./frontend/tsconfig.json ./                                                                                                                                                                                                                                                          0.0s 
 => [frontend  7/10] COPY ./frontend/vue.config.js ./                                                                                                                                                                                                                                                          0.0s 
 => [frontend  8/10] COPY ./frontend/cypress.json ./                                                                                                                                                                                                                                                           0.0s 
 => [frontend  9/10] COPY ./frontend/*.js ./                                                                                                                                                                                                                                                                   0.0s 
 => [frontend 10/10] RUN npm install                                                                                                                                                                                                                                                                          56.8s 
 => [frontend] exporting to image                                                                                                                                                                                                                                                                              5.9s 
 => => exporting layers                                                                                                                                                                                                                                                                                        5.9s 
 => => writing image sha256:a7eae9de2859b5ea9664448f6943a36e13035ea358576c091c66fbcba6793cc1                                                                                                                                                                                                                   0.0s
 => => naming to docker.io/library/admin-frontend                                                                                                                                                                                                                                                              0.0s 
[+] Running 1/0
 ✘ Network admin_admin-net  Error                                                                                                                                                                                                                                                                              0.0s 
failed to create network admin_admin-net: Error response from daemon: cannot create network d91178b13ebc5fdc698255e55056cc5fce905d458763a8c138024382538aae44 (br-d91178b13ebc): conflicts with network ae0f98ed537005cd957783b9582e494d48f28f9aa12a37f712ccb1e4fe65e3f1 (br-ae0f98ed5370): networks have overlapping
 IPv4
```
<br>

- o2.admin 서비스를 로컬에서 실행하기 위해 docker로 실행중 발생
<br>
<hr>
<br>

## ✔ Cause
- `ae0f98ed5370`에 해당하는 Network ID와 새롭게 등록될 docker network IPv4 주소가 겹침

- 즉, 서로 다른 Docker 네트워크가 동일한 서브넷을 사용하려 할 때 발생

- 등록된 docker network 확인

```
$ docker network ls
NETWORK ID     NAME                         DRIVER    SCOPE
979ad377de56   bridge                       bridge    local
bd2f9de398db   contarct_contract-net        bridge    local
951d202170e0   contarct_contract-test-net   bridge    local
087ee35ce21e   host                         host      local
90f3d834be53   jilli_wbs-net                bridge    local
442e8d94372e   none                         null      local
ae0f98ed5370   pillar2_tax-net              bridge    local
```
> 에러에 나온 'ae0f98ed5370'은 pillar2_tax-net에 해당
<br>

- 즉, 기존에 docker에 등록한 pillar2_tax-net이 새롭게 등록하려는 admin 서비스와 서브넷 주소가 동일하기 때문에
admin 서비스를 docker로 실행하지 못한 것
<br>

- pillar2와 admin에 설정한 서브넷 주소를 docker-compose.yml 파일에서 확인 
<br>

**pillar2**
```yml
networks:
  tax-net:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 192.168.251.0/24
```
> subnet: 192.168.251.0/24
<br>
<br>

**admin**
```yml
networks:
  admin-net:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 192.168.251.0/24
          ip_range: 192.168.251.0/24
```
> subnet: 192.168.251.0/24
<br>

- 두 앱의 서브넷이 동일한 것을 확인
<br>
<hr>
<br>

## ✔ Solved
**제 1안**
- 기존에 생성된 pillar2_tax-net을 docker container를 삭제
  - desktop docker로 진행
<br>

<img src="https://github.com/bjsystems/rnd/assets/121341413/1df4c559-6dfa-4e78-8225-4b81e1c9c855" width="1400px">
<br>

- 이후 admin에서 docker 실행

```
$ docker compose up frontend
```
<br>
<br>

**제 2안**
- 기 등록된 pillar2 서브넷은 유지

- 새로 등록할 admin의 서브넷을 변경시켜 도커 실행

- admin의 docker-compose.yml 파일 오픈
  - 최 하단 스크립트쪽에 있는 subnet을 변경
  - 변경 전 : 192.168.251.0/24
  - 변경 후 : 192.168.253.0/24

```yml
networks:
  admin-net:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 192.168.253.0/24
          ip_range: 192.168.253.0/24
```
<br>

- admin에서 도커 재실행

```
$ docekr compose up frontend

...
...

admin-frontend  | No type errors found
admin-frontend  | Version: typescript 3.8.3
admin-frontend  | Time: 3832ms
admin-frontend  | <s> [webpack.Progress] 100%
admin-frontend  | 
admin-frontend  | 
admin-frontend  | 
admin-frontend  |   App running at:
admin-frontend  |   - Local:   http://localhost:8080/
admin-frontend  | 
admin-frontend  |   It seems you are running Vue CLI inside a container.
admin-frontend  |   Access the dev server via http://localhost:<your container's external mapped port>/
admin-frontend  | 
admin-frontend  | 
admin-frontend  |   Note that the development build is not optimized.
admin-frontend  |   To create a production build, run npm run build.
admin-frontend  | 
admin-frontend  | 
```
> 도커 정상 실행
<br>
<hr>
<br>

## ✔ docker network
- 현재 위와 같은 에러가 발생한 이유는 도커 네트워크 설정에 있어서 충돌이 발생하였기 때문이다.

- 그렇다면 도커 네트워크가 무엇을 말하는지 확인해보자.
<br>

- 먼저 컨테이너는 격리된 환경에서 실행되며, 이는 각 컨테이너가
자체 네트워크 인터페이스와 IP 주소를 가질 수 있음을 의미한다.

- 즉, 도커는 컨테이너에 내부 IP를 순차적으로 할당한다.

- 그러나 컨테이너 간 통신 또는 컨테이너 - 호스트간 통신이 필요한 경우가 생기는데, 이때는 내부 IP 만으로
통신이 불가능하다.

- 이를 위해 도커 네트워크를 통해서 서로 간 통신을 가능하게 한다.
<br>
<hr>
<br>

## ✔ docker network 구조
![image](https://github.com/bjsystems/rnd/assets/121341413/1ec0e1d4-7107-4a73-bdf5-3cdd8c161726)
<br>

### veth
- 컨테이너의 내부 IP를 외부와 연결해 주는 역할을 하는 가상 인터페이스

- 컨테이너가 생성될 때 veth가 동시에 생성되며, 이 veth를 호스트의 eth0과 연결시킴으로써 외부와의 통신이 가능

- 그리고 이 연결을 docker0과 같은 브리지가 수행
<br>


### docker0
- 도커가 설치될 때, 기본적으로 구성되며, 호스트의 네트워크인 eth0과 컨테이너의 네트워크와
연결을 해주는 역할을 함.

- 하나의 브리지 안에서 컨테이너간 연결을 해줄 수 있으며, 새로운 브리지를 생성하는 것도 가능
<br>

### eth0
- 실제 외부와 연결하기위해 IP가 할당된 호스트 네트워크 인터페이스 - 내 PC
<br>
<br>

- 즉, 내부 IP 설정 및 docker0 라는 브릿지에 연결하기 위해서 docker.compose.yml에 설정을 잡아주는 것
  - 물론 아무런 설정을 하지 않을 경우 도커에서 자동으로 생성
  - 브릿지는 추가 생성이 가능하기 때문에 특정 브릿지로 연결을 원하는 경우 설정을 필히 잡아야 함
<br>
<hr>
<br>

## ✔ docker network driver
- 이전에 docker network의 목록을 확인하였을 때, bridge, host, none 이렇게 존재하는 걸 볼 수 있었다.

```
bjsystems@DESKTOP-S70PNEV MINGW64 ~/Desktop/service/pillar2 (develop)
$ docker network ls
NETWORK ID     NAME                         DRIVER    SCOPE
5cafcffb1c95   admin_admin-net              bridge    local
979ad377de56   bridge                       bridge    local
bd2f9de398db   contarct_contract-net        bridge    local
951d202170e0   contarct_contract-test-net   bridge    local
087ee35ce21e   host                         host      local
90f3d834be53   jilli_wbs-net                bridge    local
442e8d94372e   none                         null      local
ae0f98ed5370   pillar2_tax-net              bridge    local
```
<br>

- 이처럼 driver 종류가 나뉘어 있는데, 이를 확인해보자.
<br>

### host
- 호스트의 네트워크 주소를 사용 (= 내 PC)

- 컨테이너가 실행될 때, 새로운 내부 IP를 할당받을 필요 없이 호스트의 네트워크를 곧바로 사용

- 즉, 내 PC에서 애플리케이션을 실행한 것과 같아진다.

- dockker network 구조로 보면 'docker0' 브릿지 없이 'eth0'과 연결
<br>

### bridge
- docker network 구조에서 말한 'docker0' 이며, 컨테이너간 통신을 가능하게 한다.

- 기본 네트워크 드라이버로, 도커가 자동으로 생성

- 현재 기본 bridge 설정 확인

```
$ docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "979ad377de560dd500ef9db0d080507f90db97f833931337441c8d93f1eb65c4",
        "Created": "2024-05-24T00:52:35.839193707Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```
> 현재 docker의 default bridge 주소 범위 -> Subnet": "172.17.0.0/16"
<br>

- 출력에서 IP 세부 정보를 살펴보면, 172.17.x.x 대역대의 내부 IP를 사용하고 있음을 볼 수 있다.

-  해당 브리지와 연결되는 컨테이너는 172.17.x.x 대역대의 IP를 순차적을 부여받게 된다.

- 그러나 pillar2와 admin의 docker network 설정을 보면 동일한 bridge를 사용하지만 ip 주소 범위는 개별로 설정

- pillar2의 docker-compose.
```
networks:
  tax-net:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 192.168.251.0/24
```
> 서브넷 설정을 따로 잡아주었음
<br>

- pillar2의 docker ip 주소 확인
  - `docker ps`를 먼저 입력하여 container id 확인
```
$ docker ps
CONTAINER ID   IMAGE               COMMAND                   CREATED        STATUS             PORTS                    NAMES
f75ae70fd5ca   admin-frontend      "docker-entrypoint.s…"   3 hours ago    Up 3 hours         0.0.0.0:8080->8080/tcp   admin-frontend
070d5784b0b5   kimchang-frontend   "docker-entrypoint.s…"   22 hours ago   Up About an hour   0.0.0.0:9001->9001/tcp   pillar2-frontend

$ docker inspect 070d5784b0b5
...
...
...

  "Aliases": [
                        "pillar2-frontend",
                        "frontend",
                        "070d5784b0b5"
                    ],
                    "NetworkID": "ae0f98ed537005cd957783b9582e494d48f28f9aa12a37f712ccb1e4fe65e3f1",
                    "EndpointID": "d023ee2af09b0a2711ccc152c2f509eda216b28cc8b2a060138cc9343089fec5",
                    "Gateway": "192.168.251.1",
                    "IPAddress": "192.168.251.2",
                    "IPPrefixLen": 24,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:c0:a8:fb:02",
                    "DriverOpts": null
```
> 많은 내용이 포함되어있는데, 제일 하단에 IPAddress 속성을 보면 기존에 지정해놓은 서브넷 범위 안에서
IP 주소가 할당된 것을 볼 수 있다.
<br>

### none & null
- 말 그대로, 아무런 네트워크를 사용하지 않는 것

- 컨테이너를 어떠한 외부와도 연결하지 않을 때 사용
<br>
<hr>
<br>

**Reference**<br>

[vinca's vel🦃g : 도커 네트워크 (Docker Network)](https://velog.io/@rockwellvinca/Docker-Docker-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)<br>
[Rimo : Docker Network : 호스트와 컨테이너를 위한 네트워크를 구성해보자](https://rimo.tistory.com/entry/Docker-Network-%ED%98%B8%EC%8A%A4%ED%8A%B8%EC%99%80-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%EB%A5%BC-%EC%9C%84%ED%95%9C-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%EB%A5%BC-%EA%B5%AC%EC%84%B1%ED%95%B4%EB%B3%B4%EC%9E%90#%EB%8F%84%EC%BB%A4%20%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%EC%9D%98%20%EA%B5%AC%EC%A1%B0-1)<br>
[hyeongjun-hub.log : 도커와 도커 네트워크](https://velog.io/@hyeongjun-hub/%EB%8F%84%EC%BB%A4%EC%99%80-%EB%8F%84%EC%BB%A4-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)<br>
[승어리(Won) : 06. 도커 네트워크 (Docker Network)](https://captcha.tistory.com/70)<br>
