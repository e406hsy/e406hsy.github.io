---
layout: post
title:  "[Spring Reference] 스프링 레퍼런스 #1 핵심 - 1. IoC 컨테이너"
createdDate:   2020-05-17T18:42:00+09:00
date:   2020-06-07T14:05:00+09:00
excerpt: "한글 번역 : 스프링 레퍼런스 #1 핵심 - 1. IoC 컨테이너"
pagination: enabled
author: SoonYong Hong
categories: Spring_Reference
tags: Spring 
sitemap:
    changefreq: daily
---

이 내용은 [스프링 문서 5.2.6.RELEASE](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#spring-core)를 번역한 내용으로 오역이 있을 수 있습니다.

현재 내용 작성중입니다.

# 목차
<h2><a href="#spring-core"> 핵심 기술 </a></h2>

1. <a href="#beans"> IoC 컨테이너 </a>
    1. <a href="#beans-introduction">IoC 컨테이너와 빈의 소개</a>
    1. <a href="#beans-basics">컨테이너 개요</a>
        1. <a href="#beans-factory-metadata">메타데이터 설정하기</a>
        1. <a href="#beans-factory-instantiation">컨테이너 인스턴스 만들기</a>
            * <a href="#beans-factory-xml-import">XML 기반 설정 메타데이터 만들기</a>
            * <a href="#groovy-bean-definition-dsl">Groovy 빈 정의 DSL</a>
        1. <a href="#bean-factory-client">컨테이너 이용하기</a>
    1. <a href="#beans-definition">빈 개요</a>
        1. <a href="#beans-beanname">빈 이름 붙이기</a>
            * <a href="#beans-beanname-alias">빈 정의 외부에서 빈 별명 설정하기</a>
        1. <a href="#beans-factory-class">빈 인스턴스 만들기</a>
            * <a href="#beans-factory-class-ctor">생성자로 인스턴스 만들기</a>
            * <a href="#beans-factory-class-static-factory-method">스태틱 팩토리 메소드로 만들기</a>
            * <a href="#beans-factory-class-instance-factory-method">인스턴스 팩토리 메소드로 만들기</a>
            * <a href="#beans-factory-type-determination">빈의 런타임 타입 결정하기</a>
    1. <a href="#beans-dependencies">의존성</a>
        1. <a href="#beans-factory-collaborators">의존성 주입</a>
            * <a href="#beans-constructor-injection">생성자 기반 의존성 주입</a>
            * <a href="#beans-setter-injection">세터 기반 의존성 주입</a>
            * <a href="#beans-dependency-resolution">의존성 결정 과정</a>
            * <a href="#beans-some-examples">의존성 주입 예제</a>
        1. <a href="#beans-factory-properties-detailed">의존성과 설정 상세</a>
            * <a href="#beans-value-element">직관적인 값(Primitives, String 등등)</a>
            * <a href="#beans-ref-element">다른 빈 참조(협업)</a>
            * <a href="#beans-inner-beans">내부 빈</a>
            * <a href="#beans-collection-elements">컬렉션</a>
            * <a href="#beans-null-element">Null과 비어있는 스트링</a>
            * <a href="#beans-p-namespace">P 네임스페이스를 이용한 XML 단축</a>
            * <a href="#beans-c-namespace">c 네임스페이스를 이용한 XML 단축</a>
            * <a href="#beans-compound-property-names">복합 프로퍼티 이름</a>
        1. <a href="#beans-factory-dependson">depends-on 사용하기</a>
        1. <a href="#beans-factory-lazy-init">지연 초기화 빈</a>
        1. <a href="#beans-factory-autowire">자동연결 협력자</a>
            * <a href="#beans-autowired-exceptions">자동 연결의 한계와 단점</a>
            * <a href="#beans-factory-autowire-candidate">자동 연결에 빈 제외하기</a>
        1. <a href="#beans-factory-method-injection">메소드 주입</a>
            * <a href="#beans-factory-lookup-method-injection">Lookup 메소드 주입</a>
            * <a href="#beans-factory-arbitrary-method-replacement">임의의 메소드 대체</a>
    1. <a href="#beans-factory-scopes">빈 스코프</a>
        1. <a href="#beans-factory-scopes-singleton">싱글톤 스코프</a>
        1. <a href="#beans-factory-scopes-prototype">프로토타입 스코프</a>
        1. <a href="#beans-factory-scopes-sing-prot-interaction">프로토타입 빈 의존성을 가지는 싱글톤 빈</a>
        1. <a href="#beans-factory-scopes-other">Request, Session, Application, WebSocket 스코프</a>
            * <a href="#beans-factory-scopes-other-web-configuration">초기 웹 설정</a>
            * <a href="#beans-factory-scopes-request">Request 스코프</a>
            * <a href="#beans-factory-scopes-session">Session 스코프</a>
            * <a href="#beans-factory-scopes-application">Application 스코프</a>
            * <a href="#beans-factory-scopes-other-injection">스코프 빈 의존성</a>
        1. <a href="#beans-factory-scopes-custom">커스텀 스코프</a>
            * <a href="#beans-factory-scopes-custom-creating">커스텀 스코프 만들기</a>
            * <a href="#beans-factory-scopes-custom-using">커스텀 스코프 이용하기</a>
    1. <a href="#beans-factory-nature">빈의 특성 설정하기</a>
        1. <a href="#beans-factory-lifecycle">콜백의 생명주기</a>
            * <a href="#beans-factory-lifecycle-initializingbean">콜백 정의하기</a>
            * <a href="#beans-factory-lifecycle-disposablebean">소멸자 콜백</a>
            * <a href="#beans-factory-lifecycle-default-init-destroy-methods">기본 초기화 메소드, 파괴 메소드</a>
            * <a href="#beans-factory-lifecycle-combined-effects">생명주기 작동원리 조합하기</a>
            * <a href="#beans-factory-lifecycle-processor">Startup, Shutdown 콜백</a>
            * <a href="#beans-factory-shutdown">웹 어플리케이션이 아닌 환경에서 스프링 IoC 컨테이너 아름답게 종료하기</a>
        1. <a href="#beans-factory-aware">ApplicationContextAware, BeanNameAware</a>
        1. <a href="#awar-list">다른 Aware 인터페이스</a>
    1. <a href="#beans-child-bean-definitions">빈의 정의 상속</a>
    1. <a href="#beans-factory-extension">컨테이너 확장 시점</a>
        1. <a href="#beans-factory-extension-bpp">BeanPostProcessor를 이용하여 빈 커스터마이징 하기</a>
            * <a href="#beans-factory-extension-bpp-examples-hw">예제: Hello World, BeanPostProcessor 스타일</a>
            * <a href="#beans-factory-extension-bpp-examples-rabpp">예제: RequiredAnnotationBeanPostProcessor</a>
        1. <a href="#beans-factory-extension-factory-postprocessors">BeanFactoryPostProcessor를 이용하여 설정 메타데이터 설정하기</a>
            * <a href="#beans-factory-placeholderconfigurer">예제: 클래스 이름 대체 PropertySourcesPlaceholderConfigurer</a>
            * <a href="#beans-factory-overrideconfigurer">예제: PropertyOverrideConfigurer</a>
        1. <a href="#beans-factory-extension-factorybean">FactoryBean을 이용하여 인스턴스화 로직 커스터마이징 하기</a>
    1. <a href="#beans-annotation-config">어노테이션 기반 컨테이너 설정</a>
        1. <a href="#beans-required-annotation">@Required</a>
        1. <a href="#beans-autowired-annotation">@Autowired 사용하기</a>
        1. <a href="#beans-autowired-annotation-primary">@Primary를 이용하여 어노테이션 기반 자동 연결 미세 조정하기</a>
        1. <a href="#beans-autowired-annotation-qualifiers">Qualifers를 이용하여 어노테이션 기반 자동 연결  미세 조정하기</a>
        1. <a href="#beans-generics-as-qualifiers">제네릭을 자동연결 Qualifers로 이용하기</a>
        1. <a href="#beans-custom-autowire-configurer">CustomAutowireConfig 이용하기</a>
        1. <a href="#beans-resource-annotation">@Resource를 사용한 주입</a>
        1. <a href="#beans-value-annotations">@Value 사용하기</a>
        1. <a href="#beans-postconstruct-and-predestroy-annotations">@PostConstruct와 @PreDestory 사용하기</a>
    1. <a href="#beans-classpath-scanning">클래스패스 스캐닝과 관리되는 컴포넌트</a>
        1. <a href="#beans-stereotype-annotations">@Component와 추가적인 정형화된 어노테이션</a>
        1. <a href="#beans-meta-annotations">메타 어노테이션과 복합 어노테이션 이용하기</a>
        1. <a href="#beans-scanning-autodetection">클래스 검색과 빈 정의 등록을 자동으로 수행하기 </a>
        1. <a href="#beans-scanning-filters">필터를 이용하여 스캐닝 커스터마이징하기</a>
        1. <a href="#beans-factorybeans-annotations">컴포넌트 내부에 빈 메타데이터 정의하기</a>
        1. <a href="#beans-scanning-name-generator">자동 검색된 컴포넌트 이름 붙이기</a>
        1. <a href="#beans-scanning-scope-resolver">자동 검색된 컴포넌트에 스코프 부여하기</a>
        1. <a href="#beans-scanning-qualifiers">Qualifier 메타데이터 어노테이션으로 부여하기</a>
        1. <a href="#beans-scanning-index">후보 컴포넌트 색인 생성하기</a>
    1. <a href="#beans-standard-annotations">JSR 330 표준 어노테이션 사용하기</a>
        1. <a href="#beans-inject-named">@Inject와 @Named를 이용한 의존성 주입</a>
        1. <a href="#beans-named">@Named와 @ManagedBean: @Component 어노테이션의 표준 표현</a>
        1. <a href="#beans-stardard-annotations-limitations">JSR-330 표준 어노테이션의 한계</a>
    1. <a href="#beans-java">자바 기반 컨테이너설정</a>
        1. <a href="#beans-java-basic-concepts">기본 개념: @Bean과 @Configuration</a>
        1. <a href="#beans-java-instantiating-container">AnnotationConfigApplicationContext를 이용하여 스프링 컨테이너 인스턴스화 하기</a>
            * <a href="#beans-java-instantiating-container-constructor">간단하게 만들어보기</a>
            * <a href="#beans-java-instantiating-container-register">register(Class<?>...)을 이용하여 프로그래밍적으로 컨테이너 만들기</a>
            * <a href="#beans-java-instantiating-container-scan">scan(String...)을 이용하여 컴포넌트 스캔 활성화하기</a>
            * <a href="#beans-java-instantiating-container-web">AnnotationConfigWebApplicationContext을 이용한 웹 어플리케이션 지원</a>
        1. <a href="#beans-java-bean-annotation">@Bean 어노테이션 사용하기</a>
            * <a href="#beans-java-declaring-a-bean">빈 정의하기</a>
            * <a href="#beans-java-dependencies">빈 의존성</a>
            * <a href="#beans-java-lifecycle-callbacks">생명주기 콜백 받기</a>
            * <a href="#beans-java-specifying-bean-scope">빈 스코프 설정하기</a>
            * <a href="#beans-java-customizing-bean-naming">빈 이름 설정하기</a>
            * <a href="#beans-java-bean-aliasing">빈 별명 설정하기</a>
            * <a href="#beans-java-bean-description">빈 설명</a>
        1. <a href="#beans-java-configuration-annotation">@Configuration 어노테이션 이용하기</a>
            * <a href="#beans-java-injecting-dependencies">내부 빈 의존성 주입하기</a>
            * <a href="#beans-java-method-injection">Lookup 메소드 주입하기</a>
            * <a href="#beans-java-further-information-java-config">자바 기반 설정이 내부적으로 작동하는 방법에 대한 추가적인 정보</a>
        1. <a href="#beans-java-composing-configuration-classes">자바 기반 설정 구성하기</a>
            * <a href="#beans-java-using-import">@Import 어노테이션 사용하기</a>
            * <a href="#beans-java-conditional">조건에 따라 @Configuration 클래스와 @Bean 메소드 포함시키기</a>
            * <a href="#beans-java-combining">자바 기반 설정과 XML 기반 설정 조합하기</a>
    1. <a href="#beans-envirionment">환경 추상화</a>
        1. <a href="#beans-definition-profiles">빈 정의 프로필</a>
            * <a href="#beans-definition-profiles-java">@Profile 이용하기</a>
            * <a href="#beans-definition-profiles-xml">XML 빈 정의 프로필</a>
            * <a href="#beans-definition-profiles-enable">프로필 활성화하기</a>
            * <a href="#beans-definition-profiles-default">기본 프로필</a>
        1. <a href="#beans-property-source-abstaction">PropertySource 추상화</a>
        1. <a href="#beans-using-propertysource">@PropertySource 사용하기</a>
        1. <a href="#beans-placeholder-resolution-in-statements">표현법의 PlaceHolder Resolution</a>
    1. <a href="#context-load-time-weaver">LoadTimeWeaver 등록하기</a>
    1. <a href="#context-introduction">어플리케이션 콘텍스트의 추가적인 사용기능</a>
        1. <a href="#context-functionality-messagesource">MessageSource를 이용한 내재화</a>
        1. <a href="#context-functionality-events">표준, 커스텀 이벤트</a>
            * <a href="#context-functionality-events-annotation">어노테이션 기반 이벤트 리스너</a>
            * <a href="#context-functionality-events-async">비동기 리스너</a>
            * <a href="#context-functionality-events-order">리스너 순서 정하기</a>
            * <a href="#context-functionality-events-generics">Generic 이벤트</a>
        1. <a href="#context-functionality-resources">로우레벨 자원에 편리한 접근</a>
        1. <a href="#context-create">웹 어플리케이션을 위한 편리한 ApplicationContext 인스턴스화</a>
        1. <a href="#context-deploy-rar">스프링 ApplicationContext를 Java EE RAR 파일로 배포하기</a>
    1. <a href="#beans-beanfactory">빈 팩토리</a>
        1. <a href="#context-introduction-ctx-vs-beanfactory">BeanFactory? 혹은 ApplicationContext?</a>
1. <a href="../spring-reference-core-2-resources/#resources"> 리소스 </a>
1. <a href="../spring-reference-core-3-validation/#validation"> 검증, 데이터 바인딩, 타입 변환 </a>
1. <a href="../spring-reference-core-4-expressions/#expressions"> 스프링 표현 언어(SpEL) </a>
1. <a href="../spring-reference-core-5-aop/#aop"> 스프링과 함께하는 관점 지향 프로그래밍(AOP) </a>
1. <a href="../spring-reference-core-6-aop-api/#aop-api"> 스프링 AOP API </a>
1. <a href="../spring-reference-core-7-null-safety/#null-safety"> 안전한 Null </a>
1. <a href="../spring-reference-core-8-databuffers/#databuffers"> 데이터 버퍼와 코덱 </a>
1. <a href="../spring-reference-core-9-appendix/#appendix"> 부록 </a>

---

<h1 id="spring-core"> 핵심 기술 </h1>

레퍼런스 문서 중 이 부분은 스프링 프레임워크에 필수적인 기술을 담당한다.     
이 것들중 가장 중요한 것은 스프링 프레임워크의 역 흐름 제어(IoC)이다. 스프링 프레임워크 IoC 컨테이너의 철저한 관리는 스프링의 광범위한 관점 지향 프로그래밍(AOP) 적용 범위에 기반한다. 스프링 프레임워크는 개념적으로 이해하기 쉽고 자바 엔터프라이즈 프로그래밍에 요구조건의 핵심 80%를 성공적으로 처리하는 자체 AOP 프레임워크를 가지고 있다.     
또한 스프링은 AspectJ(현재 자바 엔터프라이즈 환경에서 가장 질 좋고 성숙한 AOP 구현)포함시켜 제공된다.

<h2 id="beans"> 1. IoC 컨테이너 </h2>

이 장은 스프링의 역 흐름 제어(IoC) 컨테이너를 담당한다.

<h3 id="beans-introduction"> 1.1. IoC 컨테이너와 빈의 소개</h3>

이 장은 스프링 프레임워크의 역흐름 제어 구현 원리를 담당한다. 역 흐름 제어(IoC)는 의존성 주입(DI)로도 알려져 있다. 이것은 객체가 생성자 어규먼트, 팩토리 메서드 어규먼트, 생성되거나 팩토리 메소드로부터 리턴된후 객체 인스턴트에 설정되는 프로퍼티만을 통해 자신의 의존성(함께 동작하는 다른 객체)을 정의하는 과정이다. IoC 컨테이너는 빈을 생성할 때, 이러한 의존성을 주입한다. 이 과정은 기본적으로 서비스 중개자 패턴(Service Locator pattern)과 같은 방법이나 객체의 직접생성 등의 방법으로 빈 스스로 자신의 의존성을 인스턴스화하는 것의 반대이다.     
`org.springframework.beans`와 `org.springframework.context` 패키지는 스프링 프레임워크 IoC 컨테이너의 기반이 된다. [`BeanFactory`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/beans/factory/BeanFactory.html)인터페이스는 어떠한 타입의 객체든 관리할 수 있는 추가적인 설정 방법을 제공한다. [`ApplicationContext`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/ApplicationContext.html)는 `BeanFactory`의 서브 인터페이스이며 아래의 것들을 추가한다.
* 스프링 AOP 기능의 손쉬운 통합
* 메세지 리소스 처리(국제화에 사용)
* 이벤트 생성
* 웹 어플리케이션에서의 `WebApplicationContext`와 같은 어플리케이션 레이어 콘텍스트

요약하면, `BeanFactory`는 프레임워크와 기본적인 기능 설정을 제공하고 `ApplicationContext`는 더 기업 특화적인 기능을 추가한다. `ApplicationContext`는 `BeanFactory`의 완전한 상위 호환이며 오로지 이 장에서 스프링 IoC 컨테이너의 설명에 사용된다. `ApplicationContext`대신 `BeanFactory`를 사용하는 방법에 대한 추가적인 정보는 [`BeanFactory`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/beans/factory/BeanFactory.html)를 보십시오.      
스프링에서 어플리케이션의 기반을 이루며 스프링 IoC 컨테이너에 의해 관리되는 객체들을 빈이라고 부른다. 빈은 스프링 IoC 컨테이너에 의하여 인스턴스화 되고 수집된다. 다른말로 하면 스프링 IoC 컨테이너에 의해 관리된다. 또 다른말로 하면, 빈은 어플리케이션에 수많은 객체중 하나이다. 빈과 그들사이의 의존성은 컨테이너에서 사용하는 설정 메타데이터에 의해 가져온다.

<h3 id="beans-basics"> 1.2. 컨테이너 개요 </h3>
`org.springframework.context.ApplicationContext`인터페이스는 스프링 IoC 컨테이너를 의미하며 빈을 인스턴스화하고 설정하며 수집하는 역할을 한다. 컨테이너를 설정 메타데이터를 읽음으로서 어떠한 객체를 인스턴스화하고 설정하고 수집할 지 수행할 명령을 가져온다. XML, 자바 어노테이션, 자바 코드의 형태로 설정 메타데이터를 표현한다. 이 메타데이터는 어플리케이션을 구성하는 객체와 객체간의 의존성을 표현할 수 있게 한다.     
스프링은 몇개의 `ApplicationContext`구현체를 제공한다. 독립형 어플리케이션에서 [`ClassPathXmlApplicationCOntext`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/support/ClassPathXmlApplicationContext.html)나 [`FileSystemXmlApplicationContext`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/support/FileSystemXmlApplicationContext.html)를 생성하는 것은 흔한 일이다. XML은 전통적으로 설정 메타데이터를 정의하는 형식으로 생각되어 왔지만 아주 약간의 XML 설정만으로 자바 어노테이션이나 코드를 메타데이터 형식으로 컨테이너에 제공할 수 있다.     
대부분의 어플리케이션 시나리오에서, 하나 혹은 그 이상의 스프링 IoC 컨테이너를 인스턴스화 하기 위해 유저의 코드는 필요없다. 예를 들어 웹 어플리케이션 시나리오에서, 어플리케이션의 `web.xml`파일의 간단한 8줄의 상용 웹 설명자 XML이면 충분하다.([웹 어플리케이션을 위한 편리한 ApplicationContext 인스턴스화 하기](#context-create)를 보십시오.) 만약 [Spring Tools for Exlipse](https://spring.io/tools)(이클립스 기반 개발 환경)을 사용한다면 몇 번의 마우스 클릭과 키보드를 누름으로서 이 상용 설정을 쉽게 만들 수 있다.     
아래의 그림은 스프링의 작동하는 방법을 추상화하여 보여준다. `ApplicationContext`가 생성되고 초기화된 뒤, 당신의 어플리케이션의 클래스들은 설정 메타데이터와 결합하여 완전히 구성되고 실행가능한 시스템이나 어플리케이션이 생겨난다.
<img src="https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/images/container-magic.png"/>
**그림 1. 스프링 IoC 컨테이너**

<h4 id="beans-factory-metadata"> 1.2.1 메타데이터 설정하기</h4>
위의 다이어그램에서 볼 수 있듯이, 스프링 IoC 컨테이너는 설정 메타데이터를 이용한다. 어플리케이션 개발자가 스프링 컨테이너에게 어플리케이션의 객체들을 인스턴스화하고 설정하고 수집하는 방법을 표현하는 것이 설정 메타데이터이다.     
전통적으로 메타데이터는 간단하고 직관적인 XML 형식으로 제공되었고 이 XML형식은 이 장에서 스프링 IoC 컨테이너의 주요한 개념과 기능을 표현하기 위해 사용된다.

| XML 기반 메타데이터는 메타데이터를 설정하는 유일한 형식이 아니다. 스프링 IoC 컨테이너 그 자체는 설정 메타데이터가 어떤 형식으로 작성되어 있는 지와 완전히 무관하다. 최근에는 많은 개발자들은 스프링 어플리케이션에서 [자바 기반 설정](#beans-java)을 선택한다. |

스프링 컨테이너 메타데이터에서 사용되는 다른 형식에 관한 정보를 보려면:

* [어노테이션 기반 설정](#beans-annotation-config): 스프링 2.5부터 어노테이션 기반 메타데이터를 지원한다.
* [자바 기반 설정](#beans-java): 스프링 3.0부터 시작된 자바설정 프로젝트에서 제공되는 많은 기능들은 스프링 프레임워크의 핵심부분이 되었다. 따라서, XML 파일 대신에 자바 클래스를 이용하여 어플리케이션 외부 빈을 정의할 수 있다. 이 기능을 사용하기 위해서 [`@Configuration`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Configuration.html), [`@Bean`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html), [`@Import`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Import.html), [`@DependsOn`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/DependsOn.html) 어노테이션을 보십시오.

스프링 설정은 컨테이너가 관리할 최소 한개의 빈 정의, 일반적으로는 여러개의 빈 정의로 구성된다. XML 기반 설정 메타데이터는 이러한 빈을 최상위 `<beans/>`요소 안에 `<bean/>` 요소로 설정한다. 자바 설정은 일반적으로 `@Configuration`클래스 내부에 `@Bean`어노테이션 메서드를 사용한다.     
이러한 빈 정의는 어플리케이션을 만드는 실제 객체와 일치한다. 일반적으로 서비스 레이어 객체, DB에 접근하는 객체(DAOs), Struts `Action`과 같은 프레젠테이션 객체, Hibernate `SessionFactories`와 같은 기반장치 객체, JMS `Queues`와 같은 것들을 정의한다. 일반적으로 잘만들어진 도메인 객체는 컨테이너에 설정하지 않는다. 왜냐하면 도메인 객체를 만들고 불러오는 것은 대개 DAO와 비즈니스 로직의 역할이기 때문인다. 그러나 스프링에 통합된 AspectJ를 이용하여 IoC 컨테이너의 통제 밖에서 생성된 객체를 설정할 수 있다. [AspectJ를 이용하여 스프링에서 도메인 객체에 의존성 주입하기](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/DependsOn.html)를 보십시오.     
아래의 예제는 기본적인 XML 기반 설정 메타데이터를 보여줍니다:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">  
        <!-- 이 빈에 사용되는 의존성과 설정이 작성되는 장소 -->
    </bean>

    <bean id="..." class="...">
        <!-- 이 빈에 사용되는 의존성과 설정이 작성되는 장소 -->
    </bean>

    <!-- 더 많은 빈 정의가 작성되는 장소 -->

</beans>
```

| 1 | `id` 어트리뷰트는 각각의 빈 정의를 식별하는 스트링이다. |
| 2 | `class` 어트리뷰트는 빈의 종류를 정의하고 완전한 클래스이름을 사용한다. |

`id` 어트리뷰트의 값은 의존하는 객체를 가리킨다. 의존하는 객체를 가리키는 XML은 이 예제에는 없다. [의존성](#beans-dependencies)에 더 자세히 나와있습니다.
<h4 id="beans-factory-instantiation"> 1.2.2. 컨테이너 인스턴스 만들기 </h4>
`ApplcationContext`생성자 에 제공된 경로는 자바 `CLASSPATH`, 로컬 파일 시스템과 같은 다양한 리소스로부터 스프링 컨테이너가 설정 메타데이터를 불러올수 있는 스트링이다.

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

```

| 스프링 IoC 컨테이너에 대하여 배운 이후, URI로 적힌 위치로부터 손쉽게 읽어오는 방법을 제공하는 스프링 `Resource` 추상화([리소스](../spring-reference-core-1-resources#resources)에 설명되어 있다.)에 대하여 알고 싶을 수도 있다. 특히 [어플리케이션 컨텍스트와 리소스 경로]에 설명되어 있는대로, 어플리케이션 컨텍스트를 만드는데 `Resource`경로를 사용한다. |

아래의 예시는 서비스 레이어 `(services.xml)`객체 설정 파일이다:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 서비스 리스트 -->

    <bean id="petStore" class="org.springframework.samples.jpetstore.services.PetStoreServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <property name="itemDao" ref="itemDao"/>
        <!-- 이 빈에 사용되는 의존성과 설정이 작성되는 장소 -->
    </bean>

    <!-- 더 많은 빈 정의가 작성되는 장소 -->

</beans>
```

아래의 예시는 DB에 접극하는 객체 `daos.xml`을 보여준다:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao"
        class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
        <!-- 이 빈에 사용되는 의존성과 설정이 작성되는 장소 -->
    </bean>

    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
        <!-- 이 빈에 사용되는 의존성과 설정이 작성되는 장소 -->
    </bean>

    <!-- 더 많은 빈 정의가 작성되는 장소 -->

</beans>
```

위의 예시에서, 서비스 레이어는 `PetStoreServiceImpl` 클래스와 (JPA 객체 관계 맵핑 표준에 기반한)`JpaAccountDao`와 `JpaItemDao` 두 종류의 DB에 접근하는 객체 두개로 구성된다. `property name`요소는 자바빈 프로퍼티의 이름을 가리키고 `ref`요소는 다른 빈 정의의 이름을 가리킨다. `id`와 `ref`요소 간의 연결은 함께 동작하는 객체들간의 의존성을 표현한다.  객체의 의존성을 설정에 대한 자세한 정보는 [의존성](#beans-dependencies)을 보십시오.

<h5 id="beans-factory-xml-import"> XML 기반 설정 메타데이터 만들기 </h5>
여러 XML 파일에 빈 정의를 나눠놓는 것은 유용할 수 있다. 종종 각각의 독립적인 XML 설정 파일이 논리적 레이어나 모듈을 표현한다.     
어플리케이션 콘텍스트의 생성자를 사용하여 이 XML 조각들에 빈 정의를 불러올 수 있다. 이 생성자는 [이전 장](#beans-factory-instantiation)에서 보여줬듯이 여러개의 `Resource`위치를 수용한다. 그 대신 `<import/>`를 이용하여 다른 파일에서 빈 정의를 불러올 수 있다. 아래에 예시가 있다:

```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

위의 예시에서 `services.xml`, `messageSource.sml`, `themeSource.xml` 세 파일로부터 외부 빈 정의를 불러온다. 모든 위치 경로는 불러오는 파일로부터 상대적이다. 따라서 `services.xml`은 불러오는 파일과 같은 디렉토리에 있거나 같은 클래스패스 위치에 있어야하며 `messageSource.xml`과 `themeSource.xml`은 불러오는 파일 하위에 `resources` 위치에 있어야한다. 이로 알수 있듯이, 맨 처음 나오는 "/"는 무시된다. 그러나 상대적인 위치라는 것을 감안하여, "/"는 사용하지 않는게 좋다. 불러와지는 파일의 내용은 최 상위 `<beans/>`를 포함하여 스프링 스키마에 따라 유효한 XML 빈 정의여야한다.

| 부모 디렉토리에 있는 파일을 "../"를 이용하여 나타내는 것은 가능하지만 추천하지 않는다. 이렇게 하면 현재 어플리케이션 외부에 파일에 의존하게된다. 특히 `classpath:`에 이러한 방법을 사용하는 것은 매우 추천하지 않는다.(예를 들어 `classpath:../services.xml`) 런타임에 "가장 가까운" 클래스패스 루트를 찾아 부모 디렉터리를 살펴보기 때문에 틀린 디렉토리를 선택하게 될 수도 있다.     
상대경로 대신 완전한 리소스 위치를 얼마든지 사용할 수 있다. 예를 들면 `file:C:/config/services.xml` 또는 `classpath:/config/services.xml`. 하지만 어플리케이션의 설정을 특정 절대경로에 묶어놓는 다는 것을 확실히 알아둬야 할 것이다. 일반적으로는 이러한 절대경로를 간접적으로 유지하는 것을 선호한다. - 예를 들어 런타임에 JVM 시스템 프로퍼티에서 처리되는 "${...}" 플레이스 홀더를 이용하는 것이다. |

네임스페이스 자체로 선언적인 기능을 제공하고 불러온다. 스프링에서 제공되는 `context`나 `util`과 같은 XML 네임스페이스를 선택하여 평범한 빈 정의를 뛰어넘는 설정도 가능하다.

<h5 id="groovy-bean-definition-dsl">Groovy 빈 정의 DSL</h5>
외부 설정 메타데이터의 더 깊은 예제로서 Grails 프레임워크로부터 알려진 Groovy 빈 정의 DSL로 빈 정의를 표현할 수 있다. 일반적으로 이러한 설정은 ".groovy" 파일이며 아래 예제와 같은 구조를 가진다:

```GROOVY
beans {
    dataSource(BasicDataSource) {
        driverClassName = "org.hsqldb.jdbcDriver"
        url = "jdbc:hsqldb:mem:grailsDB"
        username = "sa"
        password = ""
        settings = [mynew:"setting"]
    }
    sessionFactory(SessionFactory) {
        dataSource = dataSource
    }
    myService(MyService) {
        nestedBean = { AnotherBean bean ->
            dataSource = dataSource
        }
    }
}
```

이 설정 방법은 XML 빈 정의 방법과 같은 방법이며 심지어 스프링 XML 설정 네임스페이스도 지원한다. XML 빈 정의 파일을 불러오는 방법 또한 `importBeans` 명령으로 지원한다.

<h4 id="bean-factory-client">컨테이너 이용하기</h4>

`ApplicationContext`는 다양한 빈과 의존성을 목록을 유지할 수 있는 향상된 팩토리 인터페이스이다. `T getBean(String name, Class<T> requiredType)`을 이용하여 빈 인스턴스를 가져올 수 있다.     
`ApplicationContext`는 아래 예시에서 보여주듯 빈을 읽고 빈에 접근할 수 있게 한다:

```java
// 빈을 만들고 설정한다.
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// 설정된 인스턴스를 가져온다.
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// 설정된 인스턴스를 사용한다.
List<String> userList = service.getUsernameList();
```

Groovy 설정으로 진행하는 일련의 과정은 매우 흡사하다. Groovy를 알고있는( XML 빈 정의 또한 이해하는 ) 컨텍스트 클래스의 구현제를 가지고 있다. 아래의 예제가 Groovy 설정을 보여준다:

```java
ApplicationContext context = new GenericGroovyApplicationContext("services.groovy", "daos.groovy");
```

가장 유연한 형태는 읽어오기를 대신 수행하는 객체들과 조합된 `GenericApplicationContext`이다. -XML 읽어오기를 대신 수행하는 `XmlBeanDefinitionReader`를 이용한 예시:

```java
GenericApplicationContext context = new GenericApplicationContext();
new XmlBeanDefinitionReader(context).loadBeanDefinitions("services.xml", "daos.xml");
context.refresh();
```

`GroovyBeanDefinitionReader`를 이용하여 아래 예시처럼 Groovy 파일을 읽어올 수 있다:

```java
GenericApplicationContext context = new GenericApplicationContext();
new GroovyBeanDefinitionReader(context).loadBeanDefinitions("services.groovy", "daos.groovy");
context.refresh();
```

읽어오기를 대신 수행하는 객체를 짜맞춰 조합하여 한개의 `ApplicationContext`에 다양한 설정 원천으로부터 빈 정의를 읽어올 수 있다.     
그 뒤, `getBean`을 이용하여 빈 인스턴스를 가져올 수 있다. `ApplicationContext` 인터페이스는 빈을 가져오는 메소드를 몇개 가지고 있다. 하지만 이상적으로는 어플리케이션 코드가 그 메소드들을 사용하면 안된다. 사실, 어플리케이션 소드는 `getBean()`메소드를 호출하면 안되며 스프링 API에 의존성이 없어야 한다. 예를 들어, 스프링은 웹 프레임워크를 통합시켜 컨트롤러나 JSF 빈과 같은 다양한 웹 프레임워크 구성 요소에 의존성 주입을 제공한다. 자동연결 어노테이션과 같은 메타데이터로 특정 빈에 대한 의존성을 정의할 수 있게한다.

<h3 id="beans-definition">빈 개요</h3>
스프링 IoC 컨테이너는 하나 이상의 빈을 관리한다. 컨테이너에 제공한 설정 메타데이터(예를 들어 XML에서 `<bean/>` 정의)를 이용하여 이 빈들을 생성한다.     
컨테이너 내부에서 빈 정의는 아래의 메타데이터를 가지고 있는 `BeanDefinition`객체로 표현된다:
* 패키지가 포함된 클래스 이름: 일반적으로 정의된 빈의 실제 구현 클래스
* 빈이 컨테이너 내부에서 동작하는 방법을 설정하는 빈의 행동에 관한 설정 요소들 (스코프, 생명주기, 콜백 등등)
* 빈이 동작하기 위해 필요한 다른 빈에 대한 참조. 이 참조는 콜라보레이터나 의존성이라고도 불린다.
* 새로 생성된 객체에 사용하는 다른 설정 - 예를 들면, 커넥션 풀을 관리하는 빈에서 커넥션의 수, 풀의 최대 크기
이러한 메타데이터는 각각의 빈 정의를 만드는 프로퍼티로 옮겨진다. 아래 표는 그러한 프로퍼티를 설명한다.

**표 1. 빈 정의**

| 프로퍼티 | 자세한 설명 하이퍼 링크 |
| ----- | ----- |
| Class | [빈 초기화하기](#beans-factory-class) |
| Name | [빈 이름 붙이기](#beans-beanname) |
| Scope | [빈 스코프](#beans-factory-scopes) |
| Constructor arguments | [의존성 주입](beans-factory-collaborators) |
| Properties | [의존성 주입](beans-factory-collaborators) |
| Autowiring mode | [자동연결 협력자](#beans-factory-autowire) |
| Lazy initialization mode | [지연 초기화 빈](#beans-factory-lazy-init) |
| initialization method | [초기화 콜백](#beans-factory-lifecycle-initializingbean) |
| Destruction mehtod | [소멸자 콜백](#beans-factory-lifecycle-disposablebean) |

`ApplicationContext` 구현체는 빈이 어떻게 생성되야 하는지에 대한 정보를 담은 빈정의 뿐만 아니라 컨테이너 밖에서 유저에 의해 생성된 객체의 등록 또한 허용한다. 이러한 행위는 `getBeanFactory()`메소드를 통하여 ApplicationContext의 빈 팩토리에 접근하여 이루어진다. 해당 메소드는 `DefaultListableBeanFactory`를 반환한다. 이 `DefaultListableBeanFactory`는 `registerSingleton(..)`과 `registerBeanDefinition(..)` 메소드를 이용하여 빈 등록을 할 수 있다. 하지만 일반적인 어플리케이션은 보통의 빈 정의 메타데이터에서 정의된 빈으로만 동작한다.

| 빈정의와 수동으로 제공된 싱글톤 인스턴스는 가능한한 빨리 등록하어 컨테이너가 자동연결과 다른 내부 동작을 올바르게 작동할 수 있도록 해야한다. 이미 존재하는 메타데이터와 싱글톤 인스턴스를 덮어씌우는 것은 어느정도 지원하지만 새로운 빈을 런타임에 추가하는 것(팩토리에 동시접근하는 것)은 공식적으로 지원하지 않고 동시 접근 예외나 빈 컨테이너의 동시성 문제, 혹은 두 문제 모두를 발생시킬수 있다. |

<h4 id="beans-beanname">빈 이름 붙이기</h4>

모든 빈은 한개 이상의 식별자를 가지고 있다. 이러한 식별자는 해당 빈을 가지고 있는 컨테이너 내부에서는 고유해야한다. 빈은 보통은 한개의 식별자만 가진다. 하지만 여러개가 필요하다면 나머지는 별명으로 여겨진다.     
XML기반 설정 메타데이터에서 `id` 어트리뷰트, `name` 어트리뷰트, 혹은 두개 모두를 빈 식별자로 사용한다. `id` 어트리뷰트는 정확히 한개의 식별자를 가지게한다. 관례적으로 이러한 이름들은 문자와 숫자로 구성되어있다. ('myBean', 'someService' 등등) 하지만, 특수문자도 사용 가능하다. 빈에 별명을 설정하고자 한다면, `name` 어트리뷰트에 `,`, `;`, 공백으로 구분하여 작성할 수 있다. 스프링 3.1 이전에는 `id` 어트리뷰트는 문자제한이 있는`xsd:ID` 형식으로 정의되었다. 3.1부터 `xsd:string`형식으로 정의되었다. `id`의 고유성은 여전히 컨테이너에 의하여 강제된다. 하지만 더이상 XML 파서를 이용하지 않는다.     
`name`이나 `id`를 반드시 작성할 필요는 없다. `name`이나 `id`를 명시하지 않으면, 컨테이너가 고유한 이름을 만들어낸다. 하지만 `ref` 요소나 서비스 중개자 스타일을 이용하여 빈을 이름으로 참조하려면 이름을 작성해야한다. 이름을 작성하지 않는 동기는 [내부 빈](#beans-inner-beans)이나 [자동연결 협력자](#beans-factory-autowire)를 이용하기 위해서이다.

| 빈 이름 컨벤션 |
| --- |
| 빈 이름을 지을 때에 사용하는 컨벤션은 인스턴스의 필드 이름에 사용되는 표준 자바 컨벤션을 사용하는 것이다. 즉, 빈 이름은 소문자로 시작하여 camel-case를 사용한다. 이러한 이름의 예시는 `accountManager`, `accountService`, `userDao`, `loginController` 등등이 있다.     
일관성있게 이름을 지음으로써 설정을 읽기 쉽고 이해하기 쉽도록한다. 또한 스프링 AOP를 사용한다면 이름을 이용하여 어드바이스를 적용하는데 많은 도움이 된다. |

| 클래스 패스에 컴포넌트 탐색시에 스프링은 이름이 없는 컴포넌트에 이름을 생성한다. 아래의 규칙은 먼저 소개되었다: 클래스 이름을 사용한고 첫 글자를 소문자로 한다. 하지만 첫번째와 두번째글자 모두 대문자라면 원래 모습이 유지된다. 이러한 규칙은 `java.beans.Introspector.decapitalize`에 정의된다. |

<h5 id="beans-beanname-alias">빈 정의 외부에서 빈 별명 설정하기</h5>

한개의 이름을 붙일 수 있는 `id` 어트리뷰트와 여러개의 이름을 붙일 수 있는 `name` 어트리뷰트를 이용하여 빈 정의에서 여러개의 이름을 붙여줄 수 있다. 이러한 이름들은 빈에 동등한 별명이다. 특정 컨포넌트에서 고유한 이름을 사용하여도 공통의 의존성을 주입하여 줄 수 있어 유용하다.     
빈이 정의될 시기에 모든 별명을 정하는 것은 충분하지 않다. 어딘가에서 정의된 빈에 별명을 부여한는 것이 더 바람직하다. 하위 시스템이 각각의 설정을 가지고 독자적인 빈 정의를 가지고 있는 대형 시스템에 경우에 이 방법을 흔하게 사용한다. XML 기반 설정 메타데이터에서, `<alias/>` 요소를 사용하면 된다. 아래예시를 보자:
```xml
<alias name="fromName" alias="toName"/>
```
이 경우, 한 컨테이너 내부의 `fromName`이라는 빈이 이 별명 정의를 이용한 뒤부터 `toName`으로 참조될 수 있다.     
예를 들면, 하위 시스템 A에서 사용하는 설정 메타데이터는 DataSource를 `subsystemA-dataSource`라는 이름으로 참조한다. 하위 시스템 B에서 사용하는 설정 메타데이터는 DataSource를 `subsystemB-DataSource`라는 이름으로 참조한다. 주 어플리케이션에서 이 두개의 하위 시스템을 조합하여 구성할 때, 주 어플리케이서는 DataSource를 `myApp-dataSource`라는 이름으로 참조한다. 아래의 별명 정의를 설정 메타데이터에 추가하여 이 세가지 이름이 같은 객체를 참조하도록 할 수 있다:
```xml
<alias name="myApp-dataSource" alias="subsystemA-dataSource"/>
<alias name="myApp-dataSource" alias="subsystemB-dataSource"/>
```
이제 각각의 컴포넌트와 주 어플리케이션은 dataSource를 고유하면서 다른 정의에 의하여 충돌하지 않는 이름으로 참조할 수 있으며 동시에 같은 빈을 참조한다.

| 자바 기반 설정 |
| ------ |
| 만약 자바 기반 설정을 이용한다면 `@Bean` 어노테이션을 사용하여 별명을 정의할 수 있다. [`@Bean` 어노테이션 이용하기](#beans-java-bean-annotation)에 자세한 내용을 볼 수 있다. |

<h4 id="beans-factory-class">빈 인스턴스 만들기</h4>

빈 정의는 하나 혹은 여러개의 객체를 만드는데 필수적인 제작법이다. 컨테이너가 실제 객체를 만들거나 획득할 때, 빈 정으로 추상화된 설정 메타데이터를 요청하여 사용한다.     
만약 XML기반 설정 메타데이터를 사용한다면 인스턴스화될 객체의 타입(이나 클래스)를 `<bean/>`요소에 `class`어트리뷰트에 명시할 수 있다. 이 `class`어트리뷰트 (내부적으로 `BeanDefinition` 인스턴스의 `Class` 프로퍼티)는 보통 필수이다. (예외를 알고 싶다면 [인스턴스 팩토리 메서드를 이용하여 인스턴스화 하기](#beans-factory-class-instance-factory-method)와 [빈 정의 상속](#beans-child-bean-definitions)]를 보십시오) `Class` 프로퍼티를 두 방법으로 사용할 수 있다:

* 일반적으로 자바 코드의 `new`에 해당하는 생성자를 컨테이너에서 직접 호출해 생성하는 경우에 사용한다.
* 객체를 생성하기 위해 실행해야하는 `static` 팩토리 메소드를 포함하는 클래스를 명시하는 경우에 사용한다. 비교적 드믄 경우에 컨테이너는 `static` 팩토리 메소드를 호출해 빈을 생성한다. `static` 팩토리 메소드를 호출해 반환되는 객체의 타입은 같은 클래스일 수도 있고 다를 수도 있다.

| 내부 클래스 이름 |
| --- |
| 만약 `static` 중첩 클래스를 위한 빈 정의를 설정하고자 한다면, 반드시 중첩클래스의 이진이름을 사용해야한다. 예를 들어, `SomeThing`이라는 클래스가 `com.example`패키지에 있다고 하자. 이 `SomeThing` 클래스가 `static` 중첩 클래스 `OtherThing`을 가지고 있다면 `Class` 어트리뷰트의 값은 `com.example.SomeThing$OtherThing`이 될 것이다. 이름안에 `$`가 중첩 클래스와 외부 클래스를 구분해주는 문자라는 것을 주의해라. |

<h5 id="beans-factory-class-ctor">생성자로 인스턴스 만들기</h5>
생성자로 빈을 만들경우, 모든 보통의 클래스들은 스프링에서 사용가능하며 스프링과 호환된다. 이 말은 특정한 인터페이스를 구현하거나 특정한 방법으로 코드를 작성할 필요가 없다는 말이다. 단순히 빈의 클래스를 명시하는 것이면 충분하다. 하지만 어떠한 유형의 IoC 컨테이너를 사용하느냐에 따라 기본 생성자(어규먼트가 없는 생성자)가 필요할 수도 있다.     
스프링 IoC 컨테이너는 사실상 어떠한 클래스도 관리 가능하다. 진짜 자바빈만 관리할 수 있는 것이 아니다. 대부분의 스프링 사용자들은 기본 생성자만 있고 적합한 세터와 게터를 가지고 있는 진짜 자바빈을 선호한다. 자바빈 스타일 이외의 이색적인 스타일의 클래스도 사용가능하다. 예를들어 만약에 자바빈 스펙과 전혀 관련없는 레가시 커넥션풀을 사용할 필요가 있다면 스프링이 관리해 줄 수 있다.     
XML기반 설정 메타데이터에서 빈의 클래스를 아래와 같이 명시할 수 있다:
```xml
<bean id="exampleBean" class="examples.ExampleBean"/>

<bean name="anotherExample" class="examples.ExampleBeanTwo"/>
```
생성자에 어규먼트를 제공하는 방법이나 오브젝트가 생성된뒤 객체 인스턴스 프로퍼티를 설정하는 방법에 대한 자세한 정보는 [의존성 주입](#beans-factory-collaborators)에서 볼 수 있다.

<h5 id="beans-factory-class-static-factory-method">스태틱 팩토리 메소드로 만들기</h5>

스태틱 팩토리 메소드로 빈을 만들 경우, `class`어트리뷰트를 이용해 `static`팩토리 메소드를 가진 클래스를 명시할수 있고 `factory-method`어트리뷰트를 이용해 팩토리 메소드 이름을 명시할 수 있다. 이 메소드를 (나중에 설명할 선택적인 어규먼트와 함께) 호출해서 생성자로 호춯한 것처럼 다뤄질 살아있는 객체를 획득할 수 있어야한다. 이러한 빈 정의를 사용하는 한가지 방법은 레가시 코드의 `static`팩토리를 호출하는 것이다.     
아래의 빈 정의는 팩토리 메소드를 호출하여 빈이 생성된다고 명시한다. 이 빈 정의는 반환되는 객체의 종류를 명시하지 않고 팩토리 메서드를 가지고 있는 클래스만 명시한다. 이 예제에서 `createInstance()` 메소드는 반드시 스태틱 메소드여야한다. 아래의 예시는 팩토리 메소드를 명시하는 법을 보여준다:
```xml
<bean id="clientService"
    class="examples.ClientService"
    factory-method="createInstance"/>
```
아래의 예시는 위의 빈정의에 나오는 클래스를 보여준다:
```java
public class ClientService {
    private static ClientService clientService = new ClientService();
    private ClientService() {}

    public static ClientService createInstance() {
        return clientService;
    }
}
```
팩토리 메서드에 (선택적인)어규먼트를 제공하는 방법이나 팩토리에서 반환된 객체에 객체 인스턴스 프로퍼티를 설정하는 방법은 [의존성과 자세한 설정](#beans-factory-properties-detailed)에서 볼 수 있다.
<h5 id="beans-factory-class-instance-factory-method">인스턴스 팩토리 메소드로 만들기</h5>

[스태틱 팩토리 메소드](#beans-factory-class-static-factory-method)와 비슷하게 인스턴스 팩토리로 인스턴스화 하는 방법은 컨테이너에 이미 존재하는 빈의 스태틱이 아닌 메소드를 호출하여 새 빈을 만드는 것이다. 이 방법을 사용하기 위해, `class`어트리뷰트를 비워두고 `factory-bean`어트리뷰트에 현재(혹은 부모나 조상) 컨테이너에 있으며 객체를 만드는 인스턴스 메소드를 가지고 있는 빈의 이름을 명시하면 된다. 팩토리 메소드의 이름은 `factory-method`어트리뷰트에 설정하면 된다. 아래의 예시는 이러한 빈을 설정하는 것을 보여준다:
```xml
<!-- createClientServiceInstance()라는 메소드를 가지고 있는 팩토리 빈 -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- 이 빈에 필요한 의존성을 주입 -->
</bean>

<!-- 팩토리 빈에 의해 생성되는 빈 -->
<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>
```
아래의 예시는 상응하는 클래스를 보여준다:
```java
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }
}
```
하나의 팩토리 클래스는 여러개의 팩토리 메소드를 가지고 있을 수 있다. 아래에 예시이다:
```xml
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- 이 빈에 필요한 의존성 주입 -->
</bean>

<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>

<bean id="accountService"
    factory-bean="serviceLocator"
    factory-method="createAccountServiceInstance"/>
```
아래의 예시는 상응하는 클래스를 보여준다:
```java
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    private static AccountService accountService = new AccountServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }

    public AccountService createAccountServiceInstance() {
        return accountService;
    }
}
```
이 접근법은 팩토리 빈 자체도 의존성 주입(DI)에 의하여 설정되고 관리된다는 것을 보여준다.[의존성과 자세한 설정](#beans-factory-properties-detailed)을 보십시오.

| 스프링 문서에서 "팩토리 빈(factory bean)"은 스프링 컨테이너에 의하여 설정되며 [인스턴스](#beans-factory-class-instance-factory-method) 혹은 [스태틱](#beans-factory-class-static-factory-method) 팩토리 메소드로 객체를 생성하는 빈을 말한다. 반면에 `FactoryBean`(대문자임을 주목하십시오) `스프링 고유의 `FactoryBean` 구현체 클래스를 의미한다. |

<h5 id="beans-factory-type-determination">빈의 런타임 타입 결정하기</h5>

빈의 런타임 타입을 결정하는 것은 쉽지 않다. 빈 메타데이터 정의에서 명시된 클래스는 그저 초기의 클래스 참조일 뿐이며 잠재적으로 팩토리 메서드와 결합하거나 `FactoryBean` 클래스가 되어 다른 런타임타입의 빈이 되거나 인스턴스 팩토리 메소드의 경우 클래스가 설정되지 않을 수도 있다. 게다가, AOP 프록시는 빈 인스턴스를 인터페이스 기반 프록시(인터페이스의 구현체)로 감싸 빈의 실제 타입을 외부에 노출시키지 않을 수도 있다.     
빈의 실제 런타임 타입을 확인하는 방법은 `BeanFactory.getType`을 호출하는 것을 추천한다. 이 메소드는 위에 모든 경우를 계산해 두었으며 `BeanFactory.getBean`을 호출하면 반환하는 객체의 타입을 반환해 준다.

<h3 id="beans-dependencies">의존성</h3>
일반적인 엔터프라이즈 어플리케이션은 한개의 객체(스프링 용어로는 한개의 빈)로 만들어지지 않는다. 심지어 가장 간단한 어플리케이션 조차 유저가 하나로 보는 화면을 만들기 위해 여러 객체가 함께 동작한다. 독립적인 빈들을 정의하는 것부터 객체들이 목적을 달성하기 위해 협력하는 어플리케이션을 만드는 법까지 아래에서 설명할 것이다.

<h4 id="beans-factory-collaborators">의존성 주입</h4>

의존성 주입은 객체가 생성자 어규먼트나 팩토리메서드 어규먼트 혹은 객체가 생성된 후 설정되는 프로퍼티만 이용하여 자신의 의존성(자신과 함께 동작하는 다른 객체)을 정의하는 과정이다. 그 뒤, 컨테이너가 빈을 만들 때, 이러한 의존성을 주입한다. 이 과정은 기본적으로 빈이 생성자나 서비스 중개자 패턴을 이용해 자신의 의존성을 스스로 관리하는 것의 반대이다.     
DI 원리를 이용하면 코드가 더 깨끗해진다. 객체가 의존성을 제공받을 때, 결합도는 더 낮아진다. 객체는 자신의 의존성을 살펴보지 않고 의존성의 클래스나 위치를 알지 못한다. 결과적으로 클래스를 테스트하기 쉬워진다. 특히 인터페이스나 추상클래스에 의존하여 목 객체가 단위 테스트에 사용되는 경우에 그렇다.     
DI는 두 종류가 있다: [생성자 기반 의존성 주입](#beans-constructor-injection)과 [세터 기반 의존성 주입](#beans-setter-injection)

<h5 id="beans-constructor-injection">생성자 기반 의존성 주입</h5>

컨테이너가 의존성을 표현하는 생성자의 어규먼트들을 실행히켜 생성자 기반 의존성 주입이 이뤄진다. 특정된 어규먼트를 이용해 `static` 팩토리 메소드를 호출하는 것과 거의 동일하다. 이 논의에서 생성자의 어규먼트와 `static`팩토리 메서드의 어규먼트를 비슷하게 본다. 아래의 예시는 생성자 주입으로 의존성 주입이 되는 예시이다:
```java
public class SimpleMovieLister {

    // SimpleMovieLister는 MovieFinder에 의존한다.
    private MovieFinder movieFinder;

    // 스프링 컨테이너가 MovieFinder를 주입할 수 있는 생성자
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // 주입된 MovieFinder를 이용하는 비즈니스 로직은 생략되었다.
}
```
이 클래스에 특별한 것은 없다는 것을 알아차려야한다. 특정한 어노테이션, 클래스, 인터페이스에 의존하지 않는 POJO(Plain Old Java Object)이다.

###### 생성자 어규먼트 분석하기
생성자 어규먼트는 어규먼트의 타입을 사용해서 매칭한다. 빈 정의에 잠재적인 모호함이 전혀 없다면 빈 정의에서 생성자 어규먼트들의 순서가 빈의 생성될 때 생성자에 주어지는 어규먼트의 순서이다. 아래 클래스의 경우를 생각해보자:
```java
package x.y;

public class ThingOne {

    public ThingOne(ThingTwo thingTwo, ThingThree thingThree) {
        // ...
    }
}
```
`ThingTwo`와 `ThingThree` 클래스가 상속관계가 아니라 가정하면, 잠재적인 모호함이 없다. 따라서 아래의 설정은 잘 동작하며 `<constructor-arg/>`에 인덱스나 타입을 명시적으로 표시할 필요가 없다.
```xml
<beans>
    <bean id="beanOne" class="x.y.ThingOne">
        <constructor-arg ref="beanTwo"/>
        <constructor-arg ref="beanThree"/>
    </bean>

    <bean id="beanTwo" class="x.y.ThingTwo"/>

    <bean id="beanThree" class="x.y.ThingThree"/>
</beans>
```
빈이 참조될 때, 타입이 이미 알려져 있다면 매칭이 일어날 수 있다(위의 예시가 그러한 경우다). `<value>true</value>`와 같은 단순한 타입이 사용될때, 스프링이 값의 타입을 결정할 수 없다. 따라서 도움없이 타입을 매칭할 수 없다. 아래의 예시를 생각해보자:
```java
package examples;

public class ExampleBean {

    // 궁극적 해답을 계산하기 위해 필요한 년수
    private int years;

    // 삶, 대학, 모든 것들에 대한 해답
    private String ultimateAnswer;

    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

###### 생성자 어규먼트 타입 매칭하기
이전의 시나리오에서, `type`어트리뷰트를 사용해서 생성사 어규먼트의 타입을 명시한다면 컨테이너는 간단한 타입에서도 타입 매칭을 사용 가능하다. 아래의 예시를 보자:
```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```

###### 생성자 어규먼트 인덱스
아래의 예시가 보여주듯이, `index`어트리뷰트를 사용해서 생성자 어규먼트의 인덱스를 명시적으로 설정해줄 수 있다:
```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```
```
인덱스는 0부터 시작한다.
```

###### 생성자 어규먼트 이름
아래의 예시가 보여주듯이 모호함을 없애기 위해 생성자 파라메터의 이름을 사용할 수 있다:
```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```
스프링에서 생서자로부터 파라메터 이름을 볼 수 있도록 디버그 플래그가 허용된 상태로 컴파일되어야 한다는 것을 명심해라. 만약 디버그 플래그를 허용하고 싶지 않다면 [@ConstructorProperties](https://download.oracle.com/javase/8/docs/api/java/beans/ConstructorProperties.html) JDK 어노테이션을 이용하여 명시적으로 생성자 어규먼트에 이름을 지어줄 수 있다. 예시 클래슨느 아래와 같이 생겻다:
```java
package examples;

public class ExampleBean {

    // 필드 생략

    @ConstructorProperties({"years", "ultimateAnswer"})
    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

<h5 id="beans-setter-injection">세터 기반 의존성 주입</h5>

세터 기반 의존성 주입은 어규먼트가 없는 생성자나 어규먼트가 없는 `static` 팩토리 메서드를 호출한 뒤, 세터 메서드를 호출하여 이루어진다.     
아래의 예시가 세터 기반 의존성 주입이 되는 예시이다. 이 클래스는 전통적인 자바 클래스이다. 특정 인터페이스, 클래스, 어노테이션에 의존하지 않는 POJO이다.
```java
public class SimpleMovieLister {

    // SimpleMovieLister는 MovieFinder에 의존한다.
    private MovieFinder movieFinder;

    // 스프링 컨테이너가 MovieFinder를 주입할 수 있는 세더
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // 주입된 MovieFinder를 이용하는 비즈니스 로직은 생략되었다.
}
```
`ApplicationContext`는 자신이 관리하는 빈에 생성자 기반 의존성 주입과 세터 기반 의존성 주입 모두 지원한다. 생성자 기반 의존성 주입으로 일부 의존성이 해결된 후 세터로 의존성 주입하는 것도 지원한다. `BeanDefiniton`의 형태로 의존성을 설정하고 이 `BeanDefinition`은 `PropertyEditor`인스턴스를 이용하여 프로퍼티의 형식을 변환한다. 하지만 대부분의 스프링 사용자들은 이 클래스들을 직접적으로(프로그래밍적으로) 사용하지 않고 XML `bean` 정의나 어노테이션이 설정된 컴포넌트(`@Component`,`@Controller` 등등의 어노테이션이 달린 클래스)나 자바 기반 `@Configuration`클래스의 `@Bean` 메소드를 사용한다. 이러한 것들은 내부적으로 `BeanDefiniton`인스턴스로 변환되고 스프링 IoC 컨테이너 인스턴스 전체를 로드하는데 사용된다.

| 생성자 기반? 혹은 세터 기반? |
| ----- |
| 생성자 기반 의존성 주입과 세터 기반 의존성 주입을 섞을 수 있기 때문에 필수적인 의존성을 생성자에 선택적인 의존성을 세터나 설정 메서드로 사용하는 것은 좋다. [@Required](#beans-required-annotation)를 세터 메서드에 사용하여 필수적인 의존성으로 만들 수 있다. 하지만 프로그래밍적 어규먼트 검증이 포함된 생성자 기반 주입이 더 좋다. |
| 스프링 팀은 객체를 불변으로 만들며 필요한 의존성이 `null`이 아님을 확실히 할 수 있기에 생성자 기반 주입이 좋다고 말한다. 게다가 생성자로 주입된 컴포넌트들은 항상 완전히 초기화 된 후 클라이언트 (호출한) 코드에 반환된다. 별도로, 많은 생성자 어규먼트들은 클래스가 많은 책임을 가지고 있고 관심사 분리를 통해 리팩터링될 수 있는 나쁜 코드임을 의미한다. |
| 세터 기반 주입은 합리적인 기본값을 가지고 있는 선택적인 의존성에만 사용되어야 한다. 그렇지 않으면 의존성을 이용하는 곳마다 널값 확인이 이뤄줘야할 것이다. 세터기반 주입의 한 장점은 세터 메서드가 나중에 재 주입되거나 재 설정될 수 있는 클래스를 만들 수 있다는 것이다. [JMX MBeans](../spring-reference-integration-4-jmx/#jmx)을 통한 관리는 세터기반 주입을 강제하는 사용 예시이다. |

<h5 id="beans-dependency-resolution">의존성 결정 과정</h5>

컨테이너는 의존성 결정을 아래와 같이 한다:
* `ApplicationContext`를 생성하고 빈을 설명하는 설정 메타데이터를 이용하여 초기화한다. XML, 자바 코드, 자바 어노테이션으로 설정 메타데이터를 명시할 수 있다.
* 프로퍼티, 생성자 어규먼트, (일반적인 생성자 대신 사용한다면) 정적 팩토리 메서드 어규먼트의 형태로 각각의 빈마다 의존성을 표현한다. 빈이 생성될 때, 빈들에 이러한 의존성이 제공된다.
* 각각의 프로퍼티나 생성자 어규먼트는 설정할 값이거나 컨테이너가 가지고 있는 다른 빈에 대한 참조이다.
* 값인 프로퍼티나 생성자 어규먼트는 고유한 형식에서 프로퍼티나 생성자 어규먼트의 실제 타입으로 변경된다. 기본적으로, 스프링은 스트링 형식으로 주어진 값을 `int`, `long`, `String`, `boolean`과 같은 기본 형식으로 변경할 수 있다.

컨테이너가 생성될 때, 컨테이너는 각각의 빈 설정을 검증한다. 하지만 빈 프로퍼티는 실제 빈이 생성되기 전까지 설정되지 않는다. 싱글톤이면서 선 인스턴스화 설정(이 설정은 기본 설정이다)된 빈은 컨테이너가 생성될 때 만들어진다. 스코프는 [빈 스코프](#beans-factory-scopes)에 정의되 있다. 그렇지 않은 경우에는 빈이 요청될 때, 빈이 생성된다. 빈 생성은 잠재적으로 빈들로 구성된 그래프를 생성할 가능성이 있다. 빈의 의존성과 의존성의 의존성, 의존성의 의존성의 의존성, ... 이 생성되고 할당된다. 의존성간의 분석 불일치는 늦게( 영향을 받는 첫번째 빈의 생성시) 나타날 수 있다는 것에 주의하십시오.

| 순환 참조 |
| ----- |
| 주로 생성자 기반 주입을 사용한다면 해결할 수 없는 순환참조를 만드는 것도 가능하다.     
예를 들면: 클래스 A가 클래스 B의 인스턴스를 생성자 주입으로 필요하고 클래스 B가 클래스 A의 인스턴스를 생성자 주입으로 필요로 한다. 이 두 클래스가 서로 주입되도록 설정한다면 스프링 IoC 컨테이너는 이 순환 참조를 런타임에 발견하고 `BeanCurrentlyInCreationException`을 던질 것이다. |
| 한가지 해결책은 클래스의 소스코드를 고쳐 생성자 대신 세터로 설정되도록 하는 것이다. 생성자 주입을 피하고 세터 주입만 사용하는 것이다. 다시 말하면, (비록 추천하지는 않지만) 순환 참조를 세터 기반 주입으로 설정할 수 있다는 것이다.     
순환참조가 없는 일반적인 경우와는 다르게 두 빈 사이의 순환 참조는 한개의 빈이 완전히 초기화 되기 전에 다른 빈에 주입되도록 강요된다. ( 닭이 먼저냐? 달걀이 먼저냐? ) |

일반적으로 스프링이 잘 해결해 준다고 믿어도 된다. 스프링이 순환참조나 존재하지 않는 빈에대한 참조와 같은 설정 문제는 런타임에 발견해 준다. 빈이 생성될 때, 스프링은 가능한한 늦게 프로퍼티 설정과 의존성 해결을 수행한다. 즉, 정상적으로 로드된 스프링 컨테이너가 빈이나 의존성을 만드는데 문제가 발생되는 객체를 요청받았을 때가 되서야 예외를 생성할 수 있다는 말이다. 예를 들어 프로퍼티가 없거나 올바르지 못한 프로퍼티를 가진 빈을 요청받을 때 예외를 발생시킨다는 것이다. 설정 문제가 눈에 보이는 것이 늦어질수 있다는 것이고 이것이 `ApplicationContext`가 기본적으로 싱글톤 빈을 미리 생성하는 이유이다. 초기 구동시간과 아직 필요없는 빈을 만드는데 사용되는 메모리를 소모해서 `ApplicationContext`가 생성될 시 설정 문제를 발견할 수 있는 것이다. 싱글톤 빈을 미리 생성하는 대신 필요할 때 생성하도록 설정을 바꿀 수 있다. 순환 참조가 없을 시, 빈의 의존성들이 주입될 때, 이 의존성을 완전히 구성된 뒤 빈에 주입된다. 이 말은, 빈 A가 빈 B에 의존할 때, 스프링 Ioc 컨테이너는 빈 B를 먼저 생성하고 빈 A의 세터 메서드를 호출한다. 다시 말해, 미리 생성된 싱글톤 빈이 아니라면 빈의 의존성이 먼저 생성되고 관련된 생명주기 메서드들([초기화 메서드 설정하기](#beans-factory-lifecycle-initializingbean)나 [콜백 정의하기](#beans-factory-lifecycle-initializingbean))이 호출된 뒤 빈 인스턴스가 만들어진다.

<h5 id="beans-some-examples">의존성 주입 예제</h5>

아래는 세터기반 의존성주입에 XML 기반 설정 메타데이터를 사용하는 예제이다. XML 설정 파일은 빈 정의를 아래와 같이 명시한다:
```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- ref 요소를 이용한 세터 기반 주입 -->
    <property name="beanOne">
        <ref bean="anotherExampleBean"/>
    </property>

    <!-- ref 어트리뷰트를 이용한 세터 기반 주입 -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```
아래의 예시는 위에 나온 `ExampleBean`이다:
```java
public class ExampleBean {

    private AnotherBean beanOne;

    private YetAnotherBean beanTwo;

    private int i;

    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }

    public void setBeanTwo(YetAnotherBean beanTwo) {
        this.beanTwo = beanTwo;
    }

    public void setIntegerProperty(int i) {
        this.i = i;
    }
}
```
위의 예시에서 세터가 사용되어 프로퍼티를 명시하였다. 아래는 생성자 기반 의존성 주입의 예시이다:
```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- ref 요소를 이용한 생성자 기반 주입 -->
    <constructor-arg>
        <ref bean="anotherExampleBean"/>
    </constructor-arg>

    <!-- ref 어트리뷰트를 이용한 생성자 기반 주입 -->
    <constructor-arg ref="yetAnotherBean"/>

    <constructor-arg type="int" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```
아래의 예시는 위에 나온 `ExampleBean`이다:
```java
public class ExampleBean {

    private AnotherBean beanOne;

    private YetAnotherBean beanTwo;

    private int i;

    public ExampleBean(
        AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {
        this.beanOne = anotherBean;
        this.beanTwo = yetAnotherBean;
        this.i = i;
    }
}
```
`ExampleBean`의 생성자에 사용되는 어규먼트들은 빈 정의에 명시되있는 어규먼트이다.    
이 예제의 변형을 보자. 생성자 대신 `static` 팩토리 메소드를 사용하는 예시이다:
```xml
<bean id="exampleBean" class="examples.ExampleBean" factory-method="createInstance">
    <constructor-arg ref="anotherExampleBean"/>
    <constructor-arg ref="yetAnotherBean"/>
    <constructor-arg value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```
아래의 예시는 위에 나온 `ExampleBean`이다:
```java
public class ExampleBean {

    // 프라이빗 생성자
    private ExampleBean(...) {
        ...
    }

    // 정적 팩토리 메소드; 이 메서드의 어규먼트들은 해당 빈의
    // 의존성으로 여겨진다. 실제 어규먼트들이 어떻게 사용되는지와는
    // 무관하게 의존성으로 여겨진다.
    public static ExampleBean createInstance (
        AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {

        ExampleBean eb = new ExampleBean (...);
        // 추가적인 동작
        return eb;
    }
}
```
`<constructor-arg/>` 요소들은 `static` 팩토리 메서드에 어규먼트로 제공되며 생성자가 실제로 사용하는 것과 같은 요소이다. `static` 팩토리 메서드를 가지고 있는 클래스와 팩토리 메서드에 의해 반환되는 클래스는 반드시 같은 타입일 필요는 없다(이 예제에서는 같지만). 인스턴스 (정적이 아닌) 팩토리 메서드를 근본적으로는 같은 방식(`class` 어트리뷰트 대신 `factory-bean` 어트리뷰트를 사용한다)으로 사용할 수 있다. 이곳에서는 다루지 않겠다.

<h4 id="beans-factory-properties-detailed">의존성과 설정 상세</h4>

[이전 장](#beans-factory-collaborators)에서 빈 설정과 생성자 어규먼트를 다른 빈이나 값으로 정의하는 법을 배웠다. 스프링 XML 기반 설정 메타데이터는 `<property/>`요소나 `<constructor-arg/>` 요소에 방금 말한 목적으로 하위 요소를 제공한다.

<h5 id="beans-value-element">직관적인 값(Primitives, String 등등)</h5>

`<property/>`요소의 `value`어트리뷰트는 프로퍼티나 생성자 어규먼트로써 사람이 읽을 수 있는 문장표현을 명시한다. 스프링의 [변환 서비스](../spring-reference-core-3-validation/#core-convert-ConversionService-API)를 이용하여 이 값을 `String`에서 실제 프로퍼티나 어규먼트의 타입으로 변환하여준다. 아래는 다양한 값을 설정하는 예시이다:
```xml
<bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <!-- setDriverClassName(String) 이 호출한 것과 같은 결과-->
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mydb"/>
    <property name="username" value="root"/>
    <property name="password" value="masterkaoli"/>
</bean>
```
아래의 예시는 더 간결한 [P 네임스페이스](#beans-p-namespace)를 사용해 더 간결한 xml 설정을 보여준다:
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource"
        destroy-method="close"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mydb"
        p:username="root"
        p:password="masterkaoli"/>

</beans>
```
위의 예제가 더 간결하다. 하지만 빈 정의를 만들때 자동 프로퍼티 완정을 지원하는 IDE([Intellij IDEA](https://www.jetbrains.com/idea/)나 [Spring Tools for Eclipse](https://spring.io/tools))를 사용하지 않는다면 런타임에 오자를 발견하게 될것이다. 이런 IDE를 사용하기를 추천한다.     
`java.util.Properties`인스턴스를 아래 예시처럼 설정할 수 있다:
```xml
<bean id="mappings"
    class="org.springframework.context.support.PropertySourcesPlaceholderConfigurer">

    <!-- java.util.Properties로 작성되었다. -->
    <property name="properties">
        <value>
            jdbc.driver.className=com.mysql.jdbc.Driver
            jdbc.url=jdbc:mysql://localhost:3306/mydb
        </value>
    </property>
</bean>
```
스프링 컨테이너는 JavaBeans `PropertyEditor` 방식을 사용해서 `<value/>`요소 안의 문자를 `java.util.Properties` 인스턴스로 변환한다. 위 방법은 좋은 지름길이며 스프링 팀이 `value`어트리뷰트보다 `<value/>`요소를 선호하는 몇 안되는 사용법이기도 하다.

###### `idref` 요소
`idref`요소는 `<constructor-arg/>`나 `<property/>`에 컨테이너가 가지고 있는 다른 빈의 `id`를 넘기는 간단하고 오류로부터 안전한 방법이다. 아래 사용법 예시이다:
```xml
<bean id="theTargetBean" class="..."/>

<bean id="theClientBean" class="...">
    <property name="targetName">
        <idref bean="theTargetBean"/>
    </property>
</bean>
```
위의 빈정의는 아래와 (런타임시에) 같다:
```xml
<bean id="theTargetBean" class="..." />

<bean id="client" class="...">
    <property name="targetName" value="theTargetBean"/>
</bean>
```
첫 번째 형식이 두번 째 형식보다 선호된다. 왜냐하면 `idref` 태그는 컨테이너가 배포시에 참조된 빈이 실제 존재하는 지 검증할 수 있도록 해주기 때문이다. 두 번째 형식에서는 `client` 빈에 `targetName`태그에 전달된 값에 검증이 실행되지 않는다. `client`빈이 실제 인스턴스화 될 때, (대부분 매우 안좋은 결과와 함께) 오자가 확인된다. 만약 `client`빈이 [프로토타입](#beans-factory-scopes)빈이라면 오자와 그로인한 오류가 컨테이너가 배포된지 매우 오랜 시간이 지나서 발견될 것이다.

| `idref`요소의 `local` 어트리뷰트는 4.0 beans XSD부터 지원되지 않는다. 왜냐하며 `local`어트리뷰트는 더 이상 일반적인 `bean` 참조보다 추가적인 값을 제공하지 않기 때문이다. 4.0 스키마로 업그레이드 할 시, `ifref local`참조를 `idref bean`참조로 변경하십시오. |

`<idref/>`요소는 (스프링 2.0보다 이전 버전에서) `ProxyFactoryBean` 빈 정의의 [AOP 인터셉터](../spring-reference-core-6-aop-api/#aop-pfb-1) 설정에서 많이 사용된다. 인터셉터 이름을 명시하는데 `<idref/>`요소를 사용하면 인터셉터 ID를 잘못 적는 실수를 방지할 수 있다.  

<h5 id="beans-ref-element">다른 빈 참조(협업)</h5>

`<constructor-arg/>`와 `<property/>` 정의 요소 내부의 마지막 요소는 `ref`요소이다. 컨테이너에 의해 관리되는 빈을 다른 빈의 프로퍼티로 참조되도록 설정할 수 있다. 참조된 빈은 프로퍼티로 설정된 빈의 의존성이며 프로퍼티가 설정되기 전에 먼저 초기화 될 것이다. (만약 참조된 빈이 싱글톤이라면 이미 컨테이너에 의하여 초기화 되어 있을 것이다.) 모든 참조는 궁극적으로 다른 객체에 대한 참조이다. `bean`이나 `parent`어트리뷰트르 다른 객체의 이름이나 ID를 명시함으로써 스코프 설정과 검증이 가능해진다.     
`<ref/>` 태그에 `bean`어트리뷰트를 이용하여 빈을 명시하는 것이 가장 일반적인 형태이며 같은 컨테이너나 부모 컨테이너에 있는 빈(같은 XML파일에 있는 지 여부는 무관하다.)에 대한 참조를 만들어준다. `bean`어트리뷰트의 값은 참조되는 빈의 `id`어트리뷰트나 `name`어트리뷰트의 값중 하나와 같을 것이다. 아래 `ref`요소의 사용 예시이다:
```xml
<ref bean="someBean"/>
```
`parent`어트리뷰트로 참조하는 빈을 명시하는 것은 현재 컨테이너의 부모 컨테이너에 있는 빈을 참조한다. `parent` 어트리뷰트에 값은 참조되는 빈의 `id`어트리뷰트나 `name`어트리뷰트의 값중 하나와 같을 것이다. 참조되는 빈은 반드시 현재 컨테이너의 부모 컨테이너에 있어야 한다. 이 빈 참조를 컨테이너 계층에서 부모 컨테이너에 있는 빈을 감싸 같은 이름의 프록시 빈을 만들고자 할 때 주로 사용할 것이다. 아래 두개의 예시는 `parent`어트리뷰트의 사용 예시이다:
```xml
<!-- 부모 컨텍스트에서 -->
<bean id="accountService" class="com.something.SimpleAccountService">
    <!-- 필요한 의존성을 여기에 넣는다. -->
</bean>
```
```xml
<!-- 자식(자손) 컨텍스트에서 -->
<bean id="accountService" <!-- bean name is the same as the parent bean -->
    class="org.springframework.aop.framework.ProxyFactoryBean">
    <property name="target">
        <ref parent="accountService"/> <!-- 어떤 식으로 부모 빈을 참조했는지 주목하십시오 -->
    </property>
    <!-- 다른 설정과 의존성을 여기에 넣는다. -->
</bean>
```

| `idref`요소의 `local` 어트리뷰트는 4.0 beans XSD부터 지원되지 않는다. 왜냐하며 `local`어트리뷰트는 더 이상 일반적인 `bean` 참조보다 추가적인 값을 제공하지 않기 때문이다. 4.0 스키마로 업그레이드 할 시, `ifref local`참조를 `idref bean`참조로 변경하십시오. |

<h5 id="beans-inner-beans">내부 빈</h5>

`<property/>`와 `<constructor-arg/>`요소 내부에 `<bean/>`요소는 내부 빈을 아래 예시처럼 정의한다:
```xml
<bean id="outer" class="...">
    <!-- 빈을 참조하는 대신에 빈은 정의한다. -->
    <property name="target">
        <bean class="com.example.Person"> <!-- 내부 빈 -->
            <property name="name" value="Fiona Apple"/>
            <property name="age" value="25"/>
        </bean>
    </property>
</bean>
```
내부 빈 정의는 ID나 이름이 필요하지 않다. 작성하더라도 컨테이너는 그 값을 식별자로 사용하지 않는다. 컨테이너는 `scope` 플래그도 무시한다. 왜냐하면 내부빈은 언제나 익명이고 외부 빈과 함께 생성되기 때문이다. 내부 빈을 감싸는 외부 빈의 도움없이 내부 빈에 접근하거나 의존성 주입을 할 수 없다.     
커스텀 스코프 파괴 콜백을 가지는 것은 가능하다. 예를 들면, 싱글톤 빈 내부에 request 스코프 내부 빈을 가질 수 있다. 내부 빈의 생성은 외부 빈에 묶여 있지만 파괴 콜백을 통해 내부 빈을 request 스코프 생명주기에 포함시킬 수 있다. 흔한 경우는 아니다. 내부 빈은 일반적으로 외부 빈과 같은 생명주기를 가진다.

<h5 id="beans-collection-elements">컬렉션</h5>

`<list/>`,`<set/>`,`<map/>`,`<props/>` 요소는 프로퍼티와 어규먼트를 자바 `Collection`타입인 `List`,`Set`,`Map`,`Properties`에 각각 매칭시킨다. 아래는 사용 예시이다:
```xml
<bean id="moreComplexObject" class="example.ComplexObject">
    <!-- setAdminEmails(java.util.Properties) 호출한 것과 같은 결과 -->
    <property name="adminEmails">
        <props>
            <prop key="administrator">administrator@example.org</prop>
            <prop key="support">support@example.org</prop>
            <prop key="development">development@example.org</prop>
        </props>
    </property>
    <!-- setSomeList(java.util.List) 호출한 것과 같은 결과 -->
    <property name="someList">
        <list>
            <value>a list element followed by a reference</value>
            <ref bean="myDataSource" />
        </list>
    </property>
    <!-- setSomeMap(java.util.Map) 호출한 것과 같은 결과 -->
    <property name="someMap">
        <map>
            <entry key="an entry" value="just some string"/>
            <entry key ="a ref" value-ref="myDataSource"/>
        </map>
    </property>
    <!-- setSomeSet(java.util.Set) 호출한 것과 같은 결과 -->
    <property name="someSet">
        <set>
            <value>just some string</value>
            <ref bean="myDataSource" />
        </set>
    </property>
</bean>
```
map의 키, map의 값, set의 값은 아래의 요소가 될수 있다:
```
bean | ref | idref | list | set | map | props | value | null
```

###### 컬렉션 결합

스프링 컨테이너는 컬렉션 결합을 지원한다. 어플리케이션 개발자는 부모 `<list/>`,`<map/>`,`<set/>`,`<props/>` 요소를 정의하고 자식 `<list/>`,`<map/>`,`<set/>`,`<props/>`가 그 값을 상속하거나 덮어 쓰도록 정의할 수 있다. 이 말은 자식 컬렉션의 값은 부모의 요소와 자식의 요소를 결합한 결과라는 것이다. 부모 컬렉션의 값은 자식 컬렉션 요소에서 덮어쓰는 방법으로 결합한 결과이다.     
이 장에서 부모 자식 빈 동작과정을 설명한다. 부모 자식 빈 정의에 익숙하지 않다면 [관련 장](#beans-child-bean-definitions)을 읽어볼 수 있다.     
아래 예시는 컬렉션 결합의 예시이다:
```xml
<beans>
    <bean id="parent" abstract="true" class="example.ComplexObject">
        <property name="adminEmails">
            <props>
                <prop key="administrator">administrator@example.com</prop>
                <prop key="support">support@example.com</prop>
            </props>
        </property>
    </bean>
    <bean id="child" parent="parent">
        <property name="adminEmails">
            <!-- 결합은 자식 컬렉션에서 명시된다. -->
            <props merge="true">
                <prop key="sales">sales@example.com</prop>
                <prop key="support">support@example.co.uk</prop>
            </props>
        </property>
    </bean>
<beans>
```
`child` 빈 정의의 `adminEmails`프로퍼티 내부 `<props/>`요소에 `merge=true`어트리뷰트가 있다는 것을 주목하십시오. `child` 빈이 인스턴스화 될 때, 만들어진 인스턴스는 자식`adminEmails`컬렉션과 부모 `adminEmails`컬렉션을 합친 `adminEmails``Properties` 컬렉션을 가지고 있다. 아래 결합된 결과를 보여준다:
```
administrator=administrator@example.com
sales=sales@example.com
support=support@example.co.uk
```
자식 `Properties`컬렉션의 값은 부모 `<props/>`에 모든 프로퍼티값을 상속받고 `support`의 자식 컬렉션의 값이 부모 컬렉션의 값을 덮어쓴다.     
결합하는 과정은 `<list/>`,`<map/>`,`<set/>`에도 비슷하게 동작한다. `<list/>`요소의 케이스에서 `List` 컬렉션의 순서(`ordered` 컬렉션 값의 개념)는 유지된다. 부모의 값은 자식의 값 앞에 나온다. `Map`,`Set`,`Properties`컬렉션에서 순서는 없다. 게다가, 순서가 없는 컬렉션 타입 개념은 컨테이너가 내부적으로 사용하는 `Map`,`Set`,`Properties` 구현과 일치한다.

###### 컬렉션 결합의 한계

`Map`과 `List`처럼 다른 타입의 컬렉션을 결합할 수는 없다. 이런 것을 시도한다면 상황에 알맞은 `Exception`이 던져질 것이다. `merge` 어트리뷰트는 상속받는 자식 정의에 작성되어야한다. `merge`어트리뷰트를 부모 컬렉션에 작성하는 것은 중복이고 원하는 결합을 얻지 못할 것이다.

###### 제네릭이 아닌 컬렉션

자바 5에서 제네릭 타입이 소개된 후로 제네릭이 아닌 컬렉션을 사용할 수 있다. `Collection`을 (예를들어) `String`만 가지도록 정의할 수 있다. 만약 스프링 의존성 주입에서 제네릭이 아닌 컬렉션을 빈에 주입한다면, 스프링 타입 변환의 도움을 받아 제네릭이 아닌 `Collection`의 요소들을 `Collection`에 추가되기 전에 올바른 타입으로 변환할 수 있다. 아래 자바 클래스와 빈 정의가 어떻게 사용하는 지 보여준다:
```java
public class SomeClass {

    private Map<String, Float> accounts;

    public void setAccounts(Map<String, Float> accounts) {
        this.accounts = accounts;
    }
}
```
```xml
<beans>
    <bean id="something" class="x.y.SomeClass">
        <property name="accounts">
            <map>
                <entry key="one" value="9.99"/>
                <entry key="two" value="2.75"/>
                <entry key="six" value="3.99"/>
            </map>
        </property>
    </bean>
</beans>
```
`something`빈의 `accounts`프로퍼티가 주입되려 준비될 때, 리플렉션을 이용하여 `Map<String, Float>`라는 정보를 알게된다. 따라서 스프링의 타입 변환 장치는 여러 값이 `Float`가 되야된다는 것을 인식하고 String 값(`9.99, 2.27, 3.44`)를 `Float`타입으로 변환한다.

<h5 id="beans-null-element">Null과 비어있는 스트링</h5>

스프링은 프로퍼티에 사용된 빈 어규머트를 비어있는 `Strings`로 취급한다. 아래의 XML 기반 설정 메타데이터는 `email`프로퍼티를 비어있는 `String` ("")로 설정한다.
```xml
<bean class="ExampleBean">
    <property name="email" value=""/>
</bean>
```
아래는 동일한 기능의 자바 코드이다:
```java
exampleBean.setEmail("");
```
`<null/>`요소는 `null`값을 다룬다. 아래 예시이다:
```xml
<bean class="ExampleBean">
    <property name="email">
        <null/>
    </property>
</bean>
```
아래는 동일한 자바 코드이다:
```java
exampleBean.setEmail(null);
```

<h5 id="beans-p-namespace">P 네임스페이스를 이용한 XML 단축</h5>

p 네임스페이스를 이용하면 (중첩 `property/>`요소 대신에) `bean` 요소의 어트리뷰트를 사용하여 빈의 프로퍼티 값을 설정할 수 있다.      
스프링은 XML 스키마 정의에 기반한 [네임스페이스](../spring-reference-core-9-appendix/#xsd-schemas)를 사용하여 확장 설정이 가능하다. 이 장에서 다루는 `bean` 설정 형식은 XML 스키마 문서에 정의되어 있다. 하지만 p 네임스페이스는 XSD 파일로 정의되 있지 않고 스프링 코어에만 존재한다.     
아래 예시는 같은 결과의 두 XML 예시이다.(표준 XML 형식, p-네임스페이스 형식):
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="classic" class="com.example.ExampleBean">
        <property name="email" value="someone@somewhere.com"/>
    </bean>

    <bean name="p-namespace" class="com.example.ExampleBean"
        p:email="someone@somewhere.com"/>
</beans>
```
예제 빈 정의에서 p-네임스페이스에서 `email`이라는 어트리뷰트를 보여준다. 이 방법으로 스프링에게 프로퍼티 정의를 포함시킨다. 이전에 이미 언급했듯이, p-네임스페이스는 스키마 정의가 없다. 따라서 프로퍼티 이름에 맞게 어트리뷰트 이름을 정할 수 있다.     
아래 예시는 다른 빈을 참조하는 두 빈 정의의 예시이다:
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="john-classic" class="com.example.Person">
        <property name="name" value="John Doe"/>
        <property name="spouse" ref="jane"/>
    </bean>

    <bean name="john-modern"
        class="com.example.Person"
        p:name="John Doe"
        p:spouse-ref="jane"/>

    <bean name="jane" class="com.example.Person">
        <property name="name" value="Jane Doe"/>
    </bean>
</beans>
```
이 예시에서 p-네임스페이스를 이용하여 프로퍼티 값을 설정하는 방법뿐만 아니라 프로퍼티 참조를 하는 특별한 형식도 보여준다. 첫번째 빈정의에서 빈 `john`에 빈 `jane`을 참조시키기 위해 `<property name="spouse" ref="jane"/>`를 설정했다. 두번 째 빈 정의에서 `p:spouse-ref="jane"`을 사용하여 같은 동작을 하였다. 이 경우, `spouse`는 프로퍼티 이름이고 `-ref` 부분은 이 값이 참조라는 것을 표시한다.

| p-네임스페이스는 표준 XML 형식보다 유연하지 못한다. 예를 들어 `Ref`로 끝나는 프로퍼티에 참조를 시킬 수 없다. XML 문서를 만드는 데에 있어 세 접근 방법을 모두 사용하지 않도록 팀원들과 충분히 상의하고 조심스럽게 진행하기를 추천한다. |

<h5 id="beans-c-namespace">c 네임스페이스를 이용한 XML 단축</h5>

[p-네임스페이스를 사용한 XML 단축](#beans-p-namespace)과 비슷하게 스프링 3.1부터 사용가능한 c-네임스페이스는 중첩 `<constructor-arg/>`대신 어트리뷰트를 사용할 수 있도록 해준다.     
아래는 `c:`네임스페이스를 사용하여 [생성자 기반 의존성 주입](#beans-constructor-injection)과 같은 동작을 하는 예시이다:
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:c="http://www.springframework.org/schema/c"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="beanTwo" class="x.y.ThingTwo"/>
    <bean id="beanThree" class="x.y.ThingThree"/>

    <!-- 선택적인 어규먼트를 사용하는 일반적인 정의 -->
    <bean id="beanOne" class="x.y.ThingOne">
        <constructor-arg name="thingTwo" ref="beanTwo"/>
        <constructor-arg name="thingThree" ref="beanThree"/>
        <constructor-arg name="email" value="something@somewhere.com"/>
    </bean>

    <!-- 어규먼트 이름을 사용하는 c-네임스페이스 정의 -->
    <bean id="beanOne" class="x.y.ThingOne" c:thingTwo-ref="beanTwo"
        c:thingThree-ref="beanThree" c:email="something@somewhere.com"/>

</beans>
```
`c:`네임스페이스는 생성자 어규먼트를 설정하는데에 `p:`네임스페이스와 같은 컨벤션(빈 참조 마지막에 `-ref`)을 사용한다. p-네임스페이스처럼 XSD 스키마에 정의되지 않아도 사용가능하다 (스프링 코어에 포함되어 있다).     
생성자 어규먼트 이름을 사용할 수 없는 드문 경우(대개 바이트코드가 디버그 정보 없이 컴파일 되는 경우)에는, 인덱스를 아래와 같이 사용가능하다:
```xml
<!-- c-네임스페이스 인덱스 정의 -->
<bean id="beanOne" class="x.y.ThingOne" c:_0-ref="beanTwo" c:_1-ref="beanThree" c:_2="something@somewhere.com"/>
```

| XML 문법 때문에 인덱스 표현은 앞에 `_`가 필요하다. 왜냐하면 XML 어트리뷰트 이름은 숫자로 시작할 수 없기 때문이다(일부 IDE는 허용하지만). 인덱스 표현은 `constructor-arg>`에서도 사용가능하지만 단순히 순서만으로 충분하기에 잘 사용되지는 않는다. |

실제로는 생성자 분석 [과정](#beans-factory-ctor-arguments-resolution)에서 어규먼트 매칭을 효율적으로 수행한다. 따라서 정말 필요한 경우가 아니면, 이름 표현을 사용할 것을 추천한다.

<h5 id="beans-compound-property-names">복합 프로퍼티 이름</h5>

빈 프로퍼티를 설정할 때, 프로퍼티 패쓰의 마지막 요소를 제외한 모든 요소가 `null`이 아니라면 복합 프로퍼티나 중첩 프로퍼티를 사용할 수 있다. 아래 빈 정의롤 보자:
```xml
<bean id="something" class="things.ThingOne">
    <property name="fred.bob.sammy" value="123" />
</bean>
```
`something`빈은 `fred`프로퍼티를 가진다. `fred`프로퍼티는 `bob`프로퍼티를 가진다. `bob`프로퍼티는 `sammy`프로퍼티를 가진다. `sammy`프로퍼티의 값은 123으로 설정된다. 이것이 동작하기 위해서는 빈이 생성된 뒤, `fred`프로퍼티와 `bob`프로퍼티가 `null`이면 안된다. 그렇지 않다면 `NullPointerException`이 발생한다.

<h4 id="beans-factory-dependson">depends-on 사용하기</h4>

빈이 다른 빈에 의존한다는 것은, 대부분 다른 빈이 나머지 빈의 프로퍼티로 설정된 다는 것을 의미한다. 일반적으로 XML 기반 설정 메타데이터의 [`<ref/>` 요소](#beans-ref-element)로 설정된다. 하지만 종종 빈과 빈 간의 의존성을 간접적이다. 예를 들면, 데이터 베이스 드라이버 등록과 같은 정적 초기화가 실행되어야 하는 경우이다. `depends-on`어트리뷰트는 명시적으로 다른 빈이 먼저 생성되도록 강제한다. 아래의 예시는 `depends-on`어트리뷰트를 사용하는 예시이다:
```xml
<bean id="beanOne" class="ExampleBean" depends-on="manager"/>
<bean id="manager" class="ManagerBean" />
```
여러 빈들 사이의 의존성을 표시하기 위하여 빈 리스트를 `depends-on` 어트리뷰트의 값으로 제공할 수 있다(반점, 공백, 세미 콜론은 적합한 구분자이다):
```xml
<bean id="beanOne" class="ExampleBean" depends-on="manager,accountDao">
    <property name="manager" ref="manager" />
</bean>

<bean id="manager" class="ManagerBean" />
<bean id="accountDao" class="x.y.jdbc.JdbcAccountDao" />
```

| `depends-on`어트리뷰트는 초기화 시 의존성을 표현할 수 있다. [싱글톤](#beans-factory-scopes-singleton)빈의 경우, 파괴 시 의존성 또한 표현 가능하다. `depends-on`으로 다른 빈에 의존하고 있는 빈이 먼저 파괴된다. 따라서 `depends-on`은 파괴 순서도 정할 수 있다. |

<h4 id="beans-factory-lazy-init">지연 초기화 빈</h4>

기본 설정으로 `ApplicationContext` 구현체는 [싱글톤](#beans-factory-scopes-singleton)빈을 초기화 과정에서 생성하고 설정한다. 일반적으로 설정과 환경에 오류가 바로 발견되기에 선-인스턴스화는 바람직하다. 선-인스턴스화가 바람직하지 않은 경우, 빈 정의에 지연 초기화를 설정함으로 선-인스턴스화를 막을 수 있다. IoC 컨테이너는 지연 초기화 빈을 처음 시작할 때가 아닌 빈이 처음으로 필요해 질 때 빈을 생성한다.     
XML에서 이 설정은 `<bean/>`요소의 `lazy-init`어트리뷰트로 설정할 수 있다:
```xml
<bean id="lazy" class="com.something.ExpensiveToCreateBean" lazy-init="true"/>
<bean name="not.lazy" class="com.something.AnotherBean"/>
```
위에 설정에 의하여 `ApplicationContext`은 `not.lazy`빈을 `ApplicationContext`가 시작할 때 생성하고 `lazy`빈은 그렇지 않다.     
하지만 싱글톤 빈이 지연 초기화 빈을 의존하는 경우, `ApplicationContext`는 지연 초기화 빈을 시작할 때 생성한다. 왜냐하면 싱글톤 빈의 의존성을 만족시켜야 하기 때문이다. 지연 초기화 빈이 지연 초기화가 아닌 싱글톤 빈에 주입되야하기 때문이다.     
`<beans/>`요소의 `default-lazy-init`어트리뷰트를 이용해 컨테이너 레벨에서 지연 초기화 설정이 가능하다:
```xml
<beans default-lazy-init="true">
    <!-- 빈들은 선 인스턴스화 되지 않을 것이다. -->
</beans>
```

<h4 id="beans-factory-autowire">자동연결 협력자</h4>

스프링 컨테이너는 함께 동작하는 빈들 사이의 관계를 자동 연결할 수 있다. 스프링이 `ApplicationContext`의 내용을 살펴보고 빈 간의 연결을 설정하도록 할 수 있다. 자동연결은 아래와 같은 장점이 있다:
* 자동연결은 프로퍼티와 생성자 어규먼트를 명시해야할 필요성을 줄여준다. (빈 템플릿처럼 [이 장에서 언급된](#beans-child-bean-definitions) 다른 방식 또한 이 관점에서 중요하다.)
* 자동연결을 하면 객체가 바뀜에 따라 설정도 바뀐다. 예를 들어, 클래스에 의존성을 추가해야 한다면 설정 변경없이 자동으로 의존성이 해결될 것이다. 따라서 자동연결은 안정적인 코드의 명시적 연결을 제거할 필요 없는 개발과정에서 매우 유용하다.

XML 기반 설정 메타데이터([의존성 주입](#beans-factory-collaborators))에서 `<bean/>`요소에 `autowire`어트리뷰트를 이용하여 자동연결 방법을 명시할 수 있다. 자동연결 기능은 네가지 방법이 있다. 빈마다 자동연결을 명시하여 자동연결할 것을 선택할 수 있다. 아래 표가 자동연결 방법을 보여준다:

###### 표 2. 자동연결 방법

| 방법 | 설명 |
| --- | --- |
| `no` | (기본 설정) 자동연결을 하지 않는다. `ref`요소로 빈 참조를 정의해야한다. 기본 설정을 변경하는 것은 큰 프로젝트에서 추천하지 않는다. 왜냐하면 참조를 명시하는 것은 확실한 통제권과 명확함을 주기 때문이다. 추가적으로 시스템의 구조도 설명해준다. |
| `byName` | 프로퍼티 이름으로 자동연결한다. 스프링이 자동연결 해야할 프로퍼티와 같은 이름을 가진 빈을 찾는다. 예를 들면, 이름을 자동연결이 설정된 빈정의가 `master` 프로퍼티를 가지고 있다면(즉, `setMaster(..)` 메소드가 있다면), 스프링이 빈정의에서 `master`를 찾아 해당 프로퍼티로 설정한다. |
| `byType` | 컨테이너에 해당 타입의 빈이 오직 한개만 존재하면 프로퍼티로 설정해준다. 만약 해당 타입의 빈이 여러개 존재한다면 `byType` 자동연결을 사용할 수 없다는 치명적인 예외가 던져진다. 만약 해당 타입의 빈이 없다면, 프로퍼티가 설정되지 않는다. |
| `constructor` | `byType`과 비슷하지만 생성자 어규먼트에 적용된다. 만약 해당 타입의 빈이 없다면, 치명적인 에러가 발생한다. |

`byType`이나 `constructor` 자동연결 방법을 사용하면 배열과 타입이 있는 컬렉션을 연결할 수 있다. 이러한 경우에, 요구하는 타입과 제공되는 타입은 반드시 일치해야한다. 만약 키 값 타입이 `String`인 `Map`을 요구한다면 타입이 정해진 `Map` 인스턴스를 자동연결할 수 있다. 자동연결된 `Map`인스턴스가 키로 해당하는 빈의 이름을 가지고 있고 값으로 해당하는 타입의 빈을 가지고 있어야한다. 

<h5 id="beans-autowired-exceptions">자동 연결의 한계와 단점</h5>

자동연결은 프로젝트에서 일관적으로 사용될 경우 가장 좋다. 자동연결이 보편적으로 사용되지 않는 다면, 개발자에게 혼란을 줄 수 있다.     
자동연결의 한계와 단점을 생각해보자:
* `property`와 `constructor-arg`로 명시된 설정이 항상 자동연결보다 우선시된다. 원시 값, `String` `Classes`(혹은 이 값들의 배열)와 같은 단순한 프로퍼티는 자동연결 되지 않는다. 이 한계는 스프링의 디자인적 한계이다.
* 명시적 연결보다 자동연결이 불명확하다. 위에 표에서 적어놨듯이 스프링은 예상치못한 결과를 가져오는 모호한 상황을 피하려고 주의하지만 스프링이 관리하는 빈 간의 관계는 더이상 명확하게 문서화되지 않는다.
* 스프링 컨테이너를 이용하여 문서를 만드는 도구가 자동연결 정보를 사용할 수 없을 수도 있다.
* 여러 빈 정의가 프로퍼티나 생성자 어규먼트의 타입과 일치할 수 있다. 배열, 컬렉션, `Map` 인스턴스에는 문제가 아니다. 하지만 하나의 값을 요구하는 의존성에 있어서, 이 모호함은 스스로 해결되지 못한다. 만약 단 한개의 빈 정의가 제공되지 못하면 예외가 발생한다.

마지막 경우에서 여러 해결 방법이 있다:

* 명시적 연결로 자동연결을 제거한다.
* `autowire-candidate`어트리뷰트를 `false`로 하여 자동연결을 피한다.([다음 섹션](#beans-factory-autowire-candidate)에서 설명된다.)
* `<bean/>`요소의 `primary`어트리뷰트를 `true`로 변경함으로써 한 개의 빈 정의를 최 우선 빈 정의로 설정한다.
* 어노테이션 기반의 설정으로 가능한 더 정제된 설정을 사용한다.([어노테이션 기반 컨테이너 설정](#beans-annotation-config)에 설명되어 있다.)

<h5 id="beans-factory-autowire-candidate">자동 연결에 빈 제외하기</h5>

각각의 빈 마다 자동연결에서 제외할 수 있다. 스프링 xmL 형식에서 `<bean/>`요소의 `autowire-candidate`를 `false`로 변경하면 컨테이너가 그 특정 빈을 자동연결 구조([`@Autowired`](#beans-autowired-annotation)와 같은 어노테이션 스타일 설정또한 포함한다.)에서 사용불가능 하도록 변경한다.

| `autowire-candidate`어트리뷰트는 오직 타입 기반 자동연결에만 영향을 미친다. 이름을 이용한 명시적 연결에는 영향을 끼치지 않는다. 결과적으로 이름을 이용한 자동연결은 이름만 맞다면 빈을 주입할 것이다. |

빈 이름에 패턴 매칭을 이용하여 자동연결 후보 빈을 제한할 수 있다. 최 상위 `<beans/>`요소의 `default-autowire-candidates`어트리뷰트에 한개 이상의 패턴을 작성할 수 있다. 예를 들어, `Repository`로 끝나는 이름을 가진 빈만 자동연결 후보 빈으로 설정하고자 한다면 `*Repository`를 이용하면 된다. 여러개의 패턴을 작성하려면 반점`,`으로 분리되는 목록으로 작성하면 된다. 빈 정의에 `autowire-candidate`어트리뷰트를 명시적으로 작성하면 그 내용이 항상 우선시 된다. 이러한 빈에는 패턴 매칭 규칙이 적용되지 않는다.     
이러한 방법은 자동연결을 이용하여 다른 빈에 주입되는 것을 막고자 할 때 유용하다. 이 방법은 자동연결 제외된 빈이 자동연결을 이용하여 자신의 의존성을 주입받지 못한다는 것을 의미하지 않는다. 자동연결 제외된 빈이 다른 빈에 자동연결되지 않는다는 의미이다.

<h4 id="beans-factory-method-injection">메소드 주입</h4>

<h5 id="beans-factory-lookup-method-injection">Lookup 메소드 주입</h5>
<h5 id="beans-factory-arbitrary-method-replacement">임의의 메소드 대체</h5>




<h3 id="beans-factory-scopes">빈 스코프</h3>

<h4 id="beans-factory-scopes-singleton">싱글톤 스코프</h4>
<h4 id="beans-factory-scopes-prototype">프로토타입 스코프</h4>
<h4 id="beans-factory-scopes-sing-prot-interaction">프로토타입 빈 의존성을 가지는 싱글톤 빈</h4>
<h4 id="beans-factory-scopes-other">Request, Session, Application, WebSocket 스코프</h4>

<h5 id="beans-factory-scopes-other-web-configuration">초기 웹 설정</h5>
<h5 id="beans-factory-scopes-request">Request 스코프</h5>
<h5 id="beans-factory-scopes-session">Session 스코프</h5>
<h5 id="beans-factory-scopes-application">Application 스코프</h5>
<h5 id="beans-factory-scopes-other-injection">스코프 빈 의존성</h5>


<h4 id="beans-factory-scopes-custom">커스텀 스코프</h4>

<h5 id="beans-factory-scopes-custom-creating">커스텀 스코프 만들기</h5>
<h5 id="beans-factory-scopes-custom-using">커스텀 스코프 이용하기</h5>




<h3 id="beans-factory-nature">빈의 특성 설정하기</h3>

<h4 id="beans-factory-lifecycle">콜백의 생명주기</h4>

<h5 id="beans-factory-lifecycle-initializingbean">콜백 정의하기</h5>
<h5 id="beans-factory-lifecycle-disposablebean">소멸자 콜백</h5>
<h5 id="beans-factory-lifecycle-default-init-destroy-methods">기본 초기화 메소드, 파괴 메소드</h5>
<h5 id="beans-factory-lifecycle-combined-effects">생명주기 작동원리 조합하기</h5>
<h5 id="beans-factory-lifecycle-processor">Startup, Shutdown 콜백</h5>
<h5 id="beans-factory-shutdown">웹 어플리케이션이 아닌 환경에서 스프링 IoC 컨테이너 아름답게 종료하기</h5>


<h4 id="beans-factory-aware">ApplicationContextAware, BeanNameAware</h4>
<h4 id="awar-list">다른 Aware 인터페이스</h4>


<h3 id="beans-child-bean-definitions">빈의 정의 상속</h3>
<h3 id="beans-factory-extension">컨테이너 확장 시점</h3>

<h4 id="beans-factory-extension-bpp">BeanPostProcessor를 이용하여 빈 커스터마이징 하기</h4>

<h5 id="beans-factory-extension-bpp-examples-hw">예제: Hello World, BeanPostProcessor 스타일</h5>
<h5 id="beans-factory-extension-bpp-examples-rabpp">예제: RequiredAnnotationBeanPostProcessor</h5>


<h4 id="beans-factory-extension-factory-postprocessors">BeanFactoryPostProcessor를 이용하여 설정 메타데이터 설정하기</h4>

<h5 id="beans-factory-placeholderconfigurer">예제: 클래스 이름 대체 PropertySourcesPlaceholderConfigurer</h5>
<h5 id="beans-factory-overrideconfigurer">예제: PropertyOverrideConfigurer</h5>


<h4 id="beans-factory-extension-factorybean">FactoryBean을 이용하여 인스턴스화 로직 커스터마이징 하기</h4>


<h3 id="beans-annotation-config">어노테이션 기반 컨테이너 설정</h3>

<h4 id="beans-required-annotation">@Required</h4>
<h4 id="beans-autowired-annotation">@Autowired 사용하기</h4>
<h4 id="beans-autowired-annotation-primary">@Primary를 이용하여 어노테이션 기반 자동 연결 미세 조정하기</h4>
<h4 id="beans-autowired-annotation-qualifiers">Qualifers를 이용하여 어노테이션 기반 자동 연결  미세 조정하기</h4>
<h4 id="beans-generics-as-qualifiers">제네릭을 자동연결 Qualifers로 이용하기</h4>
<h4 id="beans-custom-autowire-configurer">CustomAutowireConfig 이용하기</h4>
<h4 id="beans-resource-annotation">@Resource를 사용한 주입</h4>
<h4 id="beans-value-annotations">@Value 사용하기</h4>
<h4 id="beans-postconstruct-and-predestroy-annotations">@PostConstruct와 @PreDestory 사용하기</h4>


<h3 id="beans-classpath-scanning">클래스패스 스캐닝과 관리되는 컴포넌트</h3>

<h4 id="beans-stereotype-annotations">@Component와 추가적인 정형화된 어노테이션</h4>
<h4 id="beans-meta-annotations">메타 어노테이션과 복합 어노테이션 이용하기</h4>
<h4 id="beans-scanning-autodetection">클래스 검색과 빈 정의 등록을 자동으로 수행하기 </h4>
<h4 id="beans-scanning-filters">필터를 이용하여 스캐닝 커스터마이징하기</h4>
<h4 id="beans-factorybeans-annotations">컴포넌트 내부에 빈 메타데이터 정의하기</h4>
<h4 id="beans-scanning-name-generator">자동 검색된 컴포넌트 이름 붙이기</h4>
<h4 id="beans-scanning-scope-resolver">자동 검색된 컴포넌트에 스코프 부여하기</h4>
<h4 id="beans-scanning-qualifiers">Qualifier 메타데이터 어노테이션으로 부여하기</h4>
<h4 id="beans-scanning-index">후보 컴포넌트 색인 생성하기</h4>


<h3 id="beans-standard-annotations">JSR 330 표준 어노테이션 사용하기</h3>

<h4 id="beans-inject-named">@Inject와 @Named를 이용한 의존성 주입</h4>
<h4 id="beans-named">@Named와 @ManagedBean: @Component 어노테이션의 표준 표현</h4>
<h4 id="beans-stardard-annotations-limitations">JSR-330 표준 어노테이션의 한계</h4>


<h3 id="beans-java">자바 기반 컨테이너설정</h3>

<h4 id="beans-java-basic-concepts">기본 개념: @Bean과 @Configuration</h4>
<h4 id="beans-java-instantiating-container">AnnotationConfigApplicationContext를 이용하여 스프링 컨테이너 인스턴스화 하기</h4>

<h5 id="beans-java-instantiating-container-constructor">간단하게 만들어보기</h5>
<h5 id="beans-java-instantiating-container-register">register(Class<?>...)을 이용하여 프로그래밍적으로 컨테이너 만들기</h5>
<h5 id="beans-java-instantiating-container-scan">scan(String...)을 이용하여 컴포넌트 스캔 활성화하기</h5>
<h5 id="beans-java-instantiating-container-web">AnnotationConfigWebApplicationContext을 이용한 웹 어플리케이션 지원</h5>


<h4 id="beans-java-bean-annotation">@Bean 어노테이션 사용하기</h4>

<h5 id="beans-java-declaring-a-bean">빈 정의하기</h5>
<h5 id="beans-java-dependencies">빈 의존성</h5>
<h5 id="beans-java-lifecycle-callbacks">생명주기 콜백 받기</h5>
<h5 id="beans-java-specifying-bean-scope">빈 스코프 설정하기</h5>
<h5 id="beans-java-customizing-bean-naming">빈 이름 설정하기</h5>
<h5 id="beans-java-bean-aliasing">빈 별명 설정하기</h5>
<h5 id="beans-java-bean-description">빈 설명</h5>


<h4 id="beans-java-configuration-annotation">@Configuration 어노테이션 이용하기</h4>

<h5 id="beans-java-injecting-dependencies">내부 빈 의존성 주입하기</h5>
<h5 id="beans-java-method-injection">Lookup 메소드 주입하기</h5>
<h5 id="beans-java-further-information-java-config">자바 기반 설정이 내부적으로 작동하는 방법에 대한 추가적인 정보</h5>


<h4 id="beans-java-composing-configuration-classes">자바 기반 설정 구성하기</h4>

<h5 id="beans-java-using-import">@Import 어노테이션 사용하기</h5>
<h5 id="beans-java-conditional">조건에 따라 @Configuration 클래스와 @Bean 메소드 포함시키기</h5>
<h5 id="beans-java-combining">자바 기반 설정과 XML 기반 설정 조합하기</h5>




<h3 id="beans-envirionment">환경 추상화</h3>

<h4 id="beans-definition-profiles">빈 정의 프로필</h4>

<h5 id="beans-definition-profiles-java">@Profile 이용하기</h5>
<h5 id="beans-definition-profiles-xml">XML 빈 정의 프로필</h5>
<h5 id="beans-definition-profiles-enable">프로필 활성화하기</h5>
<h5 id="beans-definition-profiles-default">기본 프로필</h5>


<h4 id="beans-property-source-abstaction">PropertySource 추상화</h4>
<h4 id="beans-using-propertysource">@PropertySource 사용하기</h4>
<h4 id="beans-placeholder-resolution-in-statements">표현법의 PlaceHolder Resolution</h4>


<h3 id="context-load-time-weaver">LoadTimeWeaver 등록하기</h3>
<h3 id="context-introduction">어플리케이션 콘텍스트의 추가적인 사용기능</h3>

<h4 id="context-functionality-messagesource">MessageSource를 이용한 내재화</h4>
<h4 id="context-functionality-events">표준, 커스텀 이벤트</h4>

<h5 id="context-functionality-events-annotation">어노테이션 기반 이벤트 리스너</h5>
<h5 id="context-functionality-events-async">비동기 리스너</h5>
<h5 id="context-functionality-events-order">리스너 순서 정하기</h5>
<h5 id="context-functionality-events-generics">Generic 이벤트</h5>


<h4 id="context-functionality-resources">로우레벨 자원에 편리한 접근</h4>
<h4 id="context-create">웹 어플리케이션을 위한 편리한 ApplicationContext 인스턴스화</h4>
<h4 id="context-deploy-rar">스프링 ApplicationContext를 Java EE RAR 파일로 배포하기</h4>


<h3 id="beans-beanfactory">빈 팩토리</h3>

<h4 id="context-introduction-ctx-vs-beanfactory">BeanFactory? 혹은 ApplicationContext?</h4>

