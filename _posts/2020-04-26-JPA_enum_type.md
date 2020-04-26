---
layout: post
title:  "[JPA] Java Enum Type 이용하기"
createdDate:   2020-04-26T13:13:00+09:00
date:   2020-04-26T13:13:00+09:00
pagination: enabled
author: SoonYong Hong
categories: jpa
tags: JPA Java Persistence
---
# JPA에서 Java Enum Type 이용하기
---

## Enum Type
Jave의 Enum Type은 상수의 그룹을 나타 낼때 사용된다.
```java
public enum Color {
    RED, GREEN, BLUE;
}
```

---

## JPA에서 Java Enum Type 이용하기
DataBase에서 특정 몇개의 값을 가지는 Attribute가 존재한다면 JPA의 해당 Entity에 field로 Enum Type을 사용하여 나타낼 수 있다.

---

#### 아래와 같은 데이터 베이스를 가정하여보자

| Table Name | User |
| Attribute Name | id | user_name | password | role |
| Attribute Description | P.K |  |  | F.K |
|:---|:---|:---|:---|:---|
| Row 1 | 0 | user1 | 1q2w3e4r | 0 |
| Row 2 | 1 | user2 | password | 0 |
| Row 3 | 2 | admin1 | qwerty | 1 |

| Table Name | Role |
| Attribute Name | id | detail |
| Attribute Description | P.K |  |
|:---|:---|:---|
| Row 1 | 0 | USER |
| Row 2 | 1 | ADMIN |

이 테이블을 JPA Entity로 나타내면 다음과 같다.

```java
@Entity
@Getter
@Setter
@NoArgsConstructor
public Class User {
    @Id
    @Column
    @GeneratedValue(strategy = GenerationType.AUTO)
    private long id;
    @Column(name="user_name")
    private String userName;
    @Column
    private String password;
    @OneToOne
    private Role role;
}
```

```java
@Entity
@Getter
@Setter
@NoArgsConstructor
public Class Role {
    @Id
    @Column
    @GeneratedValue(strategy = GenerationType.AUTO)
    private int id;
    @Column
    private String detail;
}
```

이런 식으로 구현을 하면 User 객체를 가져오면서 Role또한 함께 가져올 수 있다.     
하지만 매번 User 객체를 가져올 때, Role을 Join해서 가져온다.     
또한 필요한 Role 객체는 `id=0 detail="USER"`인 객체와 `id=1 detail="ADMIN"`인 객체 단 두개 뿐이지만 매 User마다 하나 씩의 Role 객체를 생성하여 참조하게 된다.     
이 문제를 해결하기 위해 Java Enum Type을 이용할 수 있다.

---

```java
@Entity
@Getter
@Setter
@NoArgsConstructor
public Class User {
    @Id
    @Column
    @GeneratedValue(strategy = GenerationType.AUTO)
    private long id;
    @Column(name="user_name")
    private String userName;
    @Column
    private String password;
    @Enumerated(EnumType.ORDINAL)
    private Role role;
}
```
```java
public enum Role {
    USER, ADMIN;
}
```

위 와 같이 @Enumerated 어노테이션을 이용하면 EnumType을 사용 할 수 있다.     
EnumType.ORDINAL로 설정하면 Enum Type의 순서가 DB에 저장된다. [`0`, `1`]     
EnumType.String으로 설정하면 Enum Type의 이름이 DB에 저장된다. [`USER`, `ADMIN`]     

---

#### name도 순서도 아닌 또다른 값을 이용해야 한다면?
같은 Enum Type을 사용하지만 DB에는 다른 값이 들어가는 테이블이 있다면 어떻게 해야할까?


```java
@Getter
public enum Role {
    USER("ROLE_USER"), ADMIN("ROLE_ADMIN");

    private String customType;

    Role (String customType){
        this.customType = customType;
    }
}
```

조건에 따라 [`0`, `1`]나 [`USER`, `ADMIN`]이 아닌 [`ROLE_USER`, `ROLE_ADMIN`]을 DB에 저장해야 하는 경우에는 다음과 같이 해결할 수 있다.

```java
@Entity
@Getter
@Setter
@NoArgsConstructor
public Class User {
    @Id
    @Column
    @GeneratedValue(strategy = GenerationType.AUTO)
    private long id;
    @Column(name="user_name")
    private String userName;
    @Column
    private String password;
    @Convert(converter = roleConverter.class)
    private Role role;
}
```
```java
@Converter
public class myConverter implements AttributeConverter<Role, String>{

    @Override
    public String convertToDatabaseColumn(Role attribute) {
        /* 객체에서 DB column으로 변경 */
        if (conditionTest(attribute)){
            return attribute.name();
        }
        return attribute.getCustomType();
    }

    @Override
    public Role convertToEntityAttribute(String dbData) {
        /* DB column에서 객체로 변경 */
        if (conditionTest(dbDate)){
            return Role.valueOf(dbData);
        }
        for (Role role : Role.values()){
            if (role.getCustomType().equals(dbData)){
                return role;
            }
        }

        throw new NoSuchRoleTypeException(dbData);
    }
}
```

위와 같이 AttributeConverter<변경을 원하는 객체, DB에 저장될 데이터>를 구현하여 해결할 수 있다.     
AttributeConverter는 Enum Type뿐만 아니라 객체에도 사용할 수 있다.