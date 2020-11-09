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
### intro
- 다시 asynchronous programming를 async, await와 섞어서함.
- 따라서 본 내용은 이 전 async & await 1 의 내용과  
위의 async, await 와 중복되는 내용이 많은데,  
이부분은 아래 링크에도 있듯이 서로 연결되어있거나 순환적임.  
무튼, 계속 비슷한 내용들을 조금씩 다르게 설명하고있어서  
..이 부분에 적당히 섞어 정리함.

### Asynchronous programming 
- 주로 I/O바인딩 또는 CPU바인딩때 사용하는것을 권장하고 있는데  
실제 바인딩 영역은 몇가지 더 있는듯하나 시간이 오래 걸릴가능성이 있는건 이 둘인듯.(아래 링크)  
- 아무튼 대부분의 경우  
I/O 바인딩된 코드에서는 async 메서드의 내부에서 Task 또는 Task<T>를 반환하는 작업을,  
CPU 바인딩된 코드에서는 Task.Run 메서드로 백그라운드 스레드에서 시작되는 작업을 기다린다.
- 수행하는 작업이 어떤 바인딩인지 인식하는것이 중요하다고 하는데  
코드의 성능에 큰 영향을 미치고 잠재적으로 특정 구문을 잘못 사용하게 될 수 있기 때문.
- 구별짓기 위해 고려해야하는 사항으로는  
    1. 코드가 데이터베이스의 데이터와 같은 무엇인가를 “기다리게” 되나?
        - 대답이 "예"이면 I/O 바인딩된 작업.
    2. 코드가 비용이 높은 계산을 수행하게 되나?
        - 대답이 "예"이면 CPU 바인딩된 작업.
- 위 사항에 따라 
    - I/O 바인딩된 작업이 있을 경우 Task.Run 없이 async 및 await를 사용.  
    작업 병렬 라이브러리를 사용하면 안 됨.  
    그 이유는 세부 비동기에 설명.(이건 나중에)
    - CPU 바인딩된 작업이 있고 빠른 응답이 필요할 경우  
    async 및 await를 사용하지만 Task.Run을 사용하여 또 다른 스레드에서 작업을 생성합니다.  
    작업이 동시성 및 병렬 처리에 해당할 경우  
    작업 병렬 라이브러리를 사용할 것을 고려할 수도 있습니다.
    - 또한 항상 코드 실행을 측정해야 합니다.  
    예를 들어 CPU 바인딩된 작업이 다중 스레딩 시  
    컨텍스트 전환의 오버헤드에 비해 부담이 크지 않은 상황이 될 수 있습니다.  
    모든 선택에는 절충점이 있습니다.  
    상황에 맞는 올바른 절충점을 선택해야 합니다.
- 무튼 이 부분에서 중요한 점은 
    - 비동기 코드는 I/O 바인딩된 코드와 CPU 바인딩된 코드에 둘 다 사용할 수 있지만  
    시나리오마다 다르게 사용됩니다.
    - 비동기 코드는 백그라운드에서 수행되는 작업을 모델링하는데 사용되는 구문인  
    Task<T> 및 Task를 사용합니다.
    - async 키워드는 본문에서 await 키워드를 사용할 수 있는 비동기 메서드로 메서드를 변환합니다.
    - await 키워드가 적용되면 이 키워드는 호출 메서드를 일시 중단하고 대기 작업이 완료할 때까지 제어 권한을 다시 호출자에게 양도합니다.
    - await는 비동기 메서드 내부에서만 사용.
- 백그라운드 수행
    - 비동기 작업과 관련하여 많은 작업이 수행됩니다.  
    Task 및 Task<T>의 백그라운드에서 수행되는 작업이 궁금하면 세부 비동기 문서에서 자세한 내용을 확인하세요.
    - C#에서는 컴파일러가 해당 코드를, await에 도달할 때 실행을 양도하고  
    백그라운드 작업이 완료될 때 실행을 다시 시작하는 것과 같은 작업을 추적하는 상태 시스템으로 변환합니다.
    - 이론적으로 보면 이 변환은 비동기 약속 모델입니다.
- I/O 바인딩 예제: 웹 서비스에서 데이터 다운로드
    ```c#
    private readonly HttpClient _httpClient = new HttpClient();

    downloadButton.Clicked += async (o, e) =>
    {
        // This line will yield control to the UI as the request
        // from the web service is happening.
        //
        // The UI thread is now free to perform other work.
        var stringData = await _httpClient.GetStringAsync(URL);
        DoSomethingWithData(stringData);
    };
    ```
    - 단추가 눌릴 때 웹 서비스에서 일부 데이터를 다운로드해야 할 수 있지만  
    UI 스레드를 차단하지 않으려고 합니다.
- CPU 바인딩 예제: 게임에 대한 계산 수행
    ```c#
    private DamageResult CalculateDamageDone()
    {
        // Code omitted:
        //
        // Does an expensive calculation and returns
        // the result of that calculation.
    }

    calculateButton.Clicked += async (o, e) =>
    {
        // This line will yield control to the UI while CalculateDamageDone()
        // performs its work. The UI thread is free to perform other work.
        var damageResult = await Task.Run(() => CalculateDamageDone());
        DisplayDamage(damageResult);
    };
    ```
    - 단추를 누르면 화면의 많은 적에게 손상을 입힐 수 있는 모바일 게임을 작성한다고 가정합니다. 손상 계산을 수행하는 것은 부담이 클 수 있고 UI 스레드에서 이 작업을 수행하면 계산이 수행될 때 게임이 일시 중지되는 것처럼 보입니다.
    - 이 작업을 처리하는 가장 좋은 방법은 Task.Run을 사용하여 작업을 수행하는 백그라운드 스레드를 시작하고 await를 사용하여 결과를 기다리는 것입니다. 이렇게 하면 작업이 수행되는 동안 UI가 매끄럽게 느껴질 수 있습니다.
    - 이 코드는 단추 클릭 이벤트의 의도를 표현하고, 백그라운드 스레드를 수동으로 관리할 필요가 없고, 비차단 방식으로 작업을 수행합니다.


https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/concepts/async/task-asynchronous-programming-model
https://docs.microsoft.com/ko-kr/dotnet/csharp/async

## 참고
- [async](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/async)  
- [await](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/operators/await)
- [비동기 프로그래밍](https://docs.microsoft.com/ko-kr/dotnet/csharp/async)
- [async 및 await를 사용한 비동기 프로그래밍](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/concepts/async/)  
- [C# 5.0 : async / await 키워드](http://www.csharpstudy.com/CSharp/CSharp-async-await.aspx)  

- [Async 쓸까말까 및 주의할 점](https://www.youtube.com/watch?v=HkLhKyoh3sI)
- [What's the difference between Task.Start/Wait and Async/Await?](https://stackoverflow.com/questions/9519414/whats-the-difference-between-task-start-wait-and-async-await)
- [작업 기반 비동기 패턴](https://docs.microsoft.com/ko-kr/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [작업 비동기 프로그래밍 모델](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/concepts/async/task-asynchronous-programming-model)

- [What do the terms “CPU bound” and “I/O bound” mean?](https://stackoverflow.com/questions/868568/what-do-the-terms-cpu-bound-and-i-o-bound-mean)

[Futures and promises](https://en.wikipedia.org/wiki/Futures_and_promises)