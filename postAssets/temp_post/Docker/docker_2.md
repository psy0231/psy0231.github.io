## in linux
- 환경은 vm, ubun 20.04.3 lts에서.

## 명령어 
- 기본적으로 su에서 해야하는듯하다.  
아래 명령어들은 기본user일때 안됨
- su로 user를 바꾸던지  
명령어마다 sudo를 치던지.
- 아무튼 기본적으로 쓰이는 명령어들 .

## 설치
```
apt install docker.io
```
  
## 이미지 검색
```
docker search [name]
```

```
root@ubuntu:~# docker search node
NAME                                   DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
node                                   Node.js is a JavaScript-based platform for s…   10964     [OK]       
mongo-express                          Web-based MongoDB admin interface, written w…   1113      [OK]       
nodered/node-red                       Low-code programming for event-driven applic…   403                  
nodered/node-red-docker                Deprecated - older Node-RED Docker images.      353                  [OK]
prom/node-exporter                                                                     274                  [OK]
selenium/node-chrome                                                                   229                  [OK]
kindest/node                           sigs.k8s.io/kind node image                     61                   
kkarczmarczyk/node-yarn                Node docker image with yarn package manager …   49                   [OK]
digitallyseamless/nodejs-bower-grunt    Node.js w/ Bower & Grunt Dockerfile for tru…   48                   [OK]
iron/node                              Tiny Node image                                 28                   
calico/node                            Calico's per-host DaemonSet container image.…   24                   [OK]
presearch/node                         Run a search node in Presearch's decentraliz…   16                   
appsvc/node                            Azure App Service Node.js dockerfiles           14                   [OK]
cusspvz/node                           🌐 Super small Node.js container (~15MB) bas…    8                    [OK]
opendronemap/nodeodm                   Automated build for NodeODM                     8                    [OK]
nodecg/nodecg                          Create broadcast graphics using Node.js and …   4                    [OK]
tarampampam/node                       Docker image, based on node, with git, bash,…   4                    [OK]
timbru31/node-chrome                   Node.js 10 LTS, Node.js 12 LTS or Node.js 14…   4                    [OK]
timbru31/node-alpine-git               Node.js 10 LTS (Dubnium), Node.js 12 LTS (Er…   3                    [OK]
ogazitt/node-env                       node app that shows environment variables       2                    
ppc64le/node                           Node.js is a JavaScript-based platform for s…   2                    
testim/node-chrome                     Selenium Chrome Node + Testim Extension         0                    [OK]
camptocamp/node-collectd               rancher node monitoring agent                   0                    [OK]
renovate/node                          Simple node container                           0                    [OK]
appsvctest/node                        node build                                      0                    [OK]
root@ubuntu:~# 

```
- Image Name은 [publisher]/[name] 이고  
publisher이 없는 것들은 official image.

## 이미지 다운로드
```
docker pull tomcat
```
- 이때 받으려고하는 tomcat가 이미 있다면 그냥 끝남
- TAG option이 없으면 항상 lastest

## 이미지 확인 
```
docker images
```

```
root@ubuntu:~# docker images
REPOSITORY          TAG       IMAGE ID       CREATED       SIZE
nginx               latest    605c77e624dd   11 days ago   141MB
node                latest    a283f62cb84b   2 weeks ago   993MB
mysql               latest    3218b38490ce   2 weeks ago   516MB
consol/tomcat-7.0   latest    7c34bafd1150   6 years ago   601MB
```

## 이미지 제거
```
docker rmi [imagename]
```

- nginx를 삭제하고 확인
  ```
  root@ubuntu:~# docker rmi nginx
  Untagged: nginx:latest
  Untagged: nginx@sha256:0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31
  Deleted: sha256:605c77e624ddb75e6110f997c58876baa13f8754486b461117934b24a9dc3a85
  Deleted: sha256:b625d8e29573fa369e799ca7c5df8b7a902126d2b7cbeb390af59e4b9e1210c5
  Deleted: sha256:7850d382fb05e393e211067c5ca0aada2111fcbe550a90fed04d1c634bd31a14
  Deleted: sha256:02b80ac2055edd757a996c3d554e6a8906fd3521e14d1227440afd5163a5f1c4
  Deleted: sha256:b92aa5824592ecb46e6d169f8e694a99150ccef01a2aabea7b9c02356cdabe7c
  Deleted: sha256:780238f18c540007376dd5e904f583896a69fe620876cabc06977a3af4ba4fb5
  Deleted: sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f
  root@ubuntu:~# docker images
  REPOSITORY          TAG       IMAGE ID       CREATED       SIZE
  node                latest    a283f62cb84b   2 weeks ago   993MB
  mysql               latest    3218b38490ce   2 weeks ago   516MB
  consol/tomcat-7.0   latest    7c34bafd1150   6 years ago   601MB
  root@ubuntu:~# 
  ```

## 컨테이너 생성
```
docker create [option] ...
```

```
root@ubuntu:~# docker create -p 80:80 --name tc2 consol/tomcat-7.0
e9f531bb6b912a8b2d7d77d2cbc51387988c484fd80b6184a355138cfea68add
```

- 이미지가 class라면 컨테이너는 인스턴스 같음.
## 컨테이너 실행
```
docker start [name or id]
```

```
root@ubuntu:~# docker start tc2
tc2
```

## 컨테이너 중지
```
docker stop [name or id]
```
```
root@ubuntu:~# docker stop tc2
tc2
```

## 컨테이너 삭제
```
docker rm tc2
```

- 만약 실행 중인 컨테이너라면 
```
root@ubuntu:~# docker rm tc2
Error response from daemon: You cannot remove a running container e9f531bb6b912a8b2d7d77d2cbc51387988c484fd80b6184a355138cfea68add. Stop the container before attempting removal or force remove
```

- 정상적으로 삭제되는경우 
```
root@ubuntu:~# docker ps -a
CONTAINER ID   IMAGE               COMMAND                  CREATED          STATUS                            PORTS     NAMES
e9f531bb6b91   consol/tomcat-7.0   "/bin/sh -c /opt/tom…"   14 minutes ago   Exited (137) About a minute ago             tc2
d3cbf6a7021d   consol/tomcat-7.0   "/bin/sh -c /opt/tom…"   3 days ago       Exited (137) 3 days ago                     tc

root@ubuntu:~# docker rm tc2
tc2

root@ubuntu:~# docker ps -a
CONTAINER ID   IMAGE               COMMAND                  CREATED      STATUS                    PORTS     NAMES
d3cbf6a7021d   consol/tomcat-7.0   "/bin/sh -c /opt/tom…"   3 days ago   Exited (137) 3 days ago             tc
root@ubuntu:~# 
```


## 컨테이너 확인 
- 아래 두개 차이점은 전제 컨테이너를 볼지 
실제로 실행중인것만 볼지.

### 실행중
```
docker ps
```

```
root@ubuntu:~# docker ps
CONTAINER ID   IMAGE               COMMAND                  CREATED         STATUS         PORTS                                                   NAMES
e9f531bb6b91   consol/tomcat-7.0   "/bin/sh -c /opt/tom…"   7 minutes ago   Up 6 seconds   8080/tcp, 0.0.0.0:80->80/tcp, :::80->80/tcp, 8778/tcp   tc2
root@ubuntu:~# 

```
### 전체
```
docker ps -a
```

```
root@ubuntu:~# docker ps -a
CONTAINER ID   IMAGE               COMMAND                  CREATED         STATUS                    PORTS                                                   NAMES
e9f531bb6b91   consol/tomcat-7.0   "/bin/sh -c /opt/tom…"   8 minutes ago   Up About a minute         8080/tcp, 0.0.0.0:80->80/tcp, :::80->80/tcp, 8778/tcp   tc2
d3cbf6a7021d   consol/tomcat-7.0   "/bin/sh -c /opt/tom…"   3 days ago      Exited (137) 3 days ago                                                           tc
root@ubuntu:~# 
```

## Docker Lifecycle
- 이미지
- 위 명령어들을 종합하면 이런 흐름임.