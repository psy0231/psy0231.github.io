---
title: 4.--
date: 1111-11-11 11:11:11 +0900
categories: [cate1, cate2]
tags: [tag1, tag2]
seo:
  date_modified: 1111-11-11 11:11:11 +0900
---

## pre
- 처음 포스팅에 목표로 한 것들
    ```
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
    - console, debug 출력은 사용 여부가 확실치 않으므로 기본 출력을 사용하되 사용 여부는 사용자가 설정함.
    ```
- 이랬는데 마지막 사용법이라면 다 가능함. 단, 데이터 많을때 처리 가능함을 보여야 하는데 일단 만들어보고 나중에 테스트 하기로.
- stacktrack는 매번 로그 찍을 때 마다 찍는게 처음 목표였음 class랑 마지막 method만 해서
    ```
    yyyy-mm-dd.log

    yyyy-mm-dd HH:mm:ss.ff [class][method] contents
    ```
    - 이게 처음 생각한 로그 포맷이었는데 class랑 method는 빼도 될것같음... 또는 선택적으로 집어넣거나.. 
    - 또 .log 를 날짜별로 분리할껀데 안 내용에 한번 더 들어갈 필요도 없을것같음.



## 참고
- []()  
