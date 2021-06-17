---
title: ch2
date: 1111-11-11 11:11:11 +0900
categories: [cate1, cate2]
tags: [tag1, tag2]
seo:
  date_modified: 1111-11-11 11:11:11 +0900
---
# 1. MYSQL, Work bench 설치 및 소개
- 설치는 그냥
  - root/0000 
  - admin/0000
# 2. 쿼리문 사용 방법(select, update, delete, drop, create)
- 같이 깔린 workbench는 별거없이 사용가능.
- datagrip에서 사용시 timezone 변경 필요.
  - SELECT @@GLOBAL.time_zone, @@SESSION.time_zone, @@system_time_zone
    - 이거 하면 SYSTEM 일꺼임(기본)
  - C:\ProgramData\MySQL\MySQL Server 8.0\my.ini
    - [mysqld] 항목 아래  
    default-time-zone='+9:00' 
    - 추가 후 mysql 서비스 재시작

