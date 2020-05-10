---
layout: post
title:  "[Spring Boot] JPA 환경 설정하기"
createdDate:   2020-05-10T15:24:00+09:00
date:   2020-05-10T15:24:00+09:00
pagination: enabled
author: SoonYong Hong
categories: SpringBoot
tags: JPA Java Persistence Spring
---
# Spring Boot에서 JPA 이용하기
---

## Spring Boot란 무엇인가
Spring Framework는 Java Application 개발에서 EJB에 종속되어 자바의 객체 지향 프로그래밍을 이용하지 못하는 한계를 벗어나고자 만들어진 Framework입니다.     
이 Spring Framwork를 이용하는데 있어서 복잡한 환경설정을 손쉽게 해결하고자 자동으로 환경설정을 진행해주는 Spring Boot가 만들어졌습니다.

---

## JPA란 무엇인가
Java Persistence Api는 자바의 객체(Object)와 관계형 데이터베이스의 데이터(Relation)간의 연결(Mapping)을 생성하여 Database와 Sql문에 종속되지 않고 객체 관계그래프를 이용한 객체 탐색을 가능하게 해주는 Obejct Relation Mapping(ORM)을 지원하는 Interface로 Java ORM 개발자들은 해당 인터페이스를 이용할 수 있도록 ORM을 개발합니다.     
가장 대표적인 JPA로는 hibernate 등이 있습니다.

---

## Spring Boot에 JPA 환경 설정하기

### Dependency 추가하기
가장 먼저 maven이나 gradle에 JPA dependency를 추가해줍니다.     

#### maven 예제
**Spring Boot JPA dependency**
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```
**JPA 구현체 dependency**     
이 부분은 원하는 다른 구현체로 변경이 가능합니다.
```
<dependency>
	<groupId>org.hibernate</groupId>
	<artifactId>hibernate-entitymanager</artifactId>
</dependency>
```
**JDBC 구현체 dependency**     
이 부분은 원하는 다른 Database에 맞는 JDBC로 변경이 가능합니다.
```
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<scope>runtime</scope>
</dependency>
```
---
### Spring Boot와 Database 연결 설정하기
application.properties에 아래의 내용을 추가합니다.
```
# DB 연결 설정
spring.datasource.url={your datasource url}
# example
# spring.datasource.url=jdbc:mysql://localhost:3306/website?characterEncoding=UTF-8&serverTimezone=UTC
spring.datasource.username={your username}
spring.datasource.password={your password}

# jpa 설정
# 사용할 sql문 종류 설정 - 이 부분은 선택한 JPA 구현체와 DBMS의 종류에 따라 달라집니다.
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```
---

### JPA Entity 만들기

#### 예제
아주 간단하게 ID, UserName, Password만 가지고 있는 Entity Class를 만들었습니다.
```java
@Entity
@Getter
@Setter
@NoArgsConstructor
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column
    private long Id;
    @Column(name = "name")
    private String userName;
    @Column
    private String password;
}
```
---
### JPA repository 만들기
Spring Boot에서 org.springframework.data.jpa.repository.JpaRepository<{Entity 객체}, {Primary Key}>를 상속한 interface를 만든 뒤, @Repository 어노테이션을 이용하면 Spring Boot에서 JPA Repository 구현체를 만들어 bean으로 등록해줍니다.
#### 예제
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
	Optional<User> findByUserName(String userName);
}
```
#### 참고 : findByUserName 메소드를 call 했을 경우, 사용되는 sql query이다.
```sql
select user0_.id as id1_0_, user0_.password as password2_0_, user0_.name as name3_0_ from user user0_ where user0_.name=?
```

### JPA repository 사용하기
이제 bean으로 등록된 JPA Repository를 DI하여 사용하면 된다.     
`findById`, `save`와 같은 JpaRepository 인터페이스에 정의된 메소드를 사용하여도 좋고 `findByUserName`과 같이 새로 만든 메소드를 사용할 수도 있다.     
단, 새로 만든 메소드의 경우에는 method이름을 Jpa에서 자동적으로 sql query를 만들어 줄 수 있는 형태로 정하여야 한다. 
```java
@Component
public class JpaUserDao {
    @Autowired
    private UserRepository userRepository;

    public Optional<User> getUserByid(long id) {
        return userRepository.findById(id);
    }

    public Optional<User> getUserByUserName(String userName) {
        return userRepository.findByUserName(userName);
    }

    public void insert(User user) {
        userRepository.save(user);
    }
}
```