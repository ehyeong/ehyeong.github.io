---
layout: post
title:  "Spring Boot Inflearn 01"
date:   2023-06-30 23:03:36 +0530
categories: SpringBoot
---


## 스프링 웹 개발 기초

### 정적 컨텐츠


resources - static 폴더 아래 hello-static.html 파일 생성

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>static content</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
정적 컨텐츠 입니다.
</body>
</html>
```

html 파일 작성

http://localhost:8080/hello-static.html


페이지 소스 봐도 동일

***

### MVC와 템플릿 엔진


### MVC : Model, View, Controller

controller - HelloController 클래스 생성

```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class HelloController {

    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model){
        model.addAttribute("name",name);
        return "hello-template";
    }
}
```

templates - hello-template.html 파일 생성

```html
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```



[http://localhost:8080/hello-mvc](http://localhost:8080/hello-mvc?name=spring)

이렇게 작성하면 에러페이지


💡

[http://localhost:8080/hello-mvc?name=](http://localhost:8080/hello-mvc?name=spring) <hello뒤에 나타나고자 하는 문구>


***

### API

```java
@GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name){
        return "hello " + name; //데이터를 그대로 내려줌
    }
```

controller에 작성 - 데이터를 그대로 내려준다고 생각



```java
@GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name){
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }

    static class Hello{
        private String name;

        public String getName(){
            return name;
        }
        public void setName(String name){
            this.name = name;
        }
    }
```


JSON 방식

@ResponseBody 사용

- HTTP 의 Body 에 문자 내용 직접 반환
- 'viewResolver' 대신에 'HttpMessageConverter'가 동작
- 기본 문자처리: 'StringHttpMessageConverter'
- 기본 객체처리 'MappingJackson2HttpMessageConverter'
- byte처리 등 기타 여러 HttpMessageConverter 기본적으로 등록

***

강의: 
*스프링 입문 - 코드로 배우는 스프링부트, 웹 MVC, DB 접근 기출*