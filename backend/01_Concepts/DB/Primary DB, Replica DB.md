## Primary DB란

> 서버에서 쓰기 요청을 보내는 DB

- 데이터가 직접적으로 추가, 수정, 삭제가 됨

## Replica DB란

> Primary DB를 실시간으로 백업받는 DB

- 백업용 DB
- Primary DB가 죽어버리는 상황이 발생하면 Replica DB가 Primary DB로 승격이 됨.

---

## Primary가 죽으면?

예를 들어 Primary DB가 있는 서버가 죽음.

```
Primary DB 사망
↓
app들이 DB 접속 실패
```

이때 해야 할 일은:

```
1. Replica가 최신 데이터까지 따라왔는지 확인
2. Replica의 복제 연결을 끊음
3. Replica를 쓰기 가능한 DB로 바꿈
4. app의 DB_URL을 새 Primary로 변경
5. app 재시작
6. 기존 Primary는 함부로 다시 붙이지 않음
```

#### 이걸 Replica DB 승격이라고 함

---

## 다시 A DB가 살아나면?

A가 다시 살아났다고 해서 바로 붙이면 안 됨.

왜냐면 장애 중에 B가 새 Primary가 되어 데이터를 받았기 때문.

```
A DB는 예전 데이터
B DB는 최신 데이터
```

이 상태에서 A를 그냥 붙이면 데이터 꼬임.

그래서 보통은:

```
1. B를 새 Primary로 인정
2. A의 데이터를 버리거나 백업
3. A를 B의 Replica로 다시 구성
```

이렇게 함.

즉 역할이 바뀜.

```
이전:
A Primary → B Replica

장애 후:
B Primary → A Replica
```