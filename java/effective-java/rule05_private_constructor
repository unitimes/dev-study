#Private Constructor

- 객체 생성을 막을 때는 명시적으로 private 생성자를 사용해야 한다. 생성자를 생략하면 컴파일러가 자동으로 인자 없는 public 생성자를 생성한다.
- private 생성자에 AssertionError를 던져 클래스 안에서 실수로 생성자를 호출하면 바로 알 수 있게 할 수도 있다.
```java
public class Utilz {
	private Utilz() {
		throw new AssertionError();
	}
}
```
