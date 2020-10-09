---
title: async/await_1 (sync/async, block/non-block)
date: 2020-03-13 15:00:00 +0900
categories: [Grind, C#]
tags: [c#, sync_async, block_non-block]
seo:
  date_modified: 2020-10-09 21:26:17 +0900
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
    - sync, block method들은 작업이 끝날때까지 기다려야 한다.  
    시간이 좀 걸린다 싶을때 화면 멈춤현상이 이거때문.
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

## 참고
- [비동기 개요](https://docs.microsoft.com/ko-kr/dotnet/standard/async)
- [비동기에 대한 자세한 설명](https://docs.microsoft.com/ko-kr/dotnet/standard/async-in-depth)
- [Futures and promises](https://en.wikipedia.org/wiki/Futures_and_promises)
- [비동기 프로그래밍 패턴](https://docs.microsoft.com/ko-kr/dotnet/standard/asynchronous-programming-patterns/)

- [간편한 비동기 프로그래밍:async/await (1)](http://www.simpleisbest.net/post/2013/02/06/About_Async_Await_Keyword_Part_1.aspx)  
- [간편한 비동기 프로그래밍:async/await (2)](http://www.simpleisbest.net/post/2013/02/12/About_Async_Await_Keyword_Part_2.aspx)
- [간편한 비동기 프로그래밍:async/await (3)](http://www.simpleisbest.net/post/2013/02/16/About_Async_Await_Keyword_Part_3.aspx)
- [간편한 비동기 프로그래밍: async/await (4)](http://www.simpleisbest.net/post/2013/02/28/About_Async_Await_Keyword_Part_4.aspx)

- [.NET으로 병렬 프로그래밍](https://docs.microsoft.com/ko-kr/dotnet/standard/parallel-programming/)
- [TPL(작업 병렬 라이브러리)](https://docs.microsoft.com/ko-kr/dotnet/standard/parallel-programming/task-parallel-library-tpl)
