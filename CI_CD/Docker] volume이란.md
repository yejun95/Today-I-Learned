# volume이란 무엇인가?
- docker에는 volume 부분이 존재하였는데, 이게 무엇을 뜻하는지 알아보자.

- 기본적으로 docker는 가상화 환경인 컨테이너에서 작업을 하고, 작업하는 모든 데이터는 컨테이너 내부에만 존재한다.

- 여기서 데이터 유지에 대한 문제점이 발생한다.

- 컨테이너를 삭제하면 작업했던 데이터가 모두 삭제되어 버리게 되는데, 컨테이너가 삭제되어도
데이터는 유지가 되어야 한다.
  - 컨테이너는 일회성으로 재배포시 컨테이너가 다시 생성되기 때문
<br>

- 이럴 때 사용하는것이 볼륨(volume)이다. 
<br>

**WBS의 docker-compose.yml**
```yml
frontend:
    volumes:
      - ./frontend/src:/home/wbs/src

backend : 
    volumes:
      - ./backend/src:/home/wbs/src
      - ./backend/test:/home/wbs/test
      - ./backend/public:/home/wbs/public
      - ./backend/data:/home/wbs/data
```
<br>

- 즉, 데이터 유지를 위해 컨테이너 내부 데이터가 변경되면 외부에도 똑같이 데이터가 맵핑되는 것이다.

- `./frontend/src:/home/wbs/src` 해당 코드로 예를 들어보자.
  - `home/wbs/src`에 있는 데이터를 항상 `./frontend/src`와 맵핑
  - 이처럼 실제 데이터 소스쪽과 도커쪽 소스를 맵핑시켜 데이터를 유지 시킨다.
<br>
<hr>
<br>

## ✔ volume 실습
- volume 생성
  - `docker volume create [생성할 볼륨 이름]`

```
$ docker volume create test_volume
test_volume
```
<br>

- 생성된 volume 확인

```
$ docker volume ls
DRIVER    VOLUME NAME
local     0f17c6ce23c564c6e0f971d0fba68897937d2237db259e3d66b6a53cb96f5004
local     0fec7747a8ed971d835a4f004e49cc595fbfced01e892767a41dd9ac5ef5f625
local     2fbef31fa84a7b4585955ce0cfada4dad52fe0571554018e60dc0c14a6338644
local     03bd5d3166d09007bc5872a35a9ab60fec48ce12f51ef99734fa94948609f133
local     4ee3ed7718390fd05b40031a56ab28a057ea14e6760a1539ab1824afa49ee717
local     5da5351434850dc61e5a7115c5daf9901aab168fd5a22ce48f4f4165c6ef4ff8
local     12cefa49cafbe4cf5bc9e2d7016a9404baede393d2a30d90df7eebc3b6b7cf0c
local     961de69ed86dfa90a78e284f7f5bdb99074663e176194c65d11a58b91cd102b8
local     4539e8923f1809f7c485a7ff928ebc4773c0ee44f68bac3d579ecfbf17fff742
local     c13067e1bfdf15579f721a48a2fa9e67523cdadd73f63506e9f5531e688286c6
local     c70491e91164f0a55e97c711a65b1a8b66475ea82ffa78a8f1708781698b0592
local     e3ded6ff550a50b7d88ae7b77f27951cc5886fde9526a301b504e8f06253fd8e
local     e802c7208591340c1307c1541777e9ab68f3aef7c40874a704c947baa2c29889
local     eeef1c3287de3b27d54a1d04262d69c71ef118c9f7e913702dc2342f0bdc81ac
local     f2e5aa8439720fae6844184d086b7dd8b4a652934588cc97235c5ad61f96300f
local     f16023a93b165473844c4d394cb4dd9a580649cf645b05ac637c86f000bc7664
local     f129110c18c8aa5daa447b2832464be1d21ebd7a72d1994ea2afc85c6f28b843
local     pack-cache-xodlf100_multitenant-kyma-backend_v1-f385ec948f8c.build
local     pack-cache-xodlf100_multitenant-kyma-backend_v1-f385ec948f8c.launch
local     test_volume
```
> 최하단에 생성됨
<br>

- 생성된 volume 정보 확인

```
$ docker volume inspect test_volume
[
    {
        "CreatedAt": "2024-06-19T02:03:29Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/test_volume/_data",
        "Name": "test_volume",
        "Options": null,
        "Scope": "local"
    }
]
```
> 기본 경로는 /var/lib/docker/volumes 에 저장됨
<br>

- 이제 생성한 볼륨을 컨테이너 내부 데이터와 링크를 걸어 맵핑시켜준다.

- 기존에 빌드된 'kimchang-frontend' 이미지를 가지고 test_cont라는 새로운 컨테이너 생성
  - `docker run -v [볼륨 이름]:[컨테이너 내부 디렉토리 경로] --name [컨테이너 이름] [이미지 이름]`
<br>

- 이미지 확인
```
$ docker image ls
REPOSITORY                                                TAG                                                                          IMAGE ID       CREATED         SIZE
kimchang-backend                                          latest                                                                       7ba68409b7c8   19 hours ago    439MB
new-backend                                               latest                                                                       3133ab283de5   4 days ago      329MB
new-frontend                                              latest                                                                       1f26ad921383   4 days ago      626MB
registry.bjsystems.kr/jilli-wbs/backend                   latest                                                                       8e0774a63f22   4 days ago      332MB
registry.bjsystems.kr/jilli-wbs/frontend                  latest                                                                       e4e54fa961a7   4 days ago      632MB
wbss-frontend                                             latest                                                                       c58a72314c81   4 days ago      627MB
wbss-backend                                              latest                                                                       445cc4302c51   9 days ago      326MB
paketobuildpacks/run-jammy-base                           latest                                                                       13e27d4c2b53   2 months ago    122MB
springboot                                                latest                                                                       2c80cd09227e   3 months ago    1.62GB
react                                                     latest                                                                       f5d7893b0a51   3 months ago    2.03GB
pms                                                       latest                                                                       1a317e576808   3 months ago    347MB
kimchang-frontend                                         latest                                                                       1ba60348baef   4 months ago    1.23GB
contarct-backend                                          latest                                                                       2e5c5d3f436f   9 months ago    1.15GB
contarct-backend-test                                     latest                                                                       0667d5f00291   10 months ago   1.15GB
<none>                                                    <none>                                                                       a29bc7cc3d43   10 months ago   1.15GB
contarct-frontend                                         latest                                                                       ded509052b04   10 months ago   1.7GB
hubproxy.docker.internal:5555/docker/desktop-kubernetes   kubernetes-v1.27.2-cni-v1.2.0-critools-v1.27.0-cri-dockerd-v0.3.2-1-debian   c763812a4530   12 months ago   418MB
registry.k8s.io/kube-apiserver                            v1.27.2                                                                      c5b13e4f7806   13 months ago   121MB
registry.k8s.io/kube-scheduler                            v1.27.2                                                                      89e70da428d2   13 months ago   58.4MB
registry.k8s.io/kube-controller-manager                   v1.27.2                                                                      ac2b7465ebba   13 months ago   112MB
registry.k8s.io/kube-proxy                                v1.27.2                                                                      b8aa50768fd6   13 months ago   71.1MB
docker/desktop-vpnkit-controller                          dc331cb22850be0cdd97c84a9cfecaf44a1afb6e                                     556098075b3d   13 months ago   36.2MB
registry.k8s.io/coredns/coredns                           v1.10.1                                                                      ead0a4a53df8   16 months ago   53.6MB
registry.k8s.io/etcd                                      3.5.7-0                                                                      86b6af7dd652   16 months ago   296MB
registry.k8s.io/pause                                     3.9                                                                          e6f181688397   20 months ago   744kB
mysql                                                     8.0.30                                                                       dbaea59d1b41   20 months ago   449MB
docker/desktop-storage-provisioner                        v2.0                                                                         99f89471f470   3 years ago     41.9MB
paketobuildpacks/builder-jammy-base                       latest                                                                       6b3fa06b0860   44 years ago    1.52GB
xodlf100/multitenant-kyma-backend                         v1                                                                           f80c657f873a   44 years ago    314MB
```
<br>

- 'kimchang-frontend' 이미지를 가지고 test_cont 컨테이너 생성

```
$ docker run --privileged -d -v test_volume:/vol --name test_cont kimchang-frontend
54e4d332cc46e65e23ed7e73b5a3a8e72116fbbe1aa36a662aec669b3b661e87
```
> --privileged : 컨테이너에 추가적인 권한을 부여 -> 컨테이너가 호스트 시스템 모든 장치에 접근
-d : 컨테이너를 백그라운드로 실행
-v volume_test:/vol : 호스트의 'test_volume'이라는 이름의 Docker 볼륨을 컨테이너의 /vol 디렉토리에 마운트
<br>

- 실행중인 컨테이너 확인

```
$ docker container ls
CONTAINER ID   IMAGE               COMMAND                   CREATED              STATUS              PORTS                                                      NAMES
54e4d332cc46   kimchang-frontend   "docker-entrypoint.s…"   About a minute ago   Up About a minute   9001/tcp                                                   test_cont
35247bdb12e8   wbss-backend        "docker-entrypoint.s…"   17 hours ago         Up 17 hours         0.0.0.0:8085->8085/tcp, 0.0.0.0:9228-9229->9228-9229/tcp   jilli-wbs-backend
5eb3e82a2486   wbss-frontend       "docker-entrypoint.s…"   17 hours ago         Up 17 hours         0.0.0.0:8086->8086/tcp                                     jilli-wbs-frontend
```
> test_cont 컨테이너가 'kimchang-frontend' 이미지로 만들어진것을 확인
<br>

- 컨테이너접속하여 volume 데이터 조작
  - 'vol' 디렉토리 하위에 testFile 생성
  - 도커 데스크탑에서 진행
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/16d78ad2-a9d8-4326-b768-7dbf42540a56)
> vol 디렉토리에 접근하여 파일 생성
<br>

- 외부 volume에서 똑같은 파일이 맵핑됐는지 확인

- 기존에 맵핑했던 외부 volume 저장 경로 확인

```
$ docker volume inspect test_volume
[
    {
        "CreatedAt": "2024-06-19T02:03:29Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/test_volume/_data",
        "Name": "test_volume",
        "Options": null,
        "Scope": "local"
    }
]
```
<br>

- window 환경에서는 해당 Mountpoinrt로 접근이 되지 않아 도커 데스크탑에서 확인
  - Linux 환경에서 접근 가능
  - 현재는 window 및 도커 데스크탑을 활용중
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/f11e8e11-41f2-42a3-883c-fcffe54d2c9e)
> 생성했던 외부 test_volume에 testFile이 동일하게 생성됨을 확인
<br>

- 컨테이너 삭제 후 volume이 그대로 남아 있는지 확인

```
$ docker rm -f test_cont
test_cont
``` 
> f : force
<br>

```
$ docker container ls
CONTAINER ID   IMAGE           COMMAND                   CREATED        STATUS        PORTS                                                      NAMES
35247bdb12e8   wbss-backend    "docker-entrypoint.s…"   17 hours ago   Up 17 hours   0.0.0.0:8085->8085/tcp, 0.0.0.0:9228-9229->9228-9229/tcp   jilli-wbs-backend
5eb3e82a2486   wbss-frontend   "docker-entrypoint.s…"   17 hours ago   Up 17 hours   0.0.0.0:8086->8086/tcp                                     jilli-wbs-frontend
```
> 삭제 확인
<br>

- test_volume 확인

![image](https://github.com/bjsystems/rnd/assets/121341413/b291e84b-414d-4004-ab70-259300f8a476)
> 컨테이너가 삭제됐음에도 파일 그대로 유지
<br>

- 즉, volume을 통하여 컨테이너 존재 유무에 상관없이 데이터를 유지시킬 수 있다.
<br>
<hr>
<br>

## ✔ docker-compose.yml 확인
- volume이 왜 사용되는지 알아보았다.

- 그렇다면 wbs의 docker-compose.yml 파일을 열어서 다시 한번 확인해보자.

- 아래 코드는 frontend 블록만 가져옴

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

- volume 부분이 `./frontend/src:/home/wbs/src`로 되어 있다.

- 즉, 현재 디렉토리의 frontend/src를 volume으로 잡아 컨테이너의 home/wbs/src로 맵핑시켜준 것이다.

![image](https://github.com/bjsystems/rnd/assets/121341413/b8f1c12b-68f1-41ca-aedd-ca475ac6f9f9)
<br>
<hr>
<br>

**Reference**<br>

[formulous  : 볼륨(volume) 의 개념에 대해 알아보고 활용해봅시다.](https://formulous.tistory.com/17)
