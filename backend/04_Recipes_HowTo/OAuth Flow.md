## OAuth Flow

![[Oauth.png]]

- Authorization Server와 Resource Server는 서로 통신하며 이 밑의 과정이 이루어진다.

1. 사용자는 서비스를 이용하기 위해 로그인 페이지에 접근한다.
    
2. 그럼 서비스는 로그인 페이지를 제공하게 되고, 사용자는 GitHub로 로그인을 누른다.
    
3. 특정한 url 이 Github 서버쪽으로 보내지게 된다.
    
4. 클라이언트로부터 보낸 서비스 정보와, 리소스 로그인 서버에 등록된 서비스 정보를 비교한다
    
    - 클라이언트가 보낸 redirect URL과 리소스 서버가 저장하고 있는 redirect URL을 비교한다.
    
    1. 확인이 완료되면, Resource Server로 부터 전용 로그인 페이지로 이동하여 사용자에게 보여준다.
5. ID/PW를 적어서 로그인을 하게되면, client가 사용하려는 기능(scope)에 대해 Resource Owner의 동의(승인)을 요청한다.
    
    - 내가 만든 서비스에서 깃허브의 커밋수를 필요로 하면 “너의 깃허브 프로필에 접근하겠다”와 같이 기능을 요청하는거임
    - 이로써 Resource Sever가 갖는 정보는 다음과 같다.
        - Client Id : Resource Owner와 연결된 client가 누군지
        - Client Secret: Resource Owner와 연결된 client의 비밀번호
        - Redirect URL : (진짜)client와 통신할 통로
        - user id : client와 연결된 Resource Owner의 id
        - scope : client가 Resource Owner 대신에 사용할 기능들
    
    a. Resource Owner가 Allow 버튼을 누르면 Resource Owner가 권한을 위임했다는 승인이 Resource Server 에 전달된다.
    
6. 하지만, 이미 Owner가 Client에게 권한 승인을 했더라도 아직 Server가 허락하지 않았다. 따라서, Resource Server 도 Client에게 권한 승인을 하기위해 Authorization code를 Redirect URL을 통해 사용자에게 응답하고
    
7. 다시 사용자는 그대로 Client에게 다시 보낸다.
    
8. 이제 Client가 Resource Server에게 직접 url(클라이언드 아이디, 비번, 인증코드 ...등)을 보낸다.
    
9. 그럼 Resource Server는 Client가 전달한 정보들을 비교해서 일치한다면,Access Token을 발급한다. 그리고 이제 필요없어진 Authorization code는 지운다.
    
    - 여기서 Github는
    - 리소스 서버와(GitHub의 Api들이 있는 서버)
    - Oauth서버(인증을 해주는 서버), 즉 AccessToken을 발급해주는 곳 두곳이 있는데 지금은 여기로 보내는거임
10. 그렇게 토큰을 받은 Client는 사용자에게 최종적으로 로그인이 완료되었다고 응답한다.