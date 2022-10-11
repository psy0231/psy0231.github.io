---
title: Python 16 - Built-In Functions
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-10-11 12:00:00 +0900
categories: [Grind, Python]
tags: [python, built-in functions]
---

## built-in function

- [docs](https://docs.python.org/ko/3/library/functions.html)
- 내장함수라고도 하고  
import없이 쓸 수 있는 method들

## abs(x)

- 절댓값

```python
i = -10
j = -13-4j
k = -3.14

print(abs(i))
print(abs(j))
print(abs(k))
```

```python
10
13.601470508735444 
3.14
```

## aiter(async_iterable)

- async iterable에 대한 async iterator return
- `x.__aiter__()` 랑 동일.

## all(iterable)

- iterable이 비어있거나  
모든 element가 참일경우 true return.
    
    ```python
    l = [1,2,3]
    print(all(l))
    
    l = [1,0,3]
    print(all(l))
    
    l = [1,'',3]
    print(all(l))
    
    l = [1,None,3]
    print(all(l))
    
    l = []
    print(all(l))
    ```
    
    ```python
    True
    False
    False
    False
    True
    ```
    
- `0`, `‘’`, `None`이 걸러짐

## any(iterable)

- iterable에 element가 하나라도 참일경우 true return.  
비어있는경우 false
    
    ```python
    l = [1,2,3]
    print(any(l))
    
    l = [1,0,3]
    print(any(l))
    
    l = [1,'',3]
    print(any(l))
    
    l = [1,None,3]
    print(any(l))
    
    l = []
    print(any(l))
    ```
    
    ```python
    True
    True
    True
    True
    False
    ```
    

## anext(asunc_iterator[, default])

- next의 async
- 기다리면 지정된 async iterator에서 다음 항목을 reutrn.
- next가 없는경우 default인데 이 경우는 지정해줘야함.  
아니면 StopAsyncIteration.
- async_iterator의 `x.__anext__()` 와 같음.

## ascii(object)

- 해당 문자열이 ascii에 있으면 출력  
없으면  유니코드로 표현
- 유니코드는 prefix로 `\u`를 붙여서 표현

```python
a = ascii('a가Bㅏ!@#')
print(a)
```

```python
'a\uac00B\u314f!@#'
```

## bin(x)

- 정수를 이진 문자열로 번환
- prefix로 `0b`가 붙음
- prefix는`format()`으로 수정할 수 있음

```python
a = bin(5)
print(a)
a = format(5, '#b') 
print(a)
a = format(5, 'b')
print(a)
a = f'{5:#b}' 
print(a)
a = f'{5:b}'
print(a) 
```
```
0b101
0b101
101
0b101
101
```

## bool([x])

- `True`또는 `False`return

```python
a = bool(1>2)
print(a)
a = bool(1)
print(a)
a = bool()
print(a)
a = bool(0)
print(a)
a = bool('')
print(a)
a = bool('false')
print(a)
```

```python
False
True
False
False
False
True
```

## breakpoint(*args, **kws)

- ide에서 볼수 있는 debug모드처럼  
중단점을 설정할 수 있음
- pdb(python debugger)를 호출한다.
- pdb는 내용이 길어 나중에 따로 정리하기로.

## bytearray([source[, encoding[, errors]]])

- byte배열 생성
- 가변 시퀀스
- bytes의 가변형으로 bytes의 특징과   
가변시퀀스형의 특징을 갖고있음
- bytearray의 요소에는 정수(int)를 할당한다.  
문자를 넣고 싶으면 ord를 사용해  
문자의 ASCII 코드(정수)를 넣는다.
- bytearray(num)의 num으로 길이만큼 생성하며  
0으로 초기화되어있음

## bytes([source[, encoding[, errors]]])

- bytearray에서 수정만 안됨
- `str.encode()`를 하거나 str앞에 b를 붙임
    
    ```
    s = 'asdf'
    print(s.encode())
    
    by = b'1234'
    ```
    
- encode기본값은 UTF-8
    
    ```python
    print('hell로'.encode('euc-kr'))
    print('hell로'.encode('utf-8'))
    ```
    
    ```python
    b'hell\xb7\xce'
    b'hell\xeb\xa1\x9c'
    ```
    
- `decode()`는 bytes를 str로.
    
    ```python
    s1 = b'hell\xb7\xce'.decode('euc-kr')
    s2 = b'hell\xeb\xa1\x9c'.decode('utf-8')
    
    print(s1)
    print(type(s1))
    ```
    

## callable(object)

- object 인자가 콜러블인 것처럼 보이면 `True`,  
그렇지 않으면 `False`.
- `True`일 때도 호출이 실패할 가능성이 있지만,  
`False`일 때 호출하면 반드시 실패.
- 클래스는 호출 가능임.(인스턴스 반환)
- 클래스에 `__call__()`  
 메서드가 있으면 인스턴스도 콜러블.

## chr(i)

- 유니코드 코드 포인트가 정수인  
문자를 나타내는 문자열 return.
- 유효 범위는 0에서 0x10FFFF까지.  
범위 밖은`ValueError` 발생.
- 예를 들어, `chr(97)` 은 `'a'` 이고,  
`chr(8364)` 는 `'€'`.
- `ord()` 의 반대.

## classmethod()

- 메서드를 클래스 메서드로 변환.
- `@classmethod`로 쓰나봄.
- method는 self를 가져야하는것처럼    
cls를 쓰는데 자세한건 필요하게되면.

## compile(*source*, *filename*, *mode*, *flags=0*, *dont_inherit=False*, *optimize=- 1*)

- 일반 문자열, 바이트열 또는 AST 객체등을 컴파일.
- `exec()`또는 `eval()`로 실행할 수 있음.
    - eval() : 지정 표현식 평가 후, Python 실행 구문이면 실행.
    - exec() : 지정 코드 (또는, 객체)를 실행.
    
    ```python
    codeInString = 'a = 8\nb=7\nsum=a+b\nprint("sum =",sum)'
    codeObject = compile(codeInString, 'sumstring', 'exec')
    
    exec(codeObject)
    
    # Output: 15
    ```
    

## complex([*real*[, *imag*]])

- 복소수 (Complex number)만들어줌

### complex(num1, num2)

- num1, 2 는 생략될 수 있는데  
각 상황마다 알아서 만들어줌
    
    ```python
    print(complex(2.1, 3.1))
    print(complex(2.1))
    print(complex())
    ```
    
    ```python
    (2.1+3.1j)
    (2.1+0j)
    0j
    ```
    

### complex(str)

- string를 argument로 할 경우
단 하나만 전달할 수 있음.
- real과 img와 부호에는 공백이 있으면 안됨
    
    ```python
    print(complex("2.1+3.1j"))
    ```
    
    ```python
    (2.1+3.1j)
    ```
    

## delattr(object, name)

- 객체의 해당 attr을 삭제
- `delattr(x, 'foobar')`는 
`del x.foobar`와 같음.
- 반대로 `setattr()`이 있음

## dict()

- 새 딕셔너리를 생성

## dir([object])

- arg가 없으면 현재 지역 스코프에 있는 이름들의 리스트
- arg가 있으면 해당 객체에 유효한 어트리뷰트들의 리스트
- 해당 클래스에 뭐있나 볼때 쓰던 그거.

## divmod(a,b)

- 복소수가 아닌 두 숫자를 arg로  
몫과 나머지로 구성된 숫자 쌍 반환.
- 혼합 피연산자 유형의 경우  
이진 산술 연산자에 대한 규칙이 적용.
    - 정수의 경우 (a // b, a % b)와 같음.
    - 부동 소수점의 경우 (q, a % b).   
    여기서 q는 일반적으로 math.floor(a / b)이지만  
    그보다 1 작을 수 있음.
    - 어쨌든 q * b + a % b는 a에 매우 가깝고,  
    a % b가 0이 아니면 b와 동일한 부호를 가지며  
    0 <= abs(a % b) < abs(b).

## enumerate(iteratable, start=0)

- iterable를 넣으면 열거(enumerate)객체 반환
    
    ```python
    seasons = ['Spring', 'Summer', 'Fall', 'Winter']
    print(list(enumerate(seasons)))
    ```
    
    ```python
    [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
    ```
    
- for할때 편한데
    
    ```python
    seasons = ['Spring', 'Summer', 'Fall', 'Winter']
    
    for n, item in enumerate(seasons):
        print(n,item,sep=":")
    
    for n in range(len(seasons)):
        print(n, seasons[n],sep=":")
    
    n = 0
    for item in seasons:
        print(n,item,sep=":")
        n+=1
    ```
    
    ```python
    0:Spring
    1:Summer
    2:Fall
    3:Winter
    ```
    
    세 경우 결과는 위와 같은데 
    enumerate가 훨씬 깔끔해짐
    

## eval(expression[, globals[, locals]])

- arg는 문자열 및 선택적 globals, locals이고  
return 은 표현된 식의 결과 라고 하는데
- 대충 str값을 넣으면 계산해주고 결과를 뱉는다.
- 그 대충이
    
    ```python
    print(eval('1+2'))
    
    a=10  
    print(eval('list(range(a))'))
    ```
    
    ```python
    3
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    ```
    
- import도 외부에서 먼저 할 수도,
eval안에서 할 수도 있음.
    
    ```python
    import calendar
    print(eval("calendar.prmonth(2022, 12)"))
    
    print(eval("__import__('random').randint(0,10)"))
    ```
    
    ```python
    December 2022
    Mo Tu We Th Fr Sa Su
              1  2  3  4
     5  6  7  8  9 10 11
    12 13 14 15 16 17 18
    19 20 21 22 23 24 25
    26 27 28 29 30 31
    None
    4
    ```
    

### adv

- 웹에서 데이터를 받는 경우,  
json, list인데 str로 받아지는 경우가 있는데
    
    ```python
    import pprint
    import requests
    import json
    
    url = 'https://api.upbit.com/v1/candles/minutes/1?market=KRW-BTC&count=10&to=2022-09-01T12:24:00Z'
    
    response = requests.get(url)
    print(response.text)
    ```
    
    이 경우 response.text는 str이다.
    
    ```python
    [{"market":"KRW-BTC","candle_date_time_utc":"2022-09-01T12:23:00","candle_date_time_kst":"2022-09-01T21:23:00","opening_price":27456000.00000000,"high_price":27470000.00000000,"low_price":27451000.00000000,"trade_price":27453000.00000000,"timestamp":1662035038118,"candle_acc_trade_price":62605542.26424000,"candle_acc_trade_volume":2.28024306,"unit":1},{"market":"KRW-BTC","candle_date_time_utc":"2022-09-01T12:22:00","candle_date_time_kst":"2022-09-01T21:22:00","opening_price":27466000.00000000,"high_price":27467000.00000000,"low_price":27456000.00000000,"trade_price":27457000.00000000,"timestamp":1662034979703,"candle_acc_trade_price":93717934.09229000,"candle_acc_trade_volume":3.41238910,"unit":1},{"market":"KRW-BTC","candle_date_time_utc":"2022-09-01T12:21:00","candle_date_time_kst":"2022-09-01T21:21:00","opening_price":27476000.00000000,"high_price":27478000.00000000,"low_price":27466000.00000000,"trade_price":27466000.00000000,"timestamp":1662034915054,"candle_acc_trade_price":89887982.31181000,"candle_acc_trade_volume":3.27204237,"unit":1},{"market":"KRW-BTC","candle_date_time_utc":"2022-09-01T12:20:00","candle_date_time_kst":"2022-09-01T21:20:00","opening_price":27505000.00000000,"high_price":27506000.00000000,"low_price":27476000.00000000,"trade_price":27478000.00000000,"timestamp":1662034859955,"candle_acc_trade_price":75471936.31508000,"candle_acc_trade_volume":2.74561004,"unit":1},{"market":"KRW-BTC","candle_date_time_utc":"2022-09-01T12:19:00","candle_date_time_kst":"2022-09-01T21:19:00","opening_price":27515000.00000000,"high_price":27515000.00000000,"low_price":27500000.00000000,"trade_price":27500000.00000000,"timestamp":1662034800050,"candle_acc_trade_price":293063146.74727000,"candle_acc_trade_volume":10.65468406,"unit":1}]
    ```
    
    이렇게 정리 안된 문자열로 받음.
    
    ```python
    "[{a:'1',b:2},{c:'3',d:4},{e:'5',f:6}]"
    ```
    
    짧게 줄여보면 이런식의 데이터인데  
    parsing도 애매해짐.
    
- 이럴 때 방법으로 json을 이용하면
    
    ```python
    res = json.loads(response.text)
    pprint.pprint(res)
    ```
    
    이런 방법도 있겠지만 
    
- eval을 사용하면
    
    ```python
    l = eval(response.text)
    print(l)
    print(type(l))
    ```
    
    이렇게 하면 str이 list로 나옴.
    

### disadv

- 편해보이긴한데 대부분 설명을 보면  
쓰지 않는걸 권장하는것같음.
- 이유는 자유도가 높은게 큼
    
    ```python
    print(eval(input()))
    ```
    
    실행 후 
    
    ```python
    __import__("os").listdir("E:")
    ```
    
    이런식의 맞는 입력만 들어가면  
    뭐든 할 수 있다는게 문제였음.
    

## exec(object[, globals[, locals]])

- eval은 식을 실했했다면  
exec는 문도 가능.
- eval은 그 줄에 끝나고 return이 있는데   
exec는 return이 없고 뒤로 이어질 수 있음
    
    ```python
    exec("a=0")
    exec("b=a+10")
    exec("c=b+10")
    eval("print(c)")
    ```
    
    ```python
    20
    ```
    
    이렇게 쓰일 수 있음
    
- 뿐만 아니라 문이 된다는 점은  
오만게 다 들어갈 수 있다는거임.
    
    ```python
    i = 1
    j = 2
    
    a = f'''
    def mtd(a,b):
        print(a+b)
    
    mtd({i},{j})
    '''
    
    exec(a)
    ```
    

## filter(function, iterable)

- iterable에서 function에 true인 element를 반환.
    
    ```python
    def fn(a):
        return a % 2 == 0
    
    l = list(range(0,10))
    
    nl1 = list(filter(fn, l))
    nl2 = list(filter(lambda i : i%2==0, l))
    nl3 = list(filter(None, l))
    
    print(l)
    print(nl1)
    print(nl2)
    print(nl3)
    ```
    
    ```python
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    [0, 2, 4, 6, 8]
    [0, 2, 4, 6, 8]
    [1, 2, 3, 4, 5, 6, 7, 8, 9]
    ```
    
- 첫 arg에는 function을 직접 넣거나  
보통 lambda를 쓰는게 많이보임
- 첫번쨰 arg에 None일 경우,  
false를 거르고 return.  
아마, `bool()`에서 걸러지는것들일듯.

## float([x])

- 숫자, 문자를 float으로 만들어줌  
기본은 0.0
- 조건이 많은데 귀찮. 알아서 쓰자.

## format(value[, format_apec])

- value를 format_spec에 따르는 형식화된표현으로 변환.
    
    ```python
    s = "some string {0} * {1} = {2}"
    
    print(s.format(1,2,1*2))
    
    s = "some string {info} : {mail} / {age}"
    
    print(s.format(info="name", mail="gmail", age="20"))
    ```
    
    ```python
    some string 1 * 2 = 2
    some string name : gmail / 20
    ```
    

## frozenset([iterable])

- 새 `frozenset` 객체 생성
- 선택적으로 iterable 에서 가져온 요소를 포함.
- 이거 썼을껄?…

## getattr(object, name[,default])

- 객체의 명명된 속성값 반환.
    
    ```python
    import random
    
    l = list(range(0,5))
    print(l)
    random.shuffle(l)
    print(l)
    getattr(random,'shuffle')(l)
    print(l)
    ```
    
    ```python
    [0, 1, 2, 3, 4]
    [1, 0, 3, 4, 2]
    [1, 3, 2, 0, 4]
    ```
    

## globals()

- 현재 모듈 네임스페이스를 구현하는 dic을 반환.
- 함수 내 코드의 경우 함수가 정의될 때 설정되며  
함수가 호출되는 위치에 관계없이 동일하게 유지.
    
    ```python
    import pprint
    
    g = globals()
    pprint.pprint(g)
    ```
    
    ```python
    {'__annotations__': {},
     '__builtins__': <module 'builtins' (built-in)>,
     '__cached__': None,
     '__doc__': None,
     '__file__': 'd:\\Workspace\\ETC\\PersonalProject\\Python\\temp\\main.py',
     '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x0000021543B86CD0>,
     '__name__': '__main__',
     '__package__': None,
     '__spec__': None,
     'g': <Recursion on dict with id=2290355238272>,
     'pprint': <module 'pprint' from 'C:\\Users\\psy02\\anaconda3\\lib\\pprint.py'>}
    ```
    

## hasattr(object, name)

- name(str)이 object 속성 중 하나면 true 아니면 false

## hash(object)

- 객체의 hash값.
- 정수값임.
- dic조회 중 키를 빨리 비교하는데 사용
- 같다고 비교되는 숫자 값은 같은 해시값으로 간주.  
1과 1.0의 경우와 같이 형이 다른 경우에도.

## help([object])

- 내장 대화형 도움말을 호출.
- 인자가 제공되지 않으면,  
인터프리터 콘솔에서 대화형 도움말 시작.
- 인자가 문자열이면,  
모듈, 함수, 클래스, 메서드, 키워드, 설명서 주제로 조회.
- 인자가 다른 종류의 객체면,   
객체에 대한 도움말 페이지가 만들짐.

## hex(x)

- 정수를 `0x`접두사가 붙은 16진수 문자열로 변환
- 접두사 수정 할 수 있음.
    
    ```python
    a = '%#x' % 255
    b = '%x' % 255 
    c = '%X' % 255
    print(a, b, c, sep=',')
    
    a = format(255, '#x') 
    b = format(255, 'x') 
    c = format(255, 'X')
    print(a, b, c, sep=',')
    
    a = f'{255:#x}' 
    b = f'{255:x}'
    c = f'{255:X}'
    print(a, b, c, sep=',')
    ```
    
    ```python
    0xff,ff,FF
    0xff,ff,FF
    0xff,ff,FF
    ```
    

## id(object)

- object의 id
- id는 object의 수명동안 유일하고 바뀌지 않음  
단, 수명이 겹치지 않는다면 중복이 가능함
- object의 메모리 주소를 의미함.
- id 인자로 감사 이벤트를 발생시킴..?

## input([prompt])

- 입력에서 한 줄 읽고 문자열로 변환.
- prompt가 있으면 개행문자 없이 출력.
- EOF를 읽으면 `EOFError`를 발생.
- `readline` 과,`input()` 을 함께 사용하여   
정교한 줄 편집과 히스토리 기능을 제공.
- `prompt` 인자로 감사 이벤트(auditing event)   
`builtins.input` 발생.
- `result` 인자로 감사 이벤트(auditing event)  
`builtins.input/result`발생.

## int([x]), int(x, base=10)

- 숫자, 문자열x를 정수로 만들어줌.
- arg가 없으면 0
- x에 정의된 `__int__()`, `__index__()`, `__trunc__()`를 return.
- base에 대한 셜명이 길게 있는데 뭐라는지 모르겠음.

## isinstance(object, classinfo)

- object가 classinfo의 인스턴스인지 판별.
- 범위는 classinfo의 직접, 간접, 가상의 하위클래스  
라고하면 좀 애매한데  
대충 관련된거면 어지간히 맞는듯함.
- classinfo가 type object의 여러 type의 Union Type이거나  
튜플(또는 재귀적으로, 다른 튜플)이면  
객체가 type중 하나의 인스턴스이어도 True를 반환. 
- classinfo가 type또는 type의 튜플 및  
그러한 튜플이 아닌 경우 TypeError 예외가 발생

```python
import random

class cls:
    pass

c = cls()
print(isinstance(c, cls))

r = random.Random()
print(isinstance(r,random.Random))

print(isinstance(c, random.Random))
print(isinstance(c, object))
```

```python
True
True
False
True
```

## issubclass(class, classinfo)

- isinstancefkd 비슷한데 하위클래스를 판별.
- 자신의 자신의 하위 class

```python
import random

class cls:
    pass

class cls2(cls):
    pass

print(issubclass(cls, cls2))
print(issubclass(cls2, cls))
print(issubclass(cls, cls))
```

```python
False
True
True
```

## iter(object[, sentinel])

- 반복자 객체 return.
- 첫 번째 arg는 두 번째 arg에 따라 다르게 해석됨.
- 두 번째 arg가 없으면  
객체는 `__iter__()` 메서드를 지원하는 컬렉션 객체이거나   
`__getitem__()` 메서드를 지원해야 함.  
이 둘 중 어느것도 지원하지 않으면 TypeError가 발생.
- 두 번째 arg인 sentinel이 제공되면   
object는 호출 가능한 개체여야 함.  
이 경우 `__next__()` 메서드에 대한 각 호출에 대해   
인수 없이 객체를 호출.  
반환된 값이 sentinel과 같으면 StopIteration이 발생,   
그렇지 않으면 값이 반환.
- iterable은 이 전에 collection이랑 비슷한 ..
- 종류로는 str, list, tuple, dict, set, range() 등등.
- 첫번째 arg만 있는 경우
    
    ```python
    i = iter(range(0,5))
    
    print(i.__next__())
    print(i.__next__())
    print(i.__next__())
    print(i.__next__())
    print(i.__next__())
    print(i.__next__())
    ```
    
    ```python
    0
    1
    2
    3
    4
    Traceback (most recent call last):
      File "f:\W\Python\test.py", line 9, in <module>
        print(i.__next__())
    StopIteration
    ```
    
- 두 번째 arg까지 있는 경우
    
    ```python
    import random
    
    i = iter(lambda : random.randint(0,5),2)
    
    for item in i:
        print(item, end=" ")
    
    print()
    
    while True:
        i = random.randint(0,5)
        if i == 2:
            break
        print(i,end=" ")
    ```
    
    ```python
    4 1 0 
    5 5 1 0 5 4 0 5 4
    ```
    
    대충 넘겼다가 쓰면서 확실히 달라짐을 느꼈는데 
    
    arg가 하나일때는 iterable object가 오고  
    arg가 두개 일때는 호출가능한 객체와  
    반복을 멈출 조건으로.
    

## len(s)

- 객체의 길이.
- 시퀀스 (str, bytes, tuple, list, range 등) 또는  
컬렉션 (dic, set,  frozen set 등)이 가능.
- sys.maxsize보다 긴 길이에서 OverflowError발생.

## list([iterable])

- 새 list 또는 형변환
- 가변 sequence형.

## locals()

- 현재 지역 심볼 테이블을 나타내는 딕셔너리
- 자유 변수는 함수 블록에서 호출될 때  
`locals()`에 의해 반환되지만  
클래스 블록에서는 반환되지 않음.
- 모듈 수준에서 `locals()`와 `globals()`는 같음.

## map(function, iterable)

- iterable에 function 을 적용한 후  
iterator을 return. 
- 추가 *iterable* 인자가 전달되면,  
function 은 그 수 만큼의 인자를 받아들여야 하고  
모든 이터러블에서 병렬로 제공되는 항목들에 적용. 
- 다중 이터러블의 경우,  
이터레이터는 가장 짧은 이터러블이 모두 소모되면 정지.
- 함수 입력이 이미 인자 튜플로 배치된 경우는,  
`itertools.starmap()` 참조.
- 아무튼, console에서 입력받은 str을  
int로 바꿀 때 쉬웠음.
    
    ```python
    l = '0 1 2 3 4'
    
    nl = list(map(int,l.split()))
    
    print(nl)
    ```
    
    ```python
    [0, 1, 2, 3, 4]
    ```
    

## max(iterable, *[, key, default]), max(arg1, arg2, * args[, key])

- 가장 큰 값의 항목
- 여러 항목이라면 가장 처음 항목.

## memoryview(object)

- 지정된 arg부터 만들어진 memoryview.

## min(iterable, *[,key, default]), min(arg1, arg2, *args[, key])

- 가장 작은 값의 항목
- 여러 항목이라면 가장 처음 항목.

## next()

- iter에서 `__next__()`를 썼는데
    
    ```python
    i = iter(range(0,5))
    
    print(next(i))
    print(next(i))
    print(next(i))
    print(next(i))
    print(next(i))
    print(next(i))
    ```
    
    next()가 `__next__()`를 호출하는거.
    

## object()

- 새로운 객체를 생성.
- object는 모든 클래스의 기반.
- 모든 Python 클래스의  인스턴스에 공통적인 메서드를 가짐.
- object는 __dict__가 없음.  
따라서 임의의 attribute를 대입할 수 없음.

## oct()

- 정수를 `0o`로 시작하는 8진수 문자열로 변환.  
결과는 올바른 파이썬 표현식입니다.
- format로 접두사 수정할 수 있음.

## open(file, mode='r', buffering=- 1, encoding=None, errors=None, newline=None, closefd=True, opener=None)

- file을 열고 해당 파일 객체 return.
- 파일을 열 수 없으면, `OSError` 가 발생.

## ord(c)

- 하나의 유니코드 문자를 나타내는 문자열이 주어지면  
해당 문자의 유니코드를 나타내는 정수를 return.
- 예를 들어, `ord('a')` 는 정수 `97` 이고,  
`ord('€')` (유로 기호)는 `8364`
- `chr()` 의 반대.

## pow(base, exp[,mod])

- base 의 exp 거듭제곱
- mod 가 있는 경우, base 의 exp 거듭제곱의 모듈.  
이 계산은 `pow(base, exp) % mod` 보다 더 빠름
- `pow(base, exp)` 는  `base**exp`랑 동일.

## print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)

- 텍스트 스트림 파일에 개체를 인쇄. 
- sep로 구분되고 뒤에 end로 끝남.
- sep, end, file, flush(있는 경우)는  
키워드 인수로 제공되어야 함.

## property(fget=None, fset=None, fdel=None, doc=None)

- 프로퍼티 어트리뷰트 return.
    
    ```python
    class C:
        def __init__(self):
            self._x = None
    
        def getx(self):
            print("getx")
            return self._x
    
        def setx(self, value):
            print("setx")
            self._x = value
    
        def delx(self):
            print("delx")
            del self._x
    
        x = property(getx, setx, delx, "I'm the 'x' property.")
    
    ic = C()
    ic.x = 1
    print(ic.x)
    
    del ic.x
    try :
        print(ic.x)
    except Exception as e:
        print(e)
    
    ic.x = 2
    print(ic.x)
    ```
    
    ```python
    setx
    getx
    1
    delx
    getx
    'C' object has no attribute '_x'
    setx
    getx
    2
    ```
    
    - property에 대해 x로 통일된 접근을 할 수 있음.
    - property에 대한 직접적인 접근을 막고  
    getter, setter같은 효과를 줄 수 있음.
- `@property`를 사용하면 더 편함.
    
    ```python
    class C:
        def __init__(self):
            self._x = None
    
        @property
        def x(self):
            """I'm the 'x' property."""
            return self._x
    
        @x.setter
        def x(self, value):
            self._x = value
    
        @x.deleter
        def x(self):
            del self._x
    ```
    
    - `@property`를 사용해 위와 동일하게 쓴것.
    - `@property`는 get에 대응되고  
    나머지는 데코레이터에 따름

## range(start, stop[, step])

- start, step는 생략될 수 있음  
stop는 단독으로 올 수 있고 필수임.
- 불변 시퀀스형.

## repr(object)

- 객체의 인쇄 가능한 표현을 포함하는 문자열.
- 많은 유형의 경우 이 함수는  
`eval()`에 전달될 때  
동일한 값을 가진 객체를 생성하는  
문자열을 반환.  
다시말해 `eval(repr(object))`가 됨
- 그렇지 않으면 개체의 이름과 주소를 포함하는  
추가 정보와 함께 개체 유형의 이름을 포함하는  
꺾쇠 괄호로 묶인 문자열.
- 클래스는 `__**repr__**()` 메서드를 정의하여  
이 함수가 인스턴스에 대해 반환하는 것을 제어할 수 있다.  
`sys.displayhook()`에 액세스할 수 없는 경우  
이 함수는 `RuntimeError`를 발생.
- str()이 비공식적으로 ... repr()이 공식적으로 ...  
하는 설명이 많은데 귀찮.

## reversed(seq)

- reversed iterator

## round(number[, ndigits])

- 반올림
- ndigits가 생략되거나 None면 
가장 가까운 정수로.
- x.y에서 .을 index 0 으로 하고  
y 방향이 양수 x가 음수이고  
.에서 멀어질수록 절댓값이 커짐  
ndigit는 이 전 자이에서 이 자리로 반올림.
    
    ```python
    a = 123.456
    
    print(round(a))
    print(round(a,1))
    print(round(a,2))
    print(round(a,3))
    print(round(a,4))
    
    print(round(a,-1))
    print(round(a,-2))
    ```
    
    ```python
    123
    123.5
    123.46
    123.456
    123.456
    120.0
    100.0
    ```
    
- float에서 round를 쓸때 정확하지 않은것같음.

## set([iterable])

- set객체 생성
- 선택적으로 iterable에세 가져온 요소를 갖는다.

## setattr(object, name, value)

- `getarttr()`의 반대.
- 기존 속성유뮤 상관없이 지정할 수 있음
    
    ```python
    class C:
        def init(self):
            self.x = None
    
    c = C()
    
    setattr(c,"x", "str")
    print(c.x)
    setattr(c, "y", "new attr")
    print(c.y)
    ```
    
    ```python
    str
    new attr
    ```
    
- setattr(x, 'foobar', 123)은  
x.foobar = 123과 동일.

## slice(start, stop[, step])

- start, step는 생략가능 stop는 필수
- `range(start, stop, step)`로 지정된  
인덱스 집합을 나타내는 슬라이스 객체를 return.

## sorted(iterable, /, *, key=None, reverse=False)

- iterable을 새로 정렬한 리스트를 return.
    
    ```python
    l = ['eeeeeeee','1','1111', '222222','3333', 'a']
    print(sorted(l, key=len))
    print(sorted(l, key=lambda x:ord(x[0])))
    print(l)
    ```
    
    ```python
    ['1', 'a', '1111', '3333', '222222', 'eeeeeeee']
    ['1', '1111', '222222', '3333', 'a', 'eeeeeeee']
    ['eeeeeeee', '1', '1111', '222222', '3333', 'a']
    ```
    
- 원본은 바뀌지 않음  
같은 기능의 `sort()`는 원본을 변경.
- 안정적이 보장되는데 같다고 비교되는 요소의  
상대적 순서를 변경하지 않으면 안정적임.  
위 경우에 key=len에서 ‘1’,’a’의 상대적 위치가 변하지 않는것.  
여전히 ‘1’이 앞 ‘a’가 뒤
- 정렬 기준은 key  
단, key 이외는 정렬에 영향을 주지 않음.
    
    ```python
    data = [(1,200),(1,100),(2,300), (2,400)]
    
    print(sorted(data, key=lambda data:data[0]))
    ```
    
    apple, airplane을 a로 정렬했을 떄 
    a다음 글자로 정렬해주는게 없음.
    

## staticmethod()

- 메서드를 정적 메서드로 변환.
- `@staticmethod`로 씀.
- 정적 메서드를 알아야지..

## str(object=’’), str(object=b’’, encoding=’utf-8’, errors=’strict’)

- object의 str버전.
- repr의 반대인듯.
- 문자열 str이 아님.

## sum(iterable, /, start=0)

- start 및 iterable의 항목들을 왼쪽에서 오른쪽으로 합한 결과
- iterable는 일반적으로 숫자이고 시작값은 문자불가.  
라는데 숫자만 되던데?
- 아무튼 비슷한거로는
    - `''.join(sequence)` : 문자열 sequence 연결
    - `math.fsum()` : 부동소수점 값 합
    - `itertools.chain()` : iterable 연결

## super([type[, object-or-type]])

- 메서드 호출을 부모나 형제 클래스 *type*에  
위임하는 프락시 객체 return.
- 클래스에서 재정의된 상속 된 메서드를 액세스할 때 유용.
- object-or-type은 검색할 메서드 확인 순서를 결정.  
검색은 유형 바로 뒤에 있는 클래스에서 시작.

## tuple([iterable])

- 새 tuple return
- 불변 시퀀스형

## type(object), type(name, bases, dict, **kwds)

- 인자 하나의 경우
    
    object의 type를 return.  
    return 값은 type형 객체이며 일반적으로  
    object.__class__와 같음.
    
    객체 type을 검사할 때 `isinstance()`가 권장되는데  
    subclass를 고려하기 때문.
    
- 인자 세개의 경우
    
    새 type 객체 생성으로 동적인 형태. 
    
    - name은 클래스 이름이고  
    `__name__()`어트리뷰트가 됨.
    - bases튜플은 베이스 클래스들을 포함하고    
    `__bases__()`어트리뷰트가 됨.  
    비어 있으면, `object`
    - dict는 클래스 바디의  
    어트리뷰트와 메서드 정의.  
    `__dict__()`어트리뷰트가 되기 전에   
    복사되거나 감싸질 수 있음.
- 다음 두 문장은 같은 동작을 함:
    
    ```python
    class X:
        a = 1
    x = X()
    print(x.a)
    
    y = type('Y', (), dict(a=1))
    print(getattr(y,'a'))
    ```
    
    ```python
    1
    1
    ```
    
    같은 X, Y class 를 일반적인방법과  
    type를 써서 생성할떄 차이.
    

## vars([object])

- 객체의 `__dict__()` 속성을 반환.
    - 모듈, 클래스, 인스턴스 등
    - `__dict__()` 속성이 있으면 됨
- 모듈 및 인스턴스와 같은 객체는  
업데이트 가능한 `__dict__()` 어트리뷰트를 갖지만  
다른 객체는 `__dict__()` 어트리뷰트에 쓰기 제한을 가질 수 있다.  
예를 들어, 클래스는 직접적인 딕셔너리 갱신을 방지하기 위해  
`types.MappingProxyType` 를 사용.
- 인수가 없으면 vars()는 locals()처럼 작동.  
locals dic에 대한 업데이트는 무시되므로  
locals dic은 읽기에만 유용.
- object에 `__dict__()` 속성이 없는 경우 TypeError
    - 예: 해당 클래스가 `__slots__()` 속성을 정의하는 경우
- 객체 속성볼때 쓰나봄
    
    ```python
    import pprint
    class cls:
        data1=None
        data2=2
        data3='data'
        def __init__(self) -> None:
            pass
    
        def temp():
            pass
    
    pprint.pprint(cls )
    print()
    pprint.pprint(vars(cls))
    print()
    pprint.pprint(vars())
    print()
    pprint.pprint(locals())
    ```
    
    ```python
    <class '__main__.cls'>
    
    mappingproxy({'__dict__': <attribute '__dict__' of 'cls' objects>,
                  '__doc__': None,
                  '__init__': <function cls.__init__ at 0x000001E27D9D5AF0>,
                  '__module__': '__main__',
                  '__weakref__': <attribute '__weakref__' of 'cls' objects>,
                  'data1': None,
                  'data2': 2,
                  'data3': 'data',
                  'temp': <function cls.temp at 0x000001E27D9E7280>})
    
    {'__annotations__': {},
     '__builtins__': <module 'builtins' (built-in)>,
     '__cached__': None,
     '__doc__': None,
     '__file__': 'f:\\W\\Python\\test.py',
     '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x000001E27D26D3A0>,
     '__name__': '__main__',
     '__package__': None,
     '__spec__': None,
     'cls': <class '__main__.cls'>,
     'pprint': <module 'pprint' from 'C:\\Users\\psy02\\anaconda3\\lib\\pprint.py'>}
    
    {'__annotations__': {},
     '__builtins__': <module 'builtins' (built-in)>,
     '__cached__': None,
     '__doc__': None,
     '__file__': 'f:\\W\\Python\\test.py',
     '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x000001E27D26D3A0>,
     '__name__': '__main__',
     '__package__': None,
     '__spec__': None,
     'cls': <class '__main__.cls'>,
     'pprint': <module 'pprint' from 'C:\\Users\\psy02\\anaconda3\\lib\\pprint.py'>}
    ```
    

## zip(*iterables, strict=False)

- 여러 iterable을 병렬로  
각 item의 튜플 생성
- 같은 위치 item끼리 tuple로 합치고  
iterable를 return해줌.
    
    ```python
    z = zip([1, 2, 3], ['sugar', 'spice', 'everything nice'])
    print(z.__next__())
    ```
    
    ```python
    (1, 'sugar')
    ```
    
- transpose, 그러니까 전치 행렬 만들때도 쓰임.
    
    ```python
    l = [[1,2,3],['a','b','c']]
    print(l)
    
    la = list(zip(*l))
    print(la)
    ```
    
    ```python
    [[1, 2, 3], ['a', 'b', 'c']]
    [(1, 'a'), (2, 'b'), (3, 'c')]
    ```
    
    그림으로 보면 
    
    ```python
    
    [1, 2, 3]          (1, 'a')
    ['a', 'b', 'c']    (2, 'b')
                       (3, 'c')
    ```
    
    이게 sql로 했을 pivot이었나 그런거 쓰고  
    복잡했는데 쉽게 끝나는듯.
    
- lazy 특성이 있음.  
LINQ처럼 실제 실행 할 때까지 안함.
- 전달된 iterable의 길이가 다를경우
    
    가장 짧은 길이에 맞추거나
    
    ```python
    print(list(zip([0,1,2], ['fee', 'fi', 'fo', 'fum'])))
    ```
    
    ```python
    [(0, 'fee'), (1, 'fi'), (2, 'fo')]
    ```
    
    3.10 이상에서 strict=True옵션으로  
    ValueError을 일으키거나
    
    iteratools를 사용해  
    동일한 길이로 맞추거나
    
    ```python
    import itertools as it
    
    print(list(it.zip_longest([0,1,2], ['fee', 'fi', 'fo', 'fum'])))
    ```
    
    ```python
    [(0, 'fee'), (1, 'fi'), (2, 'fo'), (None, 'fum')]
    ```
    

## __import__()

- 일반적으로 필요하지 않은 고급 함수.
- 이 함수는 `import` 문에 의해 호출.
- `import` 문의 의미를 변경하기 위해 대체함.  
`builtins` 모듈을 임포트하고   
`builtins .__ import__` 에 대입.
- 그러나 그렇게 하지 말 것을 강하게 권고하는데,  
같은 기능의 임포트 훅([PEP 302])으로 더 간단하고  
기본 임포트 구현이 사용될 것이라고 가정하는 코드들과  
문제를 일으키지 않기 때문.
- `__import__()` 의 직접 사용 역시 피하고  
`importlib.import_module()` 을 권장함.