# Thing+ OAuth2 Authorization Scopes
Thing+ OAuth2 Authorization Scopes 는 Thing+ OAuth2 AccessToken의 액세스 권한을 부여합니다. 액세스 부여 권한은 사용자 계정에 부여된 권한을 넘어설 수 없습니다.

Authorization Code Grant 방식을 사용할 때 authorize scopes 에 아래 scope 접근 권한을 부여하면 됩니다.
## Authorization Scopes

|     Scope          |                Name              | resources (API) |                          Description
| ------------------ | - | - | -
| (no scope)         |                                  | | 어떤 scopes 도 액세스 할 수 없습니다.
| user-profile       | User Profile Read and Update     | users/me, /changePassword | (TBD) 사용자 프로필 읽기 및 업데이트 권한
| user-profile-read  | User Profile Read                | users/me | 사용자 프로필 읽기 권한
| gateway            | Gateway Ceate / Read / Update / Delete | /gateways, /registerGateway, /registerGatewayKey, /manageGateway, /controlActuator, /sensorTypes, /sensorDrivers, /gatewayModels | (TBD) 권한이 있는 사용자에 대한 게이트웨이 / 디바이스 / 센서 등록, 읽기, 업데이트, 삭제 권한
| gateway-read       | Gateway Read                     | /gateways, /controlActuator | 권한이 있는 사용자에 대한 게이트웨이 / 디바이스 / 센서 / 제어 액츄에이터 읽기
| gateway-update     | Gateway Read/Update              | /gateways, /manageGateway,  /controlActuator | 권한이 있는 사용자에 대한 게이트웨이 / 디바이스 / 센서 / 제어 액츄에이터 읽기, 업데이트 권한, 게이트웨이 관리 권한
| timeline-read      | Timeline Read                    | /timelines | 사용자 타임라인 읽기 권한
| tag                | Tag Ceate / Read / Update / Delete     | /tags | 사용자 태그 생성, 읽기, 업데이트, 삭제 권한
| tag-read           | Tag Read                         | /tags | 사용자 태그 읽기 권한
| rule               | Rule Read / Update / Delete          | /rules, /pushDevices | (TBD) 사용자 규칙 읽기, 업데이트, 삭제 권한
| rule-read          | Rule Read                        | /rules | 사용자 규칙 읽기 권한
| service-read       | Service Read                     | /services | 사용자가 등록한 서비스 읽기 권한
| site-read          | Site Read                        | /site | 사용자가 등록한 사이트 읽기 권한
| billing-read       | Billing Read                     | /billings | (TBD) 사용자 청구 정보 읽기 권한

**TBD** : 현재 사용가능하지 않음
