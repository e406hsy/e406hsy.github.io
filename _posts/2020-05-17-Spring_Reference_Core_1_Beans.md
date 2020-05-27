---
layout: post
title:  "[Spring Reference] 스프링 레퍼런스 #1 핵심 - 1. IoC 컨테이너"
createdDate:   2020-05-17T18:42:00+09:00
date:   2020-05-27T09:28:00+09:00
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
<h4 id="beans-factory-autowire">자동연결 협력자</h4>

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

