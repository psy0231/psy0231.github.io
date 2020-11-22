---
title: async/await_1
date: 2020-09-30 18:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 2020-09-30 23:20:36 +0900
---

## async/await

### responsiveness / easy to write
- Asynchronyous는 모든 UI 관련 활동이 일반적으로 하나의 스레드를 공유하기 때문에  
UI 스레드에 액세스하는 애플리케이션에 특히 유용.  
동기식 애플리케이션에서 프로세스가 차단되면 모두 차단됩니다.  
애플리케이션이 응답을 중지하고 대신 대기 중일 때 실패했다고 결론을 내릴 수 있습니다.
- C# 의 async 및 await 키워드가 핵심.  
이 두 키워드를 사용하면 .NET Framework, .NET Core 또는 Windows 런타임의 리소스를 사용하여  
동기 메서드를 만드는 것만 큼 쉽게 비동기 메서드를 만들 수 있습니다.  
async키워드 를 사용하여 정의하는 비동기 메서드를 비동기 메서드 라고 합니다.

### What happens in an async method
- ex) (link)
    ```c#
    public async Task<int> GetUrlContentLengthAsync()
    {
        var client = new HttpClient();

        Task<string> getStringTask =
            client.GetStringAsync("https://docs.microsoft.com/dotnet");

        DoIndependentWork();

        string contents = await getStringTask;

        return contents.Length;
    }

    void DoIndependentWork()
    {
        Console.WriteLine("Working...");
    }
    ```
- 특징   

    메서드 서명.  
    여기에는 async 한정자가 포함됩니다.  
    메서드 이름은 Async로 끝납니다.  
    
    반환 형식.  
    Task\<int>입니다(추가 옵션은 "반환 형식" 섹션 참조).    
    method의 본문에서 GetStringAsync가 Task\<string>을 반환합니다.  
    즉, task를 await하면 string을 받게 됨(contents).  
    
    그 외(flow)  
    작업을 대기하기 전에 GetStringAsync의 string을 사용하지 않는 작업을 수행할 수 있음.  
    await에 대한 부분이 끝나면 그 뒤(이 전에 안한 부분) 이어서 실행
- await 연산자 
    - GetUrlContentLengthAsync를 일시 중단합니다.
    - GetUrlContentLengthAsync는 getStringTask가 완료될 때까지 계속할 수 없습니다.
    - 반면 제어는 GetUrlContentLengthAsync의 호출자에 반환됩니다.
    - getStringTask가 완료되면 컨트롤이 다시 시작됩니다.
    - 그런 다음, await 연산자가 getStringTask에서 string 결과를 검색합니다.
- GetUrlContentLengthAsync에 GetStringAsync 호출과 해당 완료 대기 사이에 수행할 수 있는 작업이 없는 경우 다음 단일 문을 호출하고 대기하여 코드를 단순화할 수 있습니다.
    ```c#
    string contents = await client.GetStringAsync("https://docs.microsoft.com/dotnet");
    ```
- 비동기 메서드 특성 .
    - 메서드 시그니처에 async 한정자가 포함됩니다.
    - 비동기 메서드의 이름은 규칙에 따라 "Async" 접미사로 끝납니다.
    - 반환 형식은 다음 형식 중 하나입니다.
        - 메서드에 연산자 형식이 TResult인 Return 문이 있는 경우 Task<TResult>입니다.
        - 메서드에 반환 문이 포함되지 않았거나 피연산자가 없는 반환 문이 포함된 경우 Task입니다.
        - 비동기 이벤트 처리기를 작성하는 경우 void입니다.
        - GetAwaiter 메서드가 포함된 모든 기타 형식(C# 7.0부터).
    - 메서드는 일반적으로 비동기 작업이 완료될 때까지 메서드가 계속될 수 없는 지점을 표시하는 하나 이상의 await 표현을 포함하고 있습니다.  
    한편, 메서드가 일시 중단되고 컨트롤이 메서드의 호출자로 반환됩니다.  
- 비동기 메서드에서는 제공된 키워드 및 형식을 사용해서 수행하려는 작업을 나타내고,  
컴파일러는 일시 중단된 메서드에서 컨트롤이 대기 지점으로 반환될 때 수행되어야 하는 항목을 추적하는 등의 나머지 작업을 수행합니다.  
루프 및 예외 처리와 같은 일부 루틴 프로세스는 기존의 비동기 코드에서 처리하기 어려울 수 있습니다.  
비동기 메서드에서는 동기 솔루션에서와 같이 필요한 만큼 이러한 요소를 작성하여 문제를 해결합니다.
- 위 예제 순서
    - ![Image](https://raw.githubusercontent.com/psy0231/psy0231.github.io/master/assets/img/C%23/asaw2.png "example")
    1. 호출하는 메서드는 GetUrlContentLengthAsync 비동기 메서드를 호출하고 기다립니다.
    2. GetUrlContentLengthAsync는 HttpClient 인스턴스를 만들고 GetStringAsync 비동기 메서드를 호출하여 웹 사이트의 내용을 문자열로 다운로드합니다.
    3. GetStringAsync에서 특정 작업이 발생하여 진행이 일시 중단됩니다. 웹 사이트에서 다운로드 또는 다른 차단 작업을 수행할 때까지 기다려야 할 수 있습니다. 리소스를 차단하지 않기 위해 GetStringAsync는 해당 호출자인 GetUrlContentLengthAsync에 제어 권한을 양도합니다.
        
        GetStringAsync는 TResult가 문자열인 Task<TResult>를 반환하고, GetUrlContentLengthAsync는 getStringTask 변수에 작업을 할당합니다. 이 작업은 작업이 완료될 때 실제 문자열 값을 생성하기 위한 코드와 함께 GetStringAsync를 호출하는 지속적인 프로세스를 나타냅니다.
    4. getStringTask가 아직 대기되지 않았으므로 GetUrlContentLengthAsync가 GetStringAsync의 최종 결과에 무관한 다른 작업을 계속할 수 있습니다. 이 작업은 동기 메서드 DoIndependentWork를 호출하여 나타냅니다.
    5. DoIndependentWork는 작업을 수행하고 호출자에게 반환하는 동기 메서드입니다.
    6. GetUrlContentLengthAsync에 getStringTask 결과 없이 수행할 수 있는 작업이 없습니다. 다음으로 GetUrlContentLengthAsync는 다운로드한 문자열의 길이를 계산하여 반환하려 하지만, 메서드가 문자열을 확인할 때까지 해당 값을 계산할 수 없습니다.
    
        따라서 GetUrlContentLengthAsync는 await 연산자를 사용해서 해당 프로세스를 일시 중단하고 GetUrlContentLengthAsync를 호출한 메서드에 제어 권한을 양도합니다. GetUrlContentLengthAsync는 Task<int>를 호출자에게 반환합니다. 작업은 다운로드한 문자열의 길이인 정수 결과를 만든다는 약속을 나타냅니다.
        
        참고  
        GetUrlContentLengthAsync가 대기하기 전에 GetStringAsync(및 getStringTask)가 완료되면 GetUrlContentLengthAsync에서 컨트롤이 유지됩니다. 호출된 비동기 프로세스 getStringTask가 이미 완료되었고 GetUrlContentLengthAsync가 최종 결과를 기다릴 필요가 없다면 일시 중단한 다음, GetUrlContentLengthAsync로 돌아가는 비용이 낭비됩니다.
        
        호출하는 메서드 내에서 패턴 처리가 계속됩니다. 호출자가 해당 결과를 기다리거나 즉시 기다리기 전에 GetUrlContentLengthAsync에서 결과에 의존하지 않는 다른 작업을 수행할 수 있습니다. 호출하는 메서드는 GetUrlContentLengthAsync를 기다리고 있으며 GetUrlContentLengthAsync는 GetStringAsync를 기다리고 있습니다.
    7. GetStringAsync가 완료되고 문자열 결과를 생성합니다. GetStringAsync를 호출할 경우 문자열 결과가 예상대로 반환되지 않습니다. (메서드가 이미 3단계에서 작업을 반환했습니다.) 대신 문자열 결과가 메서드 getStringTask의 완료를 나타내는 작업에 저장됩니다. await 연산자가 getStringTask에서 결과를 검색합니다. 할당 문은 검색된 결과를 contents에 할당합니다.
    8. GetUrlContentLengthAsync에 문자열 결과가 있는 경우 메서드가 문자열 길이를 계산할 수 있습니다. 그런 다음 GetUrlContentLengthAsync 작업도 완료되고 대기 이벤트 처리기를 다시 시작할 수 있습니다. 이 항목 뒷부분의 전체 예에서는 이벤트 처리기가 길이 결과 값을 검색하고 출력하는지 확인할 수 있습니다. 비동기 프로그래밍을 처음 접하는 사용자인 경우 동기 동작과 비동기 동작의 차이점을 살펴보세요. 비동기 메서드는 작업이 완료될 때 반환되지만(5단계) 비동기 메서드는 작업이 일시 중단될 때 반환됩니다. 비동기 메서드가 해당 작업을 완료하면 작업이 완료된 것으로 표시되고 결과가 있을 경우 작업에 저장됩니다.

### API 
- GetStringAsync와 같이 비동기 프로그래밍을 지원하는 메서드를 어디에서 검색해야 할지 궁금했을 것입니다. .NET Framework 4.5 이상 및 .NET Core에는 async 및 await에 작동하는 많은 멤버가 포함되어 있습니다. 멤버 이름에 붙는 “Async” 접미사와 Task 또는 Task<TResult>의 반환 형식으로 멤버를 인식할 수 있습니다. 예를 들어, System.IO.Stream 클래스는 동기 메서드인 CopyTo, Read 및 Write와 함께 CopyToAsync, ReadAsync 및 WriteAsync와 같은 메서드를 포함합니다.
- Windows 런타임에는 Windows 앱에서 async 및 await와 함께 사용할 수 있는 많은 메서드도 포함되어 있습니다. 자세한 내용은 UWP 개발의 경우 스레딩 및 비동기 프로그래밍을 참조하세요. Windows 런타임 이전 버전을 사용하는 경우 비동기 프로그래밍(Windows 스토어 앱) 및 빠른 시작: C# 또는 Visual Basic에서 비동기 API 호출을 참조하세요.

### Thread
- 비동기 메서드는 비차단 작업으로 의도되었습니다.  
비동기 메서드의 await 식은 대기한 작업이 실행되는 동안 현재 스레드를 차단하지 않습니다.  
대신에 이 식은 메서드의 나머지를 연속으로 등록하고 제어 기능을 비동기 메서드 호출자에게 반환합니다.
- async 및 await 키워드로 인해 추가 스레드가 생성되지 않습니다.  
비동기 메서드는 자체 스레드에서 실행되지 않으므로 다중 스레드가 필요하지 않습니다.  
메서드는 현재 동기화 컨텍스트에서 실행되고 메서드가 활성화된 경우에만 스레드에서 시간을 사용합니다.  
Task.Run을 사용하여 CPU 바인딩 작업을 백그라운드 스레드로 이동할 수 있지만  
백그라운드 스레드는 결과를 사용할 수 있을 때까지 기다리는 프로세스를 도와주지 않습니다.
- 비동기 프로그래밍에 대한 비동기 기반 접근 방법은 거의 모든 경우에 기존 방법보다 선호됩니다.  
특히, 이 접근 방식은 코드가 더 간단하고 경합 조건을 방지할 필요가 없기 때문에 I/O 바인딩 작업의 BackgroundWorker 클래스보다 효과적입니다.  
비동기 프로그래밍은 코드 실행에 대한 조합 세부 정보를 Task.Run이 스레드 풀로 변환하는 작업과 구분하기 때문에 Task.Run 메서드를 함께 사용하는 비동기 프로그래밍은 CPU 바인딩 작업을 위한 BackgroundWorker보다 효과가 뛰어납니다.

### async 및 await
- async 한정자를 사용해서 메서드를 비동기 메서드로 지정하면 다음 두 기능이 활성화됩니다.
    - 표시된 비동기 메서드는 Await를 사용하여 일시 중단 지점을 지정할 수 있습니다.  
    await 연산자는 대기된 비동기 프로세스가 완료될 때까지 비동기 메서드가 해당 지점을 지나 계속할 수 없도록 컴파일러에 지시합니다.  
    한편, 컨트롤이 비동기 메서드의 호출자로 반환됩니다.
    await 식에서 비동기 메서드를 일시 중단하더라도 메서드가 종료되지는 않으며 finally 블록이 실행되지 않습니다.
    - 표시된 비동기 메서드는 이 메서드를 호출한 다른 메서드에 의해 대기할 수 있습니다.
- 비동기 메서드는 일반적으로 await 연산자를 하나 이상 가지고 있지만, await 식이 없는 경우 컴파일러 오류가 발생하지는 않습니다.  
비동기 메서드에서 await 연산자를 사용하여 일시 중단 시점을 표시하지 않는 경우 메서드가 async 한정자에 상관없이 동기 메서드가 실행되는 방식으로 실행됩니다.  
컴파일러는 해당 메서드에 대해 경고를 표시합니다.

### Async
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
    - Task,Task\<TResult> 
        - 번환 형식이 없고 / 있고 차이.
        - 메소드를 호출하면 작업이 반환되지만, 작업이 완료되면 작업을 기다리는 모든 대기 표현은 무효로 평가된다. 이건잘...
    - void 
        - 호출자가 해당 메서드를 기다릴 수없고 성공적인 완료 또는 오류 조건을 보고하기 위해 다른 메커니즘을 구현해야하므로  
        이벤트 처리기 이외의 코드에는 일반적으로 비동기 method는 void return을 사용하지 않는 것이 좋다.
        - void return type은 주로 이벤트 핸들러를 정의하기 위해 사용하며, 여기에는 해당 반환 유형이 필요하다.  
        void 반환 비동기 방법을 호출한 사람은 기다릴 수 없으며 비동기 방법이 throw하는  exception을 catch할 수 없다.
    - C#7.0부터는 GetAwaiter (System.Threading.Tasks.ValueTask\<TResult>)
        - System.Threading.Tasks.ValueTask\<TResult> 형식.  
        NuGet 패키지 System.Threading.Tasks.Extensions를 추가하면 사용가능.
        - C# 7.0부터 GetAwaiter 메서드가 있는 다른 형식(일반적으로 값 형식)을 반환하여 성능이 중요한 코드 섹션에서 메모리 할당을 최소화함.
    - in, ref, out을 parameter, return으로 사용 못함.  
    단, 이런 parameter가 있는 method는 호출 가능

### Await
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
    - C# 7.1부터 애플리케이션 진입점인 Main 메서드는 Task 또는 Task\<int>를 반환하여 해당 본문에서 await 연산자를 사용할 수 있습니다.  
    이전 C# 버전에서는 Main 메서드가 비동기 작업이 완료될 때까지 대기하는지 확인하기 위해 해당 비동기 메서드에서 반환되는 Task\<TResult> 인스턴스의 Task\<TResult>.Result 속성 값을 검색할 수 있습니다.  
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

### Return types and parameters
- 비동기 메서드는 일반적으로 Task 또는 Task<TResult>를 반환합니다.  
비동기 메서드 내에서 await 연산자는 호출에서 다른 비동기 메서드로 전환되는 작업에 적용됩니다.
- 메서드에 TResult 형식의 피연산자를 지정하는 return 문이 포함되어 있을 경우 Task<TResult>를 반환 형식으로 지정합니다.
- 메서드에 return 문이 없거나 피연산자를 반환하지 않는 return 문이 있을 경우 반환 형식으로 Task를 사용합니다.
- C# 7.0부터는 형식에 GetAwaiter 메서드가 포함된 경우 다른 반환 형식을 지정할 수도 있습니다.  
ValueTask<TResult>가 이러한 형식의 예입니다. System.Threading.Tasks.Extension NuGet 패키지에서 사용할 수 있습니다.
- 다음 예제는 Task<TResult> 또는 Task를 반환하는 메서드를 선언하고 호출하는 방법을 보여줍니다.
    ```c#
    async Task<int> GetTaskOfTResultAsync()
    {
        int hours = 0;
        await Task.Delay(0);

        return hours;
    }


    Task<int> returnedTaskTResult = GetTaskOfTResultAsync();
    int intResult = await returnedTaskTResult;
    // Single line
    // int intResult = await GetTaskOfTResultAsync();

    async Task GetTaskAsync()
    {
        await Task.Delay(0);
        // No return statement needed
    }

    Task returnedTask = GetTaskAsync();
    await returnedTask;
    // Single line
    await GetTaskAsync();
    ```
- 반환된 각 작업은 진행 중인 작업을 나타냅니다. 작업은 비동기 프로세스 상태에 대한 정보를 캡슐화하며, 결과적으로 프로세스의 최종 결과 또는 성공하지 못한 경우 프로세스가 발생시키는 예외에 대한 정보를 캡슐화합니다.
- 비동기 메서드의 반환 형식은 void일 수 있습니다. 이 반환 형식은 기본적으로 void 반환 형식이 필요할 때 이벤트 처리기를 정의하는 데 사용합니다. 비동기 이벤트 처리기는 비동기 프로그램의 시작점 역할을 하는 경우가 많습니다.
- void 반환 형식을 가진 비동기 메서드는 대기할 수 없습니다. 또한 void를 반환하는 메서드의 호출자는 메서드가 throw하는 예외를 catch할 수 없습니다.
- 비동기 메서드는 모든 in, ref 또는 out 매개 변수를 선언할 수 없지만, 이러한 매개 변수가 있는 메서드를 호출할 수는 있습니다. 마찬가지로 비동기 메서드는 참조 반환 값을 사용하여 메서드를 호출할 수 있지만 참조를 통해 값을 반환할 수 없습니다.
- 자세한 내용과 예제는 비동기 반환 형식(C#)을 참조하세요. 비동기 메서드에서 예외를 처리하는 방법에 대한 자세한 내용은 try-catch를 참조하세요.
- Windows 런타임 프로그래밍의 비동기 API에는 작업과 유사한 다음 반환 형식 중 하나가 있습니다.
    - Task<TResult>에 해당하는 IAsyncOperation<TResult>
    - Task에 해당하는 IAsyncAction
    - IAsyncActionWithProgress<TProgress>
    - IAsyncOperationWithProgress<TResult,TProgress>
### 명명 규칙
- 규칙에 따라 일반적으로 대기 가능한 형식(예: Task, Task<T>, ValueTask, ValueTask<T>)을 반환하는 메서드에는 “Async”로 끝나는 이름을 사용해야 합니다.  
비동기 작업을 시작하지만 대기 가능한 형식을 반환하지 않는 메서드는 "Async"로 끝나는 이름을 사용하지 않아야 하지만,  
"Begin", "Start" 또는 일부 다른 동사로 시작하여 이 메서드가 작업 결과를 반환하거나 예외가 발생하지 않음을 알려야 합니다.
- 여기서 이벤트, 기본 클래스 또는 인터페이스 계약으로 다른 이름을 제안하는 규칙을 무시할 수 있습니다.  
예를 들어, OnButtonClick과 같은 공용 이벤트 처리기의 이름을 변경할 수 없습니다.

### 관련 항목 및 샘플(Visual Studio)

|제목|설명|예제  
|:---:|:---:|:---:|  
|async 및 await를 사용하여 병렬로 여러 웹 요청을 만드는 방법(C#)	|동시에 여러 작업을 시작하는 방법을 보여줍니다.	|비동기 샘플: 병렬로 여러 웹 요청 만들기
|비동기 반환 형식(C#)	|비동기 메서드에서 반환할 수 있는 형식을 설명하고 각 형식이 언제 적절한가를 설명합니다.	
|신호 메커니즘으로 취소 토큰이 있는 작업을 취소합니다.	|비동기 솔루션에 다음과 같은 기능을 추가하는 방법을 보여줍니다.  
||- 작업 목록 취소(C#)  
||- 일정 기간 이후 작업 취소(C#)  
||- 완료되면 비동기 작업 처리(C#)
|파일 액세스에 async 사용(C#)	|async 및 await를 사용하여 파일에 액세스하는 이점을 나열하고 보여줍니다.	
|TAP(작업 기반 비동기 패턴)	    |비동기 패턴에 대해 설명하고 패턴은 Task 및 Task<TResult> 형식을 기반으로 합니다.	
|비동기 Channel 9 비디오	    |비동기 프로그래밍에 대한 다양한 비디오로 연결되는 링크를 제공합니다.
---
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
### 상황에 따른 예
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
- 네트워크에서 데이터 추출
    ```c#
    private readonly HttpClient _httpClient = new HttpClient();

    [HttpGet, Route("DotNetCount")]
    public async Task<int> GetDotNetCount()
    {
        // Suspends GetDotNetCount() to allow the caller (the web server)
        // to accept another request, rather than blocking on this one.
        var html = await _httpClient.GetStringAsync("https://dotnetfoundation.org");

        return Regex.Matches(html, @"\.NET").Count;
    }
    ```
    - https://dotnetfoundation.org에서 HTML을 다운로드하고 문자열 ".NET"이 HTML에서 발생하는 횟수를 계산합니다.  
    이 작업을 수행하고 횟수를 반환하는 Web API 컨트롤러 메서드를 정의하기 위해 ASP.NET을 사용합니다.

    ```c#
    private readonly HttpClient _httpClient = new HttpClient();

    private async void OnSeeTheDotNetsButtonClick(object sender, RoutedEventArgs e)
    {
        // Capture the task handle here so we can await the background task later.
        var getDotNetFoundationHtmlTask = _httpClient.GetStringAsync("https://dotnetfoundation.org");

        // Any other work on the UI thread can be done here, such as enabling a Progress Bar.
        // This is important to do here, before the "await" call, so that the user
        // sees the progress bar before execution of this method is yielded.
        NetworkProgressBar.IsEnabled = true;
        NetworkProgressBar.Visibility = Visibility.Visible;

        // The await operator suspends OnSeeTheDotNetsButtonClick(), returning control to its caller.
        // This is what allows the app to be responsive and not block the UI thread.
        var html = await getDotNetFoundationHtmlTask;
        int count = Regex.Matches(html, @"\.NET").Count;

        DotNetCountLabel.Text = $"Number of .NETs on dotnetfoundation.org: {count}";

        NetworkProgressBar.IsEnabled = false;
        NetworkProgressBar.Visibility = Visibility.Collapsed;
    }
    ```
    - 버튼이 눌릴 때 같은 작업을 수행하는 유니버설 Windows 앱용으로 작성된 동일한 시나리오입니다.
- 여러 작업이 완료될 때까지 대기
    - 동시에 데이터의 여러 부분을 검색해야 하는 상황이 될 수 있습니다.  
    Task API에는 여러 백그라운드 작업에서 비차단 대기를 수행하는  
    비동기 코드를 작성할 수 있는 Task.WhenAll 및 Task.WhenAny 메서드가 포함됩니다.

    ```c#
    public async Task<User> GetUserAsync(int userId)
    {
        // Code omitted:
        //
        // Given a user Id {userId}, retrieves a User object corresponding
        // to the entry in the database with {userId} as its Id.
    }

    public static async Task<IEnumerable<User>> GetUsersAsync(IEnumerable<int> userIds)
    {
        var getUserTasks = new List<Task<User>>();
        foreach (int userId in userIds)
        {
            getUserTasks.Add(GetUserAsync(userId));
        }

        return await Task.WhenAll(getUserTasks);
    }
    ```
    - 이 예제에서는 userId 집합에 대한 User 데이터를 확인하는 방법을 보여 줍니다.

    ```c#
    public async Task<User> GetUserAsync(int userId)
    {
        // Code omitted:
        //
        // Given a user Id {userId}, retrieves a User object corresponding
        // to the entry in the database with {userId} as its Id.
    }

    public static async Task<User[]> GetUsersAsync(IEnumerable<int> userIds)
    {
        var getUserTasks = userIds.Select(id => GetUserAsync(id));
        return await Task.WhenAll(getUserTasks);
    }
    ```
    - LINQ를 사용하여 이 코드를 보다 간결하게 작성하는 또 다른 방법입니다.
    - 코드 양은 더 적지만 LINQ를 비동기 코드와 함께 사용할 때는 주의하세요.  
    LINQ는 연기된(지연) 실행을 사용하므로,  
    .ToList() 또는 .ToArray() 호출을 반복하도록 생성된 시퀀스를 적용해야  
    비동기 호출이 foreach 루프에서 수행되면 즉시 비동기 호출이 발생합니다.

### caution
- 비동기 프로그래밍을 사용하는 경우 예기치 않은 동작을 방지할 수 있는 몇 가지 세부 사항을 염두에 두어야 합니다.
    - async 메서드에는 본문에 await 키워드가 있어야 합니다. 키워드가 없으면 일시 중단되지 않습니다.
    기억해야 할 중요한 정보입니다. await가 async 메서드의 본문에서 사용되지 않으면 C# 컴파일러가 경고를 생성하지만  
    코드는 일반 메서드인 것처럼 컴파일 및 실행됩니다.  
    이는 C# 컴파일러가 비동기 메서드에 대해 생성한 상태 시스템이 아무것도 수행하지 않기 때문에 매우 비효율적입니다.
- 작성하는 모든 비동기 메서드 이름의 접미사로 "Async"를 추가해야 합니다.
    - 이 규칙을 .NET에서 사용하여 동기 및 비동기 메서드를 더 쉽게 구별할 수 있습니다.  
    코드에서 명시적으로 호출되지 않은 특정 메서드(예: 이벤트 처리기 또는 웹 컨트롤러 메서드)가 반드시 적용되는 것은 아닙니다.   
    이러한 메서드는 코드에서 명시적으로 호출되지 않으므로 명시적으로 명명하는 것은 별로 중요하지 않습니다.
- async void는 이벤트 처리기에만 사용해야 합니다.
    - 이벤트에는 반환 형식이 없어서 Task 및 Task<T>를 사용할 수 없으므로  
    비동기 이벤트 처리기가 작동하도록 허용하는 유일한 방법은 async void입니다.  
    async void의 다른 사용은 TAP 모델을 따르지 않고 다음과 같이 사용이 어려울 수 있습니다.
    - async void 메서드에서 throw된 예외는 해당 메서드 외부에서 catch될 수 없습니다.
    - async void 메서드는 테스트하기가 어렵습니다.
    - 호출자가 async void 메서드를 비동기로 예상하지 않을 경우  
    이러한 메서드는 의도하지 않은 잘못된 결과를 일으킬 수 있습니다.
- LINQ 식에서 비동기 람다를 사용할 경우 신중하게 스레드
    - LINQ의 람다 식은 연기된 실행을 사용합니다. 즉, 예상치 않은 시점에 코드 실행이 끝날 수 있습니다.  
    이 코드에 차단 작업을 도입하면 코드가 제대로 작성되지 않은 경우 교착 상태가 쉽게 발생할 수 있습니다.  
    또한 이 코드처럼 비동기 코드를 중첩하면 코드 실행에 대해 추론하기가 훨씬 더 어려울 수도 있습니다.  
    비동기 및 LINQ는 강력하지만 가능한 한 신중하고 분명하게 함께 사용되어야 합니다.
- 비차단 방식으로 작업을 기다리는 코드 작성
    - Task가 완료될 때까지 대기하는 수단으로 현재 스레드를 차단하면  
    교착 상태가 발생하고 컨텍스트 스레드가 차단될 수 있고 더 복잡한 오류 처리가 필요할 수 있습니다.  
    다음 표에서는 비차단 방식으로 작업 대기를 처리하는 방법에 대한 지침을 제공합니다.
    -   |사용 방법	         |대체 방법	                     |수행할 작업
        |:---|:---|:---|
        |await	            |Task.Wait 또는 Task.Result	    |백그라운드 작업의 결과 검색
        |await Task.WhenAny	|Task.WaitAny	                |작업이 완료될 때까지 대기
        |await Task.WhenAll	|Task.WaitAll	                |모든 작업이 완료될 때까지 대기
        |await Task.Delay	|Thread.Sleep	                |일정 기간 대기


- 가능하면 ValueTask를 사용
    - 비동기 메서드에서 Task 개체를 반환하면 특정 경로에 성능 병목 현상이 발생할 수 있습니다.  
    Task는 참조 형식이므로 이를 사용하는 것은 개체 할당을 의미합니다.  
    async 한정자로 선언된 메서드가 캐시된 결과를 반환하거나 동기적으로 완료된 경우  
    코드의 성능이 중요한 섹션에서 추가 할당에 상당한 시간이 소요될 수 있습니다.  
    연속 루프에서 이러한 할당이 발생하면 부담이 될 수 있습니다.  
    자세한 내용은 일반화된 비동기 반환 형식을 참조하세요.
- ConfigureAwait(false)를 사용
    - 일반적인 질문은 "언제 Task.ConfigureAwait(Boolean) 메서드를 사용해야 하는가"입니다.  
    이 메서드를 사용하면 Task 인스턴스가 awaiter를 구성할 수 있습니다.  
    이는 중요한 고려 사항이며 잘못 설정할 경우 성능에 영향을 미칠 수 있고  
    심지어 교착 상태가 발생할 수도 있습니다.  
    ConfigureAwait에 대한 자세한 내용은 ConfigureAwait FAQ를 참조하세요.
- 상태 저장 코드 작성 분량 감소
    - 전역 개체의 상태나 특정 메서드의 실행에 의존하지 마세요.  
    대신, 메서드의 반환 값에만 의존합니다. 이유
        - 코드를 더 쉽게 추론할 수 있습니다.
        - 코드를 더 쉽게 테스트할 수 있습니다.
        - 비동기 및 동기 코드를 훨씬 더 쉽게 혼합할 수 있습니다.
        - 일반적으로 함께 경합 상태를 피할 수 있습니다.
        - 반환 값에 의존하면 비동기 코드를 간단히 조정할 수 있습니다.
        - (이점) 이 방법은 실제로 종속성 주입에도 잘 작동합니다.
- 권장되는 목적은 코드에서 완전하거나 거의 완전한 참조 투명성을 달성하는 것입니다.  
이렇게 하면 확실히 예측 가능하고, 테스트 가능하고, 유지 관리 가능한 코드베이스가 생성됩니다.

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