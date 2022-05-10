---
title: Python 8 - Collection
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-05-08 00:00:00 +0900
categories: [Grind, Python]
tags: [python, list, dictionary, tuple, set]
---

## list

- 선언은 ```[ ]```안에
- 자료 형식에 상관없이 다 넣을 수 있음.
- 
  | name         | discription         |
  | ------------ | ------------------- |
  | append       | 확장                |
  | copy         | 복사된 새 객체)      |
  | clear        | clear               |
  | count        | 해당 요소 갯수       |
  | extend       | 확장                |
  | index        | 해당 요소 위치       |
  | insert       | 위치에 insert       |
  | pop(pop)     | 위치의 요소 꺼내옴   |
  | pop(res)     | 꺼내면 사라짐       |
  | remove       | 삭제(안꺼내옴)      |
  | reverse      | 정렬 반대로         |
  | sort         | 정렬                |

- append / extend  
pop / remove  
는 약간씩 차이가 있음.
- 
  ```python
  data1 = [0, 1, 2, 3, 4]
  data2 = ["a", "b", "c", "d", "e"]

  data3 = data1.copy()
  data4 = data2.copy()
  data3.append(data4)
  print("{0:10} : {1}".format( list.append.__name__ , data3))

  data3 = data1.copy()
  data4 = data2.copy()
  data4 = data1.copy()
  print("{0:10} : {1}".format( list.copy.__name__ , data4))

  data4 = data2.copy()
  data4.clear()
  print("{0:10} : {1}".format( list.clear.__name__ , data4))

  data4 = data2.copy()
  print("{0:10} : {1}".format( list.count.__name__ , data4.count("d")))

  data3 = data1.copy()
  data4 = data2.copy()
  data3.extend(data4)
  print("{0:10} : {1}".format( list.extend.__name__ , data3))

  data4 = data2.copy()
  print("{0:10} : {1}".format( list.index.__name__ , data4.index("d")))

  data3 = data1.copy()
  data3.insert(3, data4)
  print("{0:10} : {1}".format( list.insert.__name__ , data3))

  data4 = data2.copy()
  print("{0:10} : {1}".format( list.pop.__name__+"(pop)" , data4.pop(2)))
  print("{0:10} : {1}".format( list.pop.__name__+"(res)" , data4))

  data4 = data2.copy()
  data4.remove("d")
  print("{0:10} : {1}".format( list.remove.__name__ , data4))

  data4 = data2.copy()
  data4.reverse()
  print("{0:10} : {1}".format( list.reverse.__name__ , data4))

  data4.sort()
  print("{0:10} : {1}".format( list.sort.__name__ , data4))

  ```

  ```
  append     : [0, 1, 2, 3, 4, ['a', 'b', 'c', 'd', 'e']]
  copy       : [0, 1, 2, 3, 4]
  clear      : []
  count      : 1
  extend     : [0, 1, 2, 3, 4, 'a', 'b', 'c', 'd', 'e']
  index      : 3
  insert     : [0, 1, 2, ['a', 'b', 'c', 'd', 'e'], 3, 4]
  pop(pop)   : c
  pop(res)   : ['a', 'b', 'd', 'e']
  remove     : ['a', 'b', 'c', 'e']
  reverse    : ['e', 'd', 'c', 'b', 'a']
  sort       : ['a', 'b', 'c', 'd', 'e']

  ```

---
## dictionary

- ```{ }```로 선언.
- ```{key : value}``` 쌍의 Item으로 구성
- 
  | name         | discription             |
  | ------------ | ----------------------- |
  | fromkeys     | 새 dict생성             |
  | get          | get                     |
  | items        | key,value get           |
  | keys         | key get                 |
  | pop          | value 꺼냄              |
  | popitem      | value 꺼냄              |
  | setdefault   | get or create           |
  | update       | update value or create  |

- fromkeys는 create.
- pop은 key에 대한 value를 뽑아옴  
popitem은 맨 오른쪽 item을 뽑아옴  
둘 다 해당 item은 dict에서 사라짐  
앞으로 pop는 이런식으로 이해하면될듯.
- setdefault는 key,value를 주고  
key가 있으면 get,  
없으면 key,value로 생성
- update는 ```{key : value}``` 를 주고  
있으면 value update,  
없으면 새로 생성
- 
  ```python
  data = {1: "value1", 2: "value2", 3: "value3"}
  print("{0:10} : {1}".format(dict.fromkeys.__name__ , data.fromkeys("new", "default")))

  data = {1: "value1", 2: "value2", 3: "value3"}
  print("{0:10} : {1}".format(dict.get.__name__ , data.get(1)))
  print("{0:10} : {1}".format(dict.items.__name__ , data.items()))
  print("{0:10} : {1}".format(dict.keys.__name__ , data.keys()))

  print("{0:10} : {1}".format(dict.pop.__name__ , data.pop(2,"err")))
  print("{0:10} : {1}".format(dict.items.__name__ , data.items()))
  print("{0:10} : {1}".format(dict.pop.__name__ , data.pop(2,"err")))
  print("{0:10} : {1}".format(dict.items.__name__ , data.items()))

  data = {1: "value1", 2: "value2", 3: "value3"}
  print("{0:10} : {1}".format(dict.popitem.__name__ , data.popitem()))
  print("{0:10} : {1}".format(dict.items.__name__ , data.items()))

  print("{0:10} : {1}".format(dict.setdefault.__name__ , data.setdefault(1, "default")))
  print("{0:10} : {1}".format(dict.items.__name__ , data.items()))
  print("{0:10} : {1}".format(dict.setdefault.__name__ , data.setdefault(3, "default")))
  print("{0:10} : {1}".format(dict.items.__name__ , data.items()))

  print("{0:10} : {1}".format(dict.update.__name__ , data.update({1: "newvalue1"})))
  print("{0:10} : {1}".format(dict.items.__name__ , data.items()))
  print("{0:10} : {1}".format(dict.update.__name__ , data.update({4: "newvalue4"})))
  print("{0:10} : {1}".format(dict.items.__name__ , data.items()))
  ```

  ```
  fromkeys   : {'n': 'default', 'e': 'default', 'w': 'default'}
  get        : value1
  items      : dict_items([(1, 'value1'), (2, 'value2'), (3, 'value3')])
  keys       : dict_keys([1, 2, 3])
  pop        : value2
  items      : dict_items([(1, 'value1'), (3, 'value3')])
  pop        : err
  items      : dict_items([(1, 'value1'), (3, 'value3')])
  popitem    : (3, 'value3')
  items      : dict_items([(1, 'value1'), (2, 'value2')])
  setdefault : value1
  items      : dict_items([(1, 'value1'), (2, 'value2')])
  setdefault : default
  items      : dict_items([(1, 'value1'), (2, 'value2'), (3, 'default')])
  update     : None
  items      : dict_items([(1, 'newvalue1'), (2, 'value2'), (3, 'default')])
  update     : None
  items      : dict_items([(1, 'newvalue1'), (2, 'value2'), (3, 'default'), (4, 'newvalue4')])
  ```
  - in을 사용해 key가 사용중인지 확인할 수 있음
  - dic.get(key), dic[key]를 사용해  
  value를 찾을 수 있는데  
  dic[key]는 key가 없는경우 exception,  
  dic.get(key)는 None로 알려주는데 이 경우  
  default값을 지정해 줄 수 있음
- 참고로
  
  ```python
  from pprint import pprint
  
  data = {1: ("value1", "string"), 2: ("value2", "string"), 3: ("value3", "string"), 
          4: ("value4", "string"), 5: ("value5", "string"), 6: ("value6", "string")
          }
  print(1 in data)
  
  try:
      print(data[10])
  except KeyError:
      print("KeyError: 10")
  
  print(data)
  pprint(data)
  ```
  
  ```
  True
  KeyError: 10
  {1: ('value1', 'string'), 2: ('value2', 'string'), 3: ('value3', 'string'), 4: ('value4', 'string'), 5: ('value5', 'string'), 6: ('value6', 'string')}
  {1: ('value1', 'string'),
    2: ('value2', 'string'),
    3: ('value3', 'string'),
    4: ('value4', 'string'),
    5: ('value5', 'string'),
    6: ('value6', 'string')}
  ```
  - dict구조가 좀 복잡해지면  
  pprint로 출력하면 좀 정리해줌

---
## tuple

- update, insert가 안되는 list
- ```( )```로 선언
- 슬라이싱 조회가능
- list 보다 빠르다함
- 
  | name      | discription      |
  | --------- | ---------------- |
  | count     | 해당 elmt 개수   |
  | index     | 해당 elmt 위치   |
- 
  ```python
  data = (1, 2, 3, 4, 5, 6, 7, 8, 9, 9)
  print(data)
  print("{0:10} : {1}".format(tuple.count.__name__, data.count(9)))
  print("{0:10} : {1}".format(tuple.index.__name__, data.index(9)))
  print(data[1:5])
  ```

---
## set

- 중복안됨, 순서없음
    - set랑 dict랑 뜯어보면 다르겠지만
    둘 다 순서가 없고 중복이 없는게 같음.
- ```{ }```로 선언.
- 좀 특이한점은 집합처럼 연산이 가능  
인줄알았는데 애초에 이 목적인듯함.
- 
  ```
  | name                         | discription                 |
  | ---------------------------- | --------------------------- |
  | add                          |                             |
  | copy                         |                             |
  | difference                   | 차집합                      |
  | difference_update            | 차집합으로 update           |
  | discard                      | 삭제                        |
  | intersection                 | 교집합                      |
  | intersection_update          | 교집합으로 update           |
  | isdisjoint                   | 교집합의 여집합 여부        |
  | issubset                     | 하위집합                    |
  | issuperset                   | 상위집합                    |
  | remove                       | 삭제                        |
  | symmetric_difference         | 교집합의 여집합             |
  | symmetric_difference_update  | 교집합의 여집합으로 update  |
  | union                        | 합집합                      |
  | update                       | 합집합으로 update           |
  ```
- 
  ```python
  data1 = {1, 1, 2, 2, 3, 3, }
  data2 = {3, 4, 5}
  print(data1, data2)
  print("{0:30} : {1}".format(set.difference.__name__, data1.difference(data2)))
  print("{0:30} : {1}".format(set.difference_update.__name__, data1.difference_update(data2)))

  data1 = {1, 1, 2, 2, 3, 3, }
  data2 = {3, 4, 5}
  print("{0:30} : {1}".format(set.discard.__name__, data1.discard(1)))
  print(data1, data2)

  data1 = {1, 1, 2, 2, 3, 3, }
  data2 = {3, 4, 5}
  print("{0:30} : {1}".format(set.intersection.__name__, data1.intersection(data2)))
  print("{0:30} : {1}".format(set.intersection_update.__name__, data1.intersection_update(data2)))
  print(data1, data2)

  data1 = {1, 1, 2, 2, 3, 3, }
  data2 = {3, 4, 5}
  print("{0:30} : {1}".format(set.isdisjoint.__name__, data1.isdisjoint(data2)))
  print(data1, data2)

  data1 = {1, 2, 3, 4, 5}
  data2 = {3, 4, 5}
  print("{0:30} : {1}".format(set.issubset.__name__, data2.issubset(data1)))
  print("{0:30} : {1}".format(set.issuperset.__name__, data1.issuperset(data2)))

  data1 = {1, 1, 2, 2, 3, 3, }
  data2 = {3, 4, 5}
  print("{0:30} : {1}".format(set.symmetric_difference.__name__, data1.symmetric_difference(data2)))
  print("{0:30} : {1}".format(set.symmetric_difference_update.__name__, data1.symmetric_difference_update(data2)))

  data1 = {1, 1, 2, 2, 3, 3, }
  data2 = {3, 4, 5}
  print("{0:30} : {1}".format(set.union.__name__, data1.union(data2)))
  print(data1, data2)

  data1 = {1, 1, 2, 2, 3, 3, }
  data2 = {3, 4, 5}
  print("{0:30} : {1}".format(set.update.__name__, data1.update(data2)))
  print(data1, data2)
  ```

  ```python
  {1, 2, 3} {3, 4, 5}
  difference                     : {1, 2}
  difference_update              : None
  discard                        : None
  {2, 3} {3, 4, 5}
  intersection                   : {3}
  intersection_update            : None
  {3} {3, 4, 5}
  isdisjoint                     : False
  {1, 2, 3} {3, 4, 5}
  issubset                       : True
  issuperset                     : True
  symmetric_difference           : {1, 2, 4, 5}
  symmetric_difference_update    : None
  union                          : {1, 2, 3, 4, 5}
  {1, 2, 3} {3, 4, 5}
  update                         : None
  {1, 2, 3, 4, 5} {3, 4, 5}
  ```

---
## 자료구조의 변경

```python
temp = {1,2,3}
print(temp, type(temp))
temp = list(temp)
print(temp, type(temp))
temp = tuple(temp)
print(temp, type(temp))
temp = dict.fromkeys(temp)
print(temp, type(temp))
temp = set(temp)
print(temp, type(temp))
```

```
{1, 2, 3} <class 'set'>
[1, 2, 3] <class 'list'>
(1, 2, 3) <class 'tuple'>
{1: None, 2: None, 3: None} <class 'dict'>
{1, 2, 3} <class 'set'>
```

- 호환도 됨

---
## Call by...

- list를 보면, ```.copy()```를 써서 전달헀는데  
보통 넘기는 방식들은 value가 넘어가는데  
여기 쓴애들은 reference가 넘어가는것같다.