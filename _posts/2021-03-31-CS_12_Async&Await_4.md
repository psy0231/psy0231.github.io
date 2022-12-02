---
title: 12. async/await_3 Excention 2
date: 2021-03-31 12:00:00 +0900
categories: [Grind, C#]
tags: [c#, asynchronous]
seo:
  date_modified: 2021-07-03 12:49:33 +0900
---

## about exception of void return.
- 쉽게, sync에서 먼저.
  ```c#
  class asaw_4_sync
  {
      public void ThrowException()
      {
          throw new InvalidOperationException();
      }
      public void SyncExceptions()
      {
          try
          {
              ThrowException();
          }
          catch (Exception)
          {
              // The exception is never caught here!
              throw;
          }
      }
  }
  ////////////////////////////////////////////////////
  private void button1_Click(object sender, EventArgs e)
  {
      try
      {
          aa4s.SyncExceptions();
      }
      catch (Exception ex)
      {
          textBox1.AppendText(ex.Message + Environment.NewLine);
      }
  }
  ```
  - 예상 가능한 결과로  
  호출은 button event => SyncExceptions() =>  ThrowException() 으로,   
  Exceptions은 ThrowException() => SyncExceptions() => button event로  
  잘 타고 온다. 항상 보던 당연한 결말.
  
- 위 방식을 async로 바꿔해본다.
  ```c#
  class ASAW_4_async_1
  {
      public async void ThrowException()
      {
          throw new InvalidOperationException();
      }
      public void AsyncExceptions()
      {
          try
          {
              ThrowException();
          }
          catch (Exception)
          {
              // The exception is never caught here!
              throw;
          }
      }
  }
  //////////////////
  private void button2_Click(object sender, EventArgs e)
  {
      try
      {
          aa4a_1.AsyncExceptions();
      }
      catch (Exception ex)
      {
          textBox1.AppendText(ex.Message + Environment.NewLine);
      }
  }
  ```
  - 바뀐점은 ThrowException()가 async로 된것, 또 하지말라던 void로 한것.
  - 호출은 button event => AsyncExceptions() => ThrowException()으로 동일,  
  Exception은 throw new InvalidOperationException(); 에서 멈춘다.  
  저 라인에서 throw를 못한다.

- 그럼 void가 아니라면??
  ```c#
  class ASAW_4_async_2
  {
      public async Task ThrowException()
      {
          throw new InvalidOperationException();
      }
      public void AsyncExceptions()
      {
          try
          {
              ThrowException();
          }
          catch (Exception)
          {
              // The exception is never caught here!
              throw;
          }
      }
  }
  ////////////////////////////////////////////////////////
  private void button2_Click(object sender, EventArgs e)
  {
      try
      {
          aa4a_1.AsyncExceptions();
      }
      catch (Exception ex)
      {
          textBox1.AppendText(ex.Message + Environment.NewLine);
      }
  }
  ```
  - 위 ASAW_4_async_1과의 차이점은 void에서 Task가 된것.
  - 호출 순서야 같은데 이번엔 throw new InvalidOperationException();에서  
  Exception도 안잡히고 끝난다. 아무런 반응이 없다.
  - 이 전 async/await_3 excention 1에서 말하는 The exception is never caught here!  
  은 Exception이 잡히건 말건 해당 try/catch는 절대 안들어온다는 뜻인것같다.

- 그럼 되는방법은?
  ```c#
  class ASAW_4_async_3
  {
      //public async void ThrowException()
      //{
      //    throw new InvalidOperationException();
      //}
      public async Task ThrowException()
      {
          throw new InvalidOperationException();
      }
      public async void AsyncExceptions()
      {
          try
          {
              await ThrowException();
          }
          catch (Exception)
          {
              // The exception is never caught here!
              throw;
          }
      }
  }
  ///////////////////////////////////////////////////////////
  private void button4_Click(object sender, EventArgs e)
  {
      try
      {
          aa4a_3.AsyncExceptions();
      }
      catch (Exception ex)
      {
          textBox1.AppendText(ex.Message + Environment.NewLine);
      }
  }
  ```
  - 다른점
      - AsyncExceptions : async에 있어야할 await가 붙음
      - async void를 그대로 하면 호출할때 에러남 async Task로 수정.
  - 결과는 AsyncExceptions() 의 catch 부분에서 잡힌다.
  - 따라서 정상적인 처리는 되는것처럼 보이는데  
  AsyncExceptions에서 button event로 throw는 안됨.
  - 이 부분은 다시 button4_Click를 async void로 수정하고  
  aa4a_3.AsyncExceptions(); 호출을 await로 한다.  
  AsyncExceptions()도 async void에서 async Task로 수정. 

    ```c#
    class ASAW_4_async_3
    {
        //public async void ThrowException()
        //{
        //    throw new InvalidOperationException();
        //}
        public async Task ThrowException()
        {
            throw new InvalidOperationException();
        }
        public async Task AsyncExceptions()
        {
            try
            {
                await ThrowException();
            }
            catch (Exception)
            {
                // The exception is never caught here!
                throw;
            }
        }
    }
    ///////////////////////////////////////////////////////////
    private async void button4_Click(object sender, EventArgs e)
    {
        try
        {
            await aa4a_3.AsyncExceptions();
        }
        catch (Exception ex)
        {
            textBox1.AppendText(ex.Message + Environment.NewLine);
        }
    }
    ```
    -  이렇게 되면 button event에서 정상적으로 잡아냄 원래 sync처럼.
    - void AsyncExceptions()에서 Task AsyncExceptions()으로  
    수정했는데 되는거 보면 Task에 뭔가 있겠지?
## so...
- 결과를 보면 async 함수들은 Task를 이용해 그 안에 정보를 넘기는듯하다.  
근데 return이 Task니까 당연한건가 싶은데  
data적인return 외에도 method적인 측면에서 실행 결과까지.  
그래서 void return의 경우.. 넘길 수 없어서 그 선에서 끝내는건가??  
- 무튼 return되는 Task에는 Exception까지 있는것같은데 다시,  
async/await_3 excention 2 의 Avoid Async Void 를 보면  

    >Async void 메서드는 오류 처리 의미가 다르다.  
    비동기 작업 또는 비동기 Task\<T> 메서드에서 예외가 throw되면  
    해당 예외가 캡처되어 Task 개체에 배치함.  
    async void 메서드를 사용하면 Task 개체가 없으므로  
    async void 메서드에서 throw 된 모든 예외는  
    async void 메서드가 시작될 때 활성화 된 SynchronizationContext에서 직접 발생.  
    다음은 비동기 void 메서드에서 throw 된 exception을 catch못한 경우. 
    
    이 말이 이제 이해가 감.

- 그럼 event를 제외하고 async void를 쓰지 말라했는데  
exception의 최종 목적지이면 상관이 없어보임.  
근데 이렇게 따지느니 Task로 도배하는게 맞는것같기도하고..
- 사실 async/await가 해주는 일이 더 많고  
이것저것 더 볼게 많은것같은데 이거 찾으면서, 또는 이 관련해 더 많은건 아래.


## 참고
- [Async 쓸까말까 및 주의할 점](https://www.youtube.com/watch?v=HkLhKyoh3sI)
- [async void가 안좋은 이유](https://www.youtube.com/watch?v=-tzfIeZGL5o)
- [async 메서드의 void 반환 타입 사용에 대하여](https://www.sysnet.pe.kr/2/0/11414)
- [C# 컴파일러 대신 직접 구현하는 비동기(async/await) 코드](https://www.sysnet.pe.kr/2/0/11351)
- [async void 메서드의 예외는 정말 Fire & forget 일까?](https://m.blog.naver.com/PostView.nhn?blogId=vactorman&logNo=221180404572&proxyReferer=https:%2F%2Fwww.google.com%2F)
