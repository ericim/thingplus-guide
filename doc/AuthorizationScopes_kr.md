# Thing+ OAuth2 Authorization Scopes
Thing+ OAuth2 Authorization Scopes 는 Authorization Code Grant 을 사용하여 Thing+ OAuth2 AccessToken 을 획득할 때 Thing+ OAuth2 AccessToken에 액세스 권한을 부여합니다. 액세스 부여 권한은 사용자 계정에 부여된 권한을 넘어설 수 없습니다. Scopes 는 User, Gateway, Timeline, Tag, Rule, Service, Site, Billing 에 대한 액세스 권한을 부여할 수 있으며 권한을 넘어서서 사용자 리소스에 접근 할 경우 액세스가 거부됩니다.

권장되는 기본 부여 권한은 user-profile-read, gateway-update, timeline-read, tag, rule-read, service-read 입니다. 만약 사용자 계정에 부여된 권한을 넘어선 추가 권한을 부여할 필요가 있으신 경우 contact@thingplus.net 로 요청하시기 바랍니다.

Authorization Code Grant 방식을 사용할 때 authorize scopes 에 아래 scope 접근 권한을 부여하면 됩니다.
## Authorization Scopes

|     Scope          |                Name              | resources (API) |                          Description
| ------------------ | - | - | -
| (no scope)         |                                  | | 어떤 scopes 도 액세스 할 수 없습니다.
| user-profile       | 사용자 프로필 읽기와 업데이트     | users/me, /changePassword | (TBD) 사용자 프로필 읽기 및 업데이트 권한
| user-profile-read  | 사용자 프로필 읽기  | users/me | 사용자 프로필 읽기 권한
| gateway            | 게이트웨이 생성 / 읽기 / 업데이트 / 삭제 | /gateways, /registerGateway, /registerGatewayKey, /manageGateway, /controlActuator, /sensorTypes, /sensorDrivers, /gatewayModels | (TBD) 권한이 있는 사용자에 대한 게이트웨이 / 디바이스 / 센서 등록, 읽기, 업데이트, 삭제 권한
| gateway-read       | 게이트웨이 읽기                 | /gateways, /controlActuator | 권한이 있는 사용자에 대한 게이트웨이 / 디바이스 / 센서 / 제어 액츄에이터 읽기
| gateway-update     | 게이트웨이 읽기 / 업데이트  | /gateways, /manageGateway,  /controlActuator | 권한이 있는 사용자에 대한 게이트웨이 / 디바이스 / 센서 / 제어 액츄에이터 읽기, 업데이트 권한, 게이트웨이 관리 권한
| timeline-read      | 타임라인 읽기                   | /timelines | 사용자 타임라인 읽기 권한
| tag                | 태그 생성 / 읽기 / 업데이트 / 삭제    | /tags | 사용자 태그 생성, 읽기, 업데이트, 삭제 권한
| tag-read           | 태그 읽기 | /tags | 사용자 태그 읽기 권한
| rule               | 규칙 읽기 / 업데이트 / 삭제     | /rules, /pushDevices | (TBD) 사용자 규칙 읽기, 업데이트, 삭제 권한
| rule-read          | 규칙 읽기  | /rules | 사용자 규칙 읽기 권한
| service-read       | 서비스 읽기   | /services | 사용자가 등록한 서비스 읽기 권한
| site-read          | 사이트 읽기                     | /site | 사용자가 등록한 사이트 읽기 권한
| billing-read       | 청구 읽기 | /billings | (TBD) 사용자 청구 정보 읽기 권한

**TBD** : 현재 사용가능하지 않음
