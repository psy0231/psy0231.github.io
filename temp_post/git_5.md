---
title: 3. rebase
date: 2020-05-03 13:00:00 +0900
categories: [Grind, Git]
tags: [git]
seo:
  date_modified: 2020-05-04 19:33:17 +0900
---

## commit
- 작업 중간중간 세이브 포인트 느낌.
- 중요함. 
- 그래서 commit에 대해 조작하는걸 조금 더 보기로 함.
- 이 전까지 commit만들고, 그 commit 중 어떤 시점으로의 이동을 하는 방법을 봤다.  
이번엔 commit끼리 합하고, 나누고, message에 대한 수정 등에 대해 보려한다.

## rebase
- commit에 대한 제어(?)를 할때 씀 
- 여기서 말하는 제어는 생성을 제외한 다른 것들로 수정 제거 병합 분할등.
- 예를들어 
    ```
    $ git log --oneline
    ea2c3c5 (HEAD -> master) 5
    10fa8b4 4
    164b21c 3
    02e772a 2
    a083dea 1
    76e8086 init
    ```
- git rebase -i --root 를 해보면 아래 내용이 에디터에서 열림.
    ```
    pick 76e8086 init
    pick a083dea 1
    pick 02e772a 2
    pick 164b21c 3
    pick 10fa8b4 4
    pick ea2c3c5 5

    # Rebase ea2c3c5 onto 164b21c (6 commands)
    #
    # Commands:
    # p, pick <commit> = use commit
    # r, reword <commit> = use commit, but edit the commit message
    # e, edit <commit> = use commit, but stop for amending
    # s, squash <commit> = use commit, but meld into previous commit
    # f, fixup <commit> = like "squash", but discard this commit's log message
    # x, exec <command> = run command (the rest of the line) using shell
    # b, break = stop here (continue rebase later with 'git rebase --continue')
    # d, drop <commit> = remove commit
    # l, label <label> = label current HEAD with a name
    # t, reset <label> = reset HEAD to a label
    # m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
    # .       create a merge commit using the original merge commit's
    # .       message (or the oneline, if no original merge commit was
    # .       specified). Use -c <commit> to reword the commit message.
    #
    # These lines can be re-ordered; they are executed from top to bottom.
    #
    # If you remove a line here THAT COMMIT WILL BE LOST.
    #
    # However, if you remove everything, the rebase will be aborted.
    #
    ```
    - git rebase -i --root
        - -i는 --interective로 ... 상호작용? 대화형이라는데 ..잘 모르겠.. 그냥 이거 쓰래
        - --root는 root가 되는 commit부터 보겠다는 뜻  
        이전 다른 것들처럼 HEAD~3등 할 수 있는데 HEAD를 움직인 경우 그 다음 commit부터 수정가능.
        - 대충 이렇게 알고있는데 시작할때 --root 쳐서 했던거로 기억함.
    - pick : commit를 사용하겠다함. 근데 기본으로 다 pick되어있고.. 무슨의미인지 모르겠음
    - reword : commit message수정
    - edit : commit message수정, 근데 commit 작업 내용 수정도 가능
    - squash : 이전 commit 에 합침..
    - fixup : squash와 같은데 message는 무시함
    - exec : 그냥 cmd 실행인듯?
    - break : rebase 일시정지
    - drop : commit 지움
    - label :
    - reset : HEAD를 labal로 
    - merge : commit merge
    - 내용은 pick부분을 수정한다.

## reword, edit
- 아래 생성되는 주석부분 생략
- 둘 다 commit message 고치는것같은데 ..차이점이 있나봄 
    ```
    psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase)
    $ git log --oneline
    f11bb5a (HEAD -> rebase) 5
    e2d7109 4
    da141ef 3
    b24316b 2
    59bb278 1
    ea27983 initial commit

    ```
- 이상태로 함.
- 중요한건 이 작업을 하면 작업한 commit부터는 새 commit가 된다는거임.

### reword
- git rebase -i --root
    ```
    pick ea27983 initial commit
    pick 59bb278 1
    reword a32f81b 2
    pick 03cd84c 3
    pick 088c01e 4
    pick ab2dbf6 5
    ```
    - 이렇게 editor로 바뀌는데 그 중 바꾸고 싶은 부분을 위처럼 reword로 수정 후 저장-종료  
    하면 다시 editor로 뜨는데 내가 바꾸고 싶던 commit의 commit message가 보임.
        ```
        2-fix
        ```
    - 수정하고 wq
        ```
        $ git log --oneline
        052ae35 (HEAD -> rebase) 5
        3127def 4
        2ce337e 3
        d286381 2-fix
        59bb278 1
        ea27983 initial commit
        ```
    - commit message가 수정됨.
    - 참고로, reword하면 그 commit부터는 새 commit임. (commit Id가 바뀜)

### edit
- 이번엔 위상태에서 2-fix부분에 edit를 해보자.
    ```
    psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase)
    $ git rebase -i --root
    Stopped at d286381...  2-fix
    You can amend the commit now, with

    git commit --amend

    Once you are satisfied with your changes, run

    git rebase --continue

    psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase|REBASE 3/6)

    ```
    - 상태가 바뀌었고 힌트도 생겼는데 
    --amend는 commit message를 고치는거라함.  
    이렇게만 하면 reword랑 같을텐데 
        ```
        $ touch test2.md 
        ```
    - 이걸 해볼까?
        ```
        psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase)
        $ ls
        test.md  test2.md

        psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase)
        $ git log --oneline
        f2e9ce3 (HEAD -> rebase) 5
        5e62373 4
        607f282 3
        1ec0f9e 2-fix and add contents
        59bb278 1
        ea27983 initial commit

        psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase)
        ```
- edit는 그 commit에서의 작업 내용도 수정 할 수 있다.  
단, 이 경우 revert처럼 한 파일 안애서 그 뒤의 commit에 영향을 줄 경우 conflict나고 이건 다 처리 해줘야함.  
위는 새 파일 만들었으니 독립적인 다른시행인가봄 이것도 revert때랑 같은 경우.

## squash, fixup
- 둘 다 이전 commit에 합치는 데 합치는 commit의 message를 쓰냐(squash) 안쓰냐(fixup)

### squash
- 일단 해보자.
    ```
    pick ea27983 initial commit
    pick 59bb278 1
    pick b24316b 2
    pick da141ef 3
    pick e2d7109 4
    squash f11bb5a 5
    ```
    - 위 처럼 하면 
        ```
        # This is a combination of 2 commits.
        # This is the 1st commit message:

        4

        # This is the commit message #2:

        5

        # Please enter the commit message for your changes. Lines starting
        # with '#' will be ignored, and an empty message aborts the commit.
        #
        # Date:      Thu May 7 12:41:20 2020 +0900
        #
        # interactive rebase in progress; onto a674841
        # Last commands done (6 commands done):
        #    pick e2d7109 4
        #    squash f11bb5a 5
        # No commands remaining.
        # You are currently rebasing branch 'rebase' on 'a674841'.
        #
        # Changes to be committed:
        #       modified:   test.md
        #
        # Untracked files:
        #       test2.md
        #
        ~
        ~

        ```
    - 결과는 
        ```
        psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase)
        $ git rebase -i --root
        [detached HEAD 9a33cac] 4
        Date: Thu May 7 12:41:20 2020 +0900
        1 file changed, 3 insertions(+), 1 deletion(-)
        Successfully rebased and updated refs/heads/rebase.

        $ git log
        commit 9a33cac4aa2f70efc31a2c4a481236e2d8ccb59c (HEAD -> rebase)
        Author: psy_comp <psy0231@gmail.com>
        Date:   Thu May 7 12:41:20 2020 +0900

            4

            5

        commit da141efb96e0eef14dc3c60c3d558ecbc5e5521d (temp)
        Author: psy_comp <psy0231@gmail.com>
        Date:   Thu May 7 12:41:11 2020 +0900

            3

        commit b24316b3120631230fdb57f689a0d7c1620174b4 (reset_hard)
        Author: psy_comp <psy0231@gmail.com>
        Date:   Thu May 7 12:41:04 2020 +0900

            2

        commit 59bb2788175cb909883587f56bf0106f8c4785b5
        Author: psy_comp <psy0231@gmail.com>
        Date:   Thu May 7 12:40:55 2020 +0900

            1

        commit ea27983f66dd7881757cb5e76eaaf3f4d1a8e05c
        Author: psy_comp <psy0231@gmail.com>
        Date:   Thu May 7 12:39:33 2020 +0900

            initial commit

        ```
- 두 개가 합쳐짐
### fixup
- 요건 다르다니까  
    ```
    pick ea27983 initial commit
    pick 59bb278 1
    pick b24316b 2
    fixup da141ef 3
    pick 9a33cac 4

    psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase)
    $ git rebase -i --root
    Successfully rebased and updated refs/heads/rebase.

    psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase)
    $ git log --oneline
    e8091a3 (HEAD -> rebase) 4
    5a95b4c 2
    59bb278 1
    ea27983 initial commit
    ```
- 더 간단해짐 중간과정이 없으니까 암튼 하나로 합쳐진다. commit끼리 

## exec, drop
- 후딱 가봄
    ```
    pick ea27983 initial commit
    pick 59bb278 1
    exec git log --oneline
    drop 5a95b4c 2
    exec git log --oneline
    pick e8091a3 4

    psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase)
    $ git rebase -i --root
    Executing: git log --oneline
    59bb278 (HEAD) 1
    ea27983 initial commit
    Executing: git log --oneline
    59bb278 (HEAD) 1
    ea27983 initial commit
    error: could not apply e8091a3... 4
    Resolve all conflicts manually, mark them as resolved with
    "git add/rm <conflicted_files>", then run "git rebase --continue".
    You can instead skip this commit: run "git rebase --skip".
    To abort and get back to the state before "git rebase", run "git rebase --abort".
    Could not apply e8091a3... 4
    Auto-merging test.md
    CONFLICT (content): Merge conflict in test.md

    psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase|REBASE 6/6)
    $ git rebase --skip
    Successfully rebased and updated refs/heads/rebase.

    psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase)
    $ git log --oneline
    59bb278 (HEAD -> rebase) 1
    ea27983 initial commit

    psy02@psy-company MINGW64 ~/Desktop/root/reset (rebase)
    $ 
    ```
- exec는 중간중간 그냥 실행할거 
- commit 지움.

## merge
- 
## commit 정리.
- 다시한번, save pooint임.
- 각 commit는 message를 남겨 뭘 했나 쓸 수 있다.
    - 일일이 프로젝트를 열어 변화를 확인하는 멍청한짓은 안해도됨.  
    - git log 정도 선에서 컷.(나중에 diff도)
- 근데 한 commit은 어느선이 적당한가를 물어본적이 있었는데 잘 모르겠다 했다.
    - 정확히는 잘 모른다가 아니라 그때그떄 다르다? 이느낌이었음.  
    - 많이 하다보면 알게된다, pm의 입맛에 따라 달라진다. 정도.
    - 그래서 PR reject 사유에 a,b commit는 합치고 c는 c,c_1로 나눠서 다시 PR주세요 같은 요청도 있었음
- 이 point들을 잘 이용해서 작업을 되돌릴수도, 작업 자체를 없앨수도있었고 이에대한 기록들을 선택적으로 가져갈수도 있었다.
    - 그래서 유명한 프로젝트같은거 들어가서 commit을 처음으로 돌려보면 그 프로젝트의 처음을 볼 수 있다.?? 
    - 또, 예를들어 하나 큰 class 를 만들었던 commit하나 있다치면, 이걸 기능별로 2개로 나누고 commit로 이에따라 둘로 다시 쪼개던가 하더라.
    - 자잘한 수정이 많이 일어난 부분은 commit 서너개 다 합치거나..
- 근데 ... 말그대로 save point라면, 이걸 직접 control하는게 옳은가?  
    - git reset --hard HEAD~1 하려던걸 실수로 git reset --hard HEAD~12하면? 
