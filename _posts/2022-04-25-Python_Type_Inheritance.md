---
title: Type Inheritance
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-04-25 00:00:00 +0900
categories: [Grind, Python]
tags: [python, data type]
---

## Init

- 여기 쓸 내용이 정확한지,  
쓸만한 내용인지 모르겠음..
- int, float, complex, None은   
상속관계가 없다.
- bool은 int의 subclass.
- 그 외 나머지 type인  
`dict`, `str`, `memoryview`,  
`tuple`, `list`, `bytes`,  
`bytearray`, `set`, `frozenset`은  
`Iterable`에서 파생한다.
    
  ```python
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
    
- 예를들어 list의 경우  
parent class를 따라가면  
`MutableSequence`, `Sequence`,  
`Collection`, `Iterable`이 있는데  
이 class 들은 `typing.pyi`에 있다.  
이 module은 type hint를 위해 쓰이고  
이 type들이 실제 정의된곳은  
`typing.pyi`가 참조하는 `collections.abc`
- 아무튼 parent로 갈수록  
추상화라 쓸게 있긴한가 싶지만  
공통된걸 정리하면 좋을것같기도하고    
`collections.abc`에 다른게 있긴한데  
일단은 여기 직접 관련된것만.  
더 필요하면 나중에 하겠지.

## Iterable

- 이 계통 최상위.
- 보통 자주 보이는 설명에  
반복 가능한 객체 라고하는데  
이 의미를 잘 모르겠음.
- 말고 다른 설명으로   
갖고고있는 item을  
return할 수 있는 객체라고함.  
이건 대충 알겠고.
- 막상 `Iterable` 에는  
`__iter__`하나만 있는데   
이게 있어야하나봄.

## Collection

- `iterable`, `container`,  
`size`의 subclass.
- `container`에서 `__contains__`,  
`size`에서  `__len__`으로  
item 소유 판별 및 개수 관련인듯.

## Mapping

- `Collection`의 subclass
- key - value의 hash구조 사용.
- `__getitem__`,`get`,`items`,  
`__eq__`, `keys`, `values`,  
`__contains__`가 있음.

### MutableMapping

- `Mapping`의 subclass로  
변경 가능한 `Mapping`
- 이 뒤로 계속 그렇지만  
Mutable이 오면 변경가능.   
변경가능은 item을 바꾼다거나  
item의 변경으로인한  
전체 길이가 변경으로  
add, update정도.
- `__setitem__`, `__delitem__`,  
`clear`, `pop`, `popitem`,  
`setdefault`, `update`가 있는데  
`update`, `pop`등  
변경가능한 method가 보임.
- dict가 이 class의 하위.

## Sequence

- `Collection`의 subclass로  
`Collection`에서 순서가 생김.
- `__getitem__`, `index`, `count`,  
`__contains__`, `__iter__`,  
`__reversed__`등.
- mutable이 따로있는거보면  
기본적으로는 변경 불가능인듯.  
read-only sequence라고 써있기도 하고.
- 사이에 다른 class를 끼고 있다해도   
대부분이 최종적으로는 여기 속하는데  
`str`, `memoryview`,`tuple`,  
`list`, `bytes`,`bytearray`가 있음.

### Common Sequence Operations

- sequence에서 공통으로 가능한 연산
- 얼핏 건너뛰는경우가 있어서  
길더라도 보는게 좋아보임.
- 이 외에는 class별로 따로 구현된것.
- 우선순위는 위부터 오름차순으로  
s, t는 sequence, n,i,j,k는 정수,  
x는 sequence에서 요구하는  
형과 값제한을 만족하는 임의의 객체 일때
    
    | Operation | Result | Notes |
    | --- | --- | --- |
    | x in s | True if s have x | (1) |
    | x not in s | False if s have x | (1) |
    | s + t | concat s with t | (6)(7) |
    | s \* n or n \* s | concat n times of s | (2)(7) |
    | s[i] | item at index ‘i’ | (3) |
    | s[i:j] | slice of s from i to j | (3)(4) |
    | s[i:j:k] | slice of s from i to j with step k | (3)(5) |
    | len(s) | length of s |  |
    | min(s) | smallest item of s |  |
    | max(s) | largest item of s |  |
    | s.index(x[, i[, j]]) | index of the first occurrence of x in s <br>(at or after index i and before index j) | (8) |
    | s.count(x) | total number of occurrences of x in s |  |

  1. in은 일반적으로  
  Membership Operator로 쓰이지만  
  str, bytes, bytearray등은  
  sub sequence도 찾을 수 있음.
      
      ```python
      "gg" in "eggs"
      True
      ```
  
  2. n은 seq의 앞이나 뒤에  
  곱하는 위치는 상관없음.  
  n이 0보다 작으면 0으로 처리.  
  이 방법으로 배열복사할 경우  
  값이 아닌 참조가 복사됨.
      
      ```python
      lists = [[]] * 3
      print(lists)
      lists[0].append(3)
      print(lists)
      ```
      
      ```python
      [[], [], []]
      [[3], [3], [3]]
      ```
      
      결과적으로 한 항목을 수정해도  
      해당 항목과 동일한 참조는  
      다 영향이 감.  
      이 점이 혼동을 적지않게 주나봄.  
      권장하는 방법은 for를 사용.
      
      ```python
      lists = [[] for i in range(3)]
      ```
      
  3. i, j가 음수일때  
  seq의 끝을 기준으로  
  len(seq)+i로 치환됨.  
  -0은 0. 
  4. i에서 j까지의 s slice는  
  인덱스가 k인 항목의  
  시퀀스로 정의되어 i <= k < j.  
  i 또는 j가 len(s)보다 크면 len(s).  
  i가 생략되었거나 None이면 0.  
  j가 생략되었거나 None이면 len(s).  
  i가 j보다 크거나 같으면 빈 slice.
  5. step k 가 있는  
  i 에서 j 까지의 slice는  
  0 <= n < (j-i)/k 인  
  x = i + n*k 의 항목.  
  i, i+k, i+2*k, i+3*k 등이며  
  j에 도달할 때 멈추지만  
  j를 포함하지 않음.  
  i 또는 j 가 생략되거나 None 이면,  
  해당 “끝” 값이 되는데  
  “끝”은 k 의 부호에 따름.   
  k 는 0일 수 없고 None 이면 1로 취급. 
  6. immutable sequence를 이어 붙이면  
  항상 새로운 객체를 생성하는데  
  이것은 반복적으로 이어붙이기를 해서  
  시퀀스를 만들 때 실행 시간이  
  시퀀스의 총 길이의 제곱에 비례.  
  선형 실행 시간 비용을 얻으려면  
  아래 대안 중 하나로 전환해야 한다.
      - str :  
      list로 만들고  
      str.join()을 사용하거나  
      io.StringIO 를 사용하는 방법.
      - bytes :  
      bytes.join() 또는  
      io.BytesIO를 사용하거나,  
      bytearray 객체를 사용.  
      bytearray 객체는 가변이고  
      overallocation 메커니즘을  
      갖고 있음.
      - tuple :  
      list로 사용.
      - 다른 type의 경우 관련 문서에서.
  7. range등의 일부 sequence는   
  특정 패턴을 따르는 항목만 지원하기 때문에  
  이어붙이기나 반복을 지원하지 않음.
  8. 모든 구현이  
  i와 j 전달을 지원하진 않지만  
  sequence의 하위 섹션검색을 효율적이게함.  
  추가 인수를 전달하는 것은  
  데이터를 복사하지 않고  
  반환된 인덱스가 슬라이스의 시작이 아닌  
  시퀀스의 시작에 상대적이라는 점만 제외하면  
  `s[i:j].index(x)`를 사용하는 것과 거의 동일.
    

### Immutable Sequence Types

- hash()가 지원됨.  
mutable sequence에서는 TypeError.
  
  ```python
  t = (1,2)
  print(hash(t))
  
  t = list(t)
  print(hash(t))
  ```
  
  ```python
  -3550055125485641917
  Traceback (most recent call last):
    File "F:\W\Python\test.py", line 52, in <module>
      print(hash(t))
  TypeError: unhashable type: 'list'
  ```
  
- hash()의 지원으로  
tuple같은 immutable sequence를  
dict 키로 사용하고  
set 및 frozenset에 저장할 수 있음.

### **Mutable Sequence Types**

- s mutablesequence,  
t는 임의의 iterable,  
x는 s가 요구하는 type, 값 제한을  
충족하는 임의의 객체일때
  
    | Operation | Result | Notes |
    | --- | --- | --- |
    | s[i] = x | item i of s is replaced by x |  |
    | s[i:j] = t | slice of s from i to j <br>is replaced by the contents <br>of the iterable t |  |
    | del s[i:j] | same as s[i:j] = [] |  |
    | s[i:j:k] = t | the elements of s[i:j:k] <br>are replaced by those of t | (1) |
    | del s[i:j:k] | removes the elements of s[i:j:k] from the list |  |
    | s.append(x) | appends x to the end of the sequence <br>(same as s[len(s):len(s)] = [x]) |  |
    | s.clear() | removes all items from s (same as del s[:]) | (5) |
    | s.copy() | creates a shallow copy of s (same as s[:]) | (5) |
    | s.extend(t) or s += t | extends s with the contents of t <br>(for the most part the same as s[len(s):len(s)] = t) |  |
    | s *= n | updates s with its contents repeated n times | (6) |
    | s.insert(i, x) | inserts x into s at the index given by i <br>(same as s[i:i] = [x]) |  |
    | s.pop() or s.pop(i) | retrieves the item at i and also removes it from s | (2) |
    | s.remove(x) | remove the first item from s where s[i] is equal to x | (3) |
    | s.reverse() | reverses the items of s in place | (4) |
    
  1. t는 대체하는 slice와 길이가 같아야함.
  2. i는 default값이 -1.  
  기본적으로 마지막값을 제거/반환
  3. x가 없으면 ValueError
  4. 큰 sequence에서 공간 절약을 위해  
  제자리에서 수정함.  
  return이 None이고, 원본을 변경.
  5. slice 연산을 지원하지 않는  
  dic및 set등의 가변 컨테이너와의  
  인터페이스 일관성을 위해  
  `clear()` 및 `copy()`가 추가됨.  
  `copy()`는 MutableSequence의  
  일부가 아니지만, 대부분의 구체적인  
  MutableSequence class에서 제공됨.
  6. 값 n은 정수이거나  
  `__index__()`를 구현하는 객체.  
  n의 0 및 음수 값은 sequence를 삭제함.  
  sequence의 항목은 복사되지 않고  
  일반 시퀀스 연산에서  
  s * n과 동일하게 동작.

### Bytestring

- `Sequence`의 subclass.
- 일단 아무것도 없음.

## AbstractSet

- `Collection`의 subclass이고  
따로 있는게 아니라 `Set`을 이름바꿔 씀.
- `Set`이랑 `set`을 구별하려고 그런듯.  
`Set`는 `Collection`의 subclass,  
`set`는 `MutableSet`의 subclass
- 유한하고 이터러블한 컨테이너.
- `__contains__`, `__iter__`, `__len__`  
을 제외하고 구현이 되어있음.
- 비교 연산을 재 정의하려면  
`__le__`, `__ge__`만 하면 됨.
- 집합관련.
- `__contains__`, `__iter__`, `__len__`,  
`__le__`, `__lt__`, `__eq__`,  
`__ne__`, `__gt__`, `__ge__`,  
`__and__`, `__or__`, `__sub__`,  
`__xor__`, `isdisjoint`, `_hash`

### MutableSet

- `AbstractSet`의 subclass
- 유한하고 이터러블한 컨테이너.
- `__contains__`, `__iter__`, `__len__`,  
`add()`, `discard()`를 제외하고 구현.
- 비교를 재정의하려면  
`__le__`만 하면 됨.
- `__contains__`, `__iter__`, `__len__`,  
`add`, `discard`, `clear`,  
`pop`, `remove`, `__ior__`,   
`__iand__`, `__ixor__`, `__isub__`  
- `set`이 여기 하위.
