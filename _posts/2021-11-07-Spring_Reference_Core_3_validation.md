---
layout: post
title:  "[Spring Reference] 스프링 레퍼런스 #1 핵심 - 3. 검증, 데이터 바인딩, 타입 변환"
createdDate:   2021-11-07T14:04:00+09:00
date:   2022-07-07T18:52:00+09:00
excerpt: "한글 번역 : 스프링 레퍼런스 #1 핵심 - 3. 검증, 데이터 바인딩, 타입 변환"
pagination: enabled
author: SoonYong Hong
categories: Spring_Reference
tags: Spring 
sitemap:
    changefreq: yearly
---

이 내용은 [스프링 문서 5.2.6.RELEASE](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#spring-core)를 번역한 내용으로 오역이 있을 수 있습니다.

현재 내용 작성중입니다.

# 목차
<h2><a href="../spring-reference-core-1-beans/#spring-core"> 핵심 기술 </a></h2>

1. <a href="../spring-reference-core-1-beans"> IoC 컨테이너 </a>
1. <a href="../spring-reference-core-2-resources#resources"> 리소스 </a>
1. <a href="#validation"> 검증, 데이터 바인딩, 타입 변환 </a>
    1. <a href="#validator">스프링 Validator 인터페이스를 사용하여 검증하기</a>
    1. <a href="#validation-conversion">코드를 에러메세지에 연결하기</a>
    1. <a href="#beans-beans">`BeanWrapper`와 빈 관리하기</a>
        1. <a href="#beans-beans-conventions">기본 프로퍼티, 중첩 프로퍼티 설정하기 그리고 획득하기</a>
        1. <a href="#beans-beans-conversion">여러 내장 PropertyEditor 구현체</a> 
            1. <a href="#beans-beans-conversion-customeditor-registration">커스텀 PropertyEditor 구현체 추가적으로 등록하기</a>
    1. <a href="#core-convert">스프링 타입 변환</a>
        1. <a href="#core-convert-Converter-API">Converter SPI</a>
        1. <a href="#core-convert-ConverterFactory-SPI">ConverterFactory 사용하기</a>
        1. <a href="#core-convert-GenericConverter-SPI">GenericConverter 사용하기</a>
            1. <a href="#core-convert-ConditionalGenericConverter-SPI">ConditionalGenericConverter 사용하기</a>
        1. <a href="#core-convert-ConversionService-API">ConversionService API</a>
        1. <a href="#core-convert-Spring-config">ConversionService 설정하기</a>
        1. <a href="#core-convert-programmatic-usage">ConversionService 프로그래밍 적으로 사용하기</a>
    1. <a href="#format">스프링 필드 형식지정</a>
        1. <a href="#format-Formatter-SPI">Formatter SPI</a>
        1. <a href="#format-CustomFormatAnnotations">어노테이션 기반 형식지정</a>
            1. <a href="#format-annotations-api">형식 어노테이션</a>
        1. <a href="#format-FormatterRegistry-SPI">FormatterRegistry SPI</a>
        1. <a href="#format-FormatterRegistrar-SPI">FormatterRegistrar SPI</a>
        1. <a href="#format-configuring-formatting-mvc">스프링 MVC에 형식 설정하기</a>
    1. <a href="#format-configuring-formatting-globaldatetimeformat">글로벌 날짜 시간 형식 설정하기</a>
    1. <a href="#validation-beanvalidation">자바 빈 검증</a>
        1. <a href="#validation-beanvalidation-overview">빈 검증 개요</a>
        1. <a href="#validation-beanvalidation-spring">빈 Validator Provider 설정하기</a>
            1. <a href="#validation-beanvalidation-spring-inject">Validator 주입하기</a>
            1. <a href="#validation-beanvalidation-spring-constraints">커스텀 제한 설정하기</a>
            1. <a href="#validation-beanvalidation-spring-method">스프링 기반 메서드 검증</a>
            1. <a href="#validation-beanvalidation-spring-other">추가 설정 옵션</a>
        1. <a href="#validation-binder">DataBinder 설정하기</a>
        1. <a href="#validation-mvc">스프링 MVC 3 검증</a>
1. <a href="../spring-reference-core-4-expressions/#expressions"> 스프링 표현 언어(SpEL) </a>
1. <a href="../spring-reference-core-5-aop/#aop"> 스프링과 함께하는 관점 지향 프로그래밍(AOP) </a>
1. <a href="../spring-reference-core-6-aop-api/#aop-api"> 스프링 AOP API </a>
1. <a href="../spring-reference-core-7-null-safety/#null-safety"> 안전한 Null </a>
1. <a href="../spring-reference-core-8-databuffers/#databuffers"> 데이터 버퍼와 코덱 </a>
1. <a href="../spring-reference-core-9-appendix/#appendix"> 부록 </a>

---

<h2 id="validation"> 검증, 데이터 바인딩, 타입 변환 </h2>

검증을 비즈니스 로직의 일부로 여기는 것에는 장점과 단점이 모두 있다. 스프링은 이 두가지 방식 모두를 제공하는 검증과 데이터 바인딩 방식을 가지고 있다. 특히 검증은 웹에만 한정되지 않아야하며 일부에만 적용하기 쉬워야 하고 아무 Validator나 적용할 수 있어야한다. 이러한 관점을 고려하여 스프링은 `Validator`를 제공한다. `Validator`는 어플리케이션의 모든 레이어에 간단히, 유용하게 사용할 수 있다.

데이터 바인딩은 유저 입력을 어플리케이션의 도메인 모델로 변환하는데 유용하다(또는 유저입력을 처리하고 싶은 아무 객체에나 가능하다). 스프링은 그 기능을 하는 `DataBinder`를 제공한다. `Validator`와 `DataBinder`는 `validation` 패키지를 구성하며 이들은 웹 레이어에서만 국한되지 않는다.

`BeanWrapper`는 스프링에서 기본적인 개념이고 다양한 곳에서 사용된다. 이 챕터에서 `BeanWrapper`를 설명할 것이고 대부분의 경우 데이터 바인딩 목적으로 사용될 것이다. 따라서 `BeanWrapper`를 직접적으로 사용하지 않아도 된다. 하지만 이 문서는 레퍼런스 문서이고 순서대로 설명할 필요가 있기 에 관련 내용을 이야기할 것이다.

스프링의 `DataBinder`와 로우 레벨 `BeanWrapper`는 `PropertyEditorSupport` 구현체를 사용하여 프로퍼티 값을 파싱한다. `PropertyEditor`와 `PropertyEditorSupport` 타입은 자바 빈 스펙에 포함되있으며 이 챕터에서 설명할 것이다. 스프링 3에서부터 `core.convert` 패키지를 제공하며 이 패키지에서 일반적인 형식 변환 기능과 UI 필드 값의 형식을 정하는 하이 레벨 "format" 패키지를 제공한다. 이 패키지들을 `PropertyEditorSupport` 대신에 사용할 수 있다. 이 방법도 이 챕터에서 이야기할 것이다.

스프링에서는 스프링 자체 `Validator`와 기반 설정을 이용하여 자바 빈 검증을 제공한다. [자바 빈 검증](#validation-beanvalidation)에서 이야기한 것처럼 빈 검증을 전체적으로 활성화 할 수 있다. 또한 필요한 부분에만 사용할 수도 있다. 웹 레이어에서는 컨트롤러에만 속하는 `Validator` 인스턴스를 `DataBinder` 마다 설정해 줄수 있다. 이 방법은 [`DataBinder` 설정하기](#validation-binder)에 설명되 있으며 커스텀 검증 로직을 추가하는데에 유용하게 사용된다.

<h3 id="validator">스프링 Validator 인터페이스를 사용하여 검증하기</h3>

`Validator` 인터페이스는 객체를 검증하는데 사용한다. `Validator`인터페이스는 `Errors` 객체를 사용하여 검증 오류를 알려준다.

아래 데이터 객체 예시를 생각해보자

```java
public class Person {

    private String name;
    private int age;

    // 생략된 게터와 세터
}
```

다음 예시는 `org.springframework.validation.Validator`의 아래의 두 메소드를 구현하여 `Person`클래스를 검증하는 예시이다:
* `supports(Class)`: 이 `Validator`가 주어진 `Class`를 검증할 수 있는가?
* `validate(Object, org.springframework.validation.Errors)`: 객체를 검증하고 오류가 확인되면 주어진 `Errors` 객체에 오류를 등록한다.

`Validator` 구현은 직관적이다. 특히 스프링이 제공하는 `ValidationUtils` 헬퍼 클래스를 사용한다면 더욱 직관적이다. 아래는 `Person` 클래스를 검증하는 `Validator` 구현체의 예시이다:

```java
public class PersonValidator implements Validator {

    /**
     * 이 Validator는 Person 인스턴스만 검증한다.
     */
    public boolean supports(Class clazz) {
        return Person.class.equals(clazz);
    }

    public void validate(Object obj, Errors e) {
        ValidationUtils.rejectIfEmpty(e, "name", "name.empty");
        Person p = (Person) obj;
        if (p.getAge() < 0) {
            e.rejectValue("age", "negativevalue");
        } else if (p.getAge() > 110) {
            e.rejectValue("age", "too.darn.old");
        }
    }
}
```

`ValidationUtils`의 `static` `rejectIfEmtpy(..)`는 `name`프로퍼티가 `null`이거나 빈 문자열인 경우에 거부하기 위해 사용한다. 위의 예시 외에 [`ValidationUtils`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/validation/ValidationUtils.html)의 추가적인 기능은 자바독에서 볼 수 있다.

복잡한 중첩 객체를 검증하기위해 하나의 `Validator` 클래스를 구현하는 것도 가능하지만 각 중첩된 객체를 검증하는 `Valdiator` 구현체를 만들어 검증로직을 캡슐화 시키는 것이 더 좋은 방법이다. 두개의 `String` 프로퍼티와 `Address` 프로퍼티를 가지고 있는 `Customer` 객체의 예시를 생각해보자. `Address`는 `Customer` 객체와 별도로 사용될 수도 있을 것이다. 따라서 별개의 `AddressValidator`가 구현된다. `CustomerValidator`가 `AddressValidator`의 로직을 사용하려면 복사-붙여넣기 대신에 의존성 주입이나 인스턴스화를 이용하여 `CustomerValidator`가 `AddressValdiator`를 포함하도록 하는 것이다. 아래는 그 예시이다:

```java
public class CustomerValidator implements Validator {

    private final Validator addressValidator;

    public CustomerValidator(Validator addressValidator) {
        if (addressValidator == null) {
            throw new IllegalArgumentException("The supplied [Validator] is " +
                "required and must not be null.");
        }
        if (!addressValidator.supports(Address.class)) {
            throw new IllegalArgumentException("The supplied [Validator] must " +
                "support the validation of [Address] instances.");
        }
        this.addressValidator = addressValidator;
    }

    /**
     * 이 Validator는 Customer 인스턴스와 자식 클래스 인스턴스를 검증한다.
     */
    public boolean supports(Class clazz) {
        return Customer.class.isAssignableFrom(clazz);
    }

    public void validate(Object target, Errors errors) {
        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "firstName", "field.required");
        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "surname", "field.required");
        Customer customer = (Customer) target;
        try {
            errors.pushNestedPath("address");
            ValidationUtils.invokeValidator(this.addressValidator, customer.getAddress(), errors);
        } finally {
            errors.popNestedPath();
        }
    }
}
```

validator에 전달되는 `Errors` 객체를 통해 검증오류를 알린다. 스프링 웹 MVC의 경우 `<spring:bind/>` 태그를 사용하여 에러메세지를 살필수 있다. 혹은 `Errors`객체를 직접 확인하는 것도 가능하다. `Errors`객체가 제공하는 메서드에 대한 자세한 내용은 [자바독](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframeworkvalidation/Errors.html)에서 확인할 수 있다.

<h3 id="validation-conversion">코드를 에러메세지에 연결하기</h3>

데이터 바인딩과 검증에 관한 내용을 완료했다. 이 장에서는 검증 오류에 맞는 메세지를 결과로 반환하는 방법에 대하여 이야기할 것이다. [이전 장](#validator)의 예시에서 우리는 `name`과 `age`필드를 검증하여 거부했었다. `MessageSource`를 이용하여 검증 실패 메세지를 전달하려면 필드(여기서는 name과 age)를 거부할 때 사용한 에러코드를 이용하면 된다. 우리가 `Errors`인터페이스의 `rejectValue`나 다른 `reject`메소드를 호출하면 내부 구현체가 전달받은 에러코드를 등록할 것이며 추가적으로 다른 에러코드도 같이 등록할 수 있다. `MessageCodesResolver`는 `Errors` 인터페이스가 등록한 에러코드가 어떤 것인지 결정할 것이다. 기본적으로, `DefaultMessageCodesResolver`는 내가 전달한 에러코드에 연결된 메세지뿐만 아니라 reject 메소드에서 거부한 필드의 이름을 포함하는 다른 메세지도 등록한다. 그래서 `rejectValue("age", "too.darn.old")` 메소드로 필드를 거부하는 경우, `too.darn.old`와는 별개로, 스프링이 `too.darn.old.age`와 `too.darn.old.age.int`도 등록한다(첫번째는 필드이름이 추가됬고 두번째는 필드의 타입도 추가됬다). 에러메세지를 노출할 때, 개발자에게 도움을 주기위해 구현되었다.

`MessageCodesResolver`와 기본 전략에 대하여 더 알고 싶은 경우 [`MessageCodesResolver`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/validation/MessageCodesResolver.html)와 [`DefaultMessageCodesResolver`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/validation/DefaultMessageCodesResolver.html)의 자바독을 살펴보면 된다.

<h3 id="beans-beans">`BeanWrapper`와 빈 관리하기</h3>

`org.springframework.beans` 패키지는 자바빈 표준을 충족하고 있다. 자바빈은 어규먼트가 없는 기본 생성자가 있고 네이밍 컨벤션을 따른다. 예를 들면 `bingoMadness`라는 프로퍼티의 세터는 `setBingoMadness(..)`며 게터는 `getBingoMadness(..)`이다. 자바빈과 그 스펙에 대하여 자세히 알고 싶으면 [자바빈](https://docs.oracle.com/javase/8/docs/api/java/beans/package-summary.html)을 보아라.

`BeansWrapper` 인터페이스와 그 구현체(`BeansWrapperImpl`)은 beans패키지에서 중요한 클래스이다. 자바독에서 언급했듯이, `BeansWrapper`는 프로퍼티를 가져오고 프로퍼티를 설정하며 프로퍼티 설명자를 가져오고 프로퍼티를 질의한다. 또한 `BeanWrapper`는 중첩 프로퍼티도 지원한다. 깊이에 제한없이 하위 프로퍼티를 설정할 수 있다. 또한 `BeanWrapper`는 타겟 클래스에서 자바빈 표준을 지원하는 코드를 작성하지 않아도 자바빈 표준 `PropertyChangeListeners`와 `VetoableChangeListener`를 추가해 줄 수 있다. 또한 `BeansWrapper`는 인덱스가 설정된 프로퍼티도 지원한다. `BeanWrapper`는 일반적으로 어플리케이션에서 직접 사용되지 않고 `DataBinder`와 `BeanFactory`에서 사용된다.

`BeanWrapper`가 동작하는 방법은 이름에서 유추할수 있듯이 빈을 감싸서 프로퍼티를 설정하거나 가져오는 방식으로 되어있다.

<h4 id="beans-beans-conventions">기본 프로퍼티, 중첩 프로퍼티 설정하기 그리고 획득하기</h4>

`BeanWrapper`구현체에서 오버로드되는 메소드인 `setPropertyValue`와 `getPropertyValue`를 사용하여 프로퍼티를 설정하거나 가져온다. 자바독에 자세한 내용이 있다. 아래의 표는 이러한 컨벤션의 예시이다:

#### 표 11. 프로퍼티 예시

| 표현 | 설명 |
| ----- | ----- |
| `name` | 프로퍼티 `name`을 의미한다. `setName(..)`을 포함하며 `getName()`이나 `isName()` 메소드를 포함한다. |
| `account.name` | 프로퍼티 `account`의 중첩 프로퍼티 `name`을 의미한다. `getAccount().getName()`이나 `getAccount().setName()`와 같은 메소드들을 포함한다. |
| `account[2]` | 프로퍼티 `account`의 세번째 요소를 의미한다. 인덱스가 있는 프로퍼티는 `array`나 `list` 혹은 다른 순서가 있는 컬렉션의 형태이다. |
| `account[COMPANYNAME]` | `account` `Map` 프로퍼티의 `COMPANYNAME` 키를 가진 엔트리의 값을 의미한다 |

(이 다음장은 `BeanWrapper`를 직접적으로 사용할 계획이 없다면 별로 중요하지 않을 수 있다. `DataBinder`와 `BeanFactory`, 이 두 인터페이스의 구현체만 사용할 예정이라면, [`PropertyEditors` 섹션](#beans-beans-conversion)으로 넘어가도 좋다.)

아래의 두 예제 클래스는 `BeanWrapper`를 사용하여 프로퍼티를 설정하고 가져오는 예시이다:

```java
public class Company {

    private String name;
    private Employee managingDirector;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Employee getManagingDirector() {
        return this.managingDirector;
    }

    public void setManagingDirector(Employee managingDirector) {
        this.managingDirector = managingDirector;
    }
}
```

```java
public class Employee {

    private String name;

    private float salary;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public float getSalary() {
        return salary;
    }

    public void setSalary(float salary) {
        this.salary = salary;
    }
}
```

아래의 코드 예시는 `Companies`와 `Employees` 인스턴스의 프로퍼티를 가져오고 변경하는 예시이다:

```java
BeanWrapper company = new BeanWrapperImpl(new Company());
// company의 name을 설정한다.
company.setPropertyValue("name", "Some Company Inc.");
// ... 아래 방식으로도 설정할 수 있다.
PropertyValue value = new PropertyValue("name", "Some Company Inc.");
company.setPropertyValue(value);

// director를 만들어서 company에 연결한다.
BeanWrapper jim = new BeanWrapperImpl(new Employee());
jim.setPropertyValue("name", "Jim Stravinsky");
company.setPropertyValue("managingDirector", jim.getWrappedInstance());

// company를 통하여 managingdirector의 salary를 가져온다. 
Float salary = (Float) company.getPropertyValue("managingDirector.salary");
```


<h4 id="beans-beans-conversion">여러 내장 PropertyEditor 구현체</h4> 

스프링은 `PropertyEditor`의 개념을 `Object`와 `String`간의 변환에 사용한다. 객체의 프로퍼티를 다양한 방법으로 표현할 때 유용하게 사용한다. 예를 들면, `Date`를 사람이 읽을 수 있는 방식으로 표현할 수 있고 (예를 들면, `String`:`2008-02-13`) 반대로 사람이 읽는 형식을 원래대로 바꿀 수 있다(더 나아가 사람이 읽을 수 있는 모든 형식을 `Date`객체로 바꿀 수 있다). 이 방식을 적용하려면 `java.beans.PropertyEdiotr` 타입의 커스텀 에디터를 등록하면 된다. `BeanWrapper`나 특정 역흐름제어 컨테이너에 커스텀 에디터를 등록하여 프로퍼티를 원하는 타입으로 전환하는 방법을 알려 줄 수 있다. `PropertyEditor`에 대한 자세한 정보는 [오라클 `java.beans`패키지 자바독](https://docs.oracle.com/javase/8/docs/api/java/beans/package-summary.html)을 확인하세요.

스프링에서 프로퍼티 변환이 사용되는 예시이다:

* `PropertyEditor` 구현체를 사용하여 빈에 프로퍼티를 설정한다. XML에 스프링 빈을 정의하고 `String`을 프로퍼티 값에 사용했을 때, 해당 프로퍼티에 대응하는 세터의 파라메터가 `Class` 타입이라면 `ClassEditor`를 사용하여 `Class` 객체로 변환한다.
* 스프링 MVC 프레임워크는 `PropertyEditor` 구현체를 사용하여 HTTP 요청 파라메터를 변환한다.

스프링은 다양한 내장 `PropertyEditor` 구현체를 제공한다. `org.springframework.beans.propertyeditors` 패키지에 포함되어 있다. 대부분은 `BeanWrapperImpl`에 기본적으로 등록되어 있다. 프로퍼티 에디터는 설정가능하기 때문에 커스텀 구현체를 등록하여 기본 동작을 변경 할 수 있다. 아래 표는 스프링이 제공하는 `PropertyEditor` 구현체이다:

#### 표 12. 내장 `PropertyEditor` 구현체

| 클래스 | 설명 |
| ----- | ----- |
| `ByteArrayPropertyEditor` | 바이트 배열을 처리하는 에디터. 문자열을 대응하는 바이트 표현으로 변환한다. `BeanWrapperImpl`에 의하여 기본으로 등록된다. |
| `ClassEditor` | 클래스를 표현하는 문자열을 실제 클래스로 변환한다. 그 반대도 변환한다. 클래스가 없으면 `IllegalArgumentException`을 발생시킨다. `BeanWrapperImpl`에 의하여 기본으로 등록된다. |
| `CustomBooleanEditor` | `Boolean` 프로퍼티를 변환하며 수정가능하다. `BeanWrapperImpl`에 의하여 기본으로 등록되지만 커스텀 에디터 인스턴스를 등록하여 덮어쓰기가 가능하다. |
| `CustomCollectionEditor` | 컬렉션을 처리하는 에디터. 컬렉션을 원하는 타입의 컬렉션으로 변환한다. |
| `CustomDateEditor` | `java.util.Date`를 처리하는 에디터. 커스텸 `DateFormat`을 사용할 수 있다. 기본으로 등록되지 않는다. 반드시 적절한 날짜 형식과 함께 사용자가 직접 등록해야한다. |
| `CustomNumberEditor` | `Number`의 하위 클래스 (예를 들면, `Integer`, `Long`, `Float`, `Double`)를 처리하는 에디터. `BeanWrapperImpl`에 의하여 기본으로 등록되지만 커스텀 에디터 인스턴스를 등록하여 덮어쓰기가 가능하다. |
| `FildEditor` | 문자열을 `java.io.File` 객체로 변환한다. `BeanWrapperImpl`에 의하여 기본으로 등록된다. |
| `InputStreamEditor` | 문자열을 입뎍받아 `ResourceEditor`와 `Resource`를 거쳐 `InputStream`으로 변환하는 에디터. `InputStream`을 닫지 않는다는 것에 유의하십시오. `BeanWrapperImpl`에 의하여 기본으로 등록된다. |
| `LocaleEditor` | 문자열(`[country][variant]`형식, `Locale`의 `toString()`메소드와 동일)과 `Locale` 객체간의 변환을 한다. `BeanWrapperImpl`에 의하여 기본으로 등록된다. |
| `PatternEditor` | 문자열과 `java.util.regex.Pattern` 객체간의 변환을 한다. |
| `PropertiesEditor` | 문자열(`java.util.Properties`클래스의 자바독에 정의된 형식)과 `Properties` 객체간의 변환을 한다.  `BeanWrapperImpl`에 의하여 기본으로 등록된다. |
| `StringTrimmerEditor` | 문자열의 좌우 공백을 제거하는 에디터. 기본으로 등록되지 않는다. 사용자가 직접 등록해야한다. |
| `URLEditor` | 문자열로 표현된 URL과 `URL` 객체간의 변환을 한다.  `BeanWrapperImpl`에 의하여 기본으로 등록된다. |

스프링은 `java.beans.PropertyEditorManager`를 사용하여 프로퍼티 에디터를 찾는 검색 경로를 설정한다. 이 검색 경로에는 `sun.bean.editors`가 포함되어 있고 이 경로는 `Font`, `Color`, 대부분의 기본 타입에 대응하는 `PropertyEditor` 구현체들을 가지고 있다. 또한 표준 자바빈즈 인프라는 `PropertyEditor`클래스가 처리하는 클래스와 같은 패키지에 있고 같은 이름에 `Editor`가 붙어 있다면 자동으로 `PropertyEditor` 클래스를 찾아 등록한다(명시적으로 등록할 필요가 없다). 예를 들면, 아래와 같은 패키지와 클래스가 있으면 `SomethingEditor` 클래스가 `PropertyEditor`로 등록되어 `Something` 타입의 처리에 사용된다.

```
com
  chank
    pop
      Something
      SomethingEditor // Something 클래스에 사용되는 PropertyEditor
```

또한 `BeanInfo` 자바빈즈 동작을 이용할 수 있다([여기](https://docs.oracle.com/javase/tutorial/javabeans/advanced/customization.html)에 자세히 설명되어 있다). 아래 예시는 `BeanInfo` 동작을 사용하여 `PropertyEditor`를 등록하는 예시이다:

```
com
  chank
    pop
      Something
      SomethingBeanInfo // Something 클래스를 위한 BeanInfo
```

아래의 자바코드는 `Something` 클래스의 `age` 프로퍼티에 `CustomNumberEditor`를 연결하는 `SomethingBeanInfo` 클래스의 코드이다4

```java
public class SomethingBeanInfo extends SimpleBeanInfo {

    public PropertyDescriptor[] getPropertyDescriptors() {
        try {
            final PropertyEditor numberPE = new CustomNumberEditor(Integer.class, true);
            PropertyDescriptor ageDescriptor = new PropertyDescriptor("age", Something.class) {
                public PropertyEditor createPropertyEditor(Object bean) {
                    return numberPE;
                };
            };
            return new PropertyDescriptor[] { ageDescriptor };
        }
        catch (IntrospectionException ex) {
            throw new Error(ex.toString());
        }
    }
}
```

<h5 id="beans-beans-conversion-customeditor-registration">커스텀 PropertyEditor 구현체 추가적으로 등록하기</h5>
<h3 id="core-convert">스프링 타입 변환</h3>
<h4 id="core-convert-Converter-API">Converter SPI</h4>
<h4 id="core-convert-ConverterFactory-SPI">ConverterFactory 사용하기</h4>
<h4 id="core-convert-GenericConverter-SPI">GenericConverter 사용하기</h4>
<h5 id="core-convert-ConditionalGenericConverter-SPI">ConditionalGenericConverter 사용하기</h5>
<h4 id="core-convert-ConversionService-API">ConversionService API</h4>
<h4 id="core-convert-Spring-config">ConversionService 설정하기</h4>
<h4 id="core-convert-programmatic-usage">ConversionService 프로그래밍 적으로 사용하기</h4>
<h3 id="format">스프링 필드 형식지정</h3>
<h4 id="format-Formatter-SPI">Formatter SPI</h4>
<h4 id="format-CustomFormatAnnotations">어노테이션 기반 형식지정</h4>
<h5 id="format-annotations-api">형식 어노테이션</h5>
<h4 id="format-FormatterRegistry-SPI">FormatterRegistry SPI</h4>
<h4 id="format-FormatterRegistrar-SPI">FormatterRegistrar SPI</h4>
<h4 id="format-configuring-formatting-mvc">스프링 MVC에 형식 설정하기</h4>
<h3 id="format-configuring-formatting-globaldatetimeformat">글로벌 날짜 시간 형식 설정하기</h3>
<h3 id="validation-beanvalidation">자바 빈 검증</h3>
<h4 id="validation-beanvalidation-overview">빈 검증 개요</h4>
<h4 id="validation-beanvalidation-spring">빈 Validator Provider 설정하기</h4>
<h5 id="validation-beanvalidation-spring-inject">Validator 주입하기</h5>
<h5 id="validation-beanvalidation-spring-constraints">커스텀 제한 설정하기</h5>
<h5 id="validation-beanvalidation-spring-method">스프링 기반 메서드 검증</h5>
<h5 id="validation-beanvalidation-spring-other">추가 설정 옵션</h5>
<h4 id="validation-binder">DataBinder 설정하기</h4>
<h4 id="validation-mvc">스프링 MVC 3 검증</h4>

[다음 장에서 계속](../spring-reference-core-4-expressions)