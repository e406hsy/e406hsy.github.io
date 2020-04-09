---
layout: post
title:  "Spring AOP"
createdDate:   2020-03-31T18:58:00+09:00
date:   2020-04-09T09:05:00+09:00
pagination: enabled
author: SoonYong Hong
categories: Spring
tags: Spring AOP AspectJ PointCut Aspect Advice
---

AspectJ에서는 Spring AOP보다 더 많은 기능을 지원합니다.        
이 포스트는 Spring AOP에 사용가능한 내용들로만 작성되었습니다.

# Spring AOP
---

## AOP 이란

#### Aspect Oriented Programming 
관점지향 프로그래밍이라고도 불린다. 서비스에는 다양한 비즈니스로직들이 있다. 이러한 다양한 비즈니스 로직에서 사용되지만 핵심기능이 아닌 공통적으로 사용되는 부분, 이를테면 로깅, 보안과 같은 부분은 분리하는 것이다.

---

## AOP 용어

* Advice : 비즈니스 로직에서 분리된 공통 기능의 구현체
* JoinPoint : Advice들이 실행될 수 있는 부분은
* PointCut : Advice가 실행될 조인포인트를 결정하는 표현을 말한다.
* Weaving : PointCut에 해당하는 부분이 실행될 떄, Advice를 삽입하는 것을 의미한다.
* Aspect : 부가기능을 모듈화 한것. 조인포인트와 어드바이스를 결합하여 어드바이스가 실행될 시점도 정의하고 있다.

---

## Spring AOP의 원리
Spring AOP는 객체를 감싸고 있는 프록시 객체를 만들어 낸다.     
이 프록시 객체가 원래의 객체를 감싸게 되고 객체의 메소드가 호출될 떄, PointCut에 해당하는 Advice를 실행하게 된다.     
AspectJ에서는 메소드, 필드, 생성자 등 다양한 곳에 JoinPoint를 만들어 내지만 Spring AOP에서 가능한 JoinPoint는 public 메소드 뿐이다.     
실제 객체 내부에는 관여하지 않고 감싸는 형태이기 때문에 외부에서 public method를 호출하는 경우에만 작동하는 것이다.     
또한 객체 내부에서 메소드를 호출하는 경우에는 프록시 객체를 통하지 않고 this를 통하여 메소드를 호출하기 때문에 Spring AOP가 동작하지 않는다.     

---

## Advice 종류
* Before : 메소드 실행 전
* AfterReturning : 메소드가 실행되어 정상적으로 완료된 후
* AfterThrowing : 메소드가 실행되어 Exception을 발생한 경우
* After : 메소드가 실행된 후
* Around : 메소드가 실행되기 전과 후

---

## 포인트 컷 표현식

### 기본적인 형태

지시자(리턴타입 패키지 클래스 메소드(매개변수))

#### 예시
```java
@Aspect
public class MyAopClass () {
    @Before("execution(String com.hong.soonyong.aop.example.Student setName(String))")
    public void logger (){
        System.out.println("logger called");
    }
}
```

---

### 지시자

| 표현식 | 설명 |
|:---|:---|
| execution | 해당 메소드가 실행될 떄 |
| args | 해당 매개 변수를 가지는 메소드가 실행될 떄 |
| within | 해당 패키지 혹은 클래스 혹은 인터페이스에 해당하는 모든 메소드 |
| annotation | 해당 어노테이션이 붙은 경우 |
| bean | 특정 bean에 메소드 |

---

### 리턴타입

| 표현식 | 설명 |
|:---|:---|
| * | 모든 리턴 타입 |
| void | void 리턴타입 |
| !String | String을 제외한 리턴타입 |

---

### 패키지

| 표현식 | 설명 |
|:---|:---|
|  | 모든 패키지 |
| com.hong.soonyong. | com.hong.soonyong 패키지 |
| com.hong.soonyong.. | com.hong.soonyong 패키지와 그 하위 패키지 |

---

### 클래스

| 표현식 | 설명 |
|:---|:---|
| Student | Student 클래스 |
| *Impl | Impl로 끝난는 클래스 |
| UserService+ | 해당 클래스의 모든 자손 클래스, 혹은 해당 인터페이스의 구현체 |

---

### 메소드 매개변수

| 표현식 | 설명 |
|:---|:---|
| * | 모든 메소드 |
| getName() | 매개변수가 없는 getName 메소드 |
| setName(String) | String 타입 매개변수 1개만 가지는 setName 메소드 |
| getName(..) | 매개변수와 상관없이 모든 getName 메소드 |
| get*(..) | 매게변수와 상관없이 get으로 시작하는 모든 메소드 |
| setName(anyNameForAspect) | Aspect에 anyNameForAspect라는 이름으로 정의된 매개변수의 타입 매개변수 1개만 가지는 setName 메소드 |

---

## Aspect 사용예시
com.hong.soonyong.aop.example 패키지의      
Student 클래스의      
String을 리턴하고     
String 1개를 매개변수로 가지는     
setName 메소드가 실행하기 전에
```java
@Aspect
public class MyAopClass () {
    @Before("execution(String com.hong.soonyong.aop.example.Student setName(String))")
    public void logger (){
        System.out.println("logger called");
    }
}
```

---

com.hong.soonyong.aop.example 패키지의      
Student의 자손 클래스의      
String을 리턴하고     
String 1개를 매개변수로 가지는 (이 경우, logger 메소드에서 myName이라는 이름의 매개변수가 있어야 한다.)      
set으로 시작하는 메소드가 실행한 후에
```java
@Aspect
public class MyAopClass () {
    @After("execution(String com.hong.soonyong.aop.example.Student+ set*(myName))")
    public void logger (String myName){
        System.out.println("logger called");
    }
}
```

---

com.hong.soonyong.aop.example 패키지의 하위 모든 패키지의      
모든 클래스의      
Student를 리턴하지 않고     
set으로 시작하는 메소드가 실행하다 Exception을 발생시킨 경우에
```java
@Aspect
public class MyAopClass () {
    @AfterThrowing(pointcut = "execution(!Student com.hong.soonyong.aop.example..* set*(..))", throwing = "exceptionObject")
    public void logger (Exception exceptionObject){
        System.out.println("logger called");
        exceptionObject.printStackTrace();
    }
}
```

---

com.hong.soonyong.aop.example 패키지의 하위 모든 패키지의      
하위 모든 클래스의      
메소드가 실행하여 정상적으로 완료된 후에
```java
@Aspect
public class MyAopClass () {
    @AfterReturning("within(com.hong.soonyong.aop.example..)")
    public void logger (){
        System.out.println("logger called");
    }
}
```

---

String 1개를 매개변수로 가지는 메소드가 실행한 후에
```java
@Aspect
public class MyAopClass () {
    @After("args(String)")
    public void logger (){
        System.out.println("logger called");
    }
}
```

---

myAnnotation이 지정된 메소드가 실행될 때
```java
@Aspect
public class MyAopClass () {
    @Around("annotation(myAnnotation)")
    public Object logger (ProceedingJoinPoint pjp){
        System.out.println("method called");
        Object result = null;
        try {
            result = pjp.proceed();  // 여기서 해당 객체의 실체 메소드가 실행된다.
        }
        catch (Throwable throwable) {
            System.out.println("method failed");
        }
        System.out.println("method end");
        return result;
    }
}
```

---

get으로 시작하는 메소드나     
is로 시작하는 메소드가 실행된 후
```java
@Aspect
public class MyAopClass () {
    @Pointcut("execution(* get*(..))"
    public void allGetters() {}

    @After("allGetters() || execution(* is*(..))")
    public void logger (){
        System.out.println("logger called");
    }
}
```