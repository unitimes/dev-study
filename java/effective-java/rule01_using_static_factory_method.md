# 생성자 대신 정적 팩토리 메서드 사용을 고려하라
## 장점
### 1. 생성자와 달리 이름을 가질 수 있다.
- 가독성을 높인다.

### 2. 객체의 생성을 관리 할 수 있다.
- 객체 생성 불가능한 클래스를 만들 수 있다.
- 객체 생성 여부를 결정할 수 있다.
	- ex. 싱글톤 등

### 3. 반환 타입의 하위 타입을 반환할 수 있다.
- 반환 타입을 인터페이스 등으로 하고 하위 타입의 적합한 구현체 반환 가능
	- 인터페이스는 **_정적 메서드를 가질 수 없다._**
	- Type 인터페이스 형을 반환하는 정적 팩터릴 메소드는 Types라는 이름의 객체 생성 불가 클래스 안에 둔다.([규칙 4]())
- 서비스 제공자 프레임워크도 유연한 정적 팩터리 메소드를 활용
	- 정적 팩토리 메소드가 반환하는 객체의 클래스는 정적 팩토리 메소드가 정의된 클래스의 코드가 작성되는 순간에 존재하지 않아도 무방

> **_서비스 제공자 프레임워크의 예 JDBC_**
> 
> JDBC는 서비스 인터페이스인 Connection의 구현체를 반환하는 getConnection 정적 팩토리 메소드를 가지고 있지만, Connection 구현체를 가지고 있지 않다. 
>
> Connection 구현체는 나중에 주입된다.

## 단점
### 1. public, protected 생성자가 없어 상속이 불가
### 2. 정적 팩토리 메소드와 다른 정적 메소드의 구분이 어렵다.
