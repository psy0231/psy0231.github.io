---
title: Python 1 Intro
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-04-08 00:00:00 +0900
categories: [Grind, Python]
tags: [python]
---

## PEP 8 – Style Guide for Python Code
- 여긴 필요한것만 하고 넘어감.

### Introduction
- 규칙의 추가, 언어자체가 변함에 따라  
이 지침도 변할 수 있음. 
- 프로젝트에서 말하는 지침이 있으면  
그걸 먼저 적용함.
### A Foolish Consistency is the Hobgoblin of Little Minds
- 어리석은 일관성은 하남자 특이다.
- 코드는 쓰는것보다 읽는횟수가 더 많다.  
- 이 문서에서 중요하게 생각하는것은 코드의 일관성.  
궁극적인 목적은 PEP 20 - 7의 가독성.
- 이 문서를 무시해도 되는 특정 상황이 있는데 
  - 이 지침을 따르면 코드가 더 어려워질때
  - 다른 코드와의 일관성을 해칠때
  - 이전 가이드에 따라 적성되었거나,  
  권장하는 기능을 지원하지 않는  
  이전 버전과 호환을 유지해야하는 경우

### Code lay-out
- 들여쓰기 
  - 레벨당 4 공백.
    - 이게 얼마전에 본건데,  
    탭도 4공백, 스페이스 4번도 4공백인데  
    환경에 따라 둘이 달라지는 경우가 있다함.  
    그래서 4공백으로 박아넣은듯. 
  - 내용을 연결된 줄로 작성할 때 4 space는 선택사항이지만  
  연속된 줄을 알 수 있도록 들여써주어야 하며  
  내용으로 인한 들여쓰기 레벨보다는 더 들여써야함.
    ```python
    # Correct:
    # Aligned with opening delimiter.
    foo = long_function_name(var_one, var_two,
                            var_three, var_four)

    # Add 4 spaces (an extra level of indentation) to distinguish arguments from the rest.
    def long_function_name(
            var_one, var_two, var_three,
            var_four):
        print(var_one)

    # Hanging indents should add a level.
    foo = long_function_name(
        var_one, var_two,
        var_three, var_four)
    
    ```
    
    ```python
    # Wrong:

    # Arguments on first line forbidden when not using vertical alignment.
    foo = long_function_name(var_one, var_two,
        var_three, var_four)

    # Further indentation required as indentation is not distinguishable.
    def long_function_name(
        var_one, var_two, var_three,
        var_four):
        print(var_one)
    ```
  - if의 경우 "if ("로 인해 4space가 만들어지며  
  조건이 길어져 내려쓸 때 본문 블록과 겹쳐 애매해지는데  
  주석을 넣거나 더 들여쓴다.
    ```python
    # No extra indentation.
    # 추가 들여쓰기가 없다.
    if (this_is_one_thing and
        that_is_another_thing):
        do_something()

    # Add a comment, which will provide some distinction in editors
    # supporting syntax highlighting.
    # 문법 강조를 지원하는 에디터에서 어느 정도 구별이 되도록 코멘트를 넣는다.
    if (this_is_one_thing and
        that_is_another_thing):
        # Since both conditions are true, we can frobnicate.
        do_something()

    # Add some extra indentation on the conditional continuation line.
    # 조건문의 연속된 줄에 추가 들여쓰기를 한다.
    if (this_is_one_thing
          and that_is_another_thing):
        do_something()
    ```
  - 닫는기호 "]" , ")" , "}" 등은  
  첫번째 요소라인에 두거나 암튼 대충 두셈 
    ```python
    my_list = [
      1, 2, 3,
      4, 5, 6,
      ]
    result = some_function_that_takes_arguments(
        'a', 'b', 'c',
        'd', 'e', 'f',
        )
    my_list = [
        1, 2, 3,
        4, 5, 6,
    ]
    result = some_function_that_takes_arguments(
        'a', 'b', 'c',
        'd', 'e', 'f',
    )
    ```
    - 닫을 때 한줄 아래가 중점인가? 

- Tabs or Spaces
  - space를 기본으로 한다.
  - 2는 혼용가능이었던것같은데 3은 space만 되나봄.
  - 2도 space로 통일을 권장함.

- Maximum Line Length
  - 최대 79자로 한다.
  - 구조적 제약이 적은 독스트링 또는 주석등의  
  긴 텍스트 블록의 경우  
  줄 길이는 72자로 제한되어야 합니다.
  - 위에 쓴 암시적 줄 바꿈을 잘 쓰던가 "\"를 쓰던가.
    - 이 판단은 적절한거로 알아서.
- Should a line break before or after a binary operator?
  ```python
  # No: operators sit far away from their operands
  income = (gross_wages +
            taxable_interest +
            (dividends - qualified_dividends) -
            ira_deduction -
            student_loan_interest)

  # Yes: easy to match operators with operands
  income = (gross_wages
            + taxable_interest
            + (dividends - qualified_dividends)
            - ira_deduction
            - student_loan_interest)
  ```
  - 보통 위와 같이 썼는데 이게 가독성을 해침.  
  연산자가 오른쪽 끝에서 들쭉날쭉한게 이유인듯함.
  - 이 전에 쓰여진 코드와 일관성을 따르되  
  새로 쓴다면 이걸 권장함.

- Blank Lines
  - 최상위함수, 클래스 정의는 두 개 
  - 클래스 내의 메서드는 한 개 
  - 관련 기능의 그룹을 구분하기 위해  
  빈 줄을 드물게 쓸 수 있음.   
   (e.g. a set of dummy implementations)
  - 논리적 섹션을 나타내기 위해  
  함수 안에서 드물게 사용함
  - Python accepts the control-L  
  문단 뭔소린지 몰라 넘김.

- Source File Encoding 
  - 핵심 Python 배포판의 코드는 항상  
  UTF-8을 사용하고 인코딩 선언은 안함.
  - 표준 라이브러리에서 UTF-8이 아닌 인코딩은  
  테스트 목적으로만 사용.  
  - ASCII가 아닌 문자는 가능하면  
  장소와 사람 이름만 사용.  
  ASCII가 아닌 문자를 데이터로 사용하는 경우  
  z̯̯͡a̧͎̺l̡͓̫g̹̲o̡̼̘ 및 바이트 순서 표시와 같은  
  지저분한거 사용금지.
  - 표준 라이브러리의 모든 식별자는  
  ASCII전용 식별자를 사용하고  
  가능한한 영어를 사용한다.  

- Imports 
  - 한줄에 쓰기 금지
    ```python
    Yes:

    import os
    import sys
    No:

    import sys, os
    ```
  - 이건 가능 
    ```python 
    from subprocess import Popen, PIPE
    ```
  - 항상 파일 상단, 모듈주석 및 독스트링 바로 뒤,  
  모듈 전역 및 상수 앞에 배치.
  - 다음 순서로 그룹화해서 정리함 
    - 표준 lib import
    - 관련 third party import
    - 로컬 app/lib 특정 import
    - 위 각 그룹마다 빈 줄로 구분.
  - Absolute imports are recommended
    - 일반적으로 읽기 쉽다.
    - import 시스템이 잘못 구성되어 있어도  
    최소한 오류메시지라도 더 잘 제공함.
    - 단, 명료한 상대적 import는 절대적 import의  
    대안으로 허용될 수 있다. (절대경로 지정이  
    더 길어질 경우 등.)
    - 표준 라이브러리 코드는  
    복잡한 패키지 레이아웃을 피하고  
    Absolute imports 사용.

- Module level dunder names
  - 몰라 여긴.

### String Quotes
- "" 나 '' 나 상관없음
- 근데 문자열 중간에 포함할때  
안쓴 나머지 문자를 쓰는걸 권장.
  ```python
  a = "some sample text"
  b = "some 'sample' text"
  c = "some \"sample\" text"

  print(a)
  print(b)
  print(c)
  ```
### Whitespace in Expressions and Statements
- Pet Peeves(짜증나는거)
  - 쓸데없는 공백을 싫어함.
      - 괄호 안쪽
      - 쉼표 근처
      - 콜론, 세미콜론 근처
    ```python
    # Correct:
    spam(ham[1], {eggs: 2})
    # Wrong:
    spam( ham[ 1 ], { eggs: 2 } )
    
    # Correct:
    foo = (0,)
    # Wrong:
    bar = (0, )
    
    # Correct:
    if x == 4: print(x, y); x, y = y, x
    # Wrong:
    if x == 4 : print(x , y) ; x , y = y , x
    ```
    - 널널하게 쓰는게 좋담서...
  - 슬라이스에서 콜론은 이항 연산자처럼 작동하며  
  양쪽에 동일한 양을 쓴다. (가장 낮은 우선 순위의 연산자로 처리).  
  확장 슬라이스에서는 두 콜론에 동일한 간격을 쓴다.  
  예외: 슬라이스 매개변수가 생략되면 공백 생략.
    ```python
    # Correct:
    ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
    ham[lower:upper], ham[lower:upper:], ham[lower::step]
    ham[lower+offset : upper+offset]
    ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
    ham[lower + offset : upper + offset]
    # Wrong:
    ham[lower + offset:upper + offset]
    ham[1: 9], ham[1 :9], ham[1:9 :3]
    ham[lower : : upper]
    ham[ : upper]
    ```
  - 함수 / 인덱싱 / 슬라이싱과 괄호 사이
    ```python
    # Correct:
    spam(1)
    # Wrong:
    spam (1)

    # Correct:
    dct['key'] = lst[index]
    # Wrong:
    dct ['key'] = lst [index]
    ```
  - 할당 할 때 변수이름 자리맞춤
    ```python
    # Correct:
    x = 1
    y = 2
    long_variable = 3
    # Wrong:
    x             = 1
    y             = 2
    long_variable = 3
    ```
    - 이부분은 항상 고민이긴함

- Other Recommendations 
  - 뒤에 공백을 쓰지 않는다.  
  잘 보이지도 않고 안좋음.
  - 다음의 이항 연산자들은 항상 단일 공백을 쓴다.
    - 할당( = ), 
    - 증강 할당( += , -=, 등), 
    - 비교( == , < , > , != , <> , <= , >= ,  
    in, not in , is , is not ), 
    - 논리( and , or , not).
  - 우선 순위가 다른 연산자를 사용하는 경우  
  우선 순위가 가장 낮은 연산자 주위에  
  공백을 쓰면 좋음.  
  단, 두 개 이상의 공백을 사용하지 말고  
  이항 연산자의 양쪽에 항상 같은 양의 공백을 사용.
    ```python
    # Correct:
    i = i + 1
    submitted += 1
    x = x*2 - 1
    hypot2 = x*x + y*y
    c = (a+b) * (a-b)
    # Wrong:
    i=i+1
    submitted +=1
    x = x * 2 - 1
    hypot2 = x * x + y * y
    c = (a + b) * (a - b)
    ```
  - 여기 위로 자잘한게 더 있는데 복잡.

### When to use trailing commas
- 후행 쉼표는 한 요소의 튜플을 만들 때 필수  
그 외 일반적으로 선택적.  
명확성을 위해 후자를  
(기술적으로 중복되는) 괄호로 묶는 것이 좋음.
  ```python
  # Correct:
  FILES = ('setup.cfg',)
  # Wrong:
  FILES = 'setup.cfg',
  ```
- 후행 쉼표는 값 목록, 인수, 가져온 항목이  
추후 확장 가능성이 있는 경우 종종 유용.  
각각을 한 줄에 단독으로 넣고  
항상 후행 쉼표를 추가하고  
다음 줄에 닫는 괄호/대괄호/중괄호를 추가.  
그러나 닫는 구분 기호와 동일한 줄에  
후행 쉼표를 사용하는 것은 의미가 없다.  
단일 톤 튜플의 경우 제외.
  ```python
  # Correct:
  FILES = [
      'setup.cfg',
      'tox.ini',
      ]
  initialize(FILES,
            error=True,
            )
  # Wrong:
  FILES = ['setup.cfg', 'tox.ini',]
  initialize(FILES, error=True,)
  ```

### Comments
- 코드와 모순되는 주석은  
주석이 없는 것보다 나쁘다.  
코드가 변경될 때 항상  
주석을 최신 상태로 유지해야함.

- 주석은 완전한 문장으로 한다.  
소문자로 시작하는 식별자가 아닌 경우  
첫 번째 단어는 대문자로 한다.  
식별자의 대소문자는 변경하면 안됨.

- 블록 주석은 일반적으로 완전한 문장으로 구성된  
하나 이상의 단락으로 구성되며 각 문장끝은 마침표로.

- 여러 문장으로 된 주석에서는  
마지막 문장을 제외하고  
문장 끝 마침표 뒤에 두 개의 공백을 사용해야 합니다.

- 다른 사용자가 명확하고 쉽게 이해할 수 있어야 함.

- 비영어권 국가의 경우에도 애지간하면 영어로.

- Block Comments
- Inline Comments

- Documentation Strings
  - "docstrings"의 좋은 작성 규칙은 PEP 257로.
    - 모든 공개 모듈, 함수, 클래스, 메서드에 docstrings 작성.  
    비공개 메서드에는 선택이지만 메서드를 설명하는 주석은 필요.  
    이 주석은 def 줄 뒤에 씀 .
    - docstrings이 끝날때는  
    """가 단독으로 와야 한다.
      ```python
      """Return a foobang
      Optional plotz says to frobnicate the bizbaz first.
      """
      ```
    - 한 라인에 전부 쓰는 경우는 제외.
      ```python
      """Return an ex-parrot."""
      ```

### Naming Conventions
### Programming Recommendations