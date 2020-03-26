---
layout: post
title:  "JAVA : Interface With Default Method"
createdDate:   2020-03-26T20:40:00+09:00
date:   2020-03-26T20:40:00+09:00
pagination: enabled
author: SoonYong Hong
categories: Java
tags: Java
---

# Interface With Default Method

---

## BackGround
Default Method가 없던 시절, Interface에 새로운 Method를 추가하는 것은 해당 Interface를 구현하는 수많은 구현체들의 코드를 수정해야 했다.    
이는 Interface의 수정을 거의 불가능하게 하였고 (특히 API 개발자들에게)    
Interface를 이용하는 구현체들의 내용을 수정하지 않고 Interface에 새로운 기능을 추가하고자 하는 욕구로부터 탄생하였다.

---

## Syntax

---

### Basic Syntax
Interface의 method에 default keyword를 붙이고 구현해두면 해당 Interface를 구현하는 객체에서는 default 키워드가 붙은 method는 구현하지 않아도 기본구현으로 동작한다.

```java
public Interface MyInterface1 {
    default void myMethod1() {
        System.out.println("this is implemented at MyInterface1");
    }
}
```

```java
public class classA implements MyInterface1 {
    public static void main(String[] args) {
        classA a = new classA();
        a.myMethod1();
    }
}
```
#### 결과
```
this is implemented at MyInterface1
```

---

### Explicit Call
```[Interface이름].super.[Method이름]([파라메터들])```의 형태로 직접적으로 호출할 수 있다.

```java
public Interface MyInterface1 {
    default void myMethod1() {
        System.out.println("this is implemented at MyInterface1");
    }
}
```

```java
public class classA implements MyInterface1 {
    public void myMethod1() {
        MyInterface1.super.myMethod1();
        System.out.println("this is implemented at classA");
    }

    public static void main(String[] args) {
        classA a = new classA();
        a.myMethod1();
    }
}
```
#### 결과
```
this is implemented at MyInterface1
this is implemented at classA
```

---

## RULES

### RULE 1 : default is default
default는 어디까지나 default, 즉 기본설정일 뿐이다.     
default를 Override하는 구현이 있다면 default를 사용할 수 없다.
따라서 해당 method를
1. 자손 class의 concrete 혹은 abstract method
2. 자손 interface의 default method

로 Override하면 해당 default method를 접근할 수 없다.


```java
public Interface MyInterface1 {
    default void myMethod1() {
        System.out.println("this is implemented at MyInterface1");
    }
}
```
```java
public Interface MyInterface2 extends MyInterface1 {
    default void myMethod1() {
        System.out.println("this is implemented at MyInterface2");
    }
}
```

```java
public class classA implements MyInterface1, MyInterface2 {
    public static void main(String[] args) {
        classA a = new classA();
        a.myMethod1();
    }
}
```
#### 결과
```
this is implemented at MyInterface2
```

```java
public Interface MyInterface1 {
    default void myMethod1() {
        System.out.println("this is implemented at MyInterface1");
    }
}
```
```java
public Interface MyInterface2 extends MyInterface1 {
    default void myMethod1() {
        System.out.println("this is implemented at MyInterface2");
    }
}
```

```java
public class classA implements MyInterface1, MyInterface2 {
    public void myMethod1() {
        MyInterface1.super.myMethod1();
        System.out.println("this is implemented at classA");
    }

    public static void main(String[] args) {
        classA a = new classA();
        a.myMethod1();
    }
}
```
#### 결과
```
Illegal reference to super method method1() from type MyInterface1, cannot bypass the more specific override from type MyInterface2
```
컴파일 에러

---

### RULE 2 : More Specific One Wins
같은 이름의 method를 구현한 여러 Interface를 구현하거나 상속하는 경우, 더 가까운 Interface의 구현을 따른다.

```java
public Interface MyInterface1 {
    default void myMethod1() {
        System.out.println("this is implemented at MyInterface1");
    }
}
```
```java
public Interface MyInterface2 extends MyInterface1 {
}
```
```java
public Interface MyInterface3 extends MyInterface1 {
    default void myMethod1() {
        System.out.println("this is implemented at MyInterface3");
    }
}
```

```java
public class classA implements MyInterface2, MyInterface3 {
    public static void main(String[] args) {
        classA a = new classA();
        a.myMethod1();
    }
}
```
#### 결과
```
this is implemented at MyInterface3
```
classA에 부모 Interface인 MyInterface3가 상속을 한번더 거쳐야 되는 MyInterface1보다 가깝기 때문에 MyInterface3의 구현을 따른다.

---

### RULE 3 : Class Always Wins Over Interface
같은 이름의 method를 구현한 class와 Interface를 모두 상속할 경우, RULE 2와 상관없이 class의 구현을 따른다.


```java
public Interface MyInterface1 {
    default void myMethod1() {
        System.out.println("this is implemented at MyInterface1");
    }
}
```
```java
public class MyClass1 {
    public void myMethod1() {
        System.out.println("this is implemented at MyClass1");
    }
}
```
```java
public class MyClass2 extends MyClass1 {
}
```

```java
public class classA extends MyClass2 implements MyInterface1 {
    public static void main(String[] args) {
        classA a = new classA();
        a.myMethod1();
    }
}
```
#### 결과
```
this is implemented at MyClass1
```

---

### RULE 4 : If no one wins, it should be implemented
RULE 2와 RULE 3에 의하여 구현이 정해지지 않는다면 해당 method를 Override해줘야 한다.

```java
public Interface MyInterface1 {
    default void myMethod1() {
        System.out.println("this is implemented at MyInterface1");
    }
}
```
```java
public Interface MyInterface2 {
    default void myMethod1() {
        System.out.println("this is implemented at MyInterface2");
    }
}
```
```java
public class classA implements MyInterface1, MyInterface2 {
    public static void main(String[] args) {
        classA a = new classA();
        a.myMethod1();
    }
}
```
#### 결과
```컴파일 에러```

---

## Re Abstract
default method가 구현된 interface를
1. 상속하는 interface
2. 구현하는 class

에서 해당 method를 abstract로 선언하면 이 interface나 class를 상속할 경우, default method가 적용되지 않는다.     
즉, 다시 구현해야 한다.


```java
public Interface MyInterface1 {
    default void myMethod1() {
        System.out.println("this is implemented at MyInterface1");
    }
}
```
```java
public Interface MyInterface2 {
    public abstract void myMethod1();
}
```
```java
public class classA implements MyInterface2 {
    public static void main(String[] args) {
        classA a = new classA();
        a.myMethod1();
    }
}
```
#### 결과
```The type classA must implement the inherited abstract method MyInterface2.myMethod1()```
컴파일 에러
