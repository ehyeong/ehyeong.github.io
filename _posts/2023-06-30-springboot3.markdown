---
layout: post
title:  "Spring Boot Inflearn 03"
excerpt: "스프링 빈과 의존관계"
date:   2023-06-30 23:05:36 +0530
categories: SpringBoot
---

### 스프링 빈과 의존관계

### 컴포넌트 스캔과 자동 의존관계 설정

MemberController class 생성

```java
public class MemberController {
    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```

**스프링 빈을 등록하는 2가지 방법**

- 컴포넌트 스캔과 자동 의존관계 설정
- 자바 코드로 직접 스프링 빈 등록

컴포넌트 스캔과 자동 의존관계 설정

- @Component 애노테이션이 있으면 스프링 빈으로 자동 등록
- @Controller 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컴포넌트 스캔 때문

컴포넌트를 포함하는 다음 애노테이션도 스프링 빈으로 자동 등록

- @Controller
- @Service
- @Repository