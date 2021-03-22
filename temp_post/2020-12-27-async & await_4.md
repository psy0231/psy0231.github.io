---
title: async/await_3 excention 2
date: 1111-11-11 11:11:11 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 1111-11-11 11:11:11 +0900
---
## about exception of void return.
- 이 전 방식대로 하면
  ```c#
  private void button1_Click(object sender, EventArgs e)
  {
      try
      {
          aa4s.AsyncVoidExceptions_CannotBeCaughtByCatch();
      }
      catch (Exception ex)
      {
          textBox1.AppendText(ex.Message + Environment.NewLine);
      }
  }
  //////////////////
  class asaw_4_sync
  {
      public void ThrowExceptionAsync()
      {
          throw new InvalidOperationException();
      }
      public void AsyncVoidExceptions_CannotBeCaughtByCatch()
      {
          try
          {
              ThrowExceptionAsync();
          }
          catch (Exception)
          {
              // The exception is never caught here!
              throw;
          }
      }
  }
  ```
  - 이 결과는 button1_Click catch에 걸려 결과가 나옴.
  
- 위 방식을 async로 바꿔해본다.
  ```c#
  private void button2_Click(object sender, EventArgs e)
  {
      try
      {
          aa4a.AsyncVoidExceptions_CannotBeCaughtByCatch();
      }
      catch (Exception ex)
      {
          textBox1.AppendText(ex.Message + Environment.NewLine);
      }
  }
  //////////////////
  class ASAW_4
  {
      public async void ThrowExceptionAsync()
      {
          throw new InvalidOperationException();
      }
      public void AsyncVoidExceptions_CannotBeCaughtByCatch()
      {
          try
          {
              ThrowExceptionAsync();
          }
          catch (Exception)
          {
              // The exception is never caught here!
              throw;
          }
      }
  }
  ```
  - 바뀐점은 ThrowExceptionAsync()가 async로 된것.
  - 결과가 다른점은 ThrowExceptionAsync()의  
  throw new InvalidOperationException(); 라인에서 exception걸림  
  



## 참고
- [Async 쓸까말까 및 주의할 점](https://www.youtube.com/watch?v=HkLhKyoh3sI)
- [async void가 안좋은 이유](https://www.youtube.com/watch?v=-tzfIeZGL5o)
- [Stephen Toub](https://blog.stephencleary.com/)
- [async 메서드의 void 반환 타입 사용에 대하여](https://www.sysnet.pe.kr/2/0/11414)
- [async void 메서드의 예외는 정말 Fire & forget 일까?](https://m.blog.naver.com/PostView.nhn?blogId=vactorman&logNo=221180404572&proxyReferer=https:%2F%2Fwww.google.com%2F)