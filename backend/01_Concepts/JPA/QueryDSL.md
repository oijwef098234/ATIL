## **QueryDSL이란?**

> QueryDSL의 목적은 JPQL 생성이다.

- 기존 JPQL의 단점을 보완

## 왜 사용하냐?

- JPQL : 조건이 많이 붙거나 조회하는 값이 많을경우 오타가 날 수도 있고 유지보수, 가독성 측면이 많이 부족함
- **QueryDSL : 자동완성 가능, 코드적으로 접근하여 Q타입클래스를 통해 쉽게 DB에 접근 가능, SQL을 코드로써 짤 수 있다는 큰 장점**

---

### 기존 Repository

```java
package com.example.gujeuck_server.domain.purpose.domain.repository;

import com.example.gujeuck_server.domain.purpose.domain.Purpose;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;

import java.util.List;
import java.util.Optional;

public interface PurposeRepository extends JpaRepository<Purpose, Long>, PurposeRepositoryCustom { // 여기서 customReopsitory를 상속 받는다.

    Optional<Purpose> findByPurposeName(String purpose);

    List<Purpose> findAllByOrderByPurposeIndexAsc();

    List<Purpose> findAllByPurposeIndexGreaterThan(int purposeIndex);
}
```

### 새롭게 만든 CustomRepository

```java
package com.example.gujeuck_server.domain.purpose.domain.repository;

public interface PurposeRepositoryCustom {
    int findMaxPurposeIndex();
}
```

### 핵심 Query DSL

```java
package com.example.gujeuck_server.domain.purpose.domain.repository;

import com.example.gujeuck_server.domain.purpose.domain.QPurpose;
import com.querydsl.jpa.impl.JPAQueryFactory;
import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor
public class PurposeRepositoryCustomImpl implements PurposeRepositoryCustom {
    private final JPAQueryFactory jpaQueryFactory;
    private final QPurpose qPurpose = QPurpose.purpose; // Q타입 생성하여 .select(qPurpose.purposeIndex.max())로 적을 수 있게 함
																												// 없다면 .select(QPurpose.purpose.purposeIndex.max()) 와 같이 값이 많아지면 가독성이 떨어짐
    @Override
    public int findMaxPurposeIndex() { // 지금은 필드를 오버라이드 했지만 보통 메서드를 한다.
        Integer max = jpaQueryFactory
                .select(qPurpose.purposeIndex.max()) // select문
                .from(qPurpose) // from
                .fetchOne(); // 단 하나의 값만 가져온다.

        return max != null ? max : 0; // NPE 방지용 코드
    }
}
```