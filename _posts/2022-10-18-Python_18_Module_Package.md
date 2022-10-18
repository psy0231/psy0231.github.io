---
title: Python 18 - Module, Package
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-10-18 00:00:00 +0900
categories: [Grind, Python]
tags: [python, module, package]
---

## Module

- 보통 예시들을 보면  
인터프리터로 실행하는 경우가 많았음.  
다만, 지금까지 해왔던 방식이 있어서  
익숙한건 코드를 써서 하는쪽이고.  
처음엔 좀 신기했던점이  
입력은 한줄한줄인데  
이 전 입력을 기억하고있던거?
  
  아무튼 인터프리터로 실행을 했다면  
  당연하게도 종료하면 그 내용은 다 날아감.  

- 그래서 편집기로 내용을 준비해 실행하는것이 좋고  
  이 준비된것을 스크립트라고함.
  
  스크립트라고 하지만 일반적으로 보는 코드형태임.  
  아마 스크립트라고 하는건  
  그 코드형태의 파일 하나하나가  
  독자적으로 실행가능할 수 있기 때문인것같음.
  
- 이 스크립트가 길고 복잡해질수록  
쉽게 관리하고 싶어짐.  
또는 다른 프로그램에서 썼던  
function, class등 편리하게 재사용하고싶음.

- 이런 상황을 지원하기위해  
어떤 정의들을 파일에 넣고  
스크립트, 인터프리터에서  
사용할 수 있도록 만든것이 module.

- 필요한것 끼리 모아놓은 집합정도.  
함수, 변수, 클래스등을 모아놓은 파일.

- 확장자는 .py

- 지금까지 python을 쓴 방식을봤을 때  
module은 필요한것 끼리 모아놓은  
function, class등의 구분없는 집합.  
일단 내부 구성요소는 attr이라고 하겠음

- module은 독자적 namespace를 갖음.

### import

- 기본적으로 이 전까지 쓰던 그거임.
  
  ```python
  ## module.py
  import calendar
  import datetime
  
  print("module")
  
  modtemp = 1234
  
  def mtd():
      print("mtd")
  
  def prtcal():
      calendar.prmonth(datetime.datetime.today().year,
                      datetime.datetime.today().month)
  
  class cls():
      def __init__(self):
          pass
      def clsmtd(self):
          print("clsmtd")
  
  ```
  
  ```python
  ## main.py
  import module as md
  
  md.mtd()
  
  temp = md.cls()
  temp.clsmtd()
  
  md.prtcal()
  ```
  
- import할 때 as 붙여서 이름 바꿔쓸 수 있음.

- module에서 다른 module을 import할 수 있음.  
이 때 import module 이름이  
module 최상위 수준(함수 또는 클래스 외부)에 있는 경우  
module 전역 네임스페이스에 추가됨.
- `from module import prtcal` 이런식으로 하면  
module import를 선택적으로 할 수 있음.  
당연히 이 경우 import된것만 쓸 수 있음.  
반대로는 `import module` 또는  
`from module import *`

### Module Execution

- 위 경우 module.py를 실행하면  
아무 반응도 없음.

- module을 단독으로 실행할 수 있는데  
module 끝에 다음을 추가한다.
  ```python
  if __name__ == "__main__":
      pass
  else:
      pass
  ```
  `if`가 직접 실행부분.  
  `else`는 선택적.

- 위 예시에서
  
  ```python
  # module.py
  
  import calendar
  import datetime
  
  print("module")
  
  modtemp = 1234
  
  def mtd():
      print("mtd")
  
  def prtcal():
      calendar.prmonth(datetime.datetime.today().year,
                      datetime.datetime.today().month)
  
  class cls():
      def __init__(self):
          pass
      def clsmtd(self):
          print("clsmtd")
  
  if __name__ == "__main__":
      mtd()
  ```
  
  이런식. main.py는 그대로.
  
- 사용자 인터페이스를 제공하거나  
테스트용이라는데  
random이나 calendar 보면 얼추 그래보임.
- 생각해보면 .py로 된 파일은 뭐든 main이 될 수 있었다.  
`main()`같이 진입점이 있을때와 다른점인데  
이 구별이 없어서 저게 필요함.  
상단에 `print("module")`은  
외부에서 이 module을 쓰건 직접 실행하건  
무조건 실행하는데 이런 차이를 두고자 쓰는것같다.

### Module Path

- module를 추가할 떄 인터프리터는  
다음 규칙으로 module을 찾음.
  - 스크립트를 포함하는 디렉터리  
  또는 현재 디렉터리.
  - `PYTHONPATH`
  - 설치 종속 기본값.  
  `site module`에서 처리하는  
  `site-packages` 디렉토리를  
  포함하는 규칙에 따름.

- 그렇다고 한다.  
이게 문서에 저렇다고 하고  
설명이 조금씩 다르길래 자세한건 포기.

- 아무튼 `sys.path`가 그 결과인듯함.
  
  ```python
  import sys
  print(sys.path)
  ```
  
  ```python
  ['c:\\Users\\psy02\\Desktop\\python', 
  'C:\\Users\\psy02\\anaconda3\\python39.zip', 
  'C:\\Users\\psy02\\anaconda3\\DLLs', 
  'C:\\Users\\psy02\\anaconda3\\lib', 
  'C:\\Users\\psy02\\anaconda3', 
  'C:\\Users\\psy02\\anaconda3\\lib\\site-packages', 
  'C:\\Users\\psy02\\anaconda3\\lib\\site-packages\\win32', 
  'C:\\Users\\psy02\\anaconda3\\lib\\site-packages\\win32\\lib', 
  'C:\\Users\\psy02\\anaconda3\\lib\\site-packages\\Pythonwin']
  ```
  
  같은걸 다른데서 해보면,
  
  ```python
  ['f:\\W\\Python', 
  'C:\\Users\\psy02\\anaconda3\\python39.zip', 
  'C:\\Users\\psy02\\anaconda3\\DLLs', 
  'C:\\Users\\psy02\\anaconda3\\lib', 
  'C:\\Users\\psy02\\anaconda3', 
  'C:\\Users\\psy02\\anaconda3\\lib\\site-packages', 
  'C:\\Users\\psy02\\anaconda3\\lib\\site-packages\\win32', 
  'C:\\Users\\psy02\\anaconda3\\lib\\site-packages\\win32\\lib', 
  'C:\\Users\\psy02\\anaconda3\\lib\\site-packages\\Pythonwin']
  ```
  
  보면 project dir이 항상 위,  
  다음 python lib - 여긴 anaconda로  
  아무튼 PYTHONPATH였을것같고,  
  그 다음 site-packages가 붙은거보니  
  위 설명처럼 3개방식으로 하나봄.
  
- 또, `sys.path` return은 list임.  
수정이 가능한데  
추가할 다른 module이   
경로가 다르다면 여기 추가하면됨.

- 아니면 설치된위치-lib에 넣으면 되는듯.
- 스크립트를 포함하는 디렉터리는   
검색 경로의 처음으로,  
표준 라이브러리 경로의 앞에 온다.  
만약 두 이름이 같은 경우  
스크립트를 포함하는 디렉터리의 것이   
로드도되록 의도된 규칙인것같다.
- 근데 `sys.path.append()`맞나?  
위 규칙이면 내가 추가할거는  
`sys.path.insert()`아닌가?  
보통 `append()`로 하는듯?

### package, module 위치

- package나 module관련 정보는inspect로 본다.
- 기능이 많은것같은데 getfile만 일단.
  
  ```python
  import inspect
  import random
  import sys
  
  print(inspect.getfile(random.randint))
  print(inspect.getfile(mod2Mtd))
  print(inspect.getfile(mod3cls))
  
  ```


### module expose

- 보통의경우  
`from module import attr` 은  
특정 attr을 명시하기때문에  
조금 다르더라도  
`import module`,  
`from module import * `은 같다.

- module에 `__all__`이 있으면  
약간 달라지는데
  
  ```python
    # module.py
  
  import calendar
  import datetime
  import random
  
  __all__ = ['mtd', 'cls','modtemp']
  
  print("module")
  
  modtemp = 1234
  
  def mtd():
      print("mtd")
  
  def prtcal():
      calendar.prmonth(datetime.datetime.today().year,
                      datetime.datetime.today().month)
  
  class cls():
      def __init__(self):
          pass
      def clsmtd(self):
          print("clsmtd")
  
  if __name__ == "__main__":
      mtd()
  ```
  
  module에 `__all__`을 추가하고
  
  ```python
  from module import *
  from module import prtcal
  import module
  ```
  
  이 세 경우를 비교하면 
  
  ```python
  # main.py
  try:
      mtd()
  except Exception as e:
      print(e)
  
  try:
      cls().clsmtd()
  except Exception as e:
      print(e)
  
  try:
      print(modtemp)
  except Exception as e:
      print(e)
  
  try:
      prtcal()
  except Exception as e:
      print(e)
  ```
  
  - `from module import *`
      
      ```python
      module
      mtd
      clsmtd
      1234
      name 'prtcal' is not defined
      ```
      
  - `from module import prtcal`
      
      ```python
      name 'mtd' is not defined
      name 'cls' is not defined
      name 'modtemp' is not defined
          October 2022
      Mo Tu We Th Fr Sa Su
                      1  2
        3  4  5  6  7  8  9
      10 11 12 13 14 15 16
      17 18 19 20 21 22 23
      24 25 26 27 28 29 30
      31
      ```
      
  - `import module`
  이때는 호출이 달라짐.
      
      ```python
      import module
      
      try:
          module.mtd()
      except Exception as e:
          print(e)
      
      try:
          module.cls().clsmtd()
      except Exception as e:
          print(e)
      
      try:
          print(module.modtemp)
      except Exception as e:
          print(e)
      
      try:
          module.prtcal()
      except Exception as e:
          print(e)
      ```
      
      ```python
      module
      mtd
      clsmtd
      1234
          October 2022
      Mo Tu We Th Fr Sa Su
                      1  2
        3  4  5  6  7  8  9
      10 11 12 13 14 15 16
      17 18 19 20 21 22 23
      24 25 26 27 28 29 30
      31
      ```
      
- `__all__`은 `from module import * `할 때  
실제로 노출할 attr들을 명시해주는 역할을 한다.

- `__all__`이 없으면 전부 노출.

- `import module`, `from module import prtcal`은  
`__all__`과는 전혀 상관없어보인다.

---

## Package

- module를 dir구조로 관리.
- module 들의 집합.
- module은 기능을 모은 하나의 파일이면  
package는 module이 모인 폴더쯤.
- 설명보면 package도 독자적 namespace.  
따라서 대충 dir + namespace느낌
- 전체적으로 보면
  
  ```python
  |-   main.py
  |-   module.py
  |-   pak
        |-   mod2.py
        |-   mod3.py
  ```
  
  이런 구조같은.  
  사실 module, package들은  
  비슷한 방식의 구조화 방법은  
  흔히 볼 수 있긴함.
  
- 기본적으로
  
  ```python
  # mod2.py
  
  __all__ = ['mod2Fnc1']
  
  def mod2Fnc1():
      print("mod2Fnc 1")
  
  def mod2Fnc2():
      print("mod2Fnc 2")
  
  ```
  
  ```python
  # mod3.py
  
  class mod3cls():
      def __init__(self):
          pass
      def clsmtd(self):
          print("mod3cls.clsmtd")
  
  if __name__ == "__main__":
      print("mod3.py is being run directly")
  else:
      print("mod3.py is being imported into another module")
  ```
  
  ```python
  
  ## main.py
  import pak.mod3 as m3
  import pak.mod2 as m2
  m3.mod3cls().clsmtd()
  m2.mod2Fnc1()
  
  from pak.mod3 import mod3cls as m3cls
  from pak.mod2 import mod2Fnc1 as m2fnc
  m3cls().clsmtd()
  m2fnc()
  
  ```
  
  main에서 mod2, mod3을 불러오는 방법.
  
- import로 시작하면 module 이름까지.  
from으로 시작하면 import에  
attr또는 module이름까지 명시할 수 있음.

### package expose

- package에도 module에서 쓰는  
`__all__`을 쓸 수 있는데  
이 경우 `__init__.py`를  
해당 package 의 dir에 만들고  
그 안에 명시해야한다. 따라서
  
  ```python
  |-   main.py
  |-   module.py
  |-   pak
        |-   __init__.py
        |-   mod2.py
        |-   mod3.py
  ```
  
  ```python
  # __init__.py
  
  __all__ = ['mod2']
  ```
  
- package에서는
  - `from pak import mod3`
      
      ```python
      from pak import mod3
      
      try:
          mod3.mod3cls().clsmtd()
      except Exception as e:
          print(e)
      ```
      
      ```python
      mod3.py is being imported into another module
      mod3cls.clsmtd
      ```
      
      `__all__`에 상관 없음.
      
  - `from pak import *`
      
      ```python
      from pak import *
      
      try:
          mod3.mod3cls().clsmtd()
      except Exception as e:
          print(e)
      
      try:
          mod2.mod2Fnc2()
      except Exception as e:
          print(e)
      ```
      
      ```python
      name 'mod3' is not defined
      mod2Fnc 2
      ```
      
      `__all__`에 명시된  
      mod2만 가능
      
  - `from pak.mod2 import mod2Fnc1`
      
      ```python
      from pak.mod2 import mod2Fnc1
      from pak.mod2 import mod2Fnc2
      
      try:
          mod2Fnc1()
          mod2Fnc2()
      except Exception as e:
          print(e)
      ```
      
      명시된 module에서  
      명시된 attr만 사용가능한데  
      여기도package의 `__all__`은 상관없음
      
      또 module의 `__all__`도  
      상관없는것같은데 
      
      ```python
      from pak.mod2 import *
      
      try:
          mod2Fnc1()
      except Exception as e:
          print(e)
      
      try:
          mod2Fnc2()
      except Exception as e:
          print(e)
      
      ```
      
      ```python
      mod2Fnc 1
      name 'mod2Fnc2' is not defined
      ```
      
      이렇게 해야  
      module의 `__all__`이 적용됨
      
  - `import pak.mod2`
      
      ```python
      import pak.mod2
      
      try:
          pak.mod2.mod2Fnc1()
          pak.mod2.mod2Fnc2()
      except Exception as e:
          print(e)
      ```
      
      여기도 `__all__`은 상관없음
      
  - `import pak`
      
      ```python
      import pak
      
      try:
          print(dir(pak))
      except Exception as e:
          print(e)
      ```
      
      ```python
      ['__all__', '__builtins__', '__cached__', 
      '__doc__', '__file__', '__loader__', 
      '__name__', '__package__', '__path__', '__spec__']
      ```
      
      package에서 쓸 수 있는것만 보이는듯?  
      아무튼 하위 module은 둘 다 안보임
      

## Sum Up

- 내용이 생각보다 섞였는데  
아무튼 다시 위를 정리해보면
- module, package를 import할 때  
`from package.module import *`로 하는 경우  
`__all__`에 명시된 attr만 쓸 수 있다.
- 다른식의 import는 상관없다.
- 다르게보면 package, module를 만들 때  
`__all__`을 사용해 노출할 attr을 명시할 수 있다.
- `from item import *`에서  
item은 package 또는 module이 가능하고
  
  노출하는 대상은  
  item이 package면 package의 module을,  
  module이면 module의 attr을 대상으로 한다.
  

## pip install

- pypi에 많음.
- 잘 골라쓰자.
- pip install beautifulsoup4 이런거.
- pip list
- pip show [package]
- pip install --upgrade [package]
- pip uninstall [package]

## 내장함수

- 지금까지 쓰던 것 들 중 import 필요 없는것들.
- input, dir 등등.
- [Built-in Functions](https://docs.python.org/3/library/functions.html)

## 외장함수

- 지금까지 쓰던 것 들 중 import필요한 것들
- glop, os, time, datetime 등등.
- [Python Module Index](https://docs.python.org/3/py-modindex.html)