이 개념을 이해하려면 Authentication이 뭔지 알아보자

## Authentication이란?

> `Authentication` 객체는 “로그인 결과물”이라고 보면 됨

### 예시

- `username` : "gimjeongug"
- `password` : "암호화된 비밀번호"
- `roles` : ["USER"]

이런 정보가 들어있는 인증 객체임

---

## AuthenticationManager란?

> 로그인 정보를 받아서 진짜 인증할 수 있는지 확인한 뒤 인증 성공 시 Authentication 객체를 만들어준다