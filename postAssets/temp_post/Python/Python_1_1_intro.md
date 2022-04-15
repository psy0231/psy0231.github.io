---
title: Python 1 Intro 2
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
- python lib도 지금 영망이라함.
- 다만 이후 작성은 이 방법을 따르는기로.

- Overriding Principle
  - API에서 public으로 보이는 이름은  
  구현보다는 사용법을 반영한다.

- Descriptive: Naming Styles
  - 일반적으로 쓰이는거 다 상관없음
    - b(단일 소문자)
    - B(단일 대문자)
    - lowercase
    - lower_case_with_underscores
    - UPPERCASE
    - UPPER_CASE_WITH_UNDERSCORES
    - CapitalizedWords(CapWords / CamelCase / StudlyCaps)
      - 이 때 두문자어는 모든 문자를 대문자로 힌디.  
      ex) HTTPServerError와 HttpServerError중 전자.
    - mixedCase(CapitalizedWords와 다름)
    - Capitalized_Words_With_Underscores(혼종)
  - 이 뒤 내용은 ...아마 나중에 필요하면..

- Prescriptive: Naming Convention
  - Names to Avoid
    - 'l'(소문자 el), 'O'(대문자 oh), 'I'(대문자 eye)등  
    글꼴에 따라 1,0,l과 구별이 힘들어지는 문자를  
    단일 문자 변수 이름으로 하지 않는게 좋음.
  - ASCII Compatibility
    - 표준 라이브러리에서 사용되는 식별자는  
    PEP 3131에 따라 ASCII와 호환되어야 함.
  - Package and Module Names
    - 모듈은 모두 소문자로 된 짧은 이름으로한다.  
    가독성을 위해 모듈 이름에 밑줄을 쓸 수 있다.  
    - 패키지또한 소문자로 된 짧은 이름으로 한다.  
    여기는 밑줄 사용은 권장되지 않는다.  
    - C 또는 C++로 작성된 확장 모듈이  
    더 높은 수준(예: 더 많은 객체 지향) 인터페이스를 제공하는  
    파이썬 모듈을 동반하는 경우,  
    C/C++ 모듈은 선행 밑줄(예: _socket)을 갖는다.

  - Class Names
    - 일반적으로 CapWords를 따름.
    - 인터페이스가 문서화되고  
    주로 호출가능으로 사용되는 경우에는  
    함수와 같은 규칙이 사용될 수 있다.
    - 내장함수에는 별도 규칙을 적용. 
      - 단일 또는 두 단어.
      - CapWords는 예외 이름 및 기본 제공 상수에만 사용.
  - Type Variable Names
    - 자료형 변수(PEP484에 도입)의 이름은  
    일반적으로 CapWords를 사용  
    ex)T, AnyStr, Num.  
    - 공변 / 반공변 동작을 선언하는데 사용되는 변수에는  
    _co나 _contra접미사를 추가하는 것이 좋다.
      ```python
      from typing import TypeVar

      VT_co = TypeVar('VT_co', covariant=True)
      KT_contra = TypeVar('KT_contra', contravariant=True)
      ```
  - Exception Names
    - 예외는 클래스가 될 수 있기 때문에  
    클래스 규칙이 적용된다.  
    하지만, 접미사 "Error"를 붙인다.

  - Global Variable Names
    - 이 변수들은 하나의 모듈 내에서만 사용되기를 바란다는데  
    그렇게 쓰라는건지 그런 상황에 대한 모사인건지  
    전자로 생각함.
    - 함수랑 규칙이 거의 같음.

    - ``` from M import * ```을 통해 사용하도록 설계된 모듈은  
    ``` __all__ ```메커니즘을 사용하여  
    전역을 내보내는 것을 방지하거나  
    이러한 전역에 밑줄을 접두사를 쓴다.  
    (이런 글로벌이 "비공개 모듈"임을 나타내기 위해 쓸 수 있음)
    (which you might want to do to indicate  
    these globals are “module non-public”).

  - Function and Variable Names
    - 소문자로 씀
    - 가독성을 위해 밑줄로 단어 구분
    - 변수는 함수랑 동일한 규칙
    - mixCase는 이전 버전에서 이미 널리 쓰인 경우  
    호환성을 유지하기 위해 허용됩니다.

  - Function and Method Arguments
    - instance method  
      - 항상 첫 arg에 self를 사용.
    - class method
      - 항상 첫 arg에 cls 사용
    - 함수 인수의 이름이 예약어와 충돌하는 경우  
    일반적으로 약어나 맞춤법 손상을 사용하는 것보다  
    후행 밑줄을 추가하는 것이 좋음.  
    동의어를 사용이 더 좋고.

  - Method Names and Instance Variables
    - function과 같음.
    - 비공개 메서드 / 인스턴스 변수에 대해서만  
    선행 밑줄을 사용.
    - 서브클래스와의 이름 충돌을 피하려면  
    두 개의 선행 밑줄을 사용해  
    Python의 이름 맹글링 규칙을 호출.
    - Python은 이러한 이름을 클래스 이름으로 맹글링함.  
    Foo 클래스에 __a라는 속성이 있으면  
    Foo.__a에서 액세스할 수 없습니다.  
    (집요한 사용자는 Foo._Foo__a를 호출하여  
    액세스 권한을 얻을 수 있음.)  
    일반적으로 이중 선행 밑줄은  
    하위 클래스로 설계된 클래스의  
    속성과 이름 충돌을 피하기 위해서만 사용.
  - Constants
    - 상수는 일반적으로 모듈 수준에서 정의되며  
    단어를 구분하는 밑줄과 대문자로만 작성.  
    ex) MAX_OVERFLOW , TOTAL.

  - Designing for Inheritance
    - 항상 클래스의 메소드와 인스턴스 변수("속성")가  
    공개 또는 비공개여야 하는지 결정. 애매하면 비공개.  
    public 속성을 non-public으로 만드는 것보다 
    그 역이 더 쉬움.
    - 공개 속성은  
    이 전 버전과의 호환성을 유지하면서
    클래스의 관련 없는 클라이언트가  
    사용할 것으로 예상되는 속성.  
    비공개 속성은  
    제3자가 사용하도록 의도되지 않은 속성.  
    비공개 속성이 변경되거나  
    제거되지 않는다는 보장은 없다.
    - 파이썬에서 일반적으로 불필요한 양의 작업 없이  
    어떤 속성도 비공개가 아니기 때문에    
    "비공개(private)"라는 용어를 피하도록 한다.

    - 속성의 또 다른 범주는  
    "서브클래스 API"( "protected" 였던.)의 일부인 속성이다.  
    일부 클래스는 클래스 동작의 측면을  
    확장하거나 수정하기 위해 상속되도록 설계했다.  
    이러한 클래스를 디자인할 때  
    어떤 속성이 공개인지, 하위 클래스 API의 일부인지,  
    기본 클래스에서만 사용해야 하는지  
    명시적으로 결정한다.

    - 이를 염두에 두고 다음은 Pythonic 지침.
      - 공용 속성에는 선행 밑줄이 없어야 한다.
      - 공개 속성 이름이 예약된 키워드와 충돌하는 경우  
        
        속성 이름에 단일 후행 밑줄을 추가.  
        이것은 약어나 잘못된 맞춤법보다 좋다.  
        
        그러나 이 규칙에도 불구하고  
        'cls'는 클래스로 알려진 모든 변수나 인수,  
        특히 클래스 메서드의 첫 번째 인수에 대해  
        선호되는 철자이다.
        
        참고 1: 클래스 메서드에 대해서는  
        위의 인수 이름 권장 사항 참조.

      - 단순한 공개 데이터 속성의 경우  
        
        복잡한 접근자/변이자 메서드 없이  
        속성 이름만 노출하는 것이 가장 좋음.  
        
        단순한 데이터 속성이 기능적 동작을 확장해야 하는 경우  
        Python은 향후 향상을 위한 손쉬운 경로를 제공한다.  
        이 경우 속성을 사용하여  
        간단한 데이터 속성 액세스 구문 뒤에 기능 구현을 숨긴다.
        
        참고 1: 캐싱과 같은 부작용은 일반적으로 괜찮지만  
        기능적 동작 부작용이 없도록 유지하십시오.

        참고 2: 계산 비용이 많이 드는 작업의 경우  
        속성을 사용하지 마십시오.  
        속성 표기법은 호출자는 액세스가  
        상대적으로 저렴하다고 믿게 만된다.

      - 클래스가 서브클래싱되도록 되어 있고  
      서브클래스에서 사용하지 않으려는 속성이 있는 경우  
      
      두 개의 선행 밑줄로 이름을 지정하고  
      후행 밑줄은 사용하지 않는 것이 좋다.  
      이것은 클래스의 이름이 속성 이름으로 맹글링되는  
      파이썬의 이름 맹글링 알고리즘을 호출한다.  
      이렇게 하면 서브클래스에  
      같은 이름의 속성이 실수로 포함되는 경우  
      속성 이름 충돌을 방지하는 데 좋다.
      
      참고 1: 맹글링된 이름에는  
      단순 클래스 이름만 사용되므로  
      하위 클래스가 동일한 클래스 이름과 속성 이름을  
      모두 선택하면 여전히 이름 충돌이 발생할 수 있습니다.

      참고 2: 이름 맹글링은  
      디버깅 및 ```__getattr__()```과 같은  
      특정 용도를 덜 편리하게 만들 수 있습니다.  
      그러나 이름 맹글링 알고리즘은  
      문서화되어 있으며 수동으로 수행하기 쉽습니다.

      참고 3: 모든 사람이 이름 맹글링을 좋아하는 것은 아닙니다.  
      실수로 이름이 충돌하지 않도록 해야 하는 필요성과  
      고급 발신자가 사용할 수 있는 가능성 사이에서 균형을 유지하십시오.

- Public and Internal Interfaces
  - 이전 버전과의 호환성 보장은 공용 인터페이스에만 적용됨.  
  따라서 사용자는 공용 인터페이스와 내부 인터페이스를  
  명확하게 구분할 수 있어야 함.

  - 문서에서 일반적인 하위 호환성 보장에서 면제되는  
  임시 또는 내부 인터페이스로 명시적으로 선언하지 않는 한  
  문서화된 인터페이스는 공용으로 간주한다.  
  문서화되지 않은 모든 인터페이스는 내부로 간주한다.

  - 내성을 더 잘 지원하기 위해  
  모듈은 ```__all__``` 속성을 사용하여  
  공개 API에서 이름을 명시적으로 선언한다.  
  ```__all__```을 빈 목록으로 설정하면  
  모듈에 공개 API가 없음을 뜻한다.

  - ```__all__```이 적절하게 설정된 경우에도  
  내부 인터페이스(패키지, 모듈, 클래스, 함수, 속성 또는 기타 이름)는  
  여전히 단일 선행 밑줄 접두사로 시작한다.

  - 포함하는 네임스페이스(패키지, 모듈 또는 클래스)가  
  내부로 간주되는 경우 인터페이스도 내부로 간주한다.

  - 가져온 이름은 항상 구현 세부 사항으로 간주한다.  
  다른 모듈은 os.path 또는  
  하위 모듈의 기능을 노출하는 패키지의  
  ```__init__``` 모듈과 같이  
  포함하는 모듈 API의 명시적으로 문서화된 부분이 아닌 한  
  이러한 가져온 이름에 대한 간접 액세스에 의존해서는 안 된다.

### Programming Recommendations
- 코드는 Python의 다른 구현인 PyPy, Jython,  
IronPython, Cython, Psyco 등에  
불이익을 주지 않는 방식으로 작성한다.

  라이브러리의 성능에 민감한 부분에서는  
  ``` ''.join() ``` 형식을 대신 사용한다.  
  이렇게 하면 다양한 구현에서  
  선형 시간에 연결이 발생할것을 보장한다.

- None과 같은 싱글톤과의 비교는  
  항상 is 또는 is not으로 하며 절대 평등 연산자가 아닙니다.
  
  또한 "if x가 None이 아닌 경우"를 의미할 때  
  "if x"를 쓰는 것에 주의한다.  
  기본값이 None인 변수 또는 인수가  
  다른 값으로 설정되었는지 테스트할 때.  
  다른 값은 bool 컨텍스트에서  
  거짓일 수 있는 유형(예: 컨테이너)을 가질 수 있다!

- ```not ... is```가 아닌 ```is not``` 연산자를 사용합니다.  
두 식은 기능적으로 동일하지만 ```is not```이 읽기 쉽다.
  ```python
  # Correct:
  if foo is not None:
  # Wrong:
  if not foo is None:
  ```

- 풍부한 비교를 통해 순서 지정 작업을 구현할 때  
다른 코드에 의존해 특정 비교만 하지 않고  
```__eq__```, ```__ne__```, ```__lt__```,  
```__le__```, ```__gt__```, ```__ge__```의  
6가지 모두 구현하는 것이 가장 좋음.  

  관련된 노력을 최소화하기 위해  
  ```functools.total_ordering()``` 데코레이터는  
  누락된 비교 메서드를 생성하는 도구를 제공.

  "PEP 207"은 반사 규칙이 Python에 의해 가정됨을 나타냅니다.  
  따라서 인터프리터는  
  ```y > x```를 ```x < y```로,  
  ```y >= x```를 ```x <= y```로 바꾸고,  
  ```x == y```와 ```x != y```의 인수를 바꿀 수 있다.  
  ```sort()``` 및 ```min()``` 연산은 ```<``` 연산자를,  
  ```max()``` 함수는 ```>``` 연산자를 사용하도록 보장한다.  
  그러나 다른 컨텍스트에서 혼동이 발생하지 않도록  
  6가지 작업을 모두 구현하는 것이 가장 좋다.

- 람다 식을 식별자에 직접 바인딩하는 할당 문 대신  
항상 def 문을 사용.
  ```python
  # Correct:  
  def f(x): return 2*x
  # Wrong:
  f = lambda x: 2*x
  ```
  첫 번째 형식은 결과 함수 객체의 이름이  
  일반 ```<lambda>``` 대신 구체적으로 'f'라는 것을 의미한다.  
  이것은 일반적으로 트레이스백 및 문자열 표현에 더 유용하다.  
  할당 문을 사용하면 더 큰 식에 포함시킬 수 있는 
  명시적 정의 문 보다  
  람다 식이 제공할 수 있는 유일한 이점이 제거된다.

- BaseException이 아닌 Exception에서 예외를 파생한다. 
BaseException에서 직접 상속하는 것은  
거의 항상 잘못된 작업인 예외를 위해 예약된다.

  Exception 계층은 예외가 발생하는 위치가 아니라  
  예외를 포착하는 코드가  
  필요로 할 가능성이 높은 구분에 기반하여 설계한다.  
  "문제가 발생했습니다."라고만 말하지 않고  
  "무엇이 잘못되었습니까?"라는 질문에  
  프로그래밍 방식으로 답변하는 것을 목표로 한다 
  (내장 예외 계층에 대해 학습된 이 교훈의 예는 PEP 3151 참조).

  - 예외가 오류인 경우  
  예외 클래스에 접미사 "Error"를 추가하는데  
  클래스 명명 규칙을 적용한다.  
  비로컬 흐름 제어 또는 다른 형태의 신호 전달에 사용되는 
  비오류 예외에는 특별한 접미사가 필요없다.

- exception chaining을 적절하게 사용한다.  
```Rise X from Y```는 원래 트레이스백을 잃지 않고  
명시적인 대체를 나타낼 수 있다.
내부 예외를 의도적으로 교체할 때(```raise X from None``` 사용)  
관련 세부 정보가 새 예외로 전송되는지 확인한다. 
(예: KeyError를 AttributeError로 변환할 때  
속성 이름을 유지하거나 원래 예외의 텍스트를 새 예외 메시지에 포함).

- exception catching때는 ```exception:```절을  
그대로 쓰는것보다 특정 예외를 언급
  ```python
  try:
    import platform_specific_module
  except ImportError:
      platform_specific_module = None
  ```
  ```exception:``` 절은 SystemExit 및 KeyboardInterrupt 예외를 포착하여  
  Control-C로 프로그램을 중단하기 어렵게 만들고  
  다른 문제를 위장할 수 있습니다.  
  프로그램 오류를 알리는 모든 예외를 포착하려면  
  ```except Exception:```을 사용. 
  (bareexcept는 ```except BaseException:```과 동일).

  경험상 exception은 다음 두 가지로 제한하는 것이 좋다.
    - 예외 핸들러가 트레이스백을 출력하거나 기록할 경우  
    
      적어도 사용자는 오류가 발생했음을 알 수 있음.
    
    - 코드에서 일부 정리 작업을 수행해야 하지만  
    예외가 ```raise```으로 위쪽으로 전파되도록 하는 경우.  
    
      ```try...finally```가 이 경우를 처리하는 더 좋은 방법일 수 있다.

- 운영 체제 오류를 탐지할 때는  
"errno" 값의 내성보다  
Python 3.3에 도입된 명시적 예외 계층을 선호한다.
- 또한 모든 try/except 절에 대해  
try 절을 필요한 최소 코드 양으로 제한합니다.  
이것은 마스킹 버그를 방지한다.
  ```python
  # Correct:
  try:
      value = collection[key]
  except KeyError:
      return key_not_found(key)
  else:
      return handle_value(value)

  # Wrong:
  try:
      # Too broad!
      return handle_value(collection[key])
  except KeyError:
      # Will also catch KeyError raised by handle_value()
      return key_not_found(key)
  ```
- 리소스가 코드의 특정 섹션에 로컬인 경우  
사용 후 신속하고 안정적으로 정리되도록 with 문을 사용한다.  
try/finally 문도 가능.
- 컨텍스트 관리자는 리소스 획득 및 해제 이외의 작업을 수행할 때마다  
별도의 함수 또는 메서드를 통해 호출해야 한다.
  ```python
  # Correct:
  with conn.begin_transaction():
      do_stuff_in_transaction(conn)
  # Wrong:
  with conn:
      do_stuff_in_transaction(conn)
  ```
  후자의 예에서는 ```__enter__``` 및 ```_exit__``` 메서드가  
  트랜잭션 후 연결을 닫는 것 이외의  
  작업을 수행한다는 것을 나타내는 정보가 없다.  
  이 경우에는 명시적인 것이 중요합니다.

- return 문에서 일관성을 유지해야 한다.  
함수의 모든 return 문은 표현식을 반환해야 하거나  
아무 것도 반환하지 않아야 한다.  
반환 문이 표현식을 반환하는 경우  
값이 반환되지 않는 모든 반환 문은  
이를 명시적으로 ```return None```으로 명시해야 하며  
명시적 반환 문이 함수 끝에 있어야 한다(도달할 수 있는 경우):
  ```python
  # Correct:
  def foo(x):
      if x >= 0:
          return math.sqrt(x)
      else:
          return None
  def bar(x):
      if x < 0:
          return None
      return math.sqrt(x)
  
  # Wrong:
  def foo(x):
      if x >= 0:
          return math.sqrt(x)

  def bar(x):
      if x < 0:
          return
      return math.sqrt(x)
  ```

- 문자열 슬라이싱 대신 ```''.startswith()``` 및  
```''.endswith()```를 사용하여  
접두사 또는 접미사를 확인한다.
startswith() 및 endwith()는 더 깨끗하고 오류가 덜 발생.
  ```python
  # Correct:
  if foo.startswith('bar'):

  # Wrong:
  if foo[:3] == 'bar':
  ```
- 객체 유형 비교는 유형을 직접 비교하는 대신  
항상 isinstance()를 사용한다.
  ```python
  # Correct:
  if isinstance(obj, int):
    
  # Wrong:
  if type(obj) is type(1):
  ```
- 시퀀스(문자열, 목록, 튜플)의 경우  
빈 시퀀스가 거짓이라는 사실을 사용한다.
  ```python
  # Correct:
  if not seq:
  if seq:
    
  # Wrong:
  if len(seq):
  if not len(seq):
  ```
- 상당한 후행 공백에 의존하는 문자열 리터럴을 작성하지 않는다.  
이러한 후행 공백은 시각적으로 구별할 수 없으며  
일부 편집자(또는 최근에는 reindent.py)가 이를 자른다.
- "=="를 사용해 부울 값을 True 또는 False와 비교하지 않는다.
  ```python
  # Correct:
  if greeting:

  # Wrong:
  if greeting == True:
  ```
  
  Worse:
  
  ```ptyhon
  # Wrong:
  if greeting is True:  
  ```

- ```try...finally```의 finally 제품군 내에서  
```return/break/continue```흐름 제어 문을  
사용하는 것은 권장하지 않음.  
이는 그러한 명령문이 finally 제품군을 통해  
전파되는 활성 예외를 암시적으로 취소하기 때문.
  ```python
  # Wrong:
  def foo():
      try:
          1 / 0
      finally:
          return 42
  ```

- Function Annotations
  - PEP 484의 승인으로  
  함수 주석에 대한 스타일 규칙이 변경됨.

  - 함수 주석은 PEP 484 구문을 사용한다 

  이전 섹션에 주석에 대한 몇 가지 형식 권장 사항이 있음.
  
  - 이 PEP에서 이전에 권장되었던  
  주석 스타일에 대한 실험은 더 이상 권장하지 않는다.

  - 그러나 stdlib 외부에서는 이제 PEP 484 규칙 내에서 실험이 권장됩니다.  
  예를 들어, 대규모 타사 라이브러리 또는  
  애플리케이션을 PEP 484 스타일 유형 주석으로 마크업하고,  
  해당 주석을 추가하는 것이 얼마나 쉬운지 검토하고,  
  주석의 존재가 코드 이해도를 높이는지 관찰합니다.
  - Python 표준 라이브러리는 이러한 주석을 채택하는 데 있어  
  보수적이어야 하지만 새 코드와 대규모 리팩토링에 사용할 수 있습니다.
  - 함수 주석을 다르게 사용하려는 코드의 경우  
  다음 형식의 주석을 넣는 것이 좋습니다.
    ```
    # type: ignore
    ```
    파일 상단 근처에 위치하고  
    유형 검사기가 모든 주석을 무시하도록 지시합니다.  
    (유형 검사기의 불만 사항을 비활성화하는 보다  
    세분화된 방법은 PEP 484에서 찾을 수 있습니다.)

  - 린터와 마찬가지로 유형 검사기는  
  선택 사항이며 별도의 도구입니다.  
  기본적으로 Python 인터프리터는  
  유형 검사로 인해 메시지를 발행해서는 안 되며  
  주석을 기반으로 동작을 변경해서는 안 됩니다.

  - 유형 검사기를 사용하고 싶지 않은 사용자는 무시해도 됩니다.  
  그러나 타사 라이브러리 패키지 사용자는  
  해당 패키지에 대해 유형 검사기를 실행할 수 있습니다.  
  이를 위해 PEP 484는 stub 파일의 사용을 권장합니다.  
  해당 .py 파일보다 유형 검사기가 읽는 .pyi 파일입니다.  
  stub 파일은 라이브러리와 함께 배포되거나  
  typeshed repo를 통해 별도로(라이브러리 작성자의 허가 하에)  
  배포될 수 있습니다.
- Variable Annotations
  - PEP 526은 변수 주석을 도입했습니다.  
  이에 대한 스타일 권장 사항은  
  위에서 설명한 함수 주석의 권장 사항과 유사합니다.
  - 모듈 수준 변수, 클래스 및 인스턴스 변수,  
  지역 변수에 대한 주석은 콜론 뒤에 한 칸 띄워야 합니다.
  - 콜론 앞에 공백이 없어야 합니다.
  - 할당에 오른쪽이 있는 경우  
  등호는 양쪽에 정확히 하나의 공백이 있어야 합니다.
    ```python
    # Correct:

    code: int

    class Point:
        coords: Tuple[int, int]
        label: str = '<unknown>'
    # Wrong:

    code:int  # No space after colon
    code : int  # Space before colon

    class Test:
        result: int=0  # No spaces around equality sign
    ```
  - PEP 526이 Python 3.6에 대해 허용되지만,  
  모든 Python 버전에서 스텁 파일에 대한 기본 구문은  
  변수 주석 구문입니다(자세한 내용은 PEP 484 참조).