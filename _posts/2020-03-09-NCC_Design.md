---
title: Design
date: 2020-03-09 14:00:00 +0900
categories: [Project, NCC]
tags: [network, c#]
seo:
  date_modified: 2020-04-29 17:30:19 +0900
---

## structure
- ![upload-image]({{ "/assets/img/NCC/NCC-Page-2.jpg" | relative_url }})

- 추가, 수정사항 나중에 . 
- 더 생각 안나 일단 마무리

## add more
  - 외부에서 접근은 main에 한정적으로 하고싶음..
  - message는 사용자 전에 전부 쌓을생각 - message쌓일때마다 이벤트통지
  - 저 쌓인 message들에대한 외부 접근....
  - message queue는?? 
  - message flow control(client to client / client to server etc) 

## cautions
- accept
  - use thread vs async
  - Selectable
  - accept condition??
    ```C#
    if(rempte.ip == "111.111.111.111"){
      accepted();
    }else{
      acceptReject();
    } 
    ```
- connector
  - reconn - selectable
  - connect 할때 실제 connect까지 시간이 상황마다 다름
  - 위의 이유로 중복 실행 -> 확인 필요

- KeepAlive 
  - connect check
    - connect, disconnect, reconnect(start from S or C or both) and notify state or result
    - if didnt receive any msg over some sec, then??
  - KeepAlive term 
    - before this, 5 secs term and 5 times count. 
    That happens so often... ka and cnt both


- Receiver
  - sometimes incoming messages are merged, (so receive big one message)
  - message to packet(internal)