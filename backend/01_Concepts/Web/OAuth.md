## Oauth란?

> Auth는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용되는, 접근 위임을 위한 개방형 표준

## OAuth의 구성요소

### Client

> 자사 또는 개인이 만든 애플리케이션 서버

- 클라이언트 라는 이름은 client가 Resource server에게 필요한 자원을 요청하고 응답하는 관계여서 그렇다.
- 기존에 우리가 알고있는 프론트엔드, ios 등등 이런것들과는 다른 개념

### Resource Owner

> 웹 서비스를 이용하려는 유저, 자원(개인정보)을 소유하는 자, 사용자

### Authorization Server

> 권한을 부여해주는 서버다.

- 사용자는 이 서버로 ID, PW를 넘겨 Authorization Code를 발급 받을 수 있다.
- Client는 이 서버로 Authorization Code을 넘겨 Token을 받급 받을 수 있다.

### Resource Server

> 사용자의 개인정보를 가지고 있는 애플리케이션

- Google, FaceBook, Instargram 등등