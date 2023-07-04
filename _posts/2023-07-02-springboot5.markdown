---
layout: post
title:  "Spring Boot Inflearn 05"
excerpt: "스프링 DB 접근 기술"
date:   2023-07-02 21:30:36 +0530
categories: SpringBoot
---

### 스프링 DB 접근 기술

### H2 데이터베이스 설치

H2 Database Engine Download (all platforms)

terminal - h2 - bin 

$ chmod 755 h2.sh

$ ./h2.sh

입력 -> 다음 창 띄워짐

http://218.38.137.27:8082/?key=748cc909652350a6fed357f69cc23fc45e921665597e15eada65afa299f13aa7

앞부분 지우고 localhost:8082/? ... 되도록 고침 

H2 콘솔 창

연결 후 나가기
 
홈에 test.mv.db 있는지 확인 (완료)

jdbc URL : jdbc:h2:tcp://localhost/~/test 로 변경
(이렇게 변경해야 여러군데에서 접근 가능함)

***

다시

rm test.mv.db (지우기)

https://www.h2database.com/html/download-archive.html 여기 들어가서 1.4.200 버전으로 다시 설치

