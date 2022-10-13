---
title: Python 17 - Exception
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-10-13 12:00:00 +0900
categories: [Grind, Python]
tags: [python, excetion, try, except, else, finally, raise, with]
---

## exception

- 뭐든 적당히 하다보면 보이는..
- 아무튼 예외처리
- 주요 키워드는 `try`, `except`,  
`else`, `finally` 가 있음
- 추가로 `raise`, `with`까지.
- 아무튼 기본 형식으로
    
    ```python
    try:
        ''' some statement'''
    except condition:
        ''' statement exception '''
    except condition`:
        ''' statement exception '''
    except condition``:
        ''' statement exception '''
    else:
        ''' other exception ''' 
    finally:
    		''' some statement '''
    ```
    

---

## try

- `try` ~ `except`사이 문장들 실행
- exception이 없으면 `except`를 건너뜀
- exception이 있으면 그 블록의  
뒤 코드는 건너뛰고  
조건에 따라 `except` 실행

---

## except

- `try`에서 exception이 있고,  
해당 유형이 명명되어 있으면  
해당 블록 실행
- `except`만 쓰면 exception은 잡아주는데  
구체적인 상황을 보려면  
작업이 더 필요함 예를들어,
  
  ```python
  import sys 
  try:
      ''' some statement'''
      print(1)
      1/0
  except :
      print(sys.exc_info())
      ''' statement exception '''
      print(2)
  else:
      ''' other exception '''
      print(3)
  finally:
      print(4)
  ```
  
  ```python
  1
  (<class 'ZeroDivisionError'>, ZeroDivisionError('division by zero'), <traceback object at 0x0000013D4E508CC0>)
  2
  4
  ```
  
- 반대로, exception을 지정할 수 있고  
선택적으로 alias를 줄 수도 있음.
    
    ```python
    ~
    except Exception as e:
        print(e)
        print(e.with_traceback)
        ''' statement exception '''
        print(2)
    ~
    ```
    
    `except`만 쓸때랑 차이는  
    쓰기좀더 편하다는거?
    
- exception condition에 따라  
처음 예시처럼 여러개가 올 수 있는데  
실행은 하나만 됨  
또는 조건을 묶어서 튜플로 쓸 수 있음
  
  ```python
  ~
  except (FileExistsError, InterruptedError) as e:
      print(e)
      print(e.with_traceback)
  ~
  ```
    
- 명시된 `except`에 일치하는 exception이 아니면  
외부 try로 전달됨.

---

## else

- `else`는 선택적으로 쓸 수 있는데  
항상 `except`뒤에 와야하며  
try에서 exception이 없는 경우 실행.
- 아래 case 1, case 2의 출력을 비교해보면
  
  ```python
  def mtd(num1, num2):
      res = num1 / num2
      return res
  
  try:
      print(mtd(10, "asdf"))  # case 1
    # print(mtd(10,1))        # case 2
  except ZeroDivisionError:
      print("Error: Cannot divide by zero")
  except TypeError as err:
      print("Error: Invalid type : {0}".format(err.args))
  except:
      print("Error: Unknown error")
  else:
      print("No error")
  finally:
      print("Executed")
  ```
  
  ```python
  # case 1
  Error: Invalid type : ("unsupported operand type(s) for /: 'int' and 'str'",)
  Executed
  
  # case 2
  10.0
  No error
  Executed
  
  ```
    
  정상적인 경우 처리를 위해?
    
- 또 `try`에 코드를 추가하는 것보다  
`else` 의 사용을 권장하는듯한데,  
 except에 명시하지 않은 예외를  
우연히 잡게 되는 것을 방지하기 위해서.
  
  라고 하는거 보면 `try` 안에는  
  exception나올것만 하라는것같음  
  `except`를 명확하게 하라는 아무튼 그럼.
    

---

## finally

- `try`에서 exception에 상관없이 실행되는구간.
- `try`에서 발생한 exception이 `except`에서  
처리되지 않으면 `finally`가 실행되고  
exception다시 발생.
  
  ```python
  try:
      print('start try')
      10/'asdf'
  except ZeroDivisionError:
      print("Error")
  else:
      print("No error")
  finally:
      print("Executed")
  ```
  
  ```python
  start try
  Executed
  Traceback (most recent call last):
    File "F:\W\Python\test.py", line 31, in <module>
      10/'asdf'
  TypeError: unsupported operand type(s) for /: 'int' and 'str'
  ```
  
  `except`에 명시되지 않은 exception은  
  `finally`이후 발생.
    
- `except`, `else`에서 exception이 발생해도  
`finally`를 실행하고 다시 exception발생.
  
  ```python
  try:
      print('start try')
      10/3
  except ZeroDivisionError:
      print("Error")
  else:
      10/'a'
      print("No error")
  finally:
      print("Executed")
  ```
  
  ```python
  start try
  Executed
  Traceback (most recent call last):
    File "F:\W\Python\test.py", line 35, in <module>
      10/'a'
  TypeError: unsupported operand type(s) for /: 'int' and 'str'
  ```
  
  `else`에 exception이 있어도  
  `finally`이후 exception 발생.
    
- `finally`에서 `break`, `continue`를 실행하면  
return exception이 발생하지 않음.
- `try` 에서 `break`, `continue` , `return` 을 만나면,  
`finally` 는 `break`, `continue` ,`return` 을  
실행하기 직전에 실행.
- `finally` 절에 return 이 포함되면,  
reutrn값은 `try` 의 return 값이 아니라,  
`finally` 의 return 이 주는 값이 된다.
  
  ```python
  def divide(x, y):
      try:
          result = x / y
      except ZeroDivisionError:
          print("division by zero!")
      else:
          print("result is", result)
      finally:
          print("executing finally clause")
  
  divide(2, 1)
  divide(2, 0)
  divide("2", "1")
  ```
  
  ```python
  result is 2.0
  executing finally clause
  division by zero!
  executing finally clause
  executing finally clause
  Traceback (most recent call last):
    File "F:\W\Python\test.py", line 56, in <module>
      divide("2", "1")
    File "F:\W\Python\test.py", line 42, in divide
      result = x / y
  TypeError: unsupported operand type(s) for /: 'str' and 'str'
  ```
    
- 아무튼 어떤 경우던간에 `finally`가  
***항상***, ***마지막***에 실행된다는게 주요내용.
- `try`에서 외부 resource를 사용할 때,  
exception상황에 관계없이  
resource를 반납하는데 유용함.  
파일 네트워크 등등.

---

## raise

- 강제로 exception을 발생시킴

  ```python
  raise TypeError
  ```

  ```python
  Traceback (most recent call last):
    File "F:\W\Python\test.py", line 40, in <module>
      raise TypeError
  TypeError
  ```

---

## custom exception

- class형태로 정의해 사용할 수 있음
- 보통 Error로 끝남.
- 보통 Exception을 상속.
  
  ```python
  class MyException(Exception):
      def __init__(self, value):
          self.value = value
      def __str__(self):
          return repr(self.value)
  
  try :
      for i in range(10):
          if i == 5:
              raise MyException(i)
          print(i)
  except MyException as e:
      print("MyException:", e.value)
  ```
  
  ```python
  0
  1
  2
  3
  4
  MyException: 5
  ```
    
- Exception을 상속하는게 아니면
  
  ```python
  def f():
      print('in f')
  
  try:
      raise f()
  except Exception as e:
      print(e)
  else:
      print('no err')
  
  print('after try')
  ```
  
  ```python
  in f
  exceptions must derive from BaseException
  after try
  ```
  
  실행은 어찌어찌되는것같은데  
  BaseException을 상속하라함  
  BaseException은 Exception바로 상위라그런듯?
  

---

## with

- 일부(?) class는 객체가 필요 없을 때  
스스로 resource를 정리함.
- 객체사용의 성공여부는 상관없음.
- `finally`가 항상 붙어있는형태.
- 예를들어
  
  ```python
  f = open("text.txt", "r")
  print(f.closed)
  try:
      firstline = f.readline()
      print(firstline)
  except Exception as e:
      print(e)
      print(f.closed)
  
  print(f.closed)
  f.close()
  print(f.closed)
  ```
  
  ```python
  False
  'cp949' codec can't decode byte 0xed in position 0: illegal multibyte sequence
  False
  False
  True
  
  # text.txt
  # 한글
  ```
  
  text.txt파일에 한글이 써져있어서 Exception인 상황.  
  그래도 f.close하기 전까지 file은 열려있다.  
  이런 경우를 위함.
    
- `with`은 사용 후 resource가  
정리되도록 보장함.
  
  ```python
  with open("text.txt", "r", encoding='utf-8') as f:
      for line in f:
          print(line)
  ```
  
  `with`블록 이후 어떤 경우에도 파일은 닫힘.
    
- 여기저기 대충 요약해보면
  
  ```python
  with EXPR as VAR:
      BLOCK
  ```
  
  이게 아래와 같음.
  
  ```python
  VAR = EXPR
  VAR.__enter__()
  try:
      BLOCK
  finally:
      VAR.__exit__()
  ```
  
  근데 `except`가 없음.  
  exception은 따로처리해야함.
    
- 그래서 `with`를 사용가능하게  
class를 만들 수 있는데 예를들어
  
  ```python
  class CustomClass:
      def __init__(self, args):
          self.args = args
  
      def __enter__(self):
          # must return resource
          return self
  
      def use(self):
          print(self.args)
  
      def __exit__(self, type, value, traceback):
          self.arg = None
  
  with CustomClass("test") as cc:
      cc.use()
  ```
  
  이거 알아서 잘 써먹으면됨.
    
- class까지 귀찮으면
  
  ```python
  from contextlib import contextmanager
  
  @contextmanager
  def use(args):
      data = args
      yield data
      data = None
  
  with use('text') as u:
      print(u)
  ```
  
  ㅇㅇ decorator.  
  위랑 같은 표현임.
    
- 이부분은 더 설명이 많음.  
필요하면 나중에. 

---

## **Exception hierarchy**

```python
BaseException
 +-- SystemExit
 +-- KeyboardInterrupt
 +-- GeneratorExit
 +-- Exception
      +-- StopIteration
      +-- StopAsyncIteration
      +-- ArithmeticError
      |    +-- FloatingPointError
      |    +-- OverflowError
      |    +-- ZeroDivisionError
      +-- AssertionError
      +-- AttributeError
      +-- BufferError
      +-- EOFError
      +-- ImportError
      |    +-- ModuleNotFoundError
      +-- LookupError
      |    +-- IndexError
      |    +-- KeyError
      +-- MemoryError
      +-- NameError
      |    +-- UnboundLocalError
      +-- OSError
      |    +-- BlockingIOError
      |    +-- ChildProcessError
      |    +-- ConnectionError
      |    |    +-- BrokenPipeError
      |    |    +-- ConnectionAbortedError
      |    |    +-- ConnectionRefusedError
      |    |    +-- ConnectionResetError
      |    +-- FileExistsError
      |    +-- FileNotFoundError
      |    +-- InterruptedError
      |    +-- IsADirectoryError
      |    +-- NotADirectoryError
      |    +-- PermissionError
      |    +-- ProcessLookupError
      |    +-- TimeoutError
      +-- ReferenceError
      +-- RuntimeError
      |    +-- NotImplementedError
      |    +-- RecursionError
      +-- SyntaxError
      |    +-- IndentationError
      |         +-- TabError
      +-- SystemError
      +-- TypeError
      +-- ValueError
      |    +-- UnicodeError
      |         +-- UnicodeDecodeError
      |         +-- UnicodeEncodeError
      |         +-- UnicodeTranslateError
      +-- Warning
           +-- DeprecationWarning
           +-- PendingDeprecationWarning
           +-- RuntimeWarning
           +-- SyntaxWarning
           +-- UserWarning
           +-- FutureWarning
           +-- ImportWarning
           +-- UnicodeWarning
           +-- BytesWarning
           +-- EncodingWarning
           +-- ResourceWarning
```