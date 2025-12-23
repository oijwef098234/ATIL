## ResponseEntity란?

> 컨트롤러가 보낼 HTTP 응답을 설정하는 객체

## ResponseEntity 의 구성요소

`ResponseEntity<T>`는 그냥 이 3개를 묶은 객체다.

### 1. Status Code

```java
ResponseEntity.ok()// 200
ResponseEntity.status(401)// 401
ResponseEntity.badRequest()// 400
```

---

### 2. Headers

```java
.header("Authorization","Bearer token")
.header("X-Test","123")
```

---

### 3. Body

```java
.body(dto)
```

이걸 합치면:

```
HTTP/1.1 200 OK
Authorization: Bearer token
Content-Type: application/json
{
	dto
}
```

이런 느낌으로 요청이 보내진다.