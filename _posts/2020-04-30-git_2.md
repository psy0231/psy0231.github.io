---
title: 2.설치, 기본설정
date: 2020-04-30 23:00:00 +0900
categories: [Grind, Git]
tags: [git]
seo:
  date_modified: 2020-05-01 10:48:05 +0900
---

## 설치
- 난 윈도우만 쓰니까 일단.
- 근데 설치 과정을 쓰는게 의미가 있나 싶긴한데 ..

1. ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/1.JPG?raw=true "1")

1. ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/2.JPG?raw=true "2")  
    - 설치할 폴더. 나중에 한번 더 쓸데 있음.

1. ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/3.JPG?raw=true "3")  

    - 그 외 옵션인데
      - Windows Exporer integration : 우클릭때 git보이게
        - ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/ex_2.jpg?raw=true)

      - Associate .git configuration files with the default text editor : git 구성파일 연결
      - Associate .sh files to be run with Bash : .sh 파일 연결
    - 요정도면 되는듯

1. ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/4.JPG?raw=true "4")

1. ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/5.jpg?raw=true "5")  

    - 기본 편집기 선택.

1. ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/6.JPG?raw=true "6")
    - 환경변수 추가. : git 명령어 실행가능한곳 추가.
      - Use Git from Git Bash only : Git bash에서만 
      - Use Git from the Windows Command Prompt : 윈도우 cmd, powershell 가능
      - Use Git and optional Unix tools from the Windows Command Prompt : 윈도우 cmd에서 Git과 유닉스도구를 사용할 경우?

1. ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/7.JPG?raw=true "7")

1. ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/8.JPG?raw=true "8")  

    - 이거 그 줄바꿈 관련인것같은데 정확히는 모르겠고 그냥 했던것같음.  
  
1. ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/9.JPG?raw=true "9")

1. ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/10.JPG?raw=true "10")

1. ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/11.JPG?raw=true "11")

1. ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/12.JPG?raw=true "12")

- 기억상 3번에서 옵션선택만 하고 나머지 쭉 next

## 초기 설정
- commit(저장) 할때는 어떤사람이 했는지 정보가 남는다.
  ```
  commit 0e22dbfed79c606256bf57271857380a070460ad
  Author: psy_comp <psy0231@gmail.com>
  Date:   Wed Apr 15 17:38:06 2020 +0900

      asdf

  commit a80018657e835f5bd5bf3f0050d55b82f4dba7f1
  Author: psy_aw <psy0231@gmail.com>
  Date:   Mon Apr 13 21:39:52 2020 +0900

      asdf
  ```
  - 그럼 그 정보가 어딘가는 있어야겠지.
  - 위를 보면 Author에 이름과 email이 있다 이걸 설정해줘야함.
```
git config --global user.name="name"
git config --global user.email="name"
확인은 git config --list
```
![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/init%20setting/2.JPG?raw=true)

## 통신방식
- https 또는 ssh이 둘이 있어서 이 둘만 써보고 넘어감.
- https
  - 위 초기 절정까지만 하고 push 하면 id/pw물어본다.
  ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/init%20setting/3.JPG?raw=true)
  ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/init%20setting/4.JPG?raw=true)
  - 계정정보 입력하면 push되고 해당 사이트에도 등록이 됨. 세상에...

- ssh
  - 이건 설정을 안하면 clone도 안된다. 
  - 일단 ~/.ssh로 이동, 공개키가 있나 본다. 아래는 없는상태..
  ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/init%20setting/5.JPG?raw=true)
  - 없을 경우 만들어주는데 공개키는 ssh-keygen.exe 를 실행해야함. 이건 설치폴더/usr/bin에 있음
  - 생성위치, pw를 물어보고 생성되는데 그냥 넘기면 생성위치는 아까 처음 확인한 위치, pw는 없는상태로 생성.
  ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/init%20setting/6.JPG?raw=true)
  - 다시 확인해본다.
  ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/init%20setting/7.JPG?raw=true)
  - 생성된 공개키는 
    ```
    $ cat ~/.ssh/id_rsa.pub
    ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAklOUpkDHrfHY17SbrmTIpNLTGK9Tjom/BWDSU
    GPl+nafzlHDTYW7hdI4yZ5ew18JH4JW9jbhUFrviQzM7xlELEVf4h9lFX5QVkbPppSwg0cda3
    Pbv7kOdJ/MTyBlWXFCR+HAo3FXRitBqxiX1nKhXpHAZsMciLq8V6RjsNAQwdsdMFvSlVK/7XA
    t3FaoJoAsncM1Q9x5+3V0Ww68/eIFmb1zuUFljQJKprrX88XypNDvjYNby6vw/Pb0rwert/En
    mZ+AW4OZPnTPI89ZPmVMLuayrD2cE86Z/il8b+gw3r3+1nKatmIkjn2so1d01QraTlMqVSsbx
    NrRFi9wrf+M7Q== schacon@mylaptop.local
    ```
    이런식인데 이걸 github 또는 gitlab계정 setting하는곳에 ssh keys 에 등록하면 된다.

- 두 가지 방식 등록 방법을 써놨는데 뭐가 더 좋은건가 찾던 중 https를 더 추천한다고 함.<sup id="a1">[1](#f1)</sup>

## 참고
- [Git 최초 설정](https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%B5%9C%EC%B4%88-%EC%84%A4%EC%A0%95)
- [SSH 공개키 만들기](https://git-scm.com/book/ko/v2/Git-%EC%84%9C%EB%B2%84-SSH-%EA%B3%B5%EA%B0%9C%ED%82%A4-%EB%A7%8C%EB%93%A4%EA%B8%B0)
- [Why does GitHub recommend HTTPS over SSH?](https://stackoverflow.com/questions/11041729/why-does-github-recommend-https-over-ssh) <b id="f1"></b> [↩](#a1)
