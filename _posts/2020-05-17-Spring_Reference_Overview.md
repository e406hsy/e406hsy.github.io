---
layout: post
title:  "[Spring Reference] 스프링 레퍼런스 #0 개요"
createdDate:   2020-05-17T13:04:00+09:00
date:   2020-05-17T13:04:00+09:00
pagination: enabled
author: SoonYong Hong
categories: Spring_Reference
tags: Spring Reference
---

이 내용은 [스프링 문서 5.2.6.RELEASE](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/overview.html#overview)를 번역한 내용으로 오역이 있을 수 있습니다.

# 목차
<h2><a href="#overview"> 스프링 레퍼런스 개요 </a></h2>
* <a href="#overview-spring"> "Spring"이 의미하는 것 </a>
* <a href="#overview-history"> 스프링과 스프링 프레임워크의 역사 </a>
* <a href="#overview-history"> 디자인 철학 </a>
* <a href="#overview-history"> 사용자 의견과 기여 </a>
* <a href="#overview-getting-started"> 시작하기 </a>

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

---

다음 : [Spring Reference Core](../spring-reference-core-1-beans)