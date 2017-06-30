# Apache
## 알아두어야 할 것
### 구동 계정과 프로세스 계정
구동 계정과 프로세스 계정이 존재함. 웹서버 권한 획득 시 프로세스 계정이 탈취 될 수 있음. 프로세스 계정은 로그인 쉘 권한을 제거해야 함.
## 설정
### 설정 섹션(Configuration Section Containers)
특정 설정을 특정 조건에 적용하고자 할 경우 섹션으로 묶고 섹션에 조건을 설정
#### 적용 조건의 종류에 따른 설정 섹션의 분류
- 매 요청마다 조건을 검토하여 개별 요청에 적용/미적용 결정
- 서버 시작(재시작)시에 조건을 검토하여 모든 요청에 적용/미적용 결정
	- `<IfDefine>`, `<IfModule>`, `<IfVersion>`
#### 개별 요청 조건 검토 방식
- Filesystem 조건 검토
	- `<Directory>, <Files>`
- Webspace 조건 검토
	- `<Location>`
- Boolean 표현식 검토
	- `<If>`
- Host 검토
	- `<VirtualHost>`
- Proxy 검토
	- `<Proxy>`
#### 복수 설정 섹션의 적용
- 기본 적용 순서
`<Directory>` without regular expressions(~ 으로 조건 시작), .htaccess -> `<DirectoryMatch>`, `<Directory>` with regex -> `<Files>`, `<FilesMatch>` -> `<Location>`, `<LocationMatch>` -> `<If>`
- 동일 섹션간 적용 순서
	- `<Dirctory>`는 짧은 path먼저 적용
	- 다른 섹션들은 설정 파일의 위치대로 위에 있는 설정 먼저 적용
- 기타
	- `<Include>`는 설정 파일의 등장 위치에 삽입 처리
	- `<VirtualHost>`는 외부 설정 적용 후 적용
	- `<Proxy>`는 `<Directory>` 취급

https://httpd.apache.org/docs/2.4/en/sections.html#merging
### .htaccess
directory에 __.htaccess__ 를 생성하고 파일에 설정을 적용하면 해당 directory를 포함한 하위에만 설정이 적용.
.htaccess 를 활성화 시키려면 server config에 적절한 AllowOverride 설정을 해야 함.
### 주요 지시자(Directive)
- [ServerRoot](https://httpd.apache.org/docs/2.4/en/mod/core.html#serverroot)
	set apache server directory, 다른 설정 지시자의 상대 경로의 기준이 됨.

- [Listen](https://httpd.apache.org/docs/2.4/en/mod/mpm_common.html#listen)
- [LoadModule](https://httpd.apache.org/docs/2.4/en/mod/mod_so.html#loadmodule)
- [ServerAdmin](https://httpd.apache.org/docs/2.4/en/mod/core.html#serveradmin)

서버가 에러메시지를 client에게 보낼 때 포함할 이메일 주소
- [DocumentRoot](https://httpd.apache.org/docs/2.4/en/mod/core.html#documentroot)

서버가 제공할 파일들의 root, 별도의 설정이 없으면 url path를 DocumentRoot에 붙인 경로가 filesystem에서의 경로에 해당.
끝에 slash(/) 붙이면 안됨
- [ServerTokens](https://httpd.apache.org/docs/2.4/en/mod/core.html#servertokens)

응답 헤더에 들어갈 서버 정보 설정.
공격 방지를 위해 __Prod__ 로 설정 해야 함. 서버의 종류만 알려 줌.
- [ServerSignature](https://httpd.apache.org/docs/2.4/en/mod/core.html#serversignature)

서버가 생성하는 문서 끝에 서버 정보를 첨부.
보안 상 __Off__ 로 설정 해야 함.
- [User](https://httpd.apache.org/docs/2.4/en/mod/mod_unixd.html#user)

Unix 시스템에서 서버 구동 시 서버가 응답에 사용할 unix user.
server를 root로 구동해야 사용할 수 있는 지시자.
- [Group](https://httpd.apache.org/docs/2.4/en/mod/mod_unixd.html#group)

Unix 시스템에서 서버 구동 시 서버가 응답에 사용할 unix group.
server를 root로 구동해야 사용할 수 있는 지시자.
- [AllowOverride](https://httpd.apache.org/docs/2.4/en/mod/core.html#allowoverride)

`<Directory>`에만 설정 가능.
.htaccess 에서 적용할(상위 설정을 override 할) 지시자들을 지정.
- [Options](https://httpd.apache.org/docs/2.4/en/mod/core.html#options)

특정 디렉토리에서 활성화할 서버 기능들을 지정.
가장 정교한 directory 조건에 설정한 Options 지시자만 활성화 되나,  +/-를 붙이면 섹션 적용 순서에 따라 merge 됨.
하나의 Options 지시자에 +/-이 붙은 값과, 붙지 않은 값이 같이 있을 수 없음.
```
# 아래는 잘못된 사용 예, 서버가 구동되지 않음
Options +Indexes Includes
```
- [Order](https://httpd.apache.org/docs/2.4/en/mod/mod_access_compat.html#order)
directory, .htaccess 에 적용.
Deny, Allow 지시자의 적용 순서 지정. 후위에 오는 지시자가 우위를 가짐.
Deny, Allow 지시자 모두 match 되지 않으면 후위에 오는 지시자가 access 여부 결정.
```
# 아래와 같이 세팅하면 Deny, Allow 조건에 해당하지 않는 요청은 모두 access 가능
Order Deny,Allow
```
### Tomcat 연동
tomcat과의 통신에 사용할 tomcat의 connector를 지정
#### connector 지정
proxy_module, proxy_ajp_module, jk_module 등을 사용. protocol + host + [port] 의 조합으로 통신할 tomcat connector 지정

- proxy_module
http만 지원
- proxy_ajp_module
ajp 지원, proxy_module 필요
- mod_jk
ajp지원, 독립적 기능 제공, 설정 다소 복잡
