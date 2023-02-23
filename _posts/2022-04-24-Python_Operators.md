---
title: Operators
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-04-24 00:00:00 +0900
categories: [Grind, Python]
tags: [python, operator]
---
## Init

- python에서는 7개 그룹의 연산자가 있음
  - Arithmetic operators
  - Assignment operators
  - Comparison operators
  - Logical operators
  - Identity operators
  - Membership operators
  - Bitwise operators

## Arithmetic Operators

- 산술 연산.

| Operator | Name | Example |
| --- | --- | --- |
| + | Addition | 2 + 3 = 5 |
| - | Subtraction | 3 - 2 = 1 |
| * | Multiplication | 2 * 3 = 6 |
| / | Division | 5 / 2 = 2.5 |
| % | Modulus | 7 % 2 = 1 |
| ** | Exponentiation | 2 ** 3 = 8 |
| // | Floor division | 10 // 3 = 3 |

## Comparison Operators

- 비교연산

| Operator | Name | Example |
| --- | --- | --- |
| == | Equal to | 2 == 2 |
| != | Not equal to | 2 != 3 |
| < | Less than | 2 < 3 |
| > | Greater than | 3 > 2 |
| <= | Less than or equal to | 2 <= 3 |
| >= | Greater than or equal to | 3 >= 2 |

## Logical Operators

- 논리 연산

| Operator | Name | Example |
| --- | --- | --- |
| and | Logical AND | x > 5 and x < 10 |
| or | Logical OR | x < 5 or x > 10 |
| not | Logical NOT | not(x > 5 and x < 10) |

## Assignment Operators

- 할당연산

| Operator | Example | Same as |
| --- | --- | --- |
| = | x = 5 | x = 5 |
| += | x += 3 | x = x + 3 |
| -= | x -= 3 | x = x - 3 |
| *= | x *= 3 | x = x * 3 |
| /= | x /= 3 | x = x / 3 |
| %= | x %= 3 | x = x % 3 |
| //= | x //= 3 | x = x // 3 |
| **= | x **= 3 | x = x ** 3 |
| &= | x &= 3 | x = x & 3 |
| \|= | x \|= 3 | x = x \| 3 |
| ^= | x ^= 3 | x = x ^ 3 |
| >>= | x >>= 3 | x = x >> 3 |
| <<= | x <<= 3 | x = x << 3 |

## Identity Operators

- 항등연산
- object 비교.
- 같은 object가 아닌  
같은 memory에 있는  
object인지 비교.

| Operator | Example | Result |
| --- | --- | --- |
| is | x is y | Returns True <br>if the same object |
| is not | x is not y | Returns True <br>if not the same object |

## Membership Operators

- 멤버쉡 연산
- object에 시퀀스가 존재하는지 테스트.
- 
  | Operator | Example | Name |
  | --- | --- | --- |
  | in | x in y | Returns True <br>if y have x |
  | not in | x not in y | Returns True <br>if y have not x |
- 아무튼 좀 애매한부분을 더 봤는데
  
  ```python
  l2 = [1, 2, 3]
  l3 = [1, 2]
  item = 3
  
  print("l1 in l2 ?", (l1 in l2))
  
  print("item in l1 ?", (item in l1))
  
  print("l3 in l1 ?", (l3 in l1))
  ```
  
  ```python
  l1 in l2 ? False
  item in l1 ? True
  l3 in l1 ? False
  ```
  
  해서, 집합기호로 따지면 `∈` 관계.  
  부분집합이 아닌 원소.
    

## Bitwise Operators

- 비트연산

| Operator | Name | Example |
| --- | --- | --- |
| & | AND | x & y |
| \| | OR | x \| y |
| ^ | XOR | x ^ y |
| ~ | NOT | ~x |
| << | Zero fill left shift | x << y |
| >> | Signed right shift | x >> y |

## Operator Precedence

- 앞서 썼던 연산자 포함 연산자 우선순위.  
나머지는 앞으로 오며가며 볼꺼니까.
- 위쪽으로 우선순위가 높고   
같은 상자안에서는 순서상관없이 같음.
- 구문이 명시적이지 않으면  
연산자는 binary

| Operator | Description |
| --- | --- |
| (expressions...),[expressions...], <br>{key: value...}, {expressions...} | Binding, parenthesized expression, list display, <br>dictionary display, set display |
| x[index], x[index:index],<br> x(arguments...), x.attribute | Subscription, slicing, <br>call, attribute reference |
| await x | Await expression |
| ** | Exponentiation |
| +x, -x, ~x | Positive, negative, bitwise NOT |
| *, @, /, //, % | Multiplication, matrix multiplication, <br>division, floor division, remainder |
| +, - | Addition and subtraction |
| <<, >> | Shifts |
| & | Bitwise AND |
| ^ | Bitwise XOR |
| \| | Bitwise OR |
| in, not in, is, is not, <br><, <=, >, >=, !=, == | Comparisons, membership , identity  |
| not x | Boolean NOT |
| and | Boolean AND |
| or | Boolean OR |
| if – else | Conditional expression |
| lambda | Lambda expression |
| := | Assignment expression |