---
title: Python 8 - Collection
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-05-10 00:00:00 +0900
categories: [Grind, Python]
tags: [python, list, dictionary, tuple, set, frozenset, bytes, bytearray, memoryview]
---

- 일단, collection이  
이번 제목에 적합한지 잘 모르겠으나,  
익숙한 표현이 이거라 일단 씀.

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
  
  - in을 사용해 key가 사용중인지 확인할 수 있음
  - dic.get(key), dic[key]를 사용해
  value를 찾을 수 있는데
  dic[key]는 key가 없는경우 exception,
  dic.get(key)는 None로 알려주는데 이 경우
  default값을 지정해 줄 수 있음
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

## frozenset

- frozenset은 immutable  
set은 mutable
- 다시말해, frozenset는 수정이 안됨.
- 이 차이 외에는 set이랑 같음

---

## bytes

- binary를 조작하기위해 사용.
- 0x00부터 0xff까지 사용
- 이 class안에 method는 
string랑 같은게 대다수라 생략함.  
그 외 특징적인것만.

### 생성

```python
from pprint import pprint

# byte(dec sequence)
b = bytes([72, 101, 108, 108, 111])
print(b)

# byte(hex sequence)
b = bytes([0x48, 0x65, 0x6c, 0x6c, 0x6f])
print(b)

# byte(string sequence)
b = bytes("Hello", encoding="utf-8")
print(b)

# byte(len)
# 이 경우 0x00으로
b = bytes(5)
print(b)

# byte(range)
b = bytes(range(0, 256))
pprint(b)

# byte.fromhex(hexstring)
b = bytes.fromhex('2Ef0 F120f2  ')
print(b)
```

```python
b'Hello'
b'Hello'
b'Hello'
b'\x00\x00\x00\x00\x00'
(b'\x00\x01\x02\x03\x04\x05\x06\x07\x08\t\n\x0b\x0c\r\x0e\x0f\x10\x11\x12\x13'  
  b'\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f !"#$%&\'()*+,-./01234567'   
  b'89:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~\x7f'
  b'\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f'
  b'\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f'
  b'\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf'
  b'\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf'
  b'\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf'
  b'\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf'
  b'\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef'
  b'\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff')
b'.\xf0\xf1 \xf2'
```

- ascii에서 맞는게 있으면 그걸로 보임
- fromhex는 str로 된 byte형식을  
bytes로 반환.  
```' '```는 무시됨.

### hex그대로 출력

- ascii로 변환 안할 때(str)
  
  ```python
  # byte(range)
  b = bytes(range(0, 256))
  pprint(b.hex('-'))
  ```
  
  ```python
  '
  00-01-02-03-04-05-06-07-08-09-0a-0b-0c-0d-0e-0f-
  10-11-12-13-14-15-16-17-18-19-1a-1b-1c-1d-1e-1f-
  20-21-22-23-24-25-26-27-28-29-2a-2b-2c-2d-2e-2f-
  30-31-32-33-34-35-36-37-38-39-3a-3b-3c-3d-3e-3f-
  40-41-42-43-44-45-46-47-48-49-4a-4b-4c-4d-4e-4f-
  50-51-52-53-54-55-56-57-58-59-5a-5b-5c-5d-5e-5f-
  60-61-62-63-64-65-66-67-68-69-6a-6b-6c-6d-6e-6f-
  70-71-72-73-74-75-76-77-78-79-7a-7b-7c-7d-7e-7f-
  80-81-82-83-84-85-86-87-88-89-8a-8b-8c-8d-8e-8f-
  90-91-92-93-94-95-96-97-98-99-9a-9b-9c-9d-9e-9f-
  a0-a1-a2-a3-a4-a5-a6-a7-a8-a9-aa-ab-ac-ad-ae-af-
  b0-b1-b2-b3-b4-b5-b6-b7-b8-b9-ba-bb-bc-bd-be-bf-
  c0-c1-c2-c3-c4-c5-c6-c7-c8-c9-ca-cb-cc-cd-ce-cf-
  d0-d1-d2-d3-d4-d5-d6-d7-d8-d9-da-db-dc-dd-de-df-
  e0-e1-e2-e3-e4-e5-e6-e7-e8-e9-ea-eb-ec-ed-ee-ef-
  f0-f1-f2-f3-f4-f5-f6-f7-f8-f9-fa-fb-fc-fd-fe-ff
  '
  ```
  - ```hex()```안에  
  sep를 선택적으로 넣을 수 있다.  
  단, 한글자.

### string 변환

```python
# 1
str_original = 'Hello'

bytes_encoded = str_original.encode(encoding='utf-8')
print(type(bytes_encoded))

str_decoded = bytes_encoded.decode()
print(type(str_decoded))

print('Encoded bytes =', bytes_encoded)
print('Decoded String =', str_decoded)
print('str_original equals str_decoded =', str_original == str_decoded)

# 2
print(str(bytes_encoded, encoding='utf-8'))
```

```python
<class 'bytes'>
<class 'str'>
Encoded bytes = b'Hello'
Decoded String = Hello
str_original equals str_decoded = True
Hello
```

- string랑 변환할 떄.
- str로 표현 안되는건 exception으로 끝남
- 1처럼 decode / encode 또는  
2처럼 형변환.
- 변환 기본은 utf-8

---
## bytearray

- 위랑 같음
- 차이점은
    - bytes는 변경 불가
    - bytearray는 변경가능
- 예를들면
    
    ```python
    # 1
    b = bytearray(5)
    print(b)
    b[1] = 0xff
    print(b)
    
    # 2
    b = bytes(5)
    print(b)
    b[1] = 0xff
    print(b)
    ```
    
    ```python
    bytearray(b'\x00\x00\x00\x00\x00')
    bytearray(b'\x00\xff\x00\x00\x00')
    b'\x00\x00\x00\x00\x00'
    Traceback (most recent call last):
      File , line 11, in <module>
        b[1] = 0xff
    TypeError: 'bytes' object does not support item assignment
    ```
    

---
## memoryview

- 일단 설명은  
memoryview 객체는 파이썬 코드가  
버퍼 프로토콜을 지원하는 객체의  
내부 데이터에 복사 없이 접근할 수 있게 합니다.
- 설명만 보면, 이걸 쓸 수 있는상황은  
버퍼 프로토콜을 지원하는 개체에서만 이고,  
쓰는 이유는 ‘복사없이’를 보면  
속도때문으로 보임.
- 버퍼 프로토콜을 지원하는 내장 객체에는  
bytes 와 bytearray 가 있다.
- indexing, slicing가능.
    
    ```python
    v = memoryview(b'abcefg')
    print(v)
    print(v[1])
    print(v[-1])
    print(v[1:4])
    print(v.readonly)
    ```
    
    ```python
    98
    103
    <memory at 0x0172B9E8>
    True
    ```
    
- 위 경우 readonly = true때문에  
수정은 못하는데
    
    ```python
    b = bytearray(b'abcefg')
    v = memoryview(b)
    print(v.readonly)
    v[0] = ord('X')
    print(b)
    ```
    
    ```python
    False
    bytearray(b'Xbcefg')
    ```
    
    이 경우가 되는거보면  
    위에는 bytes로 일단 만들어진듯.
    

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
temp = frozenset(temp)
print(temp, type(temp))
temp = bytes(temp)
print(temp, type(temp))
temp = bytearray(temp)
print(temp, type(temp))
# temp = memoryview(temp)
# print(temp, type(temp))
# temp = str(temp)
# print(temp, type(temp))
temp = list(temp)
print(temp, type(temp))
```

```
{1, 2, 3} <class 'set'>
[1, 2, 3] <class 'list'>
(1, 2, 3) <class 'tuple'>
{1: None, 2: None, 3: None} <class 'dict'>
{1, 2, 3} <class 'set'>
frozenset({1, 2, 3}) <class 'frozenset'>
b'\x01\x02\x03' <class 'bytes'>
bytearray(b'\x01\x02\x03') <class 'bytearray'>
[1, 2, 3] <class 'list'>
```

- 호환도 됨
- 주석부분 끼면 암튼 달라져서 일단 뺌

---

## Call by

- list를 보면, ```.copy()```를 써서 전달헀는데  
보통 넘기는 방식들은 value가 넘어가는데  
여기 쓴애들은 reference가 넘어가는것같다.