## DDD란?

> 도메인 주도 설계

## 왜 사용하냐?

- 도메인에 규칙을 잘 정해놓으면 Fat Service를 방지할 수 있다.
- 객체의 의미와 상태가 명확하다.
- 규칙이 흩어지지 않는다.

---

## 예시 코드

```java
@Entity
public class VisitLog {

    private static final int MAX_COMPANION_COUNT = 4;

    @ManyToOne
    private User leader;

    private String purpose;

    private int maleCount;

    private int femaleCount;

    @ElementCollection
    private List<Long> companionIds = new ArrayList<>();

    protected VisitLog() {
    }

    public static VisitLog checkIn(
            User leader,
            String purpose,
            int maleCount,
            int femaleCount
    ) {
        return new VisitLog(leader, purpose, maleCount, femaleCount);
    }

    private VisitLog(User leader, String purpose, int maleCount, int femaleCount) {
        validateLeader(leader);
        validatePurpose(purpose);
        validatePeopleCount(maleCount, femaleCount);

        this.leader = leader;
        this.purpose = purpose;
        this.maleCount = maleCount;
        this.femaleCount = femaleCount;
    }

    public void changePeopleCount(int maleCount, int femaleCount) {
        validatePeopleCount(maleCount, femaleCount);

        this.maleCount = maleCount;
        this.femaleCount = femaleCount;
    }

    public void changePurpose(String purpose) {
        validatePurpose(purpose);
        this.purpose = purpose;
    }

    public void addCompanion(Long companionId) {
        if (companionIds.size() >= MAX_COMPANION_COUNT) {
            throw new IllegalArgumentException("동행인은 최대 4명입니다.");
        }

        companionIds.add(companionId);
    }

    private void validateLeader(User leader) {
        if (leader == null) {
            throw new IllegalArgumentException("리더 방문자는 필수입니다.");
        }
    }

    private void validatePurpose(String purpose) {
        if (purpose == null || purpose.isBlank()) {
            throw new IllegalArgumentException("방문 목적은 필수입니다.");
        }
    }

    private void validatePeopleCount(int maleCount, int femaleCount) {
        if (maleCount < 0 || femaleCount < 0) {
            throw new IllegalArgumentException("인원 수는 음수일 수 없습니다.");
        }

        if (maleCount + femaleCount <= 0) {
            throw new IllegalArgumentException("방문 인원은 최소 1명 이상입니다.");
        }
    }
}
```

```java
@Service
public class VisitService {

    public void checkIn(CheckInRequest request) {
        User leader = userRepository.findByNameAndBirthYmd(
                request.getName(),
                request.getBirthYmd()
        ).orElseGet(() -> userRepository.save(
                new User(request.getName(), request.getBirthYmd())
        ));

        VisitLog visitLog = VisitLog.checkIn(
                leader,
                request.getPurpose(),
                request.getMaleCount(),
                request.getFemaleCount()
        );

        for (Long companionId : request.getCompanionIds()) {
            visitLog.addCompanion(companionId);
        }

        visitLogRepository.save(visitLog);
    }
}
```

- 위와 같이 도메인이 검증, 규칙을 담당하고 service는 비교적 줄어듦
- 해당 객체가 여러곳에서 사용 되도 규칙이 흩어지지 않음

---

## 왜 전부 다 사용하지 않는걸까?

- 무조건 좋은 설계는 없음, 그렇다고 무조건 나쁜 설계도 없음
- DDD는 확장성을 고려한 설계라고 볼 수 있음
- 작은 프로젝트에는 비추