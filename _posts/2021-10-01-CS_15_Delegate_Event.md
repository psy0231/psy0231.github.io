---
title: 15. Delegate, Event
date: 2021-10-01 12:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 1111-11-11 11:11:11 +0900
---

## intro
- delegate를 찾다보면 꼭 따라오는 두 키워드가 더 있다.
  - event  
  - callback
- 그 중 event, delegate는 형태나 쓰임새나 비슷해 보여서  
한번 정리할겸 쓴다.
- callback은 뒤에서 따로 하기로하고. 

### Delegate vs Event
- 둘 다 비슷해 보이는데 비슷한 시기에 막 등장.
  - delegate를 찾다보면 event랑 엮어서  
  같이 설명하는게가 많아서 그런가봄.
- 하기전에 비슷한점을 보면
  - 실제 사용될 method를 연결은 +=로 함
  - 호출하면 다른게 트리거된다는점(+=로 연결한것.)
  - 정도?
- 비슷한건 이게 다인것같은데  
이럴거면 왜 구분지어 사용하나 싶음.


- 둘 사이 차이점을 찾아보면 공통적인 설명이
  1. event는 delegate의 일종으로 특수한 형태.
  2. event는 class외부에서 사용하지 못함.
      - delegate는 쌉가능.
      - class외부에서 추가(+=)는 됨.
  3. event는 "=" 연산자 사용 못함.

## Event

### 1. event는 delegate의 특수한 형태이다? 
- form에 Load자동생성한다. 

  ```c#
  private void Form1_Load(object sender, EventArgs e)
  {
      throw new System.NotImplementedException();
  }
  ```

- 이떄 designer에는 다음이 생성된다.
  
  ```c#
  this.Load += new System.EventHandler(this.Form1_Load);
  ```
  - 여기까지 자주보는 ..

- 이 Load를 따라가 보면 

  ```c#
  public event EventHandler Load
  {
    add => this.Events.AddHandler(Form.EVENT_LOAD, (Delegate) value);
    remove => this.Events.RemoveHandler(Form.EVENT_LOAD, (Delegate) value);
  }
  ```
  - 여기 event는 쓰여진 자리로봐서 알겠지만 그냥 키워드임.  
  - "async"때처럼 "여긴 이런 기능을 할겁니다" 느낌. 

- 그럼 EventHandler은?

  ```c#
  namespace System
  {
    [ComVisible(true)]
    [__DynamicallyInvokable]
    [Serializable]
    public delegate void EventHandler(object sender, EventArgs e);
  }
  ```
  - 미리 만들어놓은 delegate 시그니처였음

- 익숙한 순서로 정리해보면 

  ```c#
  //delegate 시그니처 정의
  public delegate void EventHandler(object sender, EventArgs e);
  
  //event키워드가 붙은 delegate정의
  public event EventHandler Load
  {
    add => this.Events.AddHandler(Form.EVENT_LOAD, (Delegate) value);
    remove => this.Events.RemoveHandler(Form.EVENT_LOAD, (Delegate) value);
  }

  //트리거될 method 연결
  this.Load += new System.EventHandler(this.Form1_Load);

  //실제 method
  private void Form1_Load(object sender, EventArgs e)
  {
      throw new System.NotImplementedException();
  }
  ```
  - 요 순서대로 하면 전에 delegate쓰던거랑 같음

- event를 빼고 똑같이 만들면 보통의 delegate가 됨.

  ```c#
  public delegate void EventHandler_1(object sender, EventArgs e); 
  public EventHandler_1 evt_1;
  ```
  - 이런식으로.

- 따라서  
  - event는 delegate의 특수한 형태가 맞다.
  - event는 키워드로써 아무튼 delegate를 인첸트 시켰겠지.  
  뭔진 아직 모르겠고.  

### 2. event는 class외부에서 사용하지 못한다?
- 예를들어

  ```c#
  class Program
  {
      static void Main(string[] args)
      {
          Console.WriteLine("Hello World!");

          Test1 t = new Test1();
          //delegate
          t.evt_1 += t.TestDelegate;
          t.evt_1 += T1_tempevent;
          //event
          t.evt_2 += t.TestEvent;
          t.evt_2 += T1_tempevent;

          t.evt_1(new object(), new EventArgs());
          //t.evt_2(new object(), new EventArgs()); //컴파일에러
      }
      private static void T1_tempevent(object sender, EventArgs e)
      {
          Console.WriteLine("T1_tempevent");
      }
  }

  ///////////////////////////////

  public class Test1
  {
      //delegate
      public delegate void EventHandler_1(object sender, EventArgs e); 
      public EventHandler_1 evt_1;

      //event
      public delegate void EventHandler_2(object sender, EventArgs e); 
      public event EventHandler_2 evt_2;
      
      public Test1()
      {
          evt_2 += TestEvent;
          evt_2(new object(), new EventArgs());
      }
      
      public void TestDelegate(object sender, EventArgs e)
      {
          Console.WriteLine("delegate");
      }

      public void TestEvent(object sender, EventArgs e)
      {
          Console.WriteLine("event");
      }
      
  }
  ```
  - Program class에서  
    - t.evt_2 += t.TestEvent;  
    즉, 이벤트에 메서드 추가는 됨.
    - 단, 주석처리된  
    t.evt_2(new object(), new EventArgs());  
    이부분은 안되는데 내용이  
    
        The event 'evt_2' can only appear  
        on the left hand side of += or -=  
        (except when used from within the class 'DelegateEvent.Test1')  
        라고함.     

        쫌 더 보편적으로 말하면  
        event는 +=의 왼쪽에만 쓰일 수 있는데  
        event가 선언 된 class는 상관 없다로 보임. 

        다시말해 event가 선언되었던 class가 아니면  
        event에 method추가외의 호출은 불가.

  - 단, event의 호출은 event가 선언된 class내부에서만 되지만  
  event에 대한 구독은 외부에서도 가능 (T1_tempevent)

- 정리하면  
  - '외부에서 사용'의 의미는  
  '선언되었던 class외부에서 호출이 가능하냐'는 뜻이고, 불가능하다. 
  - event call은 event가 선언된 class내부에서만 가능
  - event에 대한 subscribe는 제약이 없다면 내/외부 다 가능.  
  - subscribe할 method도 제약이 없다면 내/외부 상관없음.

### 3. event는 "=" 연산자 사용 못함?
- 위 예의 Program class에서  
t.evt_2 += t.TestEvent;는   
+= 를 = 로 바꾸면 에러가 난다.  
내용은 위에꺼랑 동일. 

    Test1 class의  
    evt_2 += TestEvent;에서   
    +=대신 =를 써도 상관없음.
    대신 덮어써지겠지만..  
    
    위에 에러 내용을 다시 보면 
    (except when used from  
    within the class 'DelegateEvent.Test1')  
    이 부분이 있는데  
    event가 선언됐던 class내부에서는  
    =나 +=나 딱히 상관없는듯함.

### 꼭 저런 형태여야하는가?
- event의 기본 뼈대가 delegate니까 변형도 가능할듯.  
  - parameter 추가  
  - return값 추가

  ```c#
  public class Test1
  {
      //delegate
      public delegate void EventHandler_1(object sender, EventArgs e); 
      public EventHandler_1 evt_1;

      //event
      public delegate void EventHandler_2(object sender, EventArgs e);
      public event EventHandler_2 evt_2;

      //event 2
      public delegate int EventHandler_3(object sender, EventArgs e, int i);
      public event EventHandler_3 evt_3;
      
      public Test1()
      {
          evt_2 += TestEvent;
          evt_2(new object(), new EventArgs());

          evt_3 += TestEvent_2;
          evt_3 += TestEvent_3;
          int i = evt_3(new object(), new EventArgs(), 3);
          Console.WriteLine(i);
      }
      
      public void TestDelegate(object sender, EventArgs e)
      {
          Console.WriteLine("delegate");
      }

      public void TestEvent(object sender, EventArgs e)
      {
          Console.WriteLine("event");
      }
      
      public int TestEvent_2(object sender, EventArgs e, int i)
      {
          Console.WriteLine("event2");
          return i * i;
      }
      public int TestEvent_3(object sender, EventArgs e, int i)
      {
          Console.WriteLine("event3");
          return i + i;/
          (EventHandler_3)item
      }
  }
  ```
  - 물론 parameter를 설정하는건 자유.
  - return이 문제가 되는 부분인데   
  보통 event에는 return이 없음.  
  
    만들때는 return이 있어도 상관없는데  
    event에 연결된 method가 많을경우(위처럼)  
    return된 값은 마지막에 실행(연결)된  
    method의 return값만 받을 수 있음  
    
    이래서 void를 주로 쓰는것같고  
    void아기 때문에 chain인 상황에서  
    트리거될 method들에게 통지하는 모습으로 보여지게됨.  
  
  - 아무튼 이런 이유때문에 일반적으로 void만 쓴다면  
  임의로 event를 만들어 쓸때도 이 방식을 따르는게 맞아보임.

### EventArgs
- 지금까지 예시에 parameter로 항상  
(object sender, EventArgs e)이게 들어갔음.  
  -  object는 event를 발생시킨주체로  
  예를들어 buttonclick은 button 등등  
  - EventArgs는 event의 정보를 나타내는데  
    - 이벤트 데이터를 포함하는 클래스의 기본 클래스.  
    이벤트 데이터를 포함하지 않는 이벤트에 사용할 값 제공.
    - 그래서 이벤트에 별다른 값이 필요하지 않는 것들  
      - ex) form.closed, form.load등
      - EventArgs내부에도 사실 별 내용이 없긴함
    - 그 외 이벤트에 따로 값이 필요한것들은 여기서 파생해서 사용.
      - ex)  MouseDown, , MouseClick등 에서의 MouseEventArgs. 
      - 여기는 부가적으로 위치, 클릭획수 등의 정보가 더 들어감.
      - EventArgs를 접미사로씀.
- 만약 event를 새 형식으로 만들어 사용한다면

  ```c#
  public class Test2
  {
      delegate void CustomEventDelegate(object o, CusTomEventArgs e);
      event CustomEventDelegate CusTomEventHandler;

      public Test2()
      {
          CusTomEventHandler += temp;

          CusTomEventHandler(this, 
                            new CusTomEventArgs() 
                            { 
                                msg = "message", cnt = 123 
                            });
      }

      void temp(object o, CusTomEventArgs e)
      {
          Console.WriteLine(string.Format("msg:{0}\ncnt:{1}",e.msg,e.cnt));
      }
  }

  public class CusTomEventArgs: EventArgs
  {
      public string msg;
      public int cnt;

  }
  ```

  - class CusTomEventArgs를 보면 EventArgs를 상속받는데  
  EventArgs가 기본형이니까 이걸 토대로 만드는것처럼보임.  
  - 이 형식을 따른 예제들이 자주보여 쓰긴했는데  
  object랑 eventargs가 꼭 필요한가싶음.  
  그냥 없이 간단하게 써도 될듯함.
  - 는 어림도 없지 아래 이벤트2 읽다보면 EventArgs를 기본으로 하던가  
  이거 상속하는걸 권장하고있음.

## 왜 event?
- delegate를 만들고 이걸 Action, Func로 표준화함. 
  - 이해됨. 자주쓰인다니까 귀찮아서 만들었겠지.
- delegate/event 둘 다 시그니처 선언하고  
그에 맞는 method연결하고 시그니처 호출하면  
연결되어있던 method들이 트리거됨 여기까지 공통.  
  
  그런데 delegate에 제약을 거는  
  event라는 키워드를 붙임.
  같은 동작인데 왜 제약을 줬을까?

### delegate 자유도
- event라는 제약없이 delegate라면,  
method추가는 =, += 둘 다 가능한데  
이 경우 모두 트리거됬어야될 상황이  
=으로 사라질 수 있다.  
다시말해 초기화의 위험성이 있다.  
그래서 event가 선언되었던 class내부에서는  
=을 허용한게 아닐까 싶다.  

- 그럼 호출도 이런의미로 생각하면 될듯.  
예를들어 Form.Load를 아무데서나 쓸 수 있게 하면  
좀 헷갈릴것같기도하고..  
그래서 Refresh()나 RaiseKeyEvent(,)같은걸 만들어  
event는 class내부에서만 호출하고  
그 비슷한 기능이 필요한 경우를 위해  
저렇게 따로 만들어 놓은것같음.

### Access Modifiers 
- access가 외부에서 되지 않는다는 점 때문에  
처음생각엔 event라는 키워드가 없러라도  
간단히 public을 안쓰면 되지않나 생각해봤는데  
  - public
  - protected
  - internal
  - private

- public,private는 당연히 안되고  
  
  protected는 외부호출을 막으면서  
  method추가도 할 수 없게됨.  
  상속을 받아야 하는데 그렇게 보면 event가 선녀임.

  internal은 되나싶었는데  
  외부참조한 곳의 event는 못쓰게됨  

  어차피 한정자에 대한 설명을   
  한번 더 쓴것같긴한데 어쨌든   
  위 키워드 넷 다 애매하게 빗나감.

- 결과적으로 delegate에 대한 subscribe는  
자유롭게 할 수 있으면서  
임의의 위치에서 호출을 막고  
초기화의 위험을 줄이는 용도로  
이걸 쓴다하면 대퉁 맞는것같음.  

## 참고
- [이벤트1](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/events/)
- [이벤트2](https://docs.microsoft.com/ko-kr/dotnet/standard/events/)
- [이벤트와 델리게이트](https://debuglog.tistory.com/148)
- [C# event](https://jeong-pro.tistory.com/54)
- [이벤트 개념 잡기와 만들기](https://link2me.tistory.com/864)
- [이벤드 만들고 사용하기](https://eslife.tistory.com/1255)
- [C# 이벤트](https://www.csharpstudy.com/CSharp/CSharp-event.aspx)
- [Delegate에서 Event로](https://www.csharpstudy.com/CSharp/CSharp-delegate3.aspx)
