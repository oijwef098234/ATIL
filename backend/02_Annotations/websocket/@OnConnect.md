# @OnConnect란?

> 클라이언트가 웹소켓 서버에 연결될 때 자동으로 실행되는 메서드에 붙이는 어노테이션

### 예시 코드

```java
package com.example.socket.global.websocket.connect;

import com.corundumstudio.socketio.SocketIOClient;
import com.corundumstudio.socketio.annotation.OnConnect;
import com.corundumstudio.socketio.annotation.OnDisconnect;
import com.example.socket.global.security.jwt.JwtTokenProvider;
import com.example.socket.global.websocket.property.ClientProperty;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.security.core.Authentication;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.RestController;

import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.ConcurrentMap;

@Slf4j
@RequiredArgsConstructor
@RestController
public class WebSocketJwtHandler {

    private final JwtTokenProvider jwtTokenProvider;

    public static final ConcurrentMap<String, SocketIOClient> socketIOClientMap = new ConcurrentHashMap<>();

    @OnConnect
    public void onConnect(SocketIOClient client) {
        String token = client.getHandshakeData().getHttpHeaders().get("Authorization");
        Authentication authentication = jwtTokenProvider.authentication(token);
        String accountId = authentication.getName();
        client.set(ClientProperty.USER_KEY, accountId);
    }

    @OnDisconnect
    public void onDisconnect(SocketIOClient client) {
        socketIOClientMap.remove(client.get(ClientProperty.USER_KEY).toString());
    }

}
```

- 위 코드와 같이 웹소켓이 서버에 연결될때 자동으로 실행되는 메서드에 해당 어노테이션을 할당해준다.

# @OnDisconnect란?

> 웹소켓이 서버에서 연결이 끊어지는 순간 실행되는 어노테이션

- 위의 코드와 같이 연결이 끊어질때 실행해야될 메서드에 해당 어노테이션을 할당해준다.