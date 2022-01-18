## in linux
- 환경은 vm, ubun 20.04.3 lts에서.

## 기본 명령어 
- 기본적으로 su에서 해야하는듯하다.  
아래 명령어들은 기본user일때 안됨
- su로 user를 바꾸던지  
명령어마다 sudo를 치던지.
- 아무튼 기본적으로 쓰이는 명령어들 .
- 여기 쓸때 TOC에 보이도록 씀  
앞으로 추가되는것들도 그렇게.

## 설치
```
apt install docker.io
```

## 이미지 관련
### 이미지 검색
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

### 이미지 다운로드
```
docker pull tomcat
```
- 이때 받으려고하는 tomcat가 이미 있다면 그냥 끝남
- TAG option이 없으면 항상 lastest

### 이미지 확인 
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

### 이미지 제거
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

### 이미지 정보 확인
```
docker inspect [imagename] 
```

## 컨체이너 관련
### 컨테이너 생성
```
docker create [OPTIONS] IMAGE [COMMAND] [ARG...]
```

```
root@ubuntu:~# docker create -p 80:80 --name tc2 consol/tomcat-7.0
e9f531bb6b912a8b2d7d77d2cbc51387988c484fd80b6184a355138cfea68add
```
- 이미지가 class라면 컨테이너는 인스턴스 같음.
- [docker create](https://docs.docker.com/engine/reference/commandline/create/)
- --name 
  - alias 지정
- -p
  - 포트 포워딩 할때 쓰는것같은데 포트 써야할 떄 이거 안써주면 안되는듯?
  - inspect로 보면             
  
    ```
    "ExposedPorts": {
              "50000/tcp": {},
              "8080/tcp": {}

    }
    ```
    
    이런 항목이 있는데 이게 실제 쓰일 port로   
    [공개 할 포트]:[config에 있는 포트] 쓰면됨. 

  - 예를들어 위는 jenkins의 경우였고  
  -p 9999:8080으로 한 경우 
    ```
    root@ubuntu:~# netstat -tulpn
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
    tcp        0      0 127.0.0.1:37257         0.0.0.0:*               LISTEN      815/containerd      
    tcp        0      0 0.0.0.0:9999            0.0.0.0:*               LISTEN      5607/docker-proxy   
    tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      663/systemd-resolve 
    tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      707/cupsd           
    tcp6       0      0 :::9999                 :::*                    LISTEN      5613/docker-proxy   
    tcp6       0      0 ::1:631                 :::*                    LISTEN      707/cupsd           
    udp        0      0 127.0.0.53:53           0.0.0.0:*                           663/systemd-resolve 
    udp        0      0 0.0.0.0:59969           0.0.0.0:*                           705/avahi-daemon: r 
    udp        0      0 0.0.0.0:631             0.0.0.0:*                           785/cups-browsed    
    udp        0      0 0.0.0.0:5353            0.0.0.0:*                           705/avahi-daemon: r 
    udp6       0      0 :::58761                :::*                                705/avahi-daemon: r 
    udp6       0      0 :::5353                 :::*                                705/avahi-daemon: r 
    root@ubuntu:~# 

    ```
- --rm : 컨테이너 종료시 삭제
### 컨테이너 실행
```
docker start [name or id]
```

```
root@ubuntu:~# docker start tc2
tc2
```

### 컨테이너 중지
```
docker stop [name or id]
```
```
root@ubuntu:~# docker stop tc2
tc2
```

### 컨테이너 삭제
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

### 컨테이너 확인 
- 아래 두개 차이점은 전제 컨테이너를 볼지 
실제로 실행중인것만 볼지.

-  실행중
    ```
    docker ps
    ```

    ```
    root@ubuntu:~# docker ps
    CONTAINER ID   IMAGE               COMMAND                  CREATED         STATUS         PORTS                                                   NAMES
    e9f531bb6b91   consol/tomcat-7.0   "/bin/sh -c /opt/tom…"   7 minutes ago   Up 6 seconds   8080/tcp, 0.0.0.0:80->80/tcp, :::80->80/tcp, 8778/tcp   tc2
    root@ubuntu:~# 

    ```
-  전체
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

### 컨테이너 내부 셸 사용 
- [docker exec](https://docs.docker.com/engine/reference/commandline/exec/)
```
 docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```
- option으로 -ti정도 ?

```
docker exec -ti jk /bin/bash
```

### 컨테이너 로그
```
docker logs [containerName]
```

### host - container 파일 복사
```
docker cp [srd] [dest]
```
- src, dest는 path 또는 path + file인데 
안쓰면 그냥 그 환경의 root(/)인듯
- src, dest가 container면 containername:path로 씀

- host에서 container로 복사할때
  ```
  container name = jk
  docker cp test.me jk:/
  ```
- src - dest는 
  - host - container
  - container - host
  - container - container



## Docker Lifecycle
- ![image](https://github.com/psy0231/psy0231.github.io/blob/main/postAssets/img/docker/docker%20lifecycle.jpg?raw=true)
- 위 명령어들을 종합하면 이런 흐름임.

## 이미지 정보 
```
docker inspect mysql
```

```
root@ubuntu:~# docker inspect mysql
[
    {
        "Id": "sha256:3218b38490cec8d31976a40b92e09d61377359eab878db49f025e5d464367f3b",
        "RepoTags": [
            "mysql:latest"
        ],
        "RepoDigests": [
            "mysql@sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2021-12-21T02:56:09.173474776Z",
        "Container": "aa68c4a47b8cb77d182249805229f0903d2f843badfe4aa4d700b069007dce6b",
        "ContainerConfig": {
            "Hostname": "aa68c4a47b8c",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3306/tcp": {},
                "33060/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "MYSQL_MAJOR=8.0",
                "MYSQL_VERSION=8.0.27-1debian10"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"mysqld\"]"
            ],
            "Image": "sha256:7bab64cb803b262704c0ad1fd393a352e9016324d31f9715096329e267451aca",
            "Volumes": {
                "/var/lib/mysql": {}
            },
            "WorkingDir": "",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "20.10.7",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3306/tcp": {},
                "33060/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "MYSQL_MAJOR=8.0",
                "MYSQL_VERSION=8.0.27-1debian10"
            ],
            "Cmd": [
                "mysqld"
            ],
            "Image": "sha256:7bab64cb803b262704c0ad1fd393a352e9016324d31f9715096329e267451aca",
            "Volumes": {
                "/var/lib/mysql": {}
            },
            "WorkingDir": "",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 515592044,
        "VirtualSize": 515592044,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/05e9740df42855130cf4d4e98515136105d8d226caec33bc92cec0d32a3187d5/diff:/var/lib/docker/overlay2/16698185b28b373595310f025c5c50630011fa3714ece635325209bdb5747c05/diff:/var/lib/docker/overlay2/836b304a4cb7c9b530008b7abfca2d868bf51aabc0cd55a378f905a90ca5f894/diff:/var/lib/docker/overlay2/d6dfb94f44aaece4fbe24d4cc3c9a0984cb232773be8dea72be1a94950d58254/diff:/var/lib/docker/overlay2/ef7e813577cee55896bad54d56627660329b8abbaca6f8f8d9e413e485ce4b70/diff:/var/lib/docker/overlay2/1ea4bac66121f2a36c03c3f8c32f414f3804f1bd959a5e31ef1e7b5bdbfe2a07/diff:/var/lib/docker/overlay2/ad5feba90093a57fcaa410c7b618a92244d894efbef67519455a956c585cfa1c/diff:/var/lib/docker/overlay2/a72c4b3dedab432790e0afad4f88b955a1bb3e672f8389257d0a799412fc3ee5/diff:/var/lib/docker/overlay2/e5b8721a897d62b0a0e0f2740caf4faa928297c17ced63292f5667e138022a0f/diff:/var/lib/docker/overlay2/cb1a3d4694d4394bb5fed29a942e61c7e04bdc62aec6a07a6d16a48364ce05bc/diff:/var/lib/docker/overlay2/3951970daf65f7ef0eda3a65750a9692e6f01cb46bcb4c330ac635b0a6e1e4b1/diff",
                "MergedDir": "/var/lib/docker/overlay2/eee4d72a7c681ef18366da0759a126e6c48575b7e625dae8b38fe085a30ed508/merged",
                "UpperDir": "/var/lib/docker/overlay2/eee4d72a7c681ef18366da0759a126e6c48575b7e625dae8b38fe085a30ed508/diff",
                "WorkDir": "/var/lib/docker/overlay2/eee4d72a7c681ef18366da0759a126e6c48575b7e625dae8b38fe085a30ed508/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:ad6b69b549193f81b039a1d478bc896f6e460c77c1849a4374ab95f9a3d2cea2",
                "sha256:fba7b131c5c350d828ebea6ce6d52cdc751219c6287c4a7f13a51435b35eac06",
                "sha256:0798f2528e8383f031ebd3c6d351f7d9f7731b3fd12007e5f2fdcdc4e1efc31a",
                "sha256:a0c2a050fee24f87fde784c197a8b3eb66a3881b96ea261165ac1a01807ffb80",
                "sha256:d7a777f6c3a4ded4667f61398eb1f9b380db07bf48876f64d93bf30fb1393f96",
                "sha256:0d17fee8db40d61d9ca0d85bff8b32ef04bbd09d77e02cc67c454c8f84edb3d8",
                "sha256:aad27784b7621a3e58bd03e5d798e505fb80b081a5070d7c822e41606b90a5c0",
                "sha256:1d1f48e448f9b8abb9a2aad1e76d4746b69957882d1ddb9c11115302d45fcbbd",
                "sha256:c654c2afcbba8c359565df63f6ecee333c9cc6abaeaa39838b05b4465a82758b",
                "sha256:118fee5d988ac2057ab66d87bbebd1f18b865fb02a03ba0e23762af5b55b0bd5",
                "sha256:fc8a043a3c7556d9abb4fad3aefa3ab6a5e1c02abda5f924f036c696687d094e",
                "sha256:d67a9f3f65691979bc9e2b5ee0afcd4549c994f13e1a384ecf3e11f83d82d3f2"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
root@ubuntu:~# 

```

## docker 기본 설정
```
docker info
```

```
root@ubuntu:~# docker info
Client:
 Context:    default
 Debug Mode: false

Server:
 Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
 Images: 2
 Server Version: 20.10.7
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 
 runc version: 
 init version: 
 Security Options:
  apparmor
  seccomp
   Profile: default
 Kernel Version: 5.11.0-44-generic
 Operating System: Ubuntu 20.04.3 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 3.805GiB
 Name: ubuntu
 ID: 5NEN:WHUM:QCSR:SHSG:IXFQ:YGYD:5AK2:DXBJ:KAXZ:PALI:ZUEH:UXVB
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Username: psy0231
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false

```

