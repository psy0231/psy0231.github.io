---
title: async/await_1 (sync/async, block/non-block)
date: 2020-03-13 15:00:00 +0900
categories: [Grind, C#]
tags: [c#, sync_async, block_non-block]
seo:
  date_modified: 2020-10-09 21:20:14 +0900
---

# Build Up 
> Action/Func - Task - ***async/await***

## 왜?
- async라는 키워드를 자주 접하는것같은데 막상 써본적은 적거나 저게 어떤식으로 동작하는지 몰라서 시작함
- 시작은 async/await를 바로 쓸라다가 그 와중에 자주보이는 관련 키워드부터 해야지 싶어 build up하다 이 지경이 됨

## sync/async , block/non-block
> 여기는 내가 이해한걸 토대로 작성함. 

- 조금 엇나가는 주제일수도 있지만 여기가 아니면 또 꺼낼 일이 없을것같아 정리함.
- 이 차례의 4개에 대해 설명을 많이 찾을수는 있는데 조금씩 설명이 다르거나 이해하기 좀 어려웠음(시스템 콜이 어쩌고 이래서..)
- sync/async
    - 이 둘의 차이점은 수행을 '누가' 하냐의 관점인것같음
    - 예를들어 '사과를 깎는다.'의 행위를 내가 하면 sync, 동생한테 시키면 async.
    - 명령 수행을 지시했던 thread에서 벗어나지 않고 수행을 하는경우 sync,  
    명령 수행을 지시했던 thread에서 벗어나면(thread를 만들거나 thread pool이나 암튼 분기되면) async.
    - 그래서 Task의 경우 async라고 했음.
- block/non-block
    - 이 둘의 차이점은 명령 수행후 return의 관점인것같음.
    - 먼저, 모든 method는 수행 결과를 caller한테 알린다고 가정함 
        - 이건 method return형, 유무의 문제가 아니라 수행 그 자체(또는 그 method 뭉탱이)가 끝났다는 시그널 같은거
    - block은 이 시그널을 받고 다음으로 움직이고 non-block는 그냥 감
    - 예를 들어 동생에게 사과를 깎게 시키고 나는 깎인 사과가 나올때까지 멀뚱히 기다리면 block,  
    마찬가지로 시키고 나는 게임하고 있다가 다 깎았다고 하면(signal) 호다닥 갖고와 먹으면 non-block
    - 그래서 이 전에 Task가 async이면서 block라고 했던 이유임.  
    작업은 분기되어(async) 실행하는데 끝날때까지 Wait으로 기다려(block)주고 그때까지 실제로 다음 실행도 안함.
- Task로 async/block의 경우를 볼수 있었고  
Socket.Receive()는 sync/block의 경우인데 사실 보통 일반적으로 만드는 method의 형태이기도 하고..  
또 원래 목표인 async&await가 async/non-block인듯 하다. node.js도 일껄?

## Async

### 이유
- 뭐든 반응성 좋게 할라고.
    - ui, db, io, network ... 
- 이전 API(?)들은 기본적으로 sync/block 작성되었을꺼임.  
이전에는 딱히 문제가 없었는데 가면갈수록 비효율이라는것.  
그럼에도 이전 것들을 갈아치우지 않는 이유는  
기존에 너무 많은것들이 이미 그렇게 되어있어서 라는 답이있었는데...

### 종류
- APM(Asynchronous Programming Model)  
IAsyncResult 이용하는 애들.  
Begin, End접미사 들어가는 method인데 대충 옛날꺼 찾아보면 나옴.  
레거시.

    ```c#
    public class MyClass  
    {  
        public IAsyncResult BeginRead(  
            byte [] buffer, int offset, int count,
            AsyncCallback callback, object state);  
        public int EndRead(IAsyncResult asyncResult);  
    }
    ```
- EAP(Event-based Asynchronous Pattern)  
Async접미사 있는 method.  
하나 이상의 이벤트, 이벤트 처리기 대리자 형식 및 EventArg에서 파생된 형식도 필요합니다.  
이것도 레거시.

    ```c#
    public class MyClass  
    {  
        public void ReadAsync(byte [] buffer, int offset, int count);  
        public event ReadCompletedEventHandler ReadCompleted;  
    }
    ```
- TAP(Task-based asynchronous pattern)  
async 및 await 사용. method에 Async접미사 붙은거.  
확실치는 않은데 위 두개는 작업에 칠요한 method와는 별개로 필요한 부수적인게 있으나  
이거는 단일로 가능. 이라고 한다만.. 그래도 딴거보다는 간단함.  
권장되는 방법임.  

    ```c#
    public class MyClass  
    {  
        public Task<int> ReadAsync(byte [] buffer, int offset, int count);  
    }
    ```
- 참고로 위 셋의 기본형은 
    ```c#
    public class MyClass  
    {  
        public int Read(byte [] buffer, int offset, int count);  
    }
    ```
- 아무튼 레거시 된것들 보면 좀 복잡함.  
맨첨에 공부하면서 봤을떄도 복잡해 보여서 넘겼던걸로 기억함.  
다만, 조금 연식이 된 코드들은 가끔 저게 보이는 경우도 있음.  
앞 두개는 자세히 안 할 예정이고 TAP만.

### 자세히...
- .NET에서 TAP모델을 사용하면 I/O 및 CPU 바인딩된 비동기 코드를 간단하게 작성할 수 있다.  
위 예시를 보거나 조금만 찾아봐도 예전 방법들은 확실히 복잡함.
Task 및 Task<T> 형식과 async 및 await 키워드가 사용됨.

- Task, Task<T>
    - Task는 Promise Model of Concurrency(동시성 약속 모델)을 구현하는데 사용.
    - 나중에 작업이 완료될 것이라는 "약속"을 제공.
    - await를 사용하면 태스크가 완료될 때까지 해당 호출자에게 제어가 양도되므로  
    Task가 실행되는 동안 애플리케이션 또는 서비스에서 다른 작업을 수행할 수 있다.  
    다른 비동기 방법과 달리 callback 또는 event를 사용할 필요가 없는데  
    이 작업을 언어, Task API에서 자동으로 해주기 때문.  
    Task<T>를 사용하는 경우 await 키워드는 작업이 완료될 때 반환되는 값을 추가로 “래핑 해제”함.  

- 서버에서?  
    완료되지 않은 task를 차단하는 전용 스레드가 없기 때문에 서버 스레드 풀이 훨씬 더 많은 웹 요청을 처리할 수 있다.

    서버에서 서비스 요청에 사용할 수 있는 스레드가 5개뿐이고,  
    서버가 모두 6개의 동시 요청을 받아 각 요청에서 I/O 작업을 수행한다 했을 떄  
    
    비동기 코드가 없는 서버는  
    5개의 스레드 중 하나가 I/O 바인딩된 작업을 완료하고 응답을 쓸 때까지  
    6번째 요청을 큐에 유지해야 합니다.  
    20번째 요청이 도달할 때쯤에는 큐가 너무 길어져서 서버 속도가 저하되기 시작할 수 있다.
    
    비동기 코드가 실행되고 있는 서버는  
    여섯 번째 요청을 큐에 유지하지만  
    I/O 바인딩된 작업이 완료될 때가 아니라 시작될 때 해당 스레드가 각각 해제됩니다.  
    20번째 요청이 도달할 때쯤에는 들어오는 요청의 큐가 훨씬 더 적으며(큐에 요청이 있는 경우에도)  
    서버 속도가 저하되지 않습니다.
    
    가상의 예제이지만 실제 환경에서도 매우 유사한 방식으로 작동합니다. 실제로 서버가 async 및 await를 사용할 경우 받는 각 요청에 전용 스레드를 사용하는 경우에 비해 훨씬 더 많은 요청을 처리할 것을 예상할 수 있습니다.

- 클라이언트에서?  
클라이언트에서  사용할 경우의 가장 큰 이점은 응답성 증가.  
스레드를 수동으로 생성하여 앱의 응답성을 개선할 수도 있지만  
스레드 생성 작업은 async 및 await를 사용하는 것보다 비용이 많이 듭니다.  
특히 모바일 게임 등의 경우 I/O와 관련된 UI 스레드에 대한 영향을 최소화하는 것이 중요합니다.  
무엇보다도, I/O 바인딩된 작업은 CPU 시간이 거의 필요하지 않으므로  
유용하지 않은 작업에 전체 CPU 스레드를 전용으로 지정할 경우 리소스를 잘못 사용하는 것입니다.  
또한 UI 스레드에 작업을 디스패치하는 경우(예: UI 업데이트)  
async 메서드를 사용하면 간단하며  
추가 작업(예: 스레드로부터 안전한 대리자 호출)이 필요하지 않습니다.


## 좀더 생각 해볼것?
- async
    - Task하면서 의문이었던 wait에서 block되면 의미가 없는거 아닌가 했는데 ...
    - console에서 그랬고 winform이면 다르지 않을까??
    그니까 버튼 클릭에 웹사이트 긁어오는게 있다 치자. 아주 오래걸리는.  
    이떄 event가 sync면 form이 멈추겠지 근데 async면 일단 멈추진 않을것같은데... 해보기 귀찮다.
- non-block
    - 이것도 fore~, progress, pre~ 이런식으로 있을라나?
    - 중간에 경과를 확인한다거나 또는 중간에 작업을 취소한다거나...

## 참고
- [비동기 개요](https://docs.microsoft.com/ko-kr/dotnet/standard/async)
- [비동기에 대한 자세한 설명](https://docs.microsoft.com/ko-kr/dotnet/standard/async-in-depth)
- [Futures and promises](https://en.wikipedia.org/wiki/Futures_and_promises)
- [비동기 프로그래밍 패턴](https://docs.microsoft.com/ko-kr/dotnet/standard/asynchronous-programming-patterns/)


- [간편한 비동기 프로그래밍:async/await(1)](http://www.simpleisbest.net/post/2013/02/06/About_Async_Await_Keyword_Part_1.aspx)  
- [C# 5.0 : async / await 키워드](http://www.csharpstudy.com/CSharp/CSharp-async-await.aspx)  
- [Asynchronous programming with async and await](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/)  
- [What's the difference between Task.Start/Wait and Async/Await?](https://stackoverflow.com/questions/9519414/whats-the-difference-between-task-start-wait-and-async-await)
- [TPL(작업 병렬 라이브러리)](https://docs.microsoft.com/ko-kr/dotnet/standard/parallel-programming/task-parallel-library-tpl)
- [비동기 프로그래밍 패턴](https://docs.microsoft.com/ko-kr/dotnet/standard/asynchronous-programming-patterns/)
- [작업 기반 비동기 프로그래밍](https://docs.microsoft.com/ko-kr/dotnet/standard/parallel-programming/task-based-asynchronous-programming)
- [작업 기반 비동기 패턴](https://docs.microsoft.com/ko-kr/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [비동기 프로그래밍](https://docs.microsoft.com/ko-kr/dotnet/csharp/async)
- [작업 비동기 프로그래밍 모델](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/concepts/async/task-asynchronous-programming-model)
- [.NET으로 병렬 프로그래밍](https://docs.microsoft.com/ko-kr/dotnet/standard/parallel-programming/)

- []()
- []()