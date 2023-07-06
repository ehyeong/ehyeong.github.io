---
layout: post
title:  "Spring Introduction Inflearn 03"
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

회원 리포지토리 스프링 빈 등록
```java
@Repository
public class MemoryMemberRepository implements MemberRepository {}
```

'memberService'와 'memberRepository'가 스프링 컨테이너에 스프링 빈으로 등록

***

### 자바 코드로 직접 스프링 빈 등록하기

MemberService 에 @Service, @Autowired 지우기
MemoryMemberRepository 에 @Repository 지우기

실행 -> ⚠️ 오류

SpringConfig Class생성

```java
package hello.hellospring;

import hello.hellospring.repository.MemberRepository;
import hello.hellospring.repository.MemoryMemberRepository;
import hello.hellospring.service.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {
    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }
}

```

DI
- 필드주입
- setter 주입
- 생성자 주입 (권장: 의존관계가 실행중 동적으로 변하는 경우는 거의 없기 때문)

📎 
실무에선 주로 **정형화된** 컨트롤러, 서비스, 리포지토리 같은 코드는 **컴포넌트 스캔** 사용

**정형화 되지 않은, 상황에 따라 구현 클래스를 변경**해야 하면 설정을 통해 **스프링 빈**으로 등록