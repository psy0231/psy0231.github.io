---
title: dict
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-04-26 00:00:00 +0900
categories: [Grind, Python]
tags: [python, data type, dict]
---


## Init

- mapping - mutablemapping 계통으로  
하위 type으로 dict가 유일함.
- `key`, `value`, `item`으로 구성되며    
`{key : value}` 쌍이 기본 구조이고  
이 쌍을 `item`이라 함.
- 변경가능한 hash table 구조이며   
hash가능하지 않은값으로  
mutable은 key로 사용할 수 없음.
- 괄호는 `{ }`
- `__init__`은  
`__init__(self, **kwargs)`  
`__init__(self, map, **kwargs)`  
`__init__(self, iterable, **kwargs)`  
이렇게 있다.  
해서 `dict`를 만들수 있는 방법은   
기본적으로
  
  ```python
  a = dict()
  b = {}
  ```
  이렇게 빈 dict를 만들 수 있음.  
  그 외 방법으로는 
  ```python
  a = dict(one=1, two=2, three=3)
  b = {'one': 1, 'two': 2, 'three': 3}
  c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
  d = dict([('two', 2), ('one', 1), ('three', 3)])
  e = dict({'three': 3, 'one': 1, 'two': 2})
  f = dict({'one': 1, 'three': 3}, two=2)
  ```
  이 때  
  a == b == c == d == e == f는 True.
  
- dict의 동등비교(==)는  
내부 순서가 상관없음
- <, <=, >=, >등의 비교는 TypeError
- insert는 순서상 가장 뒤에 추가되며  
3.7이상에서 이  순서를 보장함.  
value에 대한 update는  
순서에 영향 없음.
- reversed는 3.8이상.

## Method

|||
| --- |--- |
| [list(dict)](#listdict) | [dict.items()](#dictitems) |
| [len(dict)](#lendict) |[dict.keys()](#dictkeys) |
| [dict[key]](#dictkey) |[dict.pop(key[, default])](#dictpopkeydefault) |
| [dict[key]=value](#dictkey--value) |[dict.popitem()](#dictpopitem) |
| [del dict[key]](#del-dictkey) |[reversed(dict)](#reverseddict) |
| [key (not) in dict](#key-not-in-dict) | [dict.setdefauilt(key[, default])](#dictsetdefaultkeydefault) |
| [iter(dict)](#iterdict) |[dict.update([other])](#dictupdateother) |
| [dict.clear()](#dictclear) |[dict.values()](#dictvalues) |
| [dict.copy()](#dictcopy) |[dict \| other](#dict--other) |
| [fromkeys(iterable, [, value])](#fromkeysiterablevalue) |[dict \|= oather](#dict--other-1) |
| [dict.get(key[, default])](#dictgetkeydefault) |

### list(dict)
- `list`로 type cast된 새 list반환.
- key만 반영됨

```python
data = dict({1: "value1", 2: "value2", 3: "value3"})
test = list(data)
print(test)
```
```python
[1,2,3]
```

### len(dict)
- dict의 item의 수

```python
data = dict({1: "value1", 2: "value2", 3: "value3"})
print(len(data))
```
```python
3
```

### dict\[key\]
- key에 해당하는 value를 반환.
- key가 없으면 KeyError

```python
data = dict({1: "value1", 2: "value2", 3: "value3"})
print(data[1])
print(data[4])
```

```python
KeyError: 4
value1
```

- dict를 상속받고  
`__missing__`를 정의하면  
KeyError 문제를 해결할 수 있음.

### dict[key] = value
- dict[key] 를 value 로 설정
- 이미 key가 있으면 update,  
없으면 insert

```python
data = dict({1: "value1", 2: "value2", 3: "value3"})
data[1] = True
data[4] = "new value"

print(data)
```

```python
{1: True, 2: 'value2', 3: 'value3', 4: 'new value'}
```

### del dict\[key\]

- key에 해당하는 item삭제  
key가 없으면 KeyError

```python
data = dict({1: "value1", 2: "value2", 3: "value3"})
del data[1]
print(data)
```

```python
{2: 'value2', 3: 'value3'}
```

### key (not) in dict

- dict에 key가 있으면 True(False)  
없으면 False(True).

### iter(dict)

- key에 대한 iterator 반환,
- same as `iter(d.keys())`

### dict.clear()

- dict의 모든 item삭제

### dict.copy()

- dict의 얕은복사

### fromkeys(iterable,[,value])

- iterable을 key로 하고   
value를 갖는 새 dict를 반환.

```python
k = [1, 2, 3]
v = ['v1', 'v2', 'v3']

ndata = dict.fromkeys(k,v)
print(ndata)

ndata[3] = 3
print(ndata)
```

```python
{1: ['v1', 'v2', 'v3'], 2: ['v1', 'v2', 'v3'], 3: ['v1', 'v2', 'v3']}
{1: ['v1', 'v2', 'v3'], 2: ['v1', 'v2', 'v3'], 3: 3}
```

- value를 지정하지 않을수있음  
그럴경우 None
- parameter로 전달한 value는  
단일 instance로 취급하고   
새 dict의 모든 value가 됨.  
위의 결과가  
`{1: 'v1', 2: 'v2', 3: 'v3']}`이  
아닌것처럼.
- 또한 parameter로 전달한value를  
dict 생성 이후에 수정하면  
해당 value참조가 전부 바뀜
  
  ```python
  k = [1, 2, 3]
  v = ['v1', 'v2', 'v3']
  ndata = dict.fromkeys(k,v)
  print(ndata)
  v[2] = 123
  print(ndata)
  ```
  
  ```python
  {1: ['v1', 'v2', 'v3'], 2: ['v1', 'v2', 'v3'], 3: ['v1', 'v2', 'v3']}
  {1: ['v1', 'v2', 123], 2: ['v1', 'v2', 123], 3: ['v1', 'v2', 123]}
  ```
  이런 비슷한 문제는  
  type inheritance에 쓴적있음.
  

### dict.get(key[,default])

- key에 해당하는 value반환.

```python
data = dict({1: "value1", 2: "value2", 3: "value3"})

print(data.get(3))
print(data.get(4, "empty value"))
```

```python
value3
empty value
```

- key가 없을 경우 None를 반환  
이 경우에 대한 반환값은 변경 가능.
- `dict[key]`와 다르게 KeyError가 없음.

### dict.items()

- dict의 item view를 반환.

```python
data = dict({1: "value1", 2: "value2", 3: "value3"})

print(data.items())
```

```python
dict_items([(1, 'value1'), (2, 'value2'), (3, 'value3')])
```

- for에서 자주씀.  
`for item in dict:`가 안되서  
`for k, v in data.items():`

### dict.keys()

- dict의 key view를 반환.

### dict.pop(key[,default])

- key에 해당하는 value반환.

```python
print(data.pop(4, None))
```

```python
None
```

- key가 없으면 KeyError인데  
default를 지정하면  
KeyError없이 지정된 default를 반환

### dict.popitem()

- 위 기능을 LIFO로 실행함.  
LIFO순서 보장은 3.7이상
- dict가 비어있으면 KeyError

### reversed(dict)

- key에 대해 reversed iterator 반환.
- same as `reversed(d.keys())`
- iter의 반대

### dict.setdefault(key[,default])

- key가 있으면 해당 value반환
- 없으면 key insert하고  
해당 value를 default로 설정후  
default를 반환.
- default를 지정하지 않을수 있고  
그럴경우 None.

```python
data = dict({1: "value1", 2: "value2", 3: "value3"})

print(data.setdefault(3, 3))
print(data.setdefault(4, "val 4"))
print(data)
```

```python
value3
val 4
{1: 'value1', 2: 'value2', 3: 'value3', 4: 'val 4'}
```

### dict.update([other])

- other의 key/value쌍으로 update.
- 기존 key를 덮어쓰며 None를 반환.
- other에 들어갈 형식은  
dict생성방법과 동일해야함.
- key가 있으면 update,  
key가 없으면 insert

```python
data = dict({1: "value1", 2: "value2", 3: "value3"})
data.update({1:"updated", 4:"new 4"})
print(data)
```

```python
{1: 'updated', 2: 'value2', 3: 'value3', 4: 'new 4'}
```

### dict.values()

- dict의 value view반환.
- view끼리 eq비교는 항상 False
  
  ```python
  print(data.values() == data.values())
  ```
  
  ```python
  False
  ```
  

### dict | other

- dict와 other을 합친 새 dict반환.
- dict, other은 dict type이어야함.
- 둘 다 같은키를 갖는다면  
other의 value를 사용.

```python
data = dict({1: "value1", 2: "value2", 3: "value3"})
other = dict({3:3, "value 4": 4, "value 5": 5})

print(data | other)
```

```python
data = dict({1: "value1", 2: "value2", 3: "value3"})
other = dict({3:3, "value 4": 4, "value 5": 5})

print(data | other)
```

### dict |= other

- other을 이용해  
dict를 update함  
위는 새 dict반환이면  
이건 dict update.
- other은 key/value쌍의   
mapping 또는 iterable일 수 있음.

## Dictionary view objects

- 위에 한번씩 나왔지만  
`dict.keys()`, `dict.values()`,  
`dict.items()`는  
각각의 view object를 반환함.
- 이 new object들은 동적으로  
dict의 변경이 항상 반영됨.
- 지원하는 method로는  
`len`,  `iter`, `in`, `reversed`,  
`dictview.mapping`이 있음.  
dict의 method에서  
이름이 같으면 같은기능.
- `dictview.mapping`의 경우  
view의 원본 dict를 감싸는  
types.MappingProxyType을 반환.  
3.10이상 지원.
- key view는 항목이 고유하고  
hash가능하기 때문에 set과 유사.
- 모든 value가 hash 가능하여  
(key, value) 쌍이 고유하다면  
item view도 set과 유사.
- value view는 일반적으로  
항목이 고유하지 않으므로  
set과 유사하게 취급되지 않음.
- 집합형 뷰의 경우  
collections.abc.Set에 대해 정의된  
모든 연산을 사용 가능.  
예를들어 `==`, `<`, `^`
- view 사용 예
  
  ```python
  dishes = {'eggs': 2, 'sausage': 1, 'bacon': 1, 'spam': 500}
  keys = dishes.keys()
  values = dishes.values()
  
  # iteration
  n = 0
  for val in values:
      n += val
  print(n)
  
  # keys and values are iterated over in the same order (insertion order)
  print(list(keys))
  
  print(list(values))
  
  # view objects are dynamic and reflect dict changes
  del dishes['eggs']
  del dishes['sausage']
  print(list(keys))
  
  # set operations
  print(keys & {'eggs', 'bacon', 'salad'})
  
  print(keys ^ {'sausage', 'juice'})
  
  # # get back a read-only proxy for the original dictionary
  # values.mapping
  #
  # values.mapping['spam']
  ```
  
  ```python
  504
  ['eggs', 'sausage', 'bacon', 'spam']
  [2, 1, 1, 500]
  ['bacon', 'spam']
  {'bacon'}
  {'spam', 'bacon', 'sausage', 'juice'}
  ```