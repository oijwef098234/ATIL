# @MappedSuperclass란?

> **여러 엔티티 클래스가 공통으로 사용하는 매핑 정보(필드)를 상속받아 재사용할 때 사용하는 어노테이션이다.**

## 왜 사용하냐?

- 계속 같은 필드를 사용하면 불편하니까 그냥 같은걸로 묶어서 사용하는거임

## 예시 코드

```java
package com.example.gujeuck_server.global.entity;

import jakarta.persistence.*;
import lombok.Getter;

@Getter
@MappedSuperclass
public class BaseIdEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
}
```