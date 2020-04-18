---
layout: post
title:  "[Java] Wrapper Class"
createdDate:   2020-04-17T08:26:00+09:00
date:   2020-04-18T10:53:00+09:00
pagination: enabled
author: SoonYong Hong
categories: java
tags: Java WrapperClass Autoboxing Unboxing
---
# Java Wrapper Class
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

### Wrapper Class를 New로 생성하지 말자.
자바 Wrapper Class는 일부 값을 미리 인스턴스로 만들어 캐시해 둡니다.     
따라서 New를 이용하면 매번 새로운 인스턴스가 만들어지기에 해당 
캐시를 이용할 수 없습니다.     
     
아래 내용은 jdk 1.8 기준의 java.lang.Integer 클래스의 소스코드입니다.
```java
public final class Integer extends Number implements Comparable<Integer> {
    /* 중략 */
    private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer cache[];

        static {
            // high value may be configured by property
            int h = 127;
            String integerCacheHighPropValue =
                sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
            if (integerCacheHighPropValue != null) {
                try {
                    int i = parseInt(integerCacheHighPropValue);
                    i = Math.max(i, 127);
                    // Maximum array size is Integer.MAX_VALUE
                    h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                } catch( NumberFormatException nfe) {
                    // If the property cannot be parsed into an int, ignore it.
                }
            }
            high = h;

            cache = new Integer[(high - low) + 1];
            int j = low;
            for(int k = 0; k < cache.length; k++)
                cache[k] = new Integer(j++);

            // range [-128, 127] must be interned (JLS7 5.1.7)
            assert IntegerCache.high >= 127;
        }

        private IntegerCache() {}
    }

    /* 중략 */
    
    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
    
    /* 중략 */

}
```
java.lang.Integer 클래스를 보면 IntegerCache라는 내부 클래스가 존재합니다.     
코드를 보면 알 수 있듯이 127 ~ -128 까지의 수의 Integer객체를 미리 만들어 놓습니다.     
valueOf 메소드를 이용하면 이 캐시에서 Integer 객체를 가져옵니다.

```java
    public static void main(String[] args) {
        int value = 20022;

        Integer num1 = new Integer(value);
        Integer num2 = Integer.valueOf(value);
        Integer num3 = new Integer(value);
        Integer num4 = Integer.valueOf(value);
        
        System.out.println(num1.equals(num2));  // true
        System.out.println(num1.equals(num3));  // true
        System.out.println(num1.equals(num4));  // true
        System.out.println(num2.equals(num3));  // true
        System.out.println(num2.equals(num4));  // true
        System.out.println(num3.equals(num4));  // true

        System.out.println(num1==num2); // false
        System.out.println(num1==num3); // false
        System.out.println(num1==num4); // false
        System.out.println(num2==num3); // false
        System.out.println(num2==num4); // false
        System.out.println(num3==num4); // false
    }
```
20022의 Integer객체는 캐시 되지 않았기 때문에 어떠한 방법을 사용해도 새 객체가 만들어집니다.

```java
    public static void main(String[] args) {
        int value = 120;

        Integer num1 = new Integer(value);
        Integer num2 = Integer.valueOf(value);
        Integer num3 = new Integer(value);
        Integer num4 = Integer.valueOf(value);
        
        System.out.println(num1.equals(num2));  // true
        System.out.println(num1.equals(num3));  // true
        System.out.println(num1.equals(num4));  // true
        System.out.println(num2.equals(num3));  // true
        System.out.println(num2.equals(num4));  // true
        System.out.println(num3.equals(num4));  // true

        System.out.println(num1==num2); // false
        System.out.println(num1==num3); // false
        System.out.println(num1==num4); // false
        System.out.println(num2==num3); // false
        System.out.println(num2==num4); // true
        System.out.println(num3==num4); // false
    }
```
120의 Integer객체는 캐시되었기 때문에 valueOf메소드를 이용하면 같은 객체가 반환됩니다.


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