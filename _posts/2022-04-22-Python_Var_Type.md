---
title: Variable & Data Type
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-04-22 00:00:00 +0900
categories: [Grind, Python]
tags: [python, variable, data type]
---
## Variable

- 변수. 값을 저장하는 공간.
- 다른 언어에 비해 역시 편하다.
  - c
    ```c
    int a = 10;
    char * str = "test";
    //이건 배열로 쓰는등 아무튼.
    
    ```
      
  - c#
    ```csharp
    int a = 10;
    string str = "c#";
    
    ```
      
  - python
    ```python
    a = 10
    s = "python"
    ```

### highly object-oriented language

- 위 예시에서 차이점을 보면  
자료형을 명시해주지 않는다. 따라서
  ```
  a = 10
  a = "python"
  
  ```
  
  이게 가능해진다.
  
- 이유는 python에서는  
모든것이 객체로 처리되기 때문.  
이런 특성을 순수 객체지향이라 한다.  
- 변수 자체에 값이 할당되는것이 아니라  
값이 저장된 객체를 가리키기 때문.  
c에서 포인터같은 느낌인듯.
- 모든것을 object로 취급한다는것과   
class, method또한  
object로 취급할 수 있다는것은    
사용할 때 자유도가 커짐.
  
  ```python
  def func():
    return "Hello"
  
  a = func
  
  print(a())
  ```
  
  예를들어 이런식의  
  delegate같은 형태 등.  
  다만, custom class가   
  자료형으로 돌아다닐 때  
  조금 헷갈리는 경향이 있음.
  

### Variable Names

- 당연하지만, 예약어가 있어서  
그것만 피하고 만들면 됨.  
`help("keywords")`로  
예약어를 볼 수 있음.
- 명명규칙은 앞에 썼던거에 있음

## Python Data Type

- data type에는  
`int`, `bool`, `float`, `complex`,   
`dict`, `str`, `memoryview`,   
`tuple`, `list`, `bytes`,   
`bytearray`, `set`, `frozenset`  
정도로 보통 정리하는것같음.
- 아무튼, 이 type 분류가  
설명마다 조금씩 달라서  
class별 구분으로 보면  
나중에 공통적으로  
뭔가 있지 않을까해서 정리해봄
  
  ```
  ─ int
    └─ bool
  ─ float
  ─ complex
  ─ Iterable
    └─ Collection
       ├─ Mapping
       │  └─ MutableMapping
       │     └─ dict
       ├─ Sequence
       │  ├─ str
       │  ├─ memoryview
       │  ├─ tuple
       │  ├─ MutableSequence
       │  │  └─ list
       │  ├─ ByteString
       │  │  └─ bytes
       │  └─ MutableSequence, ByteString
       │     └─ bytearray
       └─ AbstractSet
          ├─ frozenset
          └─ MutableSet
             └─ set
  ```

- 해당 type함수로 초기화할수도 있고,  
직접 할당해도 상관없고. 이건 차차.
- type cast는  
변경하고 싶은 type으로 명시하면됨
  
  ```python
  x = set(list([1,2]))
  
  print(x)
  print(type(x))
  ```
  
  list를 set로 cast한 경우.  
  casting이 되는경우 해줌.