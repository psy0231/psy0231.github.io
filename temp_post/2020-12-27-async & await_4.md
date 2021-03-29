---
title: async/await_3 excention 2
date: 1111-11-11 11:11:11 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 1111-11-11 11:11:11 +0900
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
    - 바뀐점은 ThrowException()가 async로 된것.
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




## 참고
- [Async 쓸까말까 및 주의할 점](https://www.youtube.com/watch?v=HkLhKyoh3sI)
- [async void가 안좋은 이유](https://www.youtube.com/watch?v=-tzfIeZGL5o)
- [Stephen Toub](https://blog.stephencleary.com/)
- [async 메서드의 void 반환 타입 사용에 대하여](https://www.sysnet.pe.kr/2/0/11414)
- [async void 메서드의 예외는 정말 Fire & forget 일까?](https://m.blog.naver.com/PostView.nhn?blogId=vactorman&logNo=221180404572&proxyReferer=https:%2F%2Fwww.google.com%2F)