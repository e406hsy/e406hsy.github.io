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

이 내용은 [스프링 문서 5.2.6.RELEASE](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/overview.html#overview)를 번역한 내용으로 오역이 있을 수 있습니다.

현재 내용 작성중입니다.

# 목차
<h2><a href="#overview"> 핵심 기술 </a></h2>
<ol>
    <li><a href="#overview-spring"> IoC 컨테이너 </a>
        <ol>
            <li><a href="#">IoC 컨테이너와 빈의 소개</a></li>
            <li><a href="#">컨테이너 개요</a>
                <ol>
                    <li><a href="#">메타데이터 설정하기</a></li>
                    <li><a href="#">컨테이너 인스턴스 만들기</a>
                        <ul>
                            <li><a href="#">XML 기반 설정 메타데이터 만들기</a><li>
                            <li><a href="#">Groovy 빈 정의 DSL</a></li>
                        </ul>
                    </li>
                    <li><a href="#">컨테이너 이용하기</a>
                </ol>
            </li>
            <li><a href="#">빈 개요</a>
                <ol>
                    <li><a href="#">빈 이름 붙이기</a>
                        <ul>
                            <li><a href="#">빈 정의 외부에서 빈 별명 설정하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#">빈 인스턴스 만들기</a>
                        <ul>
                            <li><a href="#">컨테이너로 인스턴스 만들기</a></li>
                            <li><a href="#">스태틱 팩토리 메소드로 만들기</a></li>
                            <li><a href="#">인스턴스 팩토리 메소드로 만들기</a></li>
                            <li><a href="#">빈의 런타임 타입 정의하기</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#">의존성</a>
                <ol>
                    <li><a href="#">의존성 주입</a>
                        <ul>
                            <li><a href="#">생성자 기반 의존성 주입</a></li>
                            <li><a href="#">세터 기반 의존성 주입</a></li>
                            <li><a href="#">의존성 결정 과정</a></li>
                            <li><a href="#">의존성 주입 예제</a></li>
                        </ul>
                    </li>
                    <li><a href="#">의존성과 설정 상세</a>
                        <ul>
                            <li><a href="#">변경없는 값(Primitives, String 등등)</a></li>
                            <li><a href="#">다른 빈 참조(협업)</a></li>
                            <li><a href="#">내부 빈</a></li>
                            <li><a href="#">컬렉션</a></li>
                            <li><a href="#">Null과 비어있는 스트링</a></li>
                            <li><a href="#">P 네임스페이스를 이용한 XML 단축</a></li>
                            <li><a href="#">c 네임스페이스를 이용한 XML 단축</a></li>
                            <li><a href="#">복합 프로퍼티 이름</a></li>
                        </ul>
                    </li>
                    <li><a href="#">depends-on 사용하기</a></li>
                    <li><a href="#">지연 초기화 빈</a></li>
                    <li><a href="#">Autowiring 협력자</a>
                        <ul>
                            <li><a href="#">자동 연결의 한계와 단점</a></li>
                            <li><a href="#">자동 연결에 빈 제외하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#">메소드 주입</a>
                        <ul>
                            <li><a href="#">Lookup 메소드 주입</a></li>
                            <li><a href="#">임의의 메소드 대체</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#">빈 스코프</a>
                <ol>
                    <li><a href="#">싱글톤 스코프</a></li>
                    <li><a href="#">프로토타입 스코프</a></li>
                    <li><a href="#">프로토타입 빈 의존성을 가지는 싱글톤 빈</a></li>
                    <li><a href="#">Request, Session, Application, WebSocket 스코프</a>
                        <ul>
                            <li><a href="#">초기 웹 설정</a></li>
                            <li><a href="#">Request 스코프</a></li>
                            <li><a href="#">Session 스코프</a></li>
                            <li><a href="#">Application 스코프</a></li>
                            <li><a href="#">스코프 빈 의존성</a></li>
                        </ul>
                    </li>
                    <li><a href="#">커스텀 스코프</a>
                        <ul>
                            <li><a href="#">커스텀 스코프 만들기</a></li>
                            <li><a href="#">커스텀 스코프 이용하기</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#">빈의 특성 설정하기</a>
                <ol>
                    <li><a href="#">콜백의 생명주기</a>
                        <ul>
                            <li><a href="#">콜백 정의하기</a></li>
                            <li><a href="#">파괴 콜백</a></li>
                            <li><a href="#">기본 초기화 메소드, 파괴 메소드</a></li>
                            <li><a href="#">생명주기 작동원리 조합하기</a></li>
                            <li><a href="#">Startup, Shutdown 콜백</a></li>
                            <li><a href="#">웹 어플리케이션이 아닌 환경에서 스프링 IoC 컨테이너 아름답게 종료하기</a></li>
                        </ul>
                    </li>
                    <li><a href="#">ApplicationContextAware, BeanNameAware</a></li>
                    <li><a href="#">다른 Aware 인터페이스</a></li>
                </ol>
            </li>
            <li><a href="#">빈의 정의 상속</a></li>
            <li><a href="#">컨테이너 확장 시점</a>
                <ol>
                    <li><a href="#">BeanPostProcessor를 이용하여 빈 커스터마이징 하기</a>
                        <ul>
                            <li><a href="#">예제: Hello World, BeanPostProcessor 스타일</a></li>
                            <li><a href="#">예제: RequiredAnnotationBeanPostProcessor</a></li>
                        </ul>
                    </li>
                    <li><a href="#">BeanFactoryPostProcessor를 이용하여 설정 메타데이터 설정하기</a>
                        <ul>
                            <li><a href="#">예제: 클래스 이름 대체 PropertySourcesPlaceholderConfigurer</a></li>
                            <li><a href="#">예제: PropertyOverrideConfigurer</a></li>
                        </ul>
                    </li>
                    <li><a href="#">FactoryBean을 이용하여 인스턴스화 로직 커스터마이징 하기</a></li>
                </ol>
            </li>
            <li><a href="#">어노테이션 기반 컨테이너 설정</a>
                <ol>
                    <li><a href="#">@Required</a></li>
                    <li><a href="#">@Autowired 사용하기</a></li>
                    <li><a href="#">@Primary를 이용하여 어노테이션 기반 자동 연결 미세 조정하기</a></li>
                    <li><a href="#"></a>Qualifers를 이용하여 어노테이션 기반 자동 연결  미세 조정하기</li>
                    <li><a href="#">제네릭을 자동연결 Qualifers로 이용하기</a></li>
                    <li><a href="#">CustomAutowireConfig 이용하기</a></li>
                    <li><a href="#">@Resource를 사용한 주입</a></li>
                    <li><a href="#">@Value 사용하기</a></li>
                    <li><a href="#">@PostConstruct와 @PreDestory 사용하기</a></li>
                </ol>
            </li>
            <li><a href="#">클래스패스 스캐닝과 관리되는 컴포넌트</a>
                <ol>
                    <li><a href="#">@Component와 추가적인 정형화된 어노테이션</a></li>
                    <li><a href="#">메타 어노테이션과 복합 어노테이션 이용하기</a></li>
                    <li><a href="#">클래스 검색과 빈 정의 등록을 자동으로 수행하기 </a></li>
                    <li><a href="#">필터를 이용하여 스캐닝 커스터마이징하기</a></li>
                    <li><a href="#">컴포넌트 내부에 빈 메타데이터 정의하기</a></li>
                    <li><a href="#">자동 검색된 컴포넌트 이름 붙이기</a></li>
                    <li><a href="#">자동 검색된 컴포넌트에 스코프 부여하기</a></li>
                    <li><a href="#">Qualifier 메타데이터 어노테이션으로 부여하기</a></li>
                    <li><a href="#">후보 컴포넌트 색인 생성하기</a></li>
                </ol>
            </li>
            <li><a href="#">JSR 330 표준 어노테이션 사용하기</a>
                <ol>
                    <li><a href="#">@Inject와 @Named를 이용한 의존성 주입</a></li>
                    <li><a href="#">@Named와 @ManagedBean: @Component 어노테이션의 표준 표현</a></li>
                    <li><a href="#">JSR-330 표준 어노테이션의 한계</a></li>
                </ol>
            </li>
            <li><a href="#">자바 기반 컨테이너설정</a>
                <ol>
                    <li><a href="#">기본 개념: @Bean과 @Configuration</a></li>
                    <li><a href="#">AnnotationConfigApplicationContext를 이용하여 스프링 컨테이너 인스턴스화 하기</a>
                        <ul>
                            <li><a href="#">간단하게 만들어보기</a></li>
                            <li><a href="#">register(Class<?>...)을 이용하여 프로그래밍적으로 컨테이너 만들기</a></li>
                            <li><a href="#">scan(String...)을 이용하여 컴포넌트 스캔 활성화하기</a></li>
                            <li><a href="#">AnnotationConfigWebApplicationContext을 이용한 웹 어플리케이션 지원</a></li>
                        </ul>
                    </li>
                    <li><a href="#">@Bean 어노테이션 사용하기</a>
                        <ul>
                            <li><a href="#">빈 정의하기</a></li>
                            <li><a href="#">빈 의존성</a></li>
                            <li><a href="#">생명주기 콜백 받기</a></li>
                            <li><a href="#">빈 스코프 설정하기</a></li>
                            <li><a href="#">빈 이름 설정하기</a></li>
                            <li><a href="#">빈 별명 설정하기</a></li>
                            <li><a href="#">빈 설명</a></li>
                        </ul>
                    </li>
                    <li><a href="#">@Configuration 어노테이션 이용하기</a>
                        <ul>
                            <li><a href="#">내부 빈 의존성 주입하기</a></li>
                            <li><a href="#">Lookup 메소드 주입하기</a></li>
                            <li><a href="#">자바 기반 설정이 내부적으로 작동하는 방법에 대한 추가적인 정보</a></li>
                        </ul>
                    </li>
                    <li><a href="#">자바 기반 설정 구성하기</a>
                        <ul>
                            <li><a href="#">@Import 어노테이션 사용하기</a></li>
                            <li><a href="#">조건에 따라 @Configuration 클래스와 @Bean 메소드 포함시키기</a></li>
                            <li><a href="#">자바 기반 설정과 XML 기반 설정 조합하기</a></li>
                        </ul>
                    </li>
                </ol>
            </li>
            <li><a href="#">환경 추상화</a>
                <ol>
                    <li><a href="#">빈 정의 프로필</a>
                        <ul>
                            <li><a href="#">@Profile 이용하기</a></li>
                            <li><a href="#">XML 빈 정의 프로필</a></li>
                            <li><a href="#">프로필 활성화하기</a></li>
                            <li><a href="#">기본 프로필</a></li>
                        </ul>
                    </li>
                    <li><a href="#">PropertySource 추상화</a></li>
                    <li><a href="#">@PropertySource 사용하기</a></li>
                    <li><a href="#">표현법의 PlaceHolder Resolution</a></li>
                </ol>
            </li>
            <li><a href="#">LoadTimeWeaver 등록하기</a></li>
            <li><a href="#">어플리케이션 콘텍스트의 추가적인 사용기능</a>
                <ol>
                    <li><a href="#">MessageSource를 이용한 내재화</a></li>
                    <li><a href="#">표준, 커스텀 이벤트</a>
                        <ul>
                            <li><a href="#">어노테이션 기반 이벤트 리스너</a></li>
                            <li><a href="#">비동기 리스너</a></li>
                            <li><a href="#">리스너 순서 정하기</a></li>
                            <li><a href="#">Generic 이벤트</a></li>
                        </ul>
                    </li>
                    <li><a href="#">로우레벨 자원에 편리한 접근</a></li>
                    <li><a href="#">웹 어플리케이션을 위한 편리한 ApplicationContext 인스턴스화</a></li>
                    <li><a href="#">스프링 ApplicationContext를 Java EE RAR 파일로 배포하기</a></li>
                </ol>
            </li>
            <li><a href="#">빈 팩토리</a>
                <ol>
                    <li><a href="#">BeanFactory 또는 ApplicationContext?</a></li>
                </ol>
            </li>
        </ol>
    </li>
    <li><a href="#overview-history"> 리소스 </a>
        <ol>
            <li><a href="#">소개</a></li>
            <li><a href="#">리소스 인터페이스</a></li>
            <li><a href="#">이미 만들어진 리소스 구현체</a></li>
            <li><a href="#">ResourceLoader</a></li>
            <li><a href="#">ResourceLoaderAware 인터페이스</a></li>
            <li><a href="#">의존성으로서의 리소스</a></li>
            <li><a href="#">어플리케이션 콘텍스트와 리소스 패스</a></li>
        </ol>
    </li>
    <li><a href="#overview-history"> 검증, 데이터 바인딩, 타입 변환 </a>
        <ol>
            <li><a href="#">스프링 Validator 인터페이스를 통한 검증</a></li>
            <li><a href="#">코드에 에러메세지 적용하기</a></li>
            <li><a href="#">빈 조작과 BeanWrapper</a></li>
            <li><a href="#">스프링 타입 변환</a></li>
            <li><a href="#">스프링 필드 형식 설정</a></li>
            <li><a href="#">글로벌 Data, Time 형식 설정</a></li>
            <li><a href="#">빈 검증</a></li>
        </ol>
    </li>
    <li><a href="#overview-history"> 스프링 표현 언어(SpEL) </a>
        <ol>
            <li><a href="#">값 구하기</a></li>
            <li><a href="#">빈 정의 안의 표현</a></li>
            <li><a href="#">언어 레퍼런스</a></li>
            <li><a href="#">예시에서 사용된 클래스</a></li>
        </ol>
    </li>
    <li><a href="#overview-getting-started"> 스프링과 함께하는 관점 지향 프로그래밍(AOP) </a>
        <ol>
            <li><a href="#">AOP 개념</a></li>
            <li><a href="#">스프링 AOP 기능과 목표</a></li>
            <li><a href="#">AOP Proxies</a></li>
            <li><a href="#">@AspectJ 지원</a></li>
            <li><a href="#">스키마 기반 AOP</a></li>
            <li><a href="#">사용할 AOP 정의 스타일 선택하기</a></li>
            <li><a href="#">Aspect 타입 섞기</a></li>
            <li><a href="#">프록시 작동원리</a></li>
            <li><a href="#">프로그래밍적으로 @AspectJ 프록시 만들기</a></li>
            <li><a href="#">스프링 어플리케이션에서 AspectJ 사용하기</a></li>
            <li><a href="#">추가 자료</a></li>
        </ol>
    </li>
    <li><a href="#overview-getting-started"> 스프링 AOP API </a>
        <ol>
            <li><a href="#">스프링에서 포인트컷</a>
                <ol>
                    <li><a href="#">개념</a></li>
                    <li><a href="#">포인트컷에서 작동</a></li>
                    <li><a href="#">AspectJ 포인트컷 표현식</a></li>
                    <li><a href="#">편리한 PointCut 구현</a></li>
                    <li><a href="#">포인트컷 부모클래스</a></li>
                    <li><a href="#">커스텀 포인트컷</a></li>
                </ol>
            </li>
            <li><a href="#">스프링에서의 어드바이스</a></li>
            <li><a href="#">스프링에서의 어드바이저</a></li>
            <li><a href="#">ProxyFactoryBean을 이용하여 AOP 프록시 만들기</a></li>
            <li><a href="#">간결한 프록시 정의</a></li>
            <li><a href="#">ProxyFactory를 이용하여 AOP 프록시 프로그래밍적으로 만들기</a></li>
            <li><a href="#">Advised 객체 조작하기</a></li>
            <li><a href="#">"자동-프록시" 기능 이용하기</a></li>
            <li><a href="#">TargetSource 구현 이용하기</a></li>
            <li><a href="#">새로운 어드바이스 타입 정의하기</a></li>
        </ol>
    </li>
    <li><a href="#overview-getting-started"> 안전한 Null </a>
        <ol>
            <li><a href="#">사용 예시</a></li>
            <li><a href="#">JSR-305 메타 어노테이션</a></li>
        </ol>
    </li>
    <li><a href="#overview-getting-started"> 데이터 버퍼와 코덱 </a>
        <ol>
            <li><a href="#">DataBufferFactory</a></li>
            <li><a href="#">DataBuffer</a></li>
            <li><a href="#">PooledDataBuffer</a></li>
            <li><a href="#">DataBufferUtils</a></li>
            <li><a href="#">Codecs</a></li>
            <li><a href="#">DataBuffer 사용하기</a></li>
        </ol>
    </li>
    <li><a href="#overview-getting-started"> 부록 </a>
        <ol>
            <li><a href="#">XML 스키마</a></li>
            <li><a href="#">XML 스키마 authoring</a></li>
        </ol>
    </li>
</ol>
---

<h1 id="overview"> 스프링 레퍼런스 개요 </h1>

스프링은 Java Enterprise Application을 만들기 쉽게 하였습니다. 스프링은 JVM에서 Java를 대신할 수 있는 언어로 Groovy와 Kotlin에 대한 지원, 어플리케이션이 필요로 하는 다양한 아키텍처를 만들 수 있는 유연성과 함께 기업 환경에 자바를 받아들기 위해 필요한 모든 것을 제공하여 줍니다. 스프링 프레임워크 5.1부터, 스프링은 JDK 8버전 이상 (Java SE 8+)가 필요하며 다른 설정 변경없이 JDK 11 LTS를 지원합니다. Java SE 8 update 60이 스프링이 지원하는 java 8의 가장 낮은 버전이지만 최근 버전을 이용하는 것을 일반적으로 추천합니다.     
스프링은 넓은 범위의 어플리케이션 시나리오를 지원합니다. 큰 기업에서, 어플리케이션은 종종 긴 시간동안 지속되어 왔고 개발자가 통제할 수 없는 업그레이드 주기를 가진 어플리케이션 서버와 JDK에서 실행되어 오곤 했습니다. 다른 경우에는 서버(클라우드 환경 등)에 내장된 한개의 jar로서 배포될 수도 있습니다. 또 다른 경우에는 서버가 필요없는 독립형 어플리케이션(standalone application)일 수도 있습니다.     
스프링은 오픈 소스입니다. 스프링은 다양한 범위의 실사용 케이스로부터 지속적인 의견을 제시해 주는 거대하고 활동적인 커뮤니티를 가지고 있습니다. 이것이 스프링을 긴 시간동안 성공적으로 발전할 수 있도록 하였습니다.
<h2 id="overview-spring"> "Spring"이 의미하는 것 </h2>
"Spring"이라는 용어는 서로 다른 문맥에서 서로 다른 것들을 의미합니다. 스프링 프레임워크가 시작된 스프링 프레임워크 그 자체를 의미하기 위해서 사용될 수 있습니다. 오랜 시간동안, 다른 스프링 프로젝트들이 스프링 프레임워크를 기반으로 세워졌습니다. 주로 사람들이 "Spring"이라고 말할 때, 스프링 전체 집단을 의미합니다. 이 레퍼런스 문서는 기반, 즉 스프링 프레임워크 그 자체에 집중합니다.     
스프링 프레임워크는 여러개의 모듈로 나눠졌습니다. 어플리케이션은 필요로 하는 모듈을 선택할 수 있습니다. 핵심은 의존성 주입([DI](/spring/ioc-di/)) 방법과 구성 모델(configuraion model)을 포함한 코어 컨테이너의 모듈입니다. 추가적으로, 스프링 프레임워크는 메세지, transactional data와 영속성, 웹을 포함한 어플리케이션 아키텍처를 위한 토대를 지원합니다. 또한, Servlet기반의 스프링 MVC 웹 프레임워크와 스프링 WebFlux 반응형 웹 프레임워크 또한 지원합니다.     
모듈에 대한 메모 : 스프링 프레임워크의 jar는 JDK 9의 module path("jigsaw")배포를 허용합니다. jigsaw가 허용된 어플리케이션에서 사용하기 위하여, 스프링 프레임워크 5의 jar는 jar artifact 이름("spring-core", "spring-context" 등등)과 별개로 language level의 모듈 이름("spring.core", "spring.context" 등등)을 정의하는 "자동-모듈-이름" manifest entry를 제공합니다. 물론 JDK 8과 JDK 9의 클래스패스에서 스프링 프레임워크의 jar는 잘 동작합니다.
<h2 id="overview-history"> 스프링과 스프링 프레임워크의 역사 </h2>
스프링은 초기 [J2EE](https://en.wikipedia.org/wiki/Java_Platform,_Enterprise_Edition) 스펙의 복잡함에 대응하여 2003년에 만들어 졌습니다. 누군가는 Java EE와 스프링을 경쟁관계로 보았지만, 사실 스프링은 Java EE를 보완해줍니다. 스프링 프로그래밍 모델은 Java EE 플랫폼 스펙을 받아들이지 않았습니다. 대신 스프링은 EE의 개별 스펙을 신중하게 골라 포함시켰습니다.
* Servlet API ([JSR 340](https://jcp.org/en/jsr/detail?id=340))
* WebSocket API ([JSR 356](https://www.jcp.org/en/jsr/detail?id=356))
* Concurrency Utilities ([JSR 236](https://www.jcp.org/en/jsr/detail?id=236))
* JSON Binding API ([JSR 367](https://jcp.org/en/jsr/detail?id=367))
* Bean Validation ([JSR 303](https://jcp.org/en/jsr/detail?id=303))
* JPA ([JSR 338](https://jcp.org/en/jsr/detail?id=338))
* JMS ([JSR 914](https://jcp.org/en/jsr/detail?id=914))
* 뿐만 아니라 필요하다면, transaction coordination을 위한  JTA/JCA 설정
스프링 프레임워크는 어플리케이션 개발자가 스프링 프레임워크가 제공하는 스프링 고유 스펙 대신 사용할 수 있는 [의존성 주입](/spring/ioc-di/)([JSR 340](https://jcp.org/en/jsr/detail?id=330))과 Common Annotations ([JSR 250](https://jcp.org/en/jsr/detail?id=250)) 스펙 또한 지원합니다.     
스프링 5.0부터, 스프링은 Java EE 7(예시: Servlet 3.1+, JPA 2.1+)을 최소 사양으로 필요합니다. 또한 런타임에 직면할 수 있는 설정 변경없이 더 최신의 Java EE 8 API(예시: Servlet 4.0, JSON Binding API)에 대한 통합도 제공합니다. 이 방법으로 스프링은 Tomcat 8, Tomcat 9, WebSphere 9, JBoss EAP 7과 완전한 호환성을 유지합니다.     
오랜 시간동안 어플리케이션 개발에서 Java EE의 역할은 진화되어 왔습니다. Java EE와 스프링의 초기에는 한 어플리케이션 서버에 배포되기위해 어플리케이션이 만들어 졌습니다. 오늘날, Spring Boot의 도움으로 어플리케이션은 내장 서블릿 컨테이너와 아주 사소한 변경만으로 개발친화적, 클라우드 친화적으로 만들어집니다. 스프링 프레임워크 5부터, WebFlux 어플리케이션은 Servlet API를 직접적으로 사용하지 않고 Servlet Container가 아닌 서버(예시: Netty)에서도 작동이 가능합니다.     
스프링은 지속적으로 진화하고 혁신합니다. 스프링 프레임워크를 넘어서서 Spring Boot, Spring Security, Spring Data, Spring Cloud, Spring Batch 등과 같은 스프링 프로젝트가 있습니다. 각각의 스프링 프로젝트들은 자신만의 코드 저장소, 이슈 트래커, 출시 주기가 있다는 것을 기억하는 것은 중요합니다. 전체 스프링 프로젝트 리스트를 보려면 [spring.io/projects](spring.io/projects)를 보십시오.
<h2 id="overview-philosophy"> 디자인 철학 </h2>
프레임워크에 대하여 배울 때, 프레임워크가 무엇을 하는 지뿐만 아니라 프레임워크가 따르는 원리를 아는 것은 중요합니다. 여기 스프링 프레임워크의 지침이 되는 원리가 있습니다.
* 매 단계에 선택권을 제공하십시오. 스프링은 당신의 디자인 결정을 최대한 늦춰줍니다. 예를 들어, 영속성 제공자를 코드변경없이 전환이 가능합니다. 다른 많은 기반 장치와 써드파티 API와의 통합 또한 마찬가지 입니다.
* 다양한 관점을 수용합십시오. 스프링은 유연성을 받아들이며 어떻게 되어야하는지에 대하여 독선적이지 않습니다. 스프링은 다양한 관점에서 넓은 범위의 어플리케이션 수요를 지원합니다.
* 강력한 하위호환성을 지원하십시오. 스프링의 진화는 버전간의 파괴적인 변화가 거의 업도록 주의깊게 관리합니다. 스프링은 스프링에 의존하는 많은 라이브러리와 어플리케이션의 유지를 촉진하기위하여 주의깊게 선택한 범위의 JDK와 써드파티 라이브러리를 지원합니다.
* API 디자인을 주의깊게 하십시오. 스프링 팀은 직관적이고 긴 시간과 많은 버전동안 지속될 수 있는 API를 만들기위하여 많은 시간과 노력을 투자합니다.
* 코드의 질에 대하여 높은 기준을 설정하십시오. 스프링 프레임워크는 의미있고 정확하며 현행의 자바독에 많은 강조를 합니다. 스프링은 패키지간의 순환의존성이 없는 깨끗한 코드구조를 가지고있다고 말할 수 있는 몇안되는 프로젝트입니다.
<h2 id="overview-feedback"> 사용자 의견과 기여 </h2>
사용법에 대한 질문, 문제진단, 디버깅 문제에 대해서는 StackOverflow를 사용하는 것을 권장합니다. 그리고 사용할 태그가 있는 [questions page](https://spring.io/questions)(역주: 현재 작동하지 않고 있음)도 제공합니다. 스프링 프레임워크에 문제가 있음에 확신하거나 새 기능을 제안하고자 한다면 [GitHub Issues](https://github.com/spring-projects/spring-framework/issues)를 이용해 주십시오.     
생각하고 있는 해결방법이나 수정을 제안하고 싶다면, [GitHub](https://github.com/spring-projects/spring-framework)에 pull request를 제출할 수 있습니다. 하지만 주의하십시오. 가장 사소한 이슈까지 모두, issue tracker에 작성되어서 토론이 진행되고 추후 레퍼런스를 위한 기록이 남겨질수 있도록 하기를 우리는 기대하고 있습니다.
<h2 id="overview-getting-started"> 시작하기 </h2>
스프링에 이제 막 시작하는 단계라면 [Spring Boot](https://projects.spring.io/spring-boot/)기반 어플리케이션을 만들기를 원하실 것입니다. 스프링 부트는 빠른 (강제되는) 방법으로 바로 제작가능한 스프링 기반의 어플리케이션을 만들도록 해줍니다. 스프링 부트는 컨벤션에 맞게 구성된 스프링에 기반하고 있으며 당신을 일으켜세워 가능한한 빠르게 배울수 있도록 디자인 되었습니다.     
[start.spring.io](https://start.spring.io/)에서 기본 프로젝트를 만들어 볼 수도 있고 [Getting Started Building a RESTful Web Servicd](https://spring.io/guides/gs/rest-service/)와 같은 [시작하기 가이드](https://spring.io/guides)중 하나를 따라해 볼 수 있습니다. 이 가이드들은 이해하기 쉬우면서 문제 중심적이고 대부분 스프링 부트에 기반하고 있습니다. 특정 문제를 해결하는데 사용할 수 있는 스프링 포트폴리오에 기반한 다른 프로젝트 또한 포함합니다.