---
layout: post
title:  "Spring Boot Inflearn 04"
excerpt: "회원 관리 예제 - 웹 MVC 개발"
date:   2023-07-01 21:30:36 +0530
categories: SpringBoot
---

### 회원 관리 예제 - 웹 MVC 개발

### 회원 웹 기능 - 홈 화면 추가

controller - HomeController 클래스 추가
```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping("/")
    public String home(){
        return "home";
    }
}
```
/ : localhost8080을 들어오면 이것이 호출 됨 (home.html)

templates - home.html 생성
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">

<body>

<div class="container">
    <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
            <a href="/members/new">회원 가입</a>
            <a href="/members">회원 목록</a>
        </p>
    </div>
</div>

</body>
</html>
```
html 파일 생성

실행 (localhost8080) -> Hello Spring 페이지

회원 가입, 회원 목록 버튼 누르면 이동은 되나, 에러 페이지 (당연함)

***

### 회원 웹 기능 - 등록

MemberController (이전에 생성해놓은 클래스)

```java
@GetMapping("/members/new")
    public String createForm(){
        return "members/createMemberForm";
    }
```
home.html 파일에서 "/members/new" (회원가입) 만들었으므로 이에 연결되는 매핑 폼 생성해야함

templates - members (디렉토리 생성) - createMemberForm.html 생성

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">

<body>

<div class="container">
  
  <form action="/members/new" method="post">
    <div class="form-group">
      <label for="name">이름</label>
      <input type="text" id="name" name="name" placeholder="이름을 입력하세요">
    </div>
    <button type="submit">등록</button>
  </form>
</div>

</body>
</html>
```

localhost8080에서 회원가입 누르면 이름 입력 페이지로 이동

*http://localhost:8080/members/new* 주소지 변경


📌 확인

controller - MemberForm 클래스 생성

```java
package hello.hellospring.controller;

public class MemberForm {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
참고)
```
📎 getter setter 단축키 : command + n 
```

여기서의 name과 createMemberForm의 name 매칭

MemberController 클래스

```java
@PostMapping("/members/new")
    public String create(MemberForm form){
        Member member = new Member();
        member.setName(form.getName());

        memberService.join(member);

        return "redirect:/"; //홈으로 이동
    }
```

여기까지 하면 회원가입 -> 이름 입력 -> enter -> 다시 홈화면 (localhost8080페이지)

아직 회원 조회는 불가능!

📌 확인

**원리:**

회원가입을 들어가면 *members/new*로 들어감

url에 직접 들어가면 **get방식**으로 createMemberForm으로 이동

createMemberForm에서 html이 보여지게 됨.

form이라는 태그: 값 입력

form 안에서 **input**을 보면 text타입으로 name(서버로 넘어올때의 키 값) 을 받음

**method="post"** 이므로 post 방식으로 받아 MemberController의 **PostMapping**으로 넘어옴 

```
📎 PostMapping은 데이터를 폼 같은 곳에 넣어 전달할 때 사용, GetMapping은 조회할 때 사용
```
url은 똑같지만 ("members/new") get인지 post인지에 따라 다르게 매핑 가능!

@PostMapping에서 MemberForm (command 클릭) -> MemberForm 에 name에 넣어줌 (setName을 통해) (spring이 setName을 호출해서)

getName이 꺼내서 memberService.join(member)을 통해 가입 완료

***

### 회원 웹 기능 - 조회

MemberController 클래스

```java
@GetMapping("/members")
    public String list(Model model){
        List<Member> members = memberService.findMembers();
        model.addAttribute("members", members);
        return  "members/memberList";
    }
```
  

🧑🏻‍🏫: addAttribute 기억나죠?

아니요

📎 Model
: HashMap 형태를 갖고 있으며, Key,Value값을 가짐

- addAttribute(): 모델에 원하는 속성과 그것에 대한 값을 주어 전달할 뷰에 데이터를 전달
    - addAttribute는 Map의 put과 같은 기능과 같아서 이를 통해 해당 모델에 원하는 속성과 그것에 대한 값을 주어 전달할 뷰에 데이터를 전달
- Spring에서 Controller의 메서드를 작성할 때는 특별하게 Model이라는 타입을 파라미터로 지정
- Model 객체는 JSP에 컨트롤러에서 생성된 데이터를 담아서 전달하는 역할을 하는 존재


  

members - memberList.html 파일 생성

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
  <div>
    <table>
      <thead>

      <tr>
        <th>#</th>
        <th>이름</th> </tr>
      </thead>
      <tbody>
      <tr th:each="member : ${members}">
        <td th:text="${member.id}"></td>
        <td th:text="${member.name}"></td>
      </tr>
      </tbody>
    </table>
  </div>
</div> <!-- /container -->
</body>
</html>
```

실행 -> 회원가입 -> 이름 등록 -> 회원 목록 

📌 확인


**원리**

members는 모델 안에 있는 값을 꺼내는 것

컨트롤러에서 모델에서 넘어갈 때 model 키를 통해 리스트에 담아 루플을 돔

로직 반복하여 다음을 실행

꺼내서 member에 담고 id와 이름을 출력

getId getName에 접근

실행을 중지시키고 다시 시작하면 데이터가 모두 사라짐!
-> 다음 강의에서 계속