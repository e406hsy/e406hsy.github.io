---
layout: post
title:  "JAVA Lambda"
createdDate:   2020-03-13T10:33:00+09:00
date:   2020-03-13T10:33:00+09:00
pagination: enabled
author: SoonYong Hong
categories: JAVA
tags: JAVA JAVA8
---
# JAVA Lambda 식 기초
---

## Lambda 식 이란

람다식은 기본적으로 메쏘드를 하나만 가지고 있는 인터페이스의 구현체로 만들어진 객체이다.     
람다식은 다시 사용되지 않을 구현체를 클래스나 익명 클래스로 만들 필요 없이 구현체 및 객체를 만들 수 있게 해준다.         
이러한 인터페이스의 가장 대표적인 예시는 Runnable 인터페이스이다.

---

## Lambda 식의 형태

### 기본적인 형태
() -> method

```java
Runnable runnable = () -> System.out.println("line");   	
runnable.run();
/* 결과
line
*/
```
---

### 실행할 메쏘드가 여럿인 경우
() -> { method1; method2; ...}     
```java
Runnable runnable = () -> {System.out.println("line1"); System.out.println("line2");};   	
runnable.run();
/* 결과
line1
line2
*/
```
---

### 반환 해야할 타입이 있는 경우
#### 메쏘드를 하나만 호출할 경우 return을 생략가능합니다.
() -> value     
() -> { method1; method2; ...; return value;}
```java
Supplier<String> supplier1 = () -> "line1";
System.out.println(supplier1.get());

Supplier<String> supplier2 = () -> {System.out.println("line2"); return "line3";};
System.out.println(supplier2.get());
/* 결과
line1
line2
line3
*/
```
---

### 전달해야하는 파라메터가 있는 경우
#### type은 생략 가능합니다.
([type] parameter1, ...) -> method     
([type] paremeter1, [type] parameter2, ...) -> { method1; method2; ...; methodN;}
```java
Consumer<String> consumer = (parameter) -> System.out.println(parameter);
consumer.accept("line1");

BiConsumer<String, String> biConsumer =
    (String parameter1, String parameter2) -> {System.out.println(parameter1); System.out.println(parameter2);};
biConsumer.accept("line2", "line3");
/* 결과
line1
line2
line3
*/
```
---

### 전달해야하는 파라메터와 반환해야하는 타입이 있는 경우
(parameter1, ...) -> value     
(paremeter1, parameter2, ...) -> { method1; method2; ...; return value;}
```java
Function<String, String> func = (parameter) -> "Fuction works "+parameter;
System.out.println(func.apply("line1"));

IntFunction<String> intFunc = (parameter) -> "Fuction works line"+String.valueOf(parameter);
System.out.println(intFunc.apply(2));

Predicate<String> pred = (parameter) -> Boolean.parseBoolean(parameter);
System.out.println(pred.test("true"));

/* 결과
Fuction works line1
Fuction works line2
true
*/
```
---

## Lambda 식에 사용가능한 기본 인터페이스
#### Runnable - parameter X | return X
#### Supplier - parameter X | return O
#### Consumer - parameter O | return X
#### Function - parameter O | return O
#### Predicate - parameter O | return boolean

* Runnable - no parameter, no return
* Supplier\<T\> - no parameter, return T
    * [Int/Long/Double/Boolean]Consumer - no parameter, return [int/long/double/boolean]
* Consumer\<T\> - parameter T, no return
    * BiConsumer<T1, T2> - parameter T1, parameter T2, no return
    * [Int/Long/Double]Consumer - parameter [int/long/double], no return
    * Obj[Int/Long/Double]Consumer\<T\> - parameter T, parameter [int/long/double], no return
* Function<T1, T2> - parameter T1, return T2
    * BiFunction<T1, T2, T3> - parameter T1, parameter T2, return T3
    * [Int/Long/Double]Function\<T\> - parameter [int/long/double], return T
    * To[Int/Long/Double]Function\<T\> - parameter T, return [int/long/double]
    * To[Int/Long/Double]BiFunction<T1, T2> - parameter T1, parameter T2, return [int/long/double]
    * IntTo[Long/Double]Function - parameter int, return [long/double]
    * LongTo[Int/Double]Function - parameter long, return [int/double]
    * DoubleTo[Int/Long]Function - parameter double, return [int/long] 
    * [Int/Long/Double]UnaryOperator - parameter [int/long/double], return [int/long/double]
    * [Int/Long/Double]BinaryOperator - parameter [int/long/double], parameter [int/long/double], return [int/long/double]
* Predicate\<T\> - parameter T, return boolean
    * [Int/Long/Double]Predicate - parameter [int/long/double], return boolean

### Lambda 식에는 위의 인터페이스 이외에 커스텀 Interface 또한 가능합니다.

#### 조건
Interface에 메쏘드가 하나여야 합니다.     
해당 메쏘드의 파라메터 타입, 수, 순서, 리턴타입이 일치해야 합니다.

```java
@FunctionalInterface
public interface MyLambdaInterface {
	long onlyMethod(int int1, String str1);
}

public static void main(String[] args) {
	MyLambdaInterface myLambdaInterface = (int_param, str_param) -> {
		long value = (long) int_param;
		value += Long.parseLong(str_param);
		return value;
	};

	System.out.println(myLambdaInterface.onlyMethod(1, "2"));
}
/* 결과
3
*/
```
위 예제와 같이 원하는 형태로 Interface를 정의하여 사용이 가능합니다.

#### @FunctionalInterface
개발자에게 lambda식을 이용가능한 형태임을 알려주기위한 annotation입니다.     
실제 컴파일러는 annotation을 확인하지 않고 Interface의 형태를 확인하여 lambda식이 이용가능한 형태인지 아닌지 확인합니다.     
즉 해당 annotation이 없어도 조건을 만족한다면 lambda식이 이용가능합니다.

## Lambda식의 활용방법
#### Lazy evaluation

```java
public static class Logger {
	private boolean debug;

	public Logger() {
		this.debug = true;
	}

	void debug(String str, Supplier<String> strSupplier) {
		if (debugEnabled()) {
			this.write(str + " " + strSupplier.get());
		}
	}

	void debug(String str, String str2) {
		if (debugEnabled()) {
			this.write(str + " " + str2);
		}
	}

	private void write(String string) {
		System.out.println(string);
	}

	private boolean debugEnabled() {
		return this.debug;
	}

	public void enableDebug() {
		System.out.println("Debug Enabled");
		this.debug = true;
	}

	public void disableDebug() {
		System.out.println("Debug disabled");
		this.debug = false;
	}
}

private static String someHeavyMethod(String returnValue) {
	System.out.println("HeavyMethodDone");
	return returnValue;
}

public static void main(String[] args) {
	Logger logger = new Logger();

	logger.enableDebug();
	logger.debug("try without lambda", someHeavyMethod("line1"));
	System.out.println();

	logger.disableDebug();
	logger.debug("try without lambda", someHeavyMethod("line2"));
	System.out.println();

	logger.enableDebug();
	logger.debug("try with lambda", () -> someHeavyMethod("line3"));
	System.out.println();
		
	logger.disableDebug();
	logger.debug("try with lambda", () -> someHeavyMethod("line4"));
	/* 결과 
	Debug Enabled
	HeavyMethodDone
	try without lambda line1

	Debug disabled
	HeavyMethodDone

	Debug Enabled
	HeavyMethodDone
	try with lambda line3

	Debug disabled 
	*/
}
```

위의 예에서 Debug Disable된 상태에서는 someHeavyMethod는 작동할 필요가 없다.    
lambda를 이용하면 lazy evaluation이 가능하다.

#### anonymous class
익명 객체를 람다식으로 대체하면 훨씬 간결하고 보기 좋은 코드를 만들 수 있다.
```java
public interface Anonymous {
	public void method();
}
public static void main(String[] args) {
	Anonymous anonymous = new Anonymous(){
		public void method() {
			System.out.println("this can be replaced with lambda");
		};
	};

	anonymous.method();

	Anonymous lambda = () -> System.out.println("this is replaced with lambda");

	lambda.method();
	/* 결과
	this can be replaced with lambda
	this is replaced with lambda
	*/
}
```