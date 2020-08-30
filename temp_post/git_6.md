---
title: 6. rebase_2
date: 2020-08-30 13:00:00 +0900
categories: [Grind, Git]
tags: [git]
seo:
  date_modified: 2020-05-04 19:33:17 +0900
---

## rebase
- commit 관련 작업 할때 쓰더라
- 이전까지 그걸 썼고 이번엔 특정 시나리오 상에서 쓰일때?

## commit, branch 
```
          6-7(dev_1)
         /
1-2-3-4-5(master)
         \
          A-B(dev_2)
```
- 예를들어 위같은 상황일 떄 dev_1과 dev_2를 합치고싶다.
- 두가지 경우가 발생하는데 12345AB67 또는 1234567AB
- 보통 5까지는 remote에 잘 올라가있고 각자 다른 두 명이 dev_1과 dev_2를 각각 작업했을때,  
다시 push하게되면 둘 중 하나는(늦은사람) 이 상황이 벌어짐.  
해결은 merge를 이용하거나 rebase를 이용하는데 그 중 하나인 rebase를 이용한 방법