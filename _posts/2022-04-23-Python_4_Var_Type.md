---
title: Python 4 - Variable & Data Type 2
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-04-22 00:00:00 +0900
categories: [Grind, Python]
tags: [python, variable, data type]
---

## string
- 문자열 처리가 다른 언어보다 간단하고 직관적
- 문자 표시로 ```" "```나 ```' '```나 상관없는듯.  
단, 짝은 맞게.. 이 관련은 앞에 pep8에도 있음.
- 블록 전체를 string으로 하려면  
```''' '''```하면 되는데 
이게 docstring아닌지?
- 문자 * 숫자로 문자 반복을 할 수 있음
    - print(“0” * 3) 하면 000

```python
str = "asdf"
print(str)
str2 = 'asdf'
print(str2)

str3 = """
asdf
areg
dtuykj
"""
print(str3)
```

## string - indexing, slicing

### Indexing

- string를 배열처럼 처리하는거
- 다른 언어랑 같음.
- 특이한점은  
(-)로 indexing이 가능한데  
뒤에서부터 시작해서  
거꾸로 검색해준다.  
이건 slicing에서 더.  
- 배열처럼 취급은 같음

### Slicing

- ```[]```안에 ```:``` 로 조건을 줄 수 있음
  - ```[start : end : step]```
- start
  - 시작 index
  - 0인경우 생략 가능
  - 뒤에서셀경우 (-)
- end
  - 끝 + 1 index
  - 실제 끝나는곳은 이 앞 index.
  - 끝을 나타낼 때 생략 가능
  - 뒤에서 셀 경우 (-)
- step
  - 위 두 조건안에서 수를 세는 방법
  - for문으로 칠 때 i++역할
  - (-)가 붙으면 뒤에서 앞방향으로.
- start ≤ index < end 인듯.
  
  ```csharp
  for(int i = start; i < end; i+=step){}
  ```
  
- ex)
  
  ```python
  str = "0123456789"
  
  print(1,str[0:5])
  print(2,str[3:8])
  print(3,str[0:])
  print(4,str[:5])
  print(4,str[:-6])
  print(5,str[:])
  print(6,str[::2])
  print(7,str[::-1])
  print(8,str[::-2])
  print(9,str[-1::-2])
  print(10,str[-1::])
  print(11,str[-1::-1])
  print(12,str[-1::-2])
  print(13,str[-1::-3])
  print(14,str[-1::-4])
  print(15,str[-1::-5])
  
  ```
  
  ```
  1 01234
  2 34567
  3 0123456789
  4 01234
  4 0123
  5 0123456789
  6 02468
  7 9876543210
  8 97531
  9 97531
  10 9
  11 9876543210
  12 97531
  13 9630
  14 951
  15 94
  ```
  

## string - 처리
- ex)
  
  ```python
  
  str = "The quick brown fox jumps over the lazy dog"
  
  print(str.upper())
  print(str.lower())
  print(str.capitalize())
  print(str.title())
  print(str.swapcase())
  print(str.replace("quick", "super"))
  print(str.find("fox"))
  print(str.count("o"))
  print(str.startswith("The"))
  print(str.endswith("dog"))
  print(str.isalnum()) print(str.isalpha())
  print(str.isdigit())
  print(str.isnumeric())
  print(str.isspace())
  print(str.istitle())
  print(str.isupper())
  print(str.islower())
  print(str.isidentifier())
  print(str.isprintable())
  print(str.isdecimal())
  print(len(str))
  print(str.find("C#"))
  print(str.index("C#"))
  
  ```
  
  ```
  THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG
  the quick brown fox jumps over the lazy dog
  The quick brown fox jumps over the lazy dog
  The Quick Brown Fox Jumps Over The Lazy Dog
  tHE QUICK BROWN FOX JUMPS OVER THE LAZY DOG
  The super brown fox jumps over the lazy dog
  16
  4
  True
  True
  False
  False
  False
  False
  False
  False
  False
  False
  False
  True
  False
  43
  -1
  Traceback (most recent call last):
    File "d:\\Workspace\\ETC\\PersonalProject\\Python\\NDCoding\\Basic\\PythonWorkspace\\practice.py", line 27, in <module>
      print(str.index("C#"))
  ValueError: substring not found
  ```
  
  - 많다. 적당히 쓰면 될것같다.
  - find랑 index는 문자(열)을 찾아주는데  
  차이점은 만약 찾는 문자열이 없는 경우  
  find는 -1반환, index는 exception

## string - format

- 이쪽은 나중에 입출력할때 자주씀.

### %

- c처럼? 하는방식  
- 문자열에 %?로 표시하고  
문자열 끝에 % ? 로 값을 넣는데  
여러개일 경우 % (,) 로 표현
- d는 정수 f는 실수 s는 string
- 대충 %s에 때려박아도 됨
- ex)
  
  ```python
  print("some arguments: one, two, three")
  print("some arguments: %d, two, three" % 1)
  print("some arguments: %d, %f, three" % (1, 1.1))
  print("some arguments: %d, %f, %s" % (1, 1.1, "three"))
  print("some arguments: %s, %s, %s" % (1, 1.1, "three"))
  
  ```
  

### {}.format

- C# 비슷한거
- 문자열에 {}로 표시해놓고  
문자열 끝에 .format()를 붙이고  
그 안에 값을 넣음
- {}가 하나 이상일 경우  
.format 인자의 index를  
{}안에 명시해서 쓸 수 있음
- ex)
  ```python
  print("some arguments: one, two, three")
  print("some arguments: {}, two, three".format(1))
  print("some arguments: {0}, {1}, three".format(1, 1.1))
  print("some arguments: {2}, {0}, {1}".format(1, 1.1, "three"))
  ```
  
- 변수화
  .format안에서 변수처럼 명시하면  
  {}안에 그 이름을 쓸 수 있음
  
  ```python
  print("some arguments: one, two, three")

  print("some arguments: {arg3}, {arg2}, {arg1}".format(arg1=1, arg2=1.1, arg3="three"))
  ```

- 외부 변수 사용
python 3.6이상 지원
  
  ```python
  arg1 = 1
  arg2 = 1.1
  arg3 = "three"
  print("some arguments: one, two, three")
  print(f"some arguments: {arg3}, {arg2}, {arg1}")
  ```
  

### Excape sequence
- \랑 엮여서 예약된거

  | Escape Character | explain |
  |----------------- | ------- |
  | \\               | 백슬래시, \
  |\'                |작은따옴표, Single quote, '
  |\"                |큰따옴표, Double quote, "
  |\a                |벨, ASCII Bell, BEL
  |\b                |백스페이스, ASCII Backspace, BS
  |\f                |폼피드, ASCII Formfeed, FF
  |\n                |새 줄, 개행 문자, ASCII Linefeed, LF
  |\r                |캐리지 리턴, ASCII Carriage Return, CR
  |\t                |탭 문자, ASCII Horizontal Tab, TAB
  |\v                |수직 탭, ASCII Vertical Tab, VT
  |\ooo              |\ 뒤에 8진수 숫자를 지정하여 ASCII 코드의 문자 표현
  |                  |예) '\141'은 'a'를 표현
  |\xhh              |\ 뒤에 16진수 숫자를 지정하여 ASCII 코드의 문자 표현
  |                  |예) '\x61'은 'a'를 표현
  |                  |ASCII 코드는 다음 URL 참조
  |                  |- Ascii Table
  |                  |  https://www.asciitable.com
  |\N{name}          |{ } 안에 문자 이름을 지정하여 유니코드의 문자 표현(파이썬 3.3이상)
  |                  |예) '\N{LINE FEED}'는 '\n'을 표현
  |                  |문자 이름은 다음 URL 참조
  |                  |- formal name aliases for Unicode characters
  |                  |  http://www.unicode.org/Public/8.0.0/ucd/NameAliases.txt
  |\uxxxx            |\ 뒤에 16비트 16진수 숫자를 지정하여 유니코드의 문자 표현
  |                  |예) '\u0061'은 'a'를 표현
  |                  |유니코드는 다음 URL 참조
  |                  |- List of Unicode characters(유니코드 문자 목록)
  |                  |  https://en.wikipedia.org/wiki/List_of_Unicode_characters
  |                  |- Hangul Syllables(한글 음절)
  |                  |  https://en.wikipedia.org/wiki/Hangul_Syllables
  |\Uxxxxxxxx        |\ 뒤에 32비트 16진수 숫자를 지정하여 유니코드의 문자 표현
  |                  |예) '\U00000061'은 'a'를 표현
  |                  |유니코드는 위 URL과 동일

- 앞에 r을 붙여 무효화 가능

```python
print("c:\program\files\test.txt")
print(r"c:\program\files\test.txt")

```

```
c:\program
        iles est.txt
c:\program\files\test.txt
```

## 자료형 변경

- 앞에 바꾸고 싶은 자료형 붙이면 됨

```csharp
a = 45 
b = 1.23
c = "10.231"

print(float(a))
print(int(b))
print(float(c))

# 45.0
# 1
# 10.231
```