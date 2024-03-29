---
layout: post
title:  "[Spring Reference] 스프링 레퍼런스 #1 핵심 - 1. IoC 컨테이너"
createdDate:   2020-05-17T18:42:00+09:00
date:   2021-10-16T14:09:00+09:00
excerpt: "한글 번역 : 스프링 레퍼런스 #1 핵심 - 1. IoC 컨테이너"
pagination: enabled
author: SoonYong Hong
categories: Spring_Reference
tags: Spring 
sitemap:
    changefreq: yearly
---

이 내용은 [스프링 문서 5.2.6.RELEASE](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#spring-core)를 번역한 내용으로 오역이 있을 수 있습니다.

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
        1. <a href="#beans-factory-lifecycle">생명주기 콜백</a>
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
        1. <a href="#beans-autowired-annotation-qualifiers">Qualifiers를 이용하여 어노테이션 기반 자동 연결 미세 조정하기</a>
        1. <a href="#beans-generics-as-qualifiers">제네릭을 자동연결 Qualifiers로 이용하기</a>
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
        1. <a href="#context-functionality-messagesource">MessageSource를 이용한 국제화</a>
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
<img src="/assets/spring-framework-reference/container-magic.png"/>
**그림 1. 스프링 IoC 컨테이너**

<h4 id="beans-factory-metadata"> 1.2.1 메타데이터 설정하기</h4>
위의 다이어그램에서 볼 수 있듯이, 스프링 IoC 컨테이너는 설정 메타데이터를 이용한다. 어플리케이션 개발자가 스프링 컨테이너에게 어플리케이션의 객체들을 인스턴스화하고 설정하고 수집하는 방법을 표현하는 것이 설정 메타데이터이다.     
전통적으로 메타데이터는 간단하고 직관적인 XML 형식으로 제공되었고 이 XML형식은 이 장에서 스프링 IoC 컨테이너의 주요한 개념과 기능을 표현하기 위해 사용된다.

| |
| ----- |
| ***!** XML 기반 메타데이터는 메타데이터를 설정하는 유일한 형식이 아니다. 스프링 IoC 컨테이너 그 자체는 설정 메타데이터가 어떤 형식으로 작성되어 있는 지와 완전히 무관하다. 최근에는 많은 개발자들은 스프링 어플리케이션에서 [자바 기반 설정](#beans-java)을 선택한다.* |

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
`ApplicationContext`생성자 에 제공된 경로는 자바 `CLASSPATH`, 로컬 파일 시스템과 같은 다양한 리소스로부터 스프링 컨테이너가 설정 메타데이터를 불러올수 있는 스트링이다.

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

```

| |
| ----- |
| ***!** 스프링 IoC 컨테이너에 대하여 배운 이후, URI로 적힌 위치로부터 손쉽게 읽어오는 방법을 제공하는 스프링 `Resource` 추상화([리소스](../spring-reference-core-2-resources#resources)에 설명되어 있다.)에 대하여 알고 싶을 수도 있다. 특히 [어플리케이션 컨텍스트와 리소스 경로]에 설명되어 있는대로, 어플리케이션 컨텍스트를 만드는데 `Resource`경로를 사용한다.* |

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

| |
| ----- |
| ***!** 부모 디렉토리에 있는 파일을 "../"를 이용하여 나타내는 것은 가능하지만 추천하지 않는다. 이렇게 하면 현재 어플리케이션 외부에 파일에 의존하게된다. 특히 `classpath:`에 이러한 방법을 사용하는 것은 매우 추천하지 않는다.(예를 들어 `classpath:../services.xml`) 런타임에 "가장 가까운" 클래스패스 루트를 찾아 부모 디렉터리를 살펴보기 때문에 틀린 디렉토리를 선택하게 될 수도 있다.<br>상대경로 대신 완전한 리소스 위치를 얼마든지 사용할 수 있다. 예를 들면 `file:C:/config/services.xml` 또는 `classpath:/config/services.xml`. 하지만 어플리케이션의 설정을 특정 절대경로에 묶어놓는 다는 것을 확실히 알아둬야 할 것이다. 일반적으로는 이러한 절대경로를 간접적으로 유지하는 것을 선호한다. - 예를 들어 런타임에 JVM 시스템 프로퍼티에서 처리되는 "${...}" 플레이스 홀더를 이용하는 것이다.* |

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

| |
| ----- |
| ***!** 빈정의와 수동으로 제공된 싱글톤 인스턴스는 가능한한 빨리 등록하어 컨테이너가 자동연결과 다른 내부 동작을 올바르게 작동할 수 있도록 해야한다. 이미 존재하는 메타데이터와 싱글톤 인스턴스를 덮어씌우는 것은 어느정도 지원하지만 새로운 빈을 런타임에 추가하는 것(팩토리에 동시접근하는 것)은 공식적으로 지원하지 않고 동시 접근 예외나 빈 컨테이너의 동시성 문제, 혹은 두 문제 모두를 발생시킬수 있다.* |

<h4 id="beans-beanname">빈 이름 붙이기</h4>

모든 빈은 한개 이상의 식별자를 가지고 있다. 이러한 식별자는 해당 빈을 가지고 있는 컨테이너 내부에서는 고유해야한다. 빈은 보통은 한개의 식별자만 가진다. 하지만 여러개가 필요하다면 나머지는 별명으로 여겨진다.     
XML기반 설정 메타데이터에서 `id` 어트리뷰트, `name` 어트리뷰트, 혹은 두개 모두를 빈 식별자로 사용한다. `id` 어트리뷰트는 정확히 한개의 식별자를 가지게한다. 관례적으로 이러한 이름들은 문자와 숫자로 구성되어있다. ('myBean', 'someService' 등등) 하지만, 특수문자도 사용 가능하다. 빈에 별명을 설정하고자 한다면, `name` 어트리뷰트에 `,`, `;`, 공백으로 구분하여 작성할 수 있다. 스프링 3.1 이전에는 `id` 어트리뷰트는 문자제한이 있는`xsd:ID` 형식으로 정의되었다. 3.1부터 `xsd:string`형식으로 정의되었다. `id`의 고유성은 여전히 컨테이너에 의하여 강제된다. 하지만 더이상 XML 파서를 이용하지 않는다.     
`name`이나 `id`를 반드시 작성할 필요는 없다. `name`이나 `id`를 명시하지 않으면, 컨테이너가 고유한 이름을 만들어낸다. 하지만 `ref` 요소나 서비스 중개자 스타일을 이용하여 빈을 이름으로 참조하려면 이름을 작성해야한다. 이름을 작성하지 않는 동기는 [내부 빈](#beans-inner-beans)이나 [자동연결 협력자](#beans-factory-autowire)를 이용하기 위해서이다.

| 빈 이름 컨벤션 |
| --- |
| 빈 이름을 지을 때에 사용하는 컨벤션은 인스턴스의 필드 이름에 사용되는 표준 자바 컨벤션을 사용하는 것이다. 즉, 빈 이름은 소문자로 시작하여 camel-case를 사용한다. 이러한 이름의 예시는 `accountManager`, `accountService`, `userDao`, `loginController` 등등이 있다.     
일관성있게 이름을 지음으로써 설정을 읽기 쉽고 이해하기 쉽도록한다. 또한 스프링 AOP를 사용한다면 이름을 이용하여 어드바이스를 적용하는데 많은 도움이 된다. |

| |
| ----- |
| ***!** 클래스 패스에 컴포넌트 탐색시에 스프링은 이름이 없는 컴포넌트에 이름을 생성한다. 아래의 규칙은 먼저 소개되었다: 클래스 이름을 사용한고 첫 글자를 소문자로 한다. 하지만 첫번째와 두번째글자 모두 대문자라면 원래 모습이 유지된다. 이러한 규칙은 `java.beans.Introspector.decapitalize`에 정의된다.* |

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

| |
| ----- |
| ***!** 스프링 문서에서 "팩토리 빈(factory bean)"은 스프링 컨테이너에 의하여 설정되며 [인스턴스](#beans-factory-class-instance-factory-method) 혹은 [스태틱](#beans-factory-class-static-factory-method) 팩토리 메소드로 객체를 생성하는 빈을 말한다. 반면에 `FactoryBean`(대문자임을 주목하십시오) `스프링 고유의 `FactoryBean` 구현체 클래스를 의미한다.* |

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

| |
| ----- |
| ***!** `idref`요소의 `local` 어트리뷰트는 4.0 beans XSD부터 지원되지 않는다. 왜냐하며 `local`어트리뷰트는 더 이상 일반적인 `bean` 참조보다 추가적인 값을 제공하지 않기 때문이다. 4.0 스키마로 업그레이드 할 시, `ifref local`참조를 `idref bean`참조로 변경하십시오.* |

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

| |
| ----- |
| ***!** `idref`요소의 `local` 어트리뷰트는 4.0 beans XSD부터 지원되지 않는다. 왜냐하며 `local`어트리뷰트는 더 이상 일반적인 `bean` 참조보다 추가적인 값을 제공하지 않기 때문이다. 4.0 스키마로 업그레이드 할 시, `ifref local`참조를 `idref bean`참조로 변경하십시오.* |

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

| |
| ----- |
| ***!** p-네임스페이스는 표준 XML 형식보다 유연하지 못한다. 예를 들어 `Ref`로 끝나는 프로퍼티에 참조를 시킬 수 없다. XML 문서를 만드는 데에 있어 세 접근 방법을 모두 사용하지 않도록 팀원들과 충분히 상의하고 조심스럽게 진행하기를 추천한다.* |

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

| |
| ----- |
| ***!** XML 문법 때문에 인덱스 표현은 앞에 `_`가 필요하다. 왜냐하면 XML 어트리뷰트 이름은 숫자로 시작할 수 없기 때문이다(일부 IDE는 허용하지만). 인덱스 표현은 `constructor-arg>`에서도 사용가능하지만 단순히 순서만으로 충분하기에 잘 사용되지는 않는다.* |

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

| |
| ----- |
| ***!** `depends-on`어트리뷰트는 초기화 시 의존성을 표현할 수 있다. [싱글톤](#beans-factory-scopes-singleton)빈의 경우, 파괴 시 의존성 또한 표현 가능하다. `depends-on`으로 다른 빈에 의존하고 있는 빈이 먼저 파괴된다. 따라서 `depends-on`은 파괴 순서도 정할 수 있다.* |

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

| |
| ----- |
| ***!** `autowire-candidate`어트리뷰트는 오직 타입 기반 자동연결에만 영향을 미친다. 이름을 이용한 명시적 연결에는 영향을 끼치지 않는다. 결과적으로 이름을 이용한 자동연결은 이름만 맞다면 빈을 주입할 것이다.* |

빈 이름에 패턴 매칭을 이용하여 자동연결 후보 빈을 제한할 수 있다. 최 상위 `<beans/>`요소의 `default-autowire-candidates`어트리뷰트에 한개 이상의 패턴을 작성할 수 있다. 예를 들어, `Repository`로 끝나는 이름을 가진 빈만 자동연결 후보 빈으로 설정하고자 한다면 `*Repository`를 이용하면 된다. 여러개의 패턴을 작성하려면 반점`,`으로 분리되는 목록으로 작성하면 된다. 빈 정의에 `autowire-candidate`어트리뷰트를 명시적으로 작성하면 그 내용이 항상 우선시 된다. 이러한 빈에는 패턴 매칭 규칙이 적용되지 않는다.     
이러한 방법은 자동연결을 이용하여 다른 빈에 주입되는 것을 막고자 할 때 유용하다. 이 방법은 자동연결 제외된 빈이 자동연결을 이용하여 자신의 의존성을 주입받지 못한다는 것을 의미하지 않는다. 자동연결 제외된 빈이 다른 빈에 자동연결되지 않는다는 의미이다.

<h4 id="beans-factory-method-injection">메소드 주입</h4>

대부분의 어플리케이션에서 컨테이너에 있는 빈의 대다수는 [싱글톤](#beans-factory-scopes-singleton)이다. 싱글톤 빈이 다른 싱글톤 빈을 필요로 할 때, 혹은 싱글톤이 아닌 빈이 다른 싱글톤이 아닌 빈을 필요로 할 때, 일반적으로 하나의 빈을 다른 빈의 프로퍼티로 설정한다. 하지만 빈의 생존 주기가 다를 때는 문제가 된다. 싱글톤 빈 A가 싱글톤이 아닌(프로토타입) 빈 B를 A의 모든 메소드에서 사용한다고 하자. 컨테이너는 싱글톤 빈 A를 한번만 생성하고 프로퍼티 설정도 한번만 실행한다. 따라서 컨테이너는 빈 A에게 빈 B의 새로운 인스턴스를 매번 제공해줄 수 없다.     
해결책은 역흐름제어보다 선행된다. `ApplicationContextAware`인터페이스를 구현하여 [빈 A가 컨테이너를 인지하도록](#beans-factory-aware) 설정하고 [`getBean("b")`를 컨테이너로부터 호출하여](#beans-factory-client) 빈 A가 필요할 때마다 새로운 빈 B를 요청할 수 있다. 아래 이러한 접근 방법의 예시이다:
```java
// 상태를 가지는 커맨드를 사용하여 작업하는 클래스
package fiona.apple;

// 스프링 API를 임포트한다.
import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

public class CommandManager implements ApplicationContextAware {

    private ApplicationContext applicationContext;

    public Object process(Map commandState) {
        // 적합한 Command의 새 인스턴스를 가져온다.
        Command command = createCommand();
        // 새 Command 인스턴스에 상태를 설정한다.
        command.setState(commandState);
        return command.execute();
    }

    protected Command createCommand() {
        // 스프링 API에 의존성을 가진다.
        return this.applicationContext.getBean("command", Command.class);
    }

    public void setApplicationContext(
            ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }
}
```
위의 예시는 바람직하지 않다. 왜냐하면 비즈니스 코드가 스프링 프레임워크와 강하게 결합되어 있기 때문이다. 스프링 IoC 컨테이너의 발전된 기술인 메소드 주입은 이러한 경우를 깨끗하게 해결해준다.

| |
| ----- |
| ***!** [스프링 블로그](https://spring.io/blog/2004/08/06/method-injection/)에서 메소드 주입을 만든 동기를 볼 수 있다.* |

<h5 id="beans-factory-lookup-method-injection">Lookup 메소드 주입</h5>

Lookup 메소드 주입은 컨테이너가 관리하는 빈의 메소드를 덮어 써서 컨테이너의 다른 빈에서 결과를 반환하도록 해준다. Lookup은 일반적으로 [이전 장](#beans-factory-method-injection)에서 언급된 것 처럼 프로토타입 빈과 관련되있다. 스프링 프레임워크는 CGLIB 라이브러리의 바이트코드 생성을 사용하여 자식클래스를 동적으로 만들어내어 메소드를 덮어쓴다.

* 이 동적 자식클래스 생성이 동작하기 위해서는 클래스가 `final`이면 안되며 덮어쓰는 메소드가 `final`이면 안된다.
* `abstract`메소드를 가진 클래스를 단위테스트 하려면 `abstract`메소드를 구현한 자식클래스를 직접 만들어야한다.
* 컴포넌트 스캔이 되기 위해서는 구상 메소드가 있어야한다.
* 특히 중요한 한계점은 lookup 메소드는 팩토리 메소드와 함께 작동하지 않는 다는 것이다. 특히 설정 클래스의 `@Bean`메소드에서는 컨테이너가 인스턴스를 만드는 책임을 지지 않기 때문에 런타임 자식클래스 생성을 사용할 수 없다.

위 코드의 `CommandManager`클래스의 경우, 스프링 컨테이너가 동적으로 `createCommand()` 구현을 덮어쓴다. `CommandManager`클래스가 스프링 의존성을 가지지않도록 아래와 같이 바뀔 수 있다:
```java
package fiona.apple;

// 더 이상 스프링 임포트는 없다.

public abstract class CommandManager {

    public Object process(Object commandState) {
        // 적합한 Command의 새 인스턴스를 가져온다.
        Command command = createCommand();
        // 새 Command 인스턴스에 상태를 부여한다.
        command.setState(commandState);
        return command.execute();
    }

    // 이 메서드의 구현은 어디에 있을까?
    protected abstract Command createCommand();
}
```
주입되어야할 메소드를 가지고 있는 클라이언트 클래스(이경우 `CommandManager`클래스)에서 주입되어야하는 메소드는 아래의 형태여야 한다:
```xml
<public|protected> [abstract] <return-type> theMethodName(no-arguments);
```
만약에 메소드가 `abstract`라면 동적 생성된 자식 클래스가 해당 메소드를 구현할 것이다. 아니라면, 동적 생성된 자식 클래스가 구상 메소드를 덮어쓸 것이다. 아래의 예시를 보자:
```xml
<!-- 상태를 가지는 빈이 프로토타입으로 정의되었다. (싱글톤이 아니다) -->
<bean id="myCommand" class="fiona.apple.AsyncCommand" scope="prototype">
    <!-- 의존성 주입을 한다. -->
</bean>

<!-- commandProcessor uses statefulCommandHelper -->
<bean id="commandManager" class="fiona.apple.CommandManager">
    <lookup-method name="createCommand" bean="myCommand"/>
</bean>
```
`commandManager`빈은 `createCommand()`메소드를 새로운 `myCommand`빈이 필요할 때마다 호출한다. `myCommand`빈을 프로토타입 빈으로 정의한다. 만약
[싱글톤](#beans-factory-scopes-singleton)으로 설정하면 매번 같은 `myCommand`인스턴스가 반환될 것이다.     
어노테이션 기반 컴포넌트 모델에서는 `@Lookup`을 대신 사용하여 lookup메소드를 정할 수 있다. 아래는 예시이다:
```java
public abstract class CommandManager {

    public Object process(Object commandState) {
        Command command = createCommand();
        command.setState(commandState);
        return command.execute();
    }

    @Lookup("myCommand")
    protected abstract Command createCommand();
}
```
혹은 더 자연스럽게 lookup메소드의 리턴 타입에 따라 빈이 정해지도록 할 수 있다:
```java
public abstract class CommandManager {

    public Object process(Object commandState) {
        Command command = createCommand();
        command.setState(commandState);
        return command.execute();
    }

    @Lookup
    protected abstract Command createCommand();
}
```
일반적으로 어노테이션 기반 lookup메소드를 구상 메소드로 만든다. 왜냐하면 스프링의 컴포넌트 스캔은 추상 클래스를 기본적으로는 무시하기 때문이다. 이 한계를 명시적으로 등록되는 빈에는 적용되지 않는다.

| |
| ----- |
| ***!** 스코프가 다른 빈에 접근하는 또다른 방법은 `ObjectFactory`/`Provider`를 사용하는 방법이다. 자세한 내용은 [스코프 빈 의존성](#beans-factory-scopes-other-injection)을 보자.<br>`org.springframework.beans.factory.config`에 있는 `ServiceLocatorFactoryBean`이 유용할 수도 있다.* |

<h5 id="beans-factory-arbitrary-method-replacement">임의의 메소드 대체</h5>

메소드 주입의 덜 유용한 방법은 임의의 메소드를 다른 메소드로 변경하는 것이다. 이 장을 무시하고 넘어간 뒤, 이 장의 기능이 필요할 때 다시 읽어도 무방할 것이다.      
XML 기반 설정 메타데이터에서 `replaced-method`요소를 이용하여 메소드를 다른 것으로 교체할 수 있다. 아래의 클래스를 보자. `computeValue`를 덮어쓰고자 한다:
```java
public class MyValueCalculator {

    public String computeValue(String input) {
        // 실제 코드
    }

    // 다른 메소드
}
```
`org.springframework.beans.factory.support.MethodReplacer`인터페이스 구현체가 아래 예시처럼 새로운 메소드를 제공한다:
```java
/**
 * MyValueCalculator에 이미 존재하는 computeValue(String) 구현을 
 * 덮어쓰기 위해 사용된다.
 */
public class ReplacementComputeValue implements MethodReplacer {

    public Object reimplement(Object o, Method m, Object[] args) throws Throwable {
        // 인풋 값을 사용해 동작한 뒤, 결과를 반환한다.
        String input = (String) args[0];
        ...
        return ...;
    }
}
```
원본 클래스와 덮어쓸 메소드를 명시하는 빈 정의는 아래 예시처럼 생겼다:
```xml
<bean id="myValueCalculator" class="x.y.z.MyValueCalculator">
    <!-- 임의의 메소드 대체 -->
    <replaced-method name="computeValue" replacer="replacementComputeValue">
        <arg-type>String</arg-type>
    </replaced-method>
</bean>

<bean id="replacementComputeValue" class="a.b.c.ReplacementComputeValue"/>
```
`<replaced-mehtod/>`요소안에 한개 이상의 `<arg-type/>`요소를 사용하여 덮어쓸 메소드의 어규먼트를 명시할 수 있다. 클래스내에 해당 메소드가 오버로드되어 있다면 필수로 해야한다. 편의를 위하여 전체 타입 이름의 일부만 사용해도 가능하다. 예를 들어 `java.lang.String`에는 아래와 매칭된다:
```java
java.lang.String
String
str
```
많은 경우에서 어규먼트의 개수만으로 구분하기 충분하기 때문에 이러한 단축은 매칭되는 문장중 가장 짧은 문장을 적음으로써 글자수를 줄여줄 수 있다.

<h3 id="beans-factory-scopes">빈 스코프</h3>

빈 정의를 설정하는 것은 빈을 만드는데 사용되는 레시피를 만드는 것이다. 빈 정의가 레시피라는 생각은 매우 중요하다 왜냐하면 이 말은 인스턴스를 하나의 레시피로 만들 수 있다는 말이기 때문이다.     
빈 정의로 의존성과 설정 값을 통제할 수 있을 뿐만아니라 오브젝트의 스코프도 통제할 수 있다. 이러한 접근법은 매우 매우 강력하고 유연하다. 자바 클래스 수준에서 오브젝트의 스코프를 통제하는 대신 설정으로 오브젝트 스코프를 선택할 수 있기 때문이다. 빈은 여러 스코프중 하나를 선택하여 배포된다. 스프링 프레임워크는 6개의 스코프를 지원한다. 그 중 4개는 web-aware `ApplicationContext`를 이용하여 사용가능하다. 또한 [커스텀 스코프](#beans-factory-scopes-custom)도 만들 수 있다.     
아래의 표는 스프링 프레임워크가 지원하는 스코프이다:
###### 표 3. 빈 스코프

| 스코프 | 설명 |
| --- | --- |
| [싱글톤](#beans-factory-scopes-singleton) | (기본 값) 각 스프링 IoC 컨테이너에 단 한개 존재한다. |
| [prototype](#beans-factory-scopes-prototype) | 몇 개의 인스턴스 간에 가능한 스코프 |
| [request](#beans-factory-scopes-request) | HTTP 요청과 같은 생명 주기를 가지는 스코프. 이 말은 각각의 HTTP 요청은 해당 요청 고유의 인스턴스를 가지게 된다. web-aware `ApplicationContext`에서만 가능하다 |
| [session](#beans-factory-scopes-session) | HTTP `Session`과 같은 생명 주기를 가지는 스코프. web-aware `ApplicationContext`에서만 가능하다 |
| [application](#beans-factory-scopes-application) | `ServletCOntext`와 같은 생명 주기를 가지는 스코프. web-aware `ApplicationContext`에서만 가능하다 |
| [websocket](#websocket-stomp-websocket-scope) | `WebSocket`과 같은 생명 주기를 가지는 스코프. web-aware `ApplicationContext`에서만 가능하다 |

| |
| ----- |
| ***!** 스프링 3.0부터 쓰레드 스코프가 가능하다. 하지만 기본적으로 등록되지는 않는다. 더 자세한 정보는 [`SimpleThreadScope`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/support/SimpleThreadScope.html)문서를 보자. 다른 커스텀 스코프를 등록하는 방법은 [커스텀 스코프 사용하기](#beans-factory-scopes-custom-using)를 보자* |

<h4 id="beans-factory-scopes-singleton">싱글톤 스코프</h4>

스프링 컨테이너는 싱글톤 빈의 인스턴스 한개만 관리한다. 컨테이너에 해당 빈을 요청하면 그 특정 빈 인스턴스만 반환한다.     
다른 말로하면, 빈을 싱글톤 스코프로 정의했을 때, 스프링 IoC 컨테이너는 해당 빈 정의의 인스턴스를 단 하나만 생성한다. 이 싱글톤 빈은 다른 싱글톤 빈들과 함께 캐시에 저장되고 그 빈의 이름으로 요청이나 참조는 캐시된 객체를 반환받는다. 아래의 이미지가 싱글톤 스코프가 작동하는 그림이다:
<img src="/assets/spring-framework-reference/singleton.png"/>
스프링의 싱글톤 빈에 대한 개념은 Gang of Four (GoF) 패턴 책에 나온 싱글톤 패턴과는 다른다. 클래스로더가 특정 한 클래스의 인스턴스를 하나만 가질 수 있도록 하드코딩하는 방법이 GoF 싱글톤 패턴이다. 스프링의 싱글톤 스코프는 컨테이너마다, 빈 마다로 설명된다. 이말은 한 스프링 컨테이너에 특정 클래스의 빈 한개를 정의하면 스프링 컨테이너가 해당 빈 정의에 따라 단 한개의 빈 인스턴스를 만든다는 이야기이다. 스프링에서 싱글톤 스코프가 기본 스코프이다. XML로 싱글톤 빈을 정의하려면 아래와 같이 할 수 있다:
```xml
<bean id="accountService" class="com.something.DefaultAccountService"/>

<!-- 아래는 같은 정의이다. 따라서 중복되는 내용이다. (싱글톤 스코프는 기본 스코프이다.) -->
<bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
```

<h4 id="beans-factory-scopes-prototype">프로토타입 스코프</h4>

빈을 프로토타입으로 정의하면 해당 빈을 요청할 때마다 새로운 인스턴스가 생성된다. 즉, 빈이 다른 빈에 주입되거나 `getBean()`메소드를 호출할 때마다 새로운 인스턴스가 생성된다. 규칙으로서 상태를 가지는 빈을 프로토타입으로 상태가 없는 빈을 싱글톤으로 만들어야한다.     
아래의 그림이 프로토타입 스코프를 설명한다:
<img src="/assets/spring-framework-reference/prototype.png"/>
(데이터 접근 객체(DAO)는 일반적으로 프로토타입으로 설정되지 않는다. 왜냐하면 일반적인 DAO는 상태를 가지지 않기 때문이다. 싱글톤 그림을 재사용하는 것이 간편해서 위의 그림이 나왔다.)     
XML에서 프로토타입 빈을 아래와 같이 정의한다:
```xml
<bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
```
다른 스코프와는 다르게 프로토타입 빈의 생명주기의 일부는 스프링에 의해 통제되지 않는다. 컨테이너는 프로토타입 객체를 인스턴스화하고 설정하고 모아서 클라이언트에 전달한 뒤, 더 이상 해당 객체를 통제하지 않는다. 따라서 초기화 생명 주기 콜백 메소드는 빈의 스코프와 상관없이 호출되지만 프로토타입 빈의 경우, 파괴 생명 주기 콜백은 호출되지 않는다. 클라이언트 코드가 반드시 프로토타입 스코프 객체를 정리하고 프로토타입 빈이 가지고 있던 고비용의 자원을 풀어주어야한다. 스프링 컨테이너가 프토토타입 빈이 가지고 있던 자원을 풀어주도록 하려면 커스텀 [빈 포스트 프로세서](#beans-factory-extension-bpp)를 사용하여 정리되야 하는 빈의 참조를 가지고 있도록 할 수 있다.    
특정 관점에서 보면 프로토타입 빈과 관련된 스프링 컨테이너의 역할은 자바의 `new` 연산자를 대체한다. 해당 지점 이후의 모든 생명주기 관리는 클라이언트에 의하여 수행되야 한다. (스프링 컨테이너 내부의 빈 생명주기 관련 자세한 내용은 [생명주기 콜백](#beans-factory-lifecycle)을 보자.)

<h4 id="beans-factory-scopes-sing-prot-interaction">프로토타입 빈 의존성을 가지는 싱글톤 빈</h4>

프로토타입 빈에 의존하는 싱글톤 빈을 사용한다면, 인스턴스화 할때 의존성이 해결된다는 것을 알아야한다. 프로토타입 빈을 싱글톤 빈에 주입한다면 새로운 프로토타입 빈이 생성되 싱글톤 빈에 주입된다. 그 프로토타입 빈은 싱글톤 빈 하나에서 독점적으로 사용하는 인스턴스이다.      
하지만 싱글톤 빈이 런타임에 반복적으로 새로운 프로토타입 빈 인스턴스를 획득하길 원한다고 생각해보자. 스프링 컨테이너가 인스턴스화하여 싱글톤 빈을 만들고 의존성을 주입할 때 단 한번 의존성 주입이 일어나기 때문에 이러한 의존성 주입을 할 수 없다. 런타임에 새로운 프로토타입 빈 인스턴스가 필요하다면 [메소드 주입](#beans-factory-method-injection)을 읽어보자.

<h4 id="beans-factory-scopes-other">Request, Session, Application, WebSocket 스코프</h4>

`request`,`session`,`application`,`websocket`스코프는 (`XmlWebApplicationContext`와 같은) web-aware `ApplicationContext`를 사용해야만 이용가능하다. `ClassPathXmlApplicationContext`와 같은 일반적인 스프링 IoC 컨텍스트를 사용하면 `IllegalStateException`이 발생할 것이다.

<h5 id="beans-factory-scopes-other-web-configuration">초기 웹 설정</h5>

`request`,`session`,`application`,`websocket` 스코프의 빈을 지원하기 위해서 소소한 설정이 필요하다. (이 설정 단계는 `singleton`과 `prototype`과 같은 기본 스코프에는 필요하지 않다.)     
이러한 설정은 서블릿 환경에 따라 다르다.     
스프링 `DispatcherServlet`을 이용하는 스프링 웹 MVC를 이용하여 빈에 접근하면 특별한 설정이 필요하지 않다. `DispatcherServlet`이 이미 관련된 설정을 해준다.     
스프링 `DispatcherServlet` 외부에서 요청이 처리되는 서블릿 2.5 웹 컨테이너(예를 들면, JSF 혹은 Struts)를 사용한다면 `org.springframework.web.context.request.RequestContextListener``ServletRequestListener`를 등록해야한다. 서블릿 3.0이상에서는 `WebApplicationInitializer`인터페이스를 이용하여 프로그래밍 적으로 수행할 수 있다. 더 구버전의 컨테이너에서는 `web.xml` 파일에 아래의 정의를 추가하여야한다:
```xml
<web-app>
    ...
    <listener>
        <listener-class>
            org.springframework.web.context.request.RequestContextListener
        </listener-class>
    </listener>
    ...
</web-app>
```
리스너 설정에 문제가 있다면 스프링의 `RequestContextFilter`를 사용해 보아라. 필터 매핑은 웹 어플리케이션 환경에 달려있기에 적합하게 변경해줘야한다. 아래는 웹어플리케이션의 필터부분이다:
```xml
<web-app>
    ...
    <filter>
        <filter-name>requestContextFilter</filter-name>
        <filter-class>org.springframework.web.filter.RequestContextFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>requestContextFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    ...
</web-app>
```
`DispatcherServlet`,`RequestContextListener`,`RequestContextFilter`는 전부 같은 역할을 수행한다. 셋 모두 웹 요청을 수행하는 `Thread`에 객체를 제공해주는 역할을 한다. 이 방법으로 request, session 스코프 빈이 사용가능해진다.

<h5 id="beans-factory-scopes-request">Request 스코프</h5>

아래의 빈 정의 XML 설정을 생각해보자:
```xml
<bean id="loginAction" class="com.something.LoginAction" scope="request"/>
```
스프링 컨테이너는 `loginAction`빈 정의를 이용하여 매 HTTP 요청마다 `LoginAction`빈의 새로운 인스턴스를 생성한다. 이 말은, `loginAction`빈은 HTTP request 수준의 스코프를 가진다. 해당 인스턴스의 내부 상태를 얼마든지 변경할 수 있다. 왜냐하면 `loginAction`빈의 다른 인스턴스들은 이 변경의 영향을 받지 않기 때문이다. 이 인스턴스들은 각각의 웹 요청에 고유한 인스턴스이다. 웹 요청 수행이 완료되었을 때, request 스코브 빈은 버려진다.     
자바 설정이나 어노테이션 기반 컴포넌트를 사용할 때, `@RequestScope`어노테이션을 사용하여 컴포넌트를 `request` 스코프에 할당할 수 있다. 아래 예시이다:
```java
@RequestScope
@Component
public class LoginAction {
    // ...
}
```

<h5 id="beans-factory-scopes-session">Session 스코프</h5>

아래의 빈 정의 XML 설정을 생각해보자:
```xml
<bean id="userPreferences" class="com.something.UserPreferences" scope="session"/>
```
스프링 컨테이너는 `userPreferences`빈 정의를 이용하여 매 HTTP 요청마다 `UserPreferences`빈의 새로운 인스턴스를 생성한다. 이 말은, `userPreferences`빈은 HTTP `Session` 수준의 스코프를 가진다. request 스코프 빈처럼 해당 인스턴스의 내부 상태를 얼마든지 변경할 수 있다. 왜냐하면 `userPreferences`빈의 다른 인스턴스들은 이 변경의 영향을 받지 않기 때문이다. 이 인스턴스들은 각각의 HTTP `Session`에 고유한 인스턴스이다. HTTP `Session`이 버려질 때, 해당 HTTP `Session`에 스코프된 빈도 버려진다.     
자바 설정이나 어노테이션 기반 컴포넌트를 사용할 때, `@SessionScope`어노테이션을 사용하여 컴포넌트를 `Session` 스코프에 할당할 수 있다. 아래 예시이다:
```java
@SessionScope
@Component
public class UserPreferences {
    // ...
}
```

<h5 id="beans-factory-scopes-application">Application 스코프</h5>

아래의 빈 정의 XML 설정을 생각해보자:
```xml
<bean id="appPreferences" class="com.something.AppPreferences" scope="application"/>
```
스프링 컨테이너는 `appPreferences`빈 정의를 이용하여 전체 웹 어플리케이션에 `AppPreferences`빈의 새로운 인스턴스를 단 한개 생성한다. 이 말은, `appPreferences`빈은 `ServletContext` 수준의 스코프를 가지고 `ServletContext`의 어트리뷰트로 저장된다. 스프링 싱글톤 빈과 흡사하다 하지만 두가지 중요한 부분에서 다르다: 첫째로 Application 스코프 빈은 스프링 'ApplicationContext'마다가 아니라 `ServletContext`마다 싱글톤이다. 'ApplicationContext'는 여러개의 웹 어플리케이션을 가질 수 있다. 둘째로 Application 스코프 빈은 노출되어 있고 `Servletcontext`의 어트리뷰트로 접근 가능하다.     
자바 설정이나 어노테이션 기반 컴포넌트를 사용할 때, `@ApplicationScope`어노테이션을 사용하여 컴포넌트를 `Application` 스코프에 할당할 수 있다. 아래 예시이다:
```java
@ApplicationScope
@Component
public class AppPreferences {
    // ...
}
```

<h5 id="beans-factory-scopes-other-injection">스코프 빈 의존성</h5>

스프링 IoC 컨테이너는 객체(빈)를 인스턴스화하고 의존성을 주입한다. HTTP 요청 스코프 빈을 더 긴 스코프를 가지는 빈에 주입하고자 한다면 AOP 프록시를 주입하여야 할 것이다. 이 말은, 스코프를 가진 객체와 같은 인터페이스를 사용하면서 (HTTP 요청과 같은) 실제 스코프의 실제 인스턴스를 가져와 호출받은 메소드를 그 인스턴스에서 호출해주는 프록시 객체를 주입하여야 한다.

| |
| ----- |
| ***!** `singleton` 빈들 간에 `<aop:scoped-proxy/>`를 사용할 수 있다. 직렬화 가능한 중간 프록시를 이용하면 역직렬화 후 타겟 싱글톤 빈을 다시 획득가능해진다.<br>`prototype` 빈들에 `<aop:scoped-proxy/>`를 정의하면 프록시의 메소드를 호출할 때마다 새로운 인스턴스가 생성되어 해당 인스턴스에서 해당 메소드를 수행한다.<br>생명주기에 안전한 방법으로 더 짧은 생명주기를 가진 빈에 접근하는 방법으로는 스코프 프록시가 유일하지 않다. 인스턴스 참조를 가지고 있는 방법 대신에 `Objectfactory<MyTargetBean>`의 `getObject()`를 필요시에 호출하여 주입 순간을 정의할 수 있다.<br>`ObjectProvier<MyTargetBean>`을 이용할 수도 있다. 이는 `getIfAvailable`이나 `getIfUnique`등의 다른 접근 메소드를 제공한다.<br>JSR-330의 `Provider`는 `Provider<MyTargetBean>`으로 사용되며 `get()`을 호출하여 인스턴스에 접근한다. 자세한 내용은 [여기](#beans-standard-annotations)를 보자.* |

아래 설정에서 중요한 것은 단 한 줄이다. 하지만 해당 설정이 "왜", "어떻게" 적용되는지 이해하여야한다:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 프록시를 통하여 노출되는 HTTP 세션 스코프 빈 -->
    <bean id="userPreferences" class="com.something.UserPreferences" scope="session">
        <!-- 이 빈을 감싸는 프록시를 만들도록 컨테이너에 지시한다. -->
        <aop:scoped-proxy/>  ※1※
    </bean>

    <!-- 위의 빈의 프록시를 주입받는 싱글톤 스코프 빈 -->
    <bean id="userService" class="com.something.SimpleUserService">
        <!-- userPreferences 프록시 빈의 참조 -->
        <property name="userPreferences" ref="userPreferences"/>
    </bean>
</beans>
```
* ※1※ 이 줄이 프록시를 정의한다.  

이러한 프록시를 만들기 위해서 스코프 빈 정의 아래에 자식 `<aop:scoped-proxy/>`요소를 추가해야한다([생성할 프록시 타입 정하기](#beans-factory-scopes-other-injection-proxies)와 [XML 스키마 기반 설정](../spring-reference-core-9-appendix/#xsd-schemas)을 보자). `request`스코프, `session`스코프, 커스텀 스코프 빈 정의가 `<aop:scoped-proxy/>`요소가 필요한 이유는 무엇일까? 아래의 싱글톤 빈 정의를 무엇이 더 필요한지 생각하면서 대비해서보자.(아래의 `userPreferences`설정은 완전하지 않다는 것의 주의해라):

```xml
<bean id="userPreferences" class="com.something.UserPreferences" scope="session"/>

<bean id="userManager" class="com.something.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
``` 

위의 에시에서 싱글톤 빈(`userManager`)는 HTTP `Session` 스코프 빈(`userPreferences`)를 주입받는다. 여기서 주목할 부분은 `userManager`는 싱글톤 빈이라는 것이다: 컨테이너에서 단 한번 인스턴스화 되고 의존성 주입(이 경우 `userPreferences`빈)도 단 한번 일어난다. 이 말은 `userManager`빈이 한 개의 동일한 `userPreferences` 객체(처음 주입된 한개의 객체)와만 동작한다는 것이다.     
이 것은 우리가 생명주기가 짧은 빈과 긴 빈 사이에 일어나기를 원하는 것이 아니다(예를 들면, HTTP `Session`스코프 빈은 싱글톤 빈에 주입하는 경우). 우리가 원하는 것은 하나의 `userManager` 객체와 특정 HTTP `Session`에 종속적인 HTTP `Session`의 생명주기를 가지는 `userPreferences`객체이다. 따라서 컨테이너는 `UserPreferences`클래스와 완전히 같은 인터페이스를 가지는 객체(이상적으로는 `UserPreferences` 인스턴스)를 만든다. 이 객체는 실제 `UserPreferences`객체를 스코프 동작원리(HTTP request, `Session`, 등등)하에 가져온다. 컨테이너는 이 프록시 객체를 `userManager`빈에 주입하고 `userManager`빈은 이 객체가 프록시 객체인지 알 필요 없다. 이 예제에서 `UserManager`인스턴스가 `UserPrefernces`객체의 메소드를 호출할 때, 실제로는 프록시 객체의 메소드를 호출하는 것이다. 이 프록시는 실제 `UserPrefernces`객체를 (이 경우에는) HTTP `Session`에서 가져와 해당 메소드를 실제 `UserPrefernces` 객체에서 호출한다.     
따라서 `request-`, `session-scoped`빈을 주입할 때는 아래설정이 (정확하게 그리고 완전하게)필요하다. 아래는 예시이다:

```xml
<bean id="userPreferences" class="com.something.UserPreferences" scope="session">
    <aop:scoped-proxy/>
</bean>

<bean id="userManager" class="com.something.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```

<h6 id="beans-factory-scopes-other-injection-proxies">생성할 프록시 타입 정하기</h6>

기본적으로 스프링 컨테이너는 `<aop:scoped-proxy/>`요소가 표기된 빈의 프록시를 만들 때, CGLIB 기반 프록시를 만든다.     

```
CGLIB 프록시는 오직 퍼블릭 메소드 호출만 처리할 수 있다.
퍼블릭이 아닌 메소드를 호출하지 않아야 한다.
이 호출은 실제 객체의 메소드를 호출하지 않을 것이다.
```

`<aop:scoped-proxy/>`요소에 `proxy-target-class`를 `false`로 설정함으로써 이러한 스코프빈 프록시에 표준 JDK 인터페이스 기반 프록시를 생성하도록 설정할 수 있다. JDK 인터페이스 기반 프록시를 사용하면 추가적인 라이브러리를 요구하지 않는다. 하지만 이 말은 스코프 빈의 클래스는 반드시 하나 이상의 인터페이스를 구현해야 하며 이 빈을 참조하는 다른 빈들은 인터페이스를 통하여 해당 빈을 참조해야한다. 아래에 예시이다:
```xml
<!-- DefaultUserPreferences 는 UserPreferences 인터페이스 구현체이다. -->
<bean id="userPreferences" class="com.stuff.DefaultUserPreferences" scope="session">
    <aop:scoped-proxy proxy-target-class="false"/>
</bean>

<bean id="userManager" class="com.stuff.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```
클래스 기반 프록시와 인터페이스 기반 프록시에 대한 자세한 정보는 [프록시 작동원리](../spring-refernce-core-5-aop#aop-proxying)를 보자

<h4 id="beans-factory-scopes-custom">커스텀 스코프</h4>

빈 스코프는 확장가능하다. 새로운 스코프를 정의할 수도 있다. 또한 원래 있는 스코프를 재 정의할 수도 있다. 하지만 재정의의 경우, 별로 좋다고 여겨지지는 않고 `singleton`과 `prototype` 스코프는 재정의 불가능하다.

<h5 id="beans-factory-scopes-custom-creating">커스텀 스코프 만들기</h5>

커스텀 스코프를 스프링 컨테이너에 포함시키기 위해서는 이 장에서 소개될 `org.springframework.beans.factory.config.Scope`인터페이스를 구현해야한다. 스코프를 어떻게 구현하는지 알려면 스프링에서 제공하는 `Scope` 구현체와 [`Scope`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/beans/factory/config/Scope.html)자바독를 보아라. 이것들이 구현해야하는 것을 자세히 설명해줄 것이다.     
`Scope`인터페이스는 스코프에서 객체를 가져오거나 제거하거나 파괴하는 4개의 메소드가 있다.     
예를 들어 세션 스코프 구현체는 세션 스코프 빈을 반환한다(빈이 없다면 빈의 새로운 인스턴스를 만들어 나중에 참조하기 위해 세션에 묶은 뒤 해당 인스턴스를 반환한다). 아래의 메소드는 스코프에서 객체를 반환한다:
```java
Object get(String name, ObjectFactory<?> objectFactory)
```
예를들면 세션 스코프 구현체는 세션에서 세션 스코프 빈을 제거한다. 객체는 반드시 반환되야한다. 단 해당 이름을 가진 객체가 없으면 null을 반환할 수 있다. 아래의 메소드는 스코프에서 객체를 제거한다:
```java
Object remove(String name)
```
아래의 메소드는 스코프가 제거되거나 해당 객체가 제거될 때, 호출되어야 하는 콜백을 등록하는 메소드이다:
```java
void registerDestructionCallback(String name, Runnable destructionCallback)
```
파괴 콜백에 대한 더 자세한 내용은 [자바독](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/beans/factory/config/Scope.html#registerDestructionCallback)이나 스프링 스코프 구현체를 보아라.     
아래의 메소드는 스코프에서 회화적 식별자를 획득하는 메소드이다:
```java
String getConversationId()
```
이 식별자는 각각의 스코프마다 다르다. 세션 스코프 구현체에서의 경우, 이 식별자는 세션 식별자가 될 수 있다.

<h5 id="beans-factory-scopes-custom-using">커스텀 스코프 이용하기</h5>

커스텀 `Scope` 구현체를 작성한 이후, 스프링 컨테이너가 해당 스코프를 알 수 있도록 하여야한다. 아래의 메소드는 스프링 컨테이너에 새로운 `Scope`를 등록하는 중심 메소드이다:
```java
void registerScope(String scopeName, Scope scope);
```
이 메소드는 `ConfigurableBeanFactory`인터페이스에 정의되어 있고 이 인터페이스는 스프링에서 제공하는 `ApplicationContext`구현체의 대부분에서 `BeanFactory`프로퍼티를 통하여 사용가능하다.     
`registerScope(..)`메소드의 첫번째 어규먼트는 스코프에 고유한 이름이다. 스프링 컨테이너에서 사용되는 이러한 이름의 예시로는 `singleton`과 `prototype`이다. `reigisterScope(..)`메소드의 두번째 어규먼트는 등록하여 사용하고자 하는 `Scope`의 구현체 인스턴스이다.     
아래 예시처럼 커스텀 `Scope`구현체를 작성하고 등록했다고 가정하자.

| |
| ----- |
| ***!** 아래 예시는 스프링에 포함되있으나 기본적으로 등록되지는 않은 `SimpleThreadScope`를 사용한다. 커스텀 `Scope`구현체에서도 같은 방법으로 등록한다.* |

```java
Scope threadScope = new SimpleThreadScope();
beanFactory.registerScope("thread", threadScope);
```
커스텀 스코프 규칙을 따르는 빈 정의를 아래와 같이 만들 수 있다:
```xml
<bean id="..." class="..." scope="thread">
```
커스텀 `Scope`구현체를 프로그래밍적 등록만 할 수 있는 것은 아니다. `CustomScopeConfigurer`를 이용하여 아래 예시처럼 `Scope`등록을 선언적으로도 가능하다:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean class="org.springframework.beans.factory.config.CustomScopeConfigurer">
        <property name="scopes">
            <map>
                <entry key="thread">
                    <bean class="org.springframework.context.support.SimpleThreadScope"/>
                </entry>
            </map>
        </property>
    </bean>

    <bean id="thing2" class="x.y.Thing2" scope="thread">
        <property name="name" value="Rick"/>
        <aop:scoped-proxy/>
    </bean>

    <bean id="thing1" class="x.y.Thing1">
        <property name="thing2" ref="thing2"/>
    </bean>

</beans>
```

| |
| ----- |
| ***!** `FactoryBean`구현체에 `<aop:scoped-proxy/>`를 사용하면 팩토리빈 자체가 스코프를 가진다. `getObject()`로 반환되는 객체가 가지는 것이 아니다.* |

<h3 id="beans-factory-nature">빈의 특성 설정하기</h3>

스프링 프레임워크는 빈의 특성을 변경하는데 사용할 수 있는 많은 인터페이스를 제공한다. 이 장은 해당 인터페이스를 아래와 같이 나눈다:
* [생명주기 콜백](#beans-factory-lifecycle)
* [`ApplicationContextAware`와 `BeanNameAware`](#beans-factory-aware)
* [다른 `Aware` 인터페이스](#aware-list)

<h4 id="beans-factory-lifecycle">생명주기 콜백</h4>

컨테이너가 관리하는 빈의 생명주기와 상호작용하기 위해서는 스프링의 `InitializingBean`과 `DisposableBean` 인터페이스를 구현해야한다. 컨테이너는 전자에서는 `afterPropertiesSet()`, 후자에서는 `destory()`를 호출하여 빈이 초기화, 파괴시에 특정 행동을 하도록 한다.

| |
| ----- |
| ***!** 최신 스프링 어플리케이션에서 JSR-250 `@PostConstruct`와 `@PreDestory` 어노테이션이 생명주기 콜백을 가져오는 가장 좋은 방법으로 여겨진다. 이러한 빈을 사용하면 스프링 고유의 인터페이스에 묶이지 않는다. 자세한 내용은 [`@PostConstruct`와 `@PreDestory`사용하기]를 보자.     
JSR-250 어노테이션을 사용하지 않더라도 스프링 고유의 인터페이스에 묶이고 싶지 않다면 빈 정의 메타데이터의 `init-method`와 `destory-method`를 사용해보아라.* |

내부적으로 스프링 프레임워크는 `BeanPostProcessor`구현체를 사용하여 적합한 콜백 메소드를 찾아 호출하도록 한다. 스프링이 기본적으로 제공하지 않는 생명주기 행동이나 커스텀 특성이 필요하다면 `BeanPostProcessor`를 스스로 구현할 수 있다. 자세한 내용은 [컨테이너 확장 포인트](#beans-factory-extension)를 보아라.     

초기화 콜백, 파괴 콜백에 추가적으로 스프링에서 관리되는 빈은 `Lifecycle`인터페이스를 구현하여 이 객체들이 컨테이너의 생명주기에 따라 관리되는 생성과정, 파괴과정에 관여할 수 있다.     
생명 주기 콜백은 이 장에서 설명될 것이다.

<h5 id="beans-factory-lifecycle-initializingbean">콜백 정의하기</h5>

`org.springframework.beans.factory.InitializingBean`인터페이스는 컨테이너가 빈에 필요한 프로퍼티를 설정한 뒤, 빈이 초기화를 진행하도록 한다. `InitializingBean`인터페이스는 한 개의 메소드를 명시한다:
```java
void afterPropertiesSet() throws Exception;
```
`InitializingBean`인터페이스를 사용하는 것은 추천하지 않는다. 왜냐하면 불필요하게 스프링 프레임워크와 코드가 결합하기 때문이다. 대신 [`@PostConstruct`](#beans-postconstruct-and-predestroy-annotations)를 사용하거나 POJO 초기화 메소드를 명시하는 것을 추천한다. XML 기반 설정 메타데이타에서는 `init-method`어트리뷰트에 void, 어규먼트가 없는 메소드의 이름을 명시하여 사용할 수 있다. 자바 설정에서는 `@Bean`의 `initMethod`어트리뷰트를 사용할 수 있다. [생명주기 콜백 받기](#beans-java-lifecycle-callbacks)를 보자. 아래는 예시이다:
```xml
<bean id="exampleInitBean" class="examples.ExampleBean" init-method="init"/>
```
```java
public class ExampleBean {

    public void init() {
        // 초기화 작업을 수행한다.
    }
}
```
위의 예시는 아래의 예시와 거의 같은 동작을 수행한다:
```xml
<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>
```
```java
public class AnotherExampleBean implements InitializingBean {

    @Override
    public void afterPropertiesSet() {
        // 초기화 작업을 수행한다.
    }
}
```
하지만 두 예시 중 위의 예시는 스프링 프레임워크와 코드가 결합하지 않았다.

<h5 id="beans-factory-lifecycle-disposablebean">소멸자 콜백</h5>

`org.springframework.beans.factory.DisposableBean`인터페이스를 구현하여 해당 빈을 가지고 있는 컨테이너가 파괴될 때 호출될 콜백을 가지도록 할 수 있다. `DisposableBean`인터페이스는 한개의 메소드를 명시한다:
```java
void destroy() throws Exception;
```
`DisposableBean`을 사용하는 것은 추천하지 않는다. 왜냐하면 불필요하게 스프링 프레임워크와 코드가 결합하기 때문이다. 대신 [`@PreDestory`](#beans-postconstruct-and-predestroy-annotations)를 사용하거나 POJO 초기화 메소드를 명시하는 것을 추천한다. XML 기반 설정 메타데이타에서는 `<bean/>`요소의  `destory-method`어트리뷰트에 void, 어규먼트가 없는 메소드의 이름을 명시하여 사용할 수 있다. 자바 설정에서는 `@Bean`의 `destoryMethod`어트리뷰트를 사용할 수 있다. [생명주기 콜백 받기](#beans-java-lifecycle-callbacks)를 보자. 아래는 예시이다:
```xml
<bean id="exampleInitBean" class="examples.ExampleBean" destroy-method="cleanup"/>
```
```java
public class ExampleBean {

    public void cleanup() {
        // 커넥션 풀 정리 등 파괴할 작업을 수행한다.
    }
}
```
위의 예시는 아래의 예시와 거의 같은 동작을 수행한다:
```xml
<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>
```
```java
public class AnotherExampleBean implements DisposableBean {

    @Override
    public void destroy() {
        // 커넥션 풀 정리 등 파괴할 작업을 수행한다.
    }
}
```
하지만 두 예시 중 위의 예시는 스프링 프레임워크와 코드가 결합하지 않았다.

| |
| ----- |
| ***!** `<bean>`요소의 `destory-method`에 스프링이 자동으로 퍼블릭 `close`나 `shutdown`메소드를 찾아 암시적으로 값을 설정하도록 수 있다. (`java.lang.AutoClosable`이나 `java.io.Closeable`을 구현하면 해당한다.) 또한 `<beans>`요소의 `default-destroy-method`어트리뷰트에 `(inferred)`값을 설정하여 전체 빈에 암시적 설정을 할 수 있다([기본 초기화 메소드, 파괴 메소드](#beans-factory-lifecycle-default-init-destroy-methods)를 보자). 자바 기반 설정에서 위 설정이 기본 설정이다.* |

<h5 id="beans-factory-lifecycle-default-init-destroy-methods">기본 초기화 메소드, 파괴 메소드</h5>

스프링 고유의 `InitializingBean`과 `DisposableBean`콜백 인터페이스를 이용하지 않고 초기화 메소드, 파괴 메소드를 작성한다면 보통은 `init()`, `initialize()`, `dispose()` 등의 이름으로 작성할 것이다. 이상적으로는 그러한 이름은 프로젝트 전체에서 표준이 있을 것이고 모든 개발자들의 같은 이름을 사용하여 일관성이 유지될 것이다.     
스프링 컨테이너가 모든 빈에서 그러한 이름을 가진 메소드를 찾아보도록 설정할 수 있다. 어플리케이션 개발자가 모든 빈에 `init-metho"init"`을 작성하지 않아도 `init()`이라는 초기화 콜백을 사용하도록 작성할 수 있다. 스프링 IoC 컨테이너는 빈이 생성될 때 ([이전에 설명된](#beans-factory-lifecycle) 표준 생명주기 콜백에 따라) 해당 메소드를 호출한다. 또한 이 설정은 초기화 콜백, 파괴 콜백에 같은 이름짓기 규약을 따르도록 강요한다.     
초기화 콜백이름이 `init()`이고 파괴 콜백이름이 `destory()`라고 가정하자. 그렇다면 클래스들의 형태는 아래와 닮아 있을 것이다:
```java
public class DefaultBlogService implements BlogService {

    private BlogDao blogDao;

    public void setBlogDao(BlogDao blogDao) {
        this.blogDao = blogDao;
    }

    // (당연하게도) 초기화 콜백
    public void init() {
        if (this.blogDao == null) {
            throw new IllegalStateException("The [blogDao] property must be set.");
        }
    }
}
```
이 클래스를 아래와 같이 사용할 수 있다:
```xml
<beans default-init-method="init">

    <bean id="blogService" class="com.something.DefaultBlogService">
        <property name="blogDao" ref="blogDao" />
    </bean>

</beans>
```
최상위 `<beans/>`요소에 `default-init-method`를 설정하면 스프링 IoC 컨테이너가 `init`이라는 이름의 메소드를 인식하여 초기화 콜백으로 사용한다. 빈이 생성되어 수집될 때, 빈이 해당 이름의 메소드를 가지고 있다면 적절한 때에 호출해 줄 것이다.     
(XML의 경우) 최상위 `<beans/>`요소에 `default-destory-method`를 이용하여 파괴 콜백도 비슷하게 설정할 수 있다.      
이름짓기 규약과 다른 이름의 콜백 메소드를 가진 빈이 있을 경우, `<bean/>`에 `init-method`와 `destory-method`를 설정하여 기본 설정을 덮어쓸수 있다.     
스프링 컨테이너는 초기화 콜백이 의존성을 주입받은 직후 호출되는 것을 보장한다. 따라서 초기화 콜백은 빈 자체에 호출된다. 즉 AOP 인터셉터 등등이 적용되지 않은 빈에서 호출된다는 말이다. 빈이 생성되고나서 (예를 들면) AOP 프록시 인터셉터가 적용된다. 목표 빈과 프록시가 따로 정의된다면 빈 자체와도 상호작용가능하다. 게다가 인터셉터에 `init` 메소드를 적용하는 것을 일관적이지 못하다. 왜냐하면 목표 빈의 생명주기와 프록시나 인터셉터가 강하게 결합되며 목표 빈 자체와 상호작용할 때, 올바르지 못한 설정이 남기 때문이다.

<h5 id="beans-factory-lifecycle-combined-effects">생명주기 작동원리 조합하기</h5>

스프링 2.5부터 빈 생명주기를 통제하는 3가지 방법이 있다:
* `InitializingBean`과 `DisposableBean` 콜백 인터페이스
* 커스텀 `init()`, `destroy()`메소드
* [`@PostConstruct`와 `@PreDestory` 어노테이션](#beans-postconstruct-and-predestroy-annotations). 이 방법들을 섞어서 사용할 수 있다.

| |
| ----- |
| ***!** 여러개의 생명주기 설정이 서로 다른 메소드이름으로 설정된다면 각각의 메소드들이 아래의 순서로 모두 동작할 것이다. 하지만 같은 이름으로 설정되면 한번만 동작할 것이다. 자세한 설명은 [앞 장](#beans-factory-lifecycle-default-init-destroy-methods)에 설명되어 있다.* |

하나의 빈에 여러 생명주기 설정이 다른 메소드 이름으로 존재한다면 아래 순서로 호출될 것이다:
1. `@PostConstruct` 어노테이션으로 설정된 메소드
2. `InitializingBean`콜백 인터페이스의 `afterPropertiesSet()`
3. 커스텀 설정된 `init()` 메소드

파괴 메소드도 같은 순서로 호출된다:
1. `@PreDestory` 어노테이션으로 설정된 메소드
2. `DisposableBean`콜백 인터페이스의 `destroy()`
3. 커스텀 설정된 `destroy()` 메소드

<h5 id="beans-factory-lifecycle-processor">Startup, Shutdown 콜백</h5>

`Lifecycle`인터페이스는 생명주기에 필요한 메소드를 정의한다 (예를 들면 시작 시, 종료시):

```java
public interface Lifecycle {

    void start();

    void stop();

    boolean isRunning();
}
```

스프링에서 관리되는 객체는 `Lifecycle`인터페이스를 구현할 수 있다. 그렇게 하면 `ApplicationContext`가 시작 신호나 종료 신호를 받을 때, 컨텍스트 내에 정의된 모든 `Lifecycle`구현체의 메소드를 호출한다. 아래에 나온 `LifecycleProcessor`에 위임하여 동작이 이뤄진다:

```java
public interface LifecycleProcessor extends Lifecycle {

    void onRefresh();

    void onClose();
}
```

`LifecycleProcessor` 자체도 `Lifecycle`인터페이스를 확장한 것이라는 것에 주의하십시오. 컨텍스트가 새로 고침될 때, 종료될 때, 동작하는 두개의 메소드를 추가로 가진다.

| |
| ----- |
| ***!** 일반 `org.springframework.context.Lifecycle`인터페이스는 시작 종료시에만 동작하며 자동 시작하는 새로고침시에는 동작하지 않는다. 특정 빈의 자동 시작시에도 통제를 하고 싶다면 `org.springframework.context.SmartLifecycle`을 구현하는 것을 고려해보십시오.* |

시작, 종료 메소드 호출 순서가 중요할 수 있다. 두 객체 간에 의존관계가 있다면 의존하는 객체가 늦게 시작하고 먼저 종료한다. 하지만 직접적인 의존관계가 알려지지 않은 경우가 있다. 특정 타입의 객체가 다릍 타입의 객체보다 빨리 시작해야한다는 것만 알고 있다고 하자. 이런 경우에 `SmartLifecycle` 인터페이스를 정의하는 것이 방법이 될 수 있다. `SmartLifecycle`의 부모 인터페이스인 `Phased`인터페이스의 `getPhase()`를 정의하면 된다. 아래는 `Phased`인터페이스의 정의이다:

```java
public interface Phased {

    int getPhase();
}
```

아래는 `SmartLifecycle`의 정의이다:

```java
public interface SmartLifecycle extends Lifecycle, Phased {

    boolean isAutoStartup();

    void stop(Runnable callback);
}
```

시작시에 가장 낮은 페이즈의 객체가 먼저 시작한다. 종료시에는 반대로이다. 따라서 `SmartLifecycle`을 구현한 객체의 `getPhase()`메소드에서 `Integer.MIN_VALUE`를 반환한다면 가장 먼저 시작해서 가장 늦게 종료될 것이다. 반대로 `Integer.MAX_VALUE`를 반환하는 객체는 가장 늦게 시작하여 가장 먼저 종료될 것이다. 페이즈 값을 정할 때, `SmartLifecycle`을 구현하지 않은 "일반적인" `Lifecycle`객체의 페이즈는 `0`이다. 따라서 음수 페이즈는 일반적인 컴포넌트보다 먼저 시작하고 늦게 종료되야 한다는 뜻이다. 양수일 때는 반대이다.     
`SmartLifecycle`에 정의된 종료 메소드는 콜백을 파라메터로 수용한다. 어떠한 구현체라도 콜백의 `run()`메소드를 종료과정이 끝난뒤 호출해야한다. 이렇게 함으로써 비동기 종료를 가능하게 한다. 왜냐하면 `LifecycleProcessor`인터페이스의 기본 구현체인 `DefaultLifecycleProcessor`는 각각 페이즈의 객체들의 콜백 수행시 정해진 시간만 기다린다. 페이즈별 기다리는 시간의 기본값은 30초이다. `lifecycleProcessor`라는 이름의 빈을 정의하여 기본 생명주기 프로세서 인스턴스를 대신할 수 있다. 기다리는 시간만 바꾸고 싶다면 아래의 설정으로 충분하다:

```xml
<bean id="lifecycleProcessor" class="org.springframework.context.support.DefaultLifecycleProcessor">
    <!-- 밀리초 단위 기다리는 시간 -->
    <property name="timeoutPerShutdownPhase" value="10000"/>
</bean>
```

위에서 언급했듯이, `LifecycleProcessor`인터페이스는 컨텍스트의 새로고침시, 종료시에 콜백 메소드 또한 정의한다. 컨텍스트 종료시 콜백은 `stop()`메소드 호출과 같은 종료 과정을 작동시킨다. 하지만 '새로고침시' 콜백은 `SmartLifecycle`빈의 다른 특성을 가능하게 한다. 콘텍스트가 새로고침시에 콜백이 호출된다. 그 시점에 기본 생명주기 프로세서는 각 `SmartLifecycle` 객체의 `isAutoStartup()`메소드에서 반환하는 불리언 값을 확인한다. 만약 `true`라면 객체는 `start()`메소드의 호출을 기다리는 것이 아니라 그 시점에서 동작한다 (컨텍스트 새로고침과 다르게 컨텍스트 시작은 자동적으로 동작하지 않는다). 위에서 설명했듯이 `phase` 값과 다른 의존관계가 시작 순서를 결정한다.

<h5 id="beans-factory-shutdown">웹 어플리케이션이 아닌 환경에서 스프링 IoC 컨테이너 아름답게 종료하기</h5>

| |
| ----- |
| ***!** 이 장은 웹 어플리케이션이 아닌 환경에만 적용되는 내용을 담고 있다. 스프링의 웹 기반 `ApplicationContext` 구현체는 웹 어플리케이션이 종료할 때, 아름답게 종료되는 코드를 이미 가지고 있다.* |

웹 환경이 아닌 환경에서 스프링 IoC 컨테이너를 사용한다면 JVM에 종료 훅을 등록하여야한다. 아름다운 종료를 보장하며 자원을 해제하도록 싱글톤 빈에 파괴 메소드를 호출하여 줄것이다. 물론 파괴 콜백들을 구현하고 올바르게 설정 하여야한다.     
종료 훅을 등록하려면 `ConfigurableApplicationContext`인터페이스에 정의되어 있는 `registerShutdownHook()`을 호출하여야 한다:
```java
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public final class Boot {

    public static void main(final String[] args) throws Exception {
        ConfigurableApplicationContext ctx = new ClassPathXmlApplicationContext("beans.xml");

        // 위의 컨텍스트에 셧다운 훅을 추가한다.
        ctx.registerShutdownHook();

        // 어플리케이션이 여기서 동작한다.

        // 메인 메소드가 종료된다. 훅은 어플리케이션이 종료되기전에 호출된다.
    }
}
```

<h4 id="beans-factory-aware">ApplicationContextAware, BeanNameAware</h4>

`ApplicationContext`가 `org.springframework.contet.ApplicationContextAware`인터페이스를 구현한 객체 인스턴스를 생성할 때, 인스턴스는 `ApplicationContext`의 참조를 제공받는다. 아래 `ApplicationContextAware`의 정의이다:
```java
public interface ApplicationContextAware {

    void setApplicationContext(ApplicationContext applicationContext) throws BeansException;
}
```
따라서 빈은 자신을 만든 `ApplicationContext`를 `ApplicationContext`인터페이스를 이용하거나 (추가적인 기능이 있는 `ConfigurableApplicationContext`와 같은) 하위 클래스로 캐스팅하여 조작할 수 있다. 한가지 사용법은 프로그래밍적으로 다른 빈을 검색하는 것이다. 종종 이 기능은 유용하다. 하지만 일반적으로는 하지 않아야한다. 왜냐하면 코드와 스프링이 결합하고 역흐름 제어 방법을 따르지 않기 때문이다. 다른 `ApplicationContext`의 사용법은 파일 자원에 접근하거나 어플리케이션 이벤트를 발행하거나 `MessageSource`에 접근하는 사용법이다. 이러한 추가적인 기능은 [`ApplicationContext`의 추가적인 사용법](#context-introduction)에서 설명되어 있다.     
자동연결은 `ApplicationContext`의 참조를 획득하는 또 다른 방법이다. ([자동연결 협력자](#beans-factory-autowire)에서 설명된) `constructor`와 `byType`자동연결 모드에서 `ApplicationContext`타입의 의존성을 생성자 어규먼트나 세터 메소드 파라메터로 제공할 수 있다. 필드와 여러 파라메터를 가진 메소드에 자동 연결하는 기능을 포함한 더 유연한 기능을 위해 어노테이션기반 자동연결 기능을 사용하여라. 이렇게 하면 `@Autowired`가 있는 메소드, 생성자, 필드에 `ApplicationContext`가 제공될 것이다. 더 자세한 정보는 [`@Autowired`사용하기](#beans-autowired-annotation)를 보십시오.     
`ApplicationContext`가 `org.springframework.beans.factory.BeanNameAware`인터페이스를 구현한 클래스를 생성할 때, 빈 이름이 제공될 것이다. 아래 BeanNameAware 인터페이스의 정의이다:
```java
public interface BeanNameAware {

    void setBeanName(String name) throws BeansException;
}
```
빈 프로퍼티가 설정된 후, `InitializingBean`이나 `afterPropertiesSet`이나 커스텀 초기화 콜백이 호출되기 전에 이 콜백이 호출된다.

<h4 id="awar-list">다른 Aware 인터페이스</h4>

[이전](#beans-factory-aware)에 이야기된 `ApplicationContextAware`와 `BeanNameAware`이외에 스프링은 빈이 컨테이너를 알수 있는 `Aware`콜백 인터페이스를 제공한다. 

##### 표 4. Aware 인터페이스

| 이름 | 주입되는 의존성 | 설명 링크 |
| ---- | ---- | ---- |
| `ApplicationContextAware` | `ApplicationContext` | [`ApplicationContext`와 `BeanNameAware`](#beans-factory-aware) |
| `ApplicationEventPublisherAware` | `ApplicationContext`를 감싸는 이벤트 발행자 | [`ApplicationContext`의 추가적인 기능](#context-introduction) |
| `BeanClassLoaderAware` | 빈 클래스를 로드하는 클래스 로더 | [빈 초기화 하기](#beans-factory-class) |
| `BeanFactoryAware` | `BeanFactory` | [`ApplicationContext`와 `BeanNameAware`](#beans-factory-aware) |
| `BeanNameAware` | 빈 이름 | [`ApplicationContext`와 `BeanNameAware`](#beans-factory-aware) |
| `BootstrapContextAware` | `BootstrapContext`. 일반적으로 JCA-aware `ApplicationContext`에서만 사용 가능하다 | [JCA CCI](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/integration.html#cci)
| `LoadTimeWeaverAware` | 로드 타임에 클래스 정의를 수행하는 위버 | [스프링 프레임워크에서 AspectJ를 이용하여 로드타임 위빙하기](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#aop-aj-ltw) |
| `MessageSourceAware` | 메세지를 처리하는 전략 설정 | [`ApplicationContext`의 추가적인 기능](#context-introduction) |
| `NotificationPublisherAware` | 스프링 JMX 알림 발행자 | [알림](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/integration.html#jmx-notifications) |
| `ResourceLoaderAware` | 저수준 자원에 접근하는 로더 | [리소스](../spring-reference-core-2-resources#resources) | 
| `ServletConfigAware` | 현재 `ServletConfig`. 웹-aware `ApplicationContext`에서만 사용 가능하다 | [스프링 MVC](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/web.html#mvc) |
| `ServletContextAware` | 현재 `ServletContext`. 웹-aware `ApplicationContext`에서만 사용 가능하다 | [스프링 MVC](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/web.html#mvc) |

이러한 인터페이스를 사용하는 것은 스프링 API와 코드를 강하게 결합시키며 역제어흐름 스타일을 따르지 않는 다는 것에 주의하여야 한다. 결과적으로, 컨테이너에 프로그래밍적으로 접근할 필요가 있는 인프라빈에 사용하기를 추천한다.

<h3 id="beans-child-bean-definitions">빈의 정의 상속</h3>

빈 정의는 생성자 어규먼트, 프로퍼티 값, 초기화 메서드나 정적 팩토리 메서드와 같은 컨테이너 종속 정보 등 등 많은 설정정보를 가지고 있다. 자식 빈 정의는 부모 빈 정의를 상속한다. 자식 정의는 값을 재정의 하거나 추가할 수 있다. 부모, 자식 빈 정의를 사용하면 글자 수를 줄일 수 있다. 효율적으로 템플릿화 하는 방법이다.

`AppicationContext` 인터페이스를 프로그래밍적으로 다루고 있다면, `ChildBeanDefinition` 클래스는 자식 빈 정의를 나타낸다. 대부분의 유저는 이 정도 레벨에서 프로그래밍하지 않는다. 대신 `ClassPathXmlApplicationContext`와 같은 클래스에서 선언적으로 빈을 정의한다. XML기반 빈 메타데이터를 사용한다면 `parent` 요소에 부모 빈을 명시함으로서 자식 빈정의를 표시할 수 있다. 아래의 예시가 방금 언급한 방법을 보여준다 :

```xml
<bean id="inheritedTestBean" abstract="true"
        class="org.springframework.beans.TestBean">
    <property name="name" value="parent"/>
    <property name="age" value="1"/>
</bean>

<bean id="inheritsWithDifferentClass"
        class="org.springframework.beans.DerivedTestBean"
        parent="inheritedTestBean" init-method="initialize">  
    <property name="name" value="override"/>
    <!-- age 프로퍼티의 값은 부모로 부터 상속되어 1이다. -->
</bean>
```
#### `parent` 요소를 주의깊게 보십시오.

자식 빈 정의는 부모 빈 정의에 표시된 빈의 클래스를 사용한다. 하지만 재정의할 수 있다. 재정의할 경우, 자식 빈의 클래스는 부모 빈의 클래스와 호환되어야 한다.(부모의 프로퍼티 값을 허용해야 한다)

자식 빈 정의는 스코프, 생성자 어규먼트 값, 프로퍼티, 메소드를 상속받으며 새로운 값을 추가할 수 있다. 스코프, 초기화 메소드, 파괴 메소드, `정적` 팩토리 메소드는 재정의 할 수 있다.

다음의 설정은 상속받지 않고 자식 빈 정의를 따른다: depends on, autowire mode, dependency check, singleton, lazy init

아래의 예시는 부모 빈 정의를 `abstract`요소를 사용하여 추상 정의로 표시한다. 부모 빈의 클래스를 명시하지 않는다면 아래 예시처럼 `abstract`요소가 필수이다:

```xml
<bean id="inheritedTestBeanWithoutClass" abstract="true">
    <property name="name" value="parent"/>
    <property name="age" value="1"/>
</bean>

<bean id="inheritsWithClass" class="org.springframework.beans.DerivedTestBean"
        parent="inheritedTestBeanWithoutClass" init-method="initialize">
    <property name="name" value="override"/>
    <!-- age 프로퍼티의 값은 부모로 부터 상속되어 1이다. -->
</bean>
```

부모 빈은 인스턴스화 되지 않는다. 부모 빈은 완전히 설정되지 않았고 `abstract`로 표시되어 있기 때문이다. 빈 정의가 `abstract`하면 템플릿 빈 정의로서 자식 빈을 설정하는 데에만 사용된다. 다른 빈에 ref 프로퍼티에 명시를 하거나 부모 빈 ID를 이용하여 `getBean()`을 호출하는 방법으로 `abstract` 부모 빈 자체를 사용하려하면 에러가 날것이다. 비슷하게 컨테이너 내부의 `preInstantiateSingletons()` 메소드는 추상 빈 정의를 무시할 것이다.

| |
| ----- |
| ***!** `ApplicationContext`는 기본적으로 모든 싱글톤 빈을 초기에 인스턴스화 시킨다. 따라서 템플릿으로 사용하려는 (부모) 빈 정의에 `abstract` 요소를 설정하는 것은 (적어도 싱글톤 빈에서는) 중요하다. 그렇지 않으면 어플리케이션 컨텍스트는 `abstract` 빈을 인스턴스화 하려할 것이다.* |

<h3 id="beans-factory-extension">컨테이너 확장 시점</h3>

일반적으로 어플리케이션 개발자는 `ApplicationContext` 구현체의 자식 클래스가 필요하지 않다. 대신 스프링 IoC 컨테이너는 특정 통합 인터페이스의 구현체를 연결함으로써 확장 가능하다. 아래 몇 섹션을 이러한 통합 인터페이스를 설명한다.

<h4 id="beans-factory-extension-bpp">BeanPostProcessor를 이용하여 빈 커스터마이징 하기</h4>

`BeanPostProcessor` 인터페이스는 콜백 메서드를 정의한다. 이 콜백 메서드에 당신만의 (컨테이너의 기본 로직을 재정의하는) 초기화 로직, 의존성 해소 로직 등 등을 정의할 수 있다. 만약 스프링 컨테이너가 인스턴스화, 설정, 빈 초기화를 마친 이후 커스텀 로직을 추가하고 싶다면 `BeanPostProcessor` 구현체를 하나 혹은 여러개 연결할 수 있다.

여러개의 `BeanPostProcessor`인스턴스를 설정할 수 있다. `BeanPostProcessor`인스턴스의 실행 순서를 `order` 프로퍼티를 이용하여 정할 수 있다. `BeanPostProcessor`구현체가 `Ordered` 인터페이스를 구현할 경우에만 설정 가능하다. 자신만의 `BeanPostProcessor`를 만든다면 `Ordered`인터페이스 또한 구현하는 것이 좋을 것이다. 추가적인 자세한 내용은, [`BeanPostProcessor`](https://docs.spring.io/spring-framework/docs/5.2.8.RELEASE/javadoc-api/org/springframework/beans/factory/config/BeanPostProcessor.html)와 [`Ordered`](https://docs.spring.io/spring-framework/docs/5.2.8.RELEASE/javadoc-api/org/springframework/core/Ordered.html)인터페이스의 자바독을 보십시오. [`BeanPostProcessor` 인스턴스의 프로그래밍적 등록](#Programmatically_registering_BeanPostProcessor) 또한 보십시오.


| |
| ----- |
| ***!** `BeanPostProcessor` 인스턴스는 빈 이나 객체 인스턴스에 동작한다. 이 말은 스프링 IoC 컨테이너가 빈은 인스턴스화 한 뒤 `BeanPostProcessor`가 작업을 수행한다는 말이다.<br>`BeanPostProcessor`인스턴스는 컨테이너 스코프를 가진다. 컨테이너 계층을 활용할 때만 관련이있는 정보이다. `BeanPostProcessor`를 한 컨테이너에서 정의하면 그 컨테이너에 있는 빈만 후처리할 것이다. 다시 말하자면 `BeanPostProcessor`는 다른 컨테이너에 등록된 빈들의 후처리를 하지 않을 것이다. 두 컨테이너가 같은 계층구조상에 있더라고 마찬가지이다.<br>빈 정의를 수정하기 위해서는 `BeanFactoryPostProcessor`를 사용하면 된다. 자세한 설명은 [`BeanFactoryPostProcessor`를 이용하여 설정 메타데이터 설정하기](#beans-factory-extension-factory-postprocessors)에 설명되어 있다.* |

`org.springframework.beans.factory.config.BeanPostProcessor` 인터페이스는 정확히 두개의 콜백 메소드로 구성되어있다. 이런 클래스가 컨테이너에 후 처리기로 등록되면 컨테이너에서 생성되는 각각의 빈 인스턴스마다 후처리기 콜백이 실행된다. 이 콜백은 컨테이너의 (`InitializingBean.afterPropertiesSet`과 같은) 초기화 메서드나 `init` 메서드 보다 먼저 실행된며 빈의 초기화 콜백보다 늦게 실행된다. 빈 후처리기는 빈 인스턴스에 작업을 할 수도 있고 완전히 무시하고 넘어갈 수도 있다. 빈 후처리기는 일반적으로 콜백 인터페이스를 확인하거나 프록시로 빈을 감싸는 동작을 한다. 일부 스프링 AOP 인프라 클래스들은 빈 후처리기로 구현되어 프록시로 감싸는 동작을 수행한다.

`ApplicationContext`는 자동으로 설정메타데이터에서 `BeanPostProcessor`를 구현한 빈을 찾는다. `ApplicationContext`는 이러한 빈들을 빈 후처리기로 등록하여 빈들이 생성된 후 호출되도록 한다. 빈 후처리기는 다른 빈들과 같은 방법으로 컨테이너에 배포된다.

설정 클래스에서 `@Bean` 팩토리 메소드를 이용하여 `BeanPostProcessor`를 선언한다면 해당 팩토리 메소드의 반환 타입은 해당 구현체 클래스 자체이거나 최소한 `org.springframework.beans.factory.BeanPostProcessor` 인터페이스로 선언하여 명확하게 빈 후처리기임을 나타내야한다. 그렇지 않다면, `ApplicationContext`가 해당 빈을 완전히 생성하기 전까지 타입으로 확인을 할 수가 없을 것이다. `BeanPostProcessor`는 다른 빈보다 먼저 생성되어 다른 빈을 초기화 하는데 사용되야하기 때문에 타입으로 확인을 하는 것은 매우 중요하다.


<div id="Programmatically_registering_BeanPostProcessor" style="display:none"></div>


| |
| ----- |
| ***!** <b>`BeanPostProcessor` 인스턴스의 프로그래밍적 등록</b><br>`BeanPostProcessor`를 등록하는 방법으로 `ApplicationContext`의 자동발견을 사용하는 것을 추천하지만 `ConfigurableBeanFactory`의 `addBeanPostProcessor` 메소드를 이용하여 프로그래밍적으로 등록할 수도 있다. 빈 후처리기등록 전에 분기문을 타야하는 상황이나 컨텍스트 계층에서 빈 후처리기를 복사해와야 하는 경우 유용하게 사용될 수 있다. 하지만 `BeanPostProcessor`를 프로그래밍적으로 등록하면 `Ordered`인터페이스의 영향을 받지 않는다는 것을 명심하여야한다. 등록한 순서가 실행의 순서가 된다. 또한 프로그래밍적으로 등록된 `BeanPostProcessor`가 자동발견되어 등록된 것들보다 먼저 실행된다는 것도 명심해야한다.* |
| |
| ***!** <b>`BeanPostProcessor` 인스턴스와 AOP 자동 프록시 생성</b><br>컨테이너는 `BeanPostProcessor`를 구현한 클래스들을 다르게 취급한다. 모든 `BeanPostProcessor`인스턴스들은 `ApplicationContext`시작 동작의 특수한 단계에서 인스턴스화 된다. 그 뒤, 모든 `BeanPostProcessor`인스턴스들은 순서대로 등록되어 앞으로 생성되는 모든 빈에 적용된다. AOP 자동 프록시 생성은 `BeanPostProcessor`로 구현되었기 때문에 `BeanPostProcessor` 인스턴스나 직접참조되는 인스턴스들은 프록시가 적용되지 않는다.<br>그러한 모든 빈에 다음와 같은 Info레벨 로그를 확인할 수 있다:`Bean someBean is not eligible for getting processed by all BeanPostProcessor interfaces (for example: not eligible for auto-proxying).`<br>만약 빈 자동연결이나 `@Resource`를 이용하여 `BeanPostProcessor`에 연결한 빈이 있다면 스프링은 타입이 일치하는 빈을 찾는 과정에서 의도하지 않은 빈들을 자동 프록시 객체 생성이나 다른 후처리가 적용되지 못하도록 할것이다. 예를 들어 `@Resource`어노테이션을 이용해 필드기반 의존성 주입을 사용할 때, name 어트리뷰트를 사용하지 않았고 필드의 이름과 매칭되는 빈이 없다면 스프링은 해당 타입의 다른 빈들에 접근할 것이다.* |

아래의 예시는 `ApplicationContext`에 `BeanPostProcessor`를 작성하고 등록하고 사용하는 예시이다.

<h5 id="beans-factory-extension-bpp-examples-hw">예제: Hello World, BeanPostProcessor 스타일</h5>

첫 예시는 기본적인 사용법을 알려준다. 이 예시는 각각의 빈에 `toString()`메소드를 호출하여 시스템 콘솔로 출력하는 커스텀 `BeanPostProcessor` 구현체를 보여준다.

아래의 예시는 커스텀 `BeanPostProcessor`구현클래스의 정의이다:
```java
package scripting;

import org.springframework.beans.factory.config.BeanPostProcessor;

public class InstantiationTracingBeanPostProcessor implements BeanPostProcessor {

    // 단순히 빈 인스턴스를 그대로 반환한다.
    public Object postProcessBeforeInitialization(Object bean, String beanName) {
        return bean; // 여기에 어떠한 형태의 객체 참조를 반환할 수 있다.
    }

    public Object postProcessAfterInitialization(Object bean, String beanName) {
        System.out.println("Bean '" + beanName + "' created : " + bean.toString());
        return bean;
    }
}
```

다음의 `beans` 요소는 `InstantiationTracingBeanPostProcessor`를 사용한다:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:lang="http://www.springframework.org/schema/lang"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/lang
        https://www.springframework.org/schema/lang/spring-lang.xsd">

    <lang:groovy id="messenger"
            script-source="classpath:org/springframework/scripting/groovy/Messenger.groovy">
        <lang:property name="message" value="Fiona Apple Is Just So Dreamy."/>
    </lang:groovy>

    <!--
    위에 빈 (messenger)가 인스턴스화 될 때, 이 커스텀
    BeanPostProcessor는 시스템 콘솔에 정보를 출력할 것이다.
    -->
    <bean class="scripting.InstantiationTracingBeanPostProcessor"/>

</beans>
```

`InstantiationTracingBeanPostProcessor`가 단순히 정의된 것을 확인하십시오. 심지어 이름도 없습니다. 또한 이 빈후처리기가 빈이기 때문에 다른 빈에 의존성 주입이 될 수 있습니다. (이 빈 정의는 Groovy 스크립트 기반 빈들또한 정의합니다. 스프링의 다양한 언어 지원은 [동적 언어 지원](#)이라는 챕터에서 자세히 설명되어 있습니다)

아래의 자바 어플리케이션은 이전의 코드와 설정을 실행합니다:
```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.scripting.Messenger;

public final class Boot {

    public static void main(final String[] args) throws Exception {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("scripting/beans.xml");
        Messenger messenger = ctx.getBean("messenger", Messenger.class);
        System.out.println(messenger);
    }

}
```

위 어플리케이션의 결과는 아래와 비슷할 것입니다:
```
Bean 'messenger' created : org.springframework.scripting.groovy.GroovyMessenger@272961
org.springframework.scripting.groovy.GroovyMessenger@272961
```

<h5 id="beans-factory-extension-bpp-examples-rabpp">예제: RequiredAnnotationBeanPostProcessor</h5>

커스텀 `BeanPostProcessor` 구현체와 콜백 인터페이스나 어노테이션을 함께 사용하는 것은 스프링 IoC 컨테이너를 확장하는 보편적인 방법입니다. 스프링의 `RequiredAnnotationBeanPostProcessor`가 좋은 예시입니다. - 어노테이션이 설정된 빈 프로퍼티가 실제 값으로 빈주입이 되었음을 보장하는 `BeanPostProcessor`


<h4 id="beans-factory-extension-factory-postprocessors">BeanFactoryPostProcessor를 이용하여 설정 메타데이터 설정하기</h4>

다음 스프링 프레임워크 확장 지점은 `org.springframework.beans.factory.config.BeanFactoryPostProcessor`이다. 이 인터페이스의 의미는 `BeanPostProcessor`와 비슷하다. 한가지 중요한 다른 점은 `BeanFactoryPostProcessor`는 빈 설정 메타데이터에 동작한다는 것이다. 이 말은 스프링 IoC 컨테이너는 `BeanFactoryPostProcessor`가 설정 메타데이터를 읽고 `BeanFactoryPostProcessor`가 아닌 다른 빈들을 인스턴스화 하기 전에 변경할수 있도록 해준다는 것이다.

`BeanFactoryPostProcessor`는 여러 개 설정될 수 있다. `order`프로퍼티를 이용하여 동작 순서를 결정할 수 있다. 하지만 `BeanFactoryPostProcessor`가 `Ordered`인터페이스를 구현할 때만 사용가능하다. 자신만의 `BeanFactoryPostProcessor`를 작성한다면 `Ordered`인터페이스 또한 구현하는 것을 고려해야한다. [`BeanFactoryPostProcessor`](https://docs.spring.io/spring-framework/docs/5.2.8.RELEASE/javadoc-api/org/springframework/beans/factory/config/BeanFactoryPostProcessor.html)와 [`Ordered`](https://docs.spring.io/spring-framework/docs/5.2.8.RELEASE/javadoc-api/org/springframework/core/Ordered.html)인터페이스의 자바독을 보면 자세한 내용을 알 수 있습니다.

| |
| ----- |
| ***!** 설정메타데이터를 이용하여 생성된 객체인 빈 인스턴스 자체를 변경하고 싶으면 `BeanPostProcessor`를 사용하십시오.([`BeanPostProcessor`를 이용하여 빈 커스터마이징 하기](#beans-factory-extension-bpp)에 설명되어 있습니다.) `BeanFactoryPostProcessor`에서 빈 인스턴스에 작업하는 것이 불가능하지 않지만(예를 들면, `BeanFactory.getBean()`을 사용하여서) 그렇게 하면 빈 인스턴스를 이른 시점에 생성하게 되어 컨테이너 표준 생명주기를 위반하게 됩니다. 이 방법은 빈 후처리를 생략하게 되는 등, 부작용을 야기할 수 있습니다.<br>또한 `BeanFactoryPostProcessor` 인스턴스는 컨테이너 스코프를 가집니다. 이는 컨테이너 계층을 활용할 때만 관련이있는 정보이다. `BeanFactoryPostProcessor`를 한 컨테이너에서 정의하면 그 컨테이너에 있는 빈 정의만 후처리할 것이다. 다시 말하자면 `BeanFactoryPostProcessor`는 다른 컨테이너에 등록된 빈 정의의 후처리를 하지 않을 것이다. 두 컨테이너가 같은 계층구조상에 있더라고 마찬가지이다.*  |

빈 팩토리 후처리기가 `ApplicationContext`에서 정의되면 생성자는 자동으로 동작하여 컨테이너에 정의된 빈 정의 설정 메타데이터에 변경을 가합니다. 스프링은 `PropertyOverrideConfigurer`나 `PropertySourcesPlaceholderConfigurer`와 같은 미리 정의된 다수의 빈 팩토리 후처리기를 포한하고 있습니다. 커스텀 `BeanFactoryPostProcessor`를 사용할 수 있습니다. - 예를 들면 커스텀 프로퍼티 에디터를 등록하는 빈 팩토리 후처리기.

`ApplicationContext`는 자동으로 설정메타데이터에서 `BeanFactoryPostProcessor`를 구현한 빈을 찾는다. `ApplicationContext`는 이러한 빈들을 빈 팩토리 후처리기로 등록하여 적절한 떄에 사용합니다. 빈 팩토리 후처리기는 다른 빈들과 같은 방법으로 컨테이너에 배포된다.


| |
| ----- |
| ***!** 일반적인 경우 `BeanPostProcessor`와 마찬가지로 `BeanFactoryPostProcessor`를 지연 초기화하지는 않을 것이다. 다른 어떠한 빈도 `BeanPostProcessor`나 `BeanFactoryPostProcessor`를 참조하지 않으면 후처리기는 인스턴스화되지 않을 것이다. 따라서 지연 초기화 설정은 무시된다. `BeanPostProcessor`와 `BeanFactoryPostProcessor`는 지연 초기화 되지 않을 것이다. 심지어 `beans`요소의 `default-lazy-init`어트리뷰트를 `true`로 설정하여도 마찬가지이다.* |

<h5 id="beans-factory-placeholderconfigurer">예제: 클래스 이름 대체 PropertySourcesPlaceholderConfigurer</h5>

`PropertySourcesPlaceholderConfigurer`를 이용하면 자바 표준 `Properties`를 사용하여 프로퍼티의 값을 빈 정의로부터 다른 파일로 분리할 수 있다. 이렇게 함으로써 DB url과 패스워드 같은 환경에 종속된 프로퍼티를 변경할 떄, 컨테이너의 복잡한 주요 xml 파일을 변경하는 위험성 없이 어플리케이션을 배포할 수 있다.

`DataSource`가 플레이스홀더 값으로 정의된 xml 기반 설정 메타데이터의 경우를 생각해 보자:
```xml
<bean class="org.springframework.context.support.PropertySourcesPlaceholderConfigurer">
    <property name="locations" value="classpath:com/something/jdbc.properties"/>
</bean>

<bean id="dataSource" destroy-method="close"
        class="org.apache.commons.dbcp.BasicDataSource">
    <property name="driverClassName" value="${jdbc.driverClassName}"/>
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</bean>
```
이 예시는 외부 `Properties` 파일로 프로퍼티를 설정하는 것을 보여준다. 런타임에 `PropertySourcesPlaceholderConfigurer`가 메타데이터에 적용되어 DataSource의 프로퍼티를 변경한다. Ant, log4j, JSP EL 형식을 따르는 `${property-name}`의 형식으로 작성된 플레이스홀더의 값을 변경한다.

실제 값은 다른 파일에 저장되어 Java 표준 `Properties` 형식으로 전달된다:
```
jdbc.driverClassName=org.hsqldb.jdbcDriver
jdbc.url=jdbc:hsqldb:hsql://production:9002
jdbc.username=sa
jdbc.password=root
```

따라서, `${jdbc.username}` 문자열은 런타임에 'sa'로 변경된다. 다른 플레이스홀더 값도 매칭되는 키값에 따른 프로퍼티 값으로 치환된다. `PropertySourcesPlaceholderConfigurer`가 빈 정의의 프로퍼티와 어트리뷰트의 플레이스홀더의 값을 확인하여 변경한다. 추가적으로 플레이스홀더의 접두사와 접미사를 설정할 수 있다.

스프링 2.5부터 지원하는 `context` 네임스페이스를 이용하여 프로퍼티 플레이스홀더를 해당 목적으로 사용되는 설정 요소와 함께 적용할 수 있다. 아래 예시처럼 `location`어트리뷰트에 ','로 구분되는 리스트를 작성하여 파일 위치를 지정할 수 있다:
```xml
<context:property-placeholder location="classpath:com/something/jdbc.properties"/>
```

`PropertySourcesPlaceholderConfigurer`는 개발자가 명시한 `Properties` 파일 이외에도 다른 프로퍼티를 이용한다. 명시된 파일에서 프로퍼티를 찾지 못하는 경우, 스프링의 `Environment`프로퍼티와 자바 `System`프로퍼티를 확인한다.

-----

**!** `PropertySourcesPlaceholderConfigurer`를 이용하여 클래스 이름을 대체할 수 있다. 이 방법은 런타임에 특정한 클래스 구현을 선택해야할 때 유용하다. 아래의 예시가 방법을 보여준다:
```xml
<bean class="org.springframework.beans.factory.config.PropertySourcesPlaceholderConfigurer">
    <property name="locations">
        <value>classpath:com/something/strategy.properties</value>
    </property>
    <property name="properties">
        <value>custom.strategy.class=com.something.DefaultStrategy</value>
    </property>
</bean>

<bean id="serviceStrategy" class="${custom.strategy.class}"/>
```
런타임에 올바른 클래스를 제공할 수 없다면 빈이 생성될 때 오류가 발생한다. 지연 초기화하지 않는 빈의 경우, `ApplicationContext`의 `preInstantiateSingletons()`를 실행하는 도중을 말한다.

-----

<h5 id="beans-factory-overrideconfigurer">예제: PropertyOverrideConfigurer</h5>

또 다른 빈 팩토리 후처리기인 `PropertyOverrideConfigurer`는 `PropertySourcesPlaceholderConfigurer`와 비슷하다. 다른점은 빈 정의가 기본값을 가지거나 값을 가지지않을 수 있다는 것이다. `Properties` 파일에 해당 빈 프로퍼티가 없다면 기본 컨텍스트 정의가 사용된다.

빈 정의는 값이 재작성 된다는 것을 알지 못한다. 재작성되는 빈이 정의된 xml 빈 정의에서 재작성 된다는 사실이 명확하지 않다. `PropertyOverrideConfigurer`가 같은 프로퍼티에 다른 값으로 여러개 정의된다면 재작성 동작방법에 따라 마지막 것이 사용된다.

프로퍼티 파일은 아래형식으로 설정한다:
```
beanName.property=value
```

아래 리스트는 그 예시를 보여준다:
```
dataSource.driverClassName=com.mysql.jdbc.Driver
dataSource.url=jdbc:mysql:mydb
```

이 예제는 `driver`와 `url`프로퍼티가 있는 `dataSource`라는 빈이 정의된 컨테이너에서 사용될 수 있다.

재작성될 마지막 프로퍼티를 제외한 나머지 프로퍼티가 null이 아니라면 복합 프로퍼티 이름또한 지원한다. 아래의 예시에서는 `tom`빈의 `fred`프로퍼티의 `bob`프로퍼티의 `sammy`프로퍼티의 값이 `123`으로 설정될 것이다:
```
tom.fred.bob.sammy=123
```

| |
| ----- |
| ***!** 명시된 재작성 값은 항상 문자값이여야한다. 이 값들은 빈 참조로 해석되지 않을 것이다. 이 규칙은 빈 참조를 설정하는 xml빈 정의의 값에도 적용된다.* |

스프링 2.5부터 지원하는 `context` 네임스페이스를 이용하여 재작성할 프로퍼티를 아래 예시처럼 설정할 수 있다:
```xml
<context:property-override location="classpath:override.properties"/>
```

<h4 id="beans-factory-extension-factorybean">FactoryBean을 이용하여 인스턴스화 로직 커스터마이징 하기</h4>

그 자체로 팩토리인 객체를 생성하기 위해 `org.springframework.beans.factory.FactoryBean` 인터페이스를 구현할 수 있다.

`FactoryBean` 인터페이스는 스프링 IoC 컨테이너의 인스턴스화 로직에 접촉할 수 있는 지점이다. 장황한 xml대신 자바 로직으로 잘 표현된 초기화 코드를 가지고 있다면 자신만의 `FactoryBean`을 만들고 복잡한 초기화를 클래스 내부에 작성하여 커스텀 `FactoryBean`을 컨테이너에 연결할 수 있다.

`FactoryBean` 인터페이스는 3가지 메소드를 제공한다.

* `Object getObject()` : 팩토리가 만드는 객체의 인스턴스를 반환한다. 팩토리가 싱글톤을 리턴하는지 프로토타입을 리턴하는지에 따라 이 인스턴스는 공유될 수 있다.
* `boolean isSingleton()` : `FactoryBean`이 싱글톤을 반환하면 `true`, 그렇지않으면 `false`로
* `Class getObjectType()` : `getObject()`메소드를 호출하면 반환하는 객체의 타입. 타입을 알지 못하면 `null`값을 리턴

`FactoryBean` 컨셉과 인터페이스는 스프링 프레임워크에서 널리 사용된다. 스프링 자체적으로 50개 이상의 `FactoryBean`구현체를 가지고 있다.

만약 컨테이너에서 `FactoryBean`인스턴스를 가져오고 싶다면, `ApplicationContext`의 `getBean()` 메소드를 호출할 떄, 빈의 `id`앞에 앰퍼샌드 기호(`&`)를 붙여서 사용하면 된다. `myBean`이라는 `id`의 `FactoryBean`이 있을 때, `getBean("myBean")`을 호출하면 `FactoryBean`이 생성한 객체를 반환한다. `getBean("&myBean")`은 `FactoryBean`자체를 반환한다.

<h3 id="beans-annotation-config">어노테이션 기반 컨테이너 설정</h3>

| |
| ----- |
| **어노테이션 설정이 xml 설정보다 더 나을까요?**<br>어노테이션 기반 설정이 나오면서 xml보다 더 나은 방법인지 의문이 생겨났다. 짧게 대답하면 "경우에 따라 다르다"이다. 길게 설명하면 각각의 방법은 장단점이 있고 대개의 경우 개발자가 자신에게 맞는 방법을 선택하는 것이 좋다. 어노테이션이 정의되는 방법에 의하여 어노테이션은 컨텍스트 정보를 많이 제공한다. 따라서 더 짧고 정확한 설정이 된다. 하지만 xml은 소스코드를 건드리거나 다시 컴파일할 필요없이 컴포넌트를 연결한다. 일부 개발자들은 소스코드에 연결을 작성하는 것을 선호하지만 다른 개발자들은 어노테이션이 적용된 클래스들은 더이상 POJO가 아니게 되며 설정파일이 분산되어 통제하기 힘들어진다고 말한다.<br>어떤 방법을 선택하든지 스프링은 잘 동작한다. 심지어 같이 섞어서 사용할 수 있다. [자바기반설정](#beans-java)을 또한 고려해보십시오. 이 방법은 소스코드를 건드릴 필요없이 어노테이션을 사용하게 해준다. [Spring Tools for Eclipse](https://spring.io/tools)는 이 모든 설정 방법을 지원합니다. |

xml설정을 대신하는 방법은 어노테이션 기반 설정이다. 이 방법은 꺽쇠('<','>')로 컴포넌트를 연결하는 대신에 바이트코드 메타데이터를 이용한다. 빈을 연결하는데 xml을 사용하는 대신 컴포넌트 클래스 그 자체에 어노테이션을 이용하여 설정을 작성한다. 위 [예제 : `RequiredAnnotationBeanPostProcessor`](#beans-factory-extension-bpp-examples-rabpp)에서 언급했듯이, 어노테이션을 이용한 `BeanPostProcessor`설정은 스프링 IoC 컨테이너를 확장하는 보편적인 방법이다. 예를 들면, 스프링 2.0부터 프로퍼티를 반드시 설정되게 하는 [`@Required`](https://docs.spring.io/spring-framework/docs/5.2.8.RELEASE/spring-framework-reference/core.html#beans-required-annotation)어노테이션을 사용가능하다. 스프링 2.5에서 스프링 의존성 주입이 같은 방식으로 동작할 수 있도록 만들었다. `@Autowired` 어노테이션은 [자동연결 협력자](#beans-factory-autowire)에 설명된 기능뿐만 아니라 더 많은 기능을 지원한다. 스프링 2.5부터 `@PostConstruct`나 `@PreDestory`와 같은 JSR-250 어노테이션도 지원한다. 스프링 3.0은 `javax.inject`패키지의 `@Inject`와 `@Named`를 포함하는 JSR-330(자바 의존성 주입) 어노테이션을 지원한다. 이 어노테이션에 대한 자세한 내용은 [관련 섹션](#beans-standard-annotations)에서 볼 수 있다.

| |
| ----- |
| ***!** 어노테이션 빈 주입은 xml 빈 주입보다 먼저 실행됩니다. 따라서 xml설정이 어노테이션 기반 프로퍼티의 값을 재 작성할 것입니다.* |

항상 그랬듯이, 각각의 빈정의를 등록할 수 있다. 하지만 xml기반 스프링 설정에서 아래의 태그를 사용하여 함축적으로 등록할 수 있다.(`context` 네임스페이스를 사용할 것을 확인하십시오):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```
함축적으로 등록된 후처리기에는 [`AutowiredAnnotationBeanPostProcessor`]
(https://docs.spring.io/spring-framework/docs/5.2.8.RELEASE/javadoc-api/org/springframework/beans/factory/annotation/AutowiredAnnotationBeanPostProcessor.html),[ `CommonAnnotationBeanPostProcessor`](https://docs.spring.io/spring-framework/docs/5.2.8.RELEASE/javadoc-api/org/springframework/context/annotation/CommonAnnotationBeanPostProcessor.html),[ `PersistenceAnnotationBeanPostProcessor`](https://docs.spring.io/spring-framework/docs/5.2.8.RELEASE/javadoc-api/org/springframework/orm/jpa/support/PersistenceAnnotationBeanPostProcessor.html), 이전에 언급된 [`RequiredAnnotationBeanPostProcessor`](https://docs.spring.io/spring-framework/docs/5.2.8.RELEASE/javadoc-api/org/springframework/beans/factory/annotation/RequiredAnnotationBeanPostProcessor.html)가 있다.

| |
| ----- |
| ***!** `<context:annotation-config/>`는 같은 어플리케이션 컨텍스트 내부 빈의 어노테이션만 확인한다. 이 말은 `DispatcherServlet`의 `WebApplicationContext`에 `<context:annotation-config/>`를 설정하면 컨트롤러에 있는 `@Autowired` 빈을 확인할 것이지만 서비스에 있는 빈은 확인하지 않을 것이다. 자세한 내용은 [DispatcherServlet](https://docs.spring.io/spring-framework/docs/5.2.8.RELEASE/spring-framework-reference/web.html#mvc-servlet)을 보십시오.* |

<h4 id="beans-required-annotation">@Required</h4>

`@Required`어노테이션은 아래 예시처럼 프로퍼티의 세터메소드에 사용된다:
```java
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Required
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```
이 어노테이션은 어노테이션이 설정된 빈 프로퍼티가 스프링 설정시에 명시적 프로퍼티 값이나 자동연결을 통하여 값이 주입되야 한다는 것을 말한다. 값이 주입되지 않으면 스프링 컨테이너가 예외를 던질것이다. 이 방법으로 빠른 시점에 명시적인 실패를 일으켜 나중에 발생할 `NullPointerException` 등을 피하게 한다. 빈 클래스 자체(예를 들면 초기화 메소드)에 확인을 넣는 것을 추천한다. 이렇게 하면 컨테이너 외부에서 사용할 경우에도 필수적인 프로퍼티가 설정됨을 보장할 수 있다.

| |
| ----- |
| ***!** `@Required` 어노테이션은 스프링 5.1에서 공식적으로 사용중단을 권장한다. 필수 적인 값을 설정하는데에 생성자 주입을 사용하는 것을 추천한다. (혹은 프로퍼티 세터메소드와 커스텀 `InitializingBean.afterPropertiesSet()`구현을 사용하십시오.)* |

<h4 id="beans-autowired-annotation">@Autowired 사용하기</h4>

| |
| ----- |
| ***!** JSR 330의 `@Inject`어노테이션은 스프링 `@Autowired`가 사용된 곳에 사용될 수 있다. 이 섹션에 예제가 포함되어 있다. 자세한 내용은 [여기](#beans-standard-annotations)를 보십시오.* |

`@Autowired`어노테이션을 아래예시처럼 생성자에 적용할 수 있다:
```java
public class MovieRecommender {

    private final CustomerPreferenceDao customerPreferenceDao;

    @Autowired
    public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) {
        this.customerPreferenceDao = customerPreferenceDao;
    }

    // ...
}
```

| |
| ----- |
| ***!** 스프링 4.3부터 위와 같이 생성자가 1개인 경우에 `@Autowired`어노테이션이 필요하지 않다. 하지만 여러개의 생성자가 사용가능하고 최우선/기본 생성자가 없다면 최소 한개의 생성자가 `@Autowired` 어노테이션이 설정되어 있어야한다. 자세한 내용은 [생성자 결정](#beans-autowired-annotation-constructor-resolution)을 보십시오.* |

또한 `@Autowired`어노테이션은 아래 예시처럼 세터메소드에 사용 가능합니다:
```java
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Autowired
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```
또한 임의의 이름과 여러 어규먼트를 가진 메소드에 어노테이션을 적용할 수 있습니다:

```java
public class MovieRecommender {

    private MovieCatalog movieCatalog;

    private CustomerPreferenceDao customerPreferenceDao;

    @Autowired
    public void prepare(MovieCatalog movieCatalog,
            CustomerPreferenceDao customerPreferenceDao) {
        this.movieCatalog = movieCatalog;
        this.customerPreferenceDao = customerPreferenceDao;
    }

    // ...
}
```
또한 필드에 사용가능합니다. 심지어 생성자와 섞어 쓸 수도 있습니다:

```java
public class MovieRecommender {

    private final CustomerPreferenceDao customerPreferenceDao;

    @Autowired
    private MovieCatalog movieCatalog;

    @Autowired
    public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) {
        this.customerPreferenceDao = customerPreferenceDao;
    }

    // ...
}
```

| |
| ----- |
| ***@** 필요한 컴포넌트(예를 들면, `MovieCatalog`나 `CustomerPreferenceDao`)가 `@Autowired`가 적용된 타입으로 정의되어 있어야합니다. 그렇지 않다면 의존성 주입이 실패하며 `no type match found`에러가 런타임에 발생할 것입니다.<br>xml 빈 정의나 클래스패스 스캔을 통하여 발견된 컴포넌트 클래스들은 컨테이너가 타입을 처음부터 알고 있습니다. 하지만 `@Bean` 팩토리 메소드는 선언된 반환 타입이 충분한 정보를 가지고 있어야합니다. 여러개의 인터페이스를 구현한 컴포넌트나 구현체 타입으로 참조될 가능성이 있는 컴포넌트들은 팩토리 메서드에 가장 구체적인 리턴타입으로 선언하는 것이 좋습니다.(최소한 의존성 주입이 가능한 정도로 구체적이여야 합니다)* |

특정 타입 배열 형태의 필드나 메소드에 `@Autowired`를 추가하여 `ApplicationContext`가 해당 타입의 모든 빈을 제공하도록 구성할 수 있습니다:
```java
public class MovieRecommender {

    private Set<MovieCatalog> movieCatalogs;

    @Autowired
    public void setMovieCatalogs(Set<MovieCatalog> movieCatalogs) {
        this.movieCatalogs = movieCatalogs;
    }

    // ...
}
```

| |
| ----- |
| ***@** `org.springframework.core.Ordered`인터페이스를 구현하거나 `@Order`어노테이션을 사용하거나 `@Priority`표준 어노테이션을 사용하여 빈들이 특정한 순서로 정렬된 배열이나 리스트를 가져올 수 있습니다. 그렇지 않다면 컨테이너에 빈 정의가 등록된 순서를 따를 것입니다.<br>`@Order`어노테이션은 빈의 클래스나 `@Bean`메소드에 선언할 수 있습니다. `@Order`어노테이션은 의존성 주입의 우선순위에 영향을 미칠수 있습니다. 싱글톤 빈이 생성되는 순서에는 영향이 없다는 것을 알고 있어야합니다. 싱글톤 빈의 생성 순서는 의존성 관계와 `@DependsOn`정의에 따라 수직적으로 결정됩니다.<br>표준 `javax.annotation.Priority` 어노테이션은 `@Bean`에 사용할 수 없습니다. 메소드에 적용될 수 없는 어노테이션이기 떄문입니다. 대신 `@Order`값이나 `@Primary`를 이용할 수 있습니다.* |

심지어 키 타입이 `String`이라면 `Map`인스턴스로 자동연결될 수 잇습니다. 해당 타입의 빈의 이름을 키값으로 빈 자체를 값으로 하는 Map이 됩니다:
```java
public class MovieRecommender {

    private Map<String, MovieCatalog> movieCatalogs;

    @Autowired
    public void setMovieCatalogs(Map<String, MovieCatalog> movieCatalogs) {
        this.movieCatalogs = movieCatalogs;
    }

    // ...
}
```
기본적으로 의존성 주입 지점에 걸맞은 빈이 없을 떄 자동주입은 실패합니다. 배열, 컬렉션, 맵이더라도 최소 1개의 맞는 빈이 필요합니다.

어노테이션이 적용된 필드나 메소드를 필수 의존성으로 생각하는 것이 기본 동작입니다. 아래 예시처럼 이 동작을 바꿔서 필수적이지 않은 의존성 주입에 실패하더라도 넘어가도록 변경할 수 있습니다.(`@Autowired`의 `required` 어트리뷰트를 `false`로 설정하면 됩니다):
```java
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Autowired(required = false)
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```

필수적이지 않은 세터메소드에 맞는 의존성 어규먼트들이 없다면 그 메서드는 호출되지 않을 것입니다. 이런 경우 필수적이지 않은 필드는 변경되지 않고 기본값 그대로 유지될 것입니다.

<div id="beans-autowired-annotation-constructor-resolution" style="display:none"></div>
생성자나 팩토리 메서드의 어규먼트로 주입되는 경우는 특수한 경우입니다. 스프링의 생성자 결정 알고리즘이 여러 생성자를 처리해야 할 수도 있기 때문에 `@Autowired`의 `required`어트리뷰트는 다른 의미를 가집니다. 생성자와 팩토리 메서드 어규먼트들은 사실상 필수입니다. 생성자가 1개인 경우, 약간의 특별한 규칙이 존재합니다. 이를테면 다중 요소 주입 지점(배열, 컬렉션, 맵)의 경우, 맞는 빈이 없다면 비어있는 인스턴스가 주입됩니다. 이 방법으로 필요한 모든 의존성을 한개의 생성자에 선언하는 보편적인 구현 패턴을 사용가능하게 합니다. - 예를 들면 `@Autowired`가 없는 1개의 퍼블릭 생성자로 선언하는 것이 이러한 경우입니다.

| |
| ----- |
| ***!** 빈 클래스의 1개의 생성자만 `Required`어트리뷰트가 `true`인 `@Autowired`로 선언되어 스프링 빈으로 사용할 생성자임을 나타낼 것이다. `required`어트리뷰트가 기본값이 `true` 그대로 나둔다면 단 한개의 생성자만 `@Autowired`로 설정될 것이다. 여러개의 생성자에 어노테이션을 선언한다면, 전부 `required=false`로 선언해서 자동연결 후보로 여겨져야할 것이다.(XML에서 `autowire=constructor`와 같다.) 의존성 주입이 가능한 생성자들중 가장 많은 의존성을 가지고 있는 생성자가 스프링 컨테이너에 의하여 선택될 것이다. 자동연결 후보 중 어느 것도 의존성이 해결되지 않는다면 최우선/기본 생성자가 사용될 것이다. 비슷하게 클래스가 여러개의 생성자를 가지고있고 어느 하나 `@Autowired`로 선언되지 않았으면 최우선/기본 생성자가 사용될 것이다. 클래스가 한개의 생성자만 가지고 있다면 어노테이션 존재와 상관없이 항상 그 생성자가 사용될 것이다. 어노테이션이 적용된 생성자는 public일 필요가 없다는 것을 알아두십시오.*<br>*`@Autowired`의 `required`어트리뷰트는 더이상 사용을 권장하지 않는 `@Required`어노테이션을 대신하여 세터메소드에 사용하는걸 권장한다. `required`어트리뷰트를 `false`로 설정하는 것은 자동연결시에 반드시 필요하지 않다는 것을 의미하고 자동연결이 불가하면 프로퍼티는 무시됩니다. 반면에 `@Required`는 프로퍼티가 어떠한 방법으로든 컨테이너에 의하여 설정되도록 강제하고 그렇지 않으면 예외가 발생할 것이다.* |

대신에 반드시 필요하지 않은 특정 의존성을 자바 8의 `java.util.Optional`을 이용하여 표현할수 있다:
```jvava
public class SimpleMovieLister {

    @Autowired
    public void setMovieFinder(Optional<MovieFinder> movieFinder) {
        ...
    }
}
```

스프링 5.0부터 `@Nullable` 어노테이션(어떤 패키지에 어떤 종류라도 상관없다 - 예를 들면 JSR-305의 `javax.annotation.Nullable`)을 사용할수 있다:

```java
public class SimpleMovieLister {

    @Autowired
    public void setMovieFinder(@Nullable MovieFinder movieFinder) {
        ...
    }
}
```

또한 `@Autowired`를 주입 가능한 것으로 잘 알려진 인터페이스에 사용할 수 있다: `BeanFactory`, `ApplicationContext`, `Environment`, `ResourceLoader`, `ApplicationEventPublisher`, `MessageSource`. 이러한 인터페이스와 이러한 인터페이스를 확장한 인터페이스 (`ConfigurableApplicationContext`, `ResourcePatternResolver` 등)은 다른 추가적인 설정없이 자동적으로 주입된다. 아래의 예시는 `ApplicationContext`를 자동연결하는 예시이다:

```java
public class MovieRecommender {

    @Autowired
    private ApplicationContext context;

    public MovieRecommender() {
    }

    // ...
}
```

| |
| ----- |
| ***!** `@Autowired`, `@Inject`, `@Value`, `@Resource` 어노테이션은 스프링 `BeanPostProcessor`의 구현체에 의해 적용된다. 이 말은 자신만의 `BeanPostProcessor`나 `BeanFactoryPostProcessor`에서 이러한 어노테이션을 적용할 수 없다는 이야기이다. 이러한 타입은 반드시 xml이나 `@Bean`메소드에서 명시적으로 연결되어야한다.* |

<h4 id="beans-autowired-annotation-primary">@Primary를 이용하여 어노테이션 기반 자동 연결 미세 조정하기</h4>

타입을 이용한 자동연결은 여러 후보빈들이 있을 수 있다. 따라서 종종 선택 과정에 더 자세한 조절이 필요하다. 한가지 방법은 스프링의 `@Primary` 어노테이션을 사용하는 것이다. `@Primary`는 여러 후보 빈들 중에 우선권을 준다. 만약 후보 빈들 중 단 한개의 우선 빈이 존재한다면 그 빈이 자동연결될 것이다.

최우선 `MovieCatalog`로 `firstMovieCatalog`로 정의된 아래의 설정을 생각해보자:
```java
@Configuration
public class MovieConfiguration {

    @Bean
    @Primary
    public MovieCatalog firstMovieCatalog() { ... }

    @Bean
    public MovieCatalog secondMovieCatalog() { ... }

    // ...
}
```
위의 설정을 이용하면 아래의 `MovieRecommender`에 `firstMovieCatalog`가 자동 연결된다:
```java
public class MovieRecommender {

    @Autowired
    private MovieCatalog movieCatalog;

    // ...
}
```
관련된 빈정의는 아래와 같다:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

    <bean class="example.SimpleMovieCatalog" primary="true">
        <!-- 이 빈과 관련된 의존성을 주입한다. -->
    </bean>

    <bean class="example.SimpleMovieCatalog">
        <!-- 이 빈과 관련된 의존성을 주입한다. -->
    </bean>

    <bean id="movieRecommender" class="example.MovieRecommender"/>

</beans>
```

<h4 id="beans-autowired-annotation-qualifiers">Qualifiers를 이용하여 어노테이션 기반 자동 연결  미세 조정하기</h4>

`@Primary`는 여러 인스턴스 중 한개를 최우선으로 결정했을 때 매우 유용하다. 선택과정에 더 많은 조정이 필요할 때에는 스프링의 `@Qualifier`를 사용할 수 있다. 특정 어규먼트에 자격 요건을 설정해주어 특정 빈이 선택되도록 후보 빈을 걸러낼 수 있다. 가장 단순한 예로 아래 예시처럼 설명적 값을 줄수있다.
```java
public class MovieRecommender {

    @Autowired
    @Qualifier("main")
    private MovieCatalog movieCatalog;

    // ...
}
```

또한 `@Qualifier` 어노테이션을 각 생성자 어규먼트나 메소드 파라메터에 설정할 수 있다:
```java
public class MovieRecommender {

    private MovieCatalog movieCatalog;

    private CustomerPreferenceDao customerPreferenceDao;

    @Autowired
    public void prepare(@Qualifier("main") MovieCatalog movieCatalog,
            CustomerPreferenceDao customerPreferenceDao) {
        this.movieCatalog = movieCatalog;
        this.customerPreferenceDao = customerPreferenceDao;
    }

    // ...
}
```

아래의 예시는 관련된 빈 정의를 보여준다 :
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

    <bean class="example.SimpleMovieCatalog">
        <qualifier value="main"/> <!-- 1 -->
		
        <!-- 이 빈과 관련된 의존성을 주입한다. -->
    </bean>

    <bean class="example.SimpleMovieCatalog">
        <qualifier value="action"/> <!-- 2 -->

        <!-- 이 빈과 관련된 의존성을 주입한다. -->
    </bean>

    <bean id="movieRecommender" class="example.MovieRecommender"/>

</beans>
```

1. `main` qualifier 값이 설정된 빈으로 같은 값으로 qualifier가 설정된 생성자 어규먼트와 연결된다.
2. `action` qualifier 값이 설정된 빈으로 같은 값으로 설정된 qualifier가 생성자 어규먼트와 연결된다.

qualifier 매칭을 위한 후보로써 빈 이름이 기본 qualifier값으로 여겨진다. 따라서 `main`이라는 `id`를 가진 빈을 정의하여 같은 결과를 가져올 수 있다. 이 방법을 사용하면 빈의 이름으로 편하게 연결할 수 있지만 `@Autowired`는 기본적으로 타입 기반 의존성 주입이다. 이 말은 qualifier 값은 매칭되는 빈들 중에서 줄여주는 역할이라는 것이다. 고유한 `id`를 참조하는 표현이 아니다. 좋은 qualifier 값은 `main`, `EMEA` `persistent`와 같이 컴포넌트의 특성을 나타내는 값이며 이 값들은 빈의 `id`와 무관하다.

qualifier는 또한 이전에 이야기된 타입 컬렉션에도 적용된다. - 예를 들면 `Set<MovieCatalog>`. 이러한 경우에 선언된 qualifier를 만족하는 모든 빈이 컬렉션으로 주입된다. 이 qualifier는 반드시 고유할 필요가 없다. 대신 분류하는 기준으로 표현되야한다. 예를 들면 여러개의 `MovieCatalog`를 같은 "action" qualifier 값으로 설정할 수 있다. 이렇게하면 `@Qualifier("action")`으로 선언된 `Set<MovieCatalog>`로 모두 주입될 것이다.

| |
| ----- |
| ***!** qualifier 값으로 빈을 선택하는데 반드시 `@Qualifier` 어노테이션이 필요한 것은 아니다. 선택 표시자(qualifier나 primary)가 없다면, 스프링이 의존성 주입 지점의 이름(필드 이름이나 파라메터 이름)을 이용하여 빈 이름을 찾아볼 것이다.* |

즉 이름을 이용하여 의존성 주입을 하고자 한다면 `@Autowired`를 사용하면 이름을 선택이 가능하지만 `@Autowired`를 후순위로 고려하여라. 대신에 특정한 이름을 가진 컴포넌트를 정의하는 의미를 가진 `@Resource` 어노테이션을 사용하여라. `@Autowired`는 조금 다른 의미를 가진다: 타입으로 후보 빈을 선택한 뒤에 `String` qualifer값이 이 후보 빈들 중에서 고려된다.

그 자체가 `Map`, 컬렉션, 배열로 정의된 빈을 선택할 때에는 `@Resource`를 이용하는 것이 좋다. 스프링 4.3부터 `@Autowired`를 이용하여도 `Map`, 컬렉션, 배열 타입을 의존성 주입할 수 있다. 이 경우, qualifier값을 이용하여 같은 타입의 빈에서 원하는 것을 고를 수 있다.

스프링 4.3부터 `@Autowired`는 자기 자신 참조 또한 의존성 주입할 수 있다. 자기 자신 의존성 주입은 마지막 선택지이다. 일반적인 의존성 주입이 먼저 실행되고 가장 마지막으로 자시자신 의존성 주입을 선택한다. 또한 개발자는 자기자신 의존성 주입을 가장 마지막 수단으로 고려해야한다(예를 들면 빈 내부에서 트랙잭션 프록시 메소드를 호출하는 것). 이러한 경우에 영향을 받는 메소드를 위임객체에 따로 분리하는 것을 고려해봐라. 또한 `@Resource`를 대신 사용하여 이름으로 프록시 객체를 얻어올 수 있다.

| |
| ----- |
| ***!** 설정 클래스 내부에서 `@Bean`메소드의 결과를 주입하는 것은 효과적인 자기 자신 참조 방법이다. 참조가 필요한 메소드에서 참조를 지연초기화 하거나 `@Bean`메소드를 `static`으로 선언하는 두 방법 모두 설정 클래스 인스턴스와 생명주기에서 빈을 독립시킨다. 그렇지 않으면 이러한 빈은 마지막에 고려되어 적합한 다른 설정 클래스의 빈들이 대신 주입될 것이다.* |

`@Autowired`는 필드, 생성자, 다수의 어규먼트를 가진 메소드에 적용된다. 반면에 `@Resource`는 오직 필드와 한개의 어규먼트를 가진 세터메소드에만 적용된다. 의존성 주입 지점이 생성자나 어규먼트 여러개를 가진 메소드라면 `@Autowired`를 이용해야한다.

자신만의 qualifier 어노테이션을 만들 수 있다. `@Qualifier` 어노테이션이 정의된 새로운 어노테이션을 정의하면 된다:

```java
@Target({ElementType.FIELD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier
public @interface Genre {

    String value();
}
```

그러면 자신만의 qualifier는 자동 연결하는 파라메터나 필드에 사용할 수 있다:

```java
public class MovieRecommender {

    @Autowired
    @Genre("Action")
    private MovieCatalog actionCatalog;

    private MovieCatalog comedyCatalog;

    @Autowired
    public void setComedyCatalog(@Genre("Comedy") MovieCatalog comedyCatalog) {
        this.comedyCatalog = comedyCatalog;
    }

    // ...
}
```

그 다음에 후보 빈정의에 필요한 정보를 제공해야한다. `<qualifer/>` 태그를 `<bean/>`태그 하위에 정의하고 `type`과 `value`를 설정하면 된다. type은 어노테이션의 클래스 전체 이름을 의미한다. 단 같은 이름이 없다면 짧게 클래스 이름만 사용해도 된다. 아래의 예시가 두가지 방법 모두 사용하는 예시이다:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

    <bean class="example.SimpleMovieCatalog">
        <qualifier type="Genre" value="Action"/>
        <!-- 이 빈에 필요한 의존성 주입 -->
    </bean>

    <bean class="example.SimpleMovieCatalog">
        <qualifier type="example.Genre" value="Comedy"/>
        <!-- 이 빈에 필요한 의존성 주입 -->
    </bean>

    <bean id="movieRecommender" class="example.MovieRecommender"/>

</beans>
```

[클래스패스 스캐닝과 관리되는 컴포넌트](#beans-classpath-scanning)에서 XML에 qualifier 메타데이터를 작성하는 대신 어노테이션 기반으로 설정하는 방법을 볼 수 있다. 특히, [Qualifier 메타데이터 어노테이션으로 부여하기](#beans-scanning-qualifiers)를 보십시오.

일부 경우에서 값없이 어노테이션만 사용하는 것만으로 충분할 수 있습니다. 어노테이션이 보편적인 의미를 가지고 여러 다른 타입의 의존성 걸쳐 적용될 수 있을 때, 이 방법을 사용하는 것이 유용합니다. 예를 들면 인터넷 연결이 없을 때, 사용하는 오프라인 목록이 있다고 합시다. 먼저 아래 예시처럼 간단한 어노테이션을 정의합니다:

```java
@Target({ElementType.FIELD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier
public @interface Offline {

}
```

그 다움 이 어노테이션을 자동연결할 필드나 프로퍼티에 설정합니다:

```java
public class MovieRecommender {

    @Autowired
    @Offline /* 1 */
    private MovieCatalog offlineCatalog;

    // ...
}
```

1. 이 줄에 `@Offline`어노테이션을 추가했습니다.

이게 빈 정의에는 오직 `type` qualifier만 필요합니다:

```xml
<bean class="example.SimpleMovieCatalog">
    <qualifier type="Offline"/>  <!-- 1 -->
    <!-- 이 빈에 필요한 의존성 주입 -->
</bean>
```

1. 이 요소는 qualifier를 명시합니다.

또한 자신만의 어노테이션에 간단한 `value` 어튜리부트 대신 다양한 어트리뷰트를 가지도록 할 수 있습니다. 자동연결할 필드나 파라메터에 다양한 어트리뷰트 값이 명시되면 빈 정의에 그러한 어트리뷰트 값이 같아야 자동연결 후보자로 선택될 것입니다. 아래예시를 보십시오:

```java
@Target({ElementType.FIELD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier
public @interface MovieQualifier {

    String genre();

    Format format();
}
```

이 경우 `Format`은 아래 예시와 같은 enum 입니다.:

```java
public enum Format {
    VHS, DVD, BLURAY
}
```

`genre`와 `format` 값을 가진 어노테이션이 선언된 자동연결될 필드들의 예시입니다:

```java
public class MovieRecommender {

    @Autowired
    @MovieQualifier(format=Format.VHS, genre="Action")
    private MovieCatalog actionVhsCatalog;

    @Autowired
    @MovieQualifier(format=Format.VHS, genre="Comedy")
    private MovieCatalog comedyVhsCatalog;

    @Autowired
    @MovieQualifier(format=Format.DVD, genre="Action")
    private MovieCatalog actionDvdCatalog;

    @Autowired
    @MovieQualifier(format=Format.BLURAY, genre="Comedy")
    private MovieCatalog comedyBluRayCatalog;

    // ...
}
```

마지막으로 빈정의들은 반드시 걸맞은 qualifier 값을 가지고 있어야 합니다. 이 예시는 `<qualifier/>` 대신에 빈 메타 어트리뷰트를 사용하는 예시입니다. 가능하다면 `<qualifier/>` 요소와 어트리뷰트가 우선순위지만 `<meta/>` 태그로 제공된 메타 어트리뷰트 또한 후 순위로 받아들입니다. 아래 예시의 마지막 두 빈 정의가가 그러한 예시입니다:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

    <bean class="example.SimpleMovieCatalog">
        <qualifier type="MovieQualifier">
            <attribute key="format" value="VHS"/>
            <attribute key="genre" value="Action"/>
        </qualifier>
        <!-- 이 빈에 필요한 의존성 주입 -->
    </bean>

    <bean class="example.SimpleMovieCatalog">
        <qualifier type="MovieQualifier">
            <attribute key="format" value="VHS"/>
            <attribute key="genre" value="Comedy"/>
        </qualifier>
        <!-- 이 빈에 필요한 의존성 주입 -->
    </bean>

    <bean class="example.SimpleMovieCatalog">
        <meta key="format" value="DVD"/>
        <meta key="genre" value="Action"/>
        <!-- 이 빈에 필요한 의존성 주입 -->
    </bean>

    <bean class="example.SimpleMovieCatalog">
        <meta key="format" value="BLURAY"/>
        <meta key="genre" value="Comedy"/>
        <!-- 이 빈에 필요한 의존성 주입 -->
    </bean>

</beans>
```

<h4 id="beans-generics-as-qualifiers">제네릭을 자동연결 Qualifiers로 이용하기</h4>

`@Qualifier`뿐만 아니라, 자바 제네릭 타입 또한 후보 빈 선택에 사용할 수 있다. 예를 들면 아래와 같은 설정을 했다고 하자:

```java
@Configuration
public class MyConfiguration {

    @Bean
    public StringStore stringStore() {
        return new StringStore();
    }

    @Bean
    public IntegerStore integerStore() {
        return new IntegerStore();
    }
}
```

위 빈들이 제네릭 인터페이스의 구현체라고 하자(즉, `Store<String>`과 `Store<Integer>`). `@Autowire`와 `Store` 인터페이스, 제네릭을 이용하여 빈을 아래 예시 처럼 선택할 수 있다:

```java
@Autowired
private Store<String> s1; // <String> qualifier, stringStore 빈 주입

@Autowired
private Store<Integer> s2; // <Integer> qualifier, integerStore 빈 주입
```

제네릭 qualifier는 리스트, `Map`, 배열에도 사용가능하다. 아래는 제네릭 `List`를 사용하는 예시다:

```java
// 모든 <Integer> 제네릭 Store 빈을 주입한다.
// Store<String> 빈은 이 리스트에 주입되지 않는다.
@Autowired
private List<Store<Integer>> s;
```

<h4 id="beans-custom-autowire-configurer">CustomAutowireConfig 이용하기</h4>

[`CustomAutowireConfigurer`](https://docs.spring.io/spring-framework/docs/5.2.8.RELEASE/javadoc-api/org/springframework/beans/factory/annotation/CustomAutowireConfigurer.html)은 스프링 `@Qualifier`를 사용하지 않은 어노테이션을 자신만의 qualifier로 등록하는 `BeanFactoryPostProcessor`이다. 아래예시는 `CustomAutowireConfigurer`를 사용하는 방법을 보여준다:

```xml
<bean id="customAutowireConfigurer"
        class="org.springframework.beans.factory.annotation.CustomAutowireConfigurer">
    <property name="customQualifierTypes">
        <set>
            <value>example.CustomQualifier</value>
        </set>
    </property>
</bean>
```
`AutowireCandidateResolver`가 자동연결할 후보 빈을 아래와 같은 방법으로 결정한다:

* 각 빈 정의의 `autowire-candidate`값
* `<beans/>`요소의 `default-autowire-candidate`패턴
* `CustomAutowireConfigurer`에 등록된 어노테이션과 `@Qualifier` 어노테이션

여러개의 빈이 후보빈으로 등록되면 최우선 빈을 다음과 같이 선택한다: 정확히 한개의 빈만 `primary`로 설정되있으면 선택한다.

<h4 id="beans-resource-annotation">@Resource를 사용한 주입</h4>

스프링은 JSR-250 `@Resource`(`javax.annotation.Resource`) 를 사용하여 필드나 세터메서드 빈 주입도 지원한다. Java EE에서 흔히 사용되는 방법이다. 스프링이 관리하는 객체에도 이 패턴을 사용할 수 있다.

`@Resource`는 name 어트리뷰트를 가지고있다. 기본적으로 스프링은 해당 값을 주입할 빈 이름으로 해석한다. 즉 이름에 의한 빈 주입을 한다는 의미이다. 아래는 예시이다:

```java
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Resource(name="myMovieFinder")  // [1]
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
}
```

1. 이 줄을 통해 `@Resource` 빈 주입을 한다.

이름이 명시되지 않을 경우, 필드 이름이나 세터메서드 이름에서 기본 이름을 가져온다. 필드의 경우, 필드이름을 가져온다. 세터 메서드의 경우, 프로퍼티 이름을 가져온다. 아래 예시는 `movieFinder`라는 이름의 빈을 세터메서드를 통해 주입하는 예시이다:

```java
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Resource
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
}
```

| |
| ----- |
| ***!** `CommonAnnotationBeanPostProcessor`를 가지고 있는 `ApplicationContext`는 어노테이션에 의하여 전달된 이름을 빈 이름으로 변경한다. Spring의 `SimpleJndiBeanFactory`를 설정한다면 JNDI를 통해 이름을 변경할 수 있다. 하지만 기본 설정을 사용하는 것을 추천한다. 그러면 스프링 JNDI lookup 기능을 사용하여 간접성을 유지할 수 있다.* |

`@Resource`를 직접적으로 이름을 설정하지 않고 사용한 경우, `@Autowired`와 비슷하게 타입기반 빈 주입을 실행한다. 또한 알려진 해소 가능한 의존성을 주입한다 : `BeanFactory`, `ApplicationContext`, `ResourceLoader`, `ApplicationEventPublisher`, `MessageSource`인터페이스

따라서 아래 예시에서 `customerPreferenceDao`필드는 먼저 "customerPreferenceDao"라는 이름의 빈을 찾아본다. 그 뒤 `CustomerPreferenceDao`타입의 우선시되는 빈을 찾는다:

```java
public class MovieRecommender {

    @Resource
    private CustomerPreferenceDao customerPreferenceDao;

    @Resource
    private ApplicationContext context; // [1]

    public MovieRecommender() {
    }

    // ...
}
```

1. `context`필드는 알려진 해소가능한 의존성 `ApplicationContext`타입에 의하여 빈 주입되었다.


<h4 id="beans-value-annotations">@Value 사용하기</h4>

`@Value`는 일반적으로 외후 프로퍼티 주입에 사용된다.

```java
@Component
public class MovieRecommender {

    private final String catalog;

    public MovieRecommender(@Value("${catalog.name}") String catalog) {
        this.catalog = catalog;
    }
}
```
함께 사용된 설정 :
```java
@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig { }
```
`application.properties` 파일:
```java
catalog.name=MovieCatalog
```

이 경우에 `catalog`파라메터와 필드는 `MovieCatalog`라는 값을 가진다.

스프링의 기본 내장된 값 처리기는 후한 설정을 가지고 있다. 프로퍼티값을 설정을 시도한 뒤, 실패하면 프로퍼티 이름 (예를 들면 `${catalog.name}`)을 값으로 주입한다. 존재하지 않는 값들에 엄격한 통제를 하고 싶다면 `PropertySourcesPlaceholderConfigurer`빈을 아래 예시처럼 설정해야한다:

```java
@Configuration
public class AppConfig {

     @Bean
     public static PropertySourcesPlaceholderConfigurer propertyPlaceholderConfigurer() {
           return new PropertySourcesPlaceholderConfigurer();
     }
}
```

| |
| ----- |
| ***!** `PropertyPlaceholderConfigurer`를 자바 설정으로 설정할 경우, `@Bean`메서드는 반드시 `static`이여야 한다.* |

위 설정을 사용하면 `${}` 플레이스홀더에 값 주입을 할 수 없을 경우 초기화에 실패한다. 또한 `setPlaceholderPrefix`,`setPlaceholderSuffix`,`setValueSeparator`와 같은 플레이스홀더 설정 수정을 할 수 있는 메서드를 제공한다.

| |
| ----- |
| ***!** `application.properties`와 `application.yml`로부터 프로퍼티를 읽어오는 `PropertySourcesPlaceholderConfigurer`빈이 스프링 부트에서는 자동적으로 설정된다. *|

스프링에 포함된 컨버터가 (`int`나 `Integer`와 같은) 단순한 타입 변경을 자동으로 진행한다. 콤마로 분리된 값들을 자동으로 문자열 배열로 변경한다.

아래 예시처럼 기본값을 설정하는 것도 가능하다:

```java
@Component
public class MovieRecommender {

    private final String catalog;

    public MovieRecommender(@Value("${catalog.name:defaultCatalog}") String catalog) {
        this.catalog = catalog;
    }
}
```

스프링의 `BeanPostProcessor`는 내부적으로 `ConversionService`를 사용하여 `@Value`의 문자열 값을 목표 타입의 값으로 변경한다. 새로운 타입에도 변경을 지원하고자 한다면 `ConversionService`를 작성하여 제공하여야 한다. 아래는 예시이다:

```java
@Configuration
public class AppConfig {

    @Bean
    public ConversionService conversionService() {
        DefaultFormattingConversionService conversionService = new DefaultFormattingConversionService();
        conversionService.addConverter(new MyCustomConverter());
        return conversionService;
    }
}
```

만약 `@Value`가 [`SpEL` 표현식](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/core.html#expressions)을 포함하고 있으면 런타임에 동적으로 값이 생성될 것이다. 아래는 예시이다:

```java
@Component
public class MovieRecommender {

    private final String catalog;

    public MovieRecommender(@Value("#{systemProperties['user.catalog'] + 'Catalog' }") String catalog) {
        this.catalog = catalog;
    }
}
```

SpEL은 또한 더 복잡한 데이터 구조를 사용할 수 있도록 한다:

```java
@Component
public class MovieRecommender {

    private final Map<String, Integer> countOfMoviesPerCatalog;

    public MovieRecommender(
            @Value("#{ {'Thriller': 100, 'Comedy': 300} }") Map<String, Integer> countOfMoviesPerCatalog) {
        this.countOfMoviesPerCatalog = countOfMoviesPerCatalog;
    }
}
```


<h4 id="beans-postconstruct-and-predestroy-annotations">@PostConstruct와 @PreDestory 사용하기</h4>

`CommonAnnotationBeanPostProcessor`는 `@Resource` 어노테이션처리 뿐만 아니라 JSR-250 생명주기 어노테이션도 처리한다. (`javax.annotation.PostConstruct`, `javax.annotation.PreDestroy`) 스프링 2.5부터 이러한 어노테이션을 지원하면서 [콜백 정의하기](#beans-factory-lifecycle-initializingbean)와 [소멸자 콜백](#beans-factory-lifecycle-disposablebean)에 설명된 방식 이외의 생명주기 콜백 작동 방식을 제공한다. 스프링 `ApplicationContext`내부에 등록된 `CommonAnnotationBeanPostProcessor`를 통해 스프링 생명주기 콜백이나 직접 정의된 콜백 메서드와 같은 시기에 동작한다. 아래 예시는 초기화 단계에 캐시를 생성하고 소멸시에 제거하는 예시이다:

```java
public class CachingMovieLister {

    @PostConstruct
    public void populateMovieCache() {
        // 초기화 단계에 캐시를 생성한다...
    }

    @PreDestroy
    public void clearMovieCache() {
        // 소멸 단계에 캐시를 제거한다....
    }
}
```

여러 생명주기 동작방법을 섞을 경우 효과에 대한 자세한 내용은 [생명주기 작동원리 조합하기](#beans-factory-lifecycle-combined-effects)에서 볼 수 있다.

| |
| ----- |
| ***@** `@Resource`처럼 `@PostConstruct`와 `@PreDestory`어노테이션은 JDK 6~8에 표준 자바 라이브러리였다. 하지만 `javax.annotation`패키지 전체가 JDK 9에서 자바 핵심 모듈과 분리되었고 JDK 11에서는 제거되었다. 만약 필요하다면 `javax.annotation-api` 아티펙트를 메이븐 중앙 저장소에서 얻어올 수 있다. 다른 라이브러리처럼 단순히 클래스패스에 추가하는 것으로 충분하다.* |


<h3 id="beans-classpath-scanning">클래스패스 스캐닝과 관리되는 컴포넌트</h3>

이 장은 스프링 컨테이너 내부에 `BeanDefinition`을 만드는 설정 메타데이터를 XML로 표현하는 예제들 위주로 구성되었다. 이전 섹션 ([어노테이션 기반 컨테이너 설정](#beans-annotation-config))에서는 소스레벨 어노테이션으로 설정 메타데이터를 제공하는 방법을 설명했다. 하지만 이러한 예제에서도 기반이되는 빈 정의는 XML파일로 작성되었고 어노테이션은 의존성 주입에만 사용되었다. 이 장에서 클래스패스를 스캔하여 후보 컴포넌트를 찾는 방법을 설명해줍니다. 필터 기준에 맞고 컨테이너에 등록된 빈 정의가 있어야 후보 컴포넌트이다. 클래스패스 스캔은 빈 등록할 때 XML을 사용할 필요를 없앤다. 대신 어노테이션(예를 들면, `@Component`), AspectJ 타입 표현식, 커스텀 필터를 사용하여 컨테이너에 등록된 빈 정의 중 선택한다.

| |
| ----- |
| ***!** 스프링 3.0부터 스프링 자바설정에 제공되는 많은 기능이 스프링 프레임워크 핵심에 포함되었다. 이 덕분에 XML 대신 자바를 사용해서 빈을 정의할 수 있다. 새 기능을 사용하는 방법을 확인하려면 `@Configuration`,`@Bean`,`@Import`,`@DependsOn`어노테이션 예시를 보십시오.* |

<h4 id="beans-stereotype-annotations">@Component와 추가적인 정형화된 어노테이션</h4>

`@Repository` 어노테이션은 리포지토리(DAO, Data Access Object로도 알려져있다.) 역할을 하는 클래스에 사용되는 표시이다. 이 표시를 사용하면 예외를 자동으로 변경하여 처리한다. [예외 번역](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/data-access.html#orm-exception-translation)에 자세한 내용이 있습니다.

스프링은 몇가지 정형화된 어노테이션을 추가적으로 제공한다: `@Component`, `@Service`, `@Controller`. `@Component`는 스프링에서 관리하는 컴포넌트에 사용되는 일반적인 어노테이션이다. `@Repository`, `@Service`, `@Controller`는 `@Component`의 일종으로 특정한 경우에 사용된다 (각각 영속성, 서비스, 프레젠테이션 레이어에 사용된다). 따라서 컴포넌트 클래스에 `@Component`대신 `@Repository`,`@Service`,`@Controller`를 사용할 수 있다. 이렇게 하면 어스펙트나 도구에 의해 더 적절하게 처리될 수 있다. 예를 들면 이러한 어노테이션이 AOP 포인트컷을 만드는데 사용될 수 있다. `@Repository`,`@Service`,`@Controller`는 스프링 프레임워크 추후 발표될 버전에서 추가적인 의미를 가질수 있다. 따라서 서비스 레이어에 `@Service`를 사용할지 `@Component`를 사용할 지 고민중이라면 `@Service`가 분명히 더 좋은 선택이다. 비슷하게 `@Repository`는 예외 번역을 진행하는 영속성레이어 표시로서 이미 사용되고 있다.

<h4 id="beans-meta-annotations">메타 어노테이션과 복합 어노테이션 이용하기</h4>

스프링에서 제공하는 많은 어노테이션을 메타 어노테이션으로 사용할 수 있다. 메타 어노테이션은 다른 어노테이션에 사용될 수 있는 어노테이션을 말한다. 아래 예시에서 알 수 있듯이 [이전 장](#beans-stereotype-annotations)에서 언급된 `@Service` 어노테이션은 `@Component`어노테이션이 적용된 메타 어노테이션이다:

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component // [1]
public @interface Service {

    // ...
}
```

1. `Component`는 `@Service`가 `@Component`와 같은 방식으로 처리되게 한다.

메타 어노테이션을 섞어 복합 어노테이션을 만들 수 있다. 예를 들면, 스프링 MVC의 `@RestController` 어노테이션은 `@Controller`와 `@ResponseBody`를 합한 것이다.

복합 어노테이션을 이용하여 메타어노테이션의 어트리뷰트를 재정의 할 수 있다. 메타 어노테이션의 일부 하위 부분만 노출하고 싶을 때에 유용하다. 예를 들면, 스프링의 `@SessionScope`는 `session` 스코프로 하드 코딩되있으며 `proxyMode`를 변경할 수 있다. 아래는 `SessionScope`의 정의이다.

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Scope(WebApplicationContext.SCOPE_SESSION)
public @interface SessionScope {

    /**
     * Alias for {@link Scope#proxyMode}.
     * <p>Defaults to {@link ScopedProxyMode#TARGET_CLASS}.
     */
    @AliasFor(annotation = Scope.class)
    ScopedProxyMode proxyMode() default ScopedProxyMode.TARGET_CLASS;

}
```

아래 예시처럼 `SessionSoce`를 `proxyMode`정의 없이 사용할 수 있다:

```java
@Service
@SessionScope
public class SessionScopedService {
    // ...
}
```

아래 예시처럼 `proxyMode`를 새 값으로 덮어쓸 수 있다:

```java
@Service
@SessionScope(proxyMode = ScopedProxyMode.INTERFACES)
public class SessionScopedUserService implements UserService {
    // ...
}
```

[스프링 어노테이션 처리 모델](https://github.com/spring-projects/spring-framework/wiki/Spring-Annotation-Programming-Model) 위키 페이지에 자세한 내용이 있습니다.

<h4 id="beans-scanning-autodetection">클래스 검색과 빈 정의 등록을 자동으로 수행하기 </h4>

스프링은 자동으로 스테레오타입 어노테이션이 적용된 클래스를 찾아서 `ApplicationContext`에 `BeanDefinition` 인스턴스를 등록한다. 예를 들어 아래 두 클래스는 자동 발견 대상이다:

```java
@Service
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
}
```

```java
@Repository
public class JpaMovieFinder implements MovieFinder {
    // 명료성을 위해 구현은 생략됨
}
```

이 클래스들을 자동으로 찾아서 빈으로 등록하기 위해서는 `@Configuraion` 클래스에 `@ComponentScan`을 추가하고 `basePackages`어트리뷰트에 두 클래스의 공통 부모 패키지로 설정하면된다. (또는 `,`,`;`,` `로 구분된 패키지 리스트로 설정해도 된다.)

```java
@Configuration
@ComponentScan(basePackages = "org.example")
public class AppConfig  {
    // ...
}
```

| |
| ----- |
| ***!** 간략하게 적기 위하여 위 예시에서 어노테이션의 `value`어트리뷰트를 사용할 수 있다. (`@ComponentScan("org.example")`)* |

또는 아래의 XML을 사용할 수 있다:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="org.example"/>

</beans>
```

| |
| ----- |
| ***@** `<context:component-scan>`은 내부적으로 `<context:annotation-config>`의 기능을 활성화한다. 따라서 일반적으로 `<context:component-scan>`를 사용할 때 `<context:annotation-config>`를 사용할 필요가 없다.* |

| |
| ----- |
| ***!** 클래스패스 패키지 스캔을 하기 위해서는 해당 디렉토리가 클래스패스에 있어야한다. Ant로 JARs를 빌드할 때, files-only를 활성화하지 않아야한다. 또한 클래스패스 디렉토리는 일부 환경에서 보안 정책상 노출되지 않을 수 있다 - 예를 들면 JDK 1.7.0_45나 그 이상의 독립어플리케이션과 같은 환경 ( manifest에 'Trusted-Library`설정이 필요한 환경 - [https://stackoverflow.com/questions/19394570/java-jre-7u45-breaks-classloader-getresources](https://stackoverflow.com/questions/19394570/java-jre-7u45-breaks-classloader-getresources)를 보세요).* |
| *JDK 9의 모듈 패스 (jigsaw)에서 스프링 클래스패스 스캔은 예상대로 동작한다. 하지만 컴포넌트 클래스들이 `module-info` 설명자에 익스포트 되어야 한다. 스프링이 퍼블릭이 아닌 멤버들을 호출하길 원한다면 `module-info`설명자에 `exports`대신 `opens`를 사용하면 된다.* |

추가적으로 컴포넌트 스캔 요소를 사용할 때, `AutowiredAnnotationBeanPostProcessor`와 `CommonAnnotationBeanPostProcessor`는 둘다 내부적으로 포함된다. 이 말은 이 두 컴포넌트들은 자동으로 발견되어 연결된다 - XML에 설정 메타데이터를 설정하지 않아도. 

| |
| ----- |
| ***!** `annotation-config`어트리뷰트를 `false`로 하여 `AutowiredAnnotationBeanPostProcessor`와 `CommonAnnotationBeanPostProcessor`를 등록하지 않을 수 있다.* |

<h4 id="beans-scanning-filters">필터를 이용하여 스캐닝 커스터마이징하기</h4>

`@Component` 어노테이션이 적용된 커스텀 어노테이션, `@Component`,`@Repository`,`@Service`,`@Controller`,`@Configuration`이 적용된 클래스들을 기본적으로 후보 컴포넌트로 등록한다. 하지만 커스텀 필터를 적용하여 이 동작을 변경하거나 확장할 수 있다. `@ComponentScan`어노테이션의 `includeFilters`나 `excludeFilters`어트리뷰트(혹은 `<context:component-scan>`하위 `<context:include-filter/>`요소나 `<context:exclude-filter/>`요소)로 설정하면 된다. 각 필터는 `type`과 `expression`어트리뷰트를 설정해야한다. 아래 표는 필터링 옵션을 보여준다:

#### 표 5. 필터 타입

| 필터 타입 | 예시 표현 | 설명 |
| ----- | ----- | ----- |
| annotation (기본값) | `org.example.SomeAnnotation` | 목표 컴포넌트의 타겟레벨에 존재하는 어노테이션 (혹은 그 어노테이션의 메타 어노테이션) |
| assignable | `org.example.SomeClass` | 목표 컴포넌트 클래스의 자식클래스나 목표 컴포넌트 인터페이스의 구현체인 클래스 |
| aspectj | `org.example..*Service+` | 목표 컴포넌트를 설정하는 Aspectj 표현식 |
| regex | `org\.example\.Default.*` | 목표 컴포넌트를 설정하는 정규표현식 |
| custom | `org.example.MyTypeFilter` | `org.springframework.core.type.TypeFilter`인터페이스의 커스텀 구현체 |

다음은 `@Repository` 어노테이션을 무시하고 "stub" 리포지토리를 대신 사용하는 설정이다:

```java
@Configuration
@ComponentScan(basePackages = "org.example",
        includeFilters = @Filter(type = FilterType.REGEX, pattern = ".*Stub.*Repository"),
        excludeFilters = @Filter(Repository.class))
public class AppConfig {
    ...
}
```

다음은 동일한 동작을 하는 XML 예시이다:

```xml
<beans>
    <context:component-scan base-package="org.example">
        <context:include-filter type="regex"
                expression=".*Stub.*Repository"/>
        <context:exclude-filter type="annotation"
                expression="org.springframework.stereotype.Repository"/>
    </context:component-scan>
</beans>
```

| |
| ----- |
| ***!** 어노테이션에 `useDefaultFilters=false`를 설정하거나 `<component-scan>`에 `use-default-filters="false"`를 설정해서 기본 필터를 제거할 수 있다. 이는 `@Component`, `@Repository`, `@Service`, `@Controller`, `@RestController`, `@Configuration`어노테이션이 설정된 클래스 자동 발견을 취소하는 효율적인 방법이다.* |

<h4 id="beans-factorybeans-annotations">컴포넌트 내부에 빈 메타데이터 정의하기</h4>

스프링 컴포넌트가 컨테이너의 빈 메타데이터에 영향을 끼칠 수 있다. `@Configuration`어노테이션이 적용된 클래스에 `@Bean`어노테이션을 사용하여 빈 메타데이터를 정의할 수 있다. 아래는 그 예시이다.

```java
@Component
public class FactoryMethodComponent {

    @Bean
    @Qualifier("public")
    public TestBean publicInstance() {
        return new TestBean("publicInstance");
    }

    public void doWork() {
        // 컴포넌트 메소드 구현은 생략되었다.
    }
}
```

이 클래스는 어플리케이션 로직 `doWork()`메소드를 가진 스프링 컴포넌트이다. 또한 `publicInstance()`라는 팩토리 메소드를 가진 빈 정의를 등록한다. `@Bean`어노테이션은 팩토리 메서드와 다른 빈 정의 프로퍼티(예를 들면 `@Qualifier`어노테이션을 이용하여 설정하는 qualifier값)를 설정한다. 커스텀 qualifier어노테이션, `@Scope`, `@Lazy`등의 메소드 레벨 어노테이션도 적용할 수 있다.

| |
| ----- |
| ***!** `@Lazy`어노테이션은 컴포넌트의 지연된 초기화를 설정하는 역할을 한다. 또한 `@Autowired`나 `@Inject`로 표시된 의존선 주입 지점에 설정할 수 있다. 이렇게 하면, 지연된 처리를 하는 프록시를 주입할 수 있다.* |

필드나 메소드의 자동연결도 지원한다. 이전에 이야기한 것처럼 `@Bean`메소드에 추가적으로 지원하는 것이다. 아래는 그 예시이다:

```java
@Component
public class FactoryMethodComponent {

    private static int i;

    @Bean
    @Qualifier("public")
    public TestBean publicInstance() {
        return new TestBean("publicInstance");
    }

    // 커스텀 qualifier를 사용하여 메소드 파라메터를 자동연결한다.
    @Bean
    protected TestBean protectedInstance(
            @Qualifier("public") TestBean spouse,
            @Value("#{privateInstance.age}") String country) {
        TestBean tb = new TestBean("protectedInstance", 1);
        tb.setSpouse(spouse);
        tb.setCountry(country);
        return tb;
    }

    @Bean
    private TestBean privateInstance() {
        return new TestBean("privateInstance", i++);
    }

    @Bean
    @RequestScope
    public TestBean requestScopedInstance() {
        return new TestBean("requestScopedInstance", 3);
    }
}
```

이 예제는 `String`메소드 파라메터 `country`에 `privateInstance`의 `age`프로퍼티의 값을 넣어 자동연결한다. 스프링 표현언어(SpEL)은 프로퍼티의 값을 `#{<expression>}` 표현으로 정의한다. `@Value`어노테이션에 표현언어 처리기가 미리 설정되어 빈 이름을 찾아 표현언어를 해석한다.

스프링 4.3부터 `InjectionPoint`(혹은 자식클래스인 `DependencyDescriptor`)를 팩토리 메서드의 파라메터로 정의하여 해당 빈의 생성을 요청하는 의존성 주입 지점에 접근할 수 있다. 이미 생성된 빈의 주입 지점이 아니라 생성하는 빈의 주입 지점인 것에 주의하여야 한다. 결론적으로, 프로토타입 스코프를 가진 빈에 걸맞는 방법이다. 다른 스코프에서는 빈이 생성되는 의존성 주입지점에만 동작한다(에를 들면 지연초기화 싱글톤 빈을 생성시키는 의존성 주입 지점). 이런 경우 의존성 주입 메타데이터를 제공할 수 있다. 아래는 `InjectionPoint`를 사용하는 예시이다:

```java
@Component
public class FactoryMethodComponent {

    @Bean @Scope("prototype")
    public TestBean prototypeInstance(InjectionPoint injectionPoint) {
        return new TestBean("prototypeInstance for " + injectionPoint.getMember());
    }
}
```

일반적인 스프링 컴포넌트의 `@Bean`메소드는 `@Configuration`클래스 내부의 `@Bean`메소드와 다르게 동작한다. `@Component`클래스는 메소드와 필드 호출시 CGLIB을 이용한 중간 처리가 되지 않는다. CGLIB 프록시는 `@Configuration`클래스의 `@Bean`메소드 내부 필드나 메소드를 호출하는 것이 해당되는 메타데이터 레퍼런스를 생성한다는 것이다. 이러한 메소드는 평범한 자바 로직과 같은 의미를 가지지 않는다. 이러한 메소드는 스프링 컨테이너를 통하여 처리되어 스프링 생명주기의 관리를 받게된다. 심지어 `@Bean`메소드를 프로그래밍적으로 호출하여도 다른 빈을 참조하게된다. 반면에 `@Component`클래스에서 `@Bean`메소드를 호출하는 것은 일반적인 자바 로직을 따르게 된다.

| |
| --- |
| ***!** `@Bean`메소드를 `static`으로 선언하여 메소드가 선언된 설정클래스의 인스턴스를 만들 필요없이 빈을 생성할 수 있도록 할 수 있다.  이 방법은 빈 후처리기(예를 들면 `BeanFactoryPostProcessor`나 `BeanPostProcessor`)를 정의할 때 매우 좋다. 왜냐하면 이러한 빈들은 컨테이너 생명주기 초반에 생성되며 설정의 다른 부분이 동작하지 않아야 하기 때문이다.<br>static `@Bean`메소드의 호출은 컨테이너에 의해 처리되지 않는다. CGLIB이 static 메소드를 재정의 하지 않는다는 기술적인 한계 때문에 `@Configuration`클래스 내부에서도 마찬가지이다. 결과적으로 `@Bean`메소드는 일반적인 자바 로직을 따라며 호출할 때마다 메소드 그 자체를 통하여 인스턴스를 받게된다<br>`@Bean`메소드는 또한 컴포넌트나 설정 클래스에서도 사용할 수 있다. 또한 Java 8 인터페이스의 기본 메소드 구현에서도 사용할 수 있다. 이 방법을 통해 아주 복잡한 설정을 할 수 있는 확장성을 제공한다. 스프링 4.2부터는 Java 8 인터페이스의 기본 메소드가 여러번의 상속을 통해도 동작한다.<br>마지막으로 하나의 클래스가 같은 빈을 생성하는 `@Bean`메소드를 여러개 가질 수 있다. 각각 다른 의존성을 가지는 팩토리 메소드 여러개를 설정하는 것이다. 이는 "greediest" 생성자 선택과 같은 알고리즘이다: "greediest" 생성자 선택 - 여러 개의 `@Autowired`가 설정된 생성자 중에서 가장 많은 의존성 주입이 가능한 생성자를 선택하여 생성하는 방법.* |

<h4 id="beans-scanning-name-generator">자동 검색된 컴포넌트 이름 붙이기</h4>

자동 검색 과정에서 컴포넌트가 검색되면 스프링은 `BeanNameGenerator`을 이용하여 빈 이름을 생성한다. 기본적으로 스프링 스테레오타입 어노테이션 (`@Component`,`@Repository`,`@Service`,`@Contorller`)는 `value`를 가지고 있고 이것을 이름으로 사용한다.

`value`설정되지 않은 경우나 다른 방법으로 검색된 컴포넌트들은 클래스이름의 첫글자가 소문자로 하여 이름을 가지게 된다. 예를 들면 아래 컴포넌트 클래스가 검색되면 `myMovieLister`와 `movieFinderImpl`이라는 이름을 가진다:

```java
@Service("myMovieLister")
public class SimpleMovieLister {
    // ...
}
```

```java
@Repository
public class MovieFinderImpl implements MovieFinder {
    // ...
}
```

기본 빈 이름 생성 방식을 사용하고 싶지 않다면 커스텀 빈 이름 생성기를 설정할 수 있다. 먼저 `BeanNameGenerator`인터페이스를 어규면트가 없는 생성자를 포함하여 구현한다. 그 뒤, 이름 생서기의 패키지를 포함한 전체 클래스 이름을 작성하면 된다. 아래의 어노테이션과 빈 정의가 그 예시이다:

| |
| --- |
| ***@** 여러 컴포넌트가 같은 이름을 요청하여 충돌이 발생한 경우(예를 들면, 서로 다른 패키지에 같은 이름을 가지는 클래스가 있는 경우), 패키지를 포함한 클래스이름을 빈 이름으로 설정하는 `BeanNameGenerator`를 설정해야 할 수도 있다. 스프링 5.2.3부터 제공되는 `org.springframework.context.annotation`패키지의 `FullyQualifiedAnnotationBeanNameGenerator`가 그 역할을 한다.* |

```java
@Configuration
@ComponentScan(basePackages = "org.example", nameGenerator = MyNameGenerator.class)
public class AppConfig {
    // ...
}
```

```xml
<beans>
    <context:component-scan base-package="org.example"
        name-generator="org.example.MyNameGenerator" />
</beans>
```

같은 이름이 여러개 가지게 될 경우, 어노테이션에 충돌하지 않는 이름을 설정하는 것을 일반적인 규칙으로 하는 것이 좋다. 그렇지 않은 경우에는 자동 생성된 이름으로 충분하다.

<h4 id="beans-scanning-scope-resolver">자동 검색된 컴포넌트에 스코프 부여하기</h4>

일반적으로 스프링에서 관리되는 컴포넌트의 가장 흔한 스코프이자 기본 값인 스코프는 `singleton`이다. 하지만 다른 스코프가 필요할 때, `@Scope` 어노테이션을 사용하여 설정할 수 있다. 아래 예시처럼 스코프의 이름을 어노테이션에 설정하면 된다:

```
@Scope("prototype")
@Repository
public class MovieFinderImpl implements MovieFinder {
    // ...
}
```

| |
| --- |
| ***!** `@Scope`어노테이션은 concrete 클래스(어노테이션이 적용된 컴포넌트)나 팩토리 메소드(`@Bean` 메소드)에만 사용할 수 있다. 반면에 XML 빈 정의에서, 클래스의 상속은 메타데이터와 무관하고 빈 정의 상속 개념이 없다.* |

"request", "session"처럼 웹 환경에서만 가능한 스코프에 대한 자세한 내용은 
[Request, Session, Application, WebSocket 스코프](#beans-factory-scopes-other)를 보십시오. 이러한 스코프에 적용된 어노테이션을 사용하면 스프링의 메타 어노테이션과 같은 방식으로 자신만의 스코프 어노테이션을 개발할 수 있을 것입니다: 예를 들면, `@Scope("prototype")`을 적용한 커스텀 어노테이션으로 커스텀 스코프 프록시 모드를 선언할 수 있을 것이다.

| |
| --- |
| ***!** 어노테이션 기반 스코프이외에 커스텀 스코프를 만들고자 한다면 `ScopMetadataResolver`인터페이스를 구현하여 처리할 수 있다. 어규먼트가 없는 생성자가 있는 구현체를 개발하여야 한다. 그 뒤 스캐너를 설정할 때 패키지를 포함한 클래스 이름을 명시하면 된다. 아래는 그 예시를 어노테이션 기반과 XML기반 두가지로 보여준다:* |

```java
@Configuration
@ComponentScan(basePackages = "org.example", scopeResolver = MyScopeResolver.class)
public class AppConfig {
    // ...
}
```

```xml
<beans>
    <context:component-scan base-package="org.example" scope-resolver="org.example.MyScopeResolver"/>
</beans>
```

싱글톤이 아닌 특정한 스코프를 사용한다면 스코프를 사용하는 객체에 프록시를 생성해야 할 필요가 있을 수 있다. 그 이유는 [생성할 프록시 타입 정하기](#beans-factory-scopes-other-injection)에 설명되어 있다. 이러한 이유로 component-scan요소에 스코프 프록시 어트리뷰트가 존재한다. 이 어트리뷰트에 설정가능한 값으로는 `no`,`interface`,`targetClass`가 있다. 아래는 표준 JDK 동적 프록시를 생성하는 설정의 예시이다:

```java
@Configuration
@ComponentScan(basePackages = "org.example", scopedProxy = ScopedProxyMode.INTERFACES)
public class AppConfig {
    // ...
}
```

```xml
<beans>
    <context:component-scan base-package="org.example" scoped-proxy="interfaces"/>
</beans>
```

<h4 id="beans-scanning-qualifiers">Qualifier 메타데이터 어노테이션으로 부여하기</h4>

`@Qualifier`어노테이션은 [Qualifiers를 이용하여 어노테이션 기반 자동 연결 미세 조정하기](#beans-autowired-annotation-qualifiers)에서 언급되었었다. 자동연결 후보빈들을 처리하는데 미세한 컨트롤을 할 수 있는 커스텀 후보자 어노테이션과 `@Qualifier`어노테이션의 사용 예제를 해당 섹션에서 볼 수 있었다. 해당 예제들은 XML로 작성되어 왔기에 후보빈 정의는 `bean`요소의 자식 요소인`qualifier`나 `meta`요소에 작성되어왔다. 컴포넌트 스캐닝에 의하여 자동 팔견된느 경우, qualifier 메타데이터를 타입레벨 어노테이션을 이용하여 설정할 수 있다. 아래는 그러한 예제이다:

```java
@Component
@Qualifier("Action")
public class ActionMovieCatalog implements MovieCatalog {
    // ...
}
```

```
@Component
@Genre("Action")
public class ActionMovieCatalog implements MovieCatalog {
    // ...
}
```

```
@Component
@Offline
public class CachingMovieCatalog implements MovieCatalog {
    // ...
}
```

| |
| --- |
| ***!** 대부분의 어노테이션 기반 대체제들이 그러하듯이 어노테이션 메타데이터는 클래스 정의 자체에 묶여있다. 반면에 XML을 사용하면 같은 타입의 여러 빈에 다양한 qualifier정보를 설정할 수 있다. 왜냐하면 클래스에 설정되는 qualifier가 아니라 인스턴스에 설정되는 qualifier이기 때문이다.* |

<h4 id="beans-scanning-index">후보 컴포넌트 색인 생성하기</h4>

클래스패스 스캐닝은 매우 빠르지만 컴파일 타임에 후보빈 리스트를 제공함으로서 커다란 어플리케이션의 시작 성능을 향상시킬수 있다. 이러한 방법을 사용할 시, 컴포넌트 스캔의 대상이 되는 모든 모듈이 같은 방법을 사용해야 한다.

| |
| --- |
| ***!** `@ComponentScan`이나 `<context:component-scan>`은 특정 패키지에 있는 후보를 스캔하도록 컨텍스트에 요청하는 것이다. `ApplicationContext`가 후보 빈 색인을 발견하면 클래스패스 스캔대신 해당 색인을 이용할 것이다.* |

색인을 생성하려면 컴포넌트 스캔의 대상이되는 컴포넌트를 포함하는 모든 모듈에 추가적인 의존성이 필요하다. 아래는 Maven으로 해당 설정을 하는 예시이다:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context-indexer</artifactId>
        <version>5.2.6.RELEASE</version>
        <optional>true</optional>
    </dependency>
</dependencies>
```

Gradle 4.5나 이전 버전에서는 해당 의존성은 `compileOnly`설정으로 선언되어야한다. 아래는 그 예시이다:

```Groovy
dependencies {
    compileOnly "org.springframework:spring-context-indexer:5.2.6.RELEASE"
}
```


Gradle 4.6나 이후 버전에서는 해당 의존성은 `annotationProcessor`설정으로 선언되어야한다. 아래는 그 예시이다:

```Groovy
dependencies {
    annotationProcessor "org.springframework:spring-context-indexer:5.2.6.RELEASE"
}
```

이 과정을 진행하면 `META-INF/spring.components`파일이 생성되어 jar에 포함된다.

| |
| --- |
| ***!** IDE에서 이 모드를 사용하면 `spring-context-indexer`가 등록되어 있어야 후보빈이 변경될 때에도 색인이 최신화 될것이다.* |
| ***@** 클래스패스에서 `META-INF/spring.components`가 발견되면 자동으로 색인이 사용될 것이다. 일부 라이브러리에서는 색인이 사용가능하나 전체 어플리케이션에서 사용할 수 있는 상황이 아니라면, 클래스패스 루트에 있는 `spring.properties`파일이나 시스템 프로퍼티의 `spring.index.ignore`를 `true`로 설정하여 일반적인 클래스패스를 이용하도록 대체할 수 있다.* |


<h3 id="beans-standard-annotations">JSR 330 표준 어노테이션 사용하기</h3>

스프링 3.0부터 스프링은 JSR-330 표준 어노테이션(의존성 주입)을 지원한다. 스프링 어노테이션과 같은 방법으로 스캔된다. 이 어노테이션을 이용하려면 관련된 jar가 클래스패스에 있어야한다.

| |
| --- |
| ***!** Mavan을 사용하면 `javax.inject` 아티펙트가 표준 메이븐 저장소([https://repo1.maven.org/maven2/javax/inject/javax.inject/1/](https://repo1.maven.org/maven2/javax/inject/javax.inject/1/))에서 사용가능하다. 아래 의존성을 pom.xml에 추가하여 설정할 수 있다:* |

```xml
<dependency>
    <groupId>javax.inject</groupId>
    <artifactId>javax.inject</artifactId>
    <version>1</version>
</dependency>
``` 


<h4 id="beans-inject-named">@Inject와 @Named를 이용한 의존성 주입</h4>

`@Autowired`대신 `@javax.inject.Inject`를 대신 사용할 수 있다:

```java
import javax.inject.Inject;

public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Inject
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    public void listMovies() {
        this.movieFinder.findMovies(...);
        // ...
    }
}
```

`@Autowired`처럼 `@Inject`는 필드, 메서드, 생성자 어규먼트에 사용될 수 있다. 또한 `Provider`에 의존성 주입지점을 설정하면 `Provider.get()`을 호출하여 지연 초기화 빈이나 짧은 스코프를 가지는 빈을 가져올 수 있다. 아래는 다양한 사용 예제이다:

```java
import javax.inject.Inject;
import javax.inject.Provider;

public class SimpleMovieLister {

    private Provider<MovieFinder> movieFinder;

    @Inject
    public void setMovieFinder(Provider<MovieFinder> movieFinder) {
        this.movieFinder = movieFinder;
    }

    public void listMovies() {
        this.movieFinder.get().findMovies(...);
        // ...
    }
}
```

주입해야하는 의존성에 이름이 있다면, `@Named`어노테이션을 이용하여 아래 예시처럼 설정할 수 있다:

```java
import javax.inject.Inject;
import javax.inject.Named;

public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Inject
    public void setMovieFinder(@Named("main") MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```

`@Autowired`처럼 `@Inject`는 `java.util.Optional`이나 `@Nullable`과 같이 사용될 수 있다. `Inject`는 `required`어트리뷰트를 가지고 있지 않기때문에 더욱 적합하다. `@Inject`와 `@Nullable`을 사용하는 예시이다:

```java
public class SimpleMovieLister {

    @Inject
    public void setMovieFinder(Optional<MovieFinder> movieFinder) {
        // ...
    }
}
```

```java
public class SimpleMovieLister {

    @Inject
    public void setMovieFinder(@Nullable MovieFinder movieFinder) {
        // ...
    }
}
```

<h4 id="beans-named">@Named와 @ManagedBean: @Component 어노테이션의 표준 표현</h4>

`@Component`대신에 `@javax.inject.Named`나 `javax.annotation.ManagedBean`을 사용할 수 있다. 아래는 그 예시이다:

```java
import javax.inject.Inject;
import javax.inject.Named;

@Named("movieListener")  // @ManagedBean("movieListener") 를 사용할 수도 있다.
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Inject
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```

`@Component`를 이름을 명시하지 않고 종종 사용한다. `@Named`도 같은 방법으로 사용할 수 있다. 아래는 그 예시이다:

```java
import javax.inject.Inject;
import javax.inject.Named;

@Named
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Inject
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```

`@Named`나 `@ManagedBean`을 사용할 때, 컴포넌트 스캐닝을 스프링 어노테이션을 사용할 때와 같은 방법으로 사용할 수 있다. 아래는 그 예시이다:

```java
@Configuration
@ComponentScan(basePackages = "org.example")
public class AppConfig  {
    // ...
}
```

| |
| --- |
| ***!** `@Component`와 달리 JSR-330 `@Named`와 JSR-250 `@ManagedBean`은 다른 어노테이션을 구성할 수 없다. 커스텀 어노테이션을 사용하려면 스프링 어노테이션을 사용해야 한다.* |

<h4 id="beans-stardard-annotations-limitations">JSR-330 표준 어노테이션의 한계</h4>

표준 어노테이션을 사용할 때, 중요한 특징을 알아야한다. 아래 표에 설명되어 있다:

#### 표6. 스프링 컴포넌트요소 VS JSR-330

| 스프링 | javax.inject.* | javax.inject 제한 / 주석 |
| --- | --- | ---- |
| @Autowired | @Inject | `@Inject`는 'required' 요소가 없다. 대신 Java 8의 `Optional`과 함께 사용 가능하다 |
| @Component | @Named / @ManagedBean | JSR-330 어노테이션은 다른 어노테이션을 구성할 수 없다. 이름을 통해서만 컴포넌트 구분이 가능하다. |
| @Scope("singleton") | @Singleton | JSR-330의 기본 스코프는 스프링 `prototype`스코프와 비슷하다. 하지만 스프링의 기본 설정과 일관되게 하기 위하여 스프링 컨테이너에서 선언된 JSR-330 빈은 `singleton`이 기본 스코프이다. `singleton`이외의 스코프를 사용하려면 스프링의 `@Scope`어노테이션을 사용해야한다. `javax.inject`에 [@Scope](https://download.oracle.com/javaee/6/api/javax/inject/Scope.html)어노테이션이 있지만 자신만의 어노테이션을 생성하는 목적으로 사용되는 어노테이션이다. |
| @Qualifier | @Qualifier / @Named | `javax.inject.Qualifier`는 커스텀 qualifier를 만드는데 사용되는 메타 어노테이션이다. 스프링의 `@Qualifier`와 같은 `String` 값은 `java.inject.Named`를 사용하여 설정할 수 있다. |
 | @Value | - | 동일한 기능 없음 |
 | @Required | - | 동일한 기능 없음 |
 | @Lazy | - | 동일한 기능 없음 |
 | ObjectFactory | Provider | `javax.inject.Provider`는 스프링 `ObjectFactory`의 `get()`메소드의 이름이 짧은 대체자이다. 스프링 `@Autowired`와 함께 사용가능하며 어노테이션이 적용되지 않은 생성자 어규먼트나 세터메소드에도 사용 가능하다. |

<h3 id="beans-java">자바 기반 컨테이너설정</h3>

이 장에서는 스프링 컨테이너를 설정하기 위해 자바 코드에 어노테이션을 사용하는 방법에 대하여 이야기한다. 아래 내용을 포함하고 있다:

* [기본 개념: `@Bean`과 `@Configuration`](#beans-java-basic-concepts)
* [`AnnotationConfigApplicationContext`를 이용하여 스프링 컨테이너 인스턴스화 하기](#beans-java-instantiating-container)
* [`@Bean`어노테이션 이용하기](#beans-java-bean-annotation)
* [`@Configuration`어노테이션 이용하기](#beans-java-configuration-annotation")
* [자바 기반 설정 구성하기](#beans-java-composing-configuration-classes)
* [빈 정의 프로필](#beans-definition-profiles)
* [`PropertySource` 추상화](#beans-property-source-abstaction)
* [`PropertySource` 사용하기](#beans-using-propertysource)
* [표현법의 PlaceHolder Resolution](#beans-placeholder-resolution-in-statements)

<h4 id="beans-java-basic-concepts">기본 개념: @Bean과 @Configuration</h4>

스프링의 새로운 자바 기반 설정에서 가장 중심이 되는 부분은 `@Configuration`어노테이션이 적용된 클래스와 `@Bean`어노테이션이 적용된 메서드이다.     

`@Bean`어노테이션은 스프링 IoC컨테이너에의해 관리되는 오브젝트를 초기화, 설정, 인스턴스화 하는 메소드를 표시한다. 스프링 `<beans>`XML 설정에 익숙한 사람들에게는 `@Bean`이 `<bean/>`요소와 같은 역할을 한다고 이해하면 좋다. 스프링 `@Component`에 `@Bean`어노테이션을 적용한 메서드를 사용할 수 있다. 하지만 `@Configuration`빈에서 사용되는 것이 일반적이다.     

`@Configuration`어노테이션이 적용된 클래스는 빈 정의를 제공하는 역할을 한다. 추가적으로 `@Configuration`클래스는 내부 빈 사이의 의존성을 해당 클래스의 `@Bean`메소드를 호출하여 처리될 수 있도록 한다. 가장 간단한 `Configuration`클래스는 아래와 같다:

```java
@Configuration
public class AppConfig {

    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}
```

위의 `AppConfig`클래스는 아래의 `<beans/> XML과 동일하다:

```xml
<beans>
    <bean id="myService" class="com.acme.services.MyServiceImpl"/>
</beans>
```

-----
#### @Configuraion 내부에서 처리하기 vs "lite" @Bean 사용하기

*`@Bean`메소드가 `@Configuration`이 적용되지 않은 클래스에서 사용되면 "lite"모드로 처리된다. `@Component`나 평범한 클래스에서 사용된 빈 메소드는 "lite"로 여겨진다. 이러한 클래스의 목적은 `@Configuration`클래스와 다르기에 `@Bean`메소드는 약간의 보너스일 뿐이다. 예를 들면, 서비스 컴포넌트가 `@Bean`메소드를 사용하여 컨테이너에 설정가능한 뷰를 노출할 수 있다. 이러한 경우에 `@Bean`메소드는 일반적인 팩토리 메서드 역할을 한다.*

*`@Configuration`에서 사용한 `@Bean`메소드와 달리 lite `@Bean`메소드는 내부 빈 의존성을 정의할 수 없다. 대신 메소드가 선언된 컴포넌트의 내부 상태나 어규먼트에 따라 다르게 동작할 수 있다. 따라서 이러한 `@Bean`메소드는 다른 `@Bean`메소드를 호출하면 안된다. 이러한 메소드는 단순한 팩토리 메서드일 뿐이다. 긍정적인 점은 CGLIB이 진행하는 상속을 진행하기 않아 클래스 구성에 제한이 없다.(`final`을 포함할 수 있다. 등등)*

*`@Configuration`클래스에서 `@Bean`메소드가 정의되는 것은 흔한 일이다. 이렇게 하면 스프링 생명주기의 관리를 받는 메소드 레퍼런스를 사용하게 된다. `@Bean`메소드가 실수로 일반적인 자바 호출에 따라 진행되는 것을 방지하여 추적하기 힘든 버그가 일어날 가능성을 줄여준다.*

-----

`@Bean`과 `@Configuration`어노테이션은 아래에 더욱 자세히 논의될 것이다. 하지만 자바 기반 설정을 하는 다양한 방법을 먼저 볼 것이다.

<h4 id="beans-java-instantiating-container">AnnotationConfigApplicationContext를 이용하여 스프링 컨테이너 인스턴스화 하기</h4>

이 장은 스프링 3.0부터 지원하는 `AnnotationConfigApplicationContext`을 설명한다. 이 다재다능한 `ApplicationContext` 구현체는 `@Configuration`클래스와 `@Component`클래스, JSR-330 메타데이터로 어노테이션이 적용된 클래스를 받아들인다.     

`@Configuration`클래스가 주어지면 `@Configuration`클래스 자체를 빈 정의로 등록하고 선언된 `@Bean`메소드 또한 빈 정의로 등록한다.

`@Autowired`나 `@Inject`같은 의존성 주입 메타데이터가 적용된다는 전제 하에 `@Component`와 JSR-330 클래스가 주어지면 빈 정의로 등록한다.

<h5 id="beans-java-instantiating-container-constructor">간단하게 만들어보기</h5>

`ClassPathXmlApplicationContext`에서 스프링 XML파일을 사용하여 인스턴스화할 때와 마찬가지로 `AnnotationConfigApplicationContext`는 `@Configuration`클래스를 사용하여 인스턴스화 하다. 이 방법으로 XML을 사용하지 않는 스프링 컨테이너를 만들 수 있다. 아래는 그 예시다:

```java
public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
    MyService myService = ctx.getBean(MyService.class);
    myService.doStuff();
}
```

이전에 언급했듯이, `AnnotationConfigApplicationContext`는 `@Configuration`클래스만 이용하는 것이 아니다. `@Component`클래스와 JSR-330 어노테이션이 적용된 클래스를 사용할 수 있다. 아래는 그러한 클래스에 생성자 어규먼트를 전달하는 예시이다:

```java
public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(MyServiceImpl.class, Dependency1.class, Dependency2.class);
    MyService myService = ctx.getBean(MyService.class);
    myService.doStuff();
}
```

위 예제는 `Dependency1`,`Dependency2`,`MyServiceImpl`이 `@Autowired`와 같은 스프링 의존성 주입 어노테이션을 사용한다는 전제하에 작성된 예시이다.

<h5 id="beans-java-instantiating-container-register">register(Class<?>...)을 이용하여 프로그래밍적으로 컨테이너 만들기</h5>

`AnnotationConfigApplicationContext`를 어규먼트 없는 생성자와 `register()`메소드를 사용하여 인스턴스화 할 수 있다. `AnnotationConfigApplicationContext`를 프로그래밍적으로 생성할 때 유용하다. 아래는 그 예시이다:

```java
public static void main(String[] args) {
    AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
    ctx.register(AppConfig.class, OtherConfig.class);
    ctx.register(AdditionalConfig.class);
    ctx.refresh();
    MyService myService = ctx.getBean(MyService.class);
    myService.doStuff();
}
```

<h5 id="beans-java-instantiating-container-scan">scan(String...)을 이용하여 컴포넌트 스캔 활성화하기</h5>

`@Configuration` 클래스에 어노테이션을 설정하여 컴포넌트 스캔을 아래 예시처럼 활성화 할 수 있다:

```java
@Configuration
@ComponentScan(basePackages = "com.acme") // [1] 
public class AppConfig  {
    ...
}
```

1. 이 어노테이션이 컴포넌트 스캔을 활성화 한다.

| |
| --- |
| ***@** `context` 네임스페이스를 이용하여 XML 정의 기반 컴포넌트 스캔 활성화를 사용할 수도 있다.* |

```xml
<beans>
    <context:component-scan base-package="com.acme"/>
</beans>
```

위의 예시에서 `com.acme`패키지의 `@Component`어노테이션이 적용된 클래스가 확인되어 컨테이너에 스프링 빈으로 등록된다. `AnnotationConfigApplicationContext`는 `scan(String...)`메서드를 직접 사용할 수 있도록 한다. 아래는 그 예시이다:

```java
public static void main(String[] args) {
    AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
    ctx.scan("com.acme");
    ctx.refresh();
    MyService myService = ctx.getBean(MyService.class);
}
```

| |
| --- |
| ***!** `@Component`는 `@Configuration`클래스의 [메타 어노테이션](#beans-meta-annotations)으로 적용되었다. 따라서 `@Configuration`클래스들은 컴포넌트 스캔의 대상이다. 위의 예시에서 `AppConfig`클래스가 `com.acme`패키지 혹은 그 하위 패키지에 선언되었으면 `scan()`메서드, `refresh()`메서드 실행중 클래스 내부의 `@Bean`메서드가 실행되어 컨테이너의 빈으로 등록된다.* |

<h5 id="beans-java-instantiating-container-web">AnnotationConfigWebApplicationContext을 이용한 웹 어플리케이션 지원</h5>

`AnnotationConfigApplicationContext`중 `WebApplicationContext`인 것은 `AnnotationConfigWebApplicationContext`이다. 스프링의 `ContextLoaderListener` 서블릿 리스너를 설정하거나 스프링 MVC `DispatcherServlet`을 사용할 때 등등의 경우에 이 구현체를 사용할 수 있다. 아래는 스프링 MVC 웹 어플리케이션의 `web.xml`설정의 일부이다(`contextClass` 컨테스트 파라메터와 초기화 파라메터의 사용을 주의 깊게 보십시오):

```xml
<web-app>
    <!-- ContextLoaderListener가 기본 XmlWebApplicationContext대신
        AnnotationConfigWebApplicationContext를 사용하도록 설정-->
    <context-param>
        <param-name>contextClass</param-name>
        <param-value>
            org.springframework.web.context.support.AnnotationConfigWebApplicationContext
        </param-value>
    </context-param>

    <!-- 설정 위치는 반드시 하나 이상의 콤마(,)혹은 공백( )으로 구분된 @Configuraion
        클래스의 전체 이름이여야 합니다. 패키지의 전체 이름을 사용해서 컴포넌트 스캔헐
        패키지를 설정할 수도 있습니다. -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>com.acme.AppConfig</param-value>
    </context-param>

    <!-- 보통의 경우처럼 ContextLoaderListener를 이용하여 루트 어플리케이션 컨텍스트를 설정한다. -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!-- 보통의 경우처럼 스프링 MVC DispatcherServlet을 설정한다. -->
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- DispatcherServlet이 기본 XmlWebApplicationContext 대신
            AnnotationConfigWebApplicationContext를 사용하도록 설정한다.  -->
        <init-param>
            <param-name>contextClass</param-name>
            <param-value>
                org.springframework.web.context.support.AnnotationConfigWebApplicationContext
            </param-value>
        </init-param>
        <!-- 위에 이야기했던 것과 마찬가지로 설정 위치는 반드시 하나 이상의 콤마(,) 혹은 공백( )으로
        구분된 @Configuraion 클래스의 전체 이름이여야 합니다. -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>com.acme.web.MvcConfig</param-value>
        </init-param>
    </servlet>

    <!-- /app/* 경로의 모든 요청을 dispatcher servlet으로 연결한다. -->
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/app/*</url-pattern>
    </servlet-mapping>
</web-app>
```

<h4 id="beans-java-bean-annotation">@Bean 어노테이션 사용하기</h4>

`@Bean`어노테이션은 메서드에 적용되는 어노테이션이며 XML `<bean/>` 요소와 같은 기능을 한다. 이 어노테이션은 `<bean/>`의 어트리뷰트 중 일부를 사용할 수 있다. 예를 들면: [init-method](#beans-factory-lifecycle-initializingbean), [destory-method](#beans-factory-lifecycle-disposablebean), [autowiring](#beans-factory-autowire) `name`

`@Configuration`어노테이션이나 `@Component`어노테이션이 적용된 클래스 내부에 `@Bean`어노테이션을 사용할 수 있다.

<h5 id="beans-java-declaring-a-bean">빈 정의하기</h5>

빈을 정의하려면 `@Bean`어노테이션을 메서드에 적용하면 된다. 이 메서드를 사용해서 메서드의 반환 값으로 타입이 지정된 빈정의를 `ApplicationContext`에 등록한다. 기본적으로 빈 이름은 메서드의 이름과 동일하다. 아래는 `@Bean`메소드 정의의 예시이다:

```java
@Configuration
public class AppConfig {

    @Bean
    public TransferServiceImpl transferService() {
        return new TransferServiceImpl();
    }
}
```

위 설정은 아래 스프링 XML과 동일한 기능을 한다:

```xml
<beans>
    <bean id="transferService" class="com.acme.TransferServiceImpl"/>
</beans>
```

두 정의 모두 `TranferServiceImpl`타입의 인스턴스를 생성하고 이 객체를 `ApplicationContext`에 `transferService`라는 이름으로 등록한다:

```
transferService -> com.acme.TransferServiceImpl
```

또한 아래 예시처럼 인터페이스나 부모 클래스를 반환하는 메소드에 `@Bean`어노테이션을 적용할 수 있다.

```java
@Configuration
public class AppConfig {

    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl();
    }
}
```

하지만 이 방법은 타입 예측 기능을 특정 인터페이스(`TranferService`) 수준에서만 동작하게한다. 컨테이너는 완전한 타입(`TransferServiceImpl`)을 싱글톤 객체가 생성되는 시점에만 알 수 있다. 지연초기화되지 않는 싱글톤 빈들은 정의된 순서에 따라 인스턴스화된다. 그에 따라 선언되지 않은 타입으로 컴포넌트를 찾는 시점에 따라 다른 타입 매칭 결과가 나타날 수 있다. 
(예를 들면 `@Autowired TrasferServiceImpl`, `transferService`가 인스턴스화 되어야 매칭가능한 의존성)

| |
| --- |
| ***@** 만약 일관적으로 인터페이스 타입으로 빈을 참조한다면 `@Bean` 반환 타입은 디자인 목적과 부합할 것 수도 있다. 하지만 여러 인터페이스를 구현한 컴포넌트나 구현체 타입으로 참조될 수 있는 컴포넌트들은 가장 자세한 반환 타입을 명시하는 것이 더 안전한 방법이다. (최소한 의존성 주입받는 빈에서 참조하는 타입으로 설정하는 것이 좋다)* |

<h5 id="beans-java-dependencies">빈 의존성</h5>

`@Bean`어노테이션이 적용된 메서드는 빈을 생성하는데 필요한 의존성을 나타내는 여러개의 메서드 파라메터를 가질 수 있다. 예를 들어 `TransferService`가 `AccountRepository`를 필요로 한다면 그 의존성을 메서드 파라메터로 아래 예시처럼 나타낼 수 있다:

```java
@Configuration
public class AppConfig {

    @Bean
    public TransferService transferService(AccountRepository accountRepository) {
        return new TransferServiceImpl(accountRepository);
    }
}
```

이 방법은 생성자 기반 의존성 주입과 거의 동일하다고 보면된다. 자세한 내용은 [관련 섹션](#beans-constructor-injection)에서 확인할 수 있다.

<h5 id="beans-java-lifecycle-callbacks">생명주기 콜백 받기</h5>

`@Bean`어노테이션이 적용된 클래스는 일반적인 생명주기 콜백을 사용할 수 있다. JSR-250의 `@PostConstruct`와 `@PreDestory` 어노테이션도 사용할 수 있다. 자세한 내용은 [JSR-250 어노테이션](#beans-postconstruct-and-predestroy-annotations)을 확인하면 된다.

일반적인 스프링 [생명주기](#beans-factory-nature) 콜백도 사용할 수 있다. 빈이 `InitailizingBean`, `DisposableBean`, `LifeCycle` 인터페이스를 구현한다면 각각의 메소드가 컨테이너에 의하여 호출될 것이다.

`*Aware` 인터페이스 (예를 들면, [BeanFactoryAware](#beans-beanfactory), [BeanNameAware](#beans-factory-aware), [MessageSourceAware](#context-functionality-messagesource), [ApplicationContextAware](#beans-factory-aware) 등등)도 사용 가능하다.

`@Bean`어노테이션을 사용하여 초기화 콜백, 종료 메소드를 임의로 설정하는 것도 가능하다. 스프링 XML의 `bean`요소에 `init-method`와 `destory-method`를 설정하는 것과 비슷하다. 다음은 그 예시이다:

```java
public class BeanOne {

    public void init() {
        // initialization logic
    }
}

public class BeanTwo {

    public void cleanup() {
        // destruction logic
    }
}

@Configuration
public class AppConfig {

    @Bean(initMethod = "init")
    public BeanOne beanOne() {
        return new BeanOne();
    }

    @Bean(destroyMethod = "cleanup")
    public BeanTwo beanTwo() {
        return new BeanTwo();
    }
}
```

| |
| --- |
| ***!** 기본적으로 자바로 설정된 빈정의는 퍼블릭 `close` 메소드나 `shutdown`메서드는 자동으로 종료 콜백으로 등록된다. 만약 퍼블릭 `close`나 `shutdown`메소드를 구현하고 있으며 컨테이너에 의하여 호출되기를 원하지 않는다면 `@Bean(destoryMethod="")`를 설정하여 기본으로 설정되는 종료 콜백메소드를 없앨 수 있다.* |
| *JNDI에서 가져온 리소스에 기본적으로 적용되어 생명주기가 어플리케이션 외부에서 관리되기를 원할 수도 있을 것이다. 특히 Java EE 어플리케이션 서버에서 종종 문제를 일으키는 것으로 알려진 `DataSource`에는 이러한 설정을 반드시 해줘야한다.* |
| *아래는 `DataSource`에 자동 콜백 메소드 설정을  막는 예시이다.* |

```java
@Bean(destroyMethod="")
public DataSource dataSource() throws NamingException {
    return (DataSource) jndiTemplate.lookup("MyDS");
}
```

| |
| --- |
| *또한 `@Bean`메소드와 함께 JNDI lookup을 이용하는 경우가 많다. 주로 스프링의 `JndiTemplate`이나 `JndiLocatorDelegate`를 사용하는 방법이나 JNDI의 `JndiObjectFactoryBean`을 제외한 JNDI `InitialContext`를 사용하는 경우이다.(`JndiObjectFactoryBean`은 반환 타입을 실제 사용할 타입이 아닌 `FactoryBean`으로 설정해야하기에 실제 사용할 빈을 전달받는 `@Bean`메소드의 의도와는 다르게 동작하여 사용이 어렵다)* |

위 예시의 `BeanOne`은 아래 예시와 동일하게 생성되는 중 `init()` 메소드를 호출할 것이다:

```java
@Configuration
public class AppConfig {

    @Bean
    public BeanOne beanOne() {
        BeanOne beanOne = new BeanOne();
        beanOne.init();
        return beanOne;
    }

    // ...
}
```

| |
| --- |
| ***@** 자바를 직접사용해서 설정한다면 컨테이너의 생명주기에 의존하지 않고 필요한 작업을 직접 진행할 수 있다.* |

<h5 id="beans-java-specifying-bean-scope">빈 스코프 설정하기</h5>

스프링은 스코프를 설정할 수 있는 `@Scope`어노테이션을 제공한다.

###### `@Scope` 어노테이션 사용하기

`@Bean`어노테이션으로 정의된 빈에 스코프를 설정할 수 있다. [빈 스코프](#beans-factory-scopes)에 나오는 표준 스코프 전부 사용가능하다.

기본 스코프는 `singleton`이다. 하지만 `@Scope` 어노테이션을 사용하여 아래 예시처럼 재설정할 수 있다:

```java
@Configuration
public class MyConfiguration {

    @Bean
    @Scope("prototype")
    public Encryptor encryptor() {
        // ...
    }
}
```

###### `@Scope`와 `scoped-proxy`

스프링은 [스코프 프록시](#beans-factory-scopes-other-injection)를 이용하여 스코프가 설정된 의존성을 사용하는 편리한 방법을 제공한다. XML설정으로 이러한 프록시를 만드는 가장 쉬운 방법은 `<aop:scoped-proxy/>` 요소를 사용하는 것이다. `@Scope`어노테이션을 사용하는 경우, 같은 기능을 하는 `proxyMode`어트리뷰트를 사용할 수 있다. 기본값은 프록시를 적용하지 않는 것(`ScopedProxyMode.NO`)이다. 하지만 `ScopedProxyMode.TARGET_CLASS`나 `ScopedProxyMode.INTERFACES`를 설정할 수 있다.

XML기반 스코프 프록시([스코프 프록시](#beans-factory-scopes-other-injection))에서 자바를 사용한 `@Bean`으로 변경하고자 한다면 아래와 같은 형태일 것이다:


```java
// HTTP 세션 스코프 빈 프록시
@Bean
@SessionScope
public UserPreferences userPreferences() {
    return new UserPreferences();
}

@Bean
public Service userService() {
    UserService service = new SimpleUserService();
    // 프록시가 적용된 userPreferences 빈을 참조한다.
    service.setUserPreferences(userPreferences());
    return service;
}
```

<h5 id="beans-java-customizing-bean-naming">빈 이름 설정하기</h5>

기본적으로 설정 클래스는 `@Bean`메소드의 이름을 빈의 이름으로 사용한다. 하지만 `name`어트리뷰트를 사용하여 아래 예시처럼 재설정할 수 있다:

```java
@Configuration
public class AppConfig {

    @Bean(name = "myThing")
    public Thing thing() {
        return new Thing();
    }
}
```

<h5 id="beans-java-bean-aliasing">빈 별명 설정하기</h5>

[빈 이름 설정하기](#beans-beanname)에서 언급했듯이 종종 한개의 빈에 여러개의 이름이 필요한 경우가 있다. `@Bean`어노테이션의 `name`어트리뷰트는 문자열 배열을 이러한 목적으로 허용한다. 아래는 빈에 별명을 설정하는 예시이다:

```java
@Configuration
public class AppConfig {

    @Bean({"dataSource", "subsystemA-dataSource", "subsystemB-dataSource"})
    public DataSource dataSource() {
        // DataSource 빈을 설정하고 인스턴스화 하여 반환하는 코드...
    }
}
```

<h5 id="beans-java-bean-description">빈 설명</h5>

종종 빈에 직접적인 설명을 제공하는게 도움이 될 때가 있다. 특히 빈이 모니터링을 하기위해 노출되었을 경우(예를 들면 JMX를 통해) 유용하다.

`@Bean`에 설명을 추가하려면 아래 예시처럼 [`@Description`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/annotation/Description.html)을 사용하면 된다:

```java
@Configuration
public class AppConfig {

    @Bean
    @Description("Provides a basic example of a bean")
    public Thing thing() {
        return new Thing();
    }
}
```

<h4 id="beans-java-configuration-annotation">@Configuration 어노테이션 이용하기</h4>

`@Configuration`은 빈이 정의되는 클래스를 가리키는 클래스 레벨 어노테이션이다. `@Configuration`클래스들은 `@Bean` 어노테이션이 적용된 public 메소드를 사용하여 빈을 정의한다. `@Configuration`클래스 내부에서 `@Bean`메소드를 호출하는 것은 내부 빈 사이의 의존성을 나타내기 위해 사용된다. [기본 개념: `@Bean`과 `@Configuration`](#beans-java-basic-concepts)에 일반적인 소개를 확인할 수 있다.

<h5 id="beans-java-injecting-dependencies">내부 빈 의존성 주입하기</h5>

어떠한 빈이 다른 빈에 의존할 때, 빈 메소드에서 다른 빈 메소드를 호출하는 것이 가장 간단하게 의존성을 표현하는 방법이다. 아래는 예시이다:

```java
@Configuration
public class AppConfig {

    @Bean
    public BeanOne beanOne() {
        return new BeanOne(beanTwo());
    }

    @Bean
    public BeanTwo beanTwo() {
        return new BeanTwo();
    }
}
```

위 예시에서 `beanOne`은 생성자 주입을 통하여 `beanTwo`의 참조를 얻는다.

| |
| --- |
| ***!** `@Bean`메소드가 `@Configuration`클래스 내부에 정의 되었을 때만 이러한 내부 빈 사이의 의존성 주입이 동작한다. 일반적인 `@Component`클래스에는 내부 빈 사이의 의존성을 정의할 수 없다.* |

<h5 id="beans-java-method-injection">Lookup 메소드 주입하기</h5>

이미 언급했다시피 [lookup 메소드 주입](#beans-factory-method-injection)은 가끔씩 사용하는 고급기술이다. 이 방법은 싱글톤 빈이 프로토타입 빈에 의존할 때 유용하다. 이러한 설정을 할때 자바를 사용하면 아주 자연스러운 표현을 할 수 있다. 아래는 lookup 메소드 주입을 사용하는 예시이다:

```java
public abstract class CommandManager {
    public Object process(Object commandState) {
        // 적절한 Commond 인터페이스 구현체를 가져온다.
        Command command = createCommand();
        // 커맨드 인터페이스에 상태를 설정한다. (이때, 커맨드는 새로운 객체여야 한다.)
        command.setState(commandState);
        return command.execute();
    }

    // 그렇다면 이 메소드는 어디에서 구현되는가?
    protected abstract Command createCommand();
}
```

자바 설정을 이용하면 `CommandManger`의 자식 클래스를 만들어서 `createCommand()`를 재정의 하여 새로운 (프로토타입) 커맨드 객체를 생성하도록 할 수 있다. 아래는 그 예시이다:

```java
@Bean
@Scope("prototype")
public AsyncCommand asyncCommand() {
    AsyncCommand command = new AsyncCommand();
    // 여기에 필요한 의존성 주입 코드를 작성한다.
    return command;
}

@Bean
public CommandManager commandManager() {
    // createCommand()가 구현된 익명 CommandManager 구현체를 반환한다.
    // 프로토타입 Command 객체를 반환하도록 메소드를 재정의 한다.
    return new CommandManager() {
        protected Command createCommand() {
            return asyncCommand();
        }
    }
}
```

<h5 id="beans-java-further-information-java-config">자바 기반 설정이 내부적으로 작동하는 방법에 대한 추가적인 정보</h5>

`@Bean` 어노테이션이 적용된 메소드가 두번 호출되는 예시를 생각해보자:

```java
@Configuration
public class AppConfig {

    @Bean
    public ClientService clientService1() {
        ClientServiceImpl clientService = new ClientServiceImpl();
        clientService.setClientDao(clientDao());
        return clientService;
    }

    @Bean
    public ClientService clientService2() {
        ClientServiceImpl clientService = new ClientServiceImpl();
        clientService.setClientDao(clientDao());
        return clientService;
    }

    @Bean
    public ClientDao clientDao() {
        return new ClientDaoImpl();
    }
}
```

`clientDao()`는 `clientService1()`과 `clientService2()`에서 각각 한번씩 호출되었다. 메소드는 새로운 `ClientDaoImpl`을 생성하여 반환하는 형태로 구현되었기 때문에 (각 서비스에 1개씩) 2개의 인스턴스가 생성될 것이라 생각할 것이다. 그렇다면 이 방법은 문제의 소지가 있다: 스프링은 기본적으로 `singleton` 스코프로 빈을 생성한다. 여기서 특별한 기술이 적용된다: 시작시점에 모든 `@Configuration`클래스는 `CGLIB`을 이용하여 자식클래스로 변경된다. 이 자식클래스의 메소드에서 컨테이너를 먼저 확인한 후 컨테이너의 빈을 반환한다. 컨테이너에 빈이 없다면 부모 메소드를 호출하여 새 인스턴스를 생성한다.

| |
| --- |
| ***!** 이 동작은 빈의 스코프에 따라 달라진다. 여기서는 싱글톤 빈에 대한 이야기이다.* |
| ***!** 스프링 3.2부터 CGLIB을 클래스패스에 추가할 필요가 없어졌다. CBLIB 클래스는 `org.springframework.cglib`로 다시 패키징되어 스프링 코어 JAR에 포함되었다.* |
| ***@** CGLIB이 시작지점에 기능을 생성하기에 몇가지 제한사항이 있다. 특히 configuration 클래스는 final이 아니여야 한다는 것이다. 스프링 4.3부터 configuration 클래스는 어떠한 형태의 생성자든지 허용된다. `@Autowired`나 기본 생성자의 형태가 아닌 유일한 생성자를 사용하는 방법도 포함된다. CGLIB으로 인한 한계에서 벚어나고 싶다면 `@Configuration`클래스가 아닌 클래스(예를 들면 일반적인 `@Component` 클래스)에 `@Bean`메소드를 정의하는 것을 고려해보자. 이렇게하면 `@Bean`메소드간에 호출하는 것이 특별한 동작을 하지 않아 오직 생성자나 메소드 레벨 의존성 주입을 이용해야한다.* |

<h4 id="beans-java-composing-configuration-classes">자바 기반 설정 구성하기</h4>

스프링 자바 기반 설정 기능은 어노테이션을 사용할 수 있어 설정을 복잡하지 않게 할 수 있다.

<h5 id="beans-java-using-import">@Import 어노테이션 사용하기</h5>

스프링 XML 파일의 `<import/>`처럼 `@Import`어노테이션을 설정을 모듈화 하여 다른 설정 클래스의 `@Bean` 정의를 가져올 수 있도록 한다. 아래는 그 예시이다:

```java
@Configuration
public class ConfigA {

    @Bean
    public A a() {
        return new A();
    }
}

@Configuration
@Import(ConfigA.class)
public class ConfigB {

    @Bean
    public B b() {
        return new B();
    }
}
```

이제, 콘텍스트로드 시점에 `ConfigA.class`와 `ConfigB.class` 모두 명시하지 않고 `ConfigB`만 명시하면 된다. 아래는 그 예시이다:

```java
public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(ConfigB.class);

    // now both beans A and B will be available...
    A a = ctx.getBean(A.class);
    B b = ctx.getBean(B.class);
}
```

이러한 접근법은 컨테이너 생성을 간소화한다. 수 많은 `@Configuration`클래스를 기억할 필요없이 단 한 개의 클래스만 처리하면 된다.


| |
| --- |
| ***!** 스프링 4.2부터 `@Import`는 일반적인 component 클래스의 참조도 사용 가능하여 `AnnotationConfigApplicationCOntext.register` 메소드를 대신 할수있다. 컴포넌트 스캔을 사용하지 않고 몇 개의 설정 클래스만 추가하고자 할 때 유용하다.* |

###### 임포드된 `@Bean` 정의에 의존성 주입하기

위의 예시는 잘 동작하지만 단순하다. 실제 사용 환경에서는 여러 설정 클래스 간에 의존성을 가지게 된다. XML을 사용할 때는 자바 컴파일러가 관여하지 않고 스프링 컨테이너에 동작을 맡기기 때문에 문제가 되지 않는다. `@Configuration`클래스를 사용할 경우에는 자바 컴파일러가 관여하여 제약을 가지게 되고 반드시 올바른 자바 문법을 따라야 한다.

다행이도 해결방법은 간단하다. 이미 [언급했듯이](#beans-java-dependencies) `@Bean`메소드는 여러 파라메터를 가질수 있고 이 파라메터로 의존성을 나타낼 수 있다. `@Configuration` 클래스의 실제 사용예시를 보자:

```java
@Configuration
public class ServiceConfig {

    @Bean
    public TransferService transferService(AccountRepository accountRepository) {
        return new TransferServiceImpl(accountRepository);
    }
}

@Configuration
public class RepositoryConfig {

    @Bean
    public AccountRepository accountRepository(DataSource dataSource) {
        return new JdbcAccountRepository(dataSource);
    }
}

@Configuration
@Import({ServiceConfig.class, RepositoryConfig.class})
public class SystemTestConfig {

    @Bean
    public DataSource dataSource() {
        // 새로운 DataSource를 반환한다.
    }
}

public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(SystemTestConfig.class);
    // 모든 의존성은 Configuration 클래스를 통해 해소된다.
    TransferService transferService = ctx.getBean(TransferService.class);
    transferService.transfer(100.00, "A123", "C456");
}
```

다른 방법을 통해서도 같은 결과를 얻을 수 있다. `@Configuration` 클래스 자체도 컨테이너의 빈이라고 언급했었다: 이 말은 `@Autowired`와 `@Value` 의존성 주입을 다른 빈처럼 사용할 수 있다는 의미이다.

| |
| --- |
| ***!** 의존성 주입은 가능하면 간단하게 하는 것이 좋다. `@Configuration` 클래스는 컨텍스트 초기화의 초기단계에서 처리되어 의도하지 않은 빠른 초기화를 일으킬 수 있다. 가능하면 위의 예시처럼 파라메터 기반 의존성 주입을 사용하는 것이 좋다. 또한 `@Bean`을 이용하여 `BeanPostProcessor`와 `BeanFactoryPostProcessor`를 정의할 때 주의하여야한다. 이러한 경우 `static @Bean`메소드로 선언하여 이들을 정의하고 있는 설정 클래스의 인스턴스화를 진행시키지 않아야한다. 그렇지 않으면 설정 클래스에서 `@Autowired`와 `@Value`가 동작하지 않을 것이다. 왜냐하면 [`AutowiredAnnotationBeanPostProcessor`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/beans/factory/annotation/AutowiredAnnotationBeanPostProcessor.html)보다 먼저 인스턴스화가 될 수 있기 때문이다.* |

아래 예시는 빈을 다른 빈에 자동 연결하는 예시이다:

```java
@Configuration
public class ServiceConfig {

    @Autowired
    private AccountRepository accountRepository;

    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl(accountRepository);
    }
}

@Configuration
public class RepositoryConfig {

    private final DataSource dataSource;

    public RepositoryConfig(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @Bean
    public AccountRepository accountRepository() {
        return new JdbcAccountRepository(dataSource);
    }
}

@Configuration
@Import({ServiceConfig.class, RepositoryConfig.class})
public class SystemTestConfig {

    @Bean
    public DataSource dataSource() {
        // 새로운 DataSource를 반환한다.
    }
}

public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(SystemTestConfig.class);
    // 모든 의존성은 Configuration 클래스를 통해 해소된다.
    TransferService transferService = ctx.getBean(TransferService.class);
    transferService.transfer(100.00, "A123", "C456");
}
```

| |
| --- |
| ***!** `@Configuration`클래스의 생성자 기반 의존성 주입은 스프링 4.3부터 지원한다. 생성자가 오직 한 개인 경우 `@Autowired`를 명시할 필요는 없다.* |

###### 임포트할 빈 이름 전체를 명시하여 탐색 간소화 하기

위 시나리오에서 `@Autowired`는 잘 동작하며 모듈화를 제공하지만 어떠한 빈을 선택할지는 모호하다. 개발자가 `ServiceConfig`를 보며 정의된 `@Autowired AccountRepository`가 어디있는지 알 방법은 무엇이 있을까? 코드에 명시적으로 존재하지 않을 수 있다. [Spring Tools For Eclipse](https://spring.io/tools)에서 빈들의 연결 그래프를 제공하여 연결되었는지 확인할 수 있다. 또한 Java IDE에서 `AccountRepository`가 정의되고 사용되는 모든 곳을 검색하여 `@Bean` 메소드를 확인할 수 있다.

이러한 모호함이 용인되지않는 환경에서 `@Configuration`클래스와 다른 연결되는 클래스를 바로 연결가능하도록 하고 싶다면 아래 예시처럼 설정할 수 있다:

```java
@Configuration
public class ServiceConfig {

    @Autowired
    private RepositoryConfig repositoryConfig;

    @Bean
    public TransferService transferService() {
        // 다른 설정 클래스의 '@Bean' 메소드를 통하여 연결한다.
        return new TransferServiceImpl(repositoryConfig.accountRepository());
    }
}
```

위 예시에서 `AccountRepository`는 명시적으로 정의되었다. 하지만 `ServiceCOnfig`는 이제 `RepositoryConfig`와 긴밀한 결합을 가지게 되었다. 
이것은 트레이드오프이다. 인터페이스나 추상 클래스 기반 `@Configuration`클래스를 사용하여 긴밀한 결합울 줄일 수 있다. 아래 예시를 보자:

```java
@Configuration
public class ServiceConfig {

    @Autowired
    private RepositoryConfig repositoryConfig;

    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl(repositoryConfig.accountRepository());
    }
}

@Configuration
public interface RepositoryConfig {

    @Bean
    AccountRepository accountRepository();
}

@Configuration
public class DefaultRepositoryConfig implements RepositoryConfig {

    @Bean
    public AccountRepository accountRepository() {
        return new JdbcAccountRepository(...);
    }
}

@Configuration
@Import({ServiceConfig.class, DefaultRepositoryConfig.class})  // 구상 설정 클래스를 가져온다. 
public class SystemTestConfig {

    @Bean
    public DataSource dataSource() {
        // DataSource를 반환한다.
    }

}

public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(SystemTestConfig.class);
    TransferService transferService = ctx.getBean(TransferService.class);
    transferService.transfer(100.00, "A123", "C456");
}

```

이제 `ServiceConfig`는 `DefaultRepositoryConfig`와 느슨한 결합을 가진다. IDE를 이용해 여전히 `RepositoryConfig`구현체를 쉽게 찾을 수 있다. 이렇게 하면, `@Configuration` 클래스와 의존 클래스 간에 돌아다니는 것은 인터페이스 기반 코드 간에 돌아다니는 것과 동일한 과정을 거친다.

| |
| --- |
| ***!** 특정한 빈들의 생성 순서에 영향을 미치고 싶다면, 빈들을 `@Lazy` (시작시 생성되지 않고 빈을 처음 사용할 때 생성된다)로 설정하는 것을 고려해봐라. 혹은 `@DependsOn`을 이용하여 다른 빈에 의존성을 가질 수 있다(특정한 다른 빈이 생성되고 난뒤에 생성되는 것을 보장한다)* |

<h5 id="beans-java-conditional">조건에 따라 @Configuration 클래스와 @Bean 메소드 포함시키기</h5>

`@Configuration`클래스나 `@Bean`메소드를 특정 조건에 따라 활성화하거나 비활성화하는 것은 종종 유용하게 사용된다. 자주 사용되는 예시로는 `@Profile` 어노테이션을 사용하여 특정한 스프링 `Environment`([빈 정의 프로파일](#beans-definition-profiles)에 자세한 내용이 있다)가 작동하는 경우에만 활성화하는 것이다. 

`@Profile` 어노테이션은 더 유연하게 사용될 수 있는 [`@Conditional`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/annotation/Conditional.html)어노테이션을 이용하여 구현되어 있다. `@Conditional` 어노테이션이 `@Bean`이 등록되기 전에 사용되는 `org.springframework.context.annotation.Condition` 구현체를 가리키고 있다.

`Condition` 인터페이스의 구현체는 `true`나 `false`를 반환하는 `matches(...)` 메소드를 제공한다. 다음은 `@Profile`에 사용되는 실제 `Condition` 구현체의 예시이다:

```java
@Override
public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
    // @Profile 어노테이션 어트리뷰트를 읽어온다.
    MultiValueMap<String, Object> attrs = metadata.getAllAnnotationAttributes(Profile.class.getName());
    if (attrs != null) {
        for (Object value : attrs.get("value")) {
            if (context.getEnvironment().acceptsProfiles(((String[]) value))) {
                return true;
            }
        }
        return false;
    }
    return true;
}
```

[`@Conditional`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/annotation/Conditional.html) 자바독에 자세한 내용을 볼 수 있다.

<h5 id="beans-java-combining">자바 기반 설정과 XML 기반 설정 조합하기</h5>

스프링의 `@Configuration` 클래스는 스프링 XML 설정을 100% 대체하기 위해서 만들어진 것이 아니다. 스프링 XML 네임스페이스같은 일부 기능은 여전히 컨테이너를 설정하는 이상적인 방법이다. XML이 편리하고 필요한 경우에는 `ClassPathXmlApplicationContext`를 사용하는 "XML-중심"의 방법과 `AnnotationConfigApplicationContext`에 `@ImportResource`로 XML을 추가하는 `Java-중심"의 방법 중 선택할 수 있다.

###### XML-중심의 `@Configuration` 클래스 사용하기

스프링 컨테이너를 XML로 설정하고 `@Configuration`클래스를 각각 추가하는 방법을 선호할 수도 있다. 예를 들면 커다란 스프링 XML 기반의 설정이 존재할 때, `@Configuration`클래스를 필요에 따라 생성하고 XML 파일에 포함시키는 것이다. 이 장 뒤쪽에서 "XML-중심"의 상황에서 `@COnfiguration` 클래스를 사용하는 방법에 대해 이야기 할 것이다.

###### `@Configuration` 클래스를 일반적인 스프링 `<bean/>`으로 선언하기

`@Configuration`클래스는 컨테이너에서 빈 정의라는 것을 기억하여야 한다. 다음의 예시들에서 `AppConfig`라는 `@Configuration`클래스를 생성하고 `system-test-config.xml`에 `<bean/>` 빈 정의로서 포함시킬 것이다. `<context:annotation-config/>`가 설정되면 컨테이너는 `@Configuration` 어노테이션을 찾아서 `AppConfig`에 `@Bean`메소드를 처리할 것이다.

아래 예시는 일반적인 자바 설정의 예시이다:

```java
@Configuration
public class AppConfig {

    @Autowired
    private DataSource dataSource;

    @Bean
    public AccountRepository accountRepository() {
        return new JdbcAccountRepository(dataSource);
    }

    @Bean
    public TransferService transferService() {
        return new TransferService(accountRepository());
    }
}
```

아래의 예시는 `system-test-config.xml` 파일의 일부분이다:

```xml
<beans>
    <!-- @Autowired, @Configuration 같은 어노테이션의 처리를 활성화시킨다. -->
    <context:annotation-config/>
    <context:property-placeholder location="classpath:/com/acme/jdbc.properties"/>

    <bean class="com.acme.AppConfig"/>

    <bean class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
</beans>
```

아래 예시는 `jdbc.properties` 파일의 예시이다:

```
jdbc.url=jdbc:hsqldb:hsql://localhost/xdb
jdbc.username=sa
jdbc.password=
```

```java
public static void main(String[] args) {
    ApplicationContext ctx = new ClassPathXmlApplicationContext("classpath:/com/acme/system-test-config.xml");
    TransferService transferService = ctx.getBean(TransferService.class);
    // ...
}
```

| |
| --- |
| ***!** `system-test-config.xml` 파일에서 `AppConfig` `<bean/>`에 `id` 어트리뷰트를 정의하지 않았다. 필요하지 않다면 생략할수 있기 때문이다. 예를 들면 다른 빈이 참조하지 않고 컨테이너에서 이름으로 빈을 가져올 일이 없을 경우이다. 비슷하게 `DataSource` 빈은 오직 자동연결로만 사용되어서 `id`가 반드시 필요하지 않다.* |

###### <context:component-scan>을 활용하여 `@Configuration` 클래스 선택하기

`@Configuration`이 `@Component` 메타 어노테이션이 적용된 어노테이션이기에 `@Configuration` 어노테이션이 적용된 클래스는 자동적으로 컨포넌트 스캐닝의 대상이 된다. 이전 예시와 같은 상황에서 `system-test-config.xml`을 다시 정의하여 컨포넌트 스캐닝의 장점을 이용할 수 있다. 이 경우에 우리는 `<context:annotation-config/>`를 직접 사용하지 않고 `<context:component-scan/>`을 사용하여 같은 동작을 하도록 할 것이다.

아래의 예시는 변경된 `system-test-config.xml` 파일의 예시이다.

```xml
<beans>
    <!-- AppConfig 빈 정의를 찾아서 등록하도록 한다. -->
    <context:component-scan base-package="com.acme"/>
    <context:property-placeholder location="classpath:/com/acme/jdbc.properties"/>

    <bean class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
</beans>
```

###### `@ImportResource`를 사용하여 XML에서 `@Configuration` 클래스 중심으로 사용하기

`@Configuration`이 최 우선의 설정방법으로 사용되는 어플리케이션에서도 약간의 XML을 사용할 필요가 있을 수 있다. 이러한 경우에는 `@ImportResource`를 사용하여 필요한 만큼의 XML을 정의하여 사용할 수 있다. 이렇게 하면 "Java-중심"의 접근을 가져가면서 XML을 최소한으로 유지할 수 있다. 아래의 예시는 `@ImportResource` 어노테이션을 사용하여 "Java-중심"의 XML 설정을 보여준다:

```java
@Configuration
@ImportResource("classpath:/com/acme/properties-config.xml")
public class AppConfig {

    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("${jdbc.password}")
    private String password;

    @Bean
    public DataSource dataSource() {
        return new DriverManagerDataSource(url, username, password);
    }
}
```

```xml
properties-config.xml
<beans>
    <context:property-placeholder location="classpath:/com/acme/jdbc.properties"/>
</beans>
```

```
jdbc.properties
jdbc.url=jdbc:hsqldb:hsql://localhost/xdb
jdbc.username=sa
jdbc.password=
```

```java
public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
    TransferService transferService = ctx.getBean(TransferService.class);
    // ...
}
```

<h3 id="beans-envirionment">환경 추상화</h3>

[`Environment`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/core/env/Environment.html) 인터페이스는 어플리케이션의 두 중요한 측면인 [프로필](#beans-definition-profiles)과 [프로퍼티](#beans-property-source-abstraction)를 추상화한 것이다.

프로필은 특정 프로필이 활성화 됬을 경우에만 컨테이너에 등록되는 빈 정의들을 모아서 이름지어 놓은 것이다. 빈들은 XML이나 어노테이션을 이용하여 프로필에 등록할 수 있다. `Environment` 객체의 프로필과 관련된 역할은 어떤 프로필이 현재 활성화되었는지, 어떤 프로필이 기본으로 활성화되어야 하는지 결정하는 역할을 한다.

프로퍼팀는 거의 모든 어플리케이션에서 중요한 역할을 하며 다양한 곳으로 부터 설정된다: 프로퍼티 파일, JVM 시스템 프로퍼티, 시스템 환경 변수, JNDI, 서블릿 컨텍스트 파라메터, 각각의 `Properties` 객체, `Map` 객체 등등. `Environment` 객체의 프로퍼티와 관련된 역할은 프로퍼티를 설정하는 편리한 인터페이스를 유저에게 제공하는 역할이다.

<h4 id="beans-definition-profiles">빈 정의 프로필</h4>

빈 정의 프로필은 다양한 환경에서의 다양한 빈들을 핵심 컨테이너에 등록하는 기능을 제공한다. "environment"은 다양한 의미를 가지는 단어지만 제공하는 기능은 실사용할 때 도움이 될것이다. 아래는 실사용의 예시이다:

* 개발시에 메모리기반 데이터를 사용하고 QA, 실사용시 JNDI기반 데이터를 사용한다.
* 성능이 중요한 환경에 배포시에만 어플리케이션의 인프라 모니터링을 실행한다.
* 각 사용자에게 맞춤으로 구현된 빈을 사용자에 따라 등록한다.

첫 번째 예시에서는 `DataSource`를 사용하는 어플리케이션의 예시이다. 테스트 환경에선 아래와 같은 설정을 할 것이다:

```java
@Bean
public DataSource dataSource() {
    return new EmbeddedDatabaseBuilder()
        .setType(EmbeddedDatabaseType.HSQL)
        .addScript("my-schema.sql")
        .addScript("my-test-data.sql")
        .build();
}
```

이 어플리케이션이 QA나 실사용 환경에 배포되어 JNDI에 등록된 데이터를 사용해야 한다고 생각해보자. 그렇다면 `dataSource`빈은 아래의 형태를 가질 것이다:

```java
@Bean(destroyMethod="")
public DataSource dataSource() throws Exception {
    Context ctx = new InitialContext();
    return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
}
```

문제는 환경에 따라서 둘간에 전환이 필요하다는 것이다. 긴 시간동안 스프링 사용자들은 다양한 방법으로 해결해 왔다. 보통은 적합상 설정 파일을 찾아주는 `${placeholder}`를 가진 XML `<import/>` 구문과 환경 변수를 조합하여 해결해 왔다.

이제 환경 특화된 빈 정의를 사용하여 이전의 예시를 일반화한다면, 현재 컨텍스트에 특정 빈 정의만 등록하고 나머지는 등록하지 않을 필요가 있다. 그렇게 처리하기 위해 이제부터 설정파일을 변경해갈 것이다.

<h5 id="beans-definition-profiles-java">@Profile 이용하기</h5>

[`Profile`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/annotation/Profile.html) 어노테이션은 특정 프로필이 활성화 된 경우에만 등록되는 컴포넌트롤 표시하는 어노테이션이다. 아래는 `dataSource` 설정을 새롭게 작성한 예시이다:

```java
@Configuration
@Profile("development")
public class StandaloneDataConfig {

    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.HSQL)
            .addScript("classpath:com/bank/config/sql/schema.sql")
            .addScript("classpath:com/bank/config/sql/test-data.sql")
            .build();
    }
}
```

```java
@Configuration
@Profile("production")
public class JndiDataConfig {

    @Bean(destroyMethod="")
    public DataSource dataSource() throws Exception {
        Context ctx = new InitialContext();
        return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
    }
}
```

| |
| --- |
| ***!** 이전에 언급했듯이 `@Bean` 메소드에서 스프링 `JndiTemplate`/`JndiLocateorDelegate`를 사용하여 JNDI lookup을 프로그래밍적으로 이용할 수 있고 JNDI `InitialContext`를 직접 사용할 수 있다. 하지만 `JndiObjectFactoryBean`은 `FactoryBean` 타입을 반환하기때문에 사용할 수 없다.* |

프로필 문자열은 단순한 프로필 이름(예를 들면, `production`) 혹은 프로필 표현식으로 설정할 수 있다. 프로필 표현식은 더 복잡한 프로필 선택로직을 사용할 수 있도록 해준다(예를 들면, `production & us-east`). 아래는 지원하는 표현식 연산자이다:

* `!` : "not" 연산
* `&` : "and" 연산
* `|` : "or" 연산

| |
| --- |
| ***!** `&`와 `|` 연산자를 괄호 없이 조합할 수 없다. 예를 들면 `production & us-east | eu-central`은 잘못된 표현식이다. `production & (us-east | eu-central)`로 표현되어야 한다.* |

맞춤형 복합 어노테이션을 만드는 [메타 어노테이션](#beans-meta-annotations)으로 `@Profile`을 사용할 수있다. 아래의 예시는 `@Profile("production")`을 대체하는 `@Production`어노테이션을 정의하는 예시이다:

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Profile("production")
public @interface Production {
}
```

| |
| --- |
| ***@** `@Configuration`클래스에 `@Profile`어노테이션이 적용되면 해당 프로필이 활성화되지 않을 경우, `@Bean`메소드와 `@Import` 어노테이션은 전부 무시된다. `@Component`나 `@Configuration`클래스에 `@Profile({"p1", "p2"})` 어노테이션이 적용되면 'p1'이나 'p2'프로필이 활성화되어야 등록된다. NOT 연산자 (`!`)로 표시되면 프로필이 활성화되지 않은 경우에 등록된다. 예를 들면 `@Profile({"p1", "!p2"})`가 적용되면 'p1'이 활성화되거나 'p2'가 활성화되지 않은 경우에 등록된다.* |


`@Profile`은 메소드 레벨에 적용되어 설정 클래스의 특정빈에만 동작할 수 있다. 아래는 그 예시이다:

```java
@Configuration
public class AppConfig {

    @Bean("dataSource")
    @Profile("development")     // [1]
    public DataSource standaloneDataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.HSQL)
            .addScript("classpath:com/bank/config/sql/schema.sql")
            .addScript("classpath:com/bank/config/sql/test-data.sql")
            .build();
    }

    @Bean("dataSource")
    @Profile("production")      // [2]
    public DataSource jndiDataSource() throws Exception {
        Context ctx = new InitialContext();
        return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
    }
}
```

1. `standaloneDataSource` 메소드는 `development`프로필에서만 등록된다.
2. `jndiDataSource` 메소드는 `production`프로필에서만 등록된다.

| |
| --- |
| ***@** `@Profile`이 적용된 `@Bean`메소드에서 특별한 경우가 일어날 수 있다. 같은 이름으로 오버로드된 `@Bean`메소드에서 `@Profile`은 같은 조건으로 설정되어야한다. 만약 조건이 다르다면, 오버로드된 여러 메소드 중에서 첫번째 조건만 확인한다. 따라서, `@Profile`은 오버로드된 메소드 중에서 한개를 선택하는 용도로 사용할 수 없다.* |
| *프로필에 따라 같은 이름의 빈을 선택하도록 하고 싶다면 각각의 메소드에 서로 다른 이름을 부여하고 `@Bean`의 name 어트리뷰트에 같은 이름을 부여하면 된다. 특히 모든 어규먼트 타입이 같은 경우에는 이러한 방법이 올바른 자바 문법을 사용하는 유일한 방법이다.* |


<h5 id="beans-definition-profiles-xml">XML 빈 정의 프로필</h5>

XML에는 `<beans>`요소의 `profile` 어트리뷰트를 사용해서 같은 동작을 할 수 있다. 이전의 예시가 아래와 같이 XML로 다시 작성되었다:

```xml
<beans profile="development"
    xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:jdbc="http://www.springframework.org/schema/jdbc"
    xsi:schemaLocation="...">

    <jdbc:embedded-database id="dataSource">
        <jdbc:script location="classpath:com/bank/config/sql/schema.sql"/>
        <jdbc:script location="classpath:com/bank/config/sql/test-data.sql"/>
    </jdbc:embedded-database>
</beans>
```

```xml
<beans profile="production"
    xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:jee="http://www.springframework.org/schema/jee"
    xsi:schemaLocation="...">

    <jee:jndi-lookup id="dataSource" jndi-name="java:comp/env/jdbc/datasource"/>
</beans>
```

위 예시처럼 분리하지 않고 `<beans>`안에 묶을 수 있다. 아래는 그 예시이다:

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:jdbc="http://www.springframework.org/schema/jdbc"
    xmlns:jee="http://www.springframework.org/schema/jee"
    xsi:schemaLocation="...">

    <!-- 다른 빈 정의들 -->

    <beans profile="development">
        <jdbc:embedded-database id="dataSource">
            <jdbc:script location="classpath:com/bank/config/sql/schema.sql"/>
            <jdbc:script location="classpath:com/bank/config/sql/test-data.sql"/>
        </jdbc:embedded-database>
    </beans>

    <beans profile="production">
        <jee:jndi-lookup id="dataSource" jndi-name="java:comp/env/jdbc/datasource"/>
    </beans>
</beans>
```

| |
| --- |
| ***@** 위에서 언급한 프로필 표현식을 XML에서는 사용할 수 없다. 하지만 `!`연산자를 사용할 수 있다. 또한 논리 "and"연산을 프로필내에 프로필을 설정해서 표현할 수 있다. 아래는 그 예시이다:* |

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:jdbc="http://www.springframework.org/schema/jdbc"
    xmlns:jee="http://www.springframework.org/schema/jee"
    xsi:schemaLocation="...">

    <!-- 다른 빈 정의 -->

    <beans profile="production">
        <beans profile="us-east">
            <jee:jndi-lookup id="dataSource" jndi-name="java:comp/env/jdbc/datasource"/>
        </beans>
    </beans>
</beans>
```
| |
| --- |
| *위 예시에서 `dataSource`빈은 `production`과 `us-east`프로필이 모두 활성화된 경우에만 노출된다.* |

<h5 id="beans-definition-profiles-enable">프로필 활성화하기</h5>

지금까지 우리는 설정파일을 설정해왔다. 이제는 어떤 프로필을 활성화할지 스프링에 설정할 차례이다. 우리의 예제 어플리케이션을 동작시킨다면 `NoSuchBeanDefinitionException` 예외가 발생할 것이다. 왜냐하면 `dataSource`빈을 찾을 수 없기 때문이다.

프로필은 여러가지 방법으로 활성화할 수 있다. 가장 직접적인 방법은 `ApplicationContext`를 통하여 획득할 수 있는 `Environment` API를 직접 사용하는 것이다. 아래는 그 예시이다.

```java
AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
ctx.getEnvironment().setActiveProfiles("development");
ctx.register(SomeConfig.class, StandaloneDataConfig.class, JndiDataConfig.class);
ctx.refresh();
```

또한 시스템 환경 변수, JVM 시스템 프로퍼티, `web.xml`의 서블릿 콘텍스트 파라메터, 심지어 JNDI([`PropertySource` 추상화](#beans-property-source-abstraction))를 통하여 설정할 수 있는 `spring.profiles.active` 프로퍼티를 통하여 설정할 수 있다. 통합테스트에서는 spring-test 모듈([환경 프로필로 컨텍스트 설정하기](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/testing.html#testcontext-ctx-management-env-profiles))의 `@ActiveProfiles`어노테이션을 통해 설정할 수 있다.

스프링 프로필은 양자택일 해야하는 것이 아니다. 여러개의 프로필을 동시에 활성화 할 수 있다. `String...` 어규먼트를 받는 `setActiveProfiles()` 메소드에 여러개의 프로필 이름으로 호출하여 설정할 수 있다. 아래는 그 예시이다:

```java
ctx.getEnvironment().setActiveProfiles("profile1", "profile2");
```

선언적으로 사용할 때는 `spring.profiles.active`에 반점으로 구분되는 프로필 이름 리스트를 입력하면 된다. 아래는 그 예시이다:

```
-Dspring.profiles.active="profile1,profile2"
```

<h5 id="beans-definition-profiles-default">기본 프로필</h5>

기본 프로필은 기본적으로 활성화되는 프로필을 의미한다. 아래의 예시를 보자:

```java
@Configuration
@Profile("default")
public class DefaultDataConfig {

    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.HSQL)
            .addScript("classpath:com/bank/config/sql/schema.sql")
            .build();
    }
}
```

아무런 프로필이 활성화 되지 않았다면 `dataSource` 빈이 생성된다. 이 방법으로 여러 빈들의 기본 값을 설정할 수 있다. 특정 프로필이 활성화 된다면 기본 프로필은 동작하지 않는다.

`Environment`의 `setDefaultProfiles()`를 이용하거나 `spring.profiles.default`프로퍼티를 이용하여 기본 프로필의 이름을 변경할 수 있다.


<h4 id="beans-property-source-abstaction">PropertySource 추상화</h4>

스프링 `Environment` 추상화는 프로퍼티 소스 계층에서 탐색하는 기능을 제공한다. 아래의 예를 보자:

```java
ApplicationContext ctx = new GenericApplicationContext();
Environment env = ctx.getEnvironment();
boolean containsMyProperty = env.containsProperty("my-property");
System.out.println("Does my environment contain the 'my-property' property? " + containsMyProperty);
```

위 코드에서 현재 환경에 `my-property`가 설정되었는지 확인하는 하이 레벨의 방법을 볼 수 있다. 이 질문에 대답하기 위해 `Environment`는 [`PropertySource`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/core/env/PropertySource.html)객체들을 탐색을 한다. `PropertySource`는 key-value 페어들을 추상화한 것이며 스프링 [`StandardEnvironment`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/core/env/StandardEnvironment.html)는 두개의 PropertySource 객체로 구성된다. - 하나는 JVM 시스템 프로퍼티(`System.getProperties()`)를 나타내며 나머지 하나는 시스템 환경 변수(`System.getenv()`)를 나타낸다.

| |
| --- |
| ***!** 독립 어플리케이션에 사용되는 `StandardEnvironment`에 이러한 기본 프로퍼티 소스가 존재한다. [`StandardServletEnvironment`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/web/context/support/StandardServletEnvironment.html)는 서블릿 설정과 서플릿 컨텍스트 파라메터를 가지고 있는 프로퍼티 소스를 추가로 가진다. 또한 [`JndiPropertySource`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/jndi/JndiPropertySource.html)를 원한다면 추가할 수 있다. 자세한 내용은 자바독에서 확인 가능하다.* |

즉, `StandardEnvironment`를 사용할 때, `env.containsProperty("my-property")`를 호출한다면 `my-property`가 시스템 프로퍼티나 환경변수일 경우에 true를 반환할 것이다.

| |
| --- |
| ***@** 프로퍼티 검색은 계층적으로 진행된다. 기본적으로 시스템 프로퍼티가 환경변수보다 우선이다. 그래서 만약 `my-property`가 둘 모두에 설정되어 있다면 `env.getProperty("my-property")`를 호출하면 시스템 프로퍼티가 선택되어 반환될 것이다. 프로퍼티 값이 여러곳에 설정되면 합쳐지지 않고 한개의 값만 설정된다는 것에 주의하여야한다.* |
| *`StandardServletEnvironment`에서 모든 계층 순서는 아래와 같다. 우선되는 계층이 먼저 작성되었다:* |

1. ServletConfig 파라메터 (단 사용 가능한 경우에만 -- 예를 들면, `DispatcherServlet` 컨텍스트에서 사용되는 경우)
2. ServletContext 파라메터 (web.xml 컨텍스트 파라메터)
3. JNDI 환경 변수 (`java:comp/env/`)
4. JVM 시스템 프로퍼티 (`-D` 커맨드 라인 어규먼트)
5. JVM 시스템 환경 (운영 체제 환경 변수)

이 모든 메커니즘이 설정 가능하다. 만약 커스텀 프로퍼티 소스가 검색되도록 추가하고 싶다고 가정하자. 그렇다면 자신만의 `PropertySource`를 구현하고 현재 `Environment`의 `PropertySources`로 추가하면 된다. 아래는 그 예시이다:

```java
ConfigurableApplicationContext ctx = new GenericApplicationContext();
MutablePropertySources sources = ctx.getEnvironment().getPropertySources();
sources.addFirst(new MyPropertySource());
```

위 코드에서 `MyPropertySource`가 가장 먼저 검색된다. 이 프로퍼티소스가 `my-property`를 가지고 있다면 다른 어떤 `PropertySource`의 `my-property`보다 우선적으로 이것이 선택되어 반환될 것이다. [`MutablePropertySource`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/core/env/MutablePropertySources.html) API는 프로퍼티 소스를 설정하는 다양한 기능의 메소드를 제공한다.

<h4 id="beans-using-propertysource">@PropertySource 사용하기</h4>

`@PropertySource` 어노테이션은 스프링 `Environment`에 `PropertySource`를 추가하는 편리한 방법이다.

`app.properties`라는 파일에 `testbean.name=myTestBean`이라는 key-value 페어를 가지고 있을 때, 아래의 `@Configuration`은 `@PropertySource`를 사용하여 `testBean.getName()`이 `myTestBean`을 반환하도록 설정한다:

```java
@Configuration
@PropertySource("classpath:/com/myco/app.properties")
public class AppConfig {

    @Autowired
    Environment env;

    @Bean
    public TestBean testBean() {
        TestBean testBean = new TestBean();
        testBean.setName(env.getProperty("testbean.name"));
        return testBean;
    }
}
```

`${...}` 플레이스 홀더가 `@PropertySource`의 리소스 위치에 사용되면 이미 환경에 등록된 프로퍼티로부터 값을 가져와 처리한다. 아래는 그 예시이다:


```java
@Configuration
@PropertySource("classpath:/com/${my.placeholder:default/path}/app.properties")
public class AppConfig {

    @Autowired
    Environment env;

    @Bean
    public TestBean testBean() {
        TestBean testBean = new TestBean();
        testBean.setName(env.getProperty("testbean.name"));
        return testBean;
    }
}
```

`my.placeholder`가 이미 등록된 프로퍼티 소스(예를 들면, 시스템 프로퍼티나 환경 변수)에 존재한다면 해당 값으로 처리될 것이다. 그렇지 않다면 `default/path`가 기본값으로 사용될 것이다. 기본값이 설정되지 않고 프로퍼티도 없다면 `IllegalArgumentException`이 발생할 것이다.


| |
| --- |
| ***!** `@PropertySource` 어노테이션은 자바 8 컨벤션에 따라 반복가능하다. 하지만 그러한 `@PropertySource`어노테이션은 모두 같은 수준에 사용되어야 한다. 모두 설정 클래스 자체에 설정되거나 모두 커스텀 어노테이션의 메타어노테이션으로 설정되어야 한다. 두가지를 섞는 것은 추천하지 않는다. 왜냐하면 메타 어노테이션에 설정된 값은 설정 클래스에 직접 설정된 값으로 대체될 것이기 때문이다.* |

<h4 id="beans-placeholder-resolution-in-statements">표현법의 PlaceHolder Resolution</h4>

플레이스 홀더에 처리되는 값을 보통 JVM 시스템 프로퍼티나 환경 변수로 처리되어 왔다. 하지만 더 이상 그렇게 사용하지 않는다. `Envionment` 추상화는 컨테이너에 통합되어 있기 때문에 컨테이너를 통하여 쉽게 처리할 수 있기 때문이다. 이 말은 개발자가 원하는 방식으로 처리하도록 설정할 수 있다는 것이다. 시스템 프로퍼티나 환경 변수의 우선순위를 변경할 수도 있고 아예 제거할 수도 있다. 또한 자신만의 프로퍼티 소스를 적절히 추가하여 사용할 수 있다.

즉, 아래의 표현에서 `customer`프로퍼티가 어디에 정의되어 있든 `Environment`에서 접근할 수 있다면 잘 동작할 것이다:

```xml
<beans>
    <import resource="com/bank/service/${customer}-config.xml"/>
</beans>
```

<h3 id="context-load-time-weaver">LoadTimeWeaver 등록하기</h3>

스프링은 `LoadTimeWeaver`를 사용하여 JVM에 로드되는 시점에 클래스를 동적으로 변경한다.

로드 타임 위빙을 홠겅화하려면 `@Configuration`클래스에 `@EnableLoadTimeWeaving`을 추가하면 된다. 아래는 그 예시이다:

```java
@Configuration
@EnableLoadTimeWeaving
public class AppConfig {
}
```

XML 설정에서는 `context:load-time-weaver` 요소를 사용하여 같은 동작을 할 수 있다:

```xml
<beans>
    <context:load-time-weaver/>
</beans>
```

`ApplicationContext`에 설정되었을 경우, `ApplicationContext`에 등록된 `LoadTimeWeaverAware` 구현체 빈이 있으면 로드 타임 위버 빈 참조를 획득할 수 있다. JPA 클래스 변환에 사용하기 때문에 이 방법은 [스프링 JPA](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/data-access.html#orm-jpa)에서 특히 유용하다. ['LocalContainerEntityManagerFactoryBean'](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/orm/jpa/LocalContainerEntityManagerFactoryBean.html) 자바독에 자세한 내용이 있다. AspectJ 로드 타임 위빙에 대해서는 ['스프링 프레임워크에서 AspectJ를 활용하여 로드타임위빙하기](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/core.html#aop-aj-ltw)를 확인해보자

<h3 id="context-introduction">어플리케이션 콘텍스트의 추가적인 사용기능</h3>

[챕터를 시작할 때](#beans) 언급하였듯이 `org.springframework.beans.factory` 패키지는 빈을 관리하는 기본적인 기능을 제공한다. `org.springframework.context`는 `Beanfactory`인터페이스를 확장한 [`ApplicationContext`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/ApplicationContext.html) 인터페이스를 제공하여 어플리케이션 프레임워크 기반으로 더 많은 추가 기능을 제공한다. 많은 사람들은 `ApplicationContext`를 선언적인 방식으로 사용한다. 심지어 프로그래밍적으로 생성하지도 않고 기반클래스에 의존한다. 예를 들면 JAVA EE에서 `ContextLoader`가 어플리케이션 시작과정에 자동으로 `ApplicationContext`를 생성한다.

`BeanFactory`를 더욱더 프레임워크 기반 방식으로 사용하기 위해서 context 패키지는 아래의 기능을 제공한다:

* `MessageSource` 인터페이스를 사용하여 i18n 스타일의 메세지 사용하기
* `ResourceLoader` 인터페이스를 사용하여 URL이나 파일같은 리소스 접근하기
* `ApplicationEventPublisher`를 사용하여 이벤트를 발행하고 `ApplicationListener`인터페이스를 구현한 빈에 이벤트 전달하기
* `HierarchicalBeanFactory`인터페이스를 사용하여 특정 단계에 집중한 컨텍스트를 여러개 로드하기

<h4 id="context-functionality-messagesource">MessageSource를 이용한 국제화</h4>

`ApplicationContext` 인터페이스는 `MessageSource` 인터페이스를 확장하고 있어 국제화("i18n") 기능을 제공한다. 스프링은 `HierarchicalMessageSource` 인터페이스를 제공하여 우선순위를 두어 메세지를 설정할 수 있다. 이러한 인터페이스들을 이용하여 스프링은 메세지를 처리한다. 이 인터페이스에는 아래 메소드들이 정의되어 있다:

* `String getMessage(String code, Object[] args, String default, Locale loc)`: `MessageSource`로부터 메세지를 획득하는 기본적인 메소드. 설정한 지역에 메세지가 없으면 기본 메세지가 사용된다. 전달된 어규먼트들은 표준 라이브러리에서 제공되는 `MessageFormat`을 이용하여 대체값으로 사용된다.
* `String getMessage(String code, Object[] args, Locale loc)`: 위에 언급한 메소드가 한가지만 제외하고 동일하다: 기본 메세지가 없다. 메세지가 발견되지 않으면 `NoSuchMessageException`이 발생한다.
* `String getMessage(MessageSourceResolvable resolvable, Locale locale)`: 위에 언급한 메소드들에서 사용되는 모든 프로퍼티는 `MessageSourceResolvable`이라는 클래스로 포장되어 이 메소드로 사용할 수 있다.

`ApplicationContext`가 로드되면 컨텍스트에서 `MessageSource`빈을 찾는다. 반드시 `messageSource`라는 이름을 가지고 있어야 한다. 그런 빈이 발견되면 위에 메소드들은 전부 메세지 소스에 위임하여 동작한다. 만약 빈이 발견되지 않으면 `ApplicationContext`는 부모 컨텍스트에서 그 이름을 가진 빈을 찾는다. 찾는 다면 그 빈을 `MessageSource`로 사용한다. 만약 `ApplicationContext`가 메세지 소스를 전혀 찾지 못한다면 비어있는 `DelegateMessageSource`가 생성되어 위에 메소드들을 위임받을 것이다.

스프링은 두가지 `MessageSource` 구현체를 제공한다. `ResourceBundleMessageSource`와 `StaticMessageSource`이다. 두개 모두 `HierarchicalMessageSource`를 구현하여 중첩된 메세지처리를 지원한다. `StaticMessageSource`는 자주 사용되지 않지만 프로그래밍 적으로 메세지 소스를 추가하는 방법을 제공한다. 아래는 `ResourceBundleMessageSource`의 예시이다:

```xml
<beans>
    <bean id="messageSource"
            class="org.springframework.context.support.ResourceBundleMessageSource">
        <property name="basenames">
            <list>
                <value>format</value>
                <value>exceptions</value>
                <value>windows</value>
            </list>
        </property>
    </bean>
</beans>
```

위 예제에서는 `format`, `exception`, `windows`라는 이름의 세가지 리소스 번들이 클래스패스에 존재한다고 가정하였다. 메세지 처리가 필요한 모든 요청은 JDK-표준 메세지 처리 방법인 `ResourceBundle` 객체를 통하여 처리될 것이다. 예시로 보여주기 위해 위에 언급한 세 리소스 번들 중 두개의 모습이 아래와 같다고 하자:

```
    # in format.properties
    message=Alligators rock!
```

```
    # in exceptions.properties
    argument.required=The {0} argument is required.
```

다음 예시는 프로그래밍 적으로 `MessageSource`를 실행하는 방법을 보여준다. 모든 `ApplicationContext` 구현체는 `MessageSource`의 구현체이기도 하기 때문에 `MessageSource` 인터페이스르 형변환하여 사용할 수 있다.

```java
public static void main(String[] args) {
    MessageSource resources = new ClassPathXmlApplicationContext("beans.xml");
    String message = resources.getMessage("message", null, "Default", Locale.ENGLISH);
    System.out.println(message);
}
```

위 프로그램의 실행 결과는 아래와 같을 것이다:

```
Alligators rock!
```

요약하자면 클래스패스 루트에 존재하는 `beans.xml`이라는 파일에 `MessageSource`가 정의된다. `MessageSource` 빈 정의는 여러 리소스 번들을 `basenames` 프로퍼티를 통하여 참조한다. `basenames`프로퍼티로 전달된 세가지 파일은 클래스패스의 루트에 존재하며 각각 `format.properties`, `exception.properties`, `windows.properties`라는 이름을 가진다.

다음 예시는 메시지 처리에 어규먼트가 전달되는 예시이다. 세개의 어규먼트는 `String` 객체로 변경되어 메세지 처리시 플레이스홀더에 추가된다.

```xml
<beans>

    <!-- 이 메세지 소스는 웹 어플리케이션에서 사용될 것이다. -->
    <bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">
        <property name="basename" value="exceptions"/>
    </bean>

    <!-- POJO에 위 메세지소스를 주입한다. -->
    <bean id="example" class="com.something.Example">
        <property name="messages" ref="messageSource"/>
    </bean>

</beans>
```

```Java
public class Example {

    private MessageSource messages;

    public void setMessages(MessageSource messages) {
        this.messages = messages;
    }

    public void execute() {
        String message = this.messages.getMessage("argument.required",
            new Object [] {"userDao"}, "Required", Locale.ENGLISH);
        System.out.println(message);
    }
}
```

`excute()`메소드를 실행한 결과는 아래와 같다:

```
The userDao argument is required.
```

국제화("i18n")과 관련하여 스프링의 다양한 `MessageSource` 구현체들은 모두 JDK 표준 `ResourceBundle`의 지역 처리 방법과 처리 실패 규칙을 따른다. 즉, 위에 언급한 `messageSource`예시들로 이야기하자면 영국(`en-GB`)지역으로 메세지를 처리하고자 한다면 `format_en_GB.properties`, `exceptions_en_GB.properties`, `windows_en_GB.properties`라는 이름으로 파일을 만들어야 한다.

일반적으로 지역 처리는 어플리케이션의 환경에 따라 처리된다. 아래의 예시에서 지역을 수동으로 명시하여 처리하는 예시이다:

```
# in exceptions_en_GB.properties
argument.required=Ebagum lad, the ''{0}'' argument is required, I say, required.
```

```java
public static void main(final String[] args) {
    MessageSource resources = new ClassPathXmlApplicationContext("beans.xml");
    String message = resources.getMessage("argument.required",
        new Object [] {"userDao"}, "Required", Locale.UK);
    System.out.println(message);
}
```

위 프로그램을 실행한 결과는 아래와 같다:

```
Ebagum lad, the 'userDao' argument is required, I say, required.
```

정의된 `MessageSource`의 참조를 획득하기 위해 `MessageSourceAware` 인터페이스를 사용할 수 있다. `ApplicationContext`에 정의되어 있고 `MessageSourceAware`를 구현하는 빈들이 생성될 때, 어플리케이션 컨텍스트의 `MessageSource`가 주입될 것이다.

| |
| --- |
| ***!** `ResourceBundleMessageSource`의 대체제로서 스프링은 `ReloadableResourceBundleMessageSource` 클래스를 제공한다. 이 클래스는 JDK 표준 `ResourceBundleMessageSource`구현체와 같은 번들 파일을 지원하면서 더 다양하게 사용할 수 있다. 특히 (클래스 패스 뿐만아니라) 스프링에서 지원하는 모든 리소스에서 파일을 읽어올 수 있다. 또한 번들 파일을 다시 로드할 수 있다(효율적인 캐싱이 적용된다). [`ReloadableResourceBundleMessageSource`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/support/ReloadableResourceBundleMessageSource.html) 자바독에 자세한 내용이 있다.*|

<h4 id="context-functionality-events">표준, 커스텀 이벤트</h4>

`ApplicationContext`에서 이벤트 처리는 `ApplicationEvent` 클래스와 `ApplicationListener` 인터페이스를 사용하여 처리된다. `ApplicationListener`을 구현한 빈은 컨텍스트에 배포되어 `ApplicationContext`에 `ApplicationEvent`가 발행될 때마다 전달될 것이다. 이 방법은 표준 관찰자 디자인 패턴이다.

| |
| --- |
| ***@** 스프링 4.2부터 이벤트 기반 시스템은 매우 발전되었고 [어노테이션 기반 모델](#context-functionality-events-annotation)을 제공하여 임의의 이벤트를 발생할 수 있다(즉 `ApplicationEvent`를 상속하지 않은 객체도 가능하다). 이러한 객체가 발행되면 이벤트로 감싸서 처리될 것이다.*|

아래 표는 스프링이 제공하는 표준 이벤트를 설명한다:

#### 표 7. 기본 이벤트

| 이벤트 |  설명 |
| ----- | ----- |
| `ContextRefreshedEvent` | `ApplicationContext`가 초기화되거나 새로고침될 때(예를 들면 `ConfigurableApplicationContext`인터페이스의 `refresh()` 메소드를 사용하는 경우) 발행된다. 여기서 초기화된다는 의미는 모든 빈들이 로드되고 후처리 빈들이 발견되어 활성화되고 싱글톤 빈들은 모두 인스턴스화 되고 `ApplicationContext`가 사용가능한 상태가 되었을 때를 말한다. 컨텍스트가 종료되지 않는 이상 여러번 새로 고침될 수 있다. 특히 `ApplicationContext`가 "hot" 새로고침(역주. hot refresh는 컨텍스트가 종료되지 않고 새로 고침되는 것을 의미한다)을 지원하는 경우이다. 예를 들면 `XmlWebApplicationContext`는 hot 새로 고침을 지원한지만 `GenericApplicationContext`는 그렇지 않다. |
| `ContextStartedEvent` | `ConfigurableApplicationContext`의 `start()`메소드를 사용하여 `ApplicationContext`가 시작할 때 발생된다. 여기서 시작한다는 의미는 모든 생명주기를 가진 빈이 시작 신호를 받는다는 의미이다. 일반적으로 이 신호는 빈들이 종료된후 명시적으로 재시작하기위해 사용된다. 또한 자동시작되지 않은 컴포넌트들을 시작할 때에도 사용된다(예를 들면 컨텍스트 초기화시기에 시작되지 않은 컴포넌트). |
| `ContextStoppedEvent` | `ConfigurableApplicationContext` 인터페이스의 `stop()` 메소드를 사용하여 `ApplicationContext`가 정지될 때 발행된다. 여기서 정지된다는 의미는 모든 생명주기 빈이 정지 신호를 받는다는 의미이다. 정지된 컨텍스트는 `start()` 호출을 통하여 다시 시작할 수 있다. |
|`ContextClosedEvent`| JVM 종료 훅이나 `ConfigurableApplicationContext` 인터페이스의 `close()`메소드를 호출하여 `ApplicationContext`가 종료될시 발행된다. 여기서 종료된다는 의미는 모든 싱글톤 빈이 종료되었다는 의미이다. 컨텍스트가 종료되면, 컨텍스트는 완전히 멈추어 다시 시작하거나 새로고침될 수 없다. |
| `RequestHandledEvent` | 웹에 국한된 이벤트로 HTTP 요청이 처리되었다는 내용을 모든 빈에 전달하는 이벤트이다. 요청이 처리되면 발행된다. 스프링 `DispatcherServlet`을 사용하는 웹 어플리케이션에서만 사용가능한 이벤트이다. |
| `ServletRequestHandledEvent` | `RequestHandledEvent`의 자식 클래스로 서블릿에 국한된 컨텍스트 정보를 가지고 있다. |

자신만의 이벤트를 생성하여 발행하는 것도 가능하다. 아래는 스프링의 `ApplicationEvent` 기반의 클래스를 확장하는 간단한 클래스의 예시이다:

```java
public class BlackListEvent extends ApplicationEvent {

    private final String address;
    private final String content;

    public BlackListEvent(Object source, String address, String content) {
        super(source);
        this.address = address;
        this.content = content;
    }

    // 기타 다른 메소드들...
}
```

`ApplicationEvent`를 발행하기 위해서는 `ApplicationEventPublisher`의 `publishEvent()` 메소드를 호출하면 된다. 일반적으로 `ApplicationEventPublisherAware`인터페이스를 구현하여 스프링 빈으로 등록하여 처리할 수 있다. 아래는 그러한 예시이다:

```java
public class EmailService implements ApplicationEventPublisherAware {

    private List<String> blackList;
    private ApplicationEventPublisher publisher;

    public void setBlackList(List<String> blackList) {
        this.blackList = blackList;
    }

    public void setApplicationEventPublisher(ApplicationEventPublisher publisher) {
        this.publisher = publisher;
    }

    public void sendEmail(String address, String content) {
        if (blackList.contains(address)) {
            publisher.publishEvent(new BlackListEvent(this, address, content));
            return;
        }
        // 메일을 전송하는 코드...
    }
}
```

컨테이너를 설정하는 시기에 컨테이너는 `EmailService`가 `ApplicationEventPublisherAware`를 구현하고 있다는 것을 발견하고 `setApplicationEventPublisher()`를 호출한다. 메소드에 전달하는 파라메터는 스프링 컨테이너가 자체적으로 가지고 있다. `ApplicationEventPublisher`인터페이스를 이용하여 어플리케이션 컨텍스트와 상호작용하고 있는 것이다.

자신만의 `ApplicationEvent`를 받기 위해서는 `ApplicationListener`를 구현한 클래스를 만들고 스프링 빈으로 등록해야 한다. 아래는 그 예시이다:

```java
public class BlackListNotifier implements ApplicationListener<BlackListEvent> {

    private String notificationAddress;

    public void setNotificationAddress(String notificationAddress) {
        this.notificationAddress = notificationAddress;
    }

    public void onApplicationEvent(BlackListEvent event) {
        // notificationAddress를 이용하여 적합한 부분에 알림처리한다...
    }
}
```

`ApplicationListener`는 일반적으로 자신만의 이벤트(이 예시에서는 `BlackListEvent`)를 파라메터로 가진다. 이 말은 `onApplicationEvent()`는 타입 세이프하여 다운 캐스팅을 할 필요가 없다. 기본적으로 이벤트 리스너는 이벤트를 동기적으로 전달받는다. 이 말은 `publishEvent()` 메소드는 모든 이벤트 처리가 완료될 때까지 다른 동작을 막는다는 의미이다. 이 동기적이고 단일 쓰레드식 접근의 한 장점은 트랜잭션이 사용가능한 경우 트랜잭션 내에서 동작한다는 것이다. 이벤트 발행에 다른 접근법이 필요한 경우 스프링 [`ApplicationEventMulticaster`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/event/ApplicationEventMulticaster.html)인터페이스와 [`SimpleApplicationEventMulticaster`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/event/SimpleApplicationEventMulticaster.html) 구현체를 보고 설정하면 된다.

아래의 예시는 위 클래스들로 빈 정의를 설정하여 빈을 등록하는 예시이다:

```xml
<bean id="emailService" class="example.EmailService">
    <property name="blackList">
        <list>
            <value>known.spammer@example.org</value>
            <value>known.hacker@example.org</value>
            <value>john.doe@example.org</value>
        </list>
    </property>
</bean>

<bean id="blackListNotifier" class="example.BlackListNotifier">
    <property name="notificationAddress" value="blacklist@example.org"/>
</bean>
```

`emailService`빈의 `sendEmail()` 메소드가 호출될 때, 블랙리스트 되야하는 이메일 메세지가 존재한다면 `BlackListEvent`라는 커스텀 이벤트가 발행된다. `blackListNotifier` 빈이 `ApplicationListener`로 등록되어 `BlackListEvent`를 전달받아 적절한 곳에 알림처리할 수 있다.

| |
| --- |
| ***!** 스프링의 이벤트 동작 원리는 같은 컨텍스트 내의 스프링 빈들간의 간단한 상호작용을 목표로 설계되었다. 하지만 더 복잡한 기업용 통합이 필요하다면 별도로 관리되는 [스프링 통합](https://projects.spring.io/spring-integration/) 프로젝트가 가벼우면서 [패턴 기반](https://www.enterpriseintegrationpatterns.com/)의 이벤트에 의해 동작하는 스프링으로 만들어진 아키텍처를 제공한다.*|

<h5 id="context-functionality-events-annotation">어노테이션 기반 이벤트 리스너</h5>

스프링 4.2부터 `@EventListener` 어노테이션을 사용하여 임의의 public 메소드를 이벤트 리스너로 설정할 수 있다. `BlackListNotifier`는 아래처럼 작성될 수도 있다:

```java
public class BlackListNotifier {

    private String notificationAddress;

    public void setNotificationAddress(String notificationAddress) {
        this.notificationAddress = notificationAddress;
    }

    @EventListener
    public void processBlackListEvent(BlackListEvent event) {
        // notificationAddress를 이용하여 적합한 부분에 알림처리한다...
    }
}
```

특정한 이벤트 리스너 인터페이스를 구현하지 않으며 임의의 이름과 이벤트 타입을 가진 어규먼트로 메소드가 정의되었다. 이벤트 타입은 구현체에서 타입을 설정한다면 제네릭으로 설정될 수도 있다.

만약 이벤트 여러개를 설정하거나 메소드 파라메터 없이 정의하고자한다면 어노테이션 자체에 이벤트를 설정할 수 있다. 아래는 그 예시이다:

```java
@EventListener({ContextStartedEvent.class, ContextRefreshedEvent.class})
public void handleContextStart() {
    // ...
}
```

어노테이션의 `condition` 어트리뷰트에 [`SpEL` 표현식](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/core.html#expressions)으로 특정 이벤트에만 동작하도록 추가적인 필터링 규칙을 추가할 수 있다.

아래의 예시는 이벤트의 `content` 어트리뷰트가 `my-event`인 경우에만 동작하도록 하는 예시이다:

```java
@EventListener(condition = "#blEvent.content == 'my-event'")
public void processBlackListEvent(BlackListEvent blEvent) {
        // notificationAddress를 이용하여 적합한 부분에 알림처리한다...
}
```

각각의 `SpEL` 표현식은 각각의 컨텍스트에서 해석된다. 아래의 표는 가능한 컨텍스트 리스트이다. 이벤트 조건을 설정하는데 사용할 수 있다:

#### 표 8. 이벤트 SpEL 사용가능한 메타데이터

| 이름 | 위치 | 설명 | 예시 |
| ----- | ----- | ----- | ----- |
| 이벤트 | 루트 객체 | `ApplicationEvent` 그 자체 | `#root.event` 혹은 `event` |
| 어규먼트 배열 | 루트 객체 | 메소드를 호출하는데 전달되는 어규먼트들 | `#root.args` or `args`; `args[0]`는 첫번째 어규먼트를 의미한다 |
| 어규먼트 이름 | 평가 컨텍스트 | 메소드 어규먼트의 이름. 만약에 이름을 사용할 수 없다면(예를 들면, 컴파일된 바이트코드에 디버그 정보가 없을 경우), 각각의 어규먼트들은 `#a<#arg>` 표현으로 사용가능하다 여기서 `<#arg>`는 어규먼트 인덱스(0에서 시작)를 의미한다. | `#blEvent` 또는 `#a0` (`#p0` 혹은 `#p<#arg>`를 대신하여 사용할 수 있다) |

`#root.event`는 해당하는 이벤트에 접근할 수 있도록 해준다. 메소드가 이벤트가 아닌 임의의 객체를 참조하는 형태인 경우에도 가능하다.

이벤트 처리의 결과 새로운 이벤트를 발행해야 하는 경우, 메소드가 해당하는 이벤트를 반환하도록 변경하면 된다. 아래는 그 예시이다:

```java
@EventListener
public ListUpdateEvent handleBlackListEvent(BlackListEvent event) {
        // notificationAddress를 이용하여 적합한 부분에 알림처리한다...
        // 그 후 ListUpdateEvent를 발행한다...
}
```

| |
| --- |
| ***!** 이 기능은 [비동기 리스너](#context-functionality-events-async)에서는 동작하지 않는다.*|

위 메소드에서 `BlackListEvent`를 처리할 때마다 새로운 `ListUpdateEvent`가 발행된다. 여러개의 이벤트를 발행해야 한다면 이벤트 `Collection`을 반환하면 된다.

<h5 id="context-functionality-events-async">비동기 리스너</h5>

특정 이벤트 리스너가 이벤트 처리를 비동기로 진행하기를 원한다면, [일반적인 `@Async` 지원](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/integration.html#scheduling-annotation-support-async)과 마찬가지로 사용할 수 있다. 아래는 그 예시이다:

```java
@EventListener
@Async
public void processBlackListEvent(BlackListEvent event) {
    // 별개의 쓰레드에서 BlackListEvent가 처리된다.
}
```

비동기 이벤트를 사용할 때의 한계점에 대해 잘 알고 사용해야 한다:

* 비동기 이벤트 리스너가 `Exception`을 발생시킬경우, 호출처에 전달되지 않는다. 자세한 내용은 `AsyncUncaughtExceptionHandler`를 확인하면 된다.
* 이벤트를 반환하는 방식으로 새로운 이벤트를 발생시키는 기능을 비동기 이벤트 리스너는 사용할 수 없다. 새로운 이벤트를 발생시켜야 한다면, [`ApplicationEventPublisher`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/aop/interceptor/AsyncUncaughtExceptionHandler.html)를 주입하여 수동으로 이벤트를 발생시켜야 한다.

<h5 id="context-functionality-events-order">리스너 순서 정하기</h5>

특정 리스너가 다른 리스너보다 늦게 실행되야 한다면 `@Order` 어노테이션을 메소드 정의에 추가하여 설정할 수 있다. 아래는 그 예시이다:

```java
@EventListener
@Order(42)
public void processBlackListEvent(BlackListEvent event) {
        // notificationAddress를 이용하여 적합한 부분에 알림처리한다...
}
```

<h5 id="context-functionality-events-generics">Generic 이벤트</h5>

이벤트 구조에 제네릭을 사용하는 것도 가능하다. `EntityCreatedEvent<T>`를 생각 해보자. 여기서 T는 생성된 엔티티의 타입이다. 예를 들면 `Person` 타입의 `EntityCreatedEvent`의 리스너를 아래와 같이 설정할 수 있다:

```java
@EventListener
public void onPersonCreated(EntityCreatedEvent<Person> event) {
    // ...
}
```

타입 제거때문에 이러한 방법은 오직 이벤트 리스너 필터에서 제네릭 타입을 확인할 수 있는 경우에만 사용가능하다(즉, `class PersonCreatedEvent extends EntityCreatedEvent<Person> { …​ }`와 같은 경우).

특정 환경에서 모든 이벤트가 같은 구조를 가지는 경우 이 작업은 지루한 작업이 될수 있다. 이러한 경우 `ResolvableTypeProvider`를 구현하여 프레임워크가 런타임 환경이 제공하는 것 이상의 정보를 얻도록 할 수 있다. 아래는 그 예시이다:

```java
public class EntityCreatedEvent<T> extends ApplicationEvent implements ResolvableTypeProvider {

    public EntityCreatedEvent(T entity) {
        super(entity);
    }

    @Override
    public ResolvableType getResolvableType() {
        return ResolvableType.forClassWithGenerics(getClass(), ResolvableType.forInstance(getSource()));
    }
}
```

| |
| --- |
| ***@** 이 기능은 이벤트로 `ApplicationEvent`를 사용할 때 뿐만 아니라 임의의 객체를 사용할 때에도 정상 동작한다.*|

<h4 id="context-functionality-resources">로우레벨 자원에 편리한 접근</h4>

어플리케이션 컨텍스트의 이해와 최적화를 위해 스프링 [`Resource` 추상화](../spring-reference-core-2-resources#resources)에 대해 익숙해져야 한다.

`ResourceLoader`는 `Resource` 객체를 로드하는데 사용되며 어플리케이션 컨텍스트는 그 자체로 `ResourceLoader`이다. `Resource`는 근본적으로 JDK `java.net.URL` 클래스의 확장된 버전이다. 사실 `Resource`의 구현은 `java.net.URL`을 감싸고 있는 형태로 구현되어 있다. `Resource`는 파일시스템, 클래스패스, 표준 URL등 다양한 곳으로 부터 로우레벨 자원을 획득할 수 있다. 자원의 위치가 접두사가 없는 일반적인 경로라면 어플리케이션 컨텍스트에 따라 이러한 자원을 얻는 곳이 다르다.

`ResourceLoaderAware`콜백 인터페이스를 구현한 빈을 어플리케이션 컨텍스트에 배포한다면, 초기화 시점에 어플리케이션 컨텍스트가 자기 자신을 `ResourceLoader`로써 파라메터로 콜백 메소드를 호출할 것이다. 또한 정적 자원을 접근하기 위해 `Resource`타입의 프로퍼티를 이용할 수 있다. 이러한 프로퍼티는 다른 일반적인 프로퍼티처럼 주입될 것이다. 이러한 `Resource` 프로퍼티를 일반적인 `String`형태의 경로로 나타낼 수 있다. 이러한 경우, 빈이 배포되는 시점에 자동으로 문자열로부터 실제 `Resource` 객체로 전환해줄 것이다.

`ApplicationContext`의 생성자에 제공되는 자원의 경로 또한 다른 경로와 마찬가지이다. 이러한 경로도 컨텍스트 구현체에 따라 적절한 방식으로 처리될 것이다. 예를 들면 `ClassPathXmlApplicationContext`는 단순한 경로를 클래스 패스 경로로 인식한다. 또한 어플리케이션 컨텍스트의 종류가 무엇이든간에 자원의 경로에 접두사를 붙여 특정 방식으로 인식되도록 할 수 있다.

<h4 id="context-create">웹 어플리케이션을 위한 편리한 ApplicationContext 인스턴스화</h4>

`ContextLoader`를 사용하여 `ApplicationContext` 인스턴스를 선언적으로 생성할 수 있다. 물론 `ApplicationContext` 구현체를 사용하여 프로그래밍적으로 `ApplicationContext`를 생성하는 것도 가능하다.

`ContextLoaderListener`를 사용하여 `ApplicationContext`를 등록하는 예시이다:

```xml
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/daoContext.xml /WEB-INF/applicationContext.xml</param-value>
</context-param>

<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

리스너틑 `contextConfigLocation`파라메터를 확인한다. 이 파라메터가 없으면 리스너는 `/WEB-INF/applicationContext.xml`을 
기본값으로 사용한다. 이 파라메터가 있으면 `String`을 이미 정의된 경계기호(쉼표, 쌍반점, 공백)으로 구분하여 어플리케이션 컨텍스트가 확인해야하는 위치로 사용한다. Ant 형식의 경로 표현도 사용 가능하다. 예를 들면 `/WEB-INF/*Context.xml` (`WEB-INF` 디렉토리 내부의 `Context.xml`로 끝나는 모든 파일), `/WEB-INF/**/*Context.xml`(`WEB-INF`하위의 모든 디렉토리 내부의 `Context.xml`로 끝나는 파일)

<h4 id="context-deploy-rar">스프링 ApplicationContext를 Java EE RAR 파일로 배포하기</h4>

스프링 `ApplicationContext`를 컨텍스트, 필요한 빈 클래스들, 라이브러리 JAR파일을 RAR 파일로 배포할 수 있다. 이 작업은 Java EE 서버 설비에 접근할 수 있는 독립적인 `ApplicationContext`를 구동시키는 것과 같은 작업이다. RAR 배포는 자바 EE 환경에서 스프링 `ApplicationContext`를 부팅하기 위해 HTTP 연결점이 없는 WAR 파일을 배포하는 것보다 더 자연스러운 방식이다.

RAR 배포는 HTTP 연결점이 없는 대신에 메세지 연결점이 있고 스케쥴된 작업이 있는 컨텍스트의 경우에 이상적이다. 이러한 컨텍스트의 빈들은 JTA 트랜잭션 관리자나 JNDI JDBC `DataSource` 인스턴스, JMS `ConnectionFactory`와 같은 어플리케이션 서버 자원들 사용할 수 있다. 또한 스프링의 표준 트랜잭션 관리, JNDI & JMX 지원 기능을 이용하여 플랫폼의 JMX 서버에 등록할 수 있다. 어플리케이션 컴포넌트는 스프링 `TaskManager` 추상화를 통해 어플리케이션 서버의 JCA `WorkManager`와 상호작용할 수 있다.

RAR 배포와 관련되 자세한 설정에 관한 정보는 [`SpringContextResourceAdapter`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/jca/context/SpringContextResourceAdapter.html) 자바독에서 확인할 수 있다.

스프링 어플리케이션을 Java EE RAR 파일로 손쉽게 배포하기 위해서는:

1. 모든 어플리케이션 클래스를 RAR 파일(확장자만 다른 표준 JAR 파일)로 패키징 한다. 필요한 모든 JAR를 RAR 아카이브 루트에 추가한다. `META-INF/ra.xml`을 배포 설명자([`SpringContextResourceAdapter`자바독](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/jca/context/SpringContextResourceAdapter.html)에 설명되어 있다)로 추가하고 연관되는 스프링 XML 빈 정의 파일(일반적으로 `META-INF/applicationContext.xml`)을 추가한다.

2. 생성된 RAR 파일으 어플리케이션 서버의 배포 디렉토리에 옮긴다.

| |
| --- |
| ***!** 이러한 RAR 파일은 보통 스스로를 포장한다. 내부 컴포넌트들을 외부에 노출시키지 않고 심지어 같은 어플리케이션의 다른 모듈에 노출시키지 않는다. RAR 기반 `ApplicationContext`은 보통 다른 모듈과 공유하는 JMS 목적지를 통하여 상호작용한다. RAR 기반 `ApplicationContext`는 작업을 스케쥴할 수 있고 파일시스템의 새로운 파일에 반응할 수 있다. 외부에 동기화된 접근이 필요한 경우, RMI 연결점을 노출시킬 수 있다. 이러한 RMI 연결점은 같은 기기의 다른 어플리케이션 모듈에 의해 사용된다.*|

<h3 id="beans-beanfactory">빈 팩토리</h3>

`BeanFactory` API는 스프링 IoC 컨테이너의 기반이 되는 기술을 제공한다. 이 API는 일반적으로 스프링의 다른 부분과 관련된 써드 파티 프레임워크를 통합하는데 사용된다. `DefaultListableBeanFactory` 구현체는 `GenericApplicationContext` 하이 레벨 컨테이너의 내부에서 관련된 작업을 위임받는 주요 클래스이다.

`BeanFactory`와 그와 관련된 인터페이스(`BeanFactoryAware`, `InitializingBean`, `DisposableBean`)은 다른 프레임워크 컴포넌트의 주요 통합 지점이다. 어노테이션이나 리플렉션을 필요로 하지 않기 때문에, 컨테이너나 다른 컴포넌트와 효율적인 상호작용을 할 수 있다. 어플리케이션 레벨 빈들이 같은 콜백 인터페이스를 사용할 수도 있지만 일반적으로 어노테이션이나 프로그래밍적 설정을 통한 선언적인 의존성 주입을 선호한다.

`BeanFactory` API와 `DefaultListableBeanFactory` 구현체는 설정 형식이나 컴포넌트 어노에이션에 대한 추정을 하지 않는다. 이러한 작업은 확장기능(`XmlBeanDefinitionReader`나 `AutowiredAnnotationBeanPostProcessor`)에서 처리되며 메타데이터 표현으로 `BeanDefinition` 객체에 작업한다. 이것이 스프링 컨테이너가 확장성이 있고 유연한 이유이다.

<h4 id="context-introduction-ctx-vs-beanfactory">BeanFactory? 혹은 ApplicationContext?</h4>

이 장에서 `BeanFactory`와 `ApplicationContext` 간의 차이점, 부팅과정에서 구현방법에 대해 이야기 할것이다.

`ApplicationContext`를 사용하지 않아야하는 특별한 이유가 없다면 `ApplicationContext`를 사용해야 한다. `GenericApplicationContext`, 자식클래스인 `AnnotationConfigApplicationContext`는 커스텀 부팅에 자주 사용되는 구현체이다. 이 것이 스프링 핵심 컨테이너를 일반적인 용도로 사용할 때, 가장 최선의 시작점이다: 설정 파일을 로드하고 클래스 패스 스캔을 시작하고 빈 정의와 어노테이션이 적용된 클래스들을 등록하고 (스프링 5.0부터) 함수형 빈 정의를 등록하는 용도 등등.

`ApplicationContext`는 `BeanFactory`의 모든 기능을 포함하고 있기 때문에, 일반적인 `BeanFactory`를 사용하는 것보다 더 좋다. 단, 빈 처리하는 과정을 완전히 통제할 필요가 있는 경우는 예외이다. `ApplicationContext` 내부에서 (`GenericApplicationContext`와 같은 구현체 내부를 말한다) 몇몇 종류의 빈들은 컨벤션에 따라 발견된다(후처리기와 같은 빈들을 이름이나 타입으로 발견한다). 반면에 `DefaultListableBeanFactory`는 그러한 특별한 빈에 대하여 알지 못한다.

어노테이션 처리나 AOP 프록시 처리와 같은 대부분의 컨테이너 확장 기능들을 처리하기 위해서 [`BeanPostProcessor` 확장 지점](#beans-factory-extension-bpp)이 필요하다. 단순히 `DefaultListableBeanFactory`를 사용하면 이러한 후처리기는 발견되지 않고 활성화되지도 않는다. 이러한 상황은 혼란스러울 수 있다. 왜냐하면 빈 설정에는 아무 문제가 없기 때문이다. 단지 이러한 경우에는 컨테이너가 추가적인 단계를 진행해 처리할 필요가 있는 것이다.

아래의 표는 `BeanFactory`와 `ApplicationContext` 인터페이스와 구현체에서 제공하는 기능의 리스트이다:

#### 표 9. 기능

| 기능 | `BeanFactory` | `ApplicationContext` |
| ----- | ----- | ----- |
| 빈 초기화와 빈 연결하기 | 예 | 예 |
| 생명주기 관리 | 아니오 | 예 |
| `BeanPostProcessor` 자동 등록 | 아니오 | 예 |
| `BeanFactoryPostProcessor` 자동 등록 | 아니오 | 예 |
| 편리한 `MessageSource` 접근 (국제화 처리) | 아니오 | 예 |
| `ApplicationEvent` 발행 기능 | 아니오 | 예 |

`DefaultListableBeanFactory`에 빈 후처리기 빈들을 명시적으로 등록해주기 위해서는 `addBeanPostProcessor`를 프로그래밍 적으로 호출해야 한다. 아래는 그 예시이다:

```java
DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
// 빈 정의들로 빈 팩토리를 생성한다.

// 이제 필요한 BeanPostProcessor 들을 추가한다.
factory.addBeanPostProcessor(new AutowiredAnnotationBeanPostProcessor());
factory.addBeanPostProcessor(new MyBeanPostProcessor());

// 이제 빈 팩토리를 사용하기 시작한다.
```

`DefaultListableBeanFactory`에 `BeanFactoryPostProcessor`를 적용하기 위해서는 `postProcessBeanFactory` 메소드를 호출해야 한다. 아래는 그 예시이다:

```java
DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(factory);
reader.loadBeanDefinitions(new FileSystemResource("beans.xml"));

// 프로퍼티 파일로 부터 몇몇 프로퍼티를 가져온다.
PropertySourcesPlaceholderConfigurer cfg = new PropertySourcesPlaceholderConfigurer();
cfg.setLocation(new FileSystemResource("jdbc.properties"));

// 이제 필요한 대체작업을 진행한다.
cfg.postProcessBeanFactory(factory);
```

두가지 경우 모두 불편하게도 명시적인 등록과정이 있다. 이것이 스프링 백엔드 어플리케이션에서 `ApplicationContext`를 `DefaultListableBeanFactory`보다 선호하는 이유이다. 특히 기업에서 사용하는 설정에는 `BeanFactoryPostProcessor`와 `BeanPostProcessor`에 의존하는 경우가 많아서 더욱 그러하다.

| |
| --- |
| ***!** `AnnotationConfigApplicationContext`는 일반적인 어노테이션 후처리기가 등록되어 있다. 또한 `@EnableTransactionManagement`와 같은 설정 어노테이션을 처리하기 위해 추가적인 후처리기를 가져오는 경우도 있다. 스프링 어노테이션 레벨 설정 모델을 추상화하기 위해 이러한 빈 후처리기는 단지 컨테이너 내부의 상세 구현에만 존재한다.*|

[다음 장에서 계속](../spring-reference-core-2-resources#resources)