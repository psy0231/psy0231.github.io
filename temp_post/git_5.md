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
- 일단 commit 만드는거 자체는 이 전에 했으니 그건 넘기고.

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
    - -i는 --interective로 ...
    - --root는 root가 되는 commit부터 보겠다는 뜻
- 위 설명은 ~~~(위에 대한 각 설명)
- 내용은 pick부분을 수정한다.

## commit message 수정
- 164b21c 3의 내용을 수정하려고 한다.

1. 전체 commit리스트에서 선택하는 방법
- 위 까지 상태로 만들고 아래처럼 수정 후 에디터를 저장/종료
    ```
    pick 76e8086 init
    pick a083dea 1
    pick 02e772a 2
    edit 164b21c 3
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
    - 이제부터 상태가 바뀐걸 볼 수 있음
        ```
        psy02@psy-aw MINGW64 /d/Workspace/Blog/git_test/Rebase (fix_msg|REBASE 4/6)
        $ ^C

        psy02@psy-aw MINGW64 /d/Workspace/Blog/git_test/Rebase (fix_msg|REBASE 4/6)
        $ git status
        interactive rebase in progress; onto f82d6e8
        Last commands done (4 commands done):
        pick 02e772a 2
        edit 164b21c 3
        (see more in file .git/rebase-merge/done)
        Next commands to do (2 remaining commands):
        pick e9d97ca 4
        pick c110a39 5
        (use "git rebase --edit-todo" to view and edit)
        You are currently editing a commit while rebasing branch 'fix_msg' on 'f82d6e8'.
        (use "git commit --amend" to amend the current commit)
        (use "git rebase --continue" once you are satisfied with your changes)

        nothing to commit, working tree clean

        psy02@psy-aw MINGW64 /d/Workspace/Blog/git_test/Rebase (fix_msg|REBASE 4/6)
        $ git log --oneline
        5533212 (HEAD) 3
        02e772a 2
        a083dea 1
        76e8086 init

        ```
        - 설명이 길다..... 일단 다른점을 좀 보면  
        상태가 (fix_msg)로 branch를 나타내야 하는 곳이(fix_msg|REBASE 4/6) 으로 바뀜.  
        git log로 확인 했을 때 log가 정상적으로 다 안나오고 pick를 edit로 바꿨던 줄까지만 나옴.  
        (HEAD -> master) 에서 HEAD가 edit로 바꾼 줄로 옮겨옴.
        - 다른건 잘 모르겠고 아래 두 줄만 보면

                (use "git commit --amend" to amend the current commit)
                (use "git rebase --continue" once you are satisfied with your changes)

            이라한다. 목적에 맞는걸 찾은것같다.
    
    - git commit --amend를 입력하면 아래 내용이 에디터에서 열림.
        ```
        3

        # Please enter the commit message for your changes. Lines starting
        # with '#' will be ignored, and an empty message aborts the commit.
        #
        # Date:      Wed May 6 20:57:21 2020 +0900
        #
        # interactive rebase in progress; onto f82d6e8
        # Last commands done (4 commands done):
        #    pick 02e772a 2
        #    edit 164b21c 3
        # Next commands to do (2 remaining commands):
        #    pick e9d97ca 4
        #    pick c110a39 5
        # You are currently editing a commit while rebasing branch 'fix_msg' on 'f82d6e8'.
        #
        # Changes to be committed:
        #       modified:   test.md
        #
        ~
        ```
        - 이제 commit message를 수정 할 수 있다.
        - 내용을 수정 후 저장/종료
    - 에디터를 빠져 나와서 상태 확인해도 똑같은데 위 설명처럼 계속 해보자.
    - git rebase --continue로 작업을 끝낸다.
        ```
        psy02@psy-aw MINGW64 /d/Workspace/Blog/git_test/Rebase (fix_msg|REBASE 4/6)
        $ git rebase --continue
        Successfully rebased and updated refs/heads/fix_msg.

        psy02@psy-aw MINGW64 /d/Workspace/Blog/git_test/Rebase (fix_msg)
        $

        ```
    - commit message를 다시 확인해보면
        ```
        psy02@psy-aw MINGW64 /d/Workspace/Blog/git_test/Rebase (fix_msg)
        $ git log --oneline
        c24d6c1 (HEAD -> fix_msg) 5
        219a37e 4
        7503083 3-fix
        02e772a 2
        a083dea 1
        76e8086 init

        psy02@psy-aw MINGW64 /d/Workspace/Blog/git_test/Rebase (fix_msg)
        $

        ```
    - 고쳐진게 확인됨.
    - HEAD도 다시 돌아왔다.
## commit 정리.
- 다시한번, save pooint임.
- 각 commit는 message를 남겨 뭘 했나 쓸 수 있다.
    - 일일이 프로젝트를 열어 변화를 확인하는 멍청한짓은 안해도됨.  
    - git log 정도 선에서 컷.(나중에 diff도)
- 근데 한 commit는 어떻게 쪼갤지에 대한 질문을 했었는데 그건 잘 모른다했다.
    - 정확히는 잘 모른다가 아니라 그때그떄 다르다? 이느낌이었음.  
    - 많이 하다보면 알게된다, pm의 입맛에 따라 달라진다. 정도.
    - 그래서 PR reject 사유에 a,b commit는 합치고 c는 c,c_1로 나눠서 다시 PR주세요 같은 요청도 있었음
- 이 point들을 잘 이용해서 작업을 되돌릴수도, 작업 자체를 없앨수도있었고 이에대한 기록들을 선택적으로 가져갈수도 있었다.
    - 그래서 유명한 프로젝트같은거 들어가서 commit을 처음으로 돌려보면 그 프로젝트의 처음을 볼 수 있다.?? 
    - 또, 예를들어 하나 큰 class 를 만들었던 commit하나 있다치면, 이걸 기능별로 2개로 나누고 commit로 이에따라 둘로 다시 쪼개던가 하더라.
    - 자잘한 수정이 많이 일어난 부분은 commit 서너개 다 합치거나..
- 근데 ... 말그대로 save point라면, 이걸 직접 control하는게 옳은가?  
    - git reset HEAD~1 하려던걸 git reset HEAD~12하면? 
