---
title: async/await_3 Excention 1
date: 2021-03-30 18:00:00 +0900
categories: [Grind, C#]
tags: [c#, asynchronous]
seo:
  date_modified: 2021-03-30 14:27:12 +0900
---

## AS/AW Example 
- 필요해서 직접 만들었던 예시와 공부하면서 봤던 내용들이 충돌해서 ..
- [source](https://gitlab.com/psy72006300/Test_Reference/-/tree/master/ASAW)
- ![Image](https://raw.githubusercontent.com/psy0231/psy0231.github.io/master/assets/img/C%23/asaw3.jpg "example")
- text파일을 읽고 table 형태로 만들고 grid에 넣음.  
실제 오래 걸리던 데이터에서 1분정도만 잡아먹게 줄이고 해봄.  
- 하던 과정 그대로 일단.

### example 1
```c#
// 실제 파일을 읽고 취합 후 구조화한 후, 
// DataTable를 return.
// async로 작업할 부분.
DataTable datamaker(){...}

//datamaker를 호출하는 부분
async Task<DataTable> asyncmtd_2()
{
    DataTable res = null;

    res = await Task<DataTable>.Run(() =>
    {
        return datamaker();
    });
    return res;
}

//다시, asyncmtd()를 호출하는부분
async void asyncmtd_1()
{
    try
    {
        Task<DataTable> t = asyncmtd_2();
        DataTable dt = await asyncmtd_2();
        this.dataGridView1.DataSource = dt;

        //throw new InvalidOperationException();
    }
    catch (Exception)
    {

        throw;
    }
    
}

private async void btn_async_Click(object sender, EventArgs e)
{
    this.textBox1.AppendText(Environment.NewLine + DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss.fff"));

    asyncmtd_1();

    Task<DataTable> t = asyncmtd_2();
    DataTable dt = await t;
    this.dataGridView1.DataSource = dt;

    this.textBox1.AppendText(Environment.NewLine + DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss.fff"));

}
```
- 처음에 만든게 이런식이었는데 이렇게 된 이유는  
ascyn method는 task를 return하도록하고 void는 권장하지 않음.  
단, event는 제외. 관련 내용은 이 전 포스팅에 있음.  
그런데 method내부에 await가 있으면 해당 method는 무조건 async가 붙어야함 (컴파일에러)
- asyncmtd_2는 위 조건에 따라 만들어지는데  
이건 또 일반 method에서는 호출을 못함.  
일반 method에서 호출을하려면 task, await를 써야하는데  
이러면 method에 또 async가 붙어야 하고  
또 Task를 return하게 바꾸게됨.  
- 이런식으로 계속 같은 method내용을 계속 쓰게되는데  
위 사항을 지켜가면서 만들면 끝이 안남.  
async가 번지는느낌. 결국 위처럼 void로 끝냄.  
근데 void로 끝내면 일반 method처럼 호출가능.  
이렇게 작성된게 asyncmethod_1.
- 단, event는 async void가 혀용된다고 했으니 이 점을 감안하면  
asyncmethod_2도 위처럼 호출해쓸 수 있음.
그런데 꼭 event에 의해 사용하고싶지는 않으니... 

### example 2
```c#
DataTable datamaker(){...}

async void asyncmtd_3()
{
    try
    {

        DataTable res = await Task<DataTable>.Run(() =>
        {
            return datamaker();
        });
        this.dataGridView1.DataSource = res;

    }
    catch (Exception ex)
    {
        Console.WriteLine(ex);
        this.textBox1.AppendText(ex.Message);
    }
}

private void btn_async_Click(object sender, EventArgs e)
{
    this.textBox1.AppendText(Environment.NewLine + DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss.fff"));

    asyncmtd_3();
    this.textBox1.AppendText(Environment.NewLine + DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss.fff"));

}
```
- ex1에서 asyncmtd_2처럼 쓰는데 return을 void로 하면  
다른 method처럼 호출 가능.  
다만 이 경우 exceptioon이 안잡히니 void형은 권장하지 않는 거였는데  
해보니 또 잘 잡힘...  
일단은 이렇게 써도 되는걸로? 

## Async/Await - Best Practices in Asynchronous Programming[^footnote1]
- 관련해서 좀 더 설명이 있는것같아 찾아봄.
- 가이드 라인 정도로 생각하라함.

### Avoid Async Void
- 비동기 메서드에는 Task, Task\<T> 및 void의 세 가지 가능한 반환 형식이 있지만  
비동기 메서드의 자연적인 반환 형식은 Task 및 Task\<T>임.  
동기 코드에서 비동기 코드로 변환 할 때,  
T 형식을 반환하는 모든 메서드는 Task\<T>를 반환하는 비동기 메서드가 되고  
void를 반환하는 모든 메서드는 Task를 반환하는 비동기 메서드가됨. 예를들어  

    ```c#
    void MyMethod()
    {
    // Do synchronous work.
    Thread.Sleep(1000);
    }
    async Task MyMethodAsync()
    {
    // Do asynchronous work.
    await Task.Delay(1000);
    }
    ```
- void 반환 비동기 메서드에는 비동기 이벤트 처리기를 가능하게하는 특정 목적이 있다.  
실제 유형을 반환하는 이벤트 처리기를 가질 수 있지만 언어에서는 제대로 작동하지 않는다.  
유형을 반환하는 이벤트 핸들러를 호출하는 것은 매우 어색하며  
실제로 무언가를 반환하는 이벤트 핸들러의 개념은별로 의미가 없음.  
이벤트 핸들러는 자연스럽게 void를 반환하므로 
비동기 이벤트 핸들러를 가질 수 있도록 비동기 메소드가 void를 반환합니다.
그러나 async void 메서드의 일부 의미는  
async Task 또는 async Task\<T> 메서드의 의미와 미묘하게 다르다.
- Async void 메서드는 오류 처리 의미가 다르다.  
비동기 작업 또는 비동기 Task\<T> 메서드에서 예외가 throw되면  
해당 예외가 캡처되어 Task 개체에 배치함.  
async void 메서드를 사용하면 Task 개체가 없으므로  
async void 메서드에서 throw 된 모든 예외는  
async void 메서드가 시작될 때 활성화 된 SynchronizationContext에서 직접 발생.  
다음은 비동기 void 메서드에서 throw 된 exception을 catch못한 경우.

    ```c#
    // #1
    private async void ThrowExceptionAsync()
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
    ```
    - 이러한 예외는 AppDomain.UnhandledException 또는  
    GUI / ASP.NET 응용 프로그램에 대한 유사한  
    catch-all 이벤트를 사용하여 관찰 할 수 있지만  
    이러한 이벤트를 일반 예외 처리에 사용하면 유지 관리가 불가능해짐.
- async void 메서드는 구성 의미가 다르다고 했다.  
Task 또는 Task\<T>를 반환하는 비동기 메서드는  
await, Task.WhenAny, Task.WhenAll 등을 사용하여 쉽게 구성 할 수 있는반면,  
void를 반환하는 비동기 메서드는 호출 코드에 완료를 알리는 쉬운 방법이 없다.  
여러 async void 메서드를 시작하는 것은 쉽지만 완료 시점 확인이 어렵다.  
비동기 void 메서드는 시작 및 완료 될 때 SynchronizationContext에 알리지만  
사용자 지정 SynchronizationContext는 일반 애플리케이션 코드를 위한 복잡한 방법임.
- Async void 메서드는 테스트하기가 어려움.  
오류 처리 및 구성의 차이로 인해  
비동기 void 메서드를 호출하는 단위 테스트를 작성하기가 어렵다.  
MSTest 비동기 테스트 지원은  
Task 또는 Task\<T>를 반환하는 비동기 메서드에 대해서만 작동함.  
모든 비동기 void 메서드가 완료되었을 때를 감지하고  
예외를 수집하는 SynchronizationContext를 설치할 수 있지만,  
비동기 void 메서드가 대신 Task를 반환하도록하는 것이 훨씬 쉽다.
- async void 메서드는 async Task 메서드에 비해 단점이 있지만,  
비동기 이벤트 처리기라는 특정 경우에 매우 유용함.  
의미 체계의 차이는 비동기 이벤트 처리기에서 의미가 있다.  
동기 이벤트 처리기가 작동하는 방식과 유사한  
SynchronizationContext에서 직접 예외를 발생시킨다.  
동기식 이벤트 핸들러는 일반적으로 비공개이므로 구성하거나 직접 테스트 할 수 없다.  
여기에서(?) 선호하는 접근 방식은 비동기 이벤트 처리기 코드의 최소화.  
예를 들어 실제 논리가 포함 된 비동기 작업 메서드를 기다리도록한다.  
다음 코드는 테스트 가능성을 희생하지 않고  
이벤트 처리기에 비동기 void 메서드를 사용하는이 접근 방식의 예시.

    ```c#
    // #2
    // Async Void 메서드의 예외는 Catch로 포착 할 수 없다.
    private async void button1_Click(object sender, EventArgs e)
    {
        await Button1ClickAsync();
    }
    public async Task Button1ClickAsync()
    {
        // Do asynchronous work.
        await Task.Delay(1000);
    }
    ```
- 호출자가 비동기 일 것으로 예상하지 않는 경우 비동기 void 메서드는 혼란을 일으킬 수 있다.  
반환 유형이 Task이면 호출자는 향후 작업을 처리하고 있음을 알고 있지만  
반환 유형이 void이면 호출자는 반환 할 때까지 메서드가 완료되었다고 가정 할 수 있다.  
이 문제는 예기치 않은 여러 가지 방식으로 발생할 수 있다.  
인터페이스 (또는 기본 클래스)에서 void 반환 메서드의  
비동기 구현(또는 재정의)을 제공하는 것은 일반적으로 잘못됨.  
일부 이벤트는 반환 할 때 처리기가 완료되었다고 가정한다.  
한 가지 미묘한 트랩은 비동기 람다를 Action 매개 변수를 사용하는 메서드에 전달하는 것.  
이 경우 비동기 람다는 void를 반환하고 비동기 void 메서드의 모든 문제를 상속한다.  
일반적으로 비동기 람다는 Task를 반환하는 대리자 형식(예 : Func\<Task>)으로 변환 된 경우에만 사용해야한다.

- 요약하면 async void보다 async Task를 강권함.  
비동기 작업 메서드를 사용하면 오류 처리, 구성 가능성 및 테스트 가능성이 더 쉬움.  
이 가이드 라인의 예외는 void를 반환해야하는 비동기 이벤트 처리기이며  
이 예외에는 문자 그대로 이벤트 처리기가 아니더라도 논리적으로  
이벤트 처리기 인 메서드(예 : ICommand.Execute 구현)가 포함.

### Async All the Way
- 동기식 코드를 비동기식 코드로 변환할 때  
비동기식 코드가 호출될 경우 가장 잘 작동하고  
다른 비동기식 코드에 의해 호출되며,  
이 코드는 하향식(또는 "상향식")으로 호출된다.
다른 사람들은 또한 비동기 프로그래밍의 확산 동작을 발견하고  
이를 "전염성"이라고 부르거나 좀비 바이러스와 비교했다.  
비동기 코드는 주변 코드를 비동기로 만드는 경향이 있다.
- “Async all the way”는 결과를 신중하게 고려하지 않고  
동기 코드와 비동기 코드를 혼합해서는 안된다는 의미.  
특히 Task.Wait 또는 Task.Result를 호출하여  
비동기 코드를 차단하는 것은 일반적으로 좋지 않다.  
이는 애플리케이션의 작은 부분 만 변환하고 이를 동기 API로 래핑하여  
나머지 애플리케이션이 변경 사항으로부터 격리되도록  
"발가락"을 비동기 프로그래밍으로 전환하는 프로그래머에게 특히 일반적인 문제다.  
이는 교착 상태를 유발할 가능성이 커짐.  
- 다음은 하나의 메서드가 비동기 메서드의 결과를 차단하는 간단한 예.  
이 코드는 콘솔 응용 프로그램에서 잘 작동하지만  
GUI 또는 ASP.NET 컨텍스트에서 호출되면 교착 상태가 됨.  
이 동작은 혼란 스러울 수 있는데, 특히 디버거를 단계별로 진행하는 것이  
완료되지 않는 대기라는 것을 의미한다는 점을 고려할 때 특히 그렇다.  
교착 상태의 실제 원인은 Task.Wait가 호출 될 때 호출 스택보다 더 높다.

    ```c#
    // #3
    // 비동기 코드에서 차단할 때 발생하는 일반적인 교착 상태 문제
    public static class DeadlockDemo
    {
        private static async Task DelayAsync()
        {
            await Task.Delay(1000);
        }
        // This method causes a deadlock when called in a GUI or ASP.NET context.
        public static void Test()
        {
            // Start the delay.
            var delayTask = DelayAsync();
            // Wait for the delay to complete.
            delayTask.Wait();
        }
    }
    ```
- 이 교착 상태의 근본 원인은 await가 컨텍스트를 처리하는 방식 때문인데,  
기본적으로 완료되지 않은 작업이 대기하면  
현재 "컨텍스트"가 캡처되어 작업이 완료 될 때 메서드를 재개하는 데 사용됨.  
이 "컨텍스트"는 null이 아닌 경우  
현재 SynchronizationContext이며, 이 경우 현재 TaskScheduler임.  
GUI 및 ASP.NET 응용 프로그램에는  
한 번에 하나의 코드 청크 만 실행할 수있는 SynchronizationContext가 있다.  
await가 완료되면 캡처 된 컨텍스트 내에서  
나머지 비동기 메서드를 실행하려함.  
그러나 해당 컨텍스트에는 이미 스레드가 있으며  
비동기 메서드가 완료되기를(동기적으로) 기다리고 있다.  
이 둘은 서로를 기다리고있어 교착 상태가 발생.
- 콘솔 응용 프로그램은이 교착 상태를 일으키지 않는다.  
한 번에 하나의 청크 SynchronizationContext 대신  
스레드풀 SynchronizationContext가 있으므로  
대기가 완료되면 스레드풀 스레드에서 나머지 비동기 메서드를 예약한다.  
메서드는 완료 할 수 있으며 반환 된 작업을 완료하며 교착 상태가 없다.  
이러한 동작의 차이는 프로그래머가 테스트 콘솔 프로그램을 작성하고  
부분적으로 비동기 코드가 예상대로 작동하는 것을 관찰 한 다음  
동일한 코드를 GUI 또는 ASP.NET 응용 프로그램으로 이동하여  
교착 상태가되는 경우 혼란 스러울 수 있습니다.
- 이 문제에 대한 최선의 해결책은  
코드베이스를 통해 비동기 코드가 자연스럽게 성장하도록하는 것.  
이 솔루션을 따르면 비동기 코드가  
진입점 (일반적으로 이벤트 처리기 또는 컨트롤러 작업)으로 확장된다.  
Main 메서드가 비동기 일 수 없기 때문에  
콘솔 응용 프로그램은 이 솔루션을 완전히 따를 수 없다.  
Main 메서드가 비동기 인 경우  
완료되기 전에 반환되어 프로그램이 종료 될 수 있다.  
다음은 지침에 대한이 예외로 콘솔 응용 프로그램의 Main 메서드는  
코드가 비동기 메서드에서 차단 될 수 있는 몇 안되는 상황 중 하나.

    ```c#
    // #4
    // Main 메서드는 Task.Wait 또는 Task.Result를 호출 할 수 있습니다.
    class Program
    {
        static void Main()
        {
            MainAsync().Wait();
        }
        static async Task MainAsync()
        {
            try
            {
                // Asynchronous implementation.
                await Task.Delay(1000);
            }
            catch (Exception ex)
            {
                
                // Handle exceptions.
            }
        }
    }
    ```
- 코드베이스를 통해 비동기가 성장하도록  
허용하는 것이 최상의 솔루션이지만,  
이는 애플리케이션이 비동기 코드의  
실질적인 이점을 확인하기위한  
많은 초기 작업이 있음을 의미한다.  
대규모 코드베이스를 비동기 코드로  
점진적으로 변환하는 몇 가지 기술이 있지만  
이 문서의 범위를 벗난다.  
경우에 따라 Task.Wait 또는 Task.Result를 사용하면  
부분 변환에 도움이 될 수 있지만  
교착 상태 문제와 오류 처리 문제를 알고 있어야한다.  
뒤는 오류 처리 문제를 설명하고  
이 글의 뒤에서는 교착 상태 문제를 피하는 방법을 설명함.
- 모든 작업은 예외 목록을 저장한다.  
태스크를 기다릴 때 첫 번째 예외가 다시 발생하므로  
특정 예외 유형 (예 : InvalidOperationException)을 포착 할 수 있다.  
그러나 Task.Wait 또는 Task.Result를 사용하여 Task를 동기적으로 차단하면  
모든 예외가 AggregateException으로 래핑되고 throw된다.  
다시 위 예시를 참조하면,    
MainAsync의 try/catch는 특정 예외 유형을 포착하지만,  
Main에 try/catch를 넣으면 항상 AggregateException을 포착한다.  
AggregateException이 없으면 오류 처리가 훨씬 더 쉽게 처리 할 수 있으므로  
MainAsync에 "global" try / catch를 넣음.
- 지금까지 비동기 코드 차단과 관련된 두 가지 문제,  
즉 가능한 교착 상태와 더 복잡한 오류 처리를 보았다.  
다음 예는 비동기 메서드 내에서 차단 코드를 사용할 때의 문제.  

    ```c#
    // #5
    public static class NotFullyAsynchronousDemo
    {
        // This method synchronously blocks a thread.
        public static async Task TestNotFullyAsync()
        {
            await Task.Yield();
            Thread.Sleep(5000);
        }
    }
    ```
- 위 방법은 완전히 비동기식이 아니다.  
즉시 양보하여 불완전한 작업을 반환하지만  
재개되면 실행중인 스레드를 동기적으로 차단한다.  
이 메소드가 GUI 컨텍스트에서 호출되면 GUI 스레드를 차단.  
ASP.NET 요청 컨텍스트에서 호출되면  
현재 ASP.NET 요청 스레드를 차단.  
비동기 코드는 동기적으로 차단되지 않는 경우 가장 잘 작동.  
다음은 동기식 작업을위한 비동기식 대체 방법.
    |To Do This …                             |Instead of This …         |Use This
    |:---|:--|:---|
    |Retrieve the result of a background task | Task.Wait or Task.Result | await
    |Wait for any task to complete            | Task.WaitAny             | await Task.WhenAny
    |Retrieve the results of multiple tasks   | Task.WaitAll             | await Task.WhenAll
    | Wait a period of time                   | Thread.Sleep             | await Task.Delay
- 요약하면  
비동기 코드와 차단 코드를 혼합하지 않아야한다.  
혼합 된 비동기 및 차단 코드는 교착 상태,  
더 복잡한 오류 처리 및 예기치 않은 컨텍스트 스레드 차단을 유발할 수 있다.  
이 가이드 라인의 예외는 콘솔 응용 프로그램을 위한 Main 메서드 또는  
고급 사용자 인 경우 부분적으로 비동기 코드베이스를 관리하는 것입니다.

### Configure Context
- 이 전 내용에서 불완전한 작업이 대기 할 때  
기본적으로 "컨텍스트"가 캡처되는 방법과  
해당 컨텍스트를 사용하여 비동기 메서드를  
다시 시작하는 방법에 대해 간단히 설명했다.  
#3의 예시는 컨텍스트에서 재개가  
동기 차단과 충돌하여 교착 상태를 유발하는지 보였다.  
이 컨텍스트 동작은 성능 문제를 일으킬 수도 있다.  
비동기 GUI 애플리케이션이 커짐에 따라  
GUI 스레드를 컨텍스트로 사용하는  
비동기 메소드의 많은 작은 부분을 찾을 수 있다.  
이로 인해 "수천 장의 종이 절단"으로 인해  
응답 성이 저하되므로 느려질 수 있다.
(This can cause sluggishness as responsiveness suffers  
from “thousands of paper cuts.”)
- 이를 완화하려면 가능할 때마다 ConfigureAwait의 결과를 기다린다.  
다음 예는 기본 컨텍스트 동작과 ConfigureAwait의 사용방법.

    ```c# 
    async Task MyMethodAsync()
    {
        // Code here runs in the original context.
        await Task.Delay(1000);
        // Code here runs in the original context.
        await Task.Delay(1000).ConfigureAwait(
            continueOnCapturedContext: false);
        // Code here runs without the original
        // context (in this case, on the thread pool).
    }
    ```
    - ConfigureAwait는 적은 양의 병렬 처리를 활성화 할 수 있다.  
    일부 비동기 코드는 수행 할 작업으로 끊임없이  
    배지를 표시하는 대신 GUI 스레드와 병렬로 실행할 수 있다.
- 성능 외에도 ConfigureAwait의 또 다른 중요한점은   
교착 상태를 피할 수 있게 한다는것.  
#3에서 DelayAsync의 코드 줄에  
"ConfigureAwait(false)"를 추가하면 교착 상태가 방지된다.  
이번에는 await가 완료되면 스레드풀 컨텍스트 내에서  
나머지 비동기 메서드를 실행하려한다.  
메서드는 완료 할 수 있으며  
반환 된 작업을 완료하며 교착 상태가 없다.  
이 기술은 응용 프로그램을 동기에서 비동기로  
점진적으로 변환해야하는 경우 특히 유용하다.
- 메서드 내에서 어느 시점에서 ConfigureAwait를 사용할 수 있다면  
해당 시점 이후에 해당 메서드의 모든 await에 대해 사용하는 것이 좋다.  
불완전한 작업이 대기하는 경우에만 컨텍스트가 캡처된다.  
태스크가 이미 완료된 경우 컨텍스트가 캡처되지 않는다.  
일부 작업은 다른 하드웨어 및 네트워크 상황에서  
예상보다 빨리 완료 될 수 있으며,  
기다리기 전에 완료되는 반환 된 작업을 정중하게 처리해야한다.  
#6 은 수정 된 예.

    ```c#
    // #6 
    // 기다리기 전에 완료되는 반환 된 작업 처리
    async Task MyMethodAsync()
    {
        // Code here runs in the original context.
        await Task.FromResult(1);
        // Code here runs in the original context.
        await Task.FromResult(1).ConfigureAwait(continueOnCapturedContext: false);
        // Code here runs in the original context.
        var random = new Random();
        int delay = random.Next(2); // Delay is either 0 or 1
        await Task.Delay(delay).ConfigureAwait(continueOnCapturedContext: false);
        // Code here might or might not run in the original context.
        // The same is true when you await any Task
        // that might complete very quickly.
    }
    ```
- 컨텍스트가 필요한 메서드에서  
await 후에 코드가있는 경우 ConfigureAwait를 사용하면 안됨.  
GUI 앱의 경우 여기에는 GUI 요소를 조작하거나  
데이터 바인딩 된 속성을 작성하거나  
Dispatcher/CoreDispatcher와 같은  
GUI 특정 유형에 의존하는 모든 코드가 포함된다.  
ASP.NET 앱의 경우 여기에는 컨트롤러 작업의 반환 문을 포함하여  
HttpContext.Current를 사용하거나  
ASP.NET 응답을 빌드하는 모든 코드가 포된다.  
#7 은 GUI 앱의 한 가지 일반적인 패턴을 보여준다.  
비동기 이벤트 처리기가 메서드 시작시 제어를 비활성화하고  
일부 대기를 수행 한 다음 처리기 끝에서 제어를 다시 활성화한다.  
이벤트 핸들러는 제어를 다시 활성화해야하므로 컨텍스트를 포기할 수 없다.

    ```c#
    // #7 
    // 비동기 이벤트 처리기가 제어를 비활성화하고 다시 활성화하도록 설정

    private async void button1_Click(object sender, EventArgs e)
    {
        button1.Enabled = false;
        try
        {
            // Can't use ConfigureAwait here ...
            await Task.Delay(1000);
        }
        finally
        {
            // Because we need the context here.
            button1.Enabled = true;
        }
    }
    ```
- 각 비동기 메서드에는 고유 한 컨텍스트가 있으므로  
한 비동기 메서드가 다른 비동기 메서드를 호출하면  
해당 컨텍스트는 독립적입니다.  
#8 은 #7의 수정 버전.

    ```c#
    // #8
    // 각 비동기 메서드에는 고유 한 컨텍스트가 있다.
    private async Task HandleClickAsync()
    {
        // Can use ConfigureAwait here.
        await Task.Delay(1000).ConfigureAwait(continueOnCapturedContext: false);
    }
    private async void button1_Click(object sender, EventArgs e)
    {
        button1.Enabled = false;
        try
        {
            // Can't use ConfigureAwait here.
            await HandleClickAsync();
        }
        finally
        {
            // We are back on the original context for this method.
            button1.Enabled = true;
        }
    }    
    ```
- 컨텍스트 프리 코드는 더 재사용이 가능하다.  
컨텍스트에 민감한 코드와 컨텍스트에 구애받지 않는 코드 사이에  
장벽을 만들고 컨텍스트에 민감한 코드를 최소화하도록 한다.  
그림 8에서는 상황에 맞는 이벤트 핸들러에 최소한의 코드만 남겨두고  
테스트 가능하고 컨텍스트가 없는 비동기 태스크 메서드에  
이벤트 핸들러의 모든 핵심 로직을 배치하는 것이 좋다.
ASP.NET 응용 프로그램을 작성하는 경우에도  
데스크톱 응용 프로그램과 잠재적으로 공유되는 핵심 라이브러리가있는 경우  
라이브러리 코드에서 ConfigureAwait를 사용하는 것이 좋습니다.

- 요약하면  
가능한 경우 ConfigureAwait를 사용해야한다.  
컨텍스트 프리 코드는  
GUI 애플리케이션의 성능이 더 우수하며  
부분 비동기 코드베이스로 작업 할 때  
교착 상태를 피하는 데 유용한 기술이다.  
이 가이드 라인의 예외는 컨텍스트가 필요한 메서드인 경우.

### Know Your Tools
- 복잡하다.
- #9는 일반적인 문제에 대한 솔루션의 빠른 참조.  

|문제 |해결책|
|:---|:---|  
|코드를 실행하는 작업 만들기	           |Task.Run 또는 TaskFactory.StartNew (작업의 생성자 또는 Task.Start 아님.)
|작업 또는 이벤트에 대한 작업 래퍼 만들기	|TaskFactory.FromAsync 또는 TaskCompletionSource <T>
|지원 취소	                              |CancellationTokenSource 및 CancellationToken
|진행 상황보고	                          |IProgress <T> 및 Progress <T>
|데이터 스트림 처리	                       |TPL 데이터 흐름 또는 Reactive Extensions
|공유 리소스에 대한 액세스 동기화	        |SemaphoreSlim
|리소스를 비동기 적으로 초기화	            |AsyncLazy <T>
|비동기식 생산자 / 소비자 구조	            |TPL 데이터 흐름 또는 AsyncCollection <T>

- 첫 번째 문제는 작업 생성이다.  
분명히 비동기 메서드는 작업을 생성 할 수 있으며  
이것이 가장 쉬운 옵션입니다.  
스레드 풀에서 코드를 실행해야하는 경우 Task.Run을 사용합니다.  
기존 비동기 작업 또는 이벤트에 대한 작업 래퍼를 만들려면  
TaskCompletionSource\<T>를 사용힌다.  
  
    다음으로 일반적인 문제는 취소 및 진행보고를 처리하는 방법으로   
    BCL (기본 클래스 라이브러리)에는 CancellationTokenSource / CancellationToken 및  
    IProgress\<T> / Progress\<T>와 같은 문제를 해결하기 위해  
    특별히 고안된 형식이 포함되어 있다.  
    비동기 코드는 작업 생성, 취소 및 진행보고를 자세히 설명하는  
    작업 기반 비동기 패턴 또는 TAP ( msdn.microsoft.com/library/hh873175 )를 사용해야한다.

- 또 다른 문제는 비동기 데이터 스트림을 처리하는 방법이다.  
Task는 훌륭하지만 하나의 개체 만 반환하고 한 번만 완료 할 수 있다.  
비동기 스트림의 경우 TPL Dataflow 또는 Reactive Extensions(Rx)를 사용할 수 있다.  
TPL Dataflow는 행위자 같은 느낌을주는 '메시'를 만든다.  
Rx는 더 강력하고 효율적이지만 학습 곡선이 더 어렵다.  
TPL Dataflow와 Rx에는 모두 비동기식 메서드가 있으며 비동기 코드에서 잘 작동힌다.
- 코드가 비동기 적이라고해서 안전하다는 의미는 아니다.  
공유 리소스는 여전히 보호해야하며  
잠금 내부에서 기다릴 수 없다는 사실로 인해 복잡하다.  
다음은 항상 동일한 스레드에서 실행 되더라도  
공유 상태가 두 번 실행되면 공유 상태를 손상시킬 수있는 비동기 코드의 예.

    ```c#
    int value;

    Task<int> GetNextValueAsync(int current);

    async Task UpdateValueAsync()
    {
        value = await GetNextValueAsync(value);
    }
    ```
- 문제는 메서드가 값을 읽고 대기시 자체를 일시 중단하고  
메서드가 다시 시작될 때 값이 변경되지 않은 것으로 가정한다는 것이다.  
이 문제를 해결하기 위해 SemaphoreSlim 클래스는  
async-ready WaitAsync 오버로드로 확장되었다.  
#10 은 SemaphoreSlim.WaitAsync를 보여준다.

    ```c#
    // #10
    // 그림 10 SemaphoreSlim은 비동기 동기화를 허용합니다.
    SemaphoreSlim mutex = new SemaphoreSlim(1);
    int value;

    Task<int> GetNextValueAsync(int current);

    async Task UpdateValueAsync()
    {

        await mutex.WaitAsync().ConfigureAwait(false);

        try
        {
            value = await GetNextValueAsync(value);
        }
        finally
        {
            mutex.Release();
        }

    }
    ```
- 비동기 코드는 종종 캐시되고 공유되는 리소스를 초기화하는 데 사용됩니다.  
이에 대한 기본 제공 유형은 없지만  
Stephen Toub는 Task\<T> 및 Lazy\<T>의 병합처럼 작동하는  
AsyncLazy\<T>를 개발했습니다.  
원래 유형은 그의 블로그에 설명되어 있으며[^footnote2][^footnote3][^footnote4]  
업데이트 된 버전은 내 AsyncEx 라이브러리[^footnote5]에서 사용할 수 있다.
- 마지막으로, 때때로 비동기 준비 데이터 구조가 필요합니다.  
TPL Dataflow는 비동기식 생산자 / 소비자 대기열처럼 작동하는 BufferBlock\<T>를 제공합니다.  
또는 AsyncEx는 BlockingCollection <T>의 비동기 버전 인 AsyncCollection\<T>를 제공합니다.

## 참고
[^footnote1]: [Async/Await - Best Practices in Asynchronous Programming](https://docs.microsoft.com/en-us/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming)  
[^footnote2]: [stephen cleary](https://blog.stephencleary.com/)  
[^footnote3]: [AsyncLazy](https://devblogs.microsoft.com/pfxteam/asynclazyt/)  
[^footnote4]: [AsyncLazy\<T> Class](https://docs.microsoft.com/en-us/dotnet/api/microsoft.visualstudio.threading.asynclazy-1?view=visualstudiosdk-2019)  
[^footnote5]: [StephenCleary/AsyncEx](https://github.com/StephenCleary/AsyncEx)  
