---
title: Python 6 - Control Flow
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-04-28 00:00:00 +0900
categories: [Grind, Python]
tags: [python, control flow]
---

## if
- 조건문 근본

```python
# if [condition] :
#    statement 
# elif [condition] :
#    statement
# else :
#    statement

someCondition = 1024

if someCondition == 0:
    print("0")
elif someCondition > 0 and someCondition <= 1024:
    print(" < 1k")
elif someCondition > 1024 and someCondition <= 2048:
    print(" < 2k")
else :
    print("over 2k")
  
```

- python에는 switch가 없다고 한다.

## for

```python
# for [item] in [Collection]:
#     statement

for temp in range(0,101,10):
    print("temp =", temp)

```

- collection요소를 item으로.
- ```foreach(type item in collection)```  
이랑 비슷한듯.  
하긴 type 적을 필요가 없으니  
그렇게 생각하면 똑같음.
- 근데 따지면 모든 item을  
다 검색하는 방법밖에 없는거아님?

```python
oldIndex = range(1, 10)

def temp(x):
    return x*x

newIndex = [temp(i) for i in oldIndex]
print(newIndex)

result = [num * 3 for num in oldIndex if num % 2 == 0]
print(result)
```

- 이렇게 쓰는것도 됨... ㅁ7..

## while

```python

# while [Condition]:
#     statement

index = 10

while index > 0:
    print(index)
    index -= 1

```

```python
index = 0
while True:
    index += 1
    if index % 2 != 0:
        continue
    print(index)
    if index == 10:
        break

```

- continue , break, if 등등..
- 막상 ```{ }```가 없으니까 좀...