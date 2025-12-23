## UUID란?

> 범용 고유 식별자를 의미하며 중복이 되지 않는 유일한 값을 구성하고자 할때 주로 사용이 되는 고유 식별자를 의미

## UUID의 구조

> UUID는 16바이트(128비트) 형태의 구조를 가지며 하나의 **UUID 길이는 36자리이며 “4개의 하이픈(-)”과 “32개의 16진수 문자열”로 구성**이 되어있습니다.

![image.png](attachment:d143a4be-7d57-434f-a5a1-3d7085986f89:image.png)

## 메서드의 종류

|**메서드**|**설명**|
|---|---|
|static UUID randomUUID()|무작위 UUID를 생성합니다.|
|static UUID fromString(String uuid)|주어진 UUID 문자열로부터 UUID를 생성합니다.|
|long getLeastSignificantBits()|이 UUID의 가장 낮은 64비트를 반환합니다.|
|long getMostSignificantBits()|UUID의 가장 높은 64비트를 반환합니다.|
|int compareTo(UUID val)|UUID와 주어진 UUID를 비교합니다.|
|boolean equals(Object obj)|UUID와 주어진 객체가 같은지 여부를 반환합니다.|
|String toString()|UUID를 문자열로 반환합니다.|

## 예시 코드

```java
public String createRefreshToken(String username) { // 리프레쉬 토큰 발급
        Date now = new Date();

        String refreshToken = Jwts.builder()
                .setSubject(username)
                .claim("type", "refresh")
                .setIssuedAt(now)
                .setExpiration(new java.sql.Timestamp(now.getTime() + jwtProperties.getRefreshExpiration() * 1000))
                .signWith(SignatureAlgorithm.HS256, jwtProperties.getSecret())
                .compact();

//                refreshTokenRepository.save( // refreshToken, accessToken을 둘 다 주는 방식에서 사용
//                        RefreshToken.builder()
//                                .username(username)
//                                .token(refreshToken)
//                                .timeToLive(jwtProperties.getRefreshExpiration())
//                                .build()
//                );
                refreshTokenRepository.save(
                        RefreshToken.builder()
                                .sessionId(UUID.randomUUID().toString())
                                .username(username)
                                .token(refreshToken)
                                .timeToLive(jwtProperties.getRefreshExpiration())
                                .build()
                );

                return refreshToken;
    }
```

- 위와 같이 ranmod한 UUID를 만들어서 refreshToken과 세션아이디로 구분할 수 있다.
- 또한 toString을 달아놓아서 문자열의 형태로 저장된다.