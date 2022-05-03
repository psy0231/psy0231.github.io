---
title: Concept
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-05-03 00:00:00 +0900
categories: [Grind, Stub]
tags: [method, function, argument, parameter, statement, expression]
---

- python 쓰다가 갑자기 궁금해서 찾았는데  
거기쓰긴 좀더 넓은 의미로 알아뒀음해서 옮김

## Method / Function

| name       | discription                 |
| ---------- | --------------------------- |
| Method     | 실행 가능한 코드 조각        |
| Function   | class안에 있는 function     |

## Argument / Parameter

| name       | discription                 |
| ---------- | --------------------------- |
| Parameter  | 함수에 정의된 입력 변수명    |
| Argument   | Parameter에 넘긴 실제 값     |

- ```def test(number, string):```에서  
number, string가 parameter
- ```res = test(1, "start")```에서  
1, "start"가 argument

## Statement / Expression

| name       | discription                      |
| ---------- | -------------------------------- |
| Expression | 특정 값으로 표현될 수 있는 코드   |
| Statement  | 실행 가능한 독립적인 코드조각     |

- Statement ≥ Expression 요런관계

```python
# Expression
a = 3 * 6 # Expression 이면서 Statement
b = a 

# Statement
while True: 
    pass

if True:
    pass

def b() :
    pass
```

- 그래도 좀 애매한데  
값에대한 평가가 들어가면 expression으로 하고  
그걸 포함한 실행가능한 코드면 statement로? ...