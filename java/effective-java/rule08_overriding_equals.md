#Overriding equals
equals 메소드를 잘못 정의하면 해당 규약에 의존하는 클래스들(HashMap, HashSet 등)과 함께 사용할 때 문제가 발생할 수 있다. 때문에 **_필요한 경우_**에만 재정의하는 것이 좋다.
> equals를 재정의 하지 않는 경우 객체는 오직 자기 자신과만 같다. 즉 논리적 동일성을 따지지 않는다.

##equals 메서드 정의 시 준수해야 할 일반 규약
###반사성(reflexive)
null이 아닌 참조 x는, `x.equals(x)`가 `true`를 반환해야 한다.
###대칭성(symmetric)
참조 x, y가 null이 아닐 때, `x.equals(y)`가 `true`일 때만 `y.equals(x)`가 `true`를 반환해야 한다.
###추이성(transitive)
참조 x, y, z가 null이 아닐 때, `x.equals(y)`가 `true`, `y.equals(z)`가 `true`이면 `x.equals(z)`도 `true`를 반환해야 한다.
###일관성(consistent)
참조 x, y가 null이 아닐 때 그리고 equals를 통해 비교되는 정보에 아무런 변화가 없을 때 `x.equals(y)`의 결과는 항상 같아야 한다.
###null과의 관계
x가 null이 아닐 때, `x.equals(null)`은 항상 `false`여야 한다.

##알아두어야 할 것
####객체 생성 가능 클래스를 extends 하여 새로운 값 컴포넌트를 추가하면서 equals규약을 어기지 않을 방법은 없다.
####신뢰성이 보장 되지 않는 자원들을 비교하는 equals를 구현하지 말아라.
- JVM의 메모리에 존재하지 않는 자원들은 언제나 같은 결과가 나온다고 보장할 수 없다. 때문에 equals를 구현해도 원하는 결과를 보장하기 어렵다.

####equals를 구현할 때는 hashCode도 재정의하라
####너무 비용을 투자하지 마라
####equals 메소드의 인자형을 바꾸지 마라
- 그렇게 되면 재정의가 아니다

####비용을 줄일 수 있는 방법을 생각하라
- `==` 비교를 처음으로 수행하여 `true`이면 바로 `true`반환
- `instanceof`를 사용하여 자료형이 정확한지 검사
	- 자료형이 다르면 `false` 반환