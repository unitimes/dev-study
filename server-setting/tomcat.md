# Tomcat
Tomcat은 __Java Servlet, JavaServer Pages, Java EL, Java WebSocket__ 구현의 집합. HTTP Connector를 가지고 있기 때문에 단독으로 web server 역할 수행 가능.
## Tomcat과 web applications
Tomcat이 구동되면 하나의 servlet container를 가지게 되며, 하나의 servlet container는 다수의 web applications를 가짐.
## Web application의 배포
[다양한 배포 방식](https://tomcat.apache.org/tomcat-8.5-doc/deployer-howto.html)이 존재.
자주 사용하는 자동 배포만 다루기로 함.
### Tomcat 구동 시 application 자동 배포
Host 엘리먼트의 __deployOnStartup__ 속성을 true 설정 해야 함.
#### 배포 대상
context descriptors(일반적으로 context.xml 이라 지칭), Host 엘리먼트의 __appBase__ 에 설정한 directory 내에 위치한 web applications.
#### 배포 순서
1. Context descriptors
2. 압축 되지 않은 web application. 더 최신의 .WAR 가 있으면 압축 되지 않은 web application 삭제 후 .WAR로 재배포
3. .WAR
### 구동 중인 Tomcat에 application 자동 배포
Host 엘리먼트의 __autoDeploy__ 속성을 true 설정해야 함.
#### 배포 대상
- Host appBase 안에 있는 새로운 .WAR, exploded web application __배포__
- __.WAR로 배포__ 한 web application의 최신의 .WAR 제공되면 __재배포__. 기 서비스 중인 구버전이 exploded web application 인 경우(unpackWARs 속성 true인 경우) 구버전은 삭제 됨.
- `/WEB-INF/web.xml`및 WatchedResource로 정한 파일이 업데이트 되면 __reloading__
- __Context Descriptor로 배포__ 한 web application의 Context Descriptor 업데이트 되면 __재배포__
- 공용 Context Descriptor 업데이트 되면 관계된 모든 web applications __재배포__
- Host configBase에 Context Descriptor file 추가 시, Context path에 매핑되는 서비스 중인 web application 있으면 __재배포__
- docBase 삭제 시 __undeployment__
### 자동 배포 시 알아두어야 할 것
#### Context descriptor
Context 엘리먼트 설정을 담고 있는 xml 파일.

__자동 배포 대상이 되는 Context descriptor__
각 web application의 `/META-INF/context.xml`, Host 엘리먼트의 __xmlBase__ 에 설정한 directory 내에 위치한 .xml 파일들(`/META-INF/context.xml`에 우선함). 
> server.xml 안에 Context 엘리먼트를 정의하는 것으로도 가능하나 비추하는 방법
#### Context path
uri path에 매핑되어 host가 요청을 처리할 context를 결정 함.
#### Context path의 결정
web application의 name(context descriptor, WAR, exploded application's directory)에 의해 자동으로 결정
> Web application의 이름은 `Context path + ##Version`로 정해야 하며, 이에 따라 context path, web application, context descriptor의 mapping이 이루어짐.
> Context path 중간의 `/`는 `#`로 바꿔야 함.

## 설정

### 구조 (Top - Down)
- Server 엘리먼트가 Catalina servlet container
- Server 엘리먼트는 다수의 Service 엘리먼트를 가짐
- Service 엘리먼트는 다수의 Connector 엘리먼트와 하나의 Engine 엘리먼트를 가짐
- Connector 엘리먼트는 특정 TCP port 열결을 listening
- Engine 엘리먼트는 Service의 요청을 처리
- Engine 엘리먼트는 다수의 Host 엘리먼트를 가짐
- Host 엘리먼트는 다수의 Context 엘리먼트를 가짐
- Context 엘리먼트가 하나의 web application
### 설정 파일
- 대부분의 설정은 `$CATALINA_BASE/conf/server.xml`
- 개별 web application context 설정은 개별 context descriptor
- 모든 web applications의 context 설정은 `$CATALINA_BASE/conf/context`
- 특정 host 하위 web applications의 context 설정은 `$CATALINA_BASE/conf/[enginename]/[hostname]/context.xml.default`
### 중요 설정
#### Server
- port
shutdown 명령을 기다리는 TCP/IP port 설정
- shutdown
shutdown 하기 위한 command string
#### Service
- name
이름, Sever 내에서 유일해야 함
#### Connector
- port
connector가 listening 하는 TCP/IP 포트
- protocol
통신 protocol
- redirectPort
non-SSL 요청을 지원하는 경우, SSL 이 필요한 요청에 대해 redirect 할 port 지정
#### Engine
- name
이름
- defaultHost
정의되지 않은 host name을 가리키는 요청을 처리할 기본 host
#### Host
- appBase
web application base directory
- xmlBase
배포할 context XML의 Base directory pathname. `conf/<engine_name>/<host_name>`이 기본값.
- deployXML
application에 포함된 context XML(/META-INF/context.xml)을 파싱할지 여부 지정.
- name
Virtual host의 domain name. 최하위 subdomain label에 asterisk(*) 사용 가능 `*.domainname`. 대소문자 구분 없이 자체적으로 소문자 처리. Engine 엘리먼트의 defaultHost가 참조할 수 있는 값.
- workDir
__scratch directory__ 임시 파일 읽기/쓰기에 사용할 directory. Context 엘리먼트의 workDir 설정이 이 설정을 override.
- autoDeploy
boolean.  Tomcat 운영 중 웹 어플리케이션의 변화를 지속적으로 체크하여 변화가 있을 시 배포할지 여부를 결정. __appBase, xmlBase__ 디렉토리를 체크.
- deployOnStartup
Tomcat 구동 시 자동으로 web application을 배포할지 여부 결정.
#### Context
server.xml에 설정할 수는 있으나 하지 말 것.

- docBase
web application base directory. 
server.xml에 설정하는 경우가 아니면 docBase는 Host 엘리먼트의 appBase의 외부에 있어야 함. 그렇지 않으면 설정하지 말 것. 이중 배포 등의 문제가 발생할 수 있음.
- workDir
Host 엘리먼트의 workDir override.
- reloadable
`/WEB-INF/classes/`, `/WEB-INF/lib/`의 classes 변화를 모니터링 하고 변화 발생 시 reload 여부 결정. 프로덕트 환경에서는 false로 설정 할 것.
- path
context path 설정. 자동 배포 시 web application name에 의해 자동으로 설정 되나, 다음의 경우 자동 배포 시 web application을 매핑 할 수 없으므로 path를 직접 설정해야 함.
1. server.xml에 context.xml을 정의하고 docBase가 appBase 하위에 없는 경우
2. server.xml에 context.xml을 정의하고 자동 배포(deployOnStartup, autoDeploy 모두 false)를 하지 않는 경우
