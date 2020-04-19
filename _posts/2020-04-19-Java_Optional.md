---
layout: post
title:  "[Java] Optional Class"
createdDate:   2020-04-19T20:00:00+09:00
date:   2020-04-19T20:00:00+09:00
pagination: enabled
author: SoonYong Hong
categories: java
tags: Java Optional
---
# Java Optional Class
---

## Optional Class
자바의 객체 데이터 타입의 변수는 null일 수가 있다.
Java 8에서 null을 관리하기 위하여 Optional Class가 소개되었다.

---
## Optional Class의 장점
### null 처리를 강제할 수 있다.
null을 리턴할 수 있는 method가 Optional 객체를 반환한다면 해당 메소드를 이용하는 다른 메소드에서 Optional객체에 담긴 객체를 이용하기 위해서는 null 처리를 하여야한다.
```java
/* default 값을 이용한 처리 */
Object result = Optional.ofNullable(null).orElse("default value");

/* null 여부 확인 을 이용한 처리 */
Optional<String> OptionalResult = Optional.ofNullable(null);
if (OptionalResult.isPresent()){
    String result = OptionalResult.get();
    /* do what you want */
}
```

### 선언형 프로그래밍이 가능해진다.
Optional 객체도 Stream API 처럼 선언형 프로그래밍을 이용할 수 있다.

```java
Optional<String> optionalTeam = Optional.ofNullable("str1 str2");
String result = optionalTeam.filter(str -> str.length() == 9)
                            .map(str -> str.substring(5))
                            .orElse("none");
System.out.println(result);     // str2
```

---

## Optional Class의 static Method
### empty()
null을 담고 있는 빈 Optional 객체를 반환한다.

```java
Optional<String> result = Optional.empty();
System.out.println(result.isEmpty());     // true
```

### of(\<T\> value)
value를 담고 있는 Optional\<T\> 객체를 반환한다.
value가 null이면 NullPointerException이 발생한다.

```java
Optional<String> result = Optional.of("Hello, World!");
System.out.println(result.get());     // Hello, World!
```

### of(\<T\> value)
value를 담고 있는 Optional\<T\> 객체를 반환한다.
value가 null이면 Optional.empty()와 같은 결과를 반환한다.

```java
Optional<String> result = Optional.ofNullable(null);
System.out.println(result.isEmpty());     // true
```
---

## Optional Class의 인스턴스 Method
### get()
Optional 객체가 담고 있는 객체를 반환한다.     
빈 Optional 객체의 경우, NoSuchElementException을 발생시킨다.

#### orElseThrow()도 같은 역할을 한다.

```java
Optional<String> OptionalResult = Optional.of("Hello, World!");
String result = OptionalResult.get();
System.out.println(result);     // Hello, World!
```

### isEmpty()
빈 Optional 객체인 경우 true, 아니면 false를 반환한다.

```java
Optional<String> OptionalResult = Optional.of("Hello, World!");
boolean result = OptionalResult.isEmpty();
System.out.println(result);     // false
```

### isPresent()
빈 Optional 객체인 경우 false, 아니면 true를 반환한다.

```java
Optional<String> OptionalResult = Optional.of("Hello, World!");
boolean result = OptionalResult.isPresent();
System.out.println(result);     // true
```

### orElse(\<T\> value)
Optional 객체가 담고 있는 객체를 반환한다.     
빈 Optional 객체인 경우, value를 반환한다.

```java
Optional<String> OptionalResult = Optional.ofNullable(null);
String result = OptionalResult.orElse("my string");
System.out.println(result);     // my string
```

### orElseThrow()
get()과 같은 method이다.

### orElseThrow(\<T\> exceptionYouWant)
Optional 객체가 담고 있는 객체를 반환한다.     
빈 Optional 객체의 경우, exceptionYouWant을 발생시킨다.

```java
Optional<String> OptionalResult = Optional.ofNullable(null);
try {
    String result = OptionalResult.orElseThrow(MyException::new);
} catch (MyException e) {
    System.out.println("MyException catched");     // MyException catched
}
```

### or(Supplier\<Optional\<T\>\> mySupplier)
Optional 객체가 담고 있는 객체를 반환한다.     
빈 Optional 객체인 경우, mySupplier.get()을 통해 반환된 Optional\<T\> 객체를 반환한다.
```java
Optional<String> OptionalResult = Optional.ofNullable(null);
String result = OptionalResult.or(()-> Optional.of("my String"));
System.out.println(result);     // my string
```

### isPresent(consumer\<T\> myConsumer)
Optional 객체가 담고 있는 객체를 사용하여 myConsumer.accept(\<T\> value)를 호출한다.     
빈 Optional 객체인 경우, 아무것도 하지 않는다.
```java
Optional<String> OptionalResult = Optional.of("Hello, World!");
String result = OptionalResult.isPresent((str) -> System.out.println(str)); // Hello, World!
```

### isPresentOrElse(consumer\<T\> myConsumer, Runnable myRunnable)
Optional 객체가 담고 있는 객체를 사용하여 myConsumer.accept(\<T\> value)를 호출한다.     
빈 Optional 객체인 경우, myRunnable.run()을 호출한다.
```java
Optional<String> OptionalResult = Optional.empty();
String result = OptionalResult.isPresentOrElse((str) -> System.out.println(str)
                        ,() -> System.out.println("no value")); // no value
```

### map(Fucntion\<T,U\> myFunction)
Optional\<T\> 객체가 담고 있는 객체를 사용하여 myFunction.apply(\<T\> value)를 호출하여 반환된 결과로 만들어진 Optional\<U\> 객체를 반환한다.     
빈 Optional 객체이거나 myFunction.apply(\<T\> value)의 결과가 null인 경우, 빈 Optional 객체를 반환한다.
```java
Optional<String> OptionalResult = Optional.of("Hello, World!");
String result = OptionalResult.map((str) -> str.replace("World", "Optional")).get();
System.out.println(result);     //  Hello, Optional!
```

### flatMap(Function\<T, Optional\<U\>\> myFunction)
Optional\<T\> 객체가 담고 있는 객체를 사용하여 myFunction.apply(\<T\> value)를 호출하여 반환된 Optional\<U\> 객체를 반환한다.       
빈 Optional 객체인 경우, 빈 Optional 객체를 반환한다.
```java
Optional<String> OptionalResult = Optional.of("Hello, World!");
Integer result = OptionalResult.flatMap((str) -> Optional.of(str.length())).get();
System.out.println(result);     //  11
```

### filter(predicate\<T\> myPredicate)
Optional\<T\> 객체가 담고 있는 객체를 사용하여 myPredicate.test(\<T\> value)를 호출하여 반환된 결과가 true면 해당 Optional\<T\>객체(this)를 반환한다.     
빈 Optional 객체이거나  myPredicate.test(\<T\> value)를 호출하여 반환된 결과가 false인 경우, 빈 Optional 객체를 반환한다.
```java
Optional<String> OptionalResult = Optional.of("Hello, World!");
boolean result = OptionalResult.filter((str) -> false).isEmpty();
System.out.println(true);     //  failed
```
