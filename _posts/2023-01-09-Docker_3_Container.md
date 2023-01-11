---
title: Docker 3 - Container
author:
  name: owner
  link: https://github.com/psy0231
date: 2023-01-09 00:00:00 +0900
categories: [Grind, Docker]
tags: [docker, container]
---

## Intro

- image는 container를 생성하는데 필요한  
모든 정보를 갖고있었다(inspect).
- container는 그 정보를 토대로 실행한다.
- 아무튼 image를 local로 pull한 뒤 부터.

---

## Create

- image에서 container를 생성.  
실행은 하지 않는다.
    
    ```
    docker create [OPTIONS] IMAGE [COMMAND] [ARG...]
    ```
    
- 아무런 option없으면 알아서 만들어줌.  
정상적으로 만들어지면  
container id가 나오고 끝.
    
    ```docker
    docker create tomcat:latest  

    8bb7897eacbe81e7e7530dc2381083b8e8cfe253551b9fbd48b2a9c1fb1bfa4a
    ```

- 이 때 만들어진 container는
    
    ```docker
    docker ps -a
    ```
    
    로 확인을 할 수 있음.
    
    ```docker
    CONTAINER ID   IMAGE                       COMMAND                  CREATED         STATUS                       PORTS     NAMES
    8bb7897eacbe   tomcat:latest               "catalina.sh run"        3 seconds ago   Created                                friendly_lumiere
    ```
    
- 생성에 필요한 부가정보는   
option에 선택적으로 넘겨줄 수 있다.
- 없어도 생성되는 이유는  
이미 image에 기본 정보가 있어서일듯함.

---

## Start

- create로 생성된 container를 실행한다.
    
    ```docker
    docker start [OPTIONS] CONTAINER [CONTAINER...]
    ```
    
- 위 CONTAINER에는 container을 쓰는데  
container id나 name을 쓴다.
    
    ```docker
    docker start friendly_lumiere                                  

    friendly_lumiere
    ```
    
    또는 
    
    ```docker
    docker start 8bb7897eacbe
    8bb7897eacbe
    ```
    
    - 정상실행일 경우  
    container id 또는 name을  
    상황에 맞게 출력.
- container create 이후 조작은   
create할때 나오는  
container id 또는 name으로 하게됨.  
id는 생성할때 나온게 full
- 실행중인 container는
    
    ```docker
    docker ps
    ```
    
    로 확인한다.
    
    ```docker
    CONTAINER ID   IMAGE           COMMAND             CREATED          STATUS         PORTS      NAMES
    8bb7897eacbe   tomcat:latest   "catalina.sh run"   12 minutes ago   Up 3 minutes   8080/tcp   friendly_lumiere
    ```
    
- `docker ps`는 실행하고 있는 container  
`docker ps -a`는 전체 container

---

## Restart

- container 다시시작
    
    ```bash
    docker restart [OPTIONS] CONTAINER [CONTAINER...]
    ```
    
    ```bash
    docker restart friendly_lumiere
    friendly_lumiere
    ```
    
- option으로 -t 또는 --time를 지정해  
다시 시작 전 대기시간을 설정한다.

---

## Stop

- container 정지.
    
    ```docker
    docker stop [OPTIONS] CONTAINER [CONTAINER...]
    ```
    
    ```docker
    docker stop friendly_lumiere  
    friendly_lumiere
    ```
    
- option으로 -t 또는 --time를 지정해  
정지하기전 대기시간을 설정한다.

---

## Remove

- container 삭제
    
    ```docker
    docker rm friendly_lumiere
    friendly_lumiere
    ```
    
- 정지되어있는 container만 가능.
- 여기까지 하면  
container 생성, 시작, 정지, 삭제.

---

## Run

- create - start를 동시에.
    
    ```bash
    docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
    ```
    
- 예를들어 tomcat을 해보면
    
    ```bash
    docker run tomcat:latest
    ```
    
    이 후 알아서 실행함.
    
- 확인해보면
    
    ```bash
    docker ps
    
    CONTAINER ID   IMAGE           COMMAND             CREATED              STATUS              PORTS      NAMES
    1bbadf422714   tomcat:latest   "catalina.sh run"   About a minute ago   Up About a minute   8080/tcp   gallant_yonath
    ```
    
    start 결과와 같음.
    
- 또한
    
    ```bash
    docker ps -a
    
    CONTAINER ID   IMAGE           COMMAND             CREATED         STATUS                      PORTS      NAMES
    1bbadf422714   tomcat:latest   "catalina.sh run"   8 minutes ago   Up 8 minutes                8080/tcp   gallant_yonath
    8bb7897eacbe   tomcat:latest   "catalina.sh run"   19 hours ago    Exited (143) 18 hours ago              friendly_lumiere
    ```
    
    create로 만든 것과 다른 name으로  
    새로 생성됨.
    

---

## COMMAND

- docker을 처음에 쓰면서  
가벼운 vmware정도로 생각하고 썼는데  
실제로 centos를 실행해보면 
바로 종료된다.
- vmware등의 가상화 프로그램과 차이점인데  
docker은 os자체를 실행하는게 아니라  
그 환경을 빌려 application layer에서  
격리된 환경을 만들고 실행한다.   
이 실행하는것이 정확히는 '명령어'이다.
- container가 실행되면  
주어진 환경에서 명령어를 실행했고  
그 결과를 보여줬던것.  
실행한 명령어는 COMMAND에 있다.
- centos는 "/bin/bash" 였고  
이 전에 tomcat같은 경우  
"catalina.sh run"등등.  
이 중 tomcat은 실행 후 동작하고 있었는데  
서비스 형태로 동작하는것들이 이런듯하다.  
db, 웹서버등이 이경우.
- image에 기본적으로 실행할  
command가 적혀있는데 변경도 가능함.  
run을 보면 끝에 arg를 전달할 수 있었는데
  
  ```docker
  docker run centos:latest ps
  
  PID TTY          TIME CMD
    1 ?        00:00:00 ps
  ```
  
  이 경우 
  
  ```docker
  docker ps -a

  CONTAINER ID   IMAGE                       COMMAND                  CREATED          STATUS                      PORTS     NAMES
  be1ea0bdedf3   centos:latest               "ps"                     52 seconds ago   Exited (0) 50 seconds ago             wizardly_lamarr
  ```
  
  COMMAND가 바꿘걸 확인할 수 있다.

---

## Outro

- container의 생성, 시작, 정지, 삭제는  
명령어만 보면 저게 전부임.
- 여기에는 image관련 키워드의  
기능적인면만 보려고 option을 생략함.
- 다만 container를 상황에 맞게 사용하려면  
그에 맞는 option이 필요함.
- option을 잘 못 쓰면 그만큼  
원하는 동작을 안할가능성이 크다.  
- option관련은 길어서 따로.