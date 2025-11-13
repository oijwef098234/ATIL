## UsernamePasswordAuthenticationFilter란?

> UsernamePasswordAuthenticationFilter는 Spring Security에서 제공되는 보안 필터 중 하나

- 폼 기반의 로그인을 처리하며, HTTP POST 요청을 통해 전송된 사용자의 아이디와 비밀번호를 기반으로 인증을 수행한다

### 구성요소

- principal : 인증 주체를 나타내는 객체 (일반적으로 사용자의 username 또는 email)
- credentials : 인증 주체의 자격 증명 (일반적으로 비밀번호)
- authorities : 사용자의 권한 목록을 나타내는 객체

### 사용 flow

- 사용자가 입력한 사용자명(username)과 비밀번호(password)로 Authentication 객체를 생성하여 인증 프로세스에 활용됩니다.
- 보통 사용자가 로그인하는 경우에 사용자명과 비밀번호를 받아 UsernamePasswordAuthenticationToken을 생성하여 인증 매니저에게 전달합니다