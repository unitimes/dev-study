#Singleton
##싱글톤 구현 방법
###1. public final 필드 이용
```java
public class Elvis {
	public static final Elvis INSTANCE = new Elvis();
	private Elvis() { ... }

	public void someMethod() { ... }
}
```
- 외부에서 `Elvis.INSTANCE`에 바로 접근

###2. 정적 팩토리 이용
```java
public class Elvis {
	private static final Elvis INSTANCE = new Elvis();
	private Elvis() { ... }
	public static Elvis getInstance() { return INSTANCE; }

	public void someMethod() { ... }
}
```

###3. 원소가 하나뿐이 enum
```java
public enum Elvis {
	INSTANCE;

	public void someMethod() { ... }
}
```
- 1, 2 번 방식은 Serializable을 구현하면 싱글턴 특성을 잃게 된다.
	- 싱글턴 특성을 유지하기 위해서는 특별한 처리([규칙77]())가 필요하다.
- enum은 자동으로 직렬화가 처리되며, 리플렉션 공격에도 안전하다.
	- 때문에 싱글턴을 구현하는 좋은 방법이다.