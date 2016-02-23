#Groovy 기본 문법
Gradle는 build script를 작성할 대 groovy를 스크립트 언어로 사용한다.
##문자열
###`''`단순 문자열
###`""`문자열 내에 $기호로 동적인 내용을 넣을 수 있다.
```
String name = 'John'

String title = "Project: $name"
//여러줄 표기
String greet = '''*************
Hello
************'''
```
##형지정
def를 사용하면 형 지정을 생략가능 하다.
```
def name = 'John'
```