---
title: async/await_1
date: 2020-09-30 18:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 2020-09-30 23:20:36 +0900
---

## Async
- async method라고 알려주는 정도.
    - method, lambda expression, anonymous method에 사용 가능
    - async 키워드에서 수정하는 메서드에 await 식 또는 문이 없는 경우 해당 메서드는 동기적으로 실행.  
    await 문이 포함되지 않은 모든 비동기 메서드에서는 오류가 발생할 수 있으므로 컴파일러 경고가 나타납니다.  
    - 비동기 메서드는 첫 번째 await 식에 도달 할 때까지 동 기적으로 실행되며  
    이 시점에서 awawit 된 task가 완료 될 때까지 메서드가 일시 중단.  
    그 동안 제어는 메서드 호출자에게 반환됩니다.
    - async 키워드는 메서드, 람다 식, 무명 메서드를 수정할 때만 키워드로 사용.  
    다른 모든 컨텍스트에서는 식별자로 해석.

- return 형식
    - Task,Task\<TResult\> 
        - 번환 형식이 없고 / 있고 차이.
        - 메소드를 호출하면 작업이 반환되지만, 작업이 완료되면 작업을 기다리는 모든 대기 표현은 무효로 평가된다. 이건잘...
    - void 
        - 호출자가 해당 메서드를 기다릴 수없고 성공적인 완료 또는 오류 조건을 보고하기 위해 다른 메커니즘을 구현해야하므로  
        이벤트 처리기 이외의 코드에는 일반적으로 비동기 method는 void return을 사용하지 않는 것이 좋다.
        - void return type은 주로 이벤트 핸들러를 정의하기 위해 사용하며, 여기에는 해당 반환 유형이 필요하다.  
        void 반환 비동기 방법을 호출한 사람은 기다릴 수 없으며 비동기 방법이 throw하는  exception을 catch할 수 없다.
    - C#7.0부터는 GetAwaiter (System.Threading.Tasks.ValueTask\<TResult\>)
        - System.Threading.Tasks.ValueTask<TResult> 형식.  
        NuGet 패키지 System.Threading.Tasks.Extensions를 추가하면 사용가능.
        - C# 7.0부터 GetAwaiter 메서드가 있는 다른 형식(일반적으로 값 형식)을 반환하여 성능이 중요한 코드 섹션에서 메모리 할당을 최소화함.
    - in, ref, out을 parameter, return으로 사용 못함.  
    단, 이런 parameter가 있는 method는 호출 가능

## Await
- await 연산자는 피연산자가 나타내는 비동기 작업이 완료 될 때까지 둘러싸는 비동기 메서드의 평가를 일시 중단합니다.  
비동기 작업이 완료되면 await 연산자가 작업 결과를 반환합니다 (있는 경우).  
이미 완료된 작업을 나타내는 피연산자에 await 연산자를 적용하면 둘러싸는 메서드를 중단하지 않고 즉시 작업 결과를 반환합니다.  
await 연산자는 비동기 메서드를 평가하는 스레드를 차단하지 않습니다.  
await 연산자가 바깥 쪽 비동기 메서드를 일시 중단하면 컨트롤이 메서드 호출자에게 반환됩니다.  
- ex
    ```c#
    using System;
    using System.Net.Http;
    using System.Threading.Tasks;

    public class AwaitOperator
    {
        public static async Task Main()
        {
            Task<int> downloading = DownloadDocsMainPageAsync();
            Console.WriteLine($"{nameof(Main)}: Launched downloading.");

            int bytesLoaded = await downloading;
            Console.WriteLine($"{nameof(Main)}: Downloaded {bytesLoaded} bytes.");
        }

        private static async Task<int> DownloadDocsMainPageAsync()
        {
            Console.WriteLine($"{nameof(DownloadDocsMainPageAsync)}: About to start downloading.");

            var client = new HttpClient();
            byte[] content = await client.GetByteArrayAsync("https://docs.microsoft.com/en-us/");

            Console.WriteLine($"{nameof(DownloadDocsMainPageAsync)}: Finished downloading.");
            return content.Length;
        }
    }
    // Output similar to:
    // DownloadDocsMainPageAsync: About to start downloading.
    // Main: Launched downloading.
    // DownloadDocsMainPageAsync: Finished downloading.
    // Main: Downloaded 27700 bytes.
    ```
    - HttpClient.GetByteArrayAsync 메서드는 완료시 바이트 배열을 생성하는 비동기 작업을 나타내는 Task <byte []> 인스턴스를 반환합니다.  
    작업이 완료 될 때까지 await 연산자는 DownloadDocsMainPageAsync 메서드를 일시 중단합니다.  
    DownloadDocsMainPageAsync가 일시 중단되면 제어가 DownloadDocsMainPageAsync의 호출자 인 Main 메서드로 반환됩니다.  
    Main 메서드는 DownloadDocsMainPageAsync 메서드에서 수행 한 비동기 작업의 결과가 필요할 때까지 실행됩니다.  
    GetByteArrayAsync가 모든 바이트를 가져 오면 나머지 DownloadDocsMainPageAsync 메서드가 평가됩니다.   
    그 후 나머지 Main 메서드가 평가됩니다.
    - C# 7.1부터 애플리케이션 진입점인 Main 메서드는 Task 또는 Task<int>를 반환하여 해당 본문에서 await 연산자를 사용할 수 있습니다.  
    이전 C# 버전에서는 Main 메서드가 비동기 작업이 완료될 때까지 대기하는지 확인하기 위해 해당 비동기 메서드에서 반환되는 Task<TResult> 인스턴스의 Task<TResult>.Result 속성 값을 검색할 수 있습니다.  
    값을 생성하지 않는 비동기 작업의 경우 Task.Wait 메서드를 호출할 수 있습니다.  

- await 연산자는 async 키워드로 수정 된 메서드, 람다 식, 익명 메서드에서만 사용.  
- 비동기 메서드 내에서 동기 함수 본문, Lock statement의 블록 내부 및 unsafe 컨텍스트에서 await 연산자를 사용할 수 없음.
- await의 피연산자는 일반적으로 Task, Task<TResult>, ValueTask,  ValueTask<TResult> 인데 대기 가능한 모든 식은 가능하다함.
- t 식의 형식이 Task<TResult> 또는 ValueTask<TResult>이면 await t 식의 형식은 TResult.  
t 형식이 Task 또는 ValueTask이면 await t 형식은 void입니다.  
두 경우 모두 t가 예외를 throw하면 await t는 예외를 다시 throw합니다.  
- Main 메서드의 await 연산자
C# 7.1부터 애플리케이션 진입점인 Main 메서드는 Task 또는 Task<int>를 반환하여 해당 본문에서 await 연산자를 사용할 수 있습니다.  
이전 C# 버전에서는 Main 메서드가 비동기 작업이 완료될 때까지 대기하는지 확인하기 위해  
해당 비동기 메서드에서 반환되는 Task<TResult> 인스턴스의 Task<TResult>.Result 속성 값을 검색할 수 있습니다.  
값을 생성하지 않는 비동기 작업의 경우 Task.Wait 메서드를 호출할 수 있습니다.  

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
- [Async 쓸까말까 및 주의할 점](https://www.youtube.com/watch?v=HkLhKyoh3sI)