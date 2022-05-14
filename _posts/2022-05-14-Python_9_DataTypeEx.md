---
title: Python 9 - DataType Extra
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-05-14 00:00:00 +0900
categories: [Grind, Python]
tags: [python, datatype, collection, sequence]
---

## extra?

- 약간 뒤죽박죽이긴한데  
이 앞에 collection까지 해서  
data type이 끝난다.
- collection이 적합한가를 떠나  
일단 썼던건 set, mepping등  
다른 type도 섞여 있어서였는데  
아직도 다른 표현은 못찾겠음
- 아무튼, 
data type, collection에서  
빼먹은?것들.
- 일단 전체적으로 보자면
  
  ```
  DataType
      |
      | --- |
            | - Null -------------| - None
            |
            | - Numeric --------- |
            |                     | - int
            |                     | - float
            |                     | - complex
            |        
            | - Sequence -------- |
            |                     | - list (mutable)
            |                     | - tuple (immutable)
            |                     | - range (mutable)
            |
            | - Text Sequence  ---| - str(immutable)
            |
            | - Binary Sequence - |
            |                     | - bytes(immutable)
            |                     | - bytearray(mutable)
            |                     | - memoryview
            |
            | - Set ------------- |
            |                     | - set(mutable)
            |                     | - frozenset(immutable)
            |
            | - Mapping ----------| - dict(mutable)
  ```
  
  | type            |                      |
  | --------------- | ----------------     |
  | Null            | None                 |
  | Numeric         | int                  |
  |                 | float                |
  |                 | complex              |
  | Sequence        | list(mutable)        |
  |                 | tuple(immutable)     |
  |                 | range(mutable)       |
  | Text Sequence   | str(immutable)       |
  | Binary Sequence | bytes(immutable)     |
  |                 | bytearray(mutable)   |
  |                 | memoryview           |
  | Set             | set                  |
  |                 | frozenset(immutable) |
  | Mapping         | dict(mutable)        |
  
    
- 이 내용은 [Built-in Types]([https://docs.python.org/3/library/stdtypes.html#](https://docs.python.org/3/library/stdtypes.html#)) 를 참조하는데  
잘보면 내용이 조금 다른 부분이 있긴함
- 다만, 내가 한 정리는 대충 한거니까  
저 문서를 항상 참조해야함.
- 이 다음 내용들도 간략하게 쓰고  
자세히는 위 docs를 봐야함.

---

## sequence형

- 여기 속한 type는  
list, tuple, range만 써있는데  
이름에 sequence를 쓰는  
다른 type들도 포함할 수 있는지는  
정확치 않음
- 아래 쓴 내용들은 원래  
해당 sequence type에서는  
있을수도 있고 없을수도 있는데  
없는건 여기에 쓸걸 염두해둔것.

### 공통 sequence 연산

- mutable, immutable상관없이 지원

  |연산                   |결과
  |---                    |---
  |x in s                 |s에 x 가 있으면 True
  |x not in s             |s에 x가 없으면 True
  |s + t                  |s 와 t연결
  |s * n 또는 n * s       |s를 n번 copy
  |s[i]                   |i 번째 
  |s[i:j]                 |i <= item < j
  |s[i:j:k]               |i <= item < j, k step
  |len(s)                 |길이
  |min(s)                 |가장 작은 항목
  |max(s)                 |가장 큰 항목
  |s.index(x[, i[, j]])   |item index
  |s.count(x)             |s 등장하는 x 의 총수

### 가변 sequence 연산

|연산                       |결과
|---                        |---
|s[i] = x                   |s 의 항목 i 를 x 로
|s[i:j] = t                 |조건의 영역이 이터러블 t 로 대체
|del s[i:j]                 |조건 영역 삭제
|s[i:j:k] = t               |조건의 영역이 이터러블 t 로 대체
|del s[i:j:k]               |리스트에서 조건의 항목 제거
|s.append(x)                |s 뒤에 x를 ___추가___
|s.clear()                  |모든 항목을 제거
|s.copy()                   |얕은 복사
|s.extend(t) 또는 s += t    |s 뒤로 t ___붙임___
|s *= n                     |n 번 반복되도록 s 를 갱신
|s.insert(i, x)             |i에 x  삽입합니다
|s.pop() or s.pop(i)        |i 항목을 꺼내고 s 에서 제거
|s.remove(x)                |x가 첫 번째로 나타나는 항목 제거
|s.reverse()                |역정렬

---
## str modify

- str은 immutable임.
  
  ```python
  s = "asdfa"
  s[0] = "z"
  ```
  ```python
  Traceback (most recent call last):
    File , line 2, in <module>
      s[0] = "z"
  TypeError: 'str' object does not support item assignment
  ```

- 수정하는 방법으로  
replace, translate가 있는데
    
    ```python
    s = "asdfa"
    
    s1 = s.replace("a", "z")
    print(s1)
    
    s2 = s.translate(s.maketrans("a", "z"))
    ```    
    - 문제는 이 경우 같은 글자는 다바뀜.
    - 어떤 특정 위치값을 수정못함.

### list 사용

```python
s = "asdfa"
l = list(s)
l[0] = "z"
s = "".join(l)
print(s)
```

### bytearray 사용

```python
s = "asdfa"
ba = bytearray(s, "utf-8")
ba[0] = 0x00
print(ba)
ba[0] = ord('X')
print(ba)
ba[0:3] = b"XYZ"
print(ba)
s1 = ba.decode("utf-8")
print(s1)
```

- 한글자 바꿀 때 ```ord(’X’)```를 썼는데  
```b'X'```쓰면 exception

### slicing사용

```python
s = "asdfa"
s1 = s[1:4]
print(s1)
print(type(s1))

new_s = 'new'+s1+'end'
print(new_s)
```

- 이 때 slicing 결과를 str인데  
원본 type를 따라감.
    - list slicing 하면 list.

---