---
title: async/await_1
date: 2020-09-30 18:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 2020-09-30 23:20:36 +0900
---

## Async
> Use the async modifier to specify that a method, lambda expression, or anonymous method is asynchronous. If you use this modifier on a method or expression, it's referred to as an async method.

- async method라고 알려주는 정도.
    - method, lambda expression, anonymous method에 사용 가능
- return형은 Task,Task\<TResult\>, void, C#7.0부터는 GetAwaiter (System.Threading.Tasks.ValueTask\<TResult\>)
    - TResult를 사용해야하는 경우, 그렇지 않더라도 Task를 반환형으로 쓰라는듯.
    - void는 호출자가 결과(또는 exception)를 모르니 이벤트 걸어 사용하라는 듯한 설명이 아래 더 있었는데 이게 정확한건지는 모르겠다.
- in, ref, out을 parameter, return으로 사용 못함. 단, 이런 parameter가 있는 method는 호출 가능

## Await
> The await operator suspends evaluation of the enclosing async method until the asynchronous operation represented by its operand completes. When the asynchronous operation completes, the await operator returns the result of the operation, if any. When the await operator is applied to the operand that represents already completed operation, it returns the result of the operation immediately without suspension of the enclosing method. The await operator doesn't block the thread that evaluates the async method. When the await operator suspends the enclosing async method, the control returns to the caller of the method.

- Await가 하는일이 핵심인것같은데 실제로는 이 연산자가 async로 하게 해준다는 설명... 
- async로 만들어진 블럭에만 사용가능
    - lock, unsafe등에서는 사용 불가
- await의 피연산자는 일반적으로 Task, Task<TResult>, ValueTask,  ValueTask<TResult> 인데 대기 가능한 모든 식은 가능하다함.

## async/await
- 위 설명이 빈약한 이유는 아래 더 좋은 설명들이 있어서 다시쓰는건 의미가 없고, 이랬구나 하는 느낌만 남기면 됐으니까 가물가물하면 아래꺼 함 더 보자.
- task와 비교
    ```c#
    private void button1_Click(object sender, EventArgs e)
    {
        Task t;

        t = Task.Factory.StartNew(DoSomethingThatTakesTime);
        t.Wait();
          
    }

    private async void button2_Click(object sender, EventArgs e)
    {
        var result = Task.Factory.StartNew(DoSomethingThatTakesTime);
        await result;
    }

    private void button3_Click(object sender, EventArgs e)
    {
        DoSomethingThatTakesTime();
    }

    private void DoSomethingThatTakesTime()
    {

        Console.WriteLine("Task id : {0} thread id : {1}", Task.CurrentId, Thread.CurrentThread.ManagedThreadId);
        Thread.Sleep(1000 * 1);
    }
    ```
    - 출력을 보면
        ```c#
        Task id : 1 thread id : 3  
        Task id : 2 thread id : 3  
        Task id :   thread id : 1
        ```
        - 1,3은 같아보이지만 thread가 다르고 (thread id 1이 ui thread)
        - 1,2는 확실히 다른 곳에서 실행 됨

- 

## 참고
- [C# 5.0 : async / await 키워드](http://www.csharpstudy.com/CSharp/CSharp-async-await.aspx)  
- [Asynchronous programming with async and await](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/)  
- [What's the difference between Task.Start/Wait and Async/Await?](https://stackoverflow.com/questions/9519414/whats-the-difference-between-task-start-wait-and-async-await)
- [작업 기반 비동기 프로그래밍](https://docs.microsoft.com/ko-kr/dotnet/standard/parallel-programming/task-based-asynchronous-programming)
- [작업 기반 비동기 패턴](https://docs.microsoft.com/ko-kr/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [비동기 프로그래밍](https://docs.microsoft.com/ko-kr/dotnet/csharp/async)
- [작업 비동기 프로그래밍 모델](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/concepts/async/task-asynchronous-programming-model)
- [async](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/async)  
- [await](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/operators/await)