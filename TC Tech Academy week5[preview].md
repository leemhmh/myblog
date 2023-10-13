# TC Tech Acamdemy week 5
#### Preview
---
# OAuth 2.0 이론 예습

OAuth 2.0이란 인증을 위한 개방형 표준 프로토콜
OAuth는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용되는, 접근 위임을 위한 개방형 표준

- Third-Party 프로그램에게 리소스 소유자를 대신하여 리소스 서버에서 제공하는 자원에 대한 접근 권한을 위임하는 방식을 제공
- 구글, 페이스북, 카카오, 네이버 등에서 제공하는 간편 로그인 기능도 OAuth2 프로토콜 기반의 사용자 인증 기능을 제공

 ## OAuth 2.0의 특징
- 웹 애플리케이션이 아닌 애플리케이션 지원 강화 (모바일 어플리케이션에서 사용 가능)
- 암호화가 필요 없음 HTTPS를 사용하고 HMAC을 사용하지 않는다. (반드시 HTTPS를 사용하기에 보안 강화)
- Siganature 단순화 정렬과 URL 인코딩이 필요 없다.
- Access Token 갱신 OAuth 1.0에서 Access Token을 받으면 Access Token을 계속 사용할 수 있었다. 트위터의 경우에는 Access Token을 만료시키지 않는다. OAuth 2.0에서는 보안 강화를 위해 Access Token의 Life-time을 지정할 수 있도록 했다. (Access Token 만료기간 생김)

## 관련 용어
· Resource Owner : 일반 사용자(리소스의 권한을 가진 사용자). 사용자 혹은 유저(User)라고도 합니다. 본인의 정보에 접근할 수 있는 자격을 승인하는 주체
   ex) A 쇼핑몰에서 SNS 계정으로 로그인 혹은 가입을 시도하려는 자
   
· Client : 리소스 서버에 접근을 요청하는 어플리케이션 혹은 웹 사이트. 즉, 서비스를 제공자이며 Third Party Application 라고도 합니다.
   ex) A 쇼핑몰에서는 SNS 계정으로 로그인하여 쇼핑 할 수 있다. 여기서 클라이언트는 A 쇼핑몰이다.
   
· Authorization Server : 액세스 토큰(Access Token)을 클라이언트에게 발급해주는 서버. 인가의 주체이기도 합니다.
   ex) 로그인을 통해 인증 후, 권한를 부여하는 서버(인가의 주체). 구글, 페이스북, Github 등
   
· Resource Server : Authorization Server가 발급한 액세스 토큰(Access Token)을 확인하고 리소스를 제공. API 서버이자 Resource Owner의 정보가 저장되어 있는 서버
   ex) 이름, 나이 등 아이디 관련 정보(리소스)를 제공하는 서버. 구글, 페이스북, Github 등
   cf.) Authorization Server와 Resource Server를 묶어서 OAuth Provider라고도 합니다.

## 프로세스 방식
                        1. Resoruce Owner인 User가 Client를 통해서 인가(Authorization)를 시작합니다.

                        2. Client는 User를 Authorization Server로 포워드 해줍니다.

                        3. Authorization Server는 User에게 로그인창을 제공하여 User를 인증하게 됩니다.

                        4. User가 ID/Password를 입력하여 로그인 하면, Client가 Resource에 대한 접근 권한을 요청합니다.

                        5. Authorization Server는 Authorization Code를 Client에게 제공해줍니다.

                        6. Client는 Authorization Code로 Authorization Server에 Access Token을 요청합니다.

                        7. Authorization Server는 Client에게 Access Token을 발급해줍니다.

                        8. Access Token을 이용하여 Resource Server의 자원에 접근할 수 있게 됩니다.

                        9. 이후 Access Token이 만료되면 Refresh Token을 이용하여 Access Token을 재발급 받을 수 있습니다.  

## 권한 부여 방식
1. Authorization Code Grant Type
   OAuth2.0 Grant Type에서 가장 잘 알려진 방식이며 중요한 보안 이점을 제공하는 방법

   클라이언트가 사용자 대신 특정 리소스에 접근을 요청할 때 사용함. 클라이언트는 Access Token을 발급 받기 전,

   인가코드(Authorization Code)를 사용자에 의해 받게 되고, 그 인가코드(Authorization Code)를 가지고

   인가 서버(Authorization Server)에 요청을 보내면 리소스에 대한 Access Token을 발급 받음

2. Implicit Grant Type
   Authorization Code Grant Type과 다르게, 인가코드(Authorization Code) 교환 단계 없이 Access Token을 즉시 반환받아 

   이를 인증에 이용하는 방식. 자격증명을 안전하게 저장하기 힘든 클라이언트(ex: JavaScript등의 스크립트 언어를 사용한 브라우저)에게 최적화됨

   Refresh Token 사용이 불가능하며, Access Token을 획득하기 위한 절차가 간소화되기에 응답성과 효율성은 높아지지만 Access Token이 URL로 전달된다는 단점이 있음

3. Resource Owner Password Credentials Grant Type
   클라이언트가 암호를 사용하여 Access Token에 대한 사용자의 자격 증명을 교환하는 방식. 간단하게 username, password로 Access Token을 받는 방식

   자신의 서비스에서 제공하는 어플리케이션일 경우에만 사용되는 인증 방식

4. Client Credentials Grant Type 
   클라이언트가 컨텍스트 외부에서 Access Token을 얻어 특정 리소스에 접근을 요청할 때 사용하는 방식. 클라이언트의 자격증명만으로 Access Token을 획득

   OAuth2의 권한 부여 방식 중 가장 간단한 방식으로 클라이언트 자신이 관리하는 리소스 혹은 권한 서버에 해당 클라이언트를 위한 제한된 리소스 접근 권한이 설정되어 있는 경우 사용

   자격증명을 안전하게 보관할 수 있는 클라이언트에서만 사용되어야 하며, Refresh Token은 사용할 수 없음

