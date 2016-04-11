#Overriding hashCode
**_equals 메서드를 재정의하는 경우 반드시 hashCode도 재정의해야 한다_**
##hashCode 재정의 시 준수해야 할 일반 규약
- 같은 객체의 hashCode를 여러 번 호출하는 경우, equals가 사용하는 정보들에 변화가 앖다면 hashCode는 늘 같은 integer를 반환해야 한다.
- `x.equals(y)`가 `true`를 반환할 경우 x, y의 hashCode 값은 같아야 한다.
- `x.equals(y)`가 `false`인 경우 x, y의 hashCode가 상이해야 하는 것은 아니나, 상이할 경우 hashtable의 성능이 향상될 수 있다.

##주의할 점
- 성능 개선을 위해 객체의 중요 부분을 hashCode 계산 과정에서 생략해서는 안된다.