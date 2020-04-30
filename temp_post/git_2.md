---
title: 2.설치, 기본설정
date: 1111-11-11 11:11:11 +0900
categories: [Grind, Git]
tags: [git]
seo:
  date_modified: 1111-11-11 11:11:11 +0900
---

## 설치
- 난 윈도우만 쓰니까 일단.
- 근데 설치 과정을 쓰는게 의미가 있나 싶긴한데 ..
- ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/1.JPG?raw=true "1")
- ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/2.JPG?raw=true "2")
- ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/3.JPG?raw=true "3")
- ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/4.JPG?raw=true "4")
- ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/5.JPG?raw=true "5")
- ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/6.JPG?raw=true "6")
- ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/7.JPG?raw=true "7")
- ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/8.JPG?raw=true "8")
- ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/9.JPG?raw=true "9")
- ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/10.JPG?raw=true "10")
- ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/11.JPG?raw=true "11")
- ![image](https://github.com/psy0231/psy0231.github.io/blob/master/assets/img/git/git_2/git%20install/12.JPG?raw=true "12")
- ![image]()
- ![image]()
- ![image]()

## 초기 설정
- commit 할때는 어떤사람이 했는지 정보가 남는다. 그럼 그 정보가 어딘가는 있어야겠지.
  - git config --global user.name="name"
  - git config --global user.email="name"

## 통신방식
- https 또는 ssh이 둘이 있어서 이 둘만 써보고 넘어감.
- https

- ssh


## 참고
- [Git 최초 설정](https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%B5%9C%EC%B4%88-%EC%84%A4%EC%A0%95)
- [SSH 공개키 만들기](https://git-scm.com/book/ko/v2/Git-%EC%84%9C%EB%B2%84-SSH-%EA%B3%B5%EA%B0%9C%ED%82%A4-%EB%A7%8C%EB%93%A4%EA%B8%B0)
- [Why does GitHub recommend HTTPS over SSH?](https://stackoverflow.com/questions/11041729/why-does-github-recommend-https-over-ssh)
- []()  
-  Bla bla <sup id="a1">[1](#f1)</sup>
- <b id="f1">1</b> Footnote content here. [↩](#a1)


## ㅁㄴㅇㄹ

me@hovirw10 MINGW64 ~/Desktop
$ git --help
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone             Clone a repository into a new directory
   init              Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add               Add file contents to the index
   mv                Move or rename a file, a directory, or a symlink
   restore           Restore working tree files
   rm                Remove files from the working tree and from the index
   sparse-checkout   Initialize and modify the sparse-checkout

examine the history and state (see also: git help revisions)
   bisect            Use binary search to find the commit that introduced a bug
   diff              Show changes between commits, commit and working tree, etc
   grep              Print lines matching a pattern
   log               Show commit logs
   show              Show various types of objects
   status            Show the working tree status

grow, mark and tweak your common history
   branch            List, create, or delete branches
   commit            Record changes to the repository
   merge             Join two or more development histories together
   rebase            Reapply commits on top of another base tip
   reset             Reset current HEAD to the specified state
   switch            Switch branches
   tag               Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch             Download objects and refs from another repository
   pull              Fetch from and integrate with another repository or a local branch
   push              Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
See 'git help git' for an overview of the system.

me@hovirw10 MINGW64 ~/Desktop
$
