---
layout: post
title:  "[Spring Reference] 스프링 레퍼런스 #1 핵심 - 2. 리소스"
createdDate:   2021-10-16T17:07:00+09:00
date:   2021-11-07T14:04:00+09:00
excerpt: "한글 번역 : 스프링 레퍼런스 #1 핵심 - 2. 리소스"
pagination: enabled
author: SoonYong Hong
categories: Spring_Reference
tags: Spring 
sitemap:
    changefreq: yearly
---

이 내용은 [스프링 문서 5.2.6.RELEASE](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#spring-core)를 번역한 내용으로 오역이 있을 수 있습니다.

# 목차
<h2><a href="../spring-reference-core-1-beans/#spring-core"> 핵심 기술 </a></h2>

1. <a href="../spring-reference-core-1-beans"> IoC 컨테이너 </a>
1. <a href="#resources"> 리소스 </a>
    1. <a href="#resources-introduction">소개</a>
    1. <a href="#resources-resource">Resource 인터페이스</a>
    1. <a href="#resources-implementations">내장된 Resource 구현체</a>
        1. <a href="#resources-implementations-urlresource">`UrlResource`</a>
        1. <a href="#resources-implementations-classpathresource">`ClassPathResource`</a>
        1. <a href="#resources-implementations-filesystemresource">`FileSystemResource`</a>
        1. <a href="#resources-implementations-servletcontextresource">`ServletContextResource`</a>
        1. <a href="#resources-implementations-inputstreamresource">`InputStreamResource`</a>
        1. <a href="#resources-implementations-bytearrayresource">`ByteArrayResource`</a>
    1. <a href="#resources-resourceLoader">`ResourceLoader`</a>
    1. <a href="#resources-resourceloaderaware">`ResourceLoaderAware`</a>
    1. <a href="#resources-as-dependencies">의존성으로서 리소스</a>
    1. <a href="#resources-app-ctx">어플리케이션 컨텍스트와 리소스 경로</a>
        1. <a href="#resources-app-ctx-construction">어플리케이션 컨텍스트 생성하기</a>
            1. <a href="#resources-app-ctx-classpathxml">`ClassPathXmlApplicationContext`인스턴스 생성하기 - 속성법</a>
        1. <a href="#resources-app-ctx-wildcards-in-resource-paths">어플리케이션 컨텍스트 생성자 리소스 경로에 와일드카드 사용하기</a>
            1. <a href="#resources-app-ctx-ant-patterns-in-paths">Ant 표현 형식</a>
            1. <a href="#resources-classpath-wildcards">`classpath*:` 접두사</a>
            1. <a href="#resources-wildcards-in-path-other-stuff">와일드카드와 관련된 다른 이야기들</a>
        1. <a href="#resources-filesystemresource-caveats">`FileSystemResource`에 대한 주의사항</a>
1. <a href="../spring-reference-core-3-validation/#validation"> 검증, 데이터 바인딩, 타입 변환 </a>
1. <a href="../spring-reference-core-4-expressions/#expressions"> 스프링 표현 언어(SpEL) </a>
1. <a href="../spring-reference-core-5-aop/#aop"> 스프링과 함께하는 관점 지향 프로그래밍(AOP) </a>
1. <a href="../spring-reference-core-6-aop-api/#aop-api"> 스프링 AOP API </a>
1. <a href="../spring-reference-core-7-null-safety/#null-safety"> 안전한 Null </a>
1. <a href="../spring-reference-core-8-databuffers/#databuffers"> 데이터 버퍼와 코덱 </a>
1. <a href="../spring-reference-core-9-appendix/#appendix"> 부록 </a>

---

<h2 id="resources"> 리소스 </h2>

이 챕터에서 스프링이 자원을 어떻게 처리하고 스프링 시스템내에서 처리한 자원을 어떻게 사용할 수 있는지 설명할 것이다. 아래의 주제를 포함하고 있다:

* [소개](#resources-introduction)
* [Resource 인터페이스](#resources-resource)
* [내장된 Resource 구현체](#resources-implementations)
* [`ResourceLoader`](#resources-resourceLoader)
* [`ResourceLoaderAware`](#resources-resourceloaderaware)
* [의존성으로서 리소스](#resources-as-dependencies)
* [어플리케이션 컨텍스트와 리소스 경로](#resources-app-ctx)

<h3 id="resources-introduction">소개</h3>

자바 표준의 `java.net.URL` 클래스와 핸들러만으로 모든 로우 레벨 자원에 접근하기에는 충분하지 못하다. 예를들면 클래스 패스나 `ServletContext`로부터 자원을 가져올 필요가 있을 때, 표준화된 URL 구현이 없다. 특정 `URL` 접두사에 따른 새로운 핸들러(기존에 등록되어 있는 `http:` 접두사에 등록된 핸들러와 비슷하다)를 등록하는 것은 가능하지만 매우 복잡하며 `URL` 인터페이스에는 필요한 기능이 부족하다. 예를 들면 요청하는 자원이 존재하는지 확인하는 것과 같은 기능이 부족하다.

<h3 id="resources-resource">Resource 인터페이스</h3>

스프링의 `Resource` 인터페이스는 로우 레벨 자원에 대한 접근을 추상화한 인터페이스로 더 많은 기능을 가지고 있다. 아래는 `Resource` 인터페이스의 정의이다:

```java
public interface Resource extends InputStreamSource {

    boolean exists();

    boolean isOpen();

    URL getURL() throws IOException;

    File getFile() throws IOException;

    Resource createRelative(String relativePath) throws IOException;

    String getFilename();

    String getDescription();
}
```

`Resource`의 정의가 보여주듯이 `InputStreamSource`를 확장한 인터페이스이다. 아래는 `InputStreamSource` 인터페이스의 정의이다:

```java
public interface InputStreamSource {

    InputStream getInputStream() throws IOException;
}
```

`Resource` 인터페이스의 메소드 중 중요한 메소드는 아래와 같다:

* `getInputStream()`: 자원을 열어 해당 자원을 읽는 `InputStream`을 반환한다. 매번 호출할 때마다 새로운 `InputStream`을 반환할것으로 기대된다. 스트림의 종료는 호출자가 담당한다.
* `exists()`: 자원이 물리적으로 존재하는지에 대하여 `boolean`값으로 반환한다.
* `isOpen()`: 자원이 열려있는 스트림을 의미하는지에 대하여 `boolean`값으로 반환한다. 만약 `true`를 반환한다면, `InputStream`은 여러 번 읽어올수 없기에 한 번만 읽은 후 메모리 누수를 막기 위해 종료되어야 한다. `InputStreamResource`를 제외한 일반적인 리소스 구현체에서는 `false`를 반환한다.
* `getDescription()`: 자원에 대한 설명을 반환한다. 이 자원으로 작업하는 중 에러가 발생할 시 관련된 에러 출력을 하기위해 사용된다. 일반적으로 파일경로를 포함한 파일 이름이나 자원의 실제 URL을 반환한다.

다른 메소드는 실제 `URL`이나 `File` 객체를 획득하는 메소드이다(단 해당 구현체가 기능을 지원할 경우에만 가능하다).

스프링은 자체적으로 `Resource`를 리소스가 필요한 메소드의 어규먼트로 많이 사용한다. 스프링 API의 일부 메소드(예를 들면 다양한 `ApplicationContext` 구현체의 생성자)에서는 `Resource`를 생성하는데 필요로 하는 접두사가 있는 `String`경로와 같은 값을 `String`으로 받아서 사용한다.

스프링에서 `Resource`가 많이 사용된다. 스프링의 다른 부분에 대해서 모르는 경우에도 자원을 접근할 수 있는 일반적인 유틸리티 클래스로서 `Resource`를 사용하는 것은 매우 유용하다. 이렇게 하면 스프링과 작성한 코드가 결합을 가지게되지만 매우 작은 부분이며 `URL` 클래스나 같은 동작을 하는 다른 라이브러리로 손쉽게 변경이 가능하다.

| |
| ----- |
| ***!** `Resource`추상화는 기능을 대체하지는 않는다. 예를 들면, `UrlResource`는 URL을 감싸는 클래스이며 실제 동작시에는 `URL`이 실제 작업을 진행한다.* |

<h3 id="resources-implementations">내장된 Resource 구현체</h3>

스프링은 아래의 `Resource` 구현체를 포함하고 있다:

* [`UrlResource`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/core.html#resources-implementations-urlresource)
* [`ClassPathResource`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/core.html#resources-implementations-classpathresourcehttps://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/core.html#resources-implementations-classpathresource)
* [`FileSystemResource`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/core.html#resources-implementations-filesystemresource)
* [`ServletContextResource`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/core.html#resources-implementations-servletcontextresource)
* [`InputStreamResource`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/core.html#resources-implementations-inputstreamresource)
* [`ByteArrayResource`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/core.html#resources-implementations-bytearrayresource)

<h4 id="resources-implementations-urlresource">`UrlResource`</h4>

`UrlResource`는 `java.net.URL`을 감싸는 래퍼 클래스로 파일, HTTP, FTP 등 URL을 통하여 획득할 수 있는 자원에 접근할 때 사용한다. 모든 URL은 표준 `String` 표현법을 가진다. 이 표현법에서 접두사로 URL 타입을 나타낸다. 예를 들면 파일 시스템 경로에 접근하는 `file:`, HTTP 프로토콜로 자원에 접근하는 `http:`, FTP 프로토콜로 자원에 접근하는 `ftp:` 등등 있다.

`UrlResource`는 `UrlResource` 생성자를 사용해서 명시적으로 생성된다. 하지만 경로 표현을 나타내는 `String` 어규먼트를 사용하는 API에서 내부적으로 생성하는 경우도 매우 많다. 이러한 내부적으로 생성되는 경우, `PropertyEditor`가 `Resource`의 타입을 결정한다. `classpath:`와 같은 잘 알려진 접두사를 사용한다면, 그 접두사에 알맞은 `Resource`를 생성한다. 하지만 적절한 `Resource`를 찾지 못한다면 일반적인 URL 문자열로 가정하여 `UrlResource`를 생성한다.

<h4 id="resources-implementations-classpathresource">`ClassPathResource`</h4>

이 클래스는 클래스패스에서 획득해야하는 자원을 나타내는 클래스이다. 이 클래스는 쓰레드 컨텍스트  클래스로더, 주어진 클래스로더 혹은 그 클래스 자체를 이용하여 자원을 로드한다.

이 `Resource` 구현체는 자원이 jar파일의 클래스 패스안에 있지 않고 파일시스템에 있을 경우 `java.io.File`로 전환을 지원한다. 첨언하자면, 많은 `Resource` 구현체들은 `java.net.URL`로 전환을 지원한다.

`ClassPathResource`는 `ClassPathResource` 생성자를 사용해서 명시적으로 생성된다. 하지만 경로 표현을 나타내는 `String` 어규먼트를 사용하는 API에서 내부적으로 생성하는 경우도 매우 많다. 이러한 내부적으로 생성되는 경우, `PropertyEditor`가 `Resource`의 타입을 결정한다. `classpath:` 접두사를 사용한다면, `ClassPathResource`를 생성한다.

<h4 id="resources-implementations-filesystemresource">`FileSystemResource`</h4>

`java.io.File`과 `java.nio.file.Path`를 다루는 `Resource` 구현체이다. `File`과 `URL`로의 전환을 지원한다.

<h4 id="resources-implementations-servletcontextresource">`ServletContextResource`</h4>

웹 어플리케이션 루트 디렉토리로부터 상대 경로를 해석하는 `ServletContext`의 자원을 처리하는 `Resource` 구현체이다.

URL과 스트림을 처리할 수 있다. 또한 웹 어플리케이션 아카이브가 확장되어 있고 파일시스템에 자원이 존재하는 경우 `java.io.File`도 처리할 수 있다. 확장되는가, 파일시스템에 있는가, JAR에 있는가, DB에 있는가 와는 상관없이 실제로는 처리할 수 있는지 없는지는 서블릿 컨테이너에 달려 있다.

<h4 id="resources-implementations-inputstreamresource">`InputStreamResource`</h4>

`InputStreamResource`는 `InputStream`을 처리하는 `Resource` 구현체이다. 이 구현체는 다른 `Resource` 구현체가 사용불가 할때만 사용해야 한다. 즉, `ByteArrayResource`나 여타 파일 기반 `Resource` 구현체를 사용할 수 있다면 사용해야 한다.

다른 `Resource` 구현체와 달리 이미 개봉된 자원에 대한 것이다. 즉 `isOpen()`메소드는 `true`를 반환한다. 여러번 읽을 필요가 있는 자원이나 계속 유지해야 하는 자원에는 사용하지 말아야 한다.

<h4 id="resources-implementations-bytearrayresource">`ByteArrayResource`</h4>

바이트 배열을 처리하는 `Resource` 구현체이다. 주어진 바이트 배열을 `ByteArrayInputStream`으로 바꿔 처리한다.

단 한번만 사용할 수 있는 `InputStreamResource`를 사용하지 않고 주어진 바이트 배열을 로드하는데 유용하게 사용된다.

<h3 id="resources-resourceLoader">`ResourceLoader`</h3>

`ResourceLoader` 인터페이스는 `Resource` 인스턴스를 로드하여 반환할 수 있는 객체에 사용된다. 아래는 `ResourceLoader` 인터페이스의 정의이다:

```java
 public interface ResourceLoader {

    Resource getResource(String location);
}
```

모든 어플리케이션 컨텍스트는 `ResourceLoader`인터페이스의 구현체이다. 즉 모든 어플리케이션 컨텍스트를 사용하여 `Resource`를 획득할 수 있다.

특정 어플리케이션 컨텍스트에서 `getResource()`를 호출할 경우 파라메터로 전달하는 경로에 구체적인 접두사가 없다면 어플리케이션 컨텍스트 타입에 따라서 반환되는 `Resource`의 타입이 결정된다. 아래의 예시가 `ClassPathXmlApplicationContext` 인스턴스로 실행했다고 생각해보다:

```java
Resource template = ctx.getResource("some/resource/path/myTemplate.txt");
```

`ClassPathXmlApplicationContext`의 경우 `ClassPathResource`를 반환한다. 같은 메소드가 `FileSystemXmlApplicationContext`에서 실행되면 `FileSystemResource`를 반환할 것이다. `WebApplicationContext`의 경우는 `ServletContextResource`이다. 각각에 컨텍스트에 걸맞는 것을 반환할 것이다.

즉 특정 어플리케이션 컨특스트에 걸맞는 방식으로 리소스를 로드할 수가 있다는 말이다.

반면에 `classpath:` 접두사를 사용하여 어플리케이션 컨텍스트의 종류와 관계없이 `ClassPathResource`가 사용되도록 할 수 있다. 아래는 그 예시이다:

```java
Resource template = ctx.getResource("classpath:some/resource/path/myTemplate.txt");
```

비슷하게 `java.net.URL` 표준 접두사를 사용하여 `UrlResource`를 사용되도록 할 수 있다. 아래의 예시는 `file` 접두사와 `http` 접두사의 예시이다:

```java
Resource template = ctx.getResource("file:///some/resource/path/myTemplate.txt");
```

```java
Resource template = ctx.getResource("https://myhost.com/resource/path/myTemplate.txt");
```

아래 표는 `String`경로를 `Resource`로 전환하는 전략에 대해 보여준다.

#### 표 10. 리소스 문자열

| 접두사 | 예시 | 설명 |
| ----- | ----- | ----- |
| classpath: | `classpath:com/myapp/config.xml` | 클래스 패스에서 로드한다. |
| file: | `file:///data/config.xml` | 파일 시스템에서 `URL`로서 로드한다. 자세한 내용은 [`FileSystemResource`에 대한 주의사항](#resources-filesystemresource-caveats)을 보면된다. |
| http: | `https://myserver/logo.png` | `URL`로서 로드한다. |
| (없음) | `/data/config.xml` | `ApplicationContext`에 따라 다르다. |

<h3 id="resources-resourceloaderaware">`ResourceLoaderAware`</h3>

`ResourceLoaderAware`인터페이스는 `ResourceLoacer`의 참조를 제공받는 콜백 인터페이스이다. 아래는 `ResourceLoaderAware`인터페이스의 정의이다:

```java
public interface ResourceLoaderAware {

    void setResourceLoader(ResourceLoader resourceLoader);
}
```

클래스가 `ResourceLoaderAware`를 구현하고 어플리케이션 컨텍스트에 (스프링 빈으로서) 배포되었다면, 어플리케이션 컨텍스트에 의하여 `ResourceLoaderAware`로서 처리될 것이다. 어플리케이션 컨텍스트는 `setResourceLoader(ResourceLoader)`를 호출하면서 자기자신을 어규먼트로 넘겨줄 것이다(모든 어플리케이션 컨텍스트는 `ResoureLoader`인터페이스의 구현체임을 이미 언급하였었다).

`ApplicationContext`가 `ResourceLoader`이기 때문에 `ApplicationContextAware`인터페이스를 구현한 경우에도 어플리케이션 컨텍스트를 전달받아서 자원을 로드하는데 사용할 수 있다. 하지만 일반적으로 `ResourceLoader`인터페이스만 필요하다면 `ResourceLoader`를 사용하는 것이 좋다. 그렇게하면 스프링의 `ApplicationContext` 인터페이스 전체가 아닌 자원을 로드하는데 사용하는 인터페이스(유틸리티 인터페이스)와만 결합하게 되어 결합도가 낮아지게 된다.

어플리케이션 컴포넌트에서 `ResourceLoaderAware`인터페이스 대신 빈 자동 연결을 이용하여 `ResourceLoader`를 획득할 수 있다. `constructor`나 `byType` 자동연결 모드 ([자동연결 협력자](../spring-reference-core-1-beans/#beans-factory-autowire)에서 설명되었다)를 사용하면 각각 생성자의 어규먼트와 메소드 파라메터로 `ResourceLoader`를 획득할 수 있다. 더 많은 유연성을 확보하기 위해서(필드 자동연결과 파라메터 여러개인 메소드 자동연결을 포함하는 내용이다), 어노테이션 기반 자동연결 기능을 사용하는 것도 좋다. 이러한 경우에 `ResourceLoader`는 필드, 생성자 어규먼트, 메소드 파라메터를 통하여 타입기반 자동연결 될 수 있다. 단 필드, 생성자, 메소드에 `@Autowired`를 사용했을 경우이다. 자세한 내용은 [`@Autowired` 사용하기](../spring-reference-core-1-beans/#beans-factory-autowire)에서 볼 수 있다.

<h3 id="resources-as-dependencies">의존성으로서 리소스</h3>

빈 내부에 동적으로 자원의 경로를 획득하는 과정이 있다면, `ResourceLoader`인터페이스를 사용하여 자원을 획득하는 것이 알맞다. 예를 들어, 유저의 선택에 따라서 특정한 템플릿을 로드해야하는 경우를 생각해보자. 해당 자원이 정적이라면 `ResourceLoader`를 사용하지 않는 것이 맞는 방법이다. 대신 `Resource` 프로퍼티를 가져 `Resource`를 주입받으면 된다.

이러한 `Resource`프로퍼티를 주입하는 것은 매우 간단한다. 왜냐하면 모든 어플리케이션 컨텍스트는 `PropertyEditor`라는 특별한 자바빈을 등록하여 사용한다. 이 빈은 `String`을 `Resource` 객체로 변환한다. 그래서 `myBean`이 `Resource`타입의 프로퍼티를 가지는 빈이라면 아래의 예시처럼 자원을 표현하는 간단한 문자열로 설정할 수 있다:

```xml
<bean id="myBean" class="...">
    <property name="template" value="some/resource/path/myTemplate.txt"/>
</bean>
```

위 예시에서 자원의 경로에 접두사가 없다. 모든 어플리케이션 컨텍스트는 `ResourceLoader`이기에 어플리케이션 컨텍스트의 타입에 따라 자원이 `ClassPathResource`, `FileSystemResource`, `ServletContextResource`로서 로드될 것이다.

특정한 타입의 `Resource`를 사용하려면 접두사를 사용하면 된다. 아래의 두개의 예시는 각각 `ClassPathResource`와 `UrlResource`(이 경우에서는 파일 시스템에서 자원을 획득한다)를 사용하는 예시이다:

```xml
<property name="template" value="classpath:some/resource/path/myTemplate.txt">
```

```xml
<property name="template" value="file:///some/resource/path/myTemplate.txt"/>
```


<h3 id="resources-app-ctx">어플리케이션 컨텍스트와 리소스 경로</h3>

이 장에서 어플리케이션 컨텍스트가 어떻게 자원을 생성하는지 이야기할 것이다. XML을 사용하여 자원을 생성하는 방법, 와일드 카드를 사용하는 방법 등등이 포함되어 있다.

<h4 id="resources-app-ctx-construction">어플리케이션 컨텍스트 생성하기</h4>

일반적으로 어플리케이션 컨텍스트 구현체들의 생성자는 컨텍스트 정의 xml 파일과 같은 자원의 경로를 한개 혹은 여러개의 문자열로 받아서 사용한다.

이러한 경로에 접두사가 없다면 어플리케이션 컨텍스트의 종류에 따라 특정한 `Resource`가 생성되어 빈정의를 로드하는데 사용된다. 예를 들면, 아래의 `ClassPathXmlApplicationContext`을 생성하는 예시를 보자:

```java
ApplicationContext ctx = new ClassPathXmlApplicationContext("conf/appContext.xml");
```

`ClassPathResource`가 사용되어서 클래스패스로부터 빈 정의를 로드한다. 하지만 아래의 `FileSystemXmlApplicationContext`을 생성하는 예시를 보자:

```java
ApplicationContext ctx =
    new FileSystemXmlApplicationContext("conf/appContext.xml");
```

이제 빈 정의를 파일 시스템에서 로드한다(이 경우, 현재 디렉토리에서부터 상대경로이다).

특정한 클래스패스 접두사나 표준 URL 접두사를 사용하면 기본 타입의 `Resource` 대신 특정한 타입의 `Resource`를 사용해 빈 정의를 로드한다. 아래의 예시를 보자:

```java
ApplicationContext ctx =
    new FileSystemXmlApplicationContext("classpath:conf/appContext.xml");
```

`FileSystemXmlApplicationContext`를 사용하여 클래스패스로부터 빈 정의를 읽어왔다. 클래스패스로부터 읽어왔음에도 `FileSystemXmlApplicationContext`를 사용한 경우이다. 이 어플리케이션 컨텍스트를 `ResourceLoader`로서 사용할 경우 접두사가 없는 모든 경로는 여전히 파일시스템 경로로 여길 것이다.

<h5 id="resources-app-ctx-classpathxml">`ClassPathXmlApplicationContext`인스턴스 생성하기 - 속성법</h5>

`ClassPathXmlApplicationContext`은 손쉽게 초기화 할 수 있는 다양한 생성자를 가지고 있다. 가장 기본적인 방법은 단순히 XML 파일의 이름을 포함하고 있는 문자열들의 배열과 한개의 `Class`를 제공하는 것이다. `ClassPathXmlApplicationContext`는 제공된 클래스로부터 경로 정보를 획득한다.

아래의 디렉토리 구조를 생각해보자:

```
com/
  foo/
    services.xml
    daos.xml
    MessengerService.class
```

아래의 예시는 `services.xml`과 `daos.xml`로 구성되는 `ClassPathXmlApplicationContext`를 초기화 하는 예시이다:

```java
ApplicationContext ctx = new ClassPathXmlApplicationContext(
    new String[] {"services.xml", "daos.xml"}, MessengerService.class);
```

[`ClassPathXmlApplicationContext`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/jca/context/SpringContextResourceAdapter.html) 자바독에서 다양한 생성자에 대한 자세한 내용을 볼 수 있다.

<h4 id="resources-app-ctx-wildcards-in-resource-paths">어플리케이션 컨텍스트 생성자 리소스 경로에 와일드카드 사용하기</h4>

어플리케이션 컨텍스트 생성자에 전달되는 자원의 경로는 `Resource`한 개를 의미하는 단순 경로일 수도 있고 "classpath*:" 접두사를 가지고 있거나 Ant 형식의 표현식일 수도 있다(Ant 형식의 표현식은 스프링 `PathMather` 유틸리티를 이용해 경로를 확인한다). "classpath*:" 접두사와 Ant 형식의 표현식 모두 와일드카드를 사용하는 방식이다.

여러 구성요소가 모여서 구성되는 어플리케이션을 설정할 때, 이 방법을 사용할 수 있다. 모든 구성요소는 미리 정해진 경로에 컨텍스트 정의를 발급하고 최종 어플리케이션에서 "classpath*:"접두사를 사용하여 모든 컨텍스트 정의를 모아서 어플리케이션을 생성한다.

와일드카드는 어플리케이션 컨텍스트 생성자에서만 사용된다(혹은 `PathMatcher` 유틸리티를 직접 사용할 수도 있다). `Resource` 타입 자체와는 아무 관련이 없다. `classpath*:` 접두사를 `Resource` 생성자에서 사용할 수 없다.

<h5 id="resources-app-ctx-ant-patterns-in-paths">Ant 표현 형식</h5>

아래 예시처럼 Ant 형식의 패턴을 사용하여 경로를 나타낼 수 있다:

```
/WEB-INF/*-context.xml
com/mycompany/**/applicationContext.xml
file:C:/some/path/*-context.xml
classpath:com/mycompany/**/applicationContext.xml
```

Ant 형식의 표현으로 경로를 나타내는 경우에 와일드카드를 처리하기 위한 복잡한 과정이 추가된다. 와일드카드가 나오기 전까지의 경로로 `Resource`를 획득한 뒤, 그 리소스에서 URL을 획득한다. 이 URL이 `jar:` URL이나 여타 컨테이너에 종속적인 URL(WebLogic의 `zip:`, WebSphere의 `wsjar` 등등)이 아니라면 URL로 `java.io.File`을 만들어서 파일시스템을 탐색하는데에 사용한다. jar URL인 경우에는 `java.net.JarUrlConnection`을 가져와서 탐색에 사용하거나 수동으로 jar URL 경로를 파싱하여 탐색한다.

###### 휴대성과 관련된 내용

경로가 파일 URL인 경우(기본 `ResourceLoader`에 의해서 내부적으로 파일 URL로 처리되는 경우와 명시적으로 파일 URL임을 표시한 경우 모두를 말한다), 와일드카드를 사용해서 휴대성을 보장한다.

명시된 경로가 클래스패스 경로일 경우, `ClassLoader.getResource()`를 사용하여 와일드카드가 나오기 전까지의 URL을 획득한다. 이 URL이 파일이 아닌 중간 경로이기에 어떠한 URL이 반환될지는 정해져 있지 않다(`ClassLoader` 자바독에 자세히 나와있다). 실제 사용할 때 반환되는 URL은 디렉토리를 나타내는 `java.io.File`이거나 jar URL이다. 하지만 여기에는 휴대성의 문제가 있다.

와일드카드가 나오기 전까지의 경로가 jar URL이라면 `java.net.JarURLConnection`을 가져오거나 수동으로 URL을 파싱해서 와일드카드를 처리해야한다. 이 방법은 대부분의 환경에서 실패한다. 따라서 jar에서 리소스를 가져올 때, 와일드카드를 사용하는 방법은 철처히 테스트를 한 후에 사용해야한다.

<h5 id="resources-classpath-wildcards">`classpath*:` 접두사</h5>

XML 기반의 어플리케이션 컨텍스트를 생성할 때, 아래의 예시처럼 `classpath*:` 접두사를 사용하여 위치를 나타내는 경우가 많다:

```java
ApplicationContext ctx =
    new ClassPathXmlApplicationContext("classpath*:conf/appContext.xml");
```

주어진 이름에 일치하는 모든 클래스패스 자원을 합쳐 어플리케이션 컨텍스트 정의로 사용하도록 하는 접두사이다. (내부적으로 `ClassLoader.getResources(...)`를 호출하여 자원을 획득한다.)

| |
| ----- |
| ***!** 클래스로더의 `getResources()` 메소드에 의존하여 와일드카드를 처리한다. 대부분의 어플리케이션 서버들은 자신만의 클래스로더 구현체를 가지고 있으며 각각 다르게 동작한다. 특히 jar 파일을 다루는 동작에서 서로 다르게 동작한다. `classpath*`가 동작하는지 확인하는 간단한 방법으로 `getClass().getClassLoader().getResources("<jar내부에있는아무파일">)`을 호출하여 클래스로더가 파일을 로드하도록 하는 방법이다. 다른 위치에 같은 이름을 가진 파일에 이 테스트를 실행하여 잘못된 결과를 얻게된다면 어플리케이션 서버 설정문서를 확인해서 클래스로더 동작에 영향을 미치는 설정을 찾아보도록 하자.* |

`classpath*:`접두사와 `PathMatcher`를 섞어서 사용할 수 있다(예를 들면 `classpath*:META_INF/*-beans.xml`). 이러한 경우에 처리 원리는 간단하다: 와일드 카드가 아닌 경로로 `ClassLoader.getResources()`를 호출하여 적합한 자원 경로를 획득한뒤 각각의 경로에 대하여 `PathMatcher`처리 방식을 적용하여 나머지 하위 와일드카드 경로를 처리한다.

<h5 id="resources-wildcards-in-path-other-stuff">와일드카드와 관련된 다른 이야기들</h5>

`classpath*:`가 Ant 형식의 패턴과 같이 사용되는 경우, 최소한 한개의 루트 디렉토리가 패턴이전에 존재하지 않는 경우 올바르게 동작하지 않을 수 있다. 단 파일시스템에 목표로하는 파일이 있는 경우에는 정상동작한다. 이 말은 `classpath*:*.xml`과 같은 패턴을 사용하면 jar의 루트에서 파일을 가져오지 못하고 확장된 디렉토리의 루트에서 파일을 가져오게 된다.

JDK의 `Classloader.getResources()`에 빈 문자열(검색을 위한 루트 경로를 의미한다)을 사용하여 호출하면 파일시스템 위치를 반환한다. 그리고 스프링은 이 메소드를 사용하여 클래스패트 경로를 처리한다. 스프링은 `URLClassLodaer`의 런타임 설정과 `java.class.path` 매니페스트를 고려하지만 원하는 동작을 보장하지는 못한다.

| |
| ----- |
| ***!** 클래스 패스의 패키지를 검색하기 위해서는 일치하는 디렉토리가 클래스패스에 있어야한다. Ant로 JAR를 빌드할 때, JAR 작업의 files-only를 활성화 하지 않아야한다. 또한 일부 환경에서 보안상의 이유로 클래스패스 디렉토리가 노출되지 않는다 - 예를들면 JDK 1.7.0_45 이상의 독립 어플리케이션에서 발생한다 (매니페스트에 'Trusted-Library`를 설정할 필요가 있다. 자세한 내용은 [https://stackoverflow.com/questions/19394570/java-jre-7u45-breaks-classloader-getresources](https://stackoverflow.com/questions/19394570/java-jre-7u45-breaks-classloader-getresources)를 보자). JDK 모듈 경로(jigsaw)에서 스프링의 클래스패스 스캔은 기대하는대로 동작한다. 하지만 자원을 걸맞은 위치에 넣는것을 추천한다. 위에 언급한 문제를 피할 수 있기 때문이다.* |

Ant 형식 패턴과 `classpath:`를 동시에 사용하였을 때, 루트 패키지가 여러 클래스패스 경로에서 사용되면 자원을 찾지 못할 수 있다. 아래의 예시를 보자:

```
com/mycompany/package1/service-context.xml
```

이제 Ant 형식 패턴의 예시를 보자:

```
classpath:com/mycompany/**/service-context.xml
```

이러한 자원은 한 경로에만 존재할 수 있다. 하지만 위의 예시에서 `getResources("com/mycompany");`를 먼저 호출하게 된다. 만약 기본 패키지 노드가 여러개의 클래스 로더 위치에 존재한다면 실제 자원은 발견되지 않을 수 있다. 따라서 이러한 경우 `classpath*:`과 Ant 형식 패턴을 사용하여 모든 클래스 패스 위치를 검색하도록 하는 것이 좋다.

<h4 id="resources-filesystemresource-caveats">`FileSystemResource`에 대한 주의사항</h4>

`FileSystemApplicationContext`과 관련없는 `FileSystemResource` (즉, `FileSystemApplicationContext`가 `ResourceLoader`가 아닌 경우)는 절대 경로와 상대경로를 기대한대로 처리한다. 상대경로는 현재 디렉토리로부터 상대적으로 처리하고 절대 경로는 파일시스템의 루트에서부터 처리한다.

하지만 구버전과 호환을 위하여 `FileSystemApplicationContext`가 `ResourceLoader`인 경우에는 다르다. `FileSystemApplicationContext`는 모든 `FileSystemResource`가 경로를 상대경로로 처리하도록 만든다. 경로가 /로 시작하는지와는 무관하게 모두 상대경로로 처리한다. 이 말은 아래 예시들이 모두 동일한 예시라는 의미이다:

```java
ApplicationContext ctx =
    new FileSystemXmlApplicationContext("conf/context.xml");
```

```java
ApplicationContext ctx =
    new FileSystemXmlApplicationContext("/conf/context.xml");
```

아래의 예시도 동일한 예시이다(하나는 절대 경로로 나머지 하나는 상대 경로로 처리되는 것이 알맞은 처리방법이기는 하다).

```java
FileSystemXmlApplicationContext ctx = ...;
ctx.getResource("some/resource/path/myTemplate.txt");
```

```java
FileSystemXmlApplicationContext ctx = ...;
ctx.getResource("/some/resource/path/myTemplate.txt");
```

절대 경로로 사용할 필요가 있다면 `FileSystemResource`나 `FileSystemXmlApplicationContext`에 절대 경로를 사용하는 것을 피하고 `file:` URL 접두사를 사용하여 `UrlResource`를 사용하는 것이 좋다. 아래는 그 예시이다:

```java
// 컨텍스트가 무엇인지와는 무관하게 UrlResource가 사용될 것이다.
ctx.getResource("file:///some/resource/path/myTemplate.txt");
```

```java
// FileSystemXmlApplicationContext가 UrlResource로 자원을 로드하도록 한다.
ApplicationContext ctx =
    new FileSystemXmlApplicationContext("file:///conf/context.xml");
```

[다음 장에서 계속](../spring-reference-core-3-validation/#validation)