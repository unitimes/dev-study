#Log4j 2

##기본

###Hierarchy
LoggerConfig 단위의 hierarchy를 가짐
Hierarchy는 LoggerConfig name의 dot(.)으로 구분 됨
>`"com.foo"`는 `"com.foo.bar"`의 parent

Logger는 가장 인접한 상위의 LoggerConfig reference를 가짐

###Root Logger
Root LoggerConfig가 최상위에 있으며 항상 존재 함
`LogManager.getLogger(LogManager.ROOT_LOGGER_NAME);` or `LogManager.getRootLogger();`호출로 반환
###Logger
Logger는 실제적 actions를 가지지 않음. 이름 관련 LoggerConfig의 reference를 가지고 있을 뿐
####getLogger()
Configuration에 등록 한 모든 Logger는 LoggerFactory에 static으로 저장(Map 구조) 됨	
`LogManager.getLogger("Logger name")`을 통해 반환

Logger의 name으로 class의 이름을 사용하는 것이 일반적	
component 구조와 동일한 hierarchy를 갖게 됨	
`LogManager.getLogger(Calling.class)` or `LogManager.getLogger()`	
파라미터가 없으면 default로 class 이름을 갖게 됨
###[LoggerConfig](https://logging.apache.org/log4j/2.x/log4j-core/apidocs/org/apache/logging/log4j/core/config/LoggerConfig.html)
Filter와 Appender의 reference를 가짐
####Log Levels
TRACE > DEBUG > INFO > WARN > ERROR > FATAL
####Filter
Log level 외에 filter를 가지고 log event를 걸러낼 수 있음
###[Appender](https://logging.apache.org/log4j/2.x/manual/appenders.html)
Output destination	

하나의 Logger가 복수의 appender를 가질 수 있음

조상의 appender는 자동으로 자손에게 상속 됨
상속을 하지 않으려면 Logger 설정에서 `additivity="false"`
>Logger x  x.y  x.y.z  가 있고 x.y의 additivity가 false이면,
>Logger x.y.z의 log는 x.y의 appenders에게까지 전달되며, x.y.z에겐 전달 되지 않음
