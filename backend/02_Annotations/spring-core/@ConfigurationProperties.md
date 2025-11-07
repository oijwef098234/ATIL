## @ConfigurationProperties란?

> yml또는 propotise에 있는 설정들을 자바 객체로 매핑해주는 어노테이션

### 예시 코드

```java
package com.example.springsecurity.global.security.jwt;

import lombok.Getter;
import org.springframework.boot.context.properties.ConfigurationProperties;

@Getter
@ConfigurationProperties(prefix = "jwt")
public class JwtProperties {
    private final String secret;
    private final long accessExpiration;
    private final long refreshExpiration;
    private final String header;
    private final String prefix;

    public JwtProperties(String secret, long accessExpiration, long refreshExpiration, String header, String prefix) {
        this.secret = secret;
        this.accessExpiration = accessExpiration;
        this.refreshExpiration = refreshExpiration;
        this.header = header;
        this.prefix = prefix;
    }
}
```

- jwt로 시작하는 설정을 찾아 동일한 객체에 매핑한다.