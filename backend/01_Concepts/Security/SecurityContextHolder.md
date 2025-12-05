## SecurityContextHolder란?

> 현재 로그인한 사용자의 보안 컨텍스트를 들고 있는 객체

### Security의 로그인 순서

1. 사용자가 로그인 요청 (`/login` 등)
2. `AuthenticationManager`가 아이디·비밀번호 확인
3. 인증 성공 시 `Authentication` 객체 생성, 만약에 실패시 비워짐
4. 그 `Authentication` 객체를 `SecurityContext`에 저장
5. `SecurityContextHolder`가 그 `SecurityContext`를 들고 있음

### 예시 코드

```java
package com.example.springsecurity.global.security.jwt;

import com.example.springsecurity.domain.user.exception.ExpiredTokenException;
import com.example.springsecurity.domain.user.exception.InvalidTokenException;
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import lombok.RequiredArgsConstructor;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.web.filter.OncePerRequestFilter;

import java.io.IOException;

@RequiredArgsConstructor
public class JwtTokenFilter extends OncePerRequestFilter {
    private final JwtTokenProvider jwtTokenProvider;

    @Override
    public void doFilterInternal(HttpServletRequest request,
                                 HttpServletResponse response,
                                 FilterChain filterChain) throws IOException, ServletException {

        String token = jwtTokenProvider.resolveToken(request);

        if (token == null) { // 토큰이 있으면
            filterChain.doFilter(request, response);
            return;
        }

        try {
            Authentication authentication = jwtTokenProvider.getAuthentication(token);
            SecurityContextHolder.getContext().setAuthentication(authentication);
        } catch (ExpiredTokenException | InvalidTokenException e) {
            request.setAttribute("jwtError", e);
        }

        filterChain.doFilter(request, response);
    }
}
```

- 위와 같이 authentication객체를 SecurityContext에 넣어서
- SecurityContextHolder가 관리함