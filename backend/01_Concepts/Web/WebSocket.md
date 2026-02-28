## WebSocket이란?

> 클라이언트와 서버가 한 번 연결되면, 그 연결을 계속 유지하면서 서로 마음대로 데이터를 주고받을 수 있게 해주는 통신 프로토콜, 방식임, 서버와 연결하는거임

## 특징

- 한번만 연결하여 통신(연결 유지)
- 서버도 먼저 요청을 보낼 수 있음
- 실시간 통신 가능

---

## 채팅기능 같은경우?

인스타를 예로 들자, 인스타에 들어가자마자 클라이언트와 서버가 웹소켓으로 연결됨.

A사용자 → 서버(웹소켓으로 연결)

B사용자 → 서버(웹소켓으로 연결)

이 둘이 채팅을 치기 위해서 A가 서버로 요청을 보내면 실시간으로 서버가 B사용자에게 요청을 보낸다.

---

## 예시 코드

```java
package com.example.socket.domain.chat.presentation;

import com.corundumstudio.socketio.SocketIOClient;
import com.corundumstudio.socketio.SocketIOServer;
import com.corundumstudio.socketio.annotation.OnEvent;
import com.example.socket.domain.chat.presentation.dto.request.CreateRoomRequest;
import com.example.socket.domain.chat.presentation.dto.request.JoinRoomRequest;
import com.example.socket.domain.chat.presentation.dto.request.SendChatRequest;
import com.example.socket.domain.chat.service.CreateRoomService;
import com.example.socket.domain.chat.service.JoinRoomService;
import com.example.socket.domain.chat.service.SendChatService;
import lombok.RequiredArgsConstructor;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RequiredArgsConstructor
@RestController
public class ChatSocketController {

    private final SendChatService sendChatService;
    private final CreateRoomService createRoomService;
    private final JoinRoomService joinRoomService;

    @OnEvent("chat")
    public void sendChat(SocketIOClient client, SendChatRequest request) {
        sendChatService.execute(client, request);
    }

    @OnEvent("create")
    public void createRoom(SocketIOClient client, @RequestBody CreateRoomRequest request) {
        createRoomService.execute(client, request);
    }

    @OnEvent("join")
    public void joinRoom(SocketIOClient client, @RequestBody JoinRoomRequest request) {
        joinRoomService.execute(client, request);
    }

}
```

- 이런식으로 HTTP와 아예 다른 경로로 요청을 처리한다.
- HTTP : @XXXMapping, WebSocket : @OnEvent

### 어떻게 웹소켓 요청인지 판단하냐?

웹소켓도 사실 처음엔 HTTP로 시작한다.

브라우저가 이런 요청을 보낸다:

```
GET /socket.io/
Upgrade: websocket
Connection: Upgrade
```

하지만 헤더에 이렇게 붙어있으면

```
Upgrade: websocket
```

웹소켓 요청이라라고 서버가 판단한다.

---

연결이 되면 코드에서 `@onConnect` 어노테이션이 가장 먼저 실행된다.