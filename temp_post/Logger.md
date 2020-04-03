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
  - 많이 쓰이고, 그만큼 많이 개발되어있음.
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



  https://stackoverflow.com/questions/31337030/separate-log-file-for-specific-class-instance-using-nlog
  https://stackoverflow.com/questions/31337030/separate-log-file-for-specific-class-instance-using-nlog/32065824#32065824
  https://stackoverflow.com/search?q=user:3928617+[logging]
  https://github.com/nlog/nlog/wiki/File-target#per-level-log-files
  https://github.com/nlog/nlog/wiki/EventProperties-Layout-Renderer
  https://github.com/NLog/NLog/issues/796
  https://stackoverflow.com/questions/20352325/logging-in-multiple-files-using-nlog
  https://www.google.com/search?q=NLog+separate+file&oq=NLog+separate+file&aqs=chrome..69i57j0l4.4239j0j7&sourceid=chrome&ie=UTF-8