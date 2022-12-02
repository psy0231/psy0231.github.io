---
title: 2. Action/Func
date: 2020-03-12 12:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 2020-08-31 22:04:17 +0900
---

# Build Up 
> ***Action/Func*** - Task - async/await

## Action basic
- Encapsulates a method that 
has no parameters and does not return a value.  
  ```c# 
  public delegate void Action();
  ```
  
- 그렇다고 한다. 그만알아보고 싶다.

## 의문
- delegate가 있는데 이걸 왜 만들었을까?
- 왜.... 왜.... 똑같잖아...

## 사용 
- delegate니까 delegate랑 같겠지
- 해보자
  ```c#
  class AT
  {
      public AT()
      {
          Action act = mtd;
          act();
      }

      public void mtd()
      {
          Console.WriteLine("cw");
      }
  }
  ```
- 원래 이랬을껄?
  ```c#
  class AT_1
  {
      delegate void custom_del();
      public AT_1()
      {
          custom_del custom = mtd;
          custom();
      }

      public void mtd()
      {
          Console.WriteLine("cw AT_1");
      }
  }

  class AT_2
  {
      delegate void custom_del();
      custom_del custom;
      public AT_2()
      {
          custom += mtd;
          custom();
      }

      public void mtd()
      {
          Console.WriteLine("cw AT_2");
      }
  }
  ```
- 조금씩 다른데 셋 다 비교해보자
  - AT는 제일 기본형태 action이고 AT_1은 그걸 단순히 delegate로 바꿔놓은것  
  (둘이 같고 action이라는 이름만 써줬으니 원래? 형태로 써봄)      
  - AT_2는 내가 평소 쓰는 습관(혹시나 비교를 위해 - 했는데 같은거였네...)
  - 이렇게 세개 놓고 보면 별 다른게 없음
  - 실제 Action을 보면  
  Action 부터  
  Action<T1,T2,T3,T4,T5,T6,T7,T8,T9,T10,T11,T12,T13,T14,T15,T16> 까지  
  미리 만들어 놓은거진보면짜 그런듯...
- AT_1 vs AT_2
  - 둘이 다른줄 알았는데 선언을 어디 했냐가 다른거였네 
  - AT_1의 custom은 `=` 만 되고  
  AT_2의 custom은 `+=`가 되긴하는데 그게 선언위치 때문인듯

## 결론
- 이거 쓰는 와중에도 이전에 delegate로 쓰던 것들을  
Action으로 바꾸면 더 간결해질것같음을 느낌
- Actino은 return이 void인 경우만 가능하다.  
return 을 받고싶으면 Func을 쓰면 된다한다. 
- 사용 시 무명 메서드, 람다식을 사용해  
더 간단히 쓸 수 있는데 이것도 나중에 써보자.
- Action자체가 delegate void ~~ 로 쓰는거라 더 쓸내용은 없네

## Func
- 위에 썼듯이 Action사용에서 return이 필요하면 Func로 바꾸면 됨
- 간단하게 하고 넘어감
  ```c#
  class Program
  {
      static void Main(string[] args)
      {
          new FC();
      }
  }

  class FC
  {
      public FC()
      {
          Func<int> func = mtd;
          int a = func();
          Console.WriteLine(a);

          Func<int, int> func_2 = mtd_2;
          int a_2 = func_2(1);
          Console.WriteLine(a_2);
      }

      int mtd()
      {
          return 0;
      }

      int mtd_2(int _i)
      {
          return _i+1;
      }
  }
  ```
  - 항상 return은 기본으로 있음 형식만 지정하면 됨
  - 마지막 parameter는 return이고  
  마지막 제외한 patameter는 넘겨줄 값들 
    
## 참고
- 더 좋은 설명들  
[Action Delegate](https://docs.microsoft.com/en-us/dotnet/api/system.action?view=netframework-4.8)  
[Action\<T\> Delegate](http://www.csharpstudy.com/Tip/Tip-Func.aspx)  
[Func&Action](https://mrw0119.tistory.com/23)  
[Action](https://referencesource.microsoft.com/#mscorlib/system/action.cs)
    - 안그래도 다 같이있긴하네 