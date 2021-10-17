---
layout: post
title:  "[Spring Reference] 스프링 레퍼런스 #1 핵심 - 2. 리소스"
createdDate:   2021-10-16T17:07:00+09:00
date:   2021-10-17T09:25:00+09:00
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
<h4 id="resources-filesystemresource-caveats">`FileSystemResource`에 대한 경고</a>
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
<h4 id="resources-implementations-urlresource">`UrlResource`</h4>
<h4 id="resources-implementations-classpathresource">`ClassPathResource`</h4>
<h4 id="resources-implementations-filesystemresource">`FileSystemResource`</h4>
<h4 id="resources-implementations-servletcontextresource">`ServletContextResource`</h4>
<h4 id="resources-implementations-inputstreamresource">`InputStreamResource`</h4>
<h4 id="resources-implementations-bytearrayresource">`ByteArrayResource`</h4>
<h3 id="resources-resourceLoader">`ResourceLoader`</h3>
<h3 id="resources-resourceloaderaware">`ResourceLoaderAware`</h3>
<h3 id="resources-as-dependencies">의존성으로서 리소스</h3>
<h3 id="resources-app-ctx">어플리케이션 컨텍스트와 리소스 경로</h3>
<h4 id="resources-app-ctx-construction">어플리케이션 컨텍스트 생성하기</h4>
<h5 id="resources-app-ctx-classpathxml">`ClassPathXmlApplicationContext`인스턴스 생성하기 - 속성법</h5>
<h4 id="resources-app-ctx-wildcards-in-resource-paths">어플리케이션 컨텍스트 생성자 리소스 경로에 와일드카드 사용하기</h4>
<h5 id="resources-app-ctx-ant-patterns-in-paths">Ant 표현 형식</h5>
<h5 id="resources-classpath-wildcards">`classpath:&` 접두사</h5>
<h5 id="resources-wildcards-in-path-other-stuff">와일드카드와 관련된 다른 이야기들</h5>
<h4 id="resources-filesystemresource-caveats">`FileSystemResource`에 대한 경고</h4>