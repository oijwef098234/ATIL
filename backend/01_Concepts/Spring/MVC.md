## MVC란?
> Model–View–Controller의 약자로, 역할을 3개로 쪼개서 개발하는 방식을 말한다

### 구조 예시

### Model

- Entity, DTO, 비즈니스 로직
- DB와 데이터 담당

### View

- 반환할 응답(JSON/HTML) 생성 단계
- REST API에서는 `@RestController` + Jackson이 담당

### Controller

- 요청 받고
- Model에게 로직 시키고
- View에 결과 넘기는 조정자