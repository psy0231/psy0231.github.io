---
title: 4.commit rewind
date: 2020-05-03 13:00:00 +0900
categories: [Grind, Git]
tags: [git]
seo:
  date_modified: 2020-05-04 19:33:17 +0900
---

## commit
- save point 라고 생각한다.
- 그럼, 이번판은 망했으니 이전 save로 돌아가는것도 가능한가?
- 위 질문에 대한 답은 역시 쌉가능이다. 방법은 2가지 키워드가 자주 보여 정리한다.
- 일단 이 설명들을 잘 해놓은 걸 발견해서 이걸 참고함.

## reset
- 초기화.
- 원하는 commit로 돌아가는데 그 이후 commit는 지워진다함.
- 세가지 옵션이 있는데 조금씩 다르다.
- 기본 예로 
    ```
    $ git log --oneline
    f11bb5a (HEAD -> reset_hard, reset_soft, reset_mixed, master) 5
    e2d7109 4
    da141ef 3
    b24316b 2
    59bb278 1
    ea27983 initial commit
    ```
    
- git reset --option 로 시작하는데 

        git reset --option [HEAD~?]
        git reset --option [commit id]
    
    이렇게 작성.

### hard
- 먼저 HEAD를 움직여 보자.
    ```
    $ git reset --hard HEAD~2
    HEAD is now at da141ef 3

    $ git status
    On branch reset_hard
    nothing to commit, working tree clean

    $ git log --oneline
    da141ef (HEAD -> reset_hard) 3
    b24316b 2
    59bb278 1
    ea27983 initial commit
    ```
    - 원래 있던 4,5 commit 가 사라졌고 3으로 내려왔다.  
    그러니까 HEAD~2는 HEAD를 지금 가리키고 있는 포인트에서 2개 돌아가란 뜻.
    - 이때 실제 commit를 만들기 위해 수정했던 파일은 그 당시로 돌아가있다.진짜 이전 save point로 돌아간느낌이든다.

- 직접 commit id를 넣는방법
    ```
    $ git log --oneline
    da141ef (HEAD -> reset_hard) 3
    b24316b 2
    59bb278 1
    ea27983 initial commit

    $ git reset --hard b24316b
    HEAD is now at b24316b 2

    $ git log --oneline
    b24316b (HEAD -> reset_hard) 2
    59bb278 1
    ea27983 initial commit

    $ git status
    On branch reset_hard
    nothing to commit, working tree clean
    ```
    - HEAD가 해당 commit로 옮겨 지면서 그 이후 만들어졌던 commit 들이 지워졌다.
- 어차피 HEAD를 옮기면서 commit을 제어하게 되는데 위(최근)부터 내려가나 내가 원하는 commit id를 직접넣어 바로 찾거나 하는것만 다르다.
- 아무튼 중요한건 실제 save point였던 commit들이 지워졌다는거고, 그 commit가 지워지면 그당시 작업 내용들까지 해당 commit로 돌아간다는것이다.
- 작업 내용을 어느 시점으로 화끈하게 날릴때.

### soft
- 다시, 처음으로 돌아가 옵션만 바꾸고 해본다.
    ```
    $ git reset --soft HEAD~2    
    ```
- ???  
hard와 다르게 아무런 반응이 없다.  
이상할때는 log랑 status를 수시로 쳐보는게 좋다하니까..
    ```
    $ git log --oneline
    da141ef (HEAD -> reset_soft) 3
    b24316b 2
    59bb278 1
    ea27983 initial commit

    $ git status
    On branch reset_soft
    Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
            modified:   test.md
    ```
- 뭔가 이상하다.
    
    hrad의 경우 돌아가려는 해당commit로 완벽하게 돌아갔다. commit 한 직후로.  
    근데 soft는 조금 다르다. HEAD는 원하는 시점으로 옮겨졌는데 test.md파일이 mod상태로 돌아갔다. 이건 commit를 기다리는 stage상태로 돌아갔다는 말임.

    test.md파일 내용을 확인해 보면 내용이 다 살아있다.  
    commit message가 5의( reset전 원본 commit 중 맨 마지막 commit) 작업 내용까지 다 살아있었다.

    soft는 commit만 지우고 그 작업 내용은 그대로 두는 것이다. 

- 이후 계속 해보면 
    ```
    $ git commit -m "new "
    [reset_soft c546097] new
    1 file changed, 3 insertions(+), 1 deletion(-)

    $ git log --oneline
    c546097 (HEAD -> reset_soft) new
    da141ef 3
    b24316b 2
    59bb278 1
    ea27983 initial commit

    ```
    - 실제 test.md파일 내용은 마지막 수정까지(reset soft전) 온전히 살이있다.
    - commit만 지워지고 작업내용은 남음. 새 commit로 덮음.
- 세상에.. 그럼 잦은 수정에 commit까지 너저분해지면 쓸만한건가 싶다.

### mixed
- 옵션을 적지 않으면 이게 기본이라한다.
- 옵션을 꼭 적는습관을 길러야겠다... 
- 다시 원본으로 돌아가서 옵션바꾸고 다시실행.
    ```
    $ git reset --mixed HEAD~2
    Unstaged changes after reset:
    M       test.md

    $ git log --oneline
    da141ef (HEAD -> reset_mixed) 3
    b24316b (reset_hard) 2
    59bb278 1
    ea27983 initial commit

    $ git status
    On branch reset_mixed
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
            modified:   test.md

    no changes added to commit (use "git add" and/or "git commit -a")
    ```
- 위 soft와 비슷해 보인다. 실제 파일 내용 변경도 마지막 commit 내용을 갖고있다.  
다른점은 soft는 add까지 한 상태 이건 그것도 안되어있음.
- 계속 해보면 
    ```
    $ git add .

    $ git commit -m "mixed"
    [reset_mixed 1ae9a2d] mixed
    1 file changed, 3 insertions(+), 1 deletion(-)

    $ git log --oneline
    1ae9a2d (HEAD -> reset_mixed) mixed
    da141ef 3
    b24316b (reset_hard) 2
    59bb278 1
    ea27983 initial commit
    ```
- 사실 soft와 차이는 잘 모르겠다. commit는 여전히 날라갔고 작업 내용은 여전히 남아있다.  
add차이만 있다 뿐이지..

## revert
- 되돌아가다 라고 나온다.
- 하루 종일 뻘짓하다 뭔가 이상해서 확실하게 보는 방법을 알게되 다시 작성함.
- 지금 쓰는내용은 뻘짓거리들 다 지우고 씀

    먼저, 파일 하나에 대해 수정하고있었는데 이 파일 하나에 대한 수정 commit 5개 중 revert는 HEAD에 대해 할 때 빼놓고 다른경우 전부 conflict 걸림

    여기서 다른 경우라고 하는건  
    ```
    commit5(HEAD) 5
    commit4 4
    commit3 3
    commit2 2
    commit1 1
    ```
    이렇게 commit가 있을 때 git revert HEAD 는 문제없이 함.  
    git revert commit3 하면 conflict  
    git revert HEAD~2 하면 conflict

    근데 다른 설명에선 revert할때 딱히 conflict에 대한 설명이 없었음.. 그냥 자연스러운건가 싶었는데 아래 보면 잘 되는것같다.  
    내가 뭔가 이상한것같다.  
    아니더라도 conflict내용이 이해가 잘 안간다.

    해서 예제를 바꿔서 다시 해보기로함.  
    file 하나가 아니라 5개 의 다른 파일을 생성하고 각각생성시 commit를 씀

- file
    ```
    $ ls
    1.md  2.md  3.md  4.md  5.md

    $ git log --oneline
    2fee114 (HEAD -> master) 5
    d06ff1f 4
    6730662 3
    4cd12ca 2
    d06b272 1

    ```
- 이제 다시 해보자. git revert HEAD
    ```
    Revert "5"

    This reverts commit 2fee114dcda180f753fa6957e2be58e90ed72002.

    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    #
    # On branch master
    # Changes to be committed:
    #       deleted:    5.md
    #
    ~

    ```
    - 에디터 열리면서 순조로움
    ```
    $ git revert HEAD
    Removing 5.md
    [master 1c4e1c7] Revert "5"
    1 file changed, 0 insertions(+), 0 deletions(-)
    delete mode 100644 5.md

    $ ls
    1.md  2.md  3.md  4.md

    $ git log --oneline
    1c4e1c7 (HEAD -> master) Revert "5"
    2fee114 5
    d06ff1f 4
    6730662 3
    4cd12ca 2
    d06b272 1
    ```
    - revert는 HEAD에서 했던 작업을 취소 하면서 취소 했다는 새 기록(commit)을 남긴다.  
    reset와 다른점은 이전 commit에 대해 영향을 주지 않는 다는것이다.  
    remote는 나중관점이니 배제하고 일단 이시점에서만 보면 특정 작업을 했다는 저장(commit)은 남기되 작업 내용 자체는 취소했으니 그 전으로도 다시 돌아갈수 있음.  
    여기에서 git reset --hard HEAD~1하면 처음 상태로 돌아감. reset와 달리 commit자치게 없어진게 아니라 좀 더 안전한가?  
    reset도 soft나 mixed는 작업 내용을 남길 수 있었는데 마지막 최종작업만 남기고 commit는 날라가긴함.   
    revert는 작업은 날리되 commit를 남김.

- 중간 특정 작업
    - 여기서부터 난리였는데 reset의 경우 HEAD~? 으로 정하는 경우 지정된 HEAD이후 commit는 다 지워졌다. 
    ```
    git revert HEAD~2
    ```
    - 이러면 어떻게 될까 내 예상은 이랬다.
    ```
    $ ls
    1.md  2.md  3.md

    $ git log --oneline
    1c4e1c7 (HEAD -> master) Revert "3"
    2fee114 5
    d06ff1f 4
    6730662 3
    4cd12ca 2
    d06b272 1
    ```
    - 이런식.  
    다시말해, HEAD가 옮겨진 commit 3까지 작업내용은 살아있고 이후는 다 사라짐. 여전히 commit들은 다 살아있고 revert 3 commit 하나 더 추가.  
    그런데 아님 .
    - 진짜 결과는 
    ```
    $ ls
    1.md  2.md  4.md  5.md

    $ git log --oneline
    d9dd5e7 (HEAD -> master) Revert "3"
    2fee114 5
    d06ff1f 4
    6730662 3
    4cd12ca 2
    d06b272 1
    ```
    - 이렇게 되면 위에 git revert HEAD 제외한 모든 revert에서 conflict났던게 대충 유추되는데 HEAD를 포인트까지 옮기고 지움 그리고 그 이후 commit 내용들을 계속함 .
    - 이부분은 다시,
    - reset와 revert조금씩 다름.
- 범위를 지정.
    - 그럼 이것도 얘를들어 git revert commit4..commit2 이러면 
    ```
    $ ls
    1.md   5.md

    $ git log --oneline
    d9dd5e7 (HEAD -> master) Revert "4-2"
    2fee114 5
    d06ff1f 4
    6730662 3
    4cd12ca 2
    d06b272 1
    ```
    - 이것도 아님.
    

## 참고
- []()  
-  Bla bla <sup id="a1">[1](#f1)</sup>
- <b id="f1">1</b> Footnote content here. [↩](#a1)
- https://victorydntmd.tistory.com/79
- https://github.com/HomoEfficio/dev-tips/blob/master/Git%20reverting%20multiple-commits.md
- https://blog.outsider.ne.kr/1166