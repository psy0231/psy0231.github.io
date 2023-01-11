---
title: Docker 4 - Container Option
author:
  name: owner
  link: https://github.com/psy0231
date: 2023-01-11 00:00:00 +0900
categories: [Grind, Docker]
tags: [docker, container, option]
---

## Container Option

- container의 실행부터 종료까지  
명령어만 본다면 간단히 끝났다.
- create - start 와 run을 보면  
선택적으로 option을 넣을 수 있었다.
- 이 때 들어가는 option이 좀 많은데  
이미 image에 명시된 조건을  
조정할 때 쓰이는것같다.
- create - start와 run은  
결국엔 같은 기능이므로  
대부분의 option을 공유한다.
- 자주 쓰는거 위주로 필요할때마다 추가.

---

## \--name

- container name 지정
  
  ```bash
  docker create --name tc2 tomcat:latest
  ```
  
  ```bash
  CONTAINER ID  IMAGE   COMMAND            CREATED     STATUS                      PORTS  NAMES
  b2f22b9f2fc2  tomcat  "catalina.sh run"  3 days ago  Exited (143) 40 hours ago          tc2
  ```
  
- option 없이 생성하면
  
  ```bash
  CONTAINER ID   IMAGE  COMMAND      CREATED          STATUS   PORTS  NAMES
  5ad53c428d37   centos "/bin/bash"  57 seconds ago   Created         peaceful_kare
  ```
  
  이름이 아무렇게나 만들어지는데  
  이걸 지정할 수 있음.
  
- container에 대한 조작은  
id나 name으로 하기때문에  
보통 이게 기본적으로 쓰임.

---

## -p , \--publish

- network 사용이 필요할 때  
외부로 공개할 포트를 지정
- container가 독립공간이므로  
쓰는 port가 있더라도  
net으로 외부에서 접속할 수 없고  
이 port를 host에 연결해야한다. 
- 포트 포워딩 할때 쓰는것같은데  
포트 써야할 떄 이거 안써주면 안되는듯?
- 인자값은 `<공개 port>:<image port>`.
- container에서 쓰일 port정보는  
image에 써있다. 예를들어 tomcat은
  
  ```docker
  "ExposedPorts": {
                  "8080/tcp": {}
                  },
  ```
  
- publish없이 실행은 할 수 있다.
  
  ```docker
  docker --name tc tomcat
  ```
  
  - 이 경우 NetworkSettings에  
  별다른 정보가 없다.
  - 이 상태로 실행해도  
  할수있는게 없다.
  - network를 통해 접속할 수 없다.  
  가 원래 결론이었고 별도로  
  아마 container에서 갖고있는  
  bash에는 될것같음.

- 반대로
  
  ```docker
  docker --name tc2 -p 9999:8080 tomcat
  ```
  이 경우 실행해보면  
  ```bash
  docker ps
  ```
  ```bash
  CONTAINER ID   IMAGE           COMMAND             CREATED          STATUS          PORTS                    NAMES
  cd214ecd1a84   tomcat:latest   "catalina.sh run"   19 minutes ago   Up 19 minutes   0.0.0.0:9999->8080/tcp   tc2
  ```
  - option없이 했을때랑 다르게  
  PORTS에 조건이 더 생겼다.
  - 또한NetworkSettings에  
  관련 정보들이 들어가 있는데  
  둘 간의 차이를 보면
    
    ```docker
    <tc>
    
    "NetworkSettings": {
      ...
      "Ports": {},
      "SandboxKey": "/var/run/docker/netns/e04b1ad8ff29",
      "SecondaryIPAddresses": null,
      "SecondaryIPv6Addresses": null,
      "EndpointID": "",
      "Gateway": "",
      "GlobalIPv6Address": "",
      "GlobalIPv6PrefixLen": 0,
      "IPAddress": "",
      "IPPrefixLen": 0,
      "IPv6Gateway": "",
      "MacAddress": "",
      "Networks": {
        "bridge": {
          "IPAMConfig": null,
          "Links": null,
          "Aliases": null,
          "NetworkID": "9b025930652cabdf1c38676ce480a6df5b82598f2996429ebf7067db9e0a976f",
          "EndpointID": "",
          "Gateway": "",
          "IPAddress": "",
          "IPPrefixLen": 0,
          "IPv6Gateway": "",
          "GlobalIPv6Address": "",
          "GlobalIPv6PrefixLen": 0,
          "MacAddress": "",
          "DriverOpts": null
        }
      }
    ```
    
    ```docker
    <tc2>
    
    "NetworkSettings": {
      ...
      "Ports": {
          "8080/tcp": [
              {
                  "HostIp": "0.0.0.0",
                  "HostPort": "9999"
              }
          ]
      },
      "SandboxKey": "/var/run/docker/netns/c6013703193c",
      "SecondaryIPAddresses": null,
      "SecondaryIPv6Addresses": null,
      "EndpointID": "0167447f944aa96aa3b4843afef94fe5b2c8ca4f1a7757fad4a222dffa17a261",
      "Gateway": "172.17.0.1",
      "GlobalIPv6Address": "",
      "GlobalIPv6PrefixLen": 0,
      "IPAddress": "172.17.0.2",
      "IPPrefixLen": 16,
      "IPv6Gateway": "",
      "MacAddress": "02:42:ac:11:00:02",
      "Networks": {
        "bridge": {
          "IPAMConfig": null,
          "Links": null,
          "Aliases": null,
          "NetworkID": "9b025930652cabdf1c38676ce480a6df5b82598f2996429ebf7067db9e0a976f",
          "EndpointID": "0167447f944aa96aa3b4843afef94fe5b2c8ca4f1a7757fad4a222dffa17a261",
          "Gateway": "172.17.0.1",
          "IPAddress": "172.17.0.2",
          "IPPrefixLen": 16,
          "IPv6Gateway": "",
          "GlobalIPv6Address": "",
          "GlobalIPv6PrefixLen": 0,
          "MacAddress": "02:42:ac:11:00:02",
          "DriverOpts": null
        }
      }
    ```

- host를 확인해 보면
  - windows
    
    ```bash
    
    netstat -an | findstr :9999
      TCP    0.0.0.0:9999   0.0.0.0:0  LISTENING
      TCP    [::]:9999      [::]:0     LISTENING
      TCP    [::1]:9999     [::]:0     LISTENING
    ```
      
  - wsl
      
    ```bash
    
    netstat -an | grep :9999
    tcp6       0      0 :::9999                 :::*                    LISTEN
    ```
      
  - 둘 다 option으로 줬던   
  port를 쓰기 시작하고 이 때부터  
  network 관련 기능을 쓸 수 있어진다.

---

## -t, \--tty

- tty 모드를 사용한다.
- bash를 사용하려면 써야한다.
- 먼저 centos를 option없이 해보면
  
  ```docker
  docker run --name test centos:latest
  ```
  
  ```docker
  docker ps
  
  CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
  ```
  
  ```docker
  docker ps -a   
  
  CONTAINER ID   IMAGE                       COMMAND                  CREATED             STATUS                           PORTS     NAMES
  d7de0fda2455   centos:latest               "/bin/bash"              7 seconds ago       Exited (0) 7 seconds ago                   test
  ```
  실행했던 흔적은 남아있는데  
  아무 반응없이 꺼진다.  
  Exited로 앞서 설명처럼  
  명령어 실행 후 끝난것.
  
- docker 문서에서도 tty는  
pseudo-TTY를 할당한다고 설명하는데  
tty는 terminal, bash, shell 등이 관련있고  
이걸 정리할 수 있다면 따로.
- 아무튼 대충 이해한바로는  
명령 하나만 보내는게 이 전까지 방법이면  
terminal을 사용하는 환경으로 바꿔주는듯함.  
- 다시말해 명령전달에서 terminal실행으로.
- 이 명령어를 써야 bash를 사용한다.
  
  ```docker
  docker run -t --name test centos:latest
  [root@59279bc00159 /]#
  ```
  
- 다만, 위 상태에서 뭘 더 할수있는건 없음  
그 이유와 필요한건 다음으로.

---

## -i, \--interective

- -t의 경우 bash를 실행하고  
결과만 보여주고 명령을 전달할수 없었는데  
container는 기본적으로  
stdout으로 동작한다.  
그래서 더이상 할수있는게 없던것.
- -i 는 stdin을 추가해준다.

  ```docker
  docker run --name test -i -t centos:latest
  
  [root@8e7aedf13e36 /]#
  [root@8e7aedf13e36 /]# ls
  bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
  ```
  ```docker
  docker create --name test -i -t centos
  docker start test
  ```
  - run의 경우 입력도 가능해짐.
  - create-start의 경우  
  exec, attach등으로  
  container로 접속을 하면 됨.  
  이건 나중에.

---
## -d \--detach
- run의 경우 -t -i 를 쓰면  
시작되면서 container의 bash로  
바로 연결되는데 이걸 방지함.
- daemon, background 그런거.
  ```docker
  docker run --name test -i -t -d centos:latest
  ```

---

## \--rm

- 위와같이 테스트 등에 쓸 container는  
유지할필요가 없다.
  
  ```bash
  docker run --rm -t -i centos:latest
  ```
  
- container가 종료되면 자동으로 삭제한다.

---

## -e, \--env

- 환경변수를 설정한다.
- 일단은 db쓸때 사용해봐서  
db를 예로 들기로 한다.
- 근데 다른 용도의 사용도 많은듯함.
- mysql을 기준으로
  
  ```bash
  docker create --name test -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=Sample_db \
  -e MYSQL_USER=user -e MYSQL_PASSWORD=passwd mysql:latest
  ```
    
- option에 -e는 여러개일 수 있음.
  - MYSQL_ROOT_PASSWORD : root계정 pw
  - MYSQL_DATABASE : db 생성.
  - MYSQL_USER : user 계정
  - MYSQL_PASSWORD : user 계정 pw
  - 이건 여기는 안썼는데  
  MYSQL_ALLOW_EMPTY_PASSWORD :  
  yes/no로 root pw 강제 설정 유무
- 환경설정 적용확인은 db접속으로.
  - datagrip  
  ip, port로 접속하는데  
  아무튼 됨 근데 사진찍기 귀찮음.
  - console
    
    ```bash
    docker exec -i -t test bash
    
    bash-4.4# mysql -u root -p
    Enter password: 
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 15
    Server version: 8.0.29 MySQL Community Server - GPL
    
    Copyright (c) 2000, 2022, Oracle and/or its affiliates.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | Sample_db          |
    | information_schema |
    | mysql              |
    | performance_schema |
    | sys                |
    +--------------------+
    5 rows in set (0.00 sec)
    
    mysql> exit
    Bye
    bash-4.4# mysql -u user -p
    Enter password: 
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 16
    Server version: 8.0.29 MySQL Community Server - GPL
    
    Copyright (c) 2000, 2022, Oracle and/or its affiliates.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | Sample_db          |
    | information_schema |
    +--------------------+
    2 rows in set (0.01 sec)
    
    mysql>
    ```
    - option으로 줬던 계정과  
    sample_db가 있다.
- inspection을 비교해보면  
  이와 관련해 달라지는 구간이 있는데  
  Config-Env가 그렇다.
    
    ```bash
    <mysql image>
    
    "Env": [
        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
        "GOSU_VERSION=1.14",
        "MYSQL_MAJOR=8.0",
        "MYSQL_VERSION=8.0.29-1.el8",
        "MYSQL_SHELL_VERSION=8.0.29-1.el8"
    ],
    ```
    
    ```bash
    <test container>
    
    "Env": [
        "MYSQL_ROOT_PASSWORD=root",
        "MYSQL_DATABASE=Sample_db",
        "MYSQL_USER=user",
        "MYSQL_PASSWORD=passwd",
        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
        "GOSU_VERSION=1.14",
        "MYSQL_MAJOR=8.0",
        "MYSQL_VERSION=8.0.29-1.el8",
        "MYSQL_SHELL_VERSION=8.0.29-1.el8"
    ],
    ```
        
- 일단은 db를 예시로 들었지만  
이 옵션은 활용도에 따라   
사용할수 있는 범위가 넓어지고  
image에 따라 쓸 수 있는 env가 다르다.

---

## -v, \--volume

- container의 data가 저장될  
위치를 지정한다.
- container는 상태가 저장되지 않는다.  
container가 삭제되면  
내부에서 쓰던 data도 삭제된다.
- container의 data는 필요하다면  
기본적으로 volume으로 관리되는데  
보통 container랑 생사를 같이함.
- 이 volume을 조정해 data를  
container 외부에 저장하면서  
container에 상관없이  
data를 영속적으로 보존할 수 있다.
- 부가적으로 container끼리  
data를 공유하는 효과도 있다.
- volume 관련 설정은 Mounts에 있다.
  
  ```docker
  docker create --name test -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=Sample_db \
  -e MYSQL_USER=user -e MYSQL_PASSWORD=passwd mysql:latest
  ```
  
  ```bash
  docker inspect test
  ```
  
  ```docker
  "Mounts": [
    {
      "Type": "volume",
      "Name": "7bf5fb839a6c7e953b3113b94bc8fc3977ffb94ce3c9d18df57331629798a252",
      "Source": "/var/lib/docker/volumes/7bf5fb839a6c7e953b3113b94bc8fc3977ffb94ce3c9d18df57331629798a252/_data",
      "Destination": "/var/lib/mysql",
      "Driver": "local",
      "Mode": "",
      "RW": true,
      "Propagation": ""
    }
  ],
  ```
  
  - 아무런 설정없이 만든경우  
  volume은 임의로 만들어져 할당된다.
  - volume은 필요한 경우 생성된다.  
  위는 db라서 생성됐지만  
  os나 언어 container의 경우  
  이부분이 비어있음.
  - 실제로 volume는 Source에 있다고 하는데  
  linux같은 경우가 그렇고   
  wsl은 `\\\wsl$\docker-desktop-data\`   
  `version-pack-data\community\`   
  `docker\volumes\<volume name>\\_data`  
  에 있다.
  - Destination은 container내부에  
  data가 쌓이는 위치
- 같은 기능의 option으로  
\--mount가 있는데 \--mount를 더 권장함.  
더 명시적이고 더 많이 쓸 수 있어서.
- docker document에서는   
크게 3가지 방법을 소개한다.

### Volume

- docker에서 생성, 관리하는 data.
- 이 방법을 더 권장하고 있다.
- os에 따라 dir등이 달라질 수 있는데  
이건 docker에서 관리하기때문에  
신경쓸필요가 적어짐.
- 등등 그 외에 많은 이점을 설명하고 있음.
- 아마 기본으로 설정된방법인것같은데   
위처럼 아무 조건없이 만들어졌을 떄  
type을 보면 volume이다.
- volume은
  
  ```docker
  docker volume ls
  ```
  
  ls로 확인할 수 있는데  
  보통은 이름이 무작위로 만들어져있고  
  volume에 대한 확인또한  
  
  ```docker
  docker volume inspect <volume name>
  ```
  
  inspect로 가능하다.
    
- container에 특정 volume을  
명시하기 위해서는  
먼저 volume을 생성해야한다.
  
  ```docker
  docker volume create [OPTIONS] [VOLUME]
  ```
  
  예를들어 
  
  ```docker
  docker volume create test_vol
  ```
    
- volume를 container에 연결한다.
  
  ```docker
  docker create -v test_vol:/var/lib/mysql \
  --name test -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=Sample_db \
  -e MYSQL_USER=user -e MYSQL_PASSWORD=passwd mysql:latest
  ```
  
  - -v 부분이고  
  `<volume name> : <target>`
  - target는 container내부의 경로로   
  dir이나 file상관없고  
  dir경우 없으면 생성까지 해줌
  - mysql은 Destination이 명시되어있었고  
  그 위치를 생성한 volume으로 mount한것.   
  Destination이 없더라도 혹은 필요에 따라  
  특정 dir을 쓰면 됨.
- container의 해당 부분을 확인해보면
  
  ```docker
  "Mounts": [
    {
      "Type": "volume",
      "Name": "test_vol",
      "Source": "/var/lib/docker/volumes/test_vol/_data",
      "Destination": "/var/lib/mysql",
      "Driver": "local",
      "Mode": "z",
      "RW": true,
      "Propagation": ""
    }
  ],
  ```
  
  실행하면 -e때랑 같다.
    
- 이 상태에서 container를 삭제하고  
-e MYSQL_DATABASE=Sample_db를  
지운상태로 다시 생성한다.
- 이 container에는  
Sample_db가 없어야 하지만  
이 전 data는 volume에 여전히 있으므로   
show databases; 하면 보임.
- 따라서 이 volume만 지워지지 않는다면  
container에 상관없이  
data를 보호할 수 있음.

### Bind mounts

- host의 임의의 경로에  
container를  mount한다.
- volume도 경로를 따라가면  
host에서 찾을 수 있었는데  
docker에서 관리하는 data형식이었다면  
이 경우는 host의 dir을 직접적으로 사용.
- option에서 volume 자리에  
host의 dir을 쓰면 된다.
  
  ```docker
  docker create --name test1 -i -t -v ~/test:/test centos:latest
  docker create --name test2 -i -t -v ~/test:/test centos:latest
  ```
  
  결과는 volume mount랑 같음.
    
- inspect경우 type이 달라진다.
  
  ```docker
  "Mounts": [
    {
      "Type": "bind",
      "Source": "/home/psy/test",
      "Destination": "/test",
      "Mode": "",
      "RW": true,
      "Propagation": "rprivate"
    }
  ],
  ```
    
- 일단 volume랑 bind랑 비교해보면  
docker에서 관리해주냐가 다른점.
- 직접 관리해주는 volume이 더 권장됨.

### tmpgs mounts

- 이 방법도 있는데  
container를 readonly로 만들고  
그 상태에서 뭔가 쓸 때 사용.
- 암튼 상황이 어거지긴했음.  
그래서 굳이 안하기로함.

### volume share

- volume은 container끼리  
data를 공유가 가능하게 한다.
- 같은 volume를 쓰면 됨.
  
  ```docker
  docker create --name test1 -i -t -v test_vol:/test centos:latest
  docker create --name test2 -i -t -v test_vol:/test centos:latest
  ```
    
- Volume, Bind mounts 둘 다 상관없음

---
