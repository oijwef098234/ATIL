## HttpSecurity란?

> 웹 보안 관련 설정 중심 객체

### 역할 요약

|기능|예시 설정|
|---|---|
|인증(Authentication)|로그인, 세션, 토큰, 사용자 정보 등|
|인가(Authorization)|`/admin/**`는 관리자만, `/user/**`는 로그인한 유저만 등|
|공격 방어|CSRF, CORS, XSS 같은 웹 공격 방어 설정|
|로그인 방식|formLogin, httpBasic, JWT 기반 등|
|로그아웃 처리|로그아웃 URL, 세션 무효화 등|
|커스텀 필터 추가|SecurityFilterChain에 나만의 필터 삽입|

이런식으로 웹과 관련된 보안 설정들을 다 여기서 관리한다.