## @Pattern이란?

> DTO에 들어오는 값의 형식을 검증하는 어노테이션

## 왜 사용하냐?

> 해당 어노테이션이 없으면 따로 로직 내부에서 If문으로 코드를 더럽게 만든다.

## 어떻게 사용하냐?

```java
public record SignUpRequest(

    @Pattern(
        regexp = "^[a-zA-Z0-9]{5,20}$",
        message = "아이디는 영문과 숫자 5~20자여야 합니다."
    )
    String userId

) {}
```

- 이런식으로 패턴을 정해두어 그 형식에 맞는 값을 넘어가게 만든다.
- 만약 형식에 어긋날 경우 message에서 에러메시지를 띄운다.

### `regexp` (필수)

정규표현식.

```java
regexp ="^[0-9]+$"
```

숫자만 허용.

---

### `message`

검증 실패 시 메시지.

---

### `flags`

대소문자 무시 같은 옵션.

```java
@Pattern(
    regexp = "abc",
    flags = Pattern.Flag.CASE_INSENSITIVE
)
```

---

# 자주 쓰는 표현식

## 이메일

```java
^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+$
```

## 숫자만

```java
^[0-9]+$
```

## 영문만

```java
^[a-zA-Z]+$
```

## 비밀번호 (8자 이상, 영문+숫자)

```java
^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$
```