---
layout: post
title:  "[Spring Reference] 스프링 레퍼런스 #1 핵심 - 2. 리소스"
createdDate:   2021-10-16T17:07:00+09:00
date:   2021-10-22T19:58:00+09:00
excerpt: "한글 번역 : 스프링 레퍼런스 #1 핵심 - 2. 리소스"
pagination: enabled
author: SoonYong Hong
categories: Spring_Reference
tags: Spring 
sitemap:
    changefreq: weekly
---

이 내용은 [스프링 문서 5.2.6.RELEASE](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#spring-core)를 번역한 내용으로 오역이 있을 수 있습니다.

현재 내용 작성중입니다.

# 목차
<h2><a href="../spring-reference-core-1-beans/#spring-core"> 핵심 기술 </a></h2>

1. <a href="../spring-reference-core-1-beans"> IoC 컨테이너 </a>
1. <a href="#resources"> 리소스 </a>
<h3 id="resources-introduction">소개</a>
<h3 id="resources-resource">Resource 인터페이스</a>
<h3 id="resources-implementations">내장된 Resource 구현체</a>
<h4 id="resources-implementations-urlresource">`UrlResource`</a>
<h4 id="resources-implementations-classpathresource">`ClassPathResource`</a>
<h4 id="resources-implementations-filesystemresource">`FileSystemResource`</a>
<h4 id="resources-implementations-servletcontextresource">`ServletContextResource`</a>
<h4 id="resources-implementations-inputstreamresource">`InputStreamResource`</a>
<h4 id="resources-implementations-bytearrayresource">`ByteArrayResource`</a>
<h3 id="resources-resourceLoader">`ResourceLoader`</a>
<h3 id="resources-resourceloaderaware">`ResourceLoaderAware`</a>
<h3 id="resources-as-dependencies">의존성으로서 리소스</a>
<h3 id="resources-app-ctx">어플리케이션 컨텍스트와 리소스 경로</a>
<h4 id="resources-app-ctx-construction">어플리케이션 컨텍스트 생성하기</a>
<h5 id="resources-app-ctx-classpathxml">`ClassPathXmlApplicationContext`인스턴스 생성하기 - 속성법</a>
<h4 id="resources-app-ctx-wildcards-in-resource-paths">어플리케이션 컨텍스트 생성자 리소스 경로에 와일드카드 사용하기</a>
<h5 id="resources-app-ctx-ant-patterns-in-paths">Ant 표현 형식</a>
<h5 id="resources-classpath-wildcards">`classpath:&` 접두사</a>
<h5 id="resources-wildcards-in-path-other-stuff">와일드카드와 관련된 다른 이야기들</a>
<h4 id="resources-filesystemresource-caveats">`FileSystemResource`에 대한 주의사항</a>
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
<h3 id="resources-as-dependencies">의존성으로서 리소스</h3>
<h3 id="resources-app-ctx">어플리케이션 컨텍스트와 리소스 경로</h3>
<h4 id="resources-app-ctx-construction">어플리케이션 컨텍스트 생성하기</h4>
<h5 id="resources-app-ctx-classpathxml">`ClassPathXmlApplicationContext`인스턴스 생성하기 - 속성법</h5>
<h4 id="resources-app-ctx-wildcards-in-resource-paths">어플리케이션 컨텍스트 생성자 리소스 경로에 와일드카드 사용하기</h4>
<h5 id="resources-app-ctx-ant-patterns-in-paths">Ant 표현 형식</h5>
<h5 id="resources-classpath-wildcards">`classpath:&` 접두사</h5>
<h5 id="resources-wildcards-in-path-other-stuff">와일드카드와 관련된 다른 이야기들</h5>
<h4 id="resources-filesystemresource-caveats">`FileSystemResource`에 대한 주의사항</h4>