# Thing+ OAuth2 가이드
Thing+ OAuth2 는 OAuth client 를 등록하고 AccessToken 을 획득하여 Thing+ REST API 를 사용할 수 있게 합니다. OAuth2 를 이용하면 Thing+ Cloud 사용자 리소스에 접근 할 때 비밀번호를 요청하지 않고 외부 어플리케이션에서 접근 할 수 있습니다.

Thing+ 는 AccessToken 획득에 **Authorization Code 방식** 또는 **Resource Owner Password Credentials 방식**을 지원하고 있습니다.

|획득 방식|설명
|---|----
|Authorization Code 방식|특정 사용자에게 리소스 접근권한을 위임받기 원하는 경우
|Resource Owner Password Credentials 방식|관리자(Admin)의 권한으로 관리 범위의 리소스에 접근하기 원하는 경우

**주의 : 등록한 OAuth client ID, OAuth client secret 과 획득한 AccessToken 을 잘 보관하시기 바랍니다.**

획득한 AccessToken 은 Thing+ 운영 정책에 따라 변경될 수 있습니다.

[Thing+ 기본 가이드 문서를 찾으려면 이 문서를 참조하십시오.](../README.md)

[Thing+ REST API 기술 문서를 찾으려면 https://thingplus.api-docs.io 를 참조하십시오.](https://thingplus.api-docs.io/)

## OAuth2 란 무엇인가?
Thing+ 는 클라이언트 사용자 편의를 위해 **OAuth2** 인증을 사용합니다. 아래 OAuth 2.0 flow 를 이해하시면 더 쉽게 Thing+ 를 이용한 개발을 할 수 있습니다.

![OAuth](./images/oauth2.png "OAuth")

[OAuth2 를 더 자세히 알아보려면 이 웹사이트를 참조하십시오.](https://oauth.net/2)

## Thing+ OAuth2 인증 진행하기
### 선결 요건
Thing+ 는 **실제 사용자를 위한 Commercial** 과 **실험적 기능이 추가 되어있는 Sandbox** 를 지원합니다. 이를 시작하기 전에 다음이 필요합니다.
> Commercial 사용시
* [Thing+ Portal](https://thingplus.net) 회원가입. Thing+ 는 개인 사용자를 위한 무료 계정 생성과 비즈니스 고객을 위한 계정 생성을 지원합니다.
* [Thing+ Portal](https://thingplus.net) 로그인

> Sandbox 사용시
* [Thing+ Sandbox Portal](https://www.sandbox.thingplus.net) 회원가입. Thing+ 는 개발자를 위한 무료 계정 생성을 지원합니다.
* [Thing+ Sandbox Portal](https://www.sandbox.thingplus.net) 로그인

### OAuth client 등록하기
첫 번째 단계는 Thing+ Cloud 에 **OAuth client** 를 등록하는 것입니다.

[Thing+ authClients API 를 자세히 알아보려면 이 문서를 참조하세요.](https://thingplus.api-docs.io/2.0/oauth2/create-authclients)

#### Step1. API 호출을 위한 HTTPS API 호출 도구 설치

OAuth Client 등록 API 호출을 위해 HTTPS API를 호출할 수 있는 도구가 필요합니다.
* [Google Chrome](https://www.google.co.kr/chrome/browser/desktop) : Thing+ Portal에 로그인할 때 사용합니다.
* [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en) : 원하는 HTTPS API를 호출할 때 사용하는 Google Chrome App입니다.
* [Postman Interceptor](https://chrome.google.com/webstore/detail/postman-interceptor/aicmkgpgakddgnaphhhpliifpcfhicfo?hl=en) : Thing+ Portal에 로그인 했을 때 생성된 쿠키를 Postman에서 공유할 수 있도록 지원하는 Google Chrome Extension입니다.

위의 도구를 사용하지 않더라도 Thing+ Portal에서 쿠키를 공유할 수 있는 다음과 같은 HTTPS POST API 도구를 사용하시면 됩니다.
* [Fiddler](http://www.telerik.com/fiddler)
* [DHC](https://client.restlet.com)

#### Step2. Thing+ authClients API 호출하기

**Postman** 을 설치하셨다면 툴을 실행하고, **Postman Interceptor** 가 웹브라우저로부터 쿠키를 가져올 수 있도록 **On** 한 상태에서 설정할 값을 입력한 다음 API를 호출합니다.

[Postman 사용에 도움을 얻으려면 Getting Started with the Thing+ REST APIs 이 문서를 참조하십시오.](./GettingStarted_authToken.md)

> Commercial Thing+ REST API 스펙
```
URI : https://api.thingplus.net/v2/authClients
Method : POST
Content-Type : application/json
```

> Sandbox Thing+ REST API 스펙
```
URI : https://api.sandbox.thingplus.net/v2/authClients
Method : POST
Content-Type : application/json
```

> Body 파라미터

|파라미터|설명
|---|----
|name|**(필수)** 사용자 이름
|reqId|**(필수)** 사용자 **OAuth client ID** (AccessToken 획득에 필요합니다.)
|clientSecret|**(필수)** 사용자 **OAuth client secret** (AccessToken 획득에 필요합니다.)
|scopes|**(필수)** client에 부여할 권한 **(권장 : user-profile-read, gateway-update, timeline-read, tag, rule-read, service-read)**

> Body 예시 (JSON)
```
{
  "name": "daligali",
  "reqId": "daliworks",
  "clientSecret": "gali1234",
  "scopes": ["user-profile-read", "gateway-update", "timeline-read", "tag", "rule-read", "service-read", "site-read"]
}
```

추가 권한이 필요하신 경우 contact@thingplus.net 로 요청하시기 바랍니다.

[scopes 에 대해 자세한 설명이 필요하시면 이 문서를 참조하십시오.](./AuthorizationScopes_kr.md)

### Authorization Code 방식으로 AccessToken 획득하기
`AccessToken` 을 획득하기 위해 `Authorization Code` 가 필요합니다. Authorization Code 방식으로 획득한 `AccessToken` 은 **15일간 유효**합니다. 아래 지침을 따르십시오.

[Thing+ OAuth2Token API 를 자세히 알아보려면 이 문서를 참조하세요.](https://thingplus.api-docs.io/2.0/oauth2/oauth2token)

#### Step1. Authorization Code 획득
웹 브라우저로 아래 URI 을 GET 하여 수락 후 redirect_uri Query에 부여된 `Authorization Code` 를 획득 합니다.

> Commercial Thing+ REST API 스펙
```
URI : https://api.thingplus.net/v2/oauth2/authorize?client_id={CLIENT_ID}&response_type=code&redirect_uri={REDIRECT_URI}
Method : GET
```
> 예시
```
https://api.thingplus.net/v2/oauth2/authorize?client_id=daliworks&response_type=code&redirect_uri=https://thingplus.net
```

> Sandbox Thing+ REST API 스펙
```
URI : https://api.sandbox.thingplus.net/v2/oauth2/authorize?client_id={CLIENT_ID}&response_type=code&redirect_uri={REDIRECT_URI}
Method : GET
```
> 예시
```
https://api.sandbox.thingplus.net/v2/oauth2/authorize?client_id=daliworks&response_type=code&redirect_uri=https://thingplus.net
```

> URI Query 파라미터

|파라미터|설명
|---|----
|client_id|**(필수)** 등록한 OAuth client reqId
|response_type|**(필수)** "code" 를 사용
|redirect_uri|**(필수)** Authorization Code 와 함께 redirect 될 URI

> 사용자가 요청을 수락하면 URI 에 "code" 가 포함된 redirect_uri 사이트로 리디렉션됩니다.
```
REDIRECT URI : {REDIRECT_URI}/?code={AUTHORIZATION_CODE}
```
> 예시
```
https://thingplus.net/?code=FKr1INPriNvGcMEC
```

 Sandbox Thing+ Potal 에 로그인이 안되는 경우 **브라우저 쿠키를 삭제**하십시오.

#### Step2. Authorization Code 로 AccessToken 획득
다음 API를 이용하여 `AccessToken` 을 획득합니다. `Authorization Code` 는 **10분 간 유효**하며, `AccessToken` 을 획득하면 해당 `Authorization Code` 는 만료됩니다.
> Commercial Thing+ REST API 스펙
```
URI : https://api.thingplus.net/v2/oauth2/token
Method : POST
Content-Type : x-www-form-urlencoded
```

> Sandbox Thing+ REST API 스펙
```
URI : https://api.sandbox.thingplus.net/v2/oauth2/token
Method : POST
Content-Type : x-www-form-urlencoded
```

> Body 파라미터

|파라미터|설명
|---|----
|code|**(필수)** 획득한 **Authorization Code**
|client_id|**(필수)** 위에서 등록한 사용자 **OAuth client**
|client_secret|**(필수)** 위에서 등록한 사용자 **OAuth client secret**
|redirect_uri|**(필수)** Redirect 할 URI (원하는 URI 를 쓰시면 됩니다.)
|grant_type|**(필수)** authorization_code **(주의 : 문자열을 그대로 입력합니다.)**

> x-www-form-urlencoded POST body 예시
```
code : FKr1INPriNvGcMEC
client_id : daliworks
client_secret : gali1234
redirect_uri : https://thingplus.net
grant_type : authorization_code
```

> 획득한 `AccessToken` 예시 (JSON)
```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VySWQiOiI3MzY3IiwiY2xpZW50SWQiOiJkYWxpd29ya3M1MTIiLCJpYXQiOjE1MDc3MTA4NDYsImV4cCI6MTUwOTAwNjg0Nn0.wmr6MdEDJo5qk4i5EYn34epxRmn9BQq_Nt74AfNCSMc",
    "token_type": "Bearer"
}
```

### Resource Owner Password Credentials 방식으로 AccessToken 획득하기
`AccessToken` 을 획득하기 위해 Thing+ Portal 사용자 암호로부터 생성한 `MD5 hash` 가 필요합니다. Resource Owner Password Credentials 방식으로 획득한 `AccessToken`은 **90일간 유효**합니다. 아래 지침을 따르십시오.

[Thing+ OAuth2Token API 를 자세히 알아보려면 이 문서를 참조하세요.](https://thingplus.api-docs.io/2.0/oauth2/oauth2token-1)

[MD5 hash 에 대한 자세한 내용은 이 웹페이지를 참조하십시오.](https://github.com/blueimp/JavaScript-MD5)

#### Step1. MD5 hash 생성
Mac OS X 또는 Linux 에서 아래 명령어를 이용해 `MD5 hash` 를 생성합니다.

> Mac OS X
```
$ echo -n <b>Your_Password<b> | md5
bbff9cb88fcd3e847923e1bd96aa578f
```

> Linux
```
$ echo -n <b>Your_Password<b> | md5sum
bbff9cb88fcd3e847923e1bd96aa578f
```

#### Step2. MD5 hash 로 AccessToken 획득
다음 API를 이용하여 `AccessToken` 을 획득합니다.
> Commercial Thing+ REST API 스펙
```
URI : https://api.thingplus.net/v2/oauth2/token
Method : POST
Content-Type : x-www-form-urlencoded
```

> Sandbox Thing+ REST API 스펙
```
URI : https://api.sandbox.thingplus.net/v2/oauth2/token
Method : POST
Content-Type : x-www-form-urlencoded
```

> Body 파라미터

|파라미터|설명
|---|----
|grant_type|**(필수)** password **(주의 : 문자열을 그대로 입력합니다.)**
|client_id|**(필수)** 위에서 등록한 사용자 **OAuth client**
|client_secret|**(필수)** 위에서 등록한 사용자 **OAuth client secret**
|username|**(필수)** Thing+ Portal 에 **등록한 아이디**를 입력합니다. (이메일 주소가 아닙니다.)
|password|**(필수)** Thing+ Portal 사용자 암호로 생성한 **MD5 Hash** 를 입력합니다.

> x-www-form-urlencoded POST body 예시
```
grant_type : password
client_id : daliworks
client_secret : gali1234
username : daligali
password : bbff9cb88fcd3e847923e1bd96aa578f
```

> 획득한 `AccessToken` 예시 (JSON)
```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VySWQiOiI3MzY3IiwiY2xpZW50SWQiOiJkYWxpd29ya3M1MTIyIiwiaWF0IjoxNTA3Nzk1NjM4LCJleHAiOjE1MTU1NzE2Mzh9.pejzZn8CgsXrbL9J1c1fd_hTYKdoSPmOk0HCAKC7cSc",
    "token_type": "Bearer"
}
```

### Header 에 AccessToken 등록
`AccessToken` 은 Thing+ REST API 를 호출할 때 권한 인증을 위해 **Header**에 반드시 있어야합니다.

Header 에 **Authorization 필드**를 추가하고, **token_type**과 획득한 **access_token** 값을 아래와 같이 입력해주십시오.
```
Authorization : Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VySWQiOiI3MzY3IiwiY2xpZW50SWQiOiJkYWxpd29ya3M1MTIiLCJpYXQiOjE1MDc3MTA4NDYsImV4cCI6MTUwOTAwNjg0Nn0.wmr6MdEDJo5qk4i5EYn34epxRmn9BQq_Nt74AfNCSMc
```

[Thing+ OAuth2 API Errors 에 대한 자세한 설명을 보시려면 이 문서를 참조하십시오.](./AuthorizationErrors_kr.md)

기타 추가 지원이 필요하시면 contact@thingplus.net 로 요청하시기 바랍니다. 감사합니다.
