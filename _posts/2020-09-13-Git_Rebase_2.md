---
title: 6. rebase_2
date: 2020-09-13 13:00:00 +0900
categories: [Grind, Git]
tags: [git]
seo:
  date_modified: 2020-05-04 19:33:17 +0900
---

## rebase
- commit 관련 작업 할때 쓰더라
- 이전까지 그걸 썼고 이번엔 특정 시나리오 상에서 쓰일때?

## commit, branch, conflict 
```
          6-7(dev_1_1)
         /
1-2-3-4-5(master)
         \
          A-B(dev_2_2)
```
- 주 목적은 branch병합.
- 예를들어 위같은 상황일 떄 dev_1과 dev_2를 합치고싶다.
- 두가지 경우가 발생하는데 12345AB67 또는 1234567AB
- 보통 5까지는 remote에 잘 올라가있고 각자 다른 두 명이 dev_1과 dev_2를 각각 작업했을때,  
다시 push하게되면 둘 중 하나는(늦은사람) conflict가 생김  
또는 굳이 remote rk dkslejfkeh 간단하게 branch만들어 따로 했던 작업들을 합치고 싶을 때도 마찬가지 
- 해결은 merge를 이용하거나 rebase를 이용하는데 그 중 하나인 rebase를 이용한 방법

## rebase direction
- 아래와같은 방식
  ```
  git rebase A B

  git checkout B
  git rebase A
  ```
- 둘은 같은 뜻으로  

  A뒤에 B븝 배치 시키거나  
  B의 base를 A로 한다.

  정도로 생각하면 될듯.  



### ex.1
- 1-2-3-4-5-A-B 에서  
1-2-3-4-5-6-7-A-B 로.  
- 알단 확인
  ```
  $ git log --oneline 
  6cb35d5 (HEAD -> dev_2_2, dev_2) B
  82e9dfa A
  2a87176 (origin/contents_1, origin/contents, contents_1, contents) 5
  4a123de 4
  130976b 3
  24f8565 2
  3aa9de8 1
  a693728 contents init
  73fd2f2 (origin/master, origin/HEAD, master) Initial commit
  ```
- 시작
  ```
  git rebase dev_1_1

  1
  2
  3
  4
  5
  <<<<<<< HEAD
  6
  7
  =======
  A
  >>>>>>> 82e9dfa... A
  ```
- conflict수정  
git add .  
git rebase --continue
  ```
  1
  2
  3
  4
  5
  6
  7
  A
  ```
  - 일단 이렇게 수정해봄
  - 이떄 continue를 하면 A에 대한 commit를 수정했다고 나온다.  
  따라서 rebase작업은 base로 교체한 branch 의 commit까지 다 지나고 부터 시작인듯.
- 계속하면
  ```
  1
  2
  3
  4
  5
  <<<<<<< HEAD
  6
  7
  A
  =======
  A
  B
  >>>>>>> 6cb35d5... B
  ```
  - 다시 수정하고 add, continue
- 결과
  ```
  1
  2
  3
  4
  5
  6
  7
  A
  B

  $ git log --oneline 
  0ee0b0c (HEAD -> dev_2_2) B - rebase
  00834e3 A - rebase
  9dd12c6 (dev_1_1, dev_1) 7
  86d22fb 6
  2a87176 (origin/contents_1, origin/contents, contents_1, contents) 5
  4a123de 4
  130976b 3
  24f8565 2
  3aa9de8 1
  a693728 contents init
  73fd2f2 (origin/master, origin/HEAD, master) Initial commit
  ```
- 결과를 보면 원하는데로 commit가 정렬됐고, 그에맞게 내용도 잘 수정됨.

### ex.2
- 1-2-3-4-5-6-7 에서  
1-2-3-4-5-A-B-6-7 로.  

- 위랑 다 같으므로 빠르게..
  ```
  $ git log --oneline 
  9dd12c6 (HEAD -> dev_1_1, dev_1) 7
  86d22fb 6
  2a87176 (origin/contents_1, origin/contents, contents_1, contents) 5
  4a123de 4
  130976b 3
  24f8565 2
  3aa9de8 1
  a693728 contents init
  73fd2f2 (origin/master, origin/HEAD, master) Initial commit
  ```

  ```
  $ git rebase dev_2_2
  1
  2
  3
  4
  5
  <<<<<<< HEAD
  A
  B
  =======
  6
  >>>>>>> 86d22fb... 6

  ```

  ```
  1
  2
  3
  4
  5
  A
  B
  6

  git rebase --continue

  6 - FIX

  # Please enter the commit message for your changes. Lines starting 
  # with '#' will be ignored, and an empty message aborts the commit.
  #
  # interactive rebase in progress; onto 6cb35d5
  # Last command done (1 command done):
  #    pick 86d22fb 6
  # Next command to do (1 remaining command):
  #    pick 9dd12c6 7
  # You are currently rebasing branch 'dev_1_1' on '6cb35d5'.
  #
  # Changes to be committed:
  #       modified:   contents/contents.md
  #
  ~
  ```

  ```
  1
  2
  3
  4
  5
  <<<<<<< HEAD
  A
  B
  6
  =======
  6
  7
  >>>>>>> 9dd12c6... 7

  ```

  ```
  1
  2
  3
  4
  5
  A
  B
  6
  7

  git add .
  git rebase --continue
  ```

  ```
  7 - fix

  # Please enter the commit message for your changes. Lines starting
  # with '#' will be ignored, and an empty message aborts the commit.
  #
  # interactive rebase in progress; onto 6cb35d5
  # Last commands done (2 commands done):
  #    pick 86d22fb 6
  #    pick 9dd12c6 7
  # No commands remaining.
  # You are currently rebasing branch 'dev_1_1' on '6cb35d5'.
  #
  # Changes to be committed:
  #       modified:   contents/contents.md
  #

  $ git log --oneline 
  2b497ad (HEAD -> dev_1_1) 7 - fix
  38dcc57 6 - FIX
  6cb35d5 (dev_2_2, dev_2) B
  82e9dfa A
  2a87176 (origin/contents_1, origin/contents, contents_1, contents) 5
  4a123de 4
  130976b 3
  24f8565 2
  3aa9de8 1
  a693728 contents init
  73fd2f2 (origin/master, origin/HEAD, master) Initial commit
  ```

## res
- 그래서 .. 나누어져있던 branch를 다시 합칠때.
- git rebase A B 로 하면 A뒤에 B를 붙이는 식이고 
conflict는 B시작부분 부터 발생. A까지는 쭉 진행된다.
- branch 를 다시 정렬하면서 commit의 위치들을 옮기게 되는데  
이때 commit의 위치가 바뀌는것 때문에 base가 달라진다 해서 rebase라고 하지 않았나 싶음.