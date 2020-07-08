---
layout: post
title: "[Spring AOP] Java Dynamic Proxy와 CGLIB Proxy"
createdDate:   2020-07-08T20:14:00+09:00
date:   2020-07-08T20:14:00+09:00
pagination: enabled
author: SoonYong Hong
categories: spring
tags: Spring AOP AspectJ PointCut Aspect Advice cglib
---

# Spring AOP
---

## AOP 이란

#### Aspect Oriented Programming 
관점지향 프로그래밍이라고도 불린다. 서비스에는 다양한 비즈니스로직들이 있다. 이러한 다양한 비즈니스 로직에서 사용되지만 핵심기능이 아닌 공통적으로 사용되는 부분, 이를테면 로깅, 보안과 같은 부분은 분리하는 것이다.

---

## Spring AOP의 원리
Spring AOP는 객체를 감싸고 있는 프록시 객체를 만들어 낸다.     
이 프록시 객체가 원래의 객체를 감싸게 되고 객체의 메소드가 호출될 떄, PointCut에 해당하는 Advice를 실행하게 된다.     
AspectJ에서는 메소드, 필드, 생성자 등 다양한 곳에 JoinPoint를 만들어 내지만 Spring AOP에서 가능한 JoinPoint는 public 메소드 뿐이다.     
실제 객체 내부에는 관여하지 않고 감싸는 형태이기 때문에 외부에서 public method를 호출하는 경우에만 작동하는 것이다.     
또한 객체 내부에서 메소드를 호출하는 경우에는 프록시 객체를 통하지 않고 this를 통하여 메소드를 호출하기 때문에 Spring AOP가 동작하지 않는다.     
스프링 AOP의 사용법에 대한 자세한 내용은 [스프링 AOP](../springaop)에서 확인하십시오.

---

## Java Dynamic Proxy
자바 동적 프록시는 자바에서 지원하는 표준 프록시 기술입니다. 아래와 같은 클래스가 있다고 생각해봅시다.

```java
@Controller
public class UserController {
    @Autowired
    private UserService userService;
}
```
```java
@Service
public class UserServiceImpl implements UserService {

    public String method1 (String input) {
        return "result";
    }
}
```

이런 경우 `UserServiceImpl`에 AOP를 적용하여 Proxy 객체로 만들면 아래와 같은 형태를 가지게 됩니다.

```java
@Service
public class ProxyUserServiceImpl implements UserService {
    private UserServiceImpl realObject

    public String method1(String input){
        /* Around PointCut AOP 로직이 적용되는 부분입니다. */
        /* Before PointCut AOP 로직이 적용되는 부분입니다. */
        try {
            try {
                String result = realObejct.method1(input);
            } catch (throwable t){
                /* AfterThrowing PointCut AOP 로직이 적용되는 부분입니다. */
                throw t;
            } 
        } catch (throwable t){
            /* After PointCut AOP 로직이 적용되는 부분입니다. */
            throw t;
        }
        /* AfterReturning PointCut AOP 로직이 적용되는 부분입니다. */
        /* After PointCut AOP 로직이 적용되는 부분입니다. */
        /* Around PointCut AOP 로직이 적용되는 부분입니다. */
        return result;
    }
}
```

이렇게 실제 객체를 내부에 가지고 메소드 호출이 오면 AOP로직을 실행하고 객체의 로직은 실제 내부 객체에 위임하는 형식입니다.      
이 때, 왜 프록시 객체가 `UserService`를 구현하는지 의문을 가질 수 있습니다. `ProxyUserServiceImpl`은 원래의 `UserServiceImpl`과 다른 클래스입니다. 따라서 `UserController`에서는 `UserServiceImpl`이 아닌 다른 클래스의 인스턴스를 의존성 주입받게 됩니다. 만약 `UserController`가 `UserService` 인터페이스를 이용하여 의존성 주입을 받지 않고 `UserServiceImpl`클래스를 이용하여 의존성 주입을 받고 있었다면 `ProxyUserServiceImpl`은 다른 클래스의 인스턴스기 때문에 의존성 주입이 불가능해 집니다. 따라서 자바 동적 프록시는 인터페이스를 구현하는 형식으로 프록시 객체를 생성하게 됩니다.

## CGLIB Proxy
CGLIB 프록시는 자바 동적 프록시와는 다르게 인터페이스가 아닌 클래스를 사용해도 의존성 주입이 가능합니다. 아래와 같은 클래스가 있다고 생각해 봅시다.

```java
@Controller
public class UserController {
    @Autowired
    private UserServiceImpl userService;
}
```
```java
@Service
public class UserServiceImpl {
    @Autowired
    private UserDao userDao;

    public String method1 (String input) {
        String result = userDao.method1(input);
        return result;
    }

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
}
```

CBLIB 프록시는 자바 바이트코드 조작을 통하여 클래스를 상속해서 새로운 클래스를 만들어냅니다. 이 경우, 클래스로 의존성 주입을 받는 객체라하여도 업캐스팅을 통하여 프록시 객체를 이용할 수 있습니다. 아래와 같은 형태의 객체가 될 것입니다.

```java
@Service
public class ProxyUserServiceImpl extends UserServiceImpl {
    private UserServiceImpl realObject;

    public String method1(String input){
        /* Around PointCut AOP 로직이 적용되는 부분입니다. */
        /* Before PointCut AOP 로직이 적용되는 부분입니다. */
        try {
            try {
                String result = realObejct.method1(input);
            } catch (throwable t){
                /* AfterThrowing PointCut AOP 로직이 적용되는 부분입니다. */
                throw t;
            } 
        } catch (throwable t){
            /* After PointCut AOP 로직이 적용되는 부분입니다. */
            throw t;
        }
        /* AfterReturning PointCut AOP 로직이 적용되는 부분입니다. */
        /* After PointCut AOP 로직이 적용되는 부분입니다. */
        /* Around PointCut AOP 로직이 적용되는 부분입니다. */
        return result;
    }

    /* setUserDao 생략 */
}
```

CGLIB 프록시 객체의 경우 클래스를 상속하기 때문에 주의해야 할 점이 있습니다. final 메소드를 사용하지 않아야 한다는 것입니다. final 메소드는 상속이 불가하기 때문에 AOP로직이 동작하지 않으며 내부의 실제 객체에 동작을 위임할 수도 없기때문에 원하지 않는 결과가 나오게 됩니다. 만약 위의 `UserServiceImpl`객체의 `setUserDao`메소드가 final이라면 아래와 같이 될 것입니다. 


```java
@Service
public class ProxyUserServiceImpl extends UserServiceImpl {
    private UserServiceImpl realObject

    /* method1 생략 */

    public final void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }  
}
```

위와 같은 형태가 될 것이고 `setUserDao`메소드를 호출해도 `realObject`의 `setUserDao`메소드는 호출되지 않을 것입니다. 따라서 런타임에 `ProxyUserServiceImpl`인스턴스의 `method1`이 호출되면 내부 `realObject`의 `method1`이 호출되면서 `realObject`의 `userDao` 필드는 `null`이기 때문에 `NullPointerException`이 발생할 것입니다.