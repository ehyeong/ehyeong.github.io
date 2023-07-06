---
layout: post
title:  "Spring Introduction Inflearn 06"
excerpt: "AOP"
date:   2023-07-04 21:30:36 +0530
categories: SpringBoot
---

### AOP

### AOP가 필요한 상황

- 모든 메소드의 호출 시간을 측정하고 싶다면?
- 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern) 
- 회원 가입 시간, 회원 조회 시간을 측정하고 싶다면?

service - MemberService

측정 시간 남기기

```java
public Long join(Member member){

        long start = System.currentTimeMillis();

        try {
            validateDublicateMember(member);
            memberRepository.save(member);
            return member.getId();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMS = finish - start;
            System.out.println("join = " + timeMS + "ms");
        }
}
```
전체 회원 조회 (findMembers())에도 추가

```java
public List<Member> findMembers(){
        //return memberRepository.findAll();
        long start = System.currentTimeMillis();
        try {
            return memberRepository.findAll();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("findMembers " + timeMs + "ms");
        }
    
    }
```

문제점

- 회원가입, 회원조회 시간 측정 기능은 핵심 관심사항 X
- 시간 측정 로직은 핵심 관심 사항 

그러나, 시간 측정 로직 -> 공통 로직으로 만들기 어려움 (모든 로직을 찾아가면서 변경해야 함)

***

### AOP 적용

AOP (Aspect Oriented Programming)

```
공통 관심 사항 (cross-cutting concern)
핵심 관심 사항 (core concern)
```
분리!

aop 패키지 생성

aop - TimeTraceAOP 클래스 생성

AOP는 @Aspect 필요

```java
package hello.hellospring.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Aspect;

@Aspect
public class TimeTraceAOP {
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable{
        long start = System.currentTimeMillis();
        System.out.println("START: " + joinPoint.toString());
        try {
            return joinPoint.proceed();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("END: " + joinPoint.toString()+ " " + timeMs + "ms");
        }
    }
}
```

service - SpringConfig

```java
@Bean
    public TimeTraceAOP timeTraceAOP(){
        return new TimeTraceAOP();
    }
```
추가

혹은

TimeTraceAop에 @Component추가,

@Around("execution(* hello.hellospring..*(..))") 추가

실행 -> 성공

console창에 Start: , End: 로 시간 ms 뜸

📌 확인

MemberService에 로직들 지워도 됨

```
📎 Spring에서 AOP 동작 방식

<AOP 적용 전>

컨트롤러에서 의존관계에 따라 호출

<AOP 적용 후>

가짜 memberService 생성 (프록시)
가짜가 끝나면 진짜를 호출
```


