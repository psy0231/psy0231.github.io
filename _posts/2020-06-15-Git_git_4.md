---
title: 4. reset, revert
date: 2020-06-15 13:00:00 +0900
categories: [Grind, Git]
tags: [git]
seo:
  date_modified: 2020-06-15 16:13:22 +0900
---

## commit
- save point 라고 생각한다.
- 그럼, 이번판은 망했으니 이전 save로 돌아가는것도 가능한가?  
가능함. 보통 두개의 예시를 많이 볼 수 있음.
- 좋은 설명들은 아래 참고부터.


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

    $ cat test.md
    1
    2
    3

    ```
    - 원래 있던 4,5 commit 가 사라졌고 3으로 내려왔다.  
    그러니까 HEAD~2는 HEAD를 지금 가리키고 있는 포인트에서 2개 돌아가란 뜻.
    - 이때 실제 commit를 만들기 위해 수정했던 파일은 그 당시로 돌아가있다.진짜 이전 save point로 돌아간느낌이든다.
    - 작업 내용도 그 당시로 돌아가있다.

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

    $ cat test.md
    1
    2
    3
    4
    5

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
  
    $ cat test.md
    1
    2
    3
    4
    5

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

### reset 정리
- HEAD를 원하는 시점으로 옮기고 그 이후 commit는 다 지운다.
    - HARD는 완전히 그 시점으로 돌아감.
    - SOFT는 내용은 남기고 commit를 삭제함. 단, add까지 해놓은 상태.
    - MIXED는 내용을 남기고 commit를 삭제함. 단, add 도 안되어있음.

## revert 
- 되돌아가다 라고 나온다.
- 근데 아래 두 가지 경우 결과가 다르다.
    - 5층으로 쌓은 블럭이 있다. 이 중  3층을 없애 버렸다고 했을때, 4층과 5층은 남아있어야 하는가 없어져야 하는가.
    - 5년의 역사에서 3년째에 핵전쟁으로 4,5년까지 영향이있다. 3년째를 역사에서 지우면 4,5년에 있던 핵전쟁 피해는 있나?
- 하루 종일 뻘짓하다 뭔가 이상해서 확실하게 보는 방법몰라서 다시 작성함.
- 지금 쓰는내용은 뻘짓거리들 다 지우고 씀

    먼저는 파일 하나에 대해 revert하고 있었는데 이 파일 하나에 대해 revert는 HEAD에 대해 할 때 빼놓고 다른경우 전부 conflict 걸림  
    그래서 5개의 파일을 만들면서 각각의 commit를 만들고 그 경우에 대해 revert해봄.

- case 1
    ```
    $ ls 
    test.md

    $ git log --oneline
    ea2e280 (HEAD -> 1) 5
    ba42806 4
    b8103da 3
    32e5a2d 2
    c8441a0 1
    ```
    이렇게 commit가 있을 때 git revert HEAD 는 문제없이 함.  
    git revert commit3 하면 conflict  
    git revert HEAD~2 하면 conflict

    근데 다른 설명에선 revert할때 딱히 conflict에 대한 설명이 없었음.. 그냥 자연스러운건가 싶었는데 아래 보면 잘 되는것같다.  
    내가 뭔가 이상한것같다.  
    아니더라도 conflict내용이 이해가 잘 안간다.

    해서 예제를 바꿔서 다시 해보기로함.  
    file 하나가 아니라 5개 의 다른 파일을 생성하고 각각생성시 commit를 씀

- case 2
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

- 아래에서 두 경우 다 보기로.

### revert case 1

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
    - 단순히 reset와 비슷할 거라는 생각으로 
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
    - 이렇게 되면 위에 git revert HEAD 제외한 모든 revert에서 conflict났던게 대충 유추되는데 HEAD를 포인트까지 옮기고 지움 그리고 그 이후 commit 내용들을 그대로임 .
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
    실제로 해보면 
        ```
        $ git log --oneline
        2fee114 (HEAD -> master) 5
        d06ff1f 4
        6730662 3
        4cd12ca 2
        d06b272 1

        $ git revert d06ff1f..4cd12ca
        error: empty commit set passed
        fatal: revert failed
        ```
    - 일단 에러난다. 
    
        위 commit 는 순서상 1 -> 5 순서로 쌓았는데 revert 범위를 지정할때에도 그 방향으로 해줘야 하나봄  
        commit4..commit2 가 아닌 쌓은 순서대로 commit2..commit4로 가야 맞다.
    
        ```
        $ git revert 4cd12ca..d06ff1f
        ============================================
        Revert "4"

        This reverts commit d06ff1fb28551ad8c5884399ac85660be791baa0.

        # Please enter the commit message for your changes. Lines starting
        # with '#' will be ignored, and an empty message aborts the commit.
        #
        # On branch master
        # Revert currently in progress.
        #
        # Changes to be committed:
        #       deleted:    4.md
        #
        ~
        ==============================================

        Revert "3"

        This reverts commit 67306628e3f506153ba2b45e52bec38864a105be.

        # Please enter the commit message for your changes. Lines starting
        # with '#' will be ignored, and an empty message aborts the commit.
        #
        # On branch master
        # Revert currently in progress.
        #
        # Changes to be committed:
        #       deleted:    3.md
        #
        
        ==================================================

        $ git revert 4cd12ca..d06ff1f
        Removing 4.md
        [master b719edf] Revert "4"
        1 file changed, 0 insertions(+), 0 deletions(-)
        delete mode 100644 4.md
        Removing 3.md
        [master e5b2e77] Revert "3"
        1 file changed, 0 insertions(+), 0 deletions(-)
        delete mode 100644 3.md

        $ git log --oneline
        e5b2e77 (HEAD -> master) Revert "3"
        b719edf Revert "4"
        2fee114 5
        d06ff1f 4
        6730662 3
        4cd12ca 2
        d06b272 1

        psy02@psy-company MINGW64 ~/Desktop/root/revert_2 (master)
        $ ls
        1.md  2.md  5.md

        psy02@psy-company MINGW64 ~/Desktop/root/revert_2 (master)
        $
        ```
    - 이런 결과. 아디에서 봤던것처럼 순조롭게 자동으로 된다.
    - 근데 결과가 생각과는 다르다. commit 2 작업 내용이 살아있다 실제 revert 된것도 3,4에 대해서만 나왔다.

        그러니까 revert범위는 단일 commit를 지정하면 그 commit만 revert하고  
        범위로 지정 할 경우 commit이후..commit까지로 지정이 된다.

        범위 지정은 commit순서대로 옛날commit..최근commit으로 하는데 
        실행은 최근commit -> 옛날commit순으로 됨.  
        이건 당연해 보이는게 해당 commit의 작업 내용을 없애는 작업은 최근부터 과거순으로 해야 되감아지기때문에.

### revert case 2
- 이걸 단일 파일 하나에서 하면 conflict나는데 .. 파일 자체의 내용이 내부에서 바뀌니까 그런건가?
- 근데 git help revert 내용 중에  

    > 기존 커밋이 하나 이상 있으면 관련 패치가 도입 한 변경 사항을 되돌리고 이를 기록하는 새로운 커밋을 기록하십시오. 이를 위해서는 작업 트리가 깨끗해야합니다 (HEAD 커밋의 수정 사항 없음).

    라고 하는데 ...  
    이 내용이면 revert_1의 내용이 어느정도 맞는것같음.  
    근데 "도입 한 변경 사항을 되돌리고" 라면 이후 변경사항도 없어지는게 맞다고 생각하면 revert_1도 아님.  
    그래서 지금부터 하는건 결과가 다름.  
    이 둘의 혼동으로 지금 뭐가 맞는지 모르겠음

- 5층짜리 블럭에서 3층이 없어진다면  4,5층은 있어야 하는가 사라져야 하는가.
- 다시, 원본으로 와서
    ```
    $ git log --oneline
    ea2e280 (HEAD -> 1) 5
    ba42806 4
    b8103da 3
    32e5a2d 2
    c8441a0 1

    ```
- git revert HEAD 
    ```
    $ git revert HEAD
    [1 b26730a] Revert "5"
    1 file changed, 1 insertion(+), 2 deletions(-)

    $ git log --oneline
    b26730a (HEAD -> 1) Revert "5"
    ea2e280 5
    ba42806 4
    b8103da 3
    32e5a2d 2
    c8441a0 1

    $ cat test.md
    1
    2
    3
    4
    ```
    - 잘 된다. 파일 내용도 고쳐졌고 HEAD이전으로 돌아가되 그게대한 commit가 남고 1~5까지의 commit도 전부 남아있다. 
    - 내가지금 reset, revert이내용만 3번쨰 쓰는게 빡치는건아닌데 계속 반복되는 의미없는것들은 설명안하고 넘어가겠음

- git revert HEAD~2
    ```
    psy02@psy-company MINGW64 ~/Desktop/root/revert (1)
    $ git revert HEAD~2
    Auto-merging test.md
    CONFLICT (content): Merge conflict in test.md
    error: could not revert b8103da... 3
    hint: after resolving the conflicts, mark the corrected paths
    hint: with 'git add <paths>' or 'git rm <paths>'
    hint: and commit the result with 'git commit'

    psy02@psy-company MINGW64 ~/Desktop/root/revert (1|REVERTING)
    $
    ```
    - 이 전과 다르다 conflict난다. 내용을 보자.
    ```
    1
    <<<<<<< HEAD
    2
    3
    4
    5
    =======
    2
    >>>>>>> parent of b8103da... 3
    ```
    - HEAD부터 ===까지 지우고 parent 줄도 지운다 
    ```
    1
    2
    ```
    - 이게 남은 내용일꺼임. 저장하고 add, commit 하면 
        ```
        psy02@psy-company MINGW64 ~/Desktop/root/revert (1)
        $ git log --oneline
        fb189b3 (HEAD -> 1) revert
        ea2e280 5
        ba42806 4
        b8103da 3
        32e5a2d 2
        c8441a0 1

        psy02@psy-company MINGW64 ~/Desktop/root/revert (1)
        $ cat test.md
        1
        2
        ```
    - 그럼 다시 생각해보자.

        원본에서 HEAD는 commit5에있었다. HEAD~2했으니 commit3을 revert했겠지  
        근데 "revert_파일들"에서의결과와 다르다.  
        전자는 3층의 블럭을 빼고 4,5층은 2층위에 쌓은 모양이면  
        지금은 3층부터 그 윗층을 전부 박살냈는데 이전 기억은 남겨놓은것  
        
        conflict상황에 수정을 1245이렇게 하면 이 전과 같긴한데 그래도  
        위 수정 필요한곳 incomming는 2만 붙어있음.... 내가 저 수정을 잘 못하는건가?
        
        단순하게, <<<부터 ===까지 이전내용 다시, ===부터 >>>까지 바뀔내용으로 알고있는데  
        이전내용 무시하고 새 변경사항만 하면 위처럼 되는게 맞는걸로 알고있음

    - git revert 32e5a2d..ba42806 
        ```
        psy02@psy-company MINGW64 ~/Desktop/root/revert (1)
        $ git revert 32e5a2d..ba42806
        Auto-merging test.md
        CONFLICT (content): Merge conflict in test.md
        error: could not revert ba42806... 4
        hint: after resolving the conflicts, mark the corrected paths
        hint: with 'git add <paths>' or 'git rm <paths>'
        hint: and commit the result with 'git commit'

        1
        2
        <<<<<<< HEAD
        3
        4
        5
        =======
        3
        >>>>>>> parent of ba42806... 4

        1
        2
        3

        psy02@psy-company MINGW64 ~/Desktop/root/revert (1|REVERTING)
        $ git add .

        psy02@psy-company MINGW64 ~/Desktop/root/revert (1|REVERTING)
        $ git commit -m "revert 4?"
        [1 b0c4762] revert 4?
        1 file changed, 2 deletions(-)

        psy02@psy-company MINGW64 ~/Desktop/root/revert (1|REVERTING)
        $ git revert --continue
        Auto-merging test.md
        CONFLICT (content): Merge conflict in test.md
        error: could not revert b8103da... 3
        hint: after resolving the conflicts, mark the corrected paths
        hint: with 'git add <paths>' or 'git rm <paths>'
        hint: and commit the result with 'git commit'

        1
        <<<<<<< HEAD
        2
        3
        =======
        2
        >>>>>>> parent of b8103da... 3

        1
        2


        psy02@psy-company MINGW64 ~/Desktop/root/revert (1|REVERTING)
        $ git add .

        psy02@psy-company MINGW64 ~/Desktop/root/revert (1|REVERTING)
        $ git commit -m "revert 3??"
        [1 9519ab2] revert 3??
        1 file changed, 1 deletion(-)

        psy02@psy-company MINGW64 ~/Desktop/root/revert (1)

        ```
    - 총 2번의 과정을 거침.  
    
        비슷한점은 revert는 단일 commit를 선택할 때는 그 commit에 대해 작용하고  
        범위를 지정하면 시작 commit를 포함하지 않고 다음 commit부터 최근 범위로 지정된 commit를 포함해 작용함
        
        또, 변화 과정을 보면 알겠지만 최근 변화분부터 거꾸로 하고있음.

### revert 결과
- 그럼 결과를 정리해보면 
    
    파일에 대해서는 commit때 작업만 revert함  
    내부에 대해서는 commit을 포함한 이후 전부 사라짐.  
    단 이 두 경우 모두 이 전 commit에 대해서는 관여를 안함

    전자의 경우 핵전쟁 시점으로 돌아가서 핵전쟁을 멈췄어도 전쟁 흔적은 남아있는경우고  
    후자의 경우는 핵전생시점에서 전쟁을 멈추고 평화로워짐  
    단, 둘 다 역사교과서에는 핵전쟁 났다 써있음
- 다시, 위의 revert에서 처음에 가졌던 의문에 대해 생각해보면 

    예시가 좀 부적절했지만 대충 결과는  
    revert했을 떄 이 후 결과들이 독립젹인 다른 시행이면 살아있고  
    revert한 시점의 연속적인 작업물이 있다면 이후 작업물도 다 사라짐.

## reset vs revert
- 서로 해주는게 좀 다른데 
    
    reset는 HEAD를 움직여 새 HEAD그 이후 시점부터 수정을 함  
    수정 사항은 option에 따라 다른데 commit가 지워지는건 같음.

    revert는 HEAD를 움직여 새 HEAD가 가리키는 commit의 수정사항을 취소함.  
    revert한 commit이후 서로 영향이 없는 작업을 경우 남아있고 그렇지 못한경우는 수정을 직접 확인해야하는듯하다.  
    범위를 지정했다면 범위의 가장 오래된 commit이후부터 범위 마지막을 포함한 commit까지...
    아... 해당 commit내용을 되돌리는거면 다른 commit에서 생긴 내용은 건들 필요가 없고, 해당 commit에 영향을 받으면 물어보고? 
    
- 근데 위 했던 테스트들이 확실이 이렇게 하는게 맞나는 잘 모르겠다. 
- 이 둘의 동작상의 차이점은 위에 적은 식이고   
둘은 어떤때 쓰이나 하는 말들이 참고했던 자료들 거의 마지막에 항상 따라다니던데

    일단, 혼자쓸때는 별 문제 없어보임.  
    이 전으로 돌아가는구나, 작업내용이 없어지거나 유지되거나 하는정도.   
    근데 원격의 저장소를 쓸 때는 조금 다름.

    원격 저장소로 push할 때 원격 저장소의 최신 commit까지의 내용이 같은게 내꺼에도 있어야함  
    원격에 1-2-3의 commit가 있다면 내가 올리는 commit는 1-2-3을 포함한 4-5 총 1-2-3-4-5가 되어야함.  
    
    그럼 예를들어 1-2-3-4-5를 push했다.  

    reset의 경우 1-2-3으로 돌아와 1-2-3-4'-5' 를 만들었다.  
    이건 push가 안된다.
    3-4-5는 이미 원격에 있는데 지금 올리는 내용은 3-4'-5'라서.  
    이미 commit가 원격으로 push가 된 후라면 이전 commit는 수정하기 까다로워진다는것임.  
    
    revert의 경우 1-2-3-4-5-3(revert)-4'-5'라고 수정을 하다면 push까지 가능하다.   
    이건 이 전의 commit에 영향을 주지 않으면서 돌아간 시점을 다른 commit으로 기록하기 때문에 원격과 로컬에 같은 commit부터 시작할 수 있기때문이다.

## fin
- 이거 다 하고도 이해가 되는듯 안되는듯함.
- 나중에 또 읽어보면서 수정할거 있음 계속 해야겠다.

## 참고
- [1. reset과 revert](https://victorydntmd.tistory.com/79)
- [git revert multiple-commits](https://github.com/HomoEfficio/dev-tips/blob/master/Git%20reverting%20multiple-commits.md)
- [git revert로 커밋 되돌리기](https://blog.outsider.ne.kr/1166)
- [What's the difference between Git Revert, Checkout and Reset?](https://stackoverflow.com/questions/8358035/whats-the-difference-between-git-revert-checkout-and-reset)