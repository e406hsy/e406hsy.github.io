---
layout: post
title:  "Spring Controller Parameter"
createdDate:   2020-02-05T20:48:00+09:00
date:    2020-02-05T22:07:00+09:00
pagination: enabled
author: SoonYong Hong
categories: Spring
tags: Spring SpringController
---
# Spring Controller Parameter 정리
---

### HttpServletRequest [variableName]
HttpServletRequest 객체를 parameter로 받아오면 client에서 보낸 http request에 대한 다양한 정보를 가져올 수 있다.

```java
@RequestMapping("/")
public String example(HttpServletRequest request) {
    String cookie = request.getCookies("cookie");
    if (cookie==null)
        return "error";
    return "index";
}
```
---

### HttpSession [variableName]
세션에 저장되어 있는 정보를 가져올 수 있다.

```java
@RequestMapping("/")
public String example(HttpSession session) {
    String id = session.getAttribute("id");
    if (id==null)
        return "login";
    return "content";
}
```
---

### @RequestParam [type] [variableName]
http method에 있는 parameter를 원하는 타입으로 받아서 사용할 수 있다.   
get, post 모두 사용 가능하다.

- @RequestParam [type] [variableName]
    - [variableName]과 request body parameter key가 같은 경우
    - @RequestParam([variableName]) [type] [variableName]과 같다
- @RequestParam([key]) [type] [variableName]
    - [variableName]과 request body parameter [key]가 다른 경우
    - @RequestParam(value=[key]) [type] [variableName]와 같다
- @RequestParam(value=[key], required=[boolean]) [type] [variableName]
    - 반드시 필요하지 않은 경우 false로 설정할 수 있다. default(명시되지 않은 경우)는 true
    - 이 경우 "value="을 생략할 수 없다.

```java
@RequestMapping("/")
public String example(@RequestParam int index) {
    if (index == 0)
        return "index";
    else if (index == 1)
        return "menu1";
    else if (index == 2)
        return "menu2";
    return "error";
}
```
---

### @RequestBody String [variableName]
RequestBody 전체를 String으로 받아올 수 있다. Request Body가 [key]=[value](&[key]=[value])*의 형태를 가지고 있지 않다면 이 어노테이션을 통해 가져와 사용할 수 있다.

```java
@RequestMapping("/")
public String example(@RequestBody String body) {
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("HH mm");
    Date date = simpleDateFormat.parse(body);
    if (date.after(Calendar.getInstance().getTime()))
        return "future";
    return "past";
}
```
---

### @PathVariable [type] [variableName]
@RequestMapping, @GetMapping, @PostMapping등에 ("{value}")형태로 들어있는 내용을 가져올 수 있다.
- @PathVariable [type] [variableName]
    - [variableName]과 path value가 같은 경우
    - @PathVariable([variableName]) [type] [variableName]과 같다
- @PathVariable([path value]) [type] [variableName]
    - [variableName]과 [path value]가 다른 경우
    
```java
@RequestMapping("/{index}")
public String example(@PathVariable int index) {
    if (index == 0)
        return "index";
    else if (index == 1)
        return "menu1";
    else if (index == 2)
        return "menu2";
    return "error";
}
```
---

### HttpServletResponse [variableName]
HttpServletRequest 객체를 parameter로 받아오면 client에 http response를 바로 보낼 수 있다.   
이 때 함수의 return type이 void가 아닌경우 에러가 발생할 수 있다.

```java
@RequestMapping("/")
public void example(HttpServletResponse response) {
    response.setContextType("text/plain");
    response.getWriter().write("contents what you want");
}
```
---

