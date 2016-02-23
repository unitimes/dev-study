#Java option
##`java -X`
다양한 java option을 listing 한다
###Heap szie 관련
####`-Xmx<size>`
최대 heap size
####`-Xms<size>`
initial heap size
```
java -Xmx6g <myprogram>
```
##환경변수 JAVA_OPTS
JDK 표준은 아니다. 그러나 tomcat 등이 이 환경변수를 참조한다. 즉, 이 환경변수를 바꿈으로써 이 환경변수를 참조하는 모든 apps의 java options을 한번에 변경할 수 있다.
###Unix 환경 사용 예시
java source encoding type과 jvm의 initial heap size 설정
```
export JAVA_OPTS=-Dfile.encoding=UTF-8 -Xms2g
```
환경변수의 scope은 쉘에 종속하므로 jvm을 실행시킬(`java <program>`) 쉘의 환경설정 파일에 작성해야 한다.

####Tomcat에 적용
어느 쉘에서 실행 되는 tomcat에 적용하고 싶다면 `${tomcat_home}/bin/setenv.sh`에 작성한다.