---
layout: post
title:  "[Spring Reference] 스프링 레퍼런스 #1 핵심 - 1. IoC 컨테이너"
createdDate:   2020-05-17T18:42:00+09:00
date:   2020-05-23T20:00:00+09:00
excerpt: "한글 번역 : 스프링 레퍼런스 #1 핵심 - 1. IoC 컨테이너"
pagination: enabled
author: SoonYong Hong
categories: Spring_Reference
tags: Spring Reference
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
            * <a href="#beans-factory-class-ctor">컨테이너로 인스턴스 만들기</a>
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
            * <a href="#beans-value-element">변경없는 값(Primitives, String 등등)</a>
            * <a href="#beans-ref-element">다른 빈 참조(협업)</a>
            * <a href="#beans-inner-beans">내부 빈</a>
            * <a href="#beans-collection-elements">컬렉션</a>
            * <a href="#beans-null-element">Null과 비어있는 스트링</a>
            * <a href="#beans-p-namespace">P 네임스페이스를 이용한 XML 단축</a>
            * <a href="#beans-c-namespace">c 네임스페이스를 이용한 XML 단축</a>
            * <a href="#beans-compound-property-names">복합 프로퍼티 이름</a>
        1. <a href="#beans-factory-dependson">depends-on 사용하기</a>
        1. <a href="#beans-factory-lazy-init">지연 초기화 빈</a>
        1. <a href="#beans-factory-autowire">Autowiring 협력자</a>
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
            * <a href="#beans-factory-lifecycle-disposablebean">파괴 콜백</a>
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
1. <a href="#appendix"> 부록 </a>

---

<h1 id="spring-core"> 핵심 기술 </h1>

레퍼런스 문서 중 이 부분은 스프링 프레임워크에 필수적인 기술을 담당한다.     
이 것들중 가장 중요한 것은 스프링 프레임워크의 역 흐름 제어(IoC)이다. 스프링 프레임워크 IoC 컨테이너의 철저한 관리는 스프링의 광범위한 관점 지향 프로그래밍(AOP) 적용 범위에 기반한다. 스프링 프레임워크는 개념적으로 이해하기 쉽고 자바 엔터프라이즈 프로그래밍에 요구조건의 핵심 80%를 성공적으로 처리하는 자체 AOP 프레임워크를 가지고 있다.     
또한 스프링은 AspectJ(현재 자바 엔터프라이즈 환경에서 가장 질 좋고 성숙한 AOP 구현)포함시켜 제공된다.

<h2 id="beans"> IoC 컨테이너 </h2>

이 장은 스프링의 역 흐름 제어(IoC) 컨테이너를 담당한다.

<h3 id="beans-introduction">IoC 컨테이너와 빈의 소개</h3>

이 장은 스프링 프레임워크의 역흐름 제어 구현 원리를 담당한다. 역 흐름 제어(IoC)는 의존성 주입(DI)로도 알려져 있다. 이것은 객체가 생성자 어규먼트, 팩토리 메서드 어규먼트, 생성되거나 팩토리 메소드로부터 리턴된후 객체 인스턴트에 설정되는 프로퍼티만을 통해 자신의 의존성(함께 동작하는 다른 객체)을 정의하는 과정이다. IoC 컨테이너는 빈을 생성할 때, 이러한 의존성을 주입한다. 이 과정은 기본적으로 서비스 중개자 패턴(Service Locator pattern)과 같은 방법이나 객체의 직접생성 등의 방법으로 빈 스스로 자신의 의존성을 인스턴스화하는 것의 반대이다.     
`org.springframework.beans`와 `org.springframework.context` 패키지는 스프링 프레임워크 IoC 컨테이너의 기반이 된다. [`BeanFactory`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/beans/factory/BeanFactory.html)인터페이스는 어떠한 타입의 객체든 관리할 수 있는 추가적인 설정 방법을 제공한다. [`ApplicationContext`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/ApplicationContext.html)는 `BeanFactory`의 서브 인터페이스이며 아래의 것들을 추가한다.
* 스프링 AOP 기능의 손쉬운 통합
* 메세지 리소스 처리(국제화에 사용)
* 이벤트 생성
* 웹 어플리케이션에서의 `WebApplicationContext`와 같은 어플리케이션 레이어 콘텍스트

요약하면, `BeanFactory`는 프레임워크와 기본적인 기능 설정을 제공하고 `ApplicationContext`는 더 기업 특화적인 기능을 추가한다. `ApplicationContext`는 `BeanFactory`의 완전한 상위 호환이며 오로지 이 장에서 스프링 IoC 컨테이너의 설명에 사용된다. `ApplicationContext`대신 `BeanFactory`를 사용하는 방법에 대한 추가적인 정보는 [`BeanFactory`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/beans/factory/BeanFactory.html)를 보십시오.      
스프링에서 어플리케이션의 기반을 이루며 스프링 IoC 컨테이너에 의해 관리되는 객체들을 빈이라고 부른다. 빈은 스프링 IoC 컨테이너에 의하여 인스턴스화 되고 수집된다. 다른말로 하면 스프링 IoC 컨테이너에 의해 관리된다. 또 다른말로 하면, 빈은 어플리케이션에 수많은 객체중 하나이다. 빈과 그들사이의 의존성은 컨테이너에서 사용하는 설정 메타데이터에 의해 가져온다.

<h3 id="beans-basics">컨테이너 개요</h3>
`org.springframework.context.ApplicationContext`인터페이스는 스프링 IoC 컨테이너를 의미하며 빈을 인스턴스화하고 설정하며 수집하는 역할을 한다. 컨테이너를 설정 메타데이터를 읽음으로서 어떠한 객체를 인스턴스화하고 설정하고 수집할 지 수행할 명령을 가져온다. XML, 자바 어노테이션, 자바 코드의 형태로 설정 메타데이터를 표현한다. 이 메타데이터는 어플리케이션을 구성하는 객체와 객체간의 의존성을 표현할 수 있게 한다.     
스프링은 몇개의 `ApplicationContext`구현체를 제공한다. 독립형 어플리케이션에서 [`ClassPathXmlApplicationCOntext`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/support/ClassPathXmlApplicationContext.html)나 [`FileSystemXmlApplicationContext`](https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/javadoc-api/org/springframework/context/support/FileSystemXmlApplicationContext.html)를 생성하는 것은 흔한 일이다. XML은 전통적으로 설정 메타데이터를 정의하는 형식으로 생각되어 왔지만 아주 약간의 XML 설정만으로 자바 어노테이션이나 코드를 메타데이터 형식으로 컨테이너에 제공할 수 있다.     
대부분의 어플리케이션 시나리오에서, 하나 혹은 그 이상의 스프링 IoC 컨테이너를 인스턴스화 하기 위해 유저의 코드는 필요없다. 예를 들어 웹 어플리케이션 시나리오에서, 어플리케이션의 `web.xml`파일의 간단한 8줄의 상용 웹 설명자 XML이면 충분하다.([웹 어플리케이션을 위한 편리한 ApplicationContext 인스턴스화 하기](#context-create)를 보십시오.) 만약 [Spring Tools for Exlipse](https://spring.io/tools)(이클립스 기반 개발 환경)을 사용한다면 몇 번의 마우스 클릭과 키보드를 누름으로서 이 상용 설정을 쉽게 만들 수 있다.     
아래의 그림은 스프링의 작동하는 방법을 추상화하여 보여준다. `ApplicationContext`가 생성되고 초기화된 뒤, 당신의 어플리케이션의 클래스들은 설정 메타데이터와 결합하여 완전히 구성되고 실행가능한 시스템이나 어플리케이션이 생겨난다.
<img src="https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/images/container-magic.png"/>
**그림 1. 스프링 IoC 컨테이너**

<h4 id="beans-factory-metadata">메타데이터 설정하기</h4>
위의 다이어그램에서 볼 수 있듯이, 스프링 IoC 컨테이너는 설정 메타데이터를 이용한다. 어플리케이션 개발자가 스프링 컨테이너에게 어플리케이션의 객체들을 인스턴스화하고 설정하고 수집하는 방법을 표현하는 것이 설정 메타데이터이다.     
전통적으로 메타데이터는 간단하고 직관적인 XML 형식으로 제공되었고 이 XML형식은 이 장에서 스프링 IoC 컨테이너의 주요한 개념과 기능을 표현하기 위해 사용된다.

| XML 기반 메타데이터는 메타데이터를 설정하는 유일한 형식이 아니다. 스프링 IoC 컨테이너 그 자체는 설정 메타데이터가 어떤 형식으로 작성되어 있는 지와 완전히 무관하다. 최근에는 많은 개발자들은 스프링 어플리케이션에서 [자바 기반 설정](#beans-java)을 선택한다. |

스프링 컨테이너 메타데이터에서 사용되는 다른 형식에 관한 정보를 보려면:

* [어노테이션 기반 설정](#beans-annotation-config): 스프링 2.5부터 어노테이션 기반 메타데이터를 지원한다.
* [자바 기반 설정](#beans-java): 스프링 3.0부터 시작된 자바설정 프로젝트에서 제공되는 많은 기능들은 스프링 프레임워크의 핵심부분이 되었다. 따라서, XML 파일 대신에 자바 클래스를 이용하여 어플리케이션 외부 빈을 정의할 수 있다. 이 기능을 사용하기 위해서 [`@Configuration`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Configuration.html), [`@Bean`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html), [`@Import`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Import.html), [`@DependsOn`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/DependsOn.html) 어노테이션을 보십시오.

스프링 설정은 컨테이너가 관리할 최소 한개의 빈 정의, 일반적으로는 여러개의 빈 정의로 구성된다. XML 기반 설정 메타데이터는 이러한 빈을 최상위 `<beans/>`요소 안에 `<bean/>` 요소로 설정한다. 자바 설정은 일반적으로 `@Configuration`클래스 내부에 `@Bean`어노테이션 메서드를 사용한다.     
이러한 빈 정의는 어플리케이션을 만드는 실제 객체와 일치한다. 일반적으로 서비스 레이어 객체, DB에 접근하는 객체(DAOs), Struts `Action`과 같은 프레젠테이션 객체, Hibernate `SessionFactories`와 같은 기반장치 객체, JMS `Queues`와 같은 것들을 정의한다. 일반적으로 잘만들어진 도메인 객체는 컨테이너에 설정하지 않는다. 왜냐하면 도메인 객체를 만들고 불러오는 것은 대개 DAO와 비즈니스 로직의 역할이기 때문인다. 그러나 스프링에 통합된 AspectJ를 이용하여 IoC 컨테이너의 통제 밖에서 생성된 객체를 설정할 수 있다. [AspectJ를 이용하여 스프링에서 도메인 객체에 의존성 주입하기](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/DependsOn.html)를 보십시오.     
아래의 예제는 기본적인 XML 기반 설정 메타데이터를 보여줍니다:
```
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
<h4 id="beans-factory-instantiation">컨테이너 인스턴스 만들기</h4>

<h5 id="beans-factory-xml-import">XML 기반 설정 메타데이터 만들기</h5>
<h5 id="groovy-bean-definition-dsl">Groovy 빈 정의 DSL</h5>


<h4 id="bean-factory-client">컨테이너 이용하기</h4>


<h3 id="beans-definition">빈 개요</h3>

<h4 id="beans-beanname">빈 이름 붙이기</h4>

<h5 id="beans-beanname-alias">빈 정의 외부에서 빈 별명 설정하기</h5>


<h4 id="beans-factory-class">빈 인스턴스 만들기</h4>

<h5 id="beans-factory-class-ctor">컨테이너로 인스턴스 만들기</h5>
<h5 id="beans-factory-class-static-factory-method">스태틱 팩토리 메소드로 만들기</h5>
<h5 id="beans-factory-class-instance-factory-method">인스턴스 팩토리 메소드로 만들기</h5>
<h5 id="beans-factory-type-determination">빈의 런타임 타입 결정하기</h5>




<h3 id="beans-dependencies">의존성</h3>

<h4 id="beans-factory-collaborators">의존성 주입</h4>

<h5 id="beans-constructor-injection">생성자 기반 의존성 주입</h5>
<h5 id="beans-setter-injection">세터 기반 의존성 주입</h5>
<h5 id="beans-dependency-resolution">의존성 결정 과정</h5>
<h5 id="beans-some-examples">의존성 주입 예제</h5>


<h4 id="beans-factory-properties-detailed">의존성과 설정 상세</h4>

<h5 id="beans-value-element">변경없는 값(Primitives, String 등등)</h5>
<h5 id="beans-ref-element">다른 빈 참조(협업)</h5>
<h5 id="beans-inner-beans">내부 빈</h5>
<h5 id="beans-collection-elements">컬렉션</h5>
<h5 id="beans-null-element">Null과 비어있는 스트링</h5>
<h5 id="beans-p-namespace">P 네임스페이스를 이용한 XML 단축</h5>
<h5 id="beans-c-namespace">c 네임스페이스를 이용한 XML 단축</h5>
<h5 id="beans-compound-property-names">복합 프로퍼티 이름</h5>


<h4 id="beans-factory-dependson">depends-on 사용하기</h4>
<h4 id="beans-factory-lazy-init">지연 초기화 빈</h4>
<h4 id="beans-factory-autowire">Autowiring 협력자</h4>

<h5 id="beans-autowired-exceptions">자동 연결의 한계와 단점</h5>
<h5 id="beans-factory-autowire-candidate">자동 연결에 빈 제외하기</h5>


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
<h5 id="beans-factory-lifecycle-disposablebean">파괴 콜백</h5>
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

