---
title: Python 3 - Variable & Data Type 1
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-04-23 00:00:00 +0900
categories: [Grind, Python]
tags: [python, variable, data type]
---

## 변수
- 값을 저장하는 공간.
- 다른 언어에 비해 역시 편하다.  
예를들어
  - c
      
      ```c
      int a = 10;
      char * str = "test";
      //이건 심지어 배열로 쓰는등 아무튼 그럼.
      ```
      
  - c#
      
      ```csharp
      int a = 10;
      string str = "c#";
      ```
      
  - python
      
      ```python
      a = 10
      str = "python"
      ```
      

### 순수 객체 지향

- 위 예시에서 차이점을 보면
자료형을 명시해주지 않는다. 따라서
  
  ```
  a = 10
  a = "python"
  ```
  이게 가능해진다.
  
- 이유는 python은 모든것이 객체로 처리되기 때문.  
이런 특성을 순수 객체지향이라 한다.
- python은 변수 자체에 값이 할당되는것이 아니라  
값이 저장된 객체를 가리키기 때문이다.  
내부적으로 c에서 포인터같은 짓을 하는것이다.
- 모든것을 object로 취급한다는것과  
class, method또한   
object로 취급할 수 있다는것은  
다음도 가능하게함.
  
  ```python
  def func():
    return "Hello"

  a = func
  
  print(a())
  ```
  그거 비슷하게 보이는거 맞음.  
  C# 할 떄 봤던 delegate.  
  delegate도 func pointer랑 엮여서 나왔고  
  여기도 변수가 pointer처럼 동작하니 이런것같음.
  
## python datatype
- numeric(integer, float, complex)
- string
- boolean
- NoneType?
- ex)
  
  ```python
  print(5)
  print(-10)
  
  a = 0
  b = "0"
  c = 3.15
  d = True
  e = False
  f = None
  g = 99999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999
  h = g*g*g*g
  
  print(type(a))
  print(type(b))
  print(type(c))
  print(type(d))
  print(type(e))
  print(type(f))
  print(type(g))
  print(type(h))
  
  ```
  
  ```
  5
  -10
  <class 'int'>
  <class 'str'>
  <class 'float'>
  <class 'bool'>
  <class 'bool'>
  <class 'NoneType'>
  <class 'int'>
  <class 'int'>
  ```

## Numeric
- int, float가 끝인가?
했는데 complex등장.
- g,h 를 보면 숫자 크기는 중요하지 않은것같음.
  - 이거 때문에 큰수 계산이 용이한듯.
  - 다른 언어에서는 
  biginteger를 써야함.
- ex)
  ```python
  var1 = 1
  print(var1)
  print(type(var1))
  
  var2 = 1.1e2
  print(var2)
  print(type(var2))
  
  var3 = 1 + 4j
  print(var3)
  print(var3.real)
  print(var3.imag)
  print(type(var3))
  ```
  
  ```
  1
  <class 'int'>  
  110.0
  <class 'float'>
  (1+4j)
  1.0
  4.0
  <class 'complex'>
  ```
  
  - 지수표현도되고, 복소수도 j나i써서 쓰면되고.

## Numeric - operator

- 대충 비슷. 특이한게 좀 섞여있음
  
  ```python
  print(10 + 3)
  print(10 - 3)
  print(10 * 3)
  print(10 / 3)
  print(10 % 3)
  print(10 ** 3)
  print(10 // 3)
  print()
  
  print(10 > 3)
  print(10 < 3)
  print(10 <= 3)
  print(10 >= 3)
  print(10 == 3)
  print(10 != 3)
  print()
  
  print(not(10 > 3))
  print((10 > 3) and (10 < 3))
  print((10 > 3) & (10 < 3))
  print((10 > 3) or (10 < 3))
  print((10 > 3) | (10 < 3))
  print()
  
  print(10 > 3 > 1)
  print(1 > 10 > 3)
  print()
  
  a = 10
  b = 2
  a += b
  print(a)
  a -= b
  print(a)
  a *= b
  print(a)
  a /= b
  print(a)
  a %= b
  print(a)
  a **= b
  print(a)
  a //= b
  print(a)
  print()
  
  ```
  
  ```
  13
  7
  30
  3.3333333333333335
  1
  1000
  3
  
  True
  False
  False
  True
  False
  True
  
  False
  False
  False
  True
  True
  
  True
  False
  
  12
  10
  20
  10.0
  0.0
  0.0
  0.0
  ```
  
  - ```//``` : division(int)
  - ```**``` : power

## Numeric - 숫자처리

- 기본적인거(내장)랑 math

```python
print(abs(-1))
print(pow(-1,2))
print(max(1,2))
print(min(1,2))
print(round(3.1415926,2))
print(round(3.1415926,3))
print()

from math import *
print(floor(3.1415926))
print(ceil(3.1415926))
print(sqrt(9))
print(pi)
print(e)
print(log(10))
print(log10(10))
print(log2(10))

```

```
1
1
2
1
3.14
3.142

3
4
3.0
3.141592653589793
2.718281828459045
2.302585092994046
1.0
3.321928094887362
```

### Random

- 이래저래 자주 쓰이길래.
- random()
  - 0.0부터 1.0미만 숫자.
- randrange(a,b)
  - a부터 b미만의 숫자
- randint(a,b)
  - a부터 b이하의 숫자

```python
from random import *

print(random())
print(int(random() * 100))
print(randrange(1, 100))
print(randint(1, 100))
```

## boolean

- True, False
- not은 반대로됨
  - print(not True) => False

## NoneType
- ```Null``` 대신에  
```None``` 사용