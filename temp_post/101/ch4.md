---
title: ch4 데이터 컬렉터 만들기
date: 1111-11-11 11:11:11 +0900
categories: [cate1, cate2]
tags: [tag1, tag2]
seo:
  date_modified: 1111-11-11 11:11:11 +0900
---
# 1  인터프리터와 IDE (디버깅을 활용한 코드 분석 방법 TIP)
- 인터프리터(interpreter)
  - 프로그래밍 언어의 소스코드를 바로 번역하여 실행하는 컴퓨터 프로그램 또는 환경
  - python 코드(사람이 이해하는 언어) => 인터프리터 => 기계어(컴퓨터가 이해하는 언어 / 0과 1로 구성. ex.1000101010101011...)
  - 대화형 인터프리터 : ex) 하단의 python console
    - 장점 : 바로바로 결과가 나오므로 직관적(간단한 예제에서 활용)
    - 단점 : 코드가 길어질 수록 관리가 어려움
  - IDE (Integrated Development Environment) : ex) pycharm
    - 개발을 하면서 사용되는 도구들의 집합
    - 코드 편집: 소스파일에서 코딩 작업을 하고서, 실행시키면 파이썬 인터프리터가 기계어로 번역해 실행하고 화면에 출력
    - 디버깅 : pycharm 디버깅 기능 활용 가능 (고급챕터에서도 한번 더 설명)
        - 프로그램 실행 도중 break point(중단점) 지점에서 끼어들어 중간 상세과정을 살펴 볼 수 있다
        - .py 프로그램을 우클릭 후 Debug를 클릭
        - Debugger 탭에서 현재의 변수에 어떤 값이 저장 되어 있는지 확인 가능
        - Console 탭에서 대화형으로 디버깅 가능
        - 다시 실행 할 때는 우측 상단의 디버그 버튼을 누르면 디버깅 모드로 실행 가능
        - 디버깅 옵션 : 아래 3가지 옵션 모두 break point(중단점) 이 후의 코드를 실행
          - Step Into(단축키 : F7)
            - 모든 프로젝트의 코드를 한 줄씩 실행(라이브러리 포함)
          - Step Into My Code(단축키 : Alt + Shift + F7)
            - 내가 작성한 코드 내에서만 실행(라이브러리는 건너 뜀)
          - Step Over(단축키 : F8)
            - 현재 열려있는 파일 내에서만 코드를 한 줄씩 실행(함수 호출 코드의 경우 함수 내부로 들어가지 않음)
          - Step Out(Shift + f8)
            - 현재 실행 되고 있는 메서드를 즉시 빠져나와서 다음 실행 구문으로 이동한다  
            라고 하는게 실행은 함.
          - Run to Cursor(Alt + F9)
            - 에디터의 커서가 가리키고 있는 곳 까지 중간 과정을 무시하고 이동한다.  
          - Resume Program(F9)
            - 다음 break point 로 이동(Run to Cursor 와 비슷)
          - 범위 : Step Into > Step Info My Code > Step Over

# 2  기초문법(자료형, 조건문)
## 자료형
- 기본직인 데이터형 설명
- type() : 데이터 타입을 확인 하는 내장 함수
- 문자열 
  ```python
  name = '홍길동'
  print("%s님, 안녕하세요!" % name)  # 비추천, python2 버전에 사용하던 방식
  print("{}님, 안녕하세요!".format(name))  # 추천
  print(f"{name}님, 안녕하세요!")  # 추천 / f-string 이라고 표현 / python 3.6 이상 부터 지원
  
  # 비교
  print(name, "님, 안녕하세요!")
  # "안녕하세요 홍길동님, 반갑습니다." 를 출력 하시오
  print("안녕하세요", name, "님, 반갑습니다.")
  print("안녕하세요 {} 님, 반갑습니다.".format(name))
  print(f"안녕하세요 {name} 님, 반갑습니다.")
  ```
- 여기서는 null역할을 None가 하는것같음

## 조건문
```python
# [ if ~ elif ~ else 조건문 ]
ex3_a = 1
ex3_b = 2

if ex3_a > ex3_b:
    print("ex3_a 가 크다")
elif ex3_a < ex3_b:
    print("ex3_b 가 크다")
else:
    print("ex3_a와 ex3_b는 같다.")
```

# 3  기초문법(클래스와 객체, 생성자, self)
- ex
    ```python
  class Stock:  # Stock class
    def __init__(self, stock_name, stock_price, stock_rate):  # __init__: '생성자'(특별한 함수) 라고 한다. 클래스의 객체를 만들 때 자동으로 실행이 된다.
        self.name = stock_name  # 인스턴스변수 : self.name, self.price, self.rate를 인스턴스 변수라고 한다.
        self.price = stock_price
        self.rate = stock_rate
  
  item1 = Stock('삼성전자', 60900, 3.5) # =인스턴스 생성, 할당

  ```
## class 
- 알고있는 그거
- tab 으로 구분은 기본인듯?

## 생성자
- init 자동생성으로 저 모양이 됨
- init 말고 몇개 더 있던데??

## self
- 위 에서 self에는 item1이 들어감
- 생성될 인스턴스를 넣어준다는 의미같은데  
이러면 아닌경우가 있으니까 이렇게 만드는거아님?

# 4  기초문법(클래스와 메서드)
# 5  기초문법(모듈과 패키지)
# 6  데이터베이스 연동하기
# 7  기초문법(list, for)
# 8  종목리스트 가져오기 + 기초문법(dictionary, dataframe)
# 9  종목데이터 가져오기
# 10 주식 기본 용어 및 Datagrip 설치
# 11 종목리스트 저장하기
# 12 일별 금융 데이터 콜렉팅하기
# 13 콜렉팅 데이터 업데이트 및 분별 금융 데이터 콜렉팅하기
