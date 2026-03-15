## @Pattern이란?

> 주어진 입력 값을 정규 표현식과 일치하는지 확인하는 어노테이션

## 왜 사용하냐?

- 잘못된 입력값에 대해서 정확한 에러 메시지 표현 가능
- 정규 표현식만으로 검증 가능
- 다양한 정규표현식 표현 가능

## 사용 예시

```java
import javax.validation.constraints.Pattern;

public class UserDTO {

    @Pattern(regexp = "^[a-zA-Z0-9_]{6,20}$", message = "Username must be between 6 and 20 characters and can only contain letters, numbers, and underscores.")
    private String username;
}
```

- 다음과 같이 표현할 수 있다.
- 정규표현식은 많이 사용면서 익숙해지는것이 중요할 것 같다.