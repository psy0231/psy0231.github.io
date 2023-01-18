---
title: Docker 6 - Copy
author:
  name: owner
  link: https://github.com/psy0231
date: 2023-01-18 00:00:00 +0900
categories: [Grind, Docker]
tags: [docker, copy]
---
## Intro

- 생각보다 파일을 복사할일이좀 많더라.
- 문서보니까 잘 정리되어있길래.

## cp

- container - host간  
파일 / 폴더 복사
- 양방항 가능.
- 명령어는
  
  ```docker
  docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
  docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH
  ```
  
- 대충 이런식.
  
  ```docker
  docker cp py:/FromContainer.txt test
  docker cp test py:/
  ```
  
- 왼쪽에 src 오른쪽이 dest,  
container의 경우 이름명시. 이정도.
- container실행여부는 상관없음.
- container끼리는 안됨.
- 경로는 절대경로 또는  
상대경로일 수 있음.

## SRC - DEST 동작

### `SRC_PATH`가 file일때

- `DEST_PATH`가 없을때
  - `DEST_PATH`생성, 파일저장
- `DEST_PATH`가 없고 `/`로 끝날때
  - error : 대상 dir이 있어야함.
- `DEST_PATH`가 있고 file일때
  - src파일의 내용으로 덮어씀.
- `DEST_PATH`가 있고 dir일때
  - `SRC_PATH`의 이름으로  
  dir에 file이 복사됨.

### `SRC_PATH`가 dir일때

- `DEST_PATH`가 없을때
  - `DEST_PATH`가 dir로 생성
  - `SRC_PATH`의 contents가  
  여기로 복사됨.
- `DEST_PATH`가 있고 file일때
  - error : dir을 file로 복사할 수 없음.
- `DEST_PATH`가 있고 dir일때
  - `SRC_PATH`의 끝이 `/.`일때  
  src dir이 여기로 복사됨.
  - `SRC_PATH`의 끝이 `/.`아닐때  
  src dir의 contents가 여기로 복사됨.

## Outro

- 쓰면서 보니까  
빈번하게 파일 수정이 많으면  
굳이 cp쓸거없이  
`Bind mounts`하면 더 좋을것같음.  