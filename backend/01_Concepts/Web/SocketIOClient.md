## SocketIOClient란?

> 웹소켓에 연결된 사용자를 나타내는 객체

- 웹소켓 서버 안에는 다음과 같은 구조로 클라이언트들이 존재함

```
SocketIOServer
   ├─ SocketIOClient (user1)
   ├─ SocketIOClient (user2)
   ├─ SocketIOClient (user3)
```

## 사용 예시

- 웹소켓에 연결된 하나의 클라이언트를 나타내는거기 때문에 다음과 같이 사용한다.

```java
@OnEvent("chat")
public void sendMessage(SocketIOClient client, ChatDto message) {
}
```

- client에는 메시지를 보낸 사용자의 웹소켓 연결 정보가 담겨있음.
- sessionId
- handshake 정보
- query parameter
- room 정보
- 연결 상태 등등

## socketIOServer란?

> 웹소켓 서버 본채

- 모든 클라이언트 객체들이 여기로 연결됨