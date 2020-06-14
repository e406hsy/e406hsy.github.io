---
layout: post
title:  "[Spring Reference] 스프링 레퍼런스 #1 핵심 - 1. Ioc 컨테이너"
createdDate:   2020-05-17T18:42:00+09:00
date:   2020-05-21T22:26:00+09:00
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
1. <a href="#resources"> 리소스 </a>
    1. <a href="#resources-introduction">소개</a>
    1. <a href="#resources-resource">리소스 인터페이스</a>
    1. <a href="#resources-implementations">이미 만들어진 리소스 구현체</a>
        1. <a href="#resources-implementations-urlresource">UrlResource</a>
        1. <a href="#resources-implementations-classpathresource">ClassPathResource</a>
        1. <a href="#resources-implementations-filesystemresource">FileSystemResource</a>
        1. <a href="#resources-implementations-servletcontextresource">ServletContextResource</a>
        1. <a href="#resources-implementations-inputstreamresource">InputStreamResource</a>
        1. <a href="#resources-implementations-bytearrayresource">ByteArrayResource</a>
    1. <a href="#resources-resourceloader">ResourceLoader</a>
    1. <a href="#resources-resourceloaderaware">ResourceLoaderAware 인터페이스</a>
    1. <a href="#resources-as-dependencies">의존성으로서의 리소스</a>
    1. <a href="#resources-app-ctx">어플리케이션 콘텍스트와 리소스 패스</a>
        1. <a href="#resources-app-ctx-construction">어플리케이션 콘텍스트 생성하기</a>
            * <a href="#resources-app-ctx-classpathxml">ClassPathXmlApplicationContext 인스턴스 생성하기 - 지름길</a>
        1. <a href="#resources-app-ctx-wildcards-in-resource-paths">어플리케이션 콘텍스트 생성자 리소스 패스에 와일드카드</a>
            * <a href="#resources-app-ctx-ant-patterns-in-paths">Ant 스타일 패턴</a>
            * <a href="#resources-classpath-wildcards">classpath1.: 접두사</a>
            * <a href="#resources-wildcards-in-path-other-stuff">와일드카드와 관련된 추가적인 사항</a>
        1. <a href="#resources-filesystemresource-caveats">FileSystemResource 경고</a>
1. <a href="#validation"> 검증, 데이터 바인딩, 타입 변환 </a>
    1. <a href="#validator">스프링 Validator 인터페이스를 통한 검증</a>
    1. <a href="#validation-conversion">코드에 에러메세지 적용하기</a>
    1. <a href="#beans-beans">빈 조작과 BeanWrapper</a>
        1. <a href="#beans-beans-conventions">기본적인 프로퍼티, 중첩된 프로퍼티 설정하고 가져오기</a>
        1. <a href="#beans-beans-conversion">이미 구현된 PropertyEditor</a>
            * <a href="#beans-beans-conversion-customeditor-registration">커스텀 PropertyEditor 구현체 등록하기</a>
    1. <a href="#core-convert">스프링 타입 변환</a>
        1. <a href="#core-convert-Converter-API">변환기 서비스 제공자 인터페이스</a>
        1. <a href="#core-convert-ConverterFactory-SPI">ConverterFactory 사용하기</a>
        1. <a href="#core-convert-GenericConverter-SPI">GenericConverter 사용하기</a>
            * <a href="#core-convert-ConditionalGenericConverter-SPI">ConditionalGenericConverter 사용하기</a>
        1. <a href="#core-convert-ConversionService-API">ConversionService API</a>
        1. <a href="#core-convert-Spring-config">ConversionService 설정하기</a>
        1. <a href="#core-convert-programmatic-usage">ConversionService 프로그래밍 적으로 사용하기</a>
    1. <a href="#format">스프링 필드 형식 설정</a>
        1. <a href="#format-Formatter-SPI">Formatter 서비스 제공자 인터페이스</a>
        1. <a href="#format-CustomFormatAnnotations">어노테이션 기반 형식설정</a>
            * <a href="#format-annotations-api">형식 어노테이션 API</a>
        1. <a href="#format-FormatterRegistry-SPI">FormatterRegistry 서비스 제공자 인터페이스</a>
        1. <a href="#format-FormatterRegistrar-SPI">FormatterRegistrar 서비스 제공자 인터페이스</a>
        1. <a href="#format-configuring-formatting-mvc">스프링 MVC에서 형식 설정하기</a>
    1. <a href="#format-configuring-formatting-globaldatetimeformat">글로벌 Data, Time 형식 설정</a>
    1. <a href="#validation-beanvalidation">빈 검증</a>
        1. <a href="#validation-beanvalidation-overview">빈 검증 개요</a>
        1. <a href="#validation-beanvalidation-spring">빈 검증 제공자 설정하기</a>
            * <a href="#validation-beanvalidation-spring-inject">검증자 주입하기</a>
            * <a href="#validation-beanvalidation-spring-constraints">커스텀 제한사항 설정하기</a>
            * <a href="#validation-beanvalidation-spring-method">스프링 기반 메서드 검증</a>
            * <a href="#validation-beanvalidation-spring-other">추가적인 설정 옵션</a>
        1. <a href="#validation-binder">DataBinder 설정하기</a>
        1. <a href="#validation-mvc">스프링 MVC 3 검증</a>
1. <a href="#expressions"> 스프링 표현 언어(SpEL) </a>
    1. <a href="#expressions-evaluation">값 구하기</a>
        1. <a href="#expressions-evaluation-context">EvaluationContext 이해하기</a>
            * <a href="#expressions-type-conversion">형식 변환</a>
        1. <a href="#expressions-parser-configuration">파서 설정하기</a>
        1. <a href="#expressions-spel-compilation">스프링 표현 언어 편집</a>
            * <a href="#expressions-compiler-configuration">컴파일러 설정하기</a>
            * <a href="#expressions-compiler-limitations">컴파일러 제한사항</a>
    1. <a href="#expressions-beandef">빈 정의 안의 표현</a>
        1. <a href="#expressions-beandef-xml-based">XML 설정</a>
        1. <a href="#expressions-beandef-annotation-based">어노테이션 설정</a>
    1. <a href="#expressions-langauge-ref">언어 레퍼런스</a>
        1. <a href="#expressions-ref-literal">문자 표현</a>
        1. <a href="#expressions-properties-arrays">프로퍼티, 배열, 리스트, 맵, 색인작성자</a>
        1. <a href="#expressions-inline-lists">인라인 리스트</a>
        1. <a href="#expressions-inline-maps">인라인 맵</a>
        1. <a href="#expressions-array-construction">배열 생성하기</a>
        1. <a href="#expressions-methods">메서드</a>
        1. <a href="#expressions-operators">연산자</a>
            * <a href="#expressions-operators-relational">관계 연산자</a>
            * <a href="#expressions-operators-logical">논리 연산자</a>
            * <a href="#expressions-operators-mathematical">산술 연산자</a>
            * <a href="#expressions-assignment">대입 연산자</a>
        1. <a href="#expressions-types">타입</a>
        1. <a href="#expressions-constructors">생성자</a>
        1. <a href="#expressions-ref-variables">변수</a>
            * <a href="#expressions-this-root">#this 와 #root 변수</a>
        1. <a href="#expressions-ref-functions">함수</a>
        1. <a href="#expressions-bean-references">빈 참조</a>
        1. <a href="#expressions-operator-ternary">삼항 연산자 (If-Then-Else)</a>
        1. <a href="#expressions-operator-elvis">엘비스 연산자</a>
        1. <a href="#expressions-operator-navigation">Safe Navigation 연산자</a>
        1. <a href="#expressions-collection-selection">컬렉션 선택</a>
        1. <a href="#expressions-collection-projection">컬렉션 투영</a>
        1. <a href="#expressions-templating">표현 템플릿화</a>
    1. <a href="#expressions-example-classes">예시에서 사용된 클래스</a>
1. <a href="#aop"> 스프링과 함께하는 관점 지향 프로그래밍(AOP) </a>
    1. <a href="#aop-introduction-defn">AOP 개념</a>
    1. <a href="#aop-introduction-spring-defn">스프링 AOP 기능과 목표</a>
    1. <a href="#aop-introduction-proxies">AOP Proxies</a>
    1. <a href="#aop-ataspectj">@AspectJ 지원</a>
        1. <a href="#aop-aspectj-support">@AspectJ 지원 활성화하기</a>
        1. <a href="#aop-at-aspectj">애스팩트 선언하기</a>
        1. <a href="#aop-pointcuts">포인트컷 선언하기</a>
            * <a href="#aop-pointcuts-designators">지원되는 포인트컷 지명자</a>
            * <a href="#aop-pointcuts-combining">포인트컷 표현 결합하기</a>
            * <a href="#aop-common-pointcuts">공통의 포인트컷 정의 공유하기</a>
            * <a href="#aop-pointcuts-examples">예시</a>
            * <a href="#writing-good-pointcuts">좋은 포인트컷 작성하기</a>
        1. <a href="#aop-advice">어드바이스 선언하기</a>
            * <a href="#aop-advice-before">Before 어드바이스</a>
            * <a href="#aop-advice-after-returning">AfterReturning 어드바이스</a>
            * <a href="#aop-advice-after-throwing">AfterThrowing 어드바이스</a>
            * <a href="#aop-advice-after-finally">After(Finally) 어드바이스</a>
            * <a href="#aop-advice-around-advice">Around 어드바이스</a>
            * <a href="#aop-advice-advice-params">어드바이스 파라메터</a>
            * <a href="#aop-advice-advice-ordering">어드바이스 순서 정하기</a>
        1. <a href="#aop-introduction">소개</a>
        1. <a href="#aop-instantiation-models">어스펙트 인스턴스화 모델</a>
        1. <a href="#aop-ataspectj-example">AOP 예시</a>
    1. <a href="#aop-schema">스키마 기반 AOP</a>
        1. <a href="#aop-schema-declaring-an-aspect">애스팩트 선언하기</a>
        1. <a href="#aop-schema-pointcuts">포인트컷 선언하기</a>
        1. <a href="#aop-schema-advice">어드바이스 선언하기</a>
            * <a href="#aop-schema-advice-before">Before 어드바이스</a>
            * <a href="#aop-schema-advice-after-returning">AfterReturning 어드바이스</a>
            * <a href="#aop-schema-advice-after-throwing">AfterThrowing 어드바이스</a>
            * <a href="#aop-schema-advice-after-finally">After(Finally) 어드바이스</a>
            * <a href="#aop-schema-advice-around-advice">Around 어드바이스</a>
            * <a href="#aop-schema-advice-advice-params">어드바이스 파라메터</a>
            * <a href="#aop-schema-advice-advice-ordering">어드바이스 순서 정하기</a>
        1. <a href="#aop-schema-introductions">소개</a>
        1. <a href="#aop-schema-instantiation-models">애스팩트 인스턴스화 모델</a>
        1. <a href="#aop-schema-advisors">어드바이저</a>
        1. <a href="#aop-schema-example">AOP 스키마 예시</a>
    1. <a href="#aop-choosing">사용할 AOP 정의 스타일 선택하기</a>
        1. <a href="#aop-spring-or-aspectj">스프링 AOP? 혹은 완전한 AspectJ?</a>
        1. <a href="#aop-ataspectj-or-xml">스프링 AOP에서 @AspectJ? 혹은 XML?</a>
    1. <a href="#aop-mixing-styles">Aspect 타입 섞기</a>
    1. <a href="#aop-proxying">프록시 작동원리</a>
        1. <a href="#aop-understanding-aop-proxies">AOP 프록시 이해하기</a>
    1. <a href="#aop-aspectj-programmatic">프로그래밍적으로 @AspectJ 프록시 만들기</a>
    1. <a href="#aop-using-aspectj">스프링 어플리케이션에서 AspectJ 사용하기</a>
        1. <a href="#aop-atconfigurable">스프링에서 도메인객체에 AspectJ를 이용하여 의존성 주입하기</a>
            * <a href="#aop-configurable-testing">@Configurable 객체 단위 테스트</a>
            * <a href="#aop-configurable-container">여러 개의 어플리케이션 컨텍스트 이용하기</a>
        1. <a href="#aop-ajlib-other">AspectJ를 위한 다른 스프링 애스팩트</a>
        1. <a href="#aop-aj-configure">스프링 IoC를 사용하여 AspectJ 애스팩트 설정하기</a>
        1. <a href="#aop-aj-ltw">스프링 프레임워크에서 AspectJ 로드 타임 위빙</a>
            * <a href="#aop-aj-ltw-first-example">첫 예시</a>
            * <a href="#aop-aj-ltw-the-aspects">애스팩트</a>
            * <a href="#aop-aj-ltw-aop_dot_xml">'META-INF/aop.xml'</a>
            * <a href="#aop-aj-ltw-libraries">필요 라이브러리(JARS)</a>
            * <a href="#aop-aj-ltw-spring">스프링 설정</a>
            * <a href="#aop-aj-ltw-environments">고유 환경 설정</a>
    1. <a href="#aop-resources">추가 자료</a>
1. <a href="#aop-api"> 스프링 AOP API </a>
    1. <a href="#aop-api-pointcuts">스프링에서 포인트컷</a>
        1. <a href="#aop-api-concepts">개념</a>
        1. <a href="#aop-api-pointcut-ops">포인트컷에서 작동</a>
        1. <a href="#aop-api-pointcut-aspectj">AspectJ 포인트컷 표현식</a>
        1. <a href="#aop-api-pointcuts-impls">편리한 PointCut 구현</a>
        1. <a href="#aop-api-superclasses">포인트컷 부모클래스</a>
        1. <a href="#aop-api-pointcuts-custom">커스텀 포인트컷</a>
    1. <a href="#aop-api-advice">스프링에서의 어드바이스</a>
        1. <a href="#aop-api-advice-lifecycle">어드바이스 생명주기</a>
        1. <a href="#aop-api-advice-types">스프링에서 어드바이스 유형</a>
            * <a href="#aop-api-advice-around">Interception Around 어드바이스</a>
            * <a href="#aop-api-advice-before">Before 어드바이스</a>
            * <a href="#aop-api-advice-throws">Throws 어드바이스</a>
            * <a href="#aop-api-advice-after-returning">After Returning 어드바이스</a>
            * <a href="#aop-api-advice-introduction">Introduction 어드바이스</a>
    1. <a href="#aop-api-advisor">스프링에서의 어드바이저</a>
    1. <a href="#aop-pfb">ProxyFactoryBean을 이용하여 AOP 프록시 만들기</a>
        1. <a href="#aop-pfb-1">기초</a>
        1. <a href="#aop-pfb-2">자바빈 프로퍼티</a>
        1. <a href="#aop-pfb-proxy-types">JDK 기반, CGLIB 기반 프로퍼티</a>
        1. <a href="#aop-api-proxying=intf">프록시 인터페이스</a>
        1. <a href="#aop-api-proxying-class">프록시 클래스</a>
        1. <a href="#aop-global-advisors">"글로벌" 어드바이스 사용하기</a>
    1. <a href="#aop-concise-proxy">간결한 프록시 정의</a>
    1. <a href="#aop-prog">ProxyFactory를 이용하여 AOP 프록시 프로그래밍적으로 만들기</a>
    1. <a href="#aop-api-advised">Advised 객체 조작하기</a>
    1. <a href="#aop-autoproxy">"자동-프록시" 기능 이용하기</a>
        1. <a href="#aop-autoproxy-choices">자동-프록시 빈 정의</a>
            * <a href="#aop-api-autoproxy">BeanNameAutoProxyCreator</a>
            * <a href="#aop-api-autoproxy-default">DefaultAdvisorAutoProxyCreator</a>
    1. <a href="#aop-targetsource">TargetSource 구현 이용하기</a>
        1. <a href="#aop-ts-swap">동적 전환가능한 Target Sources</a>
        1. <a href="#aop-ts-pool">Target Source 풀</a>
        1. <a href="#aop-ts-prototype">프로토타입 Target Sources</a>
        1. <a href="#aop-ts-thredlocal">ThreadLocal Target Sources</a>
    1. <a href="#aop-extensibility">새로운 어드바이스 타입 정의하기</a>
1. <a href="#null-safety"> 안전한 Null </a>
    1. <a href="#use-cases">사용 예시</a>
    1. <a href="#jsr-305-meta-annotations">JSR-305 메타 어노테이션</a>
1. <a href="#databuffers"> 데이터 버퍼와 코덱 </a>
    1. <a href="#databuffers-factory">DataBufferFactory</a>
    1. <a href="#databuffers-buffer">DataBuffer</a>
    1. <a href="#databuffers-buffer-pooled">PooledDataBuffer</a>
    1. <a href="#databuffers-utils">DataBufferUtils</a>
    1. <a href="#codecs">Codecs</a>
    1. <a href="#databuffers-using">DataBuffer 사용하기</a>
1. <a href="#appendix"> 부록 </a>
    1. <a href="#xsd-schemas">XML 스키마</a>
        1. <a href="#xsd-schemas-util">util 스키마</a>
            * <a href="#xsd-schemas-util-constant">&lt;util:constant/&gt;사용하기</a>
            * <a href="#xsd-schemas-util-property-path">&lt;util:property-path/&gt;사용하기</a>
            * <a href="#xsd-schemas-util-properties">&lt;util:properties/&gt;사용하기</a>
            * <a href="#xsd-schemas-util-list">&lt;util:list/&gt;사용하기</a>
            * <a href="#xsd-schemas-util-map">&lt;util:map/&gt;사용하기</a>
            * <a href="#xsd-schemas-util-set">&lt;util:set/&gt;사용하기</a>
        1. <a href="#xsd-schemas-aop">aop 스키마</a>
        1. <a href="#xsd-schemas-context">context 스키마</a>
            * <a href="#xsd-schemas-context-pphc">&lt;property-placeholder/&gt;사용하기</a>
            * <a href="#xsd-schemas-context-ac">&lt;annotation-config/&gt;사용하기</a>
            * <a href="#xsd-schemas-context-component-scan">&lt;component-scan/&gt;사용하기</a>
            * <a href="#xsd-schemas-context-ltw">&lt;load-time-weaver/&gt;사용하기</a>
            * <a href="#xsd-schemas-context-sc">&lt;spring-configured/&gt;사용하기</a>
            * <a href="#xsd-schemas-context-mbe">&lt;mbean-export/&gt;사용하기</a>
        1. <a href="#xsd-schemas-beans">Beans 스키마</a>
    1. <a href="#xml-custom">XML 스키마 authoring</a>
        1. <a href="#xsd-custom-schema">authoring 스키마</a>
        1. <a href="#xsd-custom-namespacehandler">NamespceHandler 코딩하기</a>
        1. <a href="#xsd-custom-parser">BeanDefinitionParser 사용하기</a>
        1. <a href="#xsd-custom-registration">Handler와 스키마 등록하기</a>
            * <a href="#xsd-custom-registration-spring-handlers">META-INF/spring.handlers 작성하기</a>
            * <a href="#xsd-custom-registration-spring-schemas">META-INF/spring.schemas 작성하기</a>
        1. <a href="#xsd-custom-using">스프링 XML 설정에서 커스텀 확장기능 이용하기</a>
        1. <a href="#xsd-custom-meat">더 자세한 예시</a>
            * <a href="#xsd-custom-custom-nested">커스텀 요소 안에 커스텀 요소 넣기</a>
            * <a href="#xsd-custom-custom-just-attributes">"일반" 요소에 커스텀 특성</a>

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




<h2 id="resources"> 리소스 </h2>

<h3 id="resources-introduction">소개</h3>
<h3 id="resources-resource">리소스 인터페이스</h3>
<h3 id="resources-implementations">이미 만들어진 리소스 구현체</h3>

<h4 id="resources-implementations-urlresource">UrlResource</h4>
<h4 id="resources-implementations-classpathresource">ClassPathResource</h4>
<h4 id="resources-implementations-filesystemresource">FileSystemResource</h4>
<h4 id="resources-implementations-servletcontextresource">ServletContextResource</h4>
<h4 id="resources-implementations-inputstreamresource">InputStreamResource</h4>
<h4 id="resources-implementations-bytearrayresource">ByteArrayResource</h4>


<h3 id="resources-resourceloader">ResourceLoader</h3>
<h3 id="resources-resourceloaderaware">ResourceLoaderAware 인터페이스</h3>
<h3 id="resources-as-dependencies">의존성으로서의 리소스</h3>
<h3 id="resources-app-ctx">어플리케이션 콘텍스트와 리소스 패스</h3>

<h4 id="resources-app-ctx-construction">어플리케이션 콘텍스트 생성하기</h4>

<h5 id="resources-app-ctx-classpathxml">ClassPathXmlApplicationContext 인스턴스 생성하기 - 지름길</h5>


<h4 id="resources-app-ctx-wildcards-in-resource-paths">어플리케이션 콘텍스트 생성자 리소스 패스에 와일드카드</h4>

<h5 id="resources-app-ctx-ant-patterns-in-paths">Ant 스타일 패턴</h5>
<h5 id="resources-classpath-wildcards">classpath1.: 접두사</h5>
<h5 id="resources-wildcards-in-path-other-stuff">와일드카드와 관련된 추가적인 사항</h5>


<h4 id="resources-filesystemresource-caveats">FileSystemResource 경고</h4>




<h2 id="validation"> 검증, 데이터 바인딩, 타입 변환 </h2>

<h3 id="validator">스프링 Validator 인터페이스를 통한 검증</h3>
<h3 id="validation-conversion">코드에 에러메세지 적용하기</h3>
<h3 id="beans-beans">빈 조작과 BeanWrapper</h3>

<h4 id="beans-beans-conventions">기본적인 프로퍼티, 중첩된 프로퍼티 설정하고 가져오기</h4>
<h4 id="beans-beans-conversion">이미 구현된 PropertyEditor</h4>

<h5 id="beans-beans-conversion-customeditor-registration">커스텀 PropertyEditor 구현체 등록하기</h5>




<h3 id="core-convert">스프링 타입 변환</h3>

<h4 id="core-convert-Converter-API">변환기 서비스 제공자 인터페이스</h4>
<h4 id="core-convert-ConverterFactory-SPI">ConverterFactory 사용하기</h4>
<h4 id="core-convert-GenericConverter-SPI">GenericConverter 사용하기</h4>

<h5 id="core-convert-ConditionalGenericConverter-SPI">ConditionalGenericConverter 사용하기</h5>


<h4 id="core-convert-ConversionService-API">ConversionService API</h4>
<h4 id="core-convert-Spring-config">ConversionService 설정하기</h4>
<h4 id="core-convert-programmatic-usage">ConversionService 프로그래밍 적으로 사용하기</h4>


<h3 id="format">스프링 필드 형식 설정</h3>

<h4 id="format-Formatter-SPI">Formatter 서비스 제공자 인터페이스</h4>
<h4 id="format-CustomFormatAnnotations">어노테이션 기반 형식설정</h4>

<h5 id="format-annotations-api">형식 어노테이션 API</h5>


<h4 id="format-FormatterRegistry-SPI">FormatterRegistry 서비스 제공자 인터페이스</h4>
<h4 id="format-FormatterRegistrar-SPI">FormatterRegistrar 서비스 제공자 인터페이스</h4>
<h4 id="format-configuring-formatting-mvc">스프링 MVC에서 형식 설정하기</h4>


<h3 id="format-configuring-formatting-globaldatetimeformat">글로벌 Data, Time 형식 설정</h3>
<h3 id="validation-beanvalidation">빈 검증</h3>

<h4 id="validation-beanvalidation-overview">빈 검증 개요</h4>
<h4 id="validation-beanvalidation-spring">빈 검증 제공자 설정하기</h4>

<h5 id="validation-beanvalidation-spring-inject">검증자 주입하기</h5>
<h5 id="validation-beanvalidation-spring-constraints">커스텀 제한사항 설정하기</h5>
<h5 id="validation-beanvalidation-spring-method">스프링 기반 메서드 검증</h5>
<h5 id="validation-beanvalidation-spring-other">추가적인 설정 옵션</h5>


<h4 id="validation-binder">DataBinder 설정하기</h4>
<h4 id="validation-mvc">스프링 MVC 3 검증</h4>




<h2 id="expressions"> 스프링 표현 언어(SpEL) </h2>

<h3 id="expressions-evaluation">값 구하기</h3>

<h4 id="expressions-evaluation-context">EvaluationContext 이해하기</h4>

<h5 id="expressions-type-conversion">형식 변환</h5>


<h4 id="expressions-parser-configuration">파서 설정하기</h4>
<h4 id="expressions-spel-compilation">스프링 표현 언어 편집</h4>

<h5 id="expressions-compiler-configuration">컴파일러 설정하기</h5>
<h5 id="expressions-compiler-limitations">컴파일러 제한사항</h5>




<h3 id="expressions-beandef">빈 정의 안의 표현</h3>

<h4 id="expressions-beandef-xml-based">XML 설정</h4>
<h4 id="expressions-beandef-annotation-based">어노테이션 설정</h4>


<h3 id="expressions-langauge-ref">언어 레퍼런스</h3>

<h4 id="expressions-ref-literal">문자 표현</h4>
<h4 id="expressions-properties-arrays">프로퍼티, 배열, 리스트, 맵, 색인작성자</h4>
<h4 id="expressions-inline-lists">인라인 리스트</h4>
<h4 id="expressions-inline-maps">인라인 맵</h4>
<h4 id="expressions-array-construction">배열 생성하기</h4>
<h4 id="expressions-methods">메서드</h4>
<h4 id="expressions-operators">연산자</h4>

<h5 id="expressions-operators-relational">관계 연산자</h5>
<h5 id="expressions-operators-logical">논리 연산자</h5>
<h5 id="expressions-operators-mathematical">산술 연산자</h5>
<h5 id="expressions-assignment">대입 연산자</h5>


<h4 id="expressions-types">타입</h4>
<h4 id="expressions-constructors">생성자</h4>
<h4 id="expressions-ref-variables">변수</h4>

<h5 id="expressions-this-root">#this 와 #root 변수</h5>


<h4 id="expressions-ref-functions">함수</h4>
<h4 id="expressions-bean-references">빈 참조</h4>
<h4 id="expressions-operator-ternary">삼항 연산자 (If-Then-Else)</h4>
<h4 id="expressions-operator-elvis">엘비스 연산자</h4>
<h4 id="expressions-operator-navigation">Safe Navigation 연산자</h4>
<h4 id="expressions-collection-selection">컬렉션 선택</h4>
<h4 id="expressions-collection-projection">컬렉션 투영</h4>
<h4 id="expressions-templating">표현 템플릿화</h4>


<h3 id="expressions-example-classes">예시에서 사용된 클래스</h3>


<h2 id="aop"> 스프링과 함께하는 관점 지향 프로그래밍(AOP) </h2>

<h3 id="aop-introduction-defn">AOP 개념</h3>
<h3 id="aop-introduction-spring-defn">스프링 AOP 기능과 목표</h3>
<h3 id="aop-introduction-proxies">AOP Proxies</h3>
<h3 id="aop-ataspectj">@AspectJ 지원</h3>

<h4 id="aop-aspectj-support">@AspectJ 지원 활성화하기</h4>
<h4 id="aop-at-aspectj">애스팩트 선언하기</h4>
<h4 id="aop-pointcuts">포인트컷 선언하기</h4>

<h5 id="aop-pointcuts-designators">지원되는 포인트컷 지명자</h5>
<h5 id="aop-pointcuts-combining">포인트컷 표현 결합하기</h5>
<h5 id="aop-common-pointcuts">공통의 포인트컷 정의 공유하기</h5>
<h5 id="aop-pointcuts-examples">예시</h5>
<h5 id="writing-good-pointcuts">좋은 포인트컷 작성하기</h5>


<h4 id="aop-advice">어드바이스 선언하기</h4>

<h5 id="aop-advice-before">Before 어드바이스</h5>
<h5 id="aop-advice-after-returning">AfterReturning 어드바이스</h5>
<h5 id="aop-advice-after-throwing">AfterThrowing 어드바이스</h5>
<h5 id="aop-advice-after-finally">After(Finally) 어드바이스</h5>
<h5 id="aop-advice-around-advice">Around 어드바이스</h5>
<h5 id="aop-advice-advice-params">어드바이스 파라메터</h5>
<h5 id="aop-advice-advice-ordering">어드바이스 순서 정하기</h5>


<h4 id="aop-introduction">소개</h4>
<h4 id="aop-instantiation-models">어스펙트 인스턴스화 모델</h4>
<h4 id="aop-ataspectj-example">AOP 예시</h4>


<h3 id="aop-schema">스키마 기반 AOP</h3>

<h4 id="aop-schema-declaring-an-aspect">애스팩트 선언하기</h4>
<h4 id="aop-schema-pointcuts">포인트컷 선언하기</h4>
<h4 id="aop-schema-advice">어드바이스 선언하기</h4>

<h5 id="aop-schema-advice-before">Before 어드바이스</h5>
<h5 id="aop-schema-advice-after-returning">AfterReturning 어드바이스</h5>
<h5 id="aop-schema-advice-after-throwing">AfterThrowing 어드바이스</h5>
<h5 id="aop-schema-advice-after-finally">After(Finally) 어드바이스</h5>
<h5 id="aop-schema-advice-around-advice">Around 어드바이스</h5>
<h5 id="aop-schema-advice-advice-params">어드바이스 파라메터</h5>
<h5 id="aop-schema-advice-advice-ordering">어드바이스 순서 정하기</h5>


<h4 id="aop-schema-introductions">소개</h4>
<h4 id="aop-schema-instantiation-models">애스팩트 인스턴스화 모델</h4>
<h4 id="aop-schema-advisors">어드바이저</h4>
<h4 id="aop-schema-example">AOP 스키마 예시</h4>


<h3 id="aop-choosing">사용할 AOP 정의 스타일 선택하기</h3>

<h4 id="aop-spring-or-aspectj">스프링 AOP? 혹은 완전한 AspectJ?</h4>
<h4 id="aop-ataspectj-or-xml">스프링 AOP에서 @AspectJ? 혹은 XML?</h5>


<h3 id="aop-mixing-styles">Aspect 타입 섞기</h3>
<h3 id="aop-proxying">프록시 작동원리</h3>

<h4 id="aop-understanding-aop-proxies">AOP 프록시 이해하기</h4>


<h3 id="aop-aspectj-programmatic">프로그래밍적으로 @AspectJ 프록시 만들기</h3>
<h3 id="aop-using-aspectj">스프링 어플리케이션에서 AspectJ 사용하기</h3>

<h4 id="aop-atconfigurable">스프링에서 도메인객체에 AspectJ를 이용하여 의존성 주입하기</h4>

<h5 id="aop-configurable-testing">@Configurable 객체 단위 테스트</h5>
<h5 id="aop-configurable-container">여러 개의 어플리케이션 컨텍스트 이용하기</h5>


<h4 id="aop-ajlib-other">AspectJ를 위한 다른 스프링 애스팩트</h4>
<h4 id="aop-aj-configure">스프링 IoC를 사용하여 AspectJ 애스팩트 설정하기</h4>
<h4 id="aop-aj-ltw">스프링 프레임워크에서 AspectJ 로드 타임 위빙</h4>

<h5 id="aop-aj-ltw-first-example">첫 예시</h5>
<h5 id="aop-aj-ltw-the-aspects">애스팩트</h5>
<h5 id="aop-aj-ltw-aop_dot_xml">'META-INF/aop.xml'</h5>
<h5 id="aop-aj-ltw-libraries">필요 라이브러리(JARS)</h5>
<h5 id="aop-aj-ltw-spring">스프링 설정</h5>
<h5 id="aop-aj-ltw-environments">고유 환경 설정</h5>




<h3 id="aop-resources">추가 자료</h3>


<h2 id="aop-api"> 스프링 AOP API </h2>

<h3 id="aop-api-pointcuts">스프링에서 포인트컷</h3>

<h4 id="aop-api-concepts">개념</h4>
<h4 id="aop-api-pointcut-ops">포인트컷에서 작동</h4>
<h4 id="aop-api-pointcut-aspectj">AspectJ 포인트컷 표현식</h4>
<h4 id="aop-api-pointcuts-impls">편리한 PointCut 구현</h4>
<h4 id="aop-api-superclasses">포인트컷 부모클래스</h4>
<h4 id="aop-api-pointcuts-custom">커스텀 포인트컷</h4>


<h3 id="aop-api-advice">스프링에서의 어드바이스</h3>

<h4 id="aop-api-advice-lifecycle">어드바이스 생명주기</h4>
<h4 id="aop-api-advice-types">스프링에서 어드바이스 유형</h4>

<h5 id="aop-api-advice-around">Interception Around 어드바이스</h5>
<h5 id="aop-api-advice-before">Before 어드바이스</h5>
<h5 id="aop-api-advice-throws">Throws 어드바이스</h5>
<h5 id="aop-api-advice-after-returning">After Returning 어드바이스</h5>
<h5 id="aop-api-advice-introduction">Introduction 어드바이스</h5>




<h3 id="aop-api-advisor">스프링에서의 어드바이저</h3>
<h3 id="aop-pfb">ProxyFactoryBean을 이용하여 AOP 프록시 만들기</h3>

<h4 id="aop-pfb-1">기초</h4>
<h4 id="aop-pfb-2">자바빈 프로퍼티</h4>
<h4 id="aop-pfb-proxy-types">JDK 기반, CGLIB 기반 프로퍼티</h4>
<h4 id="aop-api-proxying=intf">프록시 인터페이스</h4>
<h4 id="aop-api-proxying-class">프록시 클래스</h4>
<h4 id="aop-global-advisors">"글로벌" 어드바이스 사용하기</h4>


<h3 id="aop-concise-proxy">간결한 프록시 정의</h3>
<h3 id="aop-prog">ProxyFactory를 이용하여 AOP 프록시 프로그래밍적으로 만들기</h3>
<h3 id="aop-api-advised">Advised 객체 조작하기</h3>
<h3 id="aop-autoproxy">"자동-프록시" 기능 이용하기</h3>

<h4 id="aop-autoproxy-choices">자동-프록시 빈 정의</h4>

<h5 id="aop-api-autoproxy">BeanNameAutoProxyCreator</h5>
<h5 id="aop-api-autoproxy-default">DefaultAdvisorAutoProxyCreator</h5>




<h3 id="aop-targetsource">TargetSource 구현 이용하기</h3>

<h4 id="aop-ts-swap">동적 전환가능한 Target Sources</h4>
<h4 id="aop-ts-pool">Target Source 풀</h4>
<h4 id="aop-ts-prototype">프로토타입 Target Sources</h4>
<h4 id="aop-ts-thredlocal">ThreadLocal Target Sources</h4>


<h3 id="aop-extensibility">새로운 어드바이스 타입 정의하기</h3>


<h2 id="null-safety"> 안전한 Null </h2>

<h3 id="use-cases">사용 예시</h3>
<h3 id="jsr-305-meta-annotations">JSR-305 메타 어노테이션</h3>


<h2 id="databuffers"> 데이터 버퍼와 코덱 </h2>

<h3 id="databuffers-factory">DataBufferFactory</h3>
<h3 id="databuffers-buffer">DataBuffer</h3>
<h3 id="databuffers-buffer-pooled">PooledDataBuffer</h3>
<h3 id="databuffers-utils">DataBufferUtils</h3>
<h3 id="codecs">Codecs</h3>
<h3 id="databuffers-using">DataBuffer 사용하기</h3>


<h2 id="appendix"> 부록 </h2>

<h3 id="xsd-schemas">XML 스키마</h3>

<h4 id="xsd-schemas-util">util 스키마</h4>

<h5 id="xsd-schemas-util-constant">&lt;util:constant/&gt;사용하기</h5>
<h5 id="xsd-schemas-util-property-path">&lt;util:property-path/&gt;사용하기</h5>
<h5 id="xsd-schemas-util-properties">&lt;util:properties/&gt;사용하기</h5>
<h5 id="xsd-schemas-util-list">&lt;util:list/&gt;사용하기</h5>
<h5 id="xsd-schemas-util-map">&lt;util:map/&gt;사용하기</h5>
<h5 id="xsd-schemas-util-set">&lt;util:set/&gt;사용하기</h5>


<h4 id="xsd-schemas-aop">aop 스키마</h4>
<h4 id="xsd-schemas-context">context 스키마</h4>

<h5 id="xsd-schemas-context-pphc">&lt;property-placeholder/&gt;사용하기</h5>
<h5 id="xsd-schemas-context-ac">&lt;annotation-config/&gt;사용하기</h5>
<h5 id="xsd-schemas-context-component-scan">&lt;component-scan/&gt;사용하기</h5>
<h5 id="xsd-schemas-context-ltw">&lt;load-time-weaver/&gt;사용하기</h5>
<h5 id="xsd-schemas-context-sc">&lt;spring-configured/&gt;사용하기</h5>
<h5 id="xsd-schemas-context-mbe">&lt;mbean-export/&gt;사용하기</h5>


<h4 id="xsd-schemas-beans">Beans 스키마</h4>


<h3 id="xml-custom">XML 스키마 authoring</h3>

<h4 id="xsd-custom-schema">authoring 스키마</h4>
<h4 id="xsd-custom-namespacehandler">NamespceHandler 코딩하기</h4>
<h4 id="xsd-custom-parser">BeanDefinitionParser 사용하기</h4>
<h4 id="xsd-custom-registration">Handler와 스키마 등록하기</h4>

<h5 id="xsd-custom-registration-spring-handlers">META-INF/spring.handlers 작성하기</h5>
<h5 id="xsd-custom-registration-spring-schemas">META-INF/spring.schemas 작성하기</h5>


<h4 id="xsd-custom-using">스프링 XML 설정에서 커스텀 확장기능 이용하기</h4>
<h4 id="xsd-custom-meat">더 자세한 예시</h4>

<h5 id="xsd-custom-custom-nested">커스텀 요소 안에 커스텀 요소 넣기</h5>
<h5 id="xsd-custom-custom-just-attributes">"일반" 요소에 커스텀 특성</h5>
