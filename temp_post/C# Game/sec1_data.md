---
title: [C#과 유니티로 만드는 MMORPG 게임 개발 시리즈] Part1 C# 기초 프로그래밍 입문
date: 1111-11-11 11:11:11 +0900
categories: [cate1, cate2]
tags: [tag1, tag2]
seo:
  date_modified: 1111-11-11 11:11:11 +0900
---
# integer
- 주석 
  - //
  - /**/

- 데이터 + 로직
  - palyer의 체력 + 상호작용

- 정수
- 를 저장하는 변수
  - 형식 지정 
  - byte(1 byte), short(2 byte), int(4 byte), long(8 byte)
  - sbyte, ushort, uint, ulong 
  - s 는 signed, u 는 unsigned

# 2,10,16
- 2
  - 0,1
  - 0b가 prefix
  - 0b00, 0b01
- 10
  - 0,1,2,3,4,5,6,7,8,9
- 16
  - 0,1,2,3,4,5,6,7,8,9,a,b,c,d,e,f
  - 0x가 prefix
  - 0x01, 0x09, 0xf6 

# 정수 범위
- 0, 1 bit
- 1 byte = 8 bit
- 음수표현은 2의 보수이용 

# 그 외 기본형식
## boolean
- true, false;
- 1byte

## 소수
- float
  - postfix : f
  - 4 byte
- double
  - postfix : d
  - 8 byte

## chracter
- char
- 2byte

## string
- array of char

# 형식변환
- 데이터 크기가 다른경우
  - int to short
- 데이터 부호 관령
  - byte to sbyte
- 정수 소수

# 데이터 연산
- +, -, *, /, %
- ++i,  i++
- <, >,   <=, >=, ==, !=
- &&, ||, !, 