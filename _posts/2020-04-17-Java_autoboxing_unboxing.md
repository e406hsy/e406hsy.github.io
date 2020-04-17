---
layout: post
title:  "Java : Autoboxing And Unboxing"
createdDate:   2020-04-17T08:26:00+09:00
date:   2020-04-17T08:26:00+09:00
pagination: enabled
author: SoonYong Hong
categories: Java
tags: Java WrapperClass Autoboxing Unboxing
---
# JAVA Autoboxing Unboxing
---

## Wrapper Class
자바에는 기본형 데이터 타입과 객체 데이터 타입이 있다.     
하지만 기본형 데이터 타입이라도 객체로써 다뤄야 하는 경우가 있다.     
이럴 때 사용할 수 있는 것이 Wrapper Class이다.     
예를 들면, null 값을 가질 수 있는 정수형 데이터와 같은 경우이다.

```java
Integer myInt = null;
Integer myInt2 = Integer.valueOf(10);
```

---

### List Of Wrapper Classs

| Primitive Type | Wrapper Class |
|:---------------|:--------------|
| byte | Byte | 
| short | Short | 
| int | Integer | 
| long | Long |
| float | Float |
| double | Double | 
| char | Charater |
| boolean | Boolean |
| void | Void |

단 Void 타입은 객체화 되지 않고 공 참조개념을 나타낸다.     
Generic을 이용하여 선언된 Interface를 구현할 경우, 파라메터가 없음 혹은 리턴값이 없음을 나타내는데 사용된다.     

---

### Autoboxing And Unboxing
Autoboxing과 Unboxing은 JDK 1.5부터 지원되는 것으로     
Autoboxing은 기본형 데이터 타입을 Wrapper Class로 변환해주는 것을 말하고     
Unboxing은 Wrapper Class를 기본형 데이터 타입으로 변환해주는 것을 말한다.    

```java
int k = 1;
Integer a = k;
```
```java
int k = 1;
Integer a = Integer.valueOf(k);
```
Autoboxing의 예로 두 구현은 같은 동작을 한다.

#### Autoboxing이 일어나는 경우
* Wrapper Class로 선언된 변수에 기본형 값을 할당하는 경우
* Wrapper Class를 파라메터로 가지는 메소드에 기본형 값을 전달하는 경우

#### Unboxing이 일어나는 경우
* 기본형으로 선언된 변수에 Wrapper Class를 할당하는 경우
* 기본형을 파라메터로 가지는 메소드에 Wrapper Class를 전달하는 경우

---

### Test
```java
public static void main(String[] args) {
    int k = 1;
    long start = System.currentTimeMillis();
    for (long i = 0; i < 100_000_000_000L; i++) {
        Integer integer = k;
    }
    long end = System.currentTimeMillis();
    System.out.println(end-start);
}
```
```java
public static void main(String[] args) {
    int k = 1;
    long start = System.currentTimeMillis();
    for (long i = 0; i < 100_000_000_000L; i++) {
        Integer integer = Integer.valueOf(k);
    }
    long end = System.currentTimeMillis();
    System.out.println(end-start);
}
```
두 코드를 실행했을 때, 각각 30671, 30987이 나왔다.     
실제로 같은 코드임을 알 수 있다.