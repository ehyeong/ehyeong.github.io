---
layout: post
title:  "Spring Boot One-Hour-Project"
excerpt: "스프링 부트 기본기 한시간에 끝내기(기초강의)"
date:   2023-07-01 20:30:36 +0530
categories: SpringBoot
---

### 스프링 부트 기본기 한시간에 끝내기!

com.example.onehourproject

> user package 생성
>   > controller 패키지 생성
>   >   > UserController 클래스 생성

>   > service 패키지 생성

### MVC 패턴

MVC (Model View Controller)

M -> V -> C -> M ...

#### **Controller(컨트롤러)**

- 모델과 뷰 사이에서 브릿지 역할 수행
- 앱의 사용자로부터 입력에 대한 응답으로 모델 및 뷰 업데이트 로직 포함
- 사용자의 요청은 모두 컨트를러를 통해 진행
- 컨트롤러로 들어온 요청은 어떻게 처리할지 결정하여 모델로 요청 전달

ex) 쇼핑몰에서 상품을 검색하면 키워드를 컨트롤러가 받아 모델과 뷰에 적절하게 입력을 처리하여 전달

#### **Model(모델)**

- 데이터를 처리하는 영역
- 데이터베이스와 연동을 위한 DAO(Data Access Object)와 데이터 구조를 표현하는 DO(Data Object)로 구성됨

ex) 검색을 위한 키워드가 넘어오면 데이터베이스에서 관련된 상품의 데이터를 받아 뷰에 전달

#### **View(뷰)**

- 데이터를 보여주는 화면 자체의 영역
- 사용자 인터페이스(UI) 요소들이 여기에 포함, 데이터 각 요소에 배치
- 뷰에서는 별도의 데이터 보관 X

ex) 검색 결과를 보여주기 위해 모델에서 결과 상품 리스트 데이터를 받음

***

(참고)
### 레이어드 아키텍처

레이어드 아키텍처(Layered Architecture) : 어플리케이션 컨포넌트를 유사 관심사를 기준으로 레이어로 묶어 수평적으로 구성한 구조

프리젠테이션 계층 | 컴포넌트 | 컴포넌트 | 컴포넌트
비지니스 계층 | 컴포넌트 | 컴포넌트 | 컴포넌트
데이터 접근 계층 | 컴포넌트 | 컴포넌트 | 컴포넌트

***
UserController.java
```java
package com.example.oneourproject.user.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @GetMapping("/hello")
    public String getHello(){
        return "Hello Hyunji!";
    }
}

```

간단한 형태의 controller 설계

***

### RestAPI

API (Application Programming Interface)

- 응용 프로그램에서 사용할 수 있도록 다른 응용 프로그램을 제어할 수 있게 만든 인터페이스
- API를 사용하면 내부 구현 로직을 몰라도 정의되어 있는 기능 쉽게 사용 가능

인터페이스?
: 어떤 장치간 정보를 교환하기 위한 수단, 방법
ex) 마우스, 키보드, 터치패드

Rest 특징
- Server-Client 구조
    - 자원이 있는 쪽이 Server, 요청하는 쪽이 Client
    - 클라이언트와 서버가 독립적으로 분리
- Stateless
    - 요청 간에 클라이언트 정보가 서버에 저장되지 않음
    - 서버는 각 요청을 완전히 별개의 것으로 인식, 처리
- Cacheable
    - HTTP 프로토콜을 그대로 사용하기 때문에 HTTP특징인 캐싱 기능 적용
    - 대량의 요청을 효율적으로 처리하기 위해 캐시 사용