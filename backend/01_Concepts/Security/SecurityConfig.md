## SecurityConfig란?

> Jwt 인증 방식에 있어서 보안에 관한 모든 설정을 관할하는 클래스

### 예시 코드

```java
package com.example.springsecurity.global.config.jwt;

import com.example.springsecurity.global.security.jwt.JwtAuthenticationEntryPoint;
import com.example.springsecurity.global.security.jwt.JwtTokenFilter;
import com.example.springsecurity.global.security.jwt.JwtTokenProvider;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
import org.springframework.security.config.annotation.web.configurers.HeadersConfigurer;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@RequiredArgsConstructor
public class SecurityConfig {
    private final JwtAuthenticationEntryPoint jwtAuthenticationEntryPoint;
    private final JwtTokenProvider jwtTokenProvider;

    @Bean
    protected SecurityFilterChain configure(HttpSecurity httpSecurity) throws Exception {
        return httpSecurity
                .csrf(AbstractHttpConfigurer::disable) // csrf 공격 방어
                // cors 설정은 프론트엔드가 없기때문에 필요 없음
                .headers(headers -> headers // 클릭 제킹 공격 방어
                        .frameOptions(HeadersConfigurer.FrameOptionsConfig::sameOrigin))
                .sessionManagement(session -> session // 세션 사용 설정
                        .sessionCreationPolicy(SessionCreationPolicy.STATELESS)) // 사용 안함
                .authorizeHttpRequests(auth -> auth
                        .requestMatchers("/user/**").permitAll() // 사용자 경로 권한 설정
                        .requestMatchers("/admin/sign-up", "/admin/login").permitAll() // 관리자 경로 권한 설정
                        .requestMatchers("/admin/**").hasRole("ADMIN")) // 관리자 경로 권한 설정
                .exceptionHandling(exception -> exception
                        .authenticationEntryPoint(jwtAuthenticationEntryPoint))
                .addFilterBefore(
                        new JwtTokenFilter(jwtTokenProvider),
                        UsernamePasswordAuthenticationFilter.class
                )
                .build();
    }
}
```

- 위코드를 하나하나 분석해보자

---

## `.csrf(AbstractHttpConfigurer::disable)`

> CSRF 공격을 비활성화 한다.

### AbstractHttpConfigurer란?

> HttpSecurity에 있는 기능들이 상속받은 클래스

- 따라서 .disable같은 기능도 `AbstractHttpConfigurer::disable` 이런 식으로 사용이 된다.
- 위의 코드의 의미는 CSRF의 공격을 비활성화한다는 의미이다.

---

### `.headers(headers -> headers.frameOptions(HeadersConfigurer.FrameOptionsConfig::sameOrigin))`

> 클릭 제킹 공격을 방어한다.

- `.headers` : header관련 설정을 한다.
- `.frameOptions`: 헤더 설정 중에서도 **`X-Frame-Options`** 라는 헤더를 설정하는 부분
- `HeadersConfigurer` : header를 설정하는데 도움을 주는 클래스
- `FrameOptionsConfig` : HeadersConfigurer중에서도 따로 **`X-Frame-Options`** 관련 설정을 한다.
- `sameOrigin` : `X-Frame-Options: SAMEORIGIN` 이라고 응답 헤더가 나가게 돼.
    - 이 페이지를 iframe으로 띄울 수는 있는데, 같은 도메인에서 띄울 때만 허용한다.

---

### `.sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))`

> 쿠키 세션 관련 설정

- 에초에 JWT를 사용함으로 그냥 쿠키세션 관련을 비활성화 한다는 의미를 내포한다.

---

### `.authorizeHttpRequests`

> URL관련 인가 설정

- `.requestMatchers` : 해당 URL을 설정해라
    - ex) `.requestMatchers("/user/**").permitAll()`
    - 여기서 `.permitAll()` 은 권한이 필요하지 않고 자유롭게 모두 허용한다는 의미이다.
    - `.hasRole("ADMIN")` : 권한을 가져오는데 `ADMIN`인 권한을 가져와라

---

### `.exceptionHandling`

> 만약 시큐리티에서 예외가 발생했을때 여기로 들어오세요 같은 느낌

- `.authenticationEntryPoint` : 인증이 되지 않은 상태에서 자원에 접근했을때 발생시켜라
    - jwtAuthenticationEntryPoint이 클래스를 발생시켜라

### `.addFilterBefore(A, B)`

> B의 필터가 실행되기 전에 A필터를 넣어서 먼저 실행하자

- `new JwtTokenFilter(jwtTokenProvider)` : A, 즉 새로운 필터 객체
- `UsernamePasswordAuthenticationFilter.class` : B, A 가이 필터를 기준으로 앞에 들어옴