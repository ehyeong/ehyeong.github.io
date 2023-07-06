---
layout: post
title:  "Spring Introduction Inflearn 02"
excerpt: "회원 관리 예제 - 백엔드 개발"
date:   2023-06-30 23:04:36 +0530
categories: SpringBoot
---

### 회원 관리 예제 - 백엔드 개발

### 비지니스 요구사항


비지니스 요구사항

- 데이터: 회원ID, 이름
- 기능: 회원 등록, 조회
- 아직 데이터 저장소 선정 X (가상 시나리오)

일반적인 웹 애플리케이션 계층 구조

- 컨트롤러: 웹 MVC의 컨트롤러 역할
- 서비스: 핵심 비즈니스 로직 구현
- 리포지토리: 데이터베이스에 접근, 도메인 객체를 DB에 저장, 관리
- 도메인: 비즈니스 도메인 객체 ex) 회원, 주문, 쿠폰 등 주로 데이터베이스에 저장, 관리

클래스 의존관계

- 아직 데이터 저장소 선정X → 인터페이스로 구현 클래스 변경할 수 있도록 설계
- 데이터 저장소는 다양한 저장소를 고민중인 상황으로
- 개발을 진행하기 위해 초기 개발 단계에서는 구현체로 가벼운 메모리 기반 데이터 저장소 사용

***

### 회원 도메인, 리포지토리 만들기

domain 패키지 생성

domain - Member

```java
package hello.hellospring.domain;

public class Member {
    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

repository 패키지 생성

repositoy - MemberRepository

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.List;
import java.util.Optional;

public interface MemberRepository {
		//save는 왜 해준거지
    Member save(Member member);
		//Optional이 뭘까
    Optional<Member> findById(Long id);
    Optional<Member> findByName(String name);
    List<Member> findAll();
}
```

repository - MemoryMemberRepository

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.*;

public class MemoryMemberRepository implements MemberRepository{

    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(),member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {
        return store.values().stream()
								//왜 이렇게 쓰는건진 모르겠다
                .filter(member -> member.getName().equals(name))
                .findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }
}
```

***

### 회원 리포지토리 테스트 케이스 작성

Test - java - hello.hellospring - repository 패키지 생성

MemoryMemberRepositoryTest

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.util.Assert;

public class MemoryMemberRepositoryTest {
    MemberRepository repository = new MemoryMemberRepository();

    @Test
    public void save(){
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

        Member result = repository.findById(member.getId()).get();
        Assertions.assertEquals(member, result);

    }
}
```

💡 Assertions 쳤을때 org.assertj. 어쩌고 나오는거 클릭 

Assertions.assertThat 으로 작성

option + Enter 하면 import static org.assertj.core.api.Assertions.*; 생성됨

⇒ 앞으로 asserThat 만으로 작성 가능



```java
@Test
    public void save(){
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

        Member result = repository.findById(member.getId()).get();
        //Assertions.assertEquals(member, result);
        assertThat(member).isEqualTo(result);

    }
    @Test
    public void findByName(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        Member result = repository.findByName("spring1").get();

        assertThat(result).isEqualTo(member1);

    }
    @Test
    public void findAll(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        List<Member> result = repository.findAll();

        assertThat(result.size()).isEqualTo(2);
    }
```

이렇게 작성 후 전체 실행하면 에러가 발생할것임! (순서에 상관없이 되어야 …) → 클리어 해줘야함

findAll이 먼저 수행되기 때문에 어쩌고

MemoryMemberRepository 클래스에

```java
public void clearStore(){
        store.clear();
    }
```

생성

MemoryMemberRepositoyTest 클래스에

```java
MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach(){
        repository.clearStore();
    }
```

생성

실행 → 성공 !

(데이터들이 깔끔히 지워져야 함)

***

### 회원 서비스 개발

service 패키지 생성

MemberService 클래스

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.repository.MemberRepository;
import hello.hellospring.repository.MemoryMemberRepository;

import java.util.List;
import java.util.Optional;

public class MemberService {

    private final MemberRepository memberRepository = new MemoryMemberRepository();

    //회원가입
    public Long join(Member member){
        //같은 이름이 있는 중복 회원 X
        /*
        Optional<Member> result = memberRepository.findByName(member.getName());
        result.ifPresent(m -> {
            throw new IllegalStateException("이미 존재하는 회원")
        });
        */
        //중복 회원 검증
        validateDublicateMember(member);
        memberRepository.save(member);
        return member.getId();
    }

    private void validateDublicateMember(Member member) {
        memberRepository.findByName(member.getName())
            .ifPresent(m -> {
            throw new IllegalStateException("이미 존재하는 회원");
        });
    }

    public List<Member> findMembers(){
        return memberRepository.findAll();
    }

    public Optional<Member> findOne(Long memberId){
        return memberRepository.findById(memberId);
    }
}
```

***
### 회원 서비스 테스트

MemberRepository 클래스 

MemberService 

command + shift + t → create new test

MemberServiceTest 생성

join() 을 회원가입으로 변경해도 됨(테스트라 한글 가능)

//given

//when

//then 

이렇게 주석 작성하여 쓰면 편하다고 함 

```java
@Test
    void 회원가입() {
        //given
        Member member = new Member();
        member.setName("hello");

        //when
        Long saveId = memberService.join(member);

        //then
        Member findMember = memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());
    }
```

```java
@Test
    public void 중복_회원_예외(){
        //given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");

        //when
        memberService.join(member1);
        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));
```

```java
MemberService memberService = new MemberService();
    MemoryMemberRepository memberRepository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach(){
        memberRepository.clearStore();
    }
```

~~DI (??)~~

```java
private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
```

이렇게 변경 

(참고로 command + n 누르고 constructor 누르면 됨)

MemberServiceTest class를

```java
MemberService memberService;
    MemoryMemberRepository memberRepository;

    @BeforeEach
    public void beforeEach(){
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }
```

다음과 같이 변경