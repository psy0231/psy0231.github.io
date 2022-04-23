---
title: Python 1 - Intro 1
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-04-15 00:00:00 +0900
categories: [Grind, Python]
tags: [python]
---

## 1. python
- 올해 목표 중 하였고 늦기전에 후딱 하는걸로.
- 진행중인 다른 프로젝트에서   
주 언어이기도 하고 좀 관심이 가서.
- 쓰다 보니까 이 전 경험에 비춰봤을 때  
다른점들을 적는 경향이 있음.
- 아마... 항상 그렇듯 자세히 쓸 것같진않다.  
알고싶은걸 알게된것만큼 쓸듯하다.

## 2. 특징
- 고급언어, 인터프리터 방식 언어
  - 고급언어는 좀더 사람에게 친숙한
  - 인터프리터방식은 한줄한줄 해석실행.  
  일반적으로 컴파일하는 방식의  
  java, C#등이 반대포지션.  
- 얕은 식견으로는 코드가 다른 언어에 비해  
직관적이고 쉽다.
- c -> java -> C# -> python 순서로  
배울 기회가 있었는데  
배운 순서대로 쉬운느낌이었다. 
  - c
    ```c
    #include <stdio.h>
    int main(int argc, char** argv) {
        printf("Hello world");
    }
    ```
  - java
    ```java
    class Main {
      public static void main(String[] args) {
          System.out.println("Hello, world!");
      }
    } 
    ```
  - c#
    ```c#
    using System;

    class Program {
        static void Main(string[] args) {
            Console.WriteLine("Hello, world!");
        }
    }
    ```
    또는
    ```c#
    //C# 10.0 ~ 
    Console.WriteLine("Hello, world!");
    ```
  - python
    ```python
    print("Hello, world!")
    ```
  - 처음main까지 쓸게 많은건 생각보다 귀찮음.  
  namespace, class, main, import, using 등등  
  막상 ide도움없이 쓸때 한번씩 막힘..
  - 내가 실행할 내용 외에 주렁주렁 쓸게 적은게  
  인터프리터 방식이라 그렇다고. 얼추 본것같음.

- 라이브러리가 많다.  
는데 이건 사실 다른 언어도 비슷하지 않을까 싶다  

  다만, 언어가 쓰기 쉽다는 특성이랑 엮여서  
  복잡하게 다른걸로 이 작업을 하느니  
  파이선으로 호로록 하는게 많아지고  
  그러다보니 사용 빈도가 계속 불어나는것같다.
- 해서 생산성이 좋음.
- 근데 느리다함.  
물론 이게 보일정도로 써보진 않았음.  

## 3. PEP
- 여기 정보가 많다. 참고하자.
- https://peps.python.org/#
- 다음 몇 개만 보고 넘어간다.

### 3.1 PEP 1 – PEP Purpose and Guidelines
- What is a PEP?
  ```
  PEP stands for Python Enhancement Proposal.  
  A PEP is a design document  
  providing information to the Python community,  
  or describing a new feature  
  for Python or its processes or environment.  
  The PEP should provide a concise  
  technical specification  
  of the feature and a rationale for the feature.

  We intend PEPs to be the primary mechanisms  
  for proposing major new features,  
  for collecting community input on an issue,  
  and for documenting the design decisions  
  that have gone into Python.  
  The PEP author is responsible  
  for building consensus within the community  
  and documenting dissenting opinions.

  Because the PEPs are maintained as text files  
  in a versioned repository,  
  their revision history is the historical record  
  of the feature proposal.  
  This historical record is available  
  by the normal git commands  
  for retrieving older revisions,  
  and can also be browsed on  
  [GitHub](https://github.com/python/peps).
  ```
  -  Python 커뮤니티에 정보를 제공하거나  
  Python 또는 Python의 프로세스 또는  
  환경에 대한 새로운 기능을 설명하는  
  design document(? 설계문서?). 
  - 라는데 아무튼 정해진 규약이나,  
  그 외 기본이념 등 언어 전반에 걸친  
  내용을 정리해 좋은것. 

### 3.2 PEP 20 – The Zen of Python
- zen은 '선' 이라는데 뭔지 모르겠다.  
  ```
  마음을 가다듬고 정신을 통일하여  
  무아정적(無我靜寂)의 경지에 도달하는  
  정신 집중의 수행(修行) 방법.  
  명상과 정신 통일로써 번뇌를 끊고  
  진실의 법칙을 체득하는 일을 말한다.
  ```
  - 라고하는데? 일종의 기본 이념?  
  같은거라 생각하면 될듯.
  - 아래 19개가 그 내용.

1. Beautiful is better than ugly.
    - 못생긴것보다 이쁜게 좋다.
2. Explicit is better than implicit.
    - 암시적인것보다 명시적인게 좋다.
3. Simple is better than complex.
    - 복잡한것보다 간단한게 좋다.
4. Complex is better than complicated.
    - 복잡한것보다 복잡한게 좋다.
    - 개소리하나 싶었는데...  
      - Complex는 쉬운지 어려운지와는 상관없이  
      많은 요소로 이루어진 것(<-> simple)
    - Complicated는 어려움의 정도와 관계(<-> easy) 
    - 한 뭉탱이에 다 박아넣어 어렵게 하지말고  
    조각조각 많더라도 좀 쉽게하라는뜻인가?
    - 난해한것보다 복잡한게 좋다 이런건가? 
5. Flat is better than nested.
    - 중첩보다 수평적인게 좋다
    - nest가 쌓아올린 구조를 말할때도 쓰나봄.
6. Sparse is better than dense.
    - 빽빽한것보다 널널한게 좋다.
7. Readability counts.
    - 가독성은 중요하다.
8. Special cases aren't special enough to break the rules.
    - 특수한 경우는 규칙을 깰만큼 특수하지 않다.
9. Although practicality beats purity.
    - 뭐라는건지 모르겠음.
    - 순수함보다 실용성이란 뜻인가? 그 반댄가??
10. Errors should never pass silently.
    - 에러는 조용히 지나가서는 안된다.
11. Unless explicitly silenced.
    - 명시적으로 묵과하는게 아니라면.
    - 위랑 이어지는 내용?
12. In the face of ambiguity,  
refuse the temptation to guess.
    - 모호함에 직면했을 때, 추측하려는 유혹을 거부한다.
    - ambiguity는 애매한 표현을 말하는듯  
    직감이 아니라 증거에 기반하라는것같음.  
    pep목적도 그렇고.
13. There should be one--  
and preferably only one  
--obvious way to do it.
   - 무언가를 할 수 있는 방법은 하나만 있어야 한다. ??
14. Although that way may not be obvious  
at first unless you're Dutch.
    - 처음에는 그 방법이 확실치 않을 수 있다.
    - 위랑 이어지는듯 
    - 위꺼랑 합쳐서 

      당신이 네덜란드 사람이 아니라면  
      처음에는 그 방법이 분명하지 않을 수 있지만  
      하나의-그리고 바람직하게는 단 하나의-  
      분명한 방법이 있어야 합니다.
    - 네덜란드는 왜?
15. Now is better than never.
    - 안하는것보다 지금 하는게 더 좋다.
16. Although never is often better than *right* now.
    - 하는것보다 안하는것이 좋을지라도.
17. If the implementation is hard to explain,  
it's a bad idea.
    - 구현한것을 설명하기 힘들다면 나쁜 아이디어다.
18. If the implementation is easy to explain,  
it may be a good idea.
    - 구현한것을 설명하기 쉽다면 좋은 아이디어다.
19. Namespaces are one honking great idea  
-- let's do more of those!
    - Namespaces는 좋은 아이디어다. 더 써라.

- 근데 이 내용 그냥 보편적으로 적용해도 좋은 내용인것같다.
  - 진짜로 .. 코딩할 때 참고하면 좋겠다 싶었다.
- 아무튼 깔끔, 명확, 단순 이 주 키워드인것같다. 
- import this하면 이 내용이 나옴.
- 해석이 틀릴 수 있는건 한국인이기때문임.

## 참고
- [pythontutor](https://pythontutor.com/)