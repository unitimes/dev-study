#Reusing Objects
- 기능적으로 동일한 객체는 재사용을 고려할 것
	- 변경 불가능 객체는 강력한 재사용 후보
- 비교에 필요한 상수 역할의 객체는 정적으로 사용할 수 있을 지 고려할 것
	- 아래의 예에서 `boomStart`,  `boomEnd`,  `gmtCal`은 정적으로 생성하여 사용 할 수 있다.
```java
public boolean isBabyBoomer() {
	Calendar gmtCal = Calendar.getInstance(TimeZone.getTimeZone("GMT"));
	gmtCal.set(1946, Calendar.JANUARY, 1, 0, 0, 0);
	Date boomStart = gmtCal.getTime();
	gmtCal.set(1965, Calendar.JANUARY, 1, 0, 0, 0);
	Date boomEnd = gmtCal.getTime();
	return birthDate.compareTo(boomStart) >= 0 && birthDate.compareTo(boomEnd) < 0;
}
```
- 자동 객체화(autoboxing)의 비용을 고려할 것
	- 자동 객체화는 기본 자료형과 객체 표현형을 섞어 사용할 수 있도록 형 변환을 자동으로 해주는 것
- 기본 자료형의 객체 표현형을 사용하는 것은 당연히 객체 생성 비용이 발생한다.
- _**새로운 객체를 만들어야 한다면 기존 객체는 재사용하지 말 것**_
	- 재사용을 할 수는 있으나 더 많은 비용이 들 것
- 재사용을 할 지 말 지를 결정할 때 항상 _**비용**_을 고려할 것
