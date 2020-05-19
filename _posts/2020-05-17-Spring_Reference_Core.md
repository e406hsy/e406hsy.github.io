---
layout: post
title:  "[Spring Reference] 스프링 레퍼런스 #1 핵심"
createdDate:   2020-05-17T18:42:00+09:00
date:   2020-05-17T18:42:00+09:00
pagination: enabled
author: SoonYong Hong
categories: Spring_Reference
tags: Spring Reference
---

이 내용은 [스프링 문서 5.2.6.RELEASE](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#spring-core)를 번역한 내용으로 오역이 있을 수 있습니다.

현재 내용 작성중입니다.

# 목차
<h2><a href="#spring-core"> 핵심 기술 </a></h2>
<ol>
    <li><a href="#beans"> IoC 컨테이너 </a>
        <ol>
            <li><a href="#beans-introduction">IoC 컨테이너와 빈의 소개</a></li>
            <li><a href="#beans-basics">컨테이너 개요</a>
                <ol>
                    <li><a href="#beans-factory-metadata">메타데이터 설정하기</a></li>
                    <li><a href="#beans-factory-instantiation">컨테이너 인스턴스 만들기</a>
                        <ul>
                            <li><a href="#beans-factory-xml-import">XML 기반 설정 메타데이터 만들기</a></li>
                            <li><a href="#groovy-bean-definition-dsl">Groovy 빈 정의 DSL</a></li>
                        </ul>
                    </li>
                    <li><a href="#bean-factory-client">컨테이너 이용하기</a></li>
                </ol>
            </li>
            <li><a href="#beans-definition">빈 개요</a>
                <ol>
                    <li><a href="#beans-beanname">빈 이름 붙이기</a>
                        <ul>
                            <li><a href="#beans-beanname-alias">빈 정의 외부에서 빈 별명 설정하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#beans-factory-class">빈 인스턴스 만들기</a>
                        <ul>
                            <li><a href="#beans-factory-class-ctor">컨테이너로 인스턴스 만들기</a></li>
                            <li><a href="#beans-factory-class-static-factory-method">스태틱 팩토리 메소드로 만들기</a></li>
                            <li><a href="#beans-factory-class-instance-factory-method">인스턴스 팩토리 메소드로 만들기</a></li>
                            <li><a href="#beans-factory-type-determination">빈의 런타임 타입 결정하기</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#beans-dependencies">의존성</a>
                <ol>
                    <li><a href="#beans-factory-collaborators">의존성 주입</a>
                        <ul>
                            <li><a href="#beans-constructor-injection">생성자 기반 의존성 주입</a></li>
                            <li><a href="#beans-setter-injection">세터 기반 의존성 주입</a></li>
                            <li><a href="#beans-dependency-resolution">의존성 결정 과정</a></li>
                            <li><a href="#beans-some-examples">의존성 주입 예제</a></li>
                        </ul>
                    </li>
                    <li><a href="#beans-factory-properties-detailed">의존성과 설정 상세</a>
                        <ul>
                            <li><a href="#beans-value-element">변경없는 값(Primitives, String 등등)</a></li>
                            <li><a href="#beans-ref-element">다른 빈 참조(협업)</a></li>
                            <li><a href="#beans-inner-beans">내부 빈</a></li>
                            <li><a href="#beans-collection-elements">컬렉션</a></li>
                            <li><a href="#beans-null-element">Null과 비어있는 스트링</a></li>
                            <li><a href="#beans-p-namespace">P 네임스페이스를 이용한 XML 단축</a></li>
                            <li><a href="#beans-c-namespace">c 네임스페이스를 이용한 XML 단축</a></li>
                            <li><a href="#beans-compound-property-names">복합 프로퍼티 이름</a></li>
                        </ul>
                    </li>
                    <li><a href="#beans-factory-dependson">depends-on 사용하기</a></li>
                    <li><a href="#beans-factory-lazy-init">지연 초기화 빈</a></li>
                    <li><a href="#beans-factory-autowire">Autowiring 협력자</a>
                        <ul>
                            <li><a href="#beans-autowired-exceptions">자동 연결의 한계와 단점</a></li>
                            <li><a href="#beans-factory-autowire-candidate">자동 연결에 빈 제외하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#beans-factory-method-injection">메소드 주입</a>
                        <ul>
                            <li><a href="#beans-factory-lookup-method-injection">Lookup 메소드 주입</a></li>
                            <li><a href="#beans-factory-arbitrary-method-replacement">임의의 메소드 대체</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#beans-factory-scopes">빈 스코프</a>
                <ol>
                    <li><a href="#beans-factory-scopes-singleton">싱글톤 스코프</a></li>
                    <li><a href="#beans-factory-scopes-prototype">프로토타입 스코프</a></li>
                    <li><a href="#beans-factory-scopes-sing-prot-interaction">프로토타입 빈 의존성을 가지는 싱글톤 빈</a></li>
                    <li><a href="#beans-factory-scopes-other">Request, Session, Application, WebSocket 스코프</a>
                        <ul>
                            <li><a href="#beans-factory-scopes-other-web-configuration">초기 웹 설정</a></li>
                            <li><a href="#beans-factory-scopes-request">Request 스코프</a></li>
                            <li><a href="#beans-factory-scopes-session">Session 스코프</a></li>
                            <li><a href="#beans-factory-scopes-application">Application 스코프</a></li>
                            <li><a href="#beans-factory-scopes-other-injection">스코프 빈 의존성</a></li>
                        </ul>
                    </li>
                    <li><a href="#beans-factory-scopes-custom">커스텀 스코프</a>
                        <ul>
                            <li><a href="#beans-factory-scopes-custom-creating">커스텀 스코프 만들기</a></li>
                            <li><a href="#beans-factory-scopes-custom-using">커스텀 스코프 이용하기</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#beans-factory-nature">빈의 특성 설정하기</a>
                <ol>
                    <li><a href="#beans-factory-lifecycle">콜백의 생명주기</a>
                        <ul>
                            <li><a href="#beans-factory-lifecycle-initializingbean">콜백 정의하기</a></li>
                            <li><a href="#beans-factory-lifecycle-disposablebean">파괴 콜백</a></li>
                            <li><a href="#beans-factory-lifecycle-default-init-destroy-methods">기본 초기화 메소드, 파괴 메소드</a></li>
                            <li><a href="#beans-factory-lifecycle-combined-effects">생명주기 작동원리 조합하기</a></li>
                            <li><a href="#beans-factory-lifecycle-processor">Startup, Shutdown 콜백</a></li>
                            <li><a href="#beans-factory-shutdown">웹 어플리케이션이 아닌 환경에서 스프링 IoC 컨테이너 아름답게 종료하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#beans-factory-aware">ApplicationContextAware, BeanNameAware</a></li>
                    <li><a href="#awar-list">다른 Aware 인터페이스</a></li>
                </ol>
            </li>
            <li><a href="#beans-child-bean-definitions">빈의 정의 상속</a></li>
            <li><a href="#beans-factory-extension">컨테이너 확장 시점</a>
                <ol>
                    <li><a href="#beans-factory-extension-bpp">BeanPostProcessor를 이용하여 빈 커스터마이징 하기</a>
                        <ul>
                            <li><a href="#beans-factory-extension-bpp-examples-hw">예제: Hello World, BeanPostProcessor 스타일</a></li>
                            <li><a href="#beans-factory-extension-bpp-examples-rabpp">예제: RequiredAnnotationBeanPostProcessor</a></li>
                        </ul>
                    </li>
                    <li><a href="#beans-factory-extension-factory-postprocessors">BeanFactoryPostProcessor를 이용하여 설정 메타데이터 설정하기</a>
                        <ul>
                            <li><a href="#beans-factory-placeholderconfigurer">예제: 클래스 이름 대체 PropertySourcesPlaceholderConfigurer</a></li>
                            <li><a href="#beans-factory-overrideconfigurer">예제: PropertyOverrideConfigurer</a></li>
                        </ul>
                    </li>
                    <li><a href="#beans-factory-extension-factorybean">FactoryBean을 이용하여 인스턴스화 로직 커스터마이징 하기</a></li>
                </ol>
            </li>
            <li><a href="#beans-annotation-config">어노테이션 기반 컨테이너 설정</a>
                <ol>
                    <li><a href="#beans-required-annotation">@Required</a></li>
                    <li><a href="#beans-autowired-annotation">@Autowired 사용하기</a></li>
                    <li><a href="#beans-autowired-annotation-primary">@Primary를 이용하여 어노테이션 기반 자동 연결 미세 조정하기</a></li>
                    <li><a href="#beans-autowired-annotation-qualifiers"></a>Qualifers를 이용하여 어노테이션 기반 자동 연결  미세 조정하기</li>
                    <li><a href="#beans-generics-as-qualifiers">제네릭을 자동연결 Qualifers로 이용하기</a></li>
                    <li><a href="#beans-custom-autowire-configurer">CustomAutowireConfig 이용하기</a></li>
                    <li><a href="#beans-resource-annotation">@Resource를 사용한 주입</a></li>
                    <li><a href="#beans-value-annotations">@Value 사용하기</a></li>
                    <li><a href="#beans-postconstruct-and-predestroy-annotations">@PostConstruct와 @PreDestory 사용하기</a></li>
                </ol>
            </li>
            <li><a href="#beans-classpath-scanning">클래스패스 스캐닝과 관리되는 컴포넌트</a>
                <ol>
                    <li><a href="#beans-stereotype-annotations">@Component와 추가적인 정형화된 어노테이션</a></li>
                    <li><a href="#beans-meta-annotations">메타 어노테이션과 복합 어노테이션 이용하기</a></li>
                    <li><a href="#beans-scanning-autodetection">클래스 검색과 빈 정의 등록을 자동으로 수행하기 </a></li>
                    <li><a href="#beans-scanning-filters">필터를 이용하여 스캐닝 커스터마이징하기</a></li>
                    <li><a href="#beans-factorybeans-annotations">컴포넌트 내부에 빈 메타데이터 정의하기</a></li>
                    <li><a href="#beans-scanning-name-generator">자동 검색된 컴포넌트 이름 붙이기</a></li>
                    <li><a href="#beans-scanning-scope-resolver">자동 검색된 컴포넌트에 스코프 부여하기</a></li>
                    <li><a href="#beans-scanning-qualifiers">Qualifier 메타데이터 어노테이션으로 부여하기</a></li>
                    <li><a href="#beans-scanning-index">후보 컴포넌트 색인 생성하기</a></li>
                </ol>
            </li>
            <li><a href="#beans-standard-annotations">JSR 330 표준 어노테이션 사용하기</a>
                <ol>
                    <li><a href="#beans-inject-named">@Inject와 @Named를 이용한 의존성 주입</a></li>
                    <li><a href="#beans-named">@Named와 @ManagedBean: @Component 어노테이션의 표준 표현</a></li>
                    <li><a href="#beans-stardard-annotations-limitations">JSR-330 표준 어노테이션의 한계</a></li>
                </ol>
            </li>
            <li><a href="#beans-java">자바 기반 컨테이너설정</a>
                <ol>
                    <li><a href="#beans-java-basic-concepts">기본 개념: @Bean과 @Configuration</a></li>
                    <li><a href="#beans-java-instantiating-container">AnnotationConfigApplicationContext를 이용하여 스프링 컨테이너 인스턴스화 하기</a>
                        <ul>
                            <li><a href="#beans-java-instantiating-container-constructor">간단하게 만들어보기</a></li>
                            <li><a href="#beans-java-instantiating-container-register">register(Class<?>...)을 이용하여 프로그래밍적으로 컨테이너 만들기</a></li>
                            <li><a href="#beans-java-instantiating-container-scan">scan(String...)을 이용하여 컴포넌트 스캔 활성화하기</a></li>
                            <li><a href="#beans-java-instantiating-container-web">AnnotationConfigWebApplicationContext을 이용한 웹 어플리케이션 지원</a></li>
                        </ul>
                    </li>
                    <li><a href="#beans-java-bean-annotation">@Bean 어노테이션 사용하기</a>
                        <ul>
                            <li><a href="#beans-java-declaring-a-bean">빈 정의하기</a></li>
                            <li><a href="#beans-java-dependencies">빈 의존성</a></li>
                            <li><a href="#beans-java-lifecycle-callbacks">생명주기 콜백 받기</a></li>
                            <li><a href="#beans-java-specifying-bean-scope">빈 스코프 설정하기</a></li>
                            <li><a href="#beans-java-customizing-bean-naming">빈 이름 설정하기</a></li>
                            <li><a href="#beans-java-bean-aliasing">빈 별명 설정하기</a></li>
                            <li><a href="#beans-java-bean-description">빈 설명</a></li>
                        </ul>
                    </li>
                    <li><a href="#beans-java-configuration-annotation">@Configuration 어노테이션 이용하기</a>
                        <ul>
                            <li><a href="#beans-java-injecting-dependencies">내부 빈 의존성 주입하기</a></li>
                            <li><a href="#beans-java-method-injection">Lookup 메소드 주입하기</a></li>
                            <li><a href="#beans-java-further-information-java-config">자바 기반 설정이 내부적으로 작동하는 방법에 대한 추가적인 정보</a></li>
                        </ul>
                    </li>
                    <li><a href="#beans-java-composing-configuration-classes">자바 기반 설정 구성하기</a>
                        <ul>
                            <li><a href="#beans-java-using-import">@Import 어노테이션 사용하기</a></li>
                            <li><a href="#beans-java-conditional">조건에 따라 @Configuration 클래스와 @Bean 메소드 포함시키기</a></li>
                            <li><a href="#beans-java-combining">자바 기반 설정과 XML 기반 설정 조합하기</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#beans-envirionment">환경 추상화</a>
                <ol>
                    <li><a href="#beans-definition-profiles">빈 정의 프로필</a>
                        <ul>
                            <li><a href="#beans-definition-profiles-java">@Profile 이용하기</a></li>
                            <li><a href="#beans-definition-profiles-xml">XML 빈 정의 프로필</a></li>
                            <li><a href="#beans-definition-profiles-enable">프로필 활성화하기</a></li>
                            <li><a href="#beans-definition-profiles-default">기본 프로필</a></li>
                        </ul>
                    </li>
                    <li><a href="#beans-property-source-abstaction">PropertySource 추상화</a></li>
                    <li><a href="#beans-using-propertysource">@PropertySource 사용하기</a></li>
                    <li><a href="#beans-placeholder-resolution-in-statements">표현법의 PlaceHolder Resolution</a></li>
                </ol>
            </li>
            <li><a href="#context-load-time-weaver">LoadTimeWeaver 등록하기</a></li>
            <li><a href="#context-introduction">어플리케이션 콘텍스트의 추가적인 사용기능</a>
                <ol>
                    <li><a href="#context-functionality-messagesource">MessageSource를 이용한 내재화</a></li>
                    <li><a href="#context-functionality-events">표준, 커스텀 이벤트</a>
                        <ul>
                            <li><a href="#context-functionality-events-annotation">어노테이션 기반 이벤트 리스너</a></li>
                            <li><a href="#context-functionality-events-async">비동기 리스너</a></li>
                            <li><a href="#context-functionality-events-order">리스너 순서 정하기</a></li>
                            <li><a href="#context-functionality-events-generics">Generic 이벤트</a></li>
                        </ul>
                    </li>
                    <li><a href="#context-functionality-resources">로우레벨 자원에 편리한 접근</a></li>
                    <li><a href="#context-create">웹 어플리케이션을 위한 편리한 ApplicationContext 인스턴스화</a></li>
                    <li><a href="#context-deploy-rar">스프링 ApplicationContext를 Java EE RAR 파일로 배포하기</a></li>
                </ol>
            </li>
            <li><a href="#beans-beanfactory">빈 팩토리</a>
                <ol>
                    <li><a href="#context-introduction-ctx-vs-beanfactory">BeanFactory? 혹은 ApplicationContext?</a></li>
                </ol>
            </li>
        </ol>
    </li>
    <li><a href="#resources"> 리소스 </a>
        <ol>
            <li><a href="#resources-introduction">소개</a></li>
            <li><a href="#resources-resource">리소스 인터페이스</a></li>
            <li><a href="#resources-implementations">이미 만들어진 리소스 구현체</a>
                <ol>
                    <li><a href="#resources-implementations-urlresource">UrlResource</a></li>
                    <li><a href="#resources-implementations-classpathresource">ClassPathResource</a></li>
                    <li><a href="#resources-implementations-filesystemresource">FileSystemResource</a></li>
                    <li><a href="#resources-implementations-servletcontextresource">ServletContextResource</a></li>
                    <li><a href="#resources-implementations-inputstreamresource">InputStreamResource</a></li>
                    <li><a href="#resources-implementations-bytearrayresource">ByteArrayResource</a></li>
                </ol>
            </li>
            <li><a href="#resources-resourceloader">ResourceLoader</a></li>
            <li><a href="#resources-resourceloaderaware">ResourceLoaderAware 인터페이스</a></li>
            <li><a href="#resources-as-dependencies">의존성으로서의 리소스</a></li>
            <li><a href="#resources-app-ctx">어플리케이션 콘텍스트와 리소스 패스</a>
                <ol>
                    <li><a href="#resources-app-ctx-construction">어플리케이션 콘텍스트 생성하기</a>
                        <ul>
                            <li><a href="#resources-app-ctx-classpathxml">ClassPathXmlApplicationContext 인스턴스 생성하기 - 지름길</a></li>
                        </ul>
                    </li>
                    <li><a href="#resources-app-ctx-wildcards-in-resource-paths">어플리케이션 콘텍스트 생성자 리소스 패스에 와일드카드</a>
                        <ul>
                            <li><a href="#resources-app-ctx-ant-patterns-in-paths">Ant 스타일 패턴</a></li>
                            <li><a href="#resources-classpath-wildcards">classpath*: 접두사</a></li>
                            <li><a href="#resources-wildcards-in-path-other-stuff">와일드카드와 관련된 추가적인 사항</a></li>
                        </ul>
                    </li>
                    <li><a href="#resources-filesystemresource-caveats">FileSystemResource 경고</a></li>
                </ol>
            </li>
        </ol>
    </li>
    <li><a href="#validation"> 검증, 데이터 바인딩, 타입 변환 </a>
        <ol>
            <li><a href="#validator">스프링 Validator 인터페이스를 통한 검증</a></li>
            <li><a href="#validation-conversion">코드에 에러메세지 적용하기</a></li>
            <li><a href="#beans-beans">빈 조작과 BeanWrapper</a>
                <ol>
                    <li><a href="#beans-beans-conventions">기본적인 프로퍼티, 중첩된 프로퍼티 설정하고 가져오기</a></li>
                    <li><a href="#beans-beans-conversion">이미 구현된 PropertyEditor</a>
                        <ul>
                            <li><a href="#beans-beans-conversion-customeditor-registration">커스텀 PropertyEditor 구현체 등록하기</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#core-convert">스프링 타입 변환</a>
                <ol>
                    <li><a href="#core-convert-Converter-API">변환기 서비스 제공자 인터페이스</a></li>
                    <li><a href="#core-convert-ConverterFactory-SPI">ConverterFactory 사용하기</a></li>
                    <li><a href="#core-convert-GenericConverter-SPI">GenericConverter 사용하기</a>
                        <ul>
                            <li><a href="#core-convert-ConditionalGenericConverter-SPI">ConditionalGenericConverter 사용하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#core-convert-ConversionService-API">ConversionService API</a></li>
                    <li><a href="#core-convert-Spring-config">ConversionService 설정하기</a></li>
                    <li><a href="#core-convert-programmatic-usage">ConversionService 프로그래밍 적으로 사용하기</a></li>
                </ol>
            </li>
            <li><a href="#format">스프링 필드 형식 설정</a>
                <ol>
                    <li><a href="#format-Formatter-SPI">Formatter 서비스 제공자 인터페이스</a></li>
                    <li><a href="#format-CustomFormatAnnotations">어노테이션 기반 형식설정</a>
                        <ul>
                            <li><a href="#format-annotations-api">형식 어노테이션 API</a></li>
                        </ul>
                    </li>
                    <li><a href="#format-FormatterRegistry-SPI">FormatterRegistry 서비스 제공자 인터페이스</a></li>
                    <li><a href="#format-FormatterRegistrar-SPI">FormatterRegistrar 서비스 제공자 인터페이스</a></li>
                    <li><a href="#format-configuring-formatting-mvc">스프링 MVC에서 형식 설정하기</a></li>
                </ol>
            </li>
            <li><a href="#format-configuring-formatting-globaldatetimeformat">글로벌 Data, Time 형식 설정</a></li>
            <li><a href="#validation-beanvalidation">빈 검증</a>
                <ol>
                    <li><a href="#validation-beanvalidation-overview">빈 검증 개요</a></li>
                    <li><a href="#validation-beanvalidation-spring">빈 검증 제공자 설정하기</a>
                        <ul>
                            <li><a href="#validation-beanvalidation-spring-inject">검증자 주입하기</a></li>
                            <li><a href="#validation-beanvalidation-spring-constraints">커스텀 제한사항 설정하기</a></li>
                            <li><a href="#validation-beanvalidation-spring-method">스프링 기반 메서드 검증</a></li>
                            <li><a href="#validation-beanvalidation-spring-other">추가적인 설정 옵션</a></li>
                        </ul>
                    </li>
                    <li><a href="#validation-binder">DataBinder 설정하기</a></li>
                    <li><a href="#validation-mvc">스프링 MVC 3 검증</a></li>
                </ol>
            </li>
        </ol>
    </li>
    <li><a href="#expressions"> 스프링 표현 언어(SpEL) </a>
        <ol>
            <li><a href="#expressions-evaluation">값 구하기</a>
                <ol>
                    <li><a href="#expressions-evaluation-context">EvaluationContext 이해하기</a>
                        <ul>
                            <li><a href="#expressions-type-conversion">형식 변환</a></li>
                        </ul>
                    </li>
                    <li><a href="#expressions-parser-configuration">파서 설정하기</a></li>
                    <li><a href="#expressions-spel-compilation">스프링 표현 언어 편집</a>
                        <ul>
                            <li><a href="#expressions-compiler-configuration">컴파일러 설정하기</a></li>
                            <li><a href="#expressions-compiler-limitations">컴파일러 제한사항</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#expressions-beandef">빈 정의 안의 표현</a>
                <ol>
                    <li><a href="#expressions-beandef-xml-based">XML 설정</a></li>
                    <li><a href="#expressions-beandef-annotation-based">어노테이션 설정</a></li>
                </ol>
            </li>
            <li><a href="#expressions-langauge-ref">언어 레퍼런스</a>
                <ol>
                    <li><a href="#expressions-ref-literal">문자 표현</a></li>
                    <li><a href="#expressions-properties-arrays">프로퍼티, 배열, 리스트, 맵, 색인작성자</a></li>
                    <li><a href="#expressions-inline-lists">인라인 리스트</a></li>
                    <li><a href="#expressions-inline-maps">인라인 맵</a></li>
                    <li><a href="#expressions-array-construction">배열 생성하기</a></li>
                    <li><a href="#expressions-methods">메서드</a></li>
                    <li><a href="#expressions-operators">연산자</a>
                        <ul>
                            <li><a href="#expressions-operators-relational">관계 연산자</a></li>
                            <li><a href="#expressions-operators-logical">논리 연산자</a></li>
                            <li><a href="#expressions-operators-mathematical">산술 연산자</a></li>
                            <li><a href="#expressions-assignment">대입 연산자</a></li>
                        </ul>
                    </li>
                    <li><a href="#expressions-types">타입</a></li>
                    <li><a href="#expressions-constructors">생성자</a></li>
                    <li><a href="#expressions-ref-variables">변수</a>
                        <ul>
                            <li><a href="#expressions-this-root">#this 와 #root 변수</a></li>
                        </ul>
                    </li>
                    <li><a href="#expressions-ref-functions">함수</a></li>
                    <li><a href="#expressions-bean-references">빈 참조</a></li>
                    <li><a href="#expressions-operator-ternary">삼항 연산자 (If-Then-Else)</a></li>
                    <li><a href="#expressions-operator-elvis">엘비스 연산자</a></li>
                    <li><a href="#expressions-operator-navigation">Safe Navigation 연산자</a></li>
                    <li><a href="#expressions-collection-selection">컬렉션 선택</a></li>
                    <li><a href="#expressions-collection-projection">컬렉션 투영</a></li>
                    <li><a href="#expressions-templating">표현 템플릿화</a></li>
                </ol>
            </li>
            <li><a href="#expressions-example-classes">예시에서 사용된 클래스</a></li>
        </ol>
    </li>
    <li><a href="#aop"> 스프링과 함께하는 관점 지향 프로그래밍(AOP) </a>
        <ol>
            <li><a href="#aop-introduction-defn">AOP 개념</a></li>
            <li><a href="#aop-introduction-spring-defn">스프링 AOP 기능과 목표</a></li>
            <li><a href="#aop-introduction-proxies">AOP Proxies</a></li>
            <li><a href="#aop-ataspectj">@AspectJ 지원</a>
                <ol>
                    <li><a href="#aop-aspectj-support">@AspectJ 지원 활성화하기</a></li>
                    <li><a href="#aop-at-aspectj">애스팩트 선언하기</a></li>
                    <li><a href="#aop-pointcuts">포인트컷 선언하기</a>
                        <ul>
                            <li><a href="#aop-pointcuts-designators">지원되는 포인트컷 지명자</a></li>
                            <li><a href="#aop-pointcuts-combining">포인트컷 표현 결합하기</a></li>
                            <li><a href="#aop-common-pointcuts">공통의 포인트컷 정의 공유하기</a></li>
                            <li><a href="#aop-pointcuts-examples">예시</a></li>
                            <li><a href="#writing-good-pointcuts">좋은 포인트컷 작성하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#aop-advice">어드바이스 선언하기</a>
                        <ul>
                            <li><a href="#aop-advice-before">Before 어드바이스</a></li>
                            <li><a href="#aop-advice-after-returning">AfterReturning 어드바이스</a></li>
                            <li><a href="#aop-advice-after-throwing">AfterThrowing 어드바이스</a></li>
                            <li><a href="#aop-advice-after-finally">After(Finally) 어드바이스</a></li>
                            <li><a href="#aop-advice-around-advice">Around 어드바이스</a></li>
                            <li><a href="#aop-advice-advice-params">어드바이스 파라메터</a></li>
                            <li><a href="#aop-advice-advice-ordering">어드바이스 순서 정하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#aop-introduction">소개</a></li>
                    <li><a href="#aop-instantiation-models">어스펙트 인스턴스화 모델</a></li>
                    <li><a href="#aop-ataspectj-example">AOP 예시</a></li>
                </ol>
            </li>
            <li><a href="#aop-schema">스키마 기반 AOP</a>
                <ol>
                    <li><a href="#aop-schema-declaring-an-aspect">애스팩트 선언하기</a></li>
                    <li><a href="#aop-schema-pointcuts">포인트컷 선언하기</a></li>
                    <li><a href="#aop-schema-advice">어드바이스 선언하기</a>
                        <ul>
                            <li><a href="#aop-schema-advice-before">Before 어드바이스</a></li>
                            <li><a href="#aop-schema-advice-after-returning">AfterReturning 어드바이스</a></li>
                            <li><a href="#aop-schema-advice-after-throwing">AfterThrowing 어드바이스</a></li>
                            <li><a href="#aop-schema-advice-after-finally">After(Finally) 어드바이스</a></li>
                            <li><a href="#aop-schema-advice-around-advice">Around 어드바이스</a></li>
                            <li><a href="#aop-schema-advice-advice-params">어드바이스 파라메터</a></li>
                            <li><a href="#aop-schema-advice-advice-ordering">어드바이스 순서 정하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#aop-schema-introductions">소개</a></li>
                    <li><a href="#aop-schema-instantiation-models">애스팩트 인스턴스화 모델</a></li>
                    <li><a href="#aop-schema-advisors">어드바이저</a></li>
                    <li><a href="#aop-schema-example">AOP 스키마 예시</a></li>
                </ol>
            </li>
            <li><a href="#aop-choosing">사용할 AOP 정의 스타일 선택하기</a>
                <ol>
                    <li><a href="#aop-spring-or-aspectj">스프링 AOP? 혹은 완전한 AspectJ?</a></li>
                    <li><a href="#aop-ataspectj-or-xml">스프링 AOP에서 @AspectJ? 혹은 XML?</a></li>
                </ol>
            </li>
            <li><a href="#aop-mixing-styles">Aspect 타입 섞기</a></li>
            <li><a href="#aop-proxying">프록시 작동원리</a>
                <ol>
                    <li><a href="#aop-understanding-aop-proxies">AOP 프록시 이해하기</a></li>
                </ol>
            </li>
            <li><a href="#aop-aspectj-programmatic">프로그래밍적으로 @AspectJ 프록시 만들기</a></li>
            <li><a href="#aop-using-aspectj">스프링 어플리케이션에서 AspectJ 사용하기</a>
                <ol>
                    <li><a href="#aop-atconfigurable">스프링에서 도메인객체에 AspectJ를 이용하여 의존성 주입하기</a>
                        <ul>
                            <li><a href="#aop-configurable-testing">@Configurable 객체 단위 테스트</a></li>
                            <li><a href="#aop-configurable-container">여러 개의 어플리케이션 컨텍스트 이용하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#aop-ajlib-other">AspectJ를 위한 다른 스프링 애스팩트</a></li>
                    <li><a href="#aop-aj-configure">스프링 IoC를 사용하여 AspectJ 애스팩트 설정하기</a></li>
                    <li><a href="#aop-aj-ltw">스프링 프레임워크에서 AspectJ 로드 타임 위빙</a>
                        <ul>
                            <li><a href="#aop-aj-ltw-first-example">첫 예시</a></li>
                            <li><a href="#aop-aj-ltw-the-aspects">애스팩트</a></li>
                            <li><a href="#aop-aj-ltw-aop_dot_xml">'META-INF/aop.xml'</a></li>
                            <li><a href="#aop-aj-ltw-libraries">필요 라이브러리(JARS)</a></li>
                            <li><a href="#aop-aj-ltw-spring">스프링 설정</a></li>
                            <li><a href="#aop-aj-ltw-environments">고유 환경 설정</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#aop-resources">추가 자료</a></li>
        </ol>
    </li>
    <li><a href="#aop-api"> 스프링 AOP API </a>
        <ol>
            <li><a href="#aop-api-pointcuts">스프링에서 포인트컷</a>
                <ol>
                    <li><a href="#aop-api-concepts">개념</a></li>
                    <li><a href="#aop-api-pointcut-ops">포인트컷에서 작동</a></li>
                    <li><a href="#aop-api-pointcut-aspectj">AspectJ 포인트컷 표현식</a></li>
                    <li><a href="#aop-api-pointcuts-impls">편리한 PointCut 구현</a></li>
                    <li><a href="#aop-api-superclasses">포인트컷 부모클래스</a></li>
                    <li><a href="#aop-api-pointcuts-custom">커스텀 포인트컷</a></li>
                </ol>
            </li>
            <li><a href="#aop-api-advice">스프링에서의 어드바이스</a>
                <ol>
                    <li><a href="#aop-api-advice-lifecycle">어드바이스 생명주기</a></li>
                    <li><a href="#aop-api-advice-types">스프링에서 어드바이스 유형</a>
                        <ul>
                            <li><a href="#aop-api-advice-around">Interception Around 어드바이스</a></li>
                            <li><a href="#aop-api-advice-before">Before 어드바이스</a></li>
                            <li><a href="#aop-api-advice-throws">Throws 어드바이스</a></li>
                            <li><a href="#aop-api-advice-after-returning">After Returning 어드바이스</a></li>
                            <li><a href="#aop-api-advice-introduction">Introduction 어드바이스</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#aop-api-advisor">스프링에서의 어드바이저</a></li>
            <li><a href="#aop-pfb">ProxyFactoryBean을 이용하여 AOP 프록시 만들기</a>
                <ol>
                    <li><a href="#aop-pfb-1">기초</a></li>
                    <li><a href="#aop-pfb-2">자바빈 프로퍼티</a></li>
                    <li><a href="#aop-pfb-proxy-types">JDK 기반, CGLIB 기반 프로퍼티</a></li>
                    <li><a href="#aop-api-proxying=intf">프록시 인터페이스</a></li>
                    <li><a href="#aop-api-proxying-class">프록시 클래스</a></li>
                    <li><a href="#aop-global-advisors">"글로벌" 어드바이스 사용하기</a></li>
                </ol>
            </li>
            <li><a href="#aop-concise-proxy">간결한 프록시 정의</a></li>
            <li><a href="#aop-prog">ProxyFactory를 이용하여 AOP 프록시 프로그래밍적으로 만들기</a></li>
            <li><a href="#aop-api-advised">Advised 객체 조작하기</a></li>
            <li><a href="#aop-autoproxy">"자동-프록시" 기능 이용하기</a>
                <ol>
                    <li><a href="#aop-autoproxy-choices">자동-프록시 빈 정의</a>
                        <ul>
                            <li><a href="#aop-api-autoproxy">BeanNameAutoProxyCreator</a></li>
                            <li><a href="#aop-api-autoproxy-default">DefaultAdvisorAutoProxyCreator</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#aop-targetsource">TargetSource 구현 이용하기</a>
                <ol>
                    <li><a href="#aop-ts-swap">동적 전환가능한 Target Sources</a></li>
                    <li><a href="#aop-ts-pool">Target Source 풀</a></li>
                    <li><a href="#aop-ts-prototype">프로토타입 Target Sources</a></li>
                    <li><a href="#aop-ts-thredlocal">ThreadLocal Target Sources</a></li>
                </ol>
            </li>
            <li><a href="#aop-extensibility">새로운 어드바이스 타입 정의하기</a></li>
        </ol>
    </li>
    <li><a href="#null-safety"> 안전한 Null </a>
        <ol>
            <li><a href="#use-cases">사용 예시</a></li>
            <li><a href="#jsr-305-meta-annotations">JSR-305 메타 어노테이션</a></li>
        </ol>
    </li>
    <li><a href="#databuffers"> 데이터 버퍼와 코덱 </a>
        <ol>
            <li><a href="#databuffers-factory">DataBufferFactory</a></li>
            <li><a href="#databuffers-buffer">DataBuffer</a></li>
            <li><a href="#databuffers-buffer-pooled">PooledDataBuffer</a></li>
            <li><a href="#databuffers-utils">DataBufferUtils</a></li>
            <li><a href="#codecs">Codecs</a></li>
            <li><a href="#databuffers-using">DataBuffer 사용하기</a></li>
        </ol>
    </li>
    <li><a href="#appendix"> 부록 </a>
        <ol>
            <li><a href="#xsd-schemas">XML 스키마</a>
                <ol>
                    <li><a href="#xsd-schemas-util">util 스키마</a>
                        <ul>
                            <li><a href="#xsd-schemas-util-constant">&lt;util:constant/&gt;사용하기</a></li>
                            <li><a href="#xsd-schemas-util-property-path">&lt;util:property-path/&gt;사용하기</a></li>
                            <li><a href="#xsd-schemas-util-properties">&lt;util:properties/&gt;사용하기</a></li>
                            <li><a href="#xsd-schemas-util-list">&lt;util:list/&gt;사용하기</a></li>
                            <li><a href="#xsd-schemas-util-map">&lt;util:map/&gt;사용하기</a></li>
                            <li><a href="#xsd-schemas-util-set">&lt;util:set/&gt;사용하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#xsd-schemas-aop">aop 스키마</a></li>
                    <li><a href="#xsd-schemas-context">context 스키마</a>
                        <ul>
                            <li><a href="#xsd-schemas-context-pphc">&lt;property-placeholder/&gt;사용하기</a></li>
                            <li><a href="#xsd-schemas-context-ac">&lt;annotation-config/&gt;사용하기</a></li>
                            <li><a href="#xsd-schemas-context-component-scan">&lt;component-scan/&gt;사용하기</a></li>
                            <li><a href="#xsd-schemas-context-ltw">&lt;load-time-weaver/&gt;사용하기</a></li>
                            <li><a href="#xsd-schemas-context-sc">&lt;spring-configured/&gt;사용하기</a></li>
                            <li><a href="#xsd-schemas-context-mbe">&lt;mbean-export/&gt;사용하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#xsd-schemas-beans">Beans 스키마</a></li>
                </ol>
            </li>
            <li><a href="#xml-custom">XML 스키마 authoring</a>
                <ol>
                    <li><a href="#xsd-custom-schema">authoring 스키마</a></li>
                    <li><a href="#xsd-custom-namespacehandler">NamespceHandler 코딩하기</a></li>
                    <li><a href="#xsd-custom-parser">BeanDefinitionParser 사용하기</a></li>
                    <li><a href="#xsd-custom-registration">Handler와 스키마 등록하기</a>
                        <ul>
                            <li><a href="#xsd-custom-registration-spring-handlers">META-INF/spring.handlers 작성하기</a></li>
                            <li><a href="#xsd-custom-registration-spring-schemas">META-INF/spring.schemas 작성하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#xsd-custom-using">스프링 XML 설정에서 커스텀 확장기능 이용하기</a></li>
                    <li><a href="#xsd-custom-meat">더 자세한 예시</a>
                        <ul>
                            <li><a href="#xsd-custom-custom-nested">커스텀 요소 안에 커스텀 요소 넣기</a></li>
                            <li><a href="#xsd-custom-custom-just-attributes">"일반" 요소에 커스텀 특성</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
        </ol>
    </li>
</ol>

---

<h1 id="spring-core"> 핵심 기술 </h1>
레퍼런스 문서 중 이 부분은 스프링 프레임워크에 필수적인 기술을 담당한다.     
이 것들중 가장 중요한 것은 스프링 프레임워크의 역 흐름 제어(IoC)이다. 스프링 프레임워크 IoC 컨테이너의 철저한 관리는 스프링의 광범위한 관점 지향 프로그래밍(AOP) 적용 범위에 기반한다. 스프링 프레임워크는 개념적으로 이해하기 쉽고 자바 엔터프라이즈 프로그래밍에 요구조건의 핵심 80%를 성공적으로 처리하는 자체 AOP 프레임워크를 가지고 있다.     
또한 스프링은 AspectJ(현재 자바 엔터프라이즈 환경에서 가장 질 좋고 성숙한 AOP 구현)포함시켜 제공된다.
<h2 id="beans"> IoC 컨테이너 </a>
이 장은 스프링의 역 흐름 제어(IoC) 컨테이너를 담당한다.
<h3 id="beans-introduction">IoC 컨테이너와 빈의 소개</a>
이 장은 스프링 프레임워크의 역흐름 제어 구현 원리를 담당한다. 역 흐름 제어(IoC)는 의존성 주입(DI)로도 알려져 있다.
<h3 id="beans-basics">컨테이너 개요</a>

<h4 id="beans-factory-metadata">메타데이터 설정하기</a>
<h4 id="beans-factory-instantiation">컨테이너 인스턴스 만들기</a>

<h5 id="beans-factory-xml-import">XML 기반 설정 메타데이터 만들기</a>
<h5 id="groovy-bean-definition-dsl">Groovy 빈 정의 DSL</a>


<h4 id="bean-factory-client">컨테이너 이용하기</a>


<h3 id="beans-definition">빈 개요</a>

<h4 id="beans-beanname">빈 이름 붙이기</a>

<h5 id="beans-beanname-alias">빈 정의 외부에서 빈 별명 설정하기</a>


<h4 id="beans-factory-class">빈 인스턴스 만들기</a>

<h5 id="beans-factory-class-ctor">컨테이너로 인스턴스 만들기</a>
<h5 id="beans-factory-class-static-factory-method">스태틱 팩토리 메소드로 만들기</a>
<h5 id="beans-factory-class-instance-factory-method">인스턴스 팩토리 메소드로 만들기</a>
<h5 id="beans-factory-type-determination">빈의 런타임 타입 결정하기</a>




<h3 id="beans-dependencies">의존성</a>

<h4 id="beans-factory-collaborators">의존성 주입</a>

<h5 id="beans-constructor-injection">생성자 기반 의존성 주입</a>
<h5 id="beans-setter-injection">세터 기반 의존성 주입</a>
<h5 id="beans-dependency-resolution">의존성 결정 과정</a>
<h5 id="beans-some-examples">의존성 주입 예제</a>


<h4 id="beans-factory-properties-detailed">의존성과 설정 상세</a>

<h5 id="beans-value-element">변경없는 값(Primitives, String 등등)</a>
<h5 id="beans-ref-element">다른 빈 참조(협업)</a>
<h5 id="beans-inner-beans">내부 빈</a>
<h5 id="beans-collection-elements">컬렉션</a>
<h5 id="beans-null-element">Null과 비어있는 스트링</a>
<h5 id="beans-p-namespace">P 네임스페이스를 이용한 XML 단축</a>
<h5 id="beans-c-namespace">c 네임스페이스를 이용한 XML 단축</a>
<h5 id="beans-compound-property-names">복합 프로퍼티 이름</a>


<h4 id="beans-factory-dependson">depends-on 사용하기</a>
<h4 id="beans-factory-lazy-init">지연 초기화 빈</a>
<h4 id="beans-factory-autowire">Autowiring 협력자</a>

<h5 id="beans-autowired-exceptions">자동 연결의 한계와 단점</a>
<h5 id="beans-factory-autowire-candidate">자동 연결에 빈 제외하기</a>


<h4 id="beans-factory-method-injection">메소드 주입</a>

<h5 id="beans-factory-lookup-method-injection">Lookup 메소드 주입</a>
<h5 id="beans-factory-arbitrary-method-replacement">임의의 메소드 대체</a>




<h3 id="beans-factory-scopes">빈 스코프</a>

<h4 id="beans-factory-scopes-singleton">싱글톤 스코프</a>
<h4 id="beans-factory-scopes-prototype">프로토타입 스코프</a>
<h4 id="beans-factory-scopes-sing-prot-interaction">프로토타입 빈 의존성을 가지는 싱글톤 빈</a>
<h4 id="beans-factory-scopes-other">Request, Session, Application, WebSocket 스코프</a>

<h5 id="beans-factory-scopes-other-web-configuration">초기 웹 설정</a>
<h5 id="beans-factory-scopes-request">Request 스코프</a>
<h5 id="beans-factory-scopes-session">Session 스코프</a>
<h5 id="beans-factory-scopes-application">Application 스코프</a>
<h5 id="beans-factory-scopes-other-injection">스코프 빈 의존성</a>


<h4 id="beans-factory-scopes-custom">커스텀 스코프</a>

<h5 id="beans-factory-scopes-custom-creating">커스텀 스코프 만들기</a>
<h5 id="beans-factory-scopes-custom-using">커스텀 스코프 이용하기</a>




<h3 id="beans-factory-nature">빈의 특성 설정하기</a>

<h4 id="beans-factory-lifecycle">콜백의 생명주기</a>

<h5 id="beans-factory-lifecycle-initializingbean">콜백 정의하기</a>
<h5 id="beans-factory-lifecycle-disposablebean">파괴 콜백</a>
<h5 id="beans-factory-lifecycle-default-init-destroy-methods">기본 초기화 메소드, 파괴 메소드</a>
<h5 id="beans-factory-lifecycle-combined-effects">생명주기 작동원리 조합하기</a>
<h5 id="beans-factory-lifecycle-processor">Startup, Shutdown 콜백</a>
<h5 id="beans-factory-shutdown">웹 어플리케이션이 아닌 환경에서 스프링 IoC 컨테이너 아름답게 종료하기</a>


<h4 id="beans-factory-aware">ApplicationContextAware, BeanNameAware</a>
<h4 id="awar-list">다른 Aware 인터페이스</a>


<h3 id="beans-child-bean-definitions">빈의 정의 상속</a>
<h3 id="beans-factory-extension">컨테이너 확장 시점</a>

<h4 id="beans-factory-extension-bpp">BeanPostProcessor를 이용하여 빈 커스터마이징 하기</a>

<h5 id="beans-factory-extension-bpp-examples-hw">예제: Hello World, BeanPostProcessor 스타일</a>
<h5 id="beans-factory-extension-bpp-examples-rabpp">예제: RequiredAnnotationBeanPostProcessor</a>


<h4 id="beans-factory-extension-factory-postprocessors">BeanFactoryPostProcessor를 이용하여 설정 메타데이터 설정하기</a>

<h5 id="beans-factory-placeholderconfigurer">예제: 클래스 이름 대체 PropertySourcesPlaceholderConfigurer</a>
<h5 id="beans-factory-overrideconfigurer">예제: PropertyOverrideConfigurer</a>


<h4 id="beans-factory-extension-factorybean">FactoryBean을 이용하여 인스턴스화 로직 커스터마이징 하기</a>


<h3 id="beans-annotation-config">어노테이션 기반 컨테이너 설정</a>

<h4 id="beans-required-annotation">@Required</a>
<h4 id="beans-autowired-annotation">@Autowired 사용하기</a>
<h4 id="beans-autowired-annotation-primary">@Primary를 이용하여 어노테이션 기반 자동 연결 미세 조정하기</a>
<h4 id="beans-autowired-annotation-qualifiers"></a>Qualifers를 이용하여 어노테이션 기반 자동 연결  미세 조정하기
<h4 id="beans-generics-as-qualifiers">제네릭을 자동연결 Qualifers로 이용하기</a>
<h4 id="beans-custom-autowire-configurer">CustomAutowireConfig 이용하기</a>
<h4 id="beans-resource-annotation">@Resource를 사용한 주입</a>
<h4 id="beans-value-annotations">@Value 사용하기</a>
<h4 id="beans-postconstruct-and-predestroy-annotations">@PostConstruct와 @PreDestory 사용하기</a>


<h3 id="beans-classpath-scanning">클래스패스 스캐닝과 관리되는 컴포넌트</a>

<h4 id="beans-stereotype-annotations">@Component와 추가적인 정형화된 어노테이션</a>
<h4 id="beans-meta-annotations">메타 어노테이션과 복합 어노테이션 이용하기</a>
<h4 id="beans-scanning-autodetection">클래스 검색과 빈 정의 등록을 자동으로 수행하기 </a>
<h4 id="beans-scanning-filters">필터를 이용하여 스캐닝 커스터마이징하기</a>
<h4 id="beans-factorybeans-annotations">컴포넌트 내부에 빈 메타데이터 정의하기</a>
<h4 id="beans-scanning-name-generator">자동 검색된 컴포넌트 이름 붙이기</a>
<h4 id="beans-scanning-scope-resolver">자동 검색된 컴포넌트에 스코프 부여하기</a>
<h4 id="beans-scanning-qualifiers">Qualifier 메타데이터 어노테이션으로 부여하기</a>
<h4 id="beans-scanning-index">후보 컴포넌트 색인 생성하기</a>


<h3 id="beans-standard-annotations">JSR 330 표준 어노테이션 사용하기</a>

<h4 id="beans-inject-named">@Inject와 @Named를 이용한 의존성 주입</a>
<h4 id="beans-named">@Named와 @ManagedBean: @Component 어노테이션의 표준 표현</a>
<h4 id="beans-stardard-annotations-limitations">JSR-330 표준 어노테이션의 한계</a>


<h3 id="beans-java">자바 기반 컨테이너설정</a>

<h4 id="beans-java-basic-concepts">기본 개념: @Bean과 @Configuration</a>
<h4 id="beans-java-instantiating-container">AnnotationConfigApplicationContext를 이용하여 스프링 컨테이너 인스턴스화 하기</a>

<h5 id="beans-java-instantiating-container-constructor">간단하게 만들어보기</a>
<h5 id="beans-java-instantiating-container-register">register(Class<?>...)을 이용하여 프로그래밍적으로 컨테이너 만들기</a>
<h5 id="beans-java-instantiating-container-scan">scan(String...)을 이용하여 컴포넌트 스캔 활성화하기</a>
<h5 id="beans-java-instantiating-container-web">AnnotationConfigWebApplicationContext을 이용한 웹 어플리케이션 지원</a>


<h4 id="beans-java-bean-annotation">@Bean 어노테이션 사용하기</a>

<h5 id="beans-java-declaring-a-bean">빈 정의하기</a>
<h5 id="beans-java-dependencies">빈 의존성</a>
<h5 id="beans-java-lifecycle-callbacks">생명주기 콜백 받기</a>
<h5 id="beans-java-specifying-bean-scope">빈 스코프 설정하기</a>
<h5 id="beans-java-customizing-bean-naming">빈 이름 설정하기</a>
<h5 id="beans-java-bean-aliasing">빈 별명 설정하기</a>
<h5 id="beans-java-bean-description">빈 설명</a>


<h4 id="beans-java-configuration-annotation">@Configuration 어노테이션 이용하기</a>

<h5 id="beans-java-injecting-dependencies">내부 빈 의존성 주입하기</a>
<h5 id="beans-java-method-injection">Lookup 메소드 주입하기</a>
<h5 id="beans-java-further-information-java-config">자바 기반 설정이 내부적으로 작동하는 방법에 대한 추가적인 정보</a>


<h4 id="beans-java-composing-configuration-classes">자바 기반 설정 구성하기</a>

<h5 id="beans-java-using-import">@Import 어노테이션 사용하기</a>
<h5 id="beans-java-conditional">조건에 따라 @Configuration 클래스와 @Bean 메소드 포함시키기</a>
<h5 id="beans-java-combining">자바 기반 설정과 XML 기반 설정 조합하기</a>




<h3 id="beans-envirionment">환경 추상화</a>

<h4 id="beans-definition-profiles">빈 정의 프로필</a>

<h5 id="beans-definition-profiles-java">@Profile 이용하기</a>
<h5 id="beans-definition-profiles-xml">XML 빈 정의 프로필</a>
<h5 id="beans-definition-profiles-enable">프로필 활성화하기</a>
<h5 id="beans-definition-profiles-default">기본 프로필</a>


<h4 id="beans-property-source-abstaction">PropertySource 추상화</a>
<h4 id="beans-using-propertysource">@PropertySource 사용하기</a>
<h4 id="beans-placeholder-resolution-in-statements">표현법의 PlaceHolder Resolution</a>


<h3 id="context-load-time-weaver">LoadTimeWeaver 등록하기</a>
<h3 id="context-introduction">어플리케이션 콘텍스트의 추가적인 사용기능</a>

<h4 id="context-functionality-messagesource">MessageSource를 이용한 내재화</a>
<h4 id="context-functionality-events">표준, 커스텀 이벤트</a>

<h5 id="context-functionality-events-annotation">어노테이션 기반 이벤트 리스너</a>
<h5 id="context-functionality-events-async">비동기 리스너</a>
<h5 id="context-functionality-events-order">리스너 순서 정하기</a>
<h5 id="context-functionality-events-generics">Generic 이벤트</a>


<h4 id="context-functionality-resources">로우레벨 자원에 편리한 접근</a>
<h4 id="context-create">웹 어플리케이션을 위한 편리한 ApplicationContext 인스턴스화</a>
<h4 id="context-deploy-rar">스프링 ApplicationContext를 Java EE RAR 파일로 배포하기</a>


<h3 id="beans-beanfactory">빈 팩토리</a>

<h4 id="context-introduction-ctx-vs-beanfactory">BeanFactory? 혹은 ApplicationContext?</a>




<h2 id="resources"> 리소스 </a>

<h3 id="resources-introduction">소개</a>
<h3 id="resources-resource">리소스 인터페이스</a>
<h3 id="resources-implementations">이미 만들어진 리소스 구현체</a>

<h4 id="resources-implementations-urlresource">UrlResource</a>
<h4 id="resources-implementations-classpathresource">ClassPathResource</a>
<h4 id="resources-implementations-filesystemresource">FileSystemResource</a>
<h4 id="resources-implementations-servletcontextresource">ServletContextResource</a>
<h4 id="resources-implementations-inputstreamresource">InputStreamResource</a>
<h4 id="resources-implementations-bytearrayresource">ByteArrayResource</a>


<h3 id="resources-resourceloader">ResourceLoader</a>
<h3 id="resources-resourceloaderaware">ResourceLoaderAware 인터페이스</a>
<h3 id="resources-as-dependencies">의존성으로서의 리소스</a>
<h3 id="resources-app-ctx">어플리케이션 콘텍스트와 리소스 패스</a>

<h4 id="resources-app-ctx-construction">어플리케이션 콘텍스트 생성하기</a>

<h5 id="resources-app-ctx-classpathxml">ClassPathXmlApplicationContext 인스턴스 생성하기 - 지름길</a>


<h4 id="resources-app-ctx-wildcards-in-resource-paths">어플리케이션 콘텍스트 생성자 리소스 패스에 와일드카드</a>

<h5 id="resources-app-ctx-ant-patterns-in-paths">Ant 스타일 패턴</a>
<h5 id="resources-classpath-wildcards">classpath*: 접두사</a>
<h5 id="resources-wildcards-in-path-other-stuff">와일드카드와 관련된 추가적인 사항</a>


<h4 id="resources-filesystemresource-caveats">FileSystemResource 경고</a>




<h2 id="validation"> 검증, 데이터 바인딩, 타입 변환 </a>

<h3 id="validator">스프링 Validator 인터페이스를 통한 검증</a>
<h3 id="validation-conversion">코드에 에러메세지 적용하기</a>
<h3 id="beans-beans">빈 조작과 BeanWrapper</a>

<h4 id="beans-beans-conventions">기본적인 프로퍼티, 중첩된 프로퍼티 설정하고 가져오기</a>
<h4 id="beans-beans-conversion">이미 구현된 PropertyEditor</a>

<h5 id="beans-beans-conversion-customeditor-registration">커스텀 PropertyEditor 구현체 등록하기</a>




<h3 id="core-convert">스프링 타입 변환</a>

<h4 id="core-convert-Converter-API">변환기 서비스 제공자 인터페이스</a>
<h4 id="core-convert-ConverterFactory-SPI">ConverterFactory 사용하기</a>
<h4 id="core-convert-GenericConverter-SPI">GenericConverter 사용하기</a>

<h5 id="core-convert-ConditionalGenericConverter-SPI">ConditionalGenericConverter 사용하기</a>


<h4 id="core-convert-ConversionService-API">ConversionService API</a>
<h4 id="core-convert-Spring-config">ConversionService 설정하기</a>
<h4 id="core-convert-programmatic-usage">ConversionService 프로그래밍 적으로 사용하기</a>


<h3 id="format">스프링 필드 형식 설정</a>

<h4 id="format-Formatter-SPI">Formatter 서비스 제공자 인터페이스</a>
<h4 id="format-CustomFormatAnnotations">어노테이션 기반 형식설정</a>

<h5 id="format-annotations-api">형식 어노테이션 API</a>


<h4 id="format-FormatterRegistry-SPI">FormatterRegistry 서비스 제공자 인터페이스</a>
<h4 id="format-FormatterRegistrar-SPI">FormatterRegistrar 서비스 제공자 인터페이스</a>
<h4 id="format-configuring-formatting-mvc">스프링 MVC에서 형식 설정하기</a>


<h3 id="format-configuring-formatting-globaldatetimeformat">글로벌 Data, Time 형식 설정</a>
<h3 id="validation-beanvalidation">빈 검증</a>

<h4 id="validation-beanvalidation-overview">빈 검증 개요</a>
<h4 id="validation-beanvalidation-spring">빈 검증 제공자 설정하기</a>

<h5 id="validation-beanvalidation-spring-inject">검증자 주입하기</a>
<h5 id="validation-beanvalidation-spring-constraints">커스텀 제한사항 설정하기</a>
<h5 id="validation-beanvalidation-spring-method">스프링 기반 메서드 검증</a>
<h5 id="validation-beanvalidation-spring-other">추가적인 설정 옵션</a>


<h4 id="validation-binder">DataBinder 설정하기</a>
<h4 id="validation-mvc">스프링 MVC 3 검증</a>




<h2 id="expressions"> 스프링 표현 언어(SpEL) </a>

<h3 id="expressions-evaluation">값 구하기</a>

<h4 id="expressions-evaluation-context">EvaluationContext 이해하기</a>

<h5 id="expressions-type-conversion">형식 변환</a>


<h4 id="expressions-parser-configuration">파서 설정하기</a>
<h4 id="expressions-spel-compilation">스프링 표현 언어 편집</a>

<h5 id="expressions-compiler-configuration">컴파일러 설정하기</a>
<h5 id="expressions-compiler-limitations">컴파일러 제한사항</a>




<h3 id="expressions-beandef">빈 정의 안의 표현</a>

<h4 id="expressions-beandef-xml-based">XML 설정</a>
<h4 id="expressions-beandef-annotation-based">어노테이션 설정</a>


<h3 id="expressions-langauge-ref">언어 레퍼런스</a>

<h4 id="expressions-ref-literal">문자 표현</a>
<h4 id="expressions-properties-arrays">프로퍼티, 배열, 리스트, 맵, 색인작성자</a>
<h4 id="expressions-inline-lists">인라인 리스트</a>
<h4 id="expressions-inline-maps">인라인 맵</a>
<h4 id="expressions-array-construction">배열 생성하기</a>
<h4 id="expressions-methods">메서드</a>
<h4 id="expressions-operators">연산자</a>

<h5 id="expressions-operators-relational">관계 연산자</a>
<h5 id="expressions-operators-logical">논리 연산자</a>
<h5 id="expressions-operators-mathematical">산술 연산자</a>
<h5 id="expressions-assignment">대입 연산자</a>


<h4 id="expressions-types">타입</a>
<h4 id="expressions-constructors">생성자</a>
<h4 id="expressions-ref-variables">변수</a>

<h5 id="expressions-this-root">#this 와 #root 변수</a>


<h4 id="expressions-ref-functions">함수</a>
<h4 id="expressions-bean-references">빈 참조</a>
<h4 id="expressions-operator-ternary">삼항 연산자 (If-Then-Else)</a>
<h4 id="expressions-operator-elvis">엘비스 연산자</a>
<h4 id="expressions-operator-navigation">Safe Navigation 연산자</a>
<h4 id="expressions-collection-selection">컬렉션 선택</a>
<h4 id="expressions-collection-projection">컬렉션 투영</a>
<h4 id="expressions-templating">표현 템플릿화</a>


<h3 id="expressions-example-classes">예시에서 사용된 클래스</a>


<h2 id="aop"> 스프링과 함께하는 관점 지향 프로그래밍(AOP) </a>

<h3 id="aop-introduction-defn">AOP 개념</a>
<h3 id="aop-introduction-spring-defn">스프링 AOP 기능과 목표</a>
<h3 id="aop-introduction-proxies">AOP Proxies</a>
<h3 id="aop-ataspectj">@AspectJ 지원</a>

<h4 id="aop-aspectj-support">@AspectJ 지원 활성화하기</a>
<h4 id="aop-at-aspectj">애스팩트 선언하기</a>
<h4 id="aop-pointcuts">포인트컷 선언하기</a>

<h5 id="aop-pointcuts-designators">지원되는 포인트컷 지명자</a>
<h5 id="aop-pointcuts-combining">포인트컷 표현 결합하기</a>
<h5 id="aop-common-pointcuts">공통의 포인트컷 정의 공유하기</a>
<h5 id="aop-pointcuts-examples">예시</a>
<h5 id="writing-good-pointcuts">좋은 포인트컷 작성하기</a>


<h4 id="aop-advice">어드바이스 선언하기</a>

<h5 id="aop-advice-before">Before 어드바이스</a>
<h5 id="aop-advice-after-returning">AfterReturning 어드바이스</a>
<h5 id="aop-advice-after-throwing">AfterThrowing 어드바이스</a>
<h5 id="aop-advice-after-finally">After(Finally) 어드바이스</a>
<h5 id="aop-advice-around-advice">Around 어드바이스</a>
<h5 id="aop-advice-advice-params">어드바이스 파라메터</a>
<h5 id="aop-advice-advice-ordering">어드바이스 순서 정하기</a>


<h4 id="aop-introduction">소개</a>
<h4 id="aop-instantiation-models">어스펙트 인스턴스화 모델</a>
<h4 id="aop-ataspectj-example">AOP 예시</a>


<h3 id="aop-schema">스키마 기반 AOP</a>

<h4 id="aop-schema-declaring-an-aspect">애스팩트 선언하기</a>
<h4 id="aop-schema-pointcuts">포인트컷 선언하기</a>
<h4 id="aop-schema-advice">어드바이스 선언하기</a>

<h5 id="aop-schema-advice-before">Before 어드바이스</a>
<h5 id="aop-schema-advice-after-returning">AfterReturning 어드바이스</a>
<h5 id="aop-schema-advice-after-throwing">AfterThrowing 어드바이스</a>
<h5 id="aop-schema-advice-after-finally">After(Finally) 어드바이스</a>
<h5 id="aop-schema-advice-around-advice">Around 어드바이스</a>
<h5 id="aop-schema-advice-advice-params">어드바이스 파라메터</a>
<h5 id="aop-schema-advice-advice-ordering">어드바이스 순서 정하기</a>


<h4 id="aop-schema-introductions">소개</a>
<h4 id="aop-schema-instantiation-models">애스팩트 인스턴스화 모델</a>
<h4 id="aop-schema-advisors">어드바이저</a>
<h4 id="aop-schema-example">AOP 스키마 예시</a>


<h3 id="aop-choosing">사용할 AOP 정의 스타일 선택하기</a>

<h4 id="aop-spring-or-aspectj">스프링 AOP? 혹은 완전한 AspectJ?</a>
<h4 id="aop-ataspectj-or-xml">스프링 AOP에서 @AspectJ? 혹은 XML?</a>


<h3 id="aop-mixing-styles">Aspect 타입 섞기</a>
<h3 id="aop-proxying">프록시 작동원리</a>

<h4 id="aop-understanding-aop-proxies">AOP 프록시 이해하기</a>


<h3 id="aop-aspectj-programmatic">프로그래밍적으로 @AspectJ 프록시 만들기</a>
<h3 id="aop-using-aspectj">스프링 어플리케이션에서 AspectJ 사용하기</a>

<h4 id="aop-atconfigurable">스프링에서 도메인객체에 AspectJ를 이용하여 의존성 주입하기</a>

<h5 id="aop-configurable-testing">@Configurable 객체 단위 테스트</a>
<h5 id="aop-configurable-container">여러 개의 어플리케이션 컨텍스트 이용하기</a>


<h4 id="aop-ajlib-other">AspectJ를 위한 다른 스프링 애스팩트</a>
<h4 id="aop-aj-configure">스프링 IoC를 사용하여 AspectJ 애스팩트 설정하기</a>
<h4 id="aop-aj-ltw">스프링 프레임워크에서 AspectJ 로드 타임 위빙</a>

<h5 id="aop-aj-ltw-first-example">첫 예시</a>
<h5 id="aop-aj-ltw-the-aspects">애스팩트</a>
<h5 id="aop-aj-ltw-aop_dot_xml">'META-INF/aop.xml'</a>
<h5 id="aop-aj-ltw-libraries">필요 라이브러리(JARS)</a>
<h5 id="aop-aj-ltw-spring">스프링 설정</a>
<h5 id="aop-aj-ltw-environments">고유 환경 설정</a>




<h3 id="aop-resources">추가 자료</a>


<h2 id="aop-api"> 스프링 AOP API </a>

<h3 id="aop-api-pointcuts">스프링에서 포인트컷</a>

<h4 id="aop-api-concepts">개념</a>
<h4 id="aop-api-pointcut-ops">포인트컷에서 작동</a>
<h4 id="aop-api-pointcut-aspectj">AspectJ 포인트컷 표현식</a>
<h4 id="aop-api-pointcuts-impls">편리한 PointCut 구현</a>
<h4 id="aop-api-superclasses">포인트컷 부모클래스</a>
<h4 id="aop-api-pointcuts-custom">커스텀 포인트컷</a>


<h3 id="aop-api-advice">스프링에서의 어드바이스</a>

<h4 id="aop-api-advice-lifecycle">어드바이스 생명주기</a>
<h4 id="aop-api-advice-types">스프링에서 어드바이스 유형</a>

<h5 id="aop-api-advice-around">Interception Around 어드바이스</a>
<h5 id="aop-api-advice-before">Before 어드바이스</a>
<h5 id="aop-api-advice-throws">Throws 어드바이스</a>
<h5 id="aop-api-advice-after-returning">After Returning 어드바이스</a>
<h5 id="aop-api-advice-introduction">Introduction 어드바이스</a>




<h3 id="aop-api-advisor">스프링에서의 어드바이저</a>
<h3 id="aop-pfb">ProxyFactoryBean을 이용하여 AOP 프록시 만들기</a>

<h4 id="aop-pfb-1">기초</a>
<h4 id="aop-pfb-2">자바빈 프로퍼티</a>
<h4 id="aop-pfb-proxy-types">JDK 기반, CGLIB 기반 프로퍼티</a>
<h4 id="aop-api-proxying=intf">프록시 인터페이스</a>
<h4 id="aop-api-proxying-class">프록시 클래스</a>
<h4 id="aop-global-advisors">"글로벌" 어드바이스 사용하기</a>


<h3 id="aop-concise-proxy">간결한 프록시 정의</a>
<h3 id="aop-prog">ProxyFactory를 이용하여 AOP 프록시 프로그래밍적으로 만들기</a>
<h3 id="aop-api-advised">Advised 객체 조작하기</a>
<h3 id="aop-autoproxy">"자동-프록시" 기능 이용하기</a>

<h4 id="aop-autoproxy-choices">자동-프록시 빈 정의</a>

<h5 id="aop-api-autoproxy">BeanNameAutoProxyCreator</a>
<h5 id="aop-api-autoproxy-default">DefaultAdvisorAutoProxyCreator</a>




<h3 id="aop-targetsource">TargetSource 구현 이용하기</a>

<h4 id="aop-ts-swap">동적 전환가능한 Target Sources</a>
<h4 id="aop-ts-pool">Target Source 풀</a>
<h4 id="aop-ts-prototype">프로토타입 Target Sources</a>
<h4 id="aop-ts-thredlocal">ThreadLocal Target Sources</a>


<h3 id="aop-extensibility">새로운 어드바이스 타입 정의하기</a>


<h2 id="null-safety"> 안전한 Null </a>

<h3 id="use-cases">사용 예시</a>
<h3 id="jsr-305-meta-annotations">JSR-305 메타 어노테이션</a>


<h2 id="databuffers"> 데이터 버퍼와 코덱 </a>

<h3 id="databuffers-factory">DataBufferFactory</a>
<h3 id="databuffers-buffer">DataBuffer</a>
<h3 id="databuffers-buffer-pooled">PooledDataBuffer</a>
<h3 id="databuffers-utils">DataBufferUtils</a>
<h3 id="codecs">Codecs</a>
<h3 id="databuffers-using">DataBuffer 사용하기</a>


<h2 id="appendix"> 부록 </a>

<h3 id="xsd-schemas">XML 스키마</a>

<h4 id="xsd-schemas-util">util 스키마</a>

<h5 id="xsd-schemas-util-constant">&lt;util:constant/&gt;사용하기</a>
<h5 id="xsd-schemas-util-property-path">&lt;util:property-path/&gt;사용하기</a>
<h5 id="xsd-schemas-util-properties">&lt;util:properties/&gt;사용하기</a>
<h5 id="xsd-schemas-util-list">&lt;util:list/&gt;사용하기</a>
<h5 id="xsd-schemas-util-map">&lt;util:map/&gt;사용하기</a>
<h5 id="xsd-schemas-util-set">&lt;util:set/&gt;사용하기</a>


<h4 id="xsd-schemas-aop">aop 스키마</a>
<h4 id="xsd-schemas-context">context 스키마</a>

<h5 id="xsd-schemas-context-pphc">&lt;property-placeholder/&gt;사용하기</a>
<h5 id="xsd-schemas-context-ac">&lt;annotation-config/&gt;사용하기</a>
<h5 id="xsd-schemas-context-component-scan">&lt;component-scan/&gt;사용하기</a>
<h5 id="xsd-schemas-context-ltw">&lt;load-time-weaver/&gt;사용하기</a>
<h5 id="xsd-schemas-context-sc">&lt;spring-configured/&gt;사용하기</a>
<h5 id="xsd-schemas-context-mbe">&lt;mbean-export/&gt;사용하기</a>


<h4 id="xsd-schemas-beans">Beans 스키마</a>


<h3 id="xml-custom">XML 스키마 authoring</a>

<h4 id="xsd-custom-schema">authoring 스키마</a>
<h4 id="xsd-custom-namespacehandler">NamespceHandler 코딩하기</a>
<h4 id="xsd-custom-parser">BeanDefinitionParser 사용하기</a>
<h4 id="xsd-custom-registration">Handler와 스키마 등록하기</a>

<h5 id="xsd-custom-registration-spring-handlers">META-INF/spring.handlers 작성하기</a>
<h5 id="xsd-custom-registration-spring-schemas">META-INF/spring.schemas 작성하기</a>


<h4 id="xsd-custom-using">스프링 XML 설정에서 커스텀 확장기능 이용하기</a>
<h4 id="xsd-custom-meat">더 자세한 예시</a>

<h5 id="xsd-custom-custom-nested">커스텀 요소 안에 커스텀 요소 넣기</a>
<h5 id="xsd-custom-custom-just-attributes">"일반" 요소에 커스텀 특성</a>
 