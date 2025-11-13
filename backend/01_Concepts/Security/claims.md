## claims 객체란?

> Claims란 JWT 를 사용할때 사용되는 객체로 토큰 안에 Payload 부분에 정보를 담는 객체

### 예시 코드

```java
public interface Claims extends Map<String, Object>, Identifiable {
    String ISSUER = "iss"; // 토큰 발급자
    String SUBJECT = "sub"; // 토큰 제목
    String AUDIENCE = "aud"; // 토큰 대상자
    String EXPIRATION = "exp"; // 토큰 만료 시간
    String NOT_BEFORE = "nbf"; // 토큰 처리를 위해 수락되서는 안되는 시간
    String ISSUED_AT = "iat"; // 토큰 발행 시간
    String ID = "jti"; // 고유 식별자 

    String getIssuer();

    String getSubject();

    Set<String> getAudience();

    Date getExpiration();

    Date getNotBefore();

    Date getIssuedAt();

    String getId();

    <T> T get(String var1, Class<T> var2);
}
```

- 보는것과 같이 발급자, 제목 등등 토큰에 내용을 채우는 주가되는 정보를 담는 객체이다.