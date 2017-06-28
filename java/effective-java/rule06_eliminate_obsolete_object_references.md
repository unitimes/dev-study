# Eliminate obsolete object references
- Java도 메모리 누수를 신경써야 한다.
- 참조되고 있는 객체는 garbage collecting 대상이 될 수 없다.
- 더 이상 사용하지 않는 객체에 대한 참조는 반드시 제거해야 한다.
	- 객체를 참조하는 변수의 scope을 활용하라.
		- 예) 특정 메소드 안에서만 사용되는 객체를 클래스의 멤버변수에 할당하고, 사용 후 null 처리를 하지 않으면, 해당 참조는 클래스의 생명 주기동안 유지 된다.
