---
title: Docker 5 - Exec / Attach
author:
  name: owner
  link: https://github.com/psy0231
date: 2023-01-17 00:00:00 +0900
categories: [Grind, Docker]
tags: [docker, exec, attach]
---

## Intro

- 지금까지 -i -t를 써서  
직접 조작하는 일이 많았다.  
아무래도 테스트 중심이어서 그런가봄.
- 이때 container에 접속해야되는데  
`attach`와 `exec`가 있다.
- 둘이 결과는 비슷하지만 좀 다름.
- 여기에서는 python container로함.
  
  ```docker
  docker create -i -t --name py python:latest
  docker stsrt py
  ```
    

## Exec

- Container에 명령을 실행한다.
  
  ```docker
  docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
  ```
  
  ```docker
  docker exec py ps -ef
  
  UID        PID  PPID  C STIME TTY          TIME CMD    
  root         1     0  0 06:45 pts/0    00:00:00 python3
  root        19     0  0 06:45 ?        00:00:00 ps -ef
  ```
  
- container의 기본 process는 pid 1인데  
이게 실행되는동안에만 가능함.
- 일회성임.  
container를 다시 시작히도  
다시 실행하지 않음.
- command는 실행 파일이어야함.
  
  ```docker
  docker exec -i -t my_container sh -c "echo a && echo b"
  ```
  
  이건 되는데 
  
  ```docker
  docker exec -i -t my_container "echo a && echo b"
  ```
  
  이건 안됨.
    
- 명령을 실행한다고 했지만  
-i -t 가 붙으면서  
연결되는것같은 효과.

## Attach

- container에 연결한다.
  
  ```docker
  docker attach [OPTIONS] CONTAINER
  ```
  
  ```docker
  docker attach py
  >>>
  ```
  
- 실행 중인 container에  
로컬 stdin, stdout, stderr 스트림 연결
- container에서 일어나는 일들을  
host에서 볼 수 있게됨.
- 이미 실행하고 있는 프로세스에 연결.
- 연결을 특정하지 않는거보면  
pid 1 인가싶음.  
pid 1 은 container에서  
특별한 process라고 했는데  
attach된 상태에서 종료하면  
container도 같이 꺼지기도하고.
- 아무튼, attach된 상태에서는  
종료하면 container도 종료된다.  
이건 원치 않으면 option을 준다
  
  ```docker
  **docker attach --detach-keys <string> <container>**
  ```
  
  예를들어
  
  ```docker
  docker attach --detach-keys ctrl-@
  ```
  
  이런식으로.  
  해당 option에 맞춰 종료하면  
  container는 종료되지 않음.   
  이거 쓰면서 찾았는데  
  이 옵션은 exec에도 가능함.
  

## Outro

- 막상 써보면 비슷해서  
신경안썼는데  찾아보니 이정도.
  - exec는 명령어 실행
  - attach는 container에 접속