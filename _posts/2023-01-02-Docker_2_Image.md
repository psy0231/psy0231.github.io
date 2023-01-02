---
title: Docker 2 - Image
author:
  name: owner
  link: https://github.com/psy0231
date: 2023-01-02 00:00:00 +0900
categories: [Grind, Docker]
tags: [docker, image]
---
## Intro

- 처음 docker을 사용할 때  
image를 받는것부터 시작한다.  
dockerfile은 좀 나중에 다시하기로하고.
- Docker Hub - local까지 범위

---

## Image 검색

- 여러가지로 Docker Hub에서 찾는게 편한데  
조건이 별다른게 없으면 여기서 찾을 수 있음.
  
  ```docker
  docker search [OPTIONS] TERM
  ```
  
  이름만 넣고 찾는게 편함.
  
- 예를들어 centos를 찾아보면
  
  ```bash
  docker search centos
  ```
  
  ```bash
  NAME                       DESCRIPTION           STARS  OFFICIAL  AUTOMATED
  centos                     DEPRECATED; The ...   7427   [OK]      
  kasmweb/centos-7-desktop   CentOS 7 desktop ...  26               
  couchbase/centos7-systemd  centos7-systemd ...   6                [OK]
  dokken/centos-7            CentOS 7 image ...    5                
  ```
  이런식의 결과가 나옴.  
  결과는 더 있는데 생락한것.

- Image Name은 [publisher]/[name] 이고  
publisher가 없는 것들은 official image로  
OFFICIAL OK가 끝에 다시 써있다.
- AUTOMATED는 자동화 빌드 된것..?

---

## Image 다운로드

- image는 pull로 받는다.
  
  ```
  docker pull [OPTIONS] NAME[:TAG|@DIGEST]
  ```
  
  - image는 search할때 나온 이름 넣으면 됨.
  - option은 쓴적없어서 모르겠음.
- 예를들어
  
  ```docker
  docker pull centos
  ```
  
- Tag는 default option이 lastest인데  
만약 image tag에 lastest가 없으면 error.
  
  ```docker
  docker pull kasmweb/centos-7-desktop 
  Using default tag: latest
  Error response from daemon: manifest for kasmweb/centos-7-desktop:latest not found: manifest unknown: manifest unknown
  ```
  
  - 이런경우 `docker pull kasmweb/centos-7-desktop:1.12.0`같이    
  tag를 명시하면되는데 terminal에서 확인하는게 좀 복잡함.
  - terminal에서 하더라도  
  docker hub에서 읽어오는거라    
  그냥 해당 hub페이지 드가서 보는게 편함

---

## Image 확인

- 로컬에 있는 image 보여줌.
  
  ```
  docker images
  
  docker image ls
  ```
  
  둘은 같은 결과임
  
  ```
  REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
  nbmis_maria_basic   latest    cbf594fa4667   5 weeks ago     1.07GB
  nbims_java          latest    34e761e64a32   5 weeks ago     721MB
  deb                 latest    b11359bab05c   3 months ago    102MB
  debian              <none>    f8fc33af3a75   3 months ago    100MB
  ```
  
- 가끔 이렇게 다른 명령어가  
같은 결과를 보여주는게 있는데  
버전 올라가면서 새로 생기고 없어지고 함.

---

## Image 제거

- image 제거 방법
  
  ```
  docker rmi [OPTIONS] IMAGE [IMAGE...]
  ```
  
- 예를들어
  
  ```docker
  docker rmi kasmweb/centos-7-desktop:1.12.0
  ```
  
- 마찬가지로 Tag까지.

---

## Image 정보 확인

- Image자체의 정보를 확인하려면
  
  ```
  docker inspect [OPTIONS] NAME|ID [NAME|ID...]
  ```
  
- 해당 image또는 container의 정보가 나온다.
- image는 container실행에 필요한  
모든 정보를 갖고있다고 햤는데  
그 정보들을 볼 수 있다.
- 따라서 image에 따라 써있는내용은  
큰 맥락빼고는 다 다름.
- 좀 길기도하고 나중에 또 나올테니 pass

---

## Outro

- 쓰고보니 생각보다 별거 없긴한데  
그만큼 쓰기 편했음.