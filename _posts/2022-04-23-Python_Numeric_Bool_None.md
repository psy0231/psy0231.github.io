---
title: Numeric, Bool, None
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-04-23 00:00:00 +0900
categories: [Grind, Python]
tags: [python, numeric, bool, none]
---
## Numeric

- 대충 숫자 관련.
- `int`, `float`, `complex`가 있음.
- `bool`은 `int`하위.

### int

```python
a = 0
print("a =",a)
print("a type =",type(a))

c = 9999999999999999999999999999999999
print("c =",c)
print("c type =",type(c))

d = g*g*g*g*g*g*g*g*g*g
# print("d =",d)
print("d type =",type(d))
```

```python
a = 0
a type = <class 'int'>

c = 9999999999999999999999999999999999
c type = <class 'int'>

d type = <class 'int'>
```

- `int()`생성자로 생성 가능.  
0으로 생성됨.
- 정수형
- 정밀도가 무제한임.  
다시말해 길이가 무제한   
이거 때문에 큰수 계산이 편함.

### float

```python
b = 3.15
print("b =",b)
print("b type =",type(b))

f = 1.1e-2
print("f =",f)
print("f type =",type(f))
```

```python
b = 3.15
b type = <class 'float'>

f = 0.011
f type = <class 'float'>
```

- `float()`로 생성 가능.   
0.0으로 생성됨.
- `float`은 C에서의  double로 구현됨.
- 지수표현으로 쓰면 기본이 `float`
- `float` 의 정밀도 및  
내부 표현에 대한 정보는  
`sys.float_info`로 볼 수 있음.   
이게 HW특성에따라 달라지는가봄.
- `float`는 항상 정확도와  
근사값계산관련 문제가 있는데  
나중에 필요하면 여기.

### complex

```python
e = 4+3j
print("e =",e)
print("e type =",type(e))
print("e real =", e.real)
print("e image =", e.imag)
```

```python
e = (4+3j)
e type = <class 'complex'>
e real = 4.0
e image = 3.0
```

- `complex()`로 생성됨.  
0j로 생성됨.
- 복소수 부분은  
j, J, i를 쓰면됨.

## boolean

- int 하위에 있음.
- True, False
- not은 반대.
  - print(not True) => False
- 모든 object는  
if, while에서 사용하기 위해,   
and, or의 연산을 위해  
truth value가 판별됨.
- object는 기본적으로 True으로 간주.   
단, 다음의 경우는 False
  - class의 \__bool__()이 False를 return.
  - class의 \__len__()이 0을 return.
- built-in objects중에 false인것들
  - false로 정의된 상수 : None, False
  - 숫자 0 : 0, 0.0, 0j, Decimal(0), Fraction(0, 1)
  - 비어있는 sequences, collections : '', (), [], {}, set(), range(0)

## None

- `Null` 대신에`None` 사용
- return이 없는 method가 return함.
- 특별한건 없음.