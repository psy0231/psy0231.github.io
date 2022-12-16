---
title: Docker 1 - Intro
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-12-16 00:00:00 +0900
categories: [Grind, Docker]
tags: [docker, image, container]
---

## Environment

- wsl2, ubuntu 22.04
- 가끔 wsl2에서만 나오는 문제가 있는것같길래..

---

## Docker

- Linux의 container기술을 사용해 만든  
container 기술
- 이 설명이 좀 이상할 수 있는데  
vitualization에 좀 더 설명이 있음.
- 아무튼 원래 linux에는  
os vitualization을 이용한 기술이 있었고  
그걸 바탕으로 만들어졌다.
- 당연히 장/단점은 os vitualization를 따름.

### 설치

```bash
apt install docker.io
```

---

## Why Docker?

### 장점

- 일단은 가벼움.
  
  방식 특성상 hw vitualization보다 훨씬.
- 편의성이 있는데  

  image만 받아서 바로 실행 가능한 점으로  
  image는 실행 할 때 필요한  
  모든걸 갖고있는상태로   
  타겟프로그램을 실행하기 위해 필요한  
  여러 부가적인 설치를 생각할 필요가 없다.

- 격리된 공간의 이점으로  

  환경 세팅에 부담이 없고  
  테스트환경 세팅이 쉬워진다.  

  어떤 환경을 세팅할 때  
  익숙한 방식이라니라면 좀 헤메는 경우가 많음  
  게다가 어떤 프로그램은 삭제를 해도 가끔  
  이 전 정보가 남는 경우가 있음  
  oracle db 같은거.  

  container는 os, container에 독립적이었다.  
  뭔짓을해도 외부에 영향을 주지 않는다.  
  삭제 생성이 쉬워서  
  부담없이 호로록 할 수도 있고.
- vmware에서 snapshot같은기능을  
image를 이용해 사용할 수 있음.
- 배포가 쉬워지는데  
image를 그 자체로 넘기면 바로 실행 가능해지고  
또는  dockerfile을 이용하면 훨씬 쉬워진다.
- 그 외 확장이 쉽고 어찌고 하는게 있는데  
암튼 좋다.

### 단점

- os에 종속적임.  
os와 kernel을 공유하면서 
당연한거겠지만..  

  원래 linux의 container기술 기반이었음.  
  다만, 윈도우에서 linux를 사용하기가   
  비교적 편해지면서 이 불편함은 옅어졌는데  
  그래도 linux용 sw만 있는건 또 맞는것같음.
- 태생적으로 cli가 더 적합해서  
gui앱은 좀 힘든것같다.  
근데 서버는 서버고  
외부에서는 web이나 다른식으로 하는게 보통이지 않음?

---

## 용어

- 주요 용어로는 image, container가 대표적

### Image

- container 실행에 필요한  
resource, 설정값의 모음.
- container가 실행 될 모든 요소를  
온전히 갖고있는상태이다.

  예를들어 linux, db등  
  image만 받으면 실행이 됨.
- readonly속성으로  
image자체는 수정할 수 없다.
- container에서 변경된 내용을  
commit해 그 상태를   
다시 image로 저장 할 수 있다.
- 그래서 container의 어떤 시점을  
저장해 놓은거라고 봐도됨
- 이 때 새로 저장하는 부분을 layer이라함.

### Container

- image가 실행된 상태.
- application처럼 동작하는  
실제 상태가 이거.
- container 생성 및 실행 이후  
container에서의 변경사항은  
image에 영향을 주지 않는다.
- 예를들어 db container의 경우  
db에 어떤 데이터가 들어가도  
image는 그 상태를 저장하지 않고  
container에는 있음.
- container의 생성, 삭제 또한  
image나 같은 image로 부터 나온  
다른 container또한 영향을 주지 않음.
- 모든 container는 host os와 kernel을 공유한다.  
다만 위에 설명에 있듯  
각 container끼리는 영향을 주지 않음.
- kernel공유, container의  격리 와 같은 특성은  
os vitualization에서 기인함.

### Image - Container

- Image를 설치한다는 느낌이 아니고   
`Class instance = new Class();`이느낌에 가까움  
`image container = new image();`이렇게.
- class가 image, instance가 container인데   
instance(container)의 변화가  
class(image)에 영향이 없는건 비슷함.
- 단, 위 비유와 달리 container의 변경사항을  
image에 다시 쓸 수도 있는데  
이부분은 git랑 비슷함.  
이 전에 썼듯 layer라 하고,  
이 전 상태에 변경된 부분을 쌓는다는 느낌.
- 이 둘 스까 생각하면 될듯.

---

## Life Cycle

- 전반적인 주기
  ![https://github.com/psy0231/psy0231.github.io/blob/main/postAssets/img/docker/docker%20lifecycle.jpg?raw=true](https://github.com/psy0231/psy0231.github.io/blob/main/postAssets/img/docker/docker%20lifecycle.jpg?raw=true)
  - docker hub에서 pull 한게 image
  - image에서 create 또는 run을 한게 container

---

## Outro
- 써보니 확실히 편함.
- 잘쓰면 당연히 훨씬 더 편할것같음.
- 다만, 생각보다 이것저것 신경써줄게 많음.
- 또 지금 당장 필요함.