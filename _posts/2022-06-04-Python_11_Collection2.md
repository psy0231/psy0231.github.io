---
title: Python 11 - Collection 2
author:
name: owner
link: https://github.com/psy0231
date: 2022-06-04 01:00:00 +0900
categories: [Grind, Python]
tags: [python, collection]
---

## Intro

- collection1에서는  
내장인 범용 컨테이너였음.
- 여긴 collections에 있는  
특수 컨테이너.
- 를 할 계획이었는데  
정리가 잘 안되서  
필요한게 보일때마다  
추가하기로함.
- 종류로는

  |name|discription|
  |---|---|
  |namedtuple  |이름 붙은 필드를 갖는 튜플 서브 클래스를 만들기 위한 팩토리 함수
  |deque       |양쪽 끝에서 빠르게 추가와 삭제를 할 수 있는 리스트류 컨테이너
  |ChainMap    |여러 매핑의 단일 뷰를 만드는 딕셔너리류 클래스
  |Counter     |해시 가능한 객체를 세는 데 사용하는 딕셔너리 서브 클래스
  |OrderedDict |항목이 추가된 순서를 기억하는 딕셔너리 서브 클래스
  |defaultdict |누락된 값을 제공하기 위해 팩토리 함수를 호출하는 딕셔너리 서브 클래스
  |UserDict    |더 쉬운 딕셔너리 서브 클래싱을 위해 딕셔너리 객체를 감싸는 래퍼
  |UserList    |더 쉬운 리스트 서브 클래싱을 위해 리스트 객체를 감싸는 래퍼
  |UserString  |더 쉬운 문자열 서브 클래싱을 위해 문자열 객체를 감싸는 래퍼  

---

## namedtuple

- tuple의 각 item에  
이름을 쓸 수 있다.
- 이거 비슷한게 있었는데   
앞에 다시 보던가.

### structure

- 원형은
  
  ```python
  (function) namedtuple: (typename: str, 
                        field_names: str | Iterable[str], 
                        *, 
                        rename: bool = ..., 
                        module: str | None = ..., 
                        defaults: Iterable | None = ...) -> Type[tuple]
  ```
  
  이런 모습이고 
  
  ```python
  
  Returns a new subclass of tuple with named fields.
  
  >>> Point = namedtuple('Point', ['x', 'y'])
  >>> Point.__doc__                   # docstring for the new class
  'Point(x, y)'
  >>> p = Point(11, y=22)             # instantiate with positional args or keywords
  >>> p[0] + p[1]                     # indexable like a plain tuple
  33
  >>> x, y = p                        # unpack like a regular tuple
  >>> x, y
  (11, 22)
  >>> p.x + p.y                       # fields also accessible by name
  33
  >>> d = p._asdict()                 # convert to a dictionary
  >>> d['x']
  11
  >>> Point(**d)                      # convert from a dictionary
  Point(x=11, y=22)
  >>> p._replace(x=100)               # _replace() is like str.replace() but targets named fields
  Point(x=100, y=22)
  ```
  
  대충 이런 설명이 나옴
  

### parameter

- typename : 앞으로 쓰일 이름.
- field_names : item name - 여기까지 필수
  
  ```python
  abc = namedtuple('test', 'a b c')
  t = abc(c=3, b=2, a=1)
  print(t)
  # test(a=1, b=2, c=3)
  ```
  
  - ```print```하면 ```test```로 나옴.  
  처음에  ```typename```로 했던.
  - 정해진 `field_name`으로  
  직접 넣을 수 있고(위의 경우)  
  위처럼 지정하지 않으면  
  자리에 맞춰들어감.
  - ```field_name```은  
  ```'a b c'``` 또는  
  ```'a,b,c'```  또는  
  ```['a','b','c']```다 가능.
  - dict랑 비슷해지는데  
  위의 경우 ```t.a```로  
  1을 불러올 수 있음

- rename : true일떄,  
유효하지 않은 필드명 자동 대체.
  
  ```python
  abc = namedtuple('test', 'a def c a')
  a = abc(1, 2, 3, 4)
  ```
  
  - 이 경우 a는 중복,  def는 예약어로  
  원래 쓸 수 없음
  - ```rename=True```를 쓰면  
  ```’a _1 c _3’```으로 자동 치환됨.
- default : 기본값
  
  ```python
  abc = namedtuple('test', 'a b c', rename=True, defaults=(2, 3))
  a = abc(0)
  print(a)
  # test(a=0, b=2, c=3)
  ```
  
  - 필드 기본값을 설정.
  - 인덱스상 뒤부터 채워주고  
  정하지 않은 필드는 필수값이됨
- module는 ```__module__```값을 설정하는데  
왜쓰는지는 모름...

### method

- 시작은
  
  ```python
  abc = namedtuple('test', 'a b c')
  ```
  
  이것부터.
  
- **_make**
  
  ```python
  temp = [1, 2, 3]
  t = abc._make(temp)
  print(t)
  ```
  
  - 새 인스턴스 생성
- _asdict()
  
  ```python
  d = t._asdict()
  print(d)
  ```
  
  - dict로 형변환…
- 반대로  
dick → namedtuple
  
  ```python
  t1 =abc(**d)
  print(t1)
  ```
  
- _replace
  
  ```python
  t = t1._replace(a=4)
  print(t)
  print(t1)
  ```
  
  - 해당 값을 바꾼 새 instance반환
  - 원본은 안바뀜
- _fields
  
  ```python
  print(t1._fields)
  ```
  
  - read field

---

## deque

### double-ended queue

- python에는 queue, stack가 딱히 없었다.
- 그나마 list에 ```pop(index)```와  
```append(item)```, ```insert(index)```등  
잘 조합하면 됐을거다.
- ```insert(0)```, ```pop()```를 하면  
queue가 만들어지는데  
이 경우 성능이 안좋아짐
- deque는  
스택과 큐를 일반화 한 것으로  
스레드 안전하고 메모리 양쪽 끝에서  
```append```, ```pop```을 거의 같은 O(1)의  
성능을 냄
- 근데 또 중간에 끼워넣는건 비슷하다네..

### method

- 
  |||
  |---|---|
  |append     | 끝에 붙임       |
  |appendleft | 앞에 붙임       |
  |clear      | clear           |
  |copy       | 앝은 복사       |
  |count      | count           |
  |extend     | 끝에 추가       |
  |extendleft | 앞에 추가       |
  |index      | item 위치       |
  |insert     | 위치에 추가     |
  |maxlen     | 최대 크기 제한  |
  |pop        | 끝에 반환/제거  |
  |popleft    | 앞에 반환/제거  |
  |remove     | item 제거       |
  |reverse    | 역정렬          |
  |rotate     | 오른쪽으로 회전 |

- 대부분 list를 기본으로 같음.
- 특화된점으로는 ```xxxleft```가 있는데  
index상 0쪽에 붙이는 경우 씀.   
아무튼 비슷하기 때문에 패스.
- maxlen
  
  ```python
  l = [1, 2, 3, 4, 5]
  dl = deque(l, maxlen=3)
  print(dl)
  
  # deque([3, 4, 5], maxlen=3)
  ```
  
  - 최대 길이고  
  이런경우 namedtuple처럼  
  뒤에만 취급.
- rotate
  
  ```python
  l = [1, 2, 3, 4, 5]
  dl = deque(l)
  print(dl)
  dl.rotate(2)
  print(dl)
  
  # deque([1, 2, 3, 4, 5])
  # deque([4, 5, 1, 2, 3])
  ```
  
  - 기본 방향은 오른쪽.
  - index에 숫자를 더한다고 생각하면됨.
  - 결과로 위에서 2를 넣어서  
  idx=0이었던 1이  
  idx=2가됨.
  - circular queue식으로 회전.

---

## ChainMap

---

## Counter

---

## OrderedDict

---

## defaultdict

---

## UserDict

---

## UserList

---

## UserString