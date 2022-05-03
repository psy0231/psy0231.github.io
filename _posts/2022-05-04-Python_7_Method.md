---
title: Python 7 - Method
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-05-04 00:00:00 +0900
categories: [Grind, Python]
tags: [python, method, function]
---

> 이거 전에 [Concept](https://psy0231.github.io/posts/Stub_Concept/) 참고.

## Method

- python에서는  
method, function둘 다 사용.

```python
# def name(params):
#     statement
#     return ...

def temp(param1, param2):
    print(param1, param2)
    return param1 + param2, param1 - param2, [param1, param2]

plus, minus, listnum = temp(1, 2)

print(temp(1, 2))
```

```
1 2
1 2
(3, -1, [1, 2])
```

- return은 함수 원형에 적지 않음.  
내부에서 return으로 퉁침.
- return은 다수가 될 수 있음.  
각각은 ```,```로 구분함.
- print는 항상 알아서 다 해주는듯.
- default value는 param=value로 해주면 됨 위에서는  
```def temp(param1, param2=17):```이런식.
- parameter 전달 시 이름을 써줄 수 있는데  
이렇게 하면 순서가 상관 없어짐 예를들어,   
```temp(param2=1, param1=2)```를 하면  
1,2가 순서가 아닌 맞는 param으로 들어감.  
출력하면 (3, 1, [2, 1])로.

## Variadic Args

- 변수 타입을 정하지 않는다는건

  ```python
  def temp1(param):
      # for i in param:
      print("{0}", param)
      if type(param) == 'int':
          print('int')
      else:
          print('other')
  
  temp1(1)
  temp1([1,2,3])
  ```
  
  ```python
  1
  other
  [1, 2, 3]
  other
  ```
  
  - 이런식으로 뭐가 들어가도 상관없긴함
  - 다만 안에 for문이 들어가면  
  그때부터 param은 collection으로 무조건쓰인다.

- parameter를 가변으로 하려면  
```*```을 써서 표시한다.
    
    ```python
    def temp(*params):
        for i in params:
            print(i, end=' ')
        print()
    
    temp(1, 2, 3, 4, 5)
    temp("a", "b", "c")
    temp(1)
    ```
    
    ```python
    <class 'tuple'>
    1 2 3 4 5 
    <class 'tuple'>
    a b c
    <class 'tuple'>
    1
    ```
    
    - 나열해서 전달한 args는  
    함수 내부에서 tuple로 바뀜
    - 이건 args가 하나일때도 동일
- 섞어서도 쓸 수 있는데 
이때 가변 args은 생략 가능
    
  ```python
  def temp(default, *params):
      print(type(default), type(params))
      for i in params:
          print("{0} : {1}".format(default,i))
  
  temp(1, 2, 3, 4, 5)
  temp("a", "b", "c")
  temp(1)
  ```
  
  ```python
  <class 'int'> <class 'tuple'>
  1 : 2
  1 : 3
  1 : 4
  1 : 5
  <class 'str'> <class 'tuple'>
  a : b
  a : c
  <class 'int'> <class 'tuple'>
  ```
  

## Keyword Args

- 위경우 ```*args```로 들어온 값들은  
tuple형식으로 받을 수 있었다.
- 이 경우는 dict로.

```python
def temp(**kwargs):
    return kwargs

d = temp(a=1, b=2)
print(type(d))
print(d)
```

```python
<class 'dict'>
{'a': 1, 'b': 2}
```

- ```*args```랑 ```**kwargs```가  
같이 쓸만한데가 뭐가있나 싶었는데  
대충 이런식으로 쓰지 않을까 싶다
  
  ```python
  def customPrint(*args, **kwargs):
      sep = ' '
      end = '\n'
      res = ""
  
      if "sep" in kwargs:
          sep = kwargs.get("sep")
      if "end" in kwargs:
          end = kwargs.get("end")
      
      for i in args:
          res += str(i) + sep
      
      if len(args) > 0:
          l = list(res)
          l.pop()
          res = "".join(l)
      res += end
  
      return res
  
  print(customPrint(1, 2, 3, sep='-', end='^^'))
  
  print(customPrint(1, 2, 3))
  ```
  
  ```python
  1-2-3^^
  1 2 3
  ```
  

## variable scope

- 여기도 여전히 
전역변수, 지역변수 구분

```python
var = 1

def temp(arg):
    global var
    var = 2
    return var

var2 = temp(var)
print(var)
print(var2)
```

```python
2
2
```

- 이런식으로 global을 사용해  
전역변수처럼 만들수도 있음
- 위경우 외부에 있는 var로 맞췄는데  
그건 딱히 상관없는것같음.
- 다시말해 함수안에 global로 선언된 변수는  
함수 밖에서도 호출 가능.

## lambda

- lambda를 사용해 무명함수를 만들 수 있음

```python
# lambda [input] : [return]

a = (lambda x : x**x)(3)
print(a)

f = lambda x : x**x

print(f(2))
```

- input는 상관없는데 return 은 하나만 된다함.
- 아무튼. 여기도 이벤트나  
일회성으로 쓸떄 쓰는가봄.  
- 같은 input로 여러 연산을 할 경우  
  ```python
  l = [
      lambda x: x ** x, 
      lambda x: x * 100, 
      lambda x: str(x) + "!" 
      ]
  
  for i in l:
      print(i(3))
  ```
  
  ```python
  27
  300
  3!
  ```
  라던가, 

- 특정 method는 별개의 method가 필요한데  
  ```python
  target =  [ 'baa   ','a', ' asdf ', '   ba']
  print(sorted(target, key=lambda x : len(x.strip())))
  ```
  ```python
  ['a', '   ba', 'baa   ', ' asdf ']
  ```
  - 이런거..  
  - target의 각 item을 lambda로 새로 계산하고  
  결과를 기준으로 새로 정렬됨.
  
  ```python
  a = filter(lambda x: x % 2, range(10))
  print(list(a))

  # def mtd(x):
  #     if x % 2 == 0:
  #         return False
  #     else:
  #         return True

  # a = filter(mtd, range(10))
  # print(list(a))
  ```
  ```python
  [1, 3, 5, 7, 9]
  ```
  - lambda 안쓰는경우 주석처럼됨.
  - 이거랑 비슷한게  
  map, reduce 등등..
