---
title: async/await_1 (sync/async, block/non-block)
date: 2020-03-13 15:00:00 +0900
categories: [Grind, C#]
tags: [C#, sync/async, block/non-block]
seo:
  date_modified: 2020-04-16 16:11:56 +0900
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
- 더 좋은 설명들  
[간편한 비동기 프로그래밍:async/await(1)](http://www.simpleisbest.net/post/2013/02/06/About_Async_Await_Keyword_Part_1.aspx)  
[C# 5.0 : async / await 키워드](http://www.csharpstudy.com/CSharp/CSharp-async-await.aspx)  
[Asynchronous programming with async and await](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/)  
[What's the difference between Task.Start/Wait and Async/Await?](https://stackoverflow.com/questions/9519414/whats-the-difference-between-task-start-wait-and-async-await)  
