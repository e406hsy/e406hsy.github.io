---
layout: post
title:  "Spring Controller Parameter"
createdDate:   2020-02-05T20:48:00+09:00
date:    2020-05-24T15:47:00+09
excerpt: "스프링 컨트롤러에서 사용되는 파라메터 정리"
pagination: enabled
author: SoonYong Hong
categories: spring
tags: Spring SpringController
---
# Spring Controller Parameter 정리
---

### HttpServletRequest [variableName]
HttpServletRequest 객체를 parameter로 받아오면 client에서 보낸 http request에 대한 다양한 정보를 가져올 수 있다.     
ServletRequest 타입도 가능하다.

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

### Locale [variableName]
DispatcherServlet의 Locale 리졸버가 결정한 Locale 객체를 받을 수 있다.

```java
@RequestMapping("/")
public String example(Locale locale) {
    if (locale.getLanguage().equals(Locale.KOREA.getLanguage())){
        return "korean"
    }
    return "english";
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
- @RequestParam Map\<String,String\> [variableName]
    - 모든 파라메터를 Map\<String,String\>에 받아 올 수 있다.

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
RequestBody 전체를 String으로 받아올 수 있다. Request Body가 [key]=[value]\(&[key]=[value]\)*의 형태를 가지고 있지 않다면(xml이나 json과 같은 경우) 이 어노테이션을 통해 가져와 사용할 수 있다. 

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

### @CookieValue [type] [variableName]
http method에 있는 쿠키를 원하는 타입으로 받아서 사용할 수 있다.   
get, post 모두 사용 가능하다.

- @CookieValue [type] [variableName]
    - [variableName]과 Cookie key가 같은 경우
    - @RequestParam([variableName]) [type] [variableName]과 같다
- @CookieValue([key]) [type] [variableName]
    - [variableName]과 Cookie [key]가 다른 경우
    - @RequestParam(value=[key]) [type] [variableName]와 같다
- @CookieValue(value=[key], required=[boolean]) [type] [variableName]
    - 반드시 필요하지 않은 경우 false로 설정할 수 있다. default(명시되지 않은 경우)는 true
    - 이 경우 "value="을 생략할 수 없다.
- @CookieValue(value=[key], defaultValue=[String]) [type] [variableName]
    - 쿠키가 없을 경우 사용될 기본값을 설정할 수 있다.

```java
@RequestMapping("/")
public String example(@CookieValue String auth) {
    if (MyAuthUtils.auth(auth))
        return "index";
    return "error";
}
```
---

### Map, Model, ModelMap
뷰에 전달할 Model을 직접 만들지 않고 Map, Model, ModelMap을 이용하여 전달할 수 있다.

```java
@RequestMapping("/userlist")
public String example(Model model) {
    List<User> users = userService.getAllUsers();
    model.addAttribute("user", users);
    return "list";
}
```
---

### @ModelAttribute
@ModelAttribute는 메소드, 파라메터 두 곳에 사용할 수 있다. 

#### 메소드 레벨 @ModelAttribute
메소드 레벨 @ModelAttribute가 붙은 메소드는 RequestMapping이 적용된 메소드가 실행되기앞서 실행된 후, 뷰에 전달할 모델에 해당 메소드를 실행한 결과를 포함시킨다.
```java
@RequestMapping("/user")
public String example(Model model) {
    return "userView";
}

@ModelAttribute("user")
public User getUserInformation (@CookieValue String auth) {
    User user = authService.getUserByAuth(auth);
    return user;
}

```
`"/user"` URL에 접근했을 시, View에서는 `getUserInformation()` 메소드에서 리턴한 User객체에 접근할 수 있다.

#### 파라메터 레벨 @ModelAttribute
파라메터 레벨 @ModelAttribute는 @RequestParam과 거의 동일하다. 단 @ModelAttribute가 적용된 파라메터는 컨트롤러에서 따로 Model에 추가하지 않아도 View에서 접근가능하다.
```java
@RequestMapping("/user")
public String example(@ModelAttribute String id) {
    userService.doSomethingWithId(id);
    return "userView";
}
```
```java
@RequestMapping("/user")
public String example(@RequestParam String id, Model model) {
    userService.doSomethingWithId(id);
    model.addAttribute("id", id);
    return "userView";
}
```
두 컨트롤러는 같은 동작을 합니다. 두 메소드 모두 request parameter에서 id를 받아와서 View Model에 해당 파라메터를 추가해 줍니다.

##### Errors, BindingResult
@ModelAttribute는 형식 변환이 실패하더라도 메소드가 실행되어 결과를 반환한다.. 따라서 BindingResult나 Errors를 사용해서 검증을 해야한다.
```java
@RequestMapping("/user")
public String example(@ModelAttribute String id, BindingResult bindingResult) {
    if (bindingResult.getAllErrors().isEmpty()) {
        userService.doSomethingWithId(id);
        return "userView";
    }
    return "error";
}
```

---

#### @Value
여러 프로퍼티 값이나 SpEl을 이용하여 상수등을 읽어올 수 있다. 여러 메소드에서 사용된다면 필드로 유지하는 것이 더 좋다.
```java
@Value("#{mydata['common']}")
private String common;

@RequestMapping("/redirect/{path}")
public String example(@Value("#{mydata['specific']}") String specific, @PathVariable String path) {
    if (common.eqauls(path)){
        return "common";
    } else if (specific.equals(path)){
        return "specific";
    }
    return "error";
}
```

---

#### @Valid
JSR-303 빈 검증에 따라 검증을 수행할 수 있다. 검증 실패시 MethodArgumentNotValidException을 발생시킨다.
```java
@RequestMapping("/login")
public String example(@RequestParam @Valid User user){
    if (userService.login(user)){
        return "success";
    }
    return "fail";
}

@ExceptionHandler(MethodArgumentNotValidException.class)
public String invalidParameters(MethodArgumentNotValidException e){
    return "invalidParameter";
}
```

---