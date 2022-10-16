# 실수를 방지할 수 있도록 도와주는 제네릭
- Junit 과 같은 테스트 코드를 작성할 수 있다. 이런 테스트를 열심히 해도 개발자가 미처 생각지 못한 부분에 대해서는 테스트 케이스를 만들지 못할 수도 있다.
- 자바에서는 여러 타입들이 존재하기 때문에, 형 변환을 하면서 많은 예외가 발생할 수 있다. 
```java
package vol2._21.generic;

import java.io.Serializable;

public class CastingDTO implements Serializable {
	private Object object;

	public void setObject(Object object) {
		this.object = object;
	}

	public Object getObject() {
		return object;
	}
}
```
- 먼저 이렇게 private 변수, getter, setter, Serializable 구현을 해야만 제대로 된 DTO 클래스라고 할 수 있다.
```java
package vol2._21.generic;

public class GenericSample {
	public static void main(String[] args) {
		GenericSample sample = new GenericSample();
		sample.checkCastingDTO();
	}

	public void checkCastingDTO() {
		CastingDTO dto1 = new CastingDTO();
		dto1.setObject(new String());

		CastingDTO dto2 = new CastingDTO();
		dto2.setObject(new StringBuffer());

		CastingDTO dto3 = new CastingDTO();
		dto3.setObject(new StringBuilder());
	}
}
```
- ObjectCheckSample 클래스의 checkCastingDTO() 메소드에 dto1 ~ 3 까지의 CastingDTO 클래스의 객체를 만들었다.
Object 클래스는 모든 클래스의 부모 클래스이므로, 각 객체에 String, StringBuffer, StringBuilder를 지정해도 컴파일과 실행 모두 문제 없이 된다.
- 문제는 저장되어 있는 값을 꺼낼 때 발생한다. 각 객체의 getObject() 메소드를 호출했을 때 리턴값으로 넘어오는 타입은 Object이다. 그래서, 다음과 같이
String, StringBuffer, StringBuilder 타입으로 각 형 변환을 해야만 한다.
```text
String temp1 = (String)dto1.getObject();
StringBuffer temp2 = (StringBuffer)dto2.getObject();
StringBuilder temp3 = (StringBuilder)dto3.getObject();
```
- 여기서 dto2가 StringBuilder 인지, StringBuffer 인지 혼동될 경우 instanceof 예약어를 사용하여 타입을 점검하면 된다.
```java
public void checkDTO(CastingDTO dto) {
    Object tempObject = dto.getObject();
    if (tempObject instanceof StringBuilder) {
        System.out.println("StringBuilder");
    } else if (tempObject instanceof StringBuffer) {
        System.out.println("StringBuffer");
    } else if (tempObject instanceof String) {
        System.out.println("String");
    }
}
```
- 그런데 꼭 이렇게 점검을 해야 할까? 이러한 단점을 보완하기 위해 자바 5부터 제네릭 (Generic)이 추가되었다.

# 제네릭이 뭐지?
- 앞 절에서 살펴본 형 변환에서 발생할 수 있는 문제점을 "사전"에 없애기 위해서 만들어졌다. 여기서 "사전"이라고 하는 것은 실행시에 예외가 발생하는 것을
처리하는 것이 아니라, 컴파일할 때 점검할 수 있도록 한 것을 말한다.
```java
package vol2._21.generic;

import java.io.Serializable;

public class CastingDTO implements Serializable {
	private Object object;

	public void setObject(Object object) {
		this.object = object;
	}

	public Object getObject() {
		return object;
	}
}
```
- 이렇게 Object이므로 어떤 타입이든 사용할 수 있다. 이 클래스를 제네릭으로 선언하면 다음과 같다.
```java
package vol2._21.generic;

import java.io.Serializable;

public class CastingGenericDTO<GodOfJava> implements Serializable {
	private GodOfJava object;

	public void setObject(GodOfJava object) {
		this.object = object;
	}

	public GodOfJava getObject() {
		return object;
	}
}
```
- 이처럼 꺽쇠 안에는 현존하는 클래스 또는 존재하지 않는 클래스를 사용해도 된다. 단, 클래스 이름의 명명 규칙과 동일하게 지정하는 것이 좋다.
- 아래와 같이 사용할 수 있다.
```java
public void checkGenericDTO() {
    CastingGenericDTO<String> dto1 = new CastingGenericDTO<>();
    dto1.setObject(new String());
    CastingGenericDTO<StringBuffer> dto2 = new CastingGenericDTO<>();
    dto2.setObject(new StringBuffer());
    CastingGenericDTO<StringBuilder> dto3 = new CastingGenericDTO<>();
    dto3.setObject(new StringBuilder());
}
```
- 그리고 이 객체를 가져올 때에는 다음과 같이 간단해진다.
```java
String temp1 = dto1.getObject();
StringBuffer temp2 = dto2.getObject();
StringBuilder temp3 = dto3.getObject();
```
- 형 변환을 할 필요가 없어진 것을 볼 수 있다. 제네릭 타입은 각각 String, StringBuffer, StringBuilder이기 때문이다.
- 만약 잘못된 타입으로 치환하면 컴파일 자체가 안 된다. 따라서 "실행시"에 다른 타입으로 잘못 형 변환하여 예외가 발생하는 일은 없다.
- 이와 같이 명시적으로 타입을 지정할 때 사용하는 것이 제네릭이다.

# 제네릭 타입의 이름 정하기
- 어떤 단어가 <> 내에 들어가도 상관 없지만 자바에서 정의한 기본 규칙은 다음과 같다.
  - E : 요소 (Element, 자바의 컬렉션에서 주로 사용됨)
  - K : 키
  - N : 숫자
  - T : 타입
  - V : 값
  - S, U, V : 두 번쨰, 세 번째, 네 번째 선언된 타입
- 꼭 지켜야 컴파일이 되는 것은 아니지만 이 규칙을 따라야 쉽게 다른사람을 이해시킬 수 있다.

# 제네릭에 ?가 있는 것은 뭐야?
- 제네릭을 사용할 때 <>에는 어떤 타입도 되는데, 메소드의 매개 변수로 넘어가는 제네릭에 대해 알아보자.
```java
package vol2._21.generic;

public class WildcardGeneric<W> {
	W wildcard;

	public W getWildcard() {
		return wildcard;
	}

	public void setWildcard(W wildcard) {
		this.wildcard = wildcard;
	}
}
```
```java
package vol2._21.generic;

public class WildcardSample {
	public static void main(String[] args) {
		WildcardSample sample = new WildcardSample();
		sample.callWildcardMethod();
	}

	public void callWildcardMethod() {
		WildcardGeneric<String> wildcard = new WildcardGeneric<String>();
		wildcard.setWildcard("A");
		wildcardStringMethod(wildcard);
	}

	public void wildcardStringMethod(WildcardGeneric<String> c) {
		String value = c.getWildcard();
		System.out.println(value);
	}
}
```
- 위 코드는 wildcardStringMethod()에서 String을 사용하는 WildcardGeneric 객체만 받을 수 있다.
- 만약 다른 타입으로 선언된 WildcardGeneric 객체를 받으려면 다음과 같이 선언하는 것이 가능하다.
```java
public void wildcardStringMethod(WildcardGeneric<?> c) {
    Object value = c.getWildcard();
    System.out.println(value);
}
```
- 이렇게 ? 로 명시한 타입을 wildcard 라고 부른다. 만약 넘어오는 타입이 2-3가지라면 instanceof 예약어를 사용하여 해당 타입을 확인하면 된다.
```java
public void wildcardStringMethod(WildcardGeneric<?> c) {
    Object value = c.getWildcard();
    if (value instanceof String) {
        System.out.println(value);
    }
}
```
- 그리고 wildcard는 메소드의 매개 변수로만 사용하는 것이 좋다. 아래와 같이 사용하면 정상적인 컴파일이 되지 않는다.
```text
public void callWildcardMethod() {
    WildcardGeneric<?> wildcard = new WildcardGeneric<String>();
    wildcard.setWildcard("A");
    wildcardStringMethod(wildcard);
}
```
- 즉, 알 수 없는 타입에 String을 지정할 수 없다는 말이다. 다시 말해, 어떤 객체를 wildcard로 선언하고, 그 객체의 값은 가져올 수 있지만, 와일드 카드로
객체를 선언했을 떄에는 특정 타입으로 값을 지정하는 것은 "불가능"하다.

# 제네릭 선언에 사용하는 타입의 범위도 지정할 수 있다.
- "?" 대신 "? extends 타입"으로 사용하는 것이다.
```java
package vol2._21.generic;

public class Car {
	protected String name;

	public Car(String name) {
		this.name = name;
	}

	@Override
	public String toString() {
		return "Car name=" + name;
	}
}
```
```java
package vol2._21.generic;

public class Bus extends Car {
	public Bus(String name) {
		super(name);
	}

	@Override
	public String toString() {
		return "Bus name=" + name;
	}
}
```
- Car 클래스와 Car 클래스를 상속받은 Bus 클래스를 만들고 예제를 만들어보자.
```java
package vol2._21.generic;

public class CarWildcardSample {
	public static void main(String[] args) {
		CarWildcardSample sample = new CarWildcardSample();
		sample.callBoundedWildcardMethod();
	}

	public void callBoundedWildcardMethod() {
		WildcardGeneric<Car> wildcard = new WildcardGeneric<>();
		wildcard.setWildcard(new Car("Mustang"));
		boundedWildcardMethod(wildcard);
	}

	public void boundedWildcardMethod(WildcardGeneric<? extends Car> c) {
		Car value = c.getWildcard();
		System.out.println(value);
	}
}
```
```text
Car name=Mustang
```
- boundedWildcardMethod()의 매개 변수에 "?" 대신 "? extends Car"라고 적어주었다. 이렇게 정의한 것은 제네릭 타입으로 Car를 상속받은 모든 클래스를
사용할 수 있다는 의미다. 다른 타입을 제네릭 타입으로 선언한 객체가 넘어올 수 없다. 즉, Car와 Car를 상속한 클래스가 넘어와야 한다.
- Car를 상속받은 Bus 클래스를 알아보자.
```java
public void callBusBoundedWildcardMethod() {
    WildcardGeneric<Bus> wildcard = new WildcardGeneric<>();
    wildcard.setWildcard(new Bus("7000"));
    boundedWildcardMethod(wildcard);
}
```
```text
Bus name=7000
```
- "? extends 타입" 같은 것을 "Bounded Wildcards"라고 부른다. Bound라는 말은 "경계"라는 의미도 있기 때문에, 매개 변수로 넘어오는 제네릭 타입의 경계를
지정하는 데 사용한다는 의미이다.
- "?"로 사용하는 wildcard와 마찬가지로 Bounded Wildcards 로 선언한 타입에는 값을 할당할 수 없다. 조회용 매개 변수로 사용해야 한다.

# 메소드를 제네릭하게 선ㅇ언하기
- wildcard로 메소드를 선언하였을 때 매개 변수로 사용된 객체에 값을 추가할 수 없었다. 그 방법을 알아보자.
```java
package vol2._21.generic;

public class GenericWildcardSample {
	public static void main(String[] args) {
		GenericWildcardSample sample = new GenericWildcardSample();
		sample.callGenericMethod();
	}

	public void callGenericMethod() {
		WildcardGeneric<String> wildcard = new WildcardGeneric<>();
		genericMethod(wildcard, "Data");
	}

	public <T> void genericMethod(WildcardGeneric<T> c, T addValue) {
		c.setWildcard(addValue);
		T value = c.getWildcard();
		System.out.println(value);
	}
}
```
```text
Data
```
- 메소드 선언부에 리턴 타입 앞에 <> 로 제네릭 타입을 선언해 놓았다. 그리고 매개 변수에서 그 제네릭 타입이 포함된 객체를 받아서 처리하였다.
메소드 내에선 setWildcard() 메소드를 통하여 값을 할당하였다.
- 이렇게 ?를 사용하는 Wildcard 처럼 타입을 두리뭉실하게 적는 것 보다는 이처럼 명시적으로 메소드 선언시 타입을 지저앻 주면 더 견고한 코드를 작성할 수 있다.
- Bounded Wildcard 또한 사용 가능하다.
```text
public <T extends Car> voi boundedGenericMethod(WildcardGeneric<T> c, T addValue)
```
- 제네릭 타입이 두 개 일때에는 콤마로 구분하여 나열하면 된다.
```text
public <S, T extends Car> void multiGenericMethod(WildcardGeneric<T> c, T addValue, S another)
```
- 이렇게 S, T라는 제네릭 타입을 메소드에서 사용할 수 있다.
