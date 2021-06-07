---
title: ch2
date: 1111-11-11 11:11:11 +0900
categories: [cate1, cate2]
tags: [tag1, tag2]
seo:
  date_modified: 1111-11-11 11:11:11 +0900
---
# 증권사 Open API 연동하기 + 기초 문법(변수, 함수)
## 증권사 Open API 연동하기
- openapi 개발 가이드 항상 참고할것.
- 처음 openapi.py를 열면 QAxContainer package inport가 안됨
  - pip uninsttall PyQt5
  - pip install --user PyQt5==5.13  
  하면 해결
- 정상실행 후 사용자 권한을 물어보는 화면에서 
  - 자세한 내용 표시
  - 다음 알림이 표시될 떄 변경
  - 알리지 않음
  - 으로 해야 다음부터 권리자권한으로 막히지 않음
- 정상 연결이라면 아래 매시지 나옴
  ``` 
  connected
  계좌번호: 8167604911
  ```

## 기초 문법(변수, 함수)
- 주석 
  ```py
  # 주석

  # ctrl + /
  ```

- 변수 : 방
  - Type는 그닥 신경 안쓰는것같음 (var 느낌)

- 함수
  - def로 선언 (define)
  - 함수명은 자유
  - 함수 내에서 들여쓰기
  - 일단, 함수 이름은 중복되면 안된다.
  ```py
  # 선언
  def print_test():
      print("test!!!!")
  # 호출
  print_test()
  ```  
  - parameter 있는형
  ```py
  def print_test2(x):
      print(x)
  ```
  - return 
  ```py
  
  def print_test4(x, y):
      return x + y

  ```
  - 변수 범위
    - 지역 / 전역
    ```py    
    def ex6():
        ex6_x = 2
        print("지역변수 : ", ex6_x)

    ex6_x = 1
    ex6()
    print("전역변수 :", ex6_x)
    ```
    - 알것는디 생긴게 헷깔림 
    - 다만, 이 경우 경고뜸 이름이 같다고...