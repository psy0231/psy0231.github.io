---
title: Python 2 Variable
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-04-08 00:00:00 +0900
categories: [Grind, Python]
tags: [python]
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
    ```c#
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
  
  ```python
  a = 10
  a = "python"
  ```
  이게 가능해진다.

- 그 이유는 python은 모든것이 객체로 처리되기 때문이다.  
이런 특성을 순수 객체지향이라 한다.
- python은 변수 자체에 값이 할당되는것이 아니라  
값이 저장된 객체를 가리키기 때문이다.  
내부적으로 c에서 포인터같은 짓을 하는것이다.
- 모든것을 object로 취급한다는것과  
class, method또한 object로 취급할 수 있다는것은  
다음도 가능하게함. 
  ```pyhon
  def func():
    return "Hello"

  a = func

  print(a())
  ```
  그거 비슷하게 보이는거 맞음.   
  C# 할 떄 봤던 delegate.  
  delegate도 func pointer랑 엮여서 나왔고  
  여기도 변수가 pointer처럼 동작하니 이런것같음. 

## operator
- 연산자.
- 뭘 놓어야될지 몰라서 간단히만 넣음.

| operator        | symbol |  ex  |   res |
|-----------------|:------:|:----:|------:|
| addition        |    +   |  5+3 |     8 |
| subtraction     |    -   |  5-3 |     2 |
| multiplication  |    *   |  5*3 |    15 |
| division(float) |    /   |  5/3 | 1.667 |
| division(int)   |   //   | 5//3 |     1 |
| module(??)      |    %   |  5%3 |     2 |
| modulo          |   **   | 5**3 |   125 |

- power이 math가 아니라 내장임.
- 나누기 졀과는 실수나 정수 선택 할 수 있음.
