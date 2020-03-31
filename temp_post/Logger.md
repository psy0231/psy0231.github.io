---
title: Logger
date: 2020-03-04 16:00:00 +0900
categories: [Project, Logger]
tags: [Logger, C#]
seo:
  date_modified: 2020-03-06 08:08:04 +0900
---

## Log 
- 전에 다른사람이 만든 Logger쓰다가 프로그램이 뻗은적있음
- 이미 좋은 것들이 많았음 
  - 이미 많이 쓰이고 개발되어있음.
  - 최소한의 검증이 모두 완료된 상태라고하고 쓸수있음. 
- 적당히 바꿔 쓰자.

## 목표
- serilog, NLog 등의 Logger중 이 목표에 가능한거 베이스 
- lib로 만들고 최소한의 config만 할 수 있도록.
  - 실제 위 로그들은 세세하게 config control 가능하나 그렇게까지 필요없음
  - 로그 쓸때마다 새로 저걸 다 작성하기 번거로움
  - 위 로그 다 .config로 따로 작성 가능하나 전부 없애고 코드상에서 하는걸로.
- class A는 log_1.txt, log_2.txt가 가능해야함
- class B는 위의 log_1.txt에 작성 가능해야함.
- StackTrace 출력
  - 어떤 class의 어떤 method에서 call했는지.
- 데이터 많을때 처리 가능 
- 로그파일 날짜별 또는 날짜-크기별 분리 



## 참고
- [stack trace](https://www.codeproject.com/Articles/7964/Logging-method-name-in-NET)


## memo 
- 시작
  - NLog.LogManager 에서 config 불러온다 
  - config is null => new config
  - config is !null => get old config
  - addrule

- every new Logger creage Targets for File and Console???
  - then cant control log level
  - all log's are use same layout

  