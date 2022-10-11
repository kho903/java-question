1. 인터페이스와 abstract 클래스는 왜 시용할까요?
- 설계시 선언해 두면 개발할 때 기능을 구현하는 데에만 집중할 수 있습니다.
- 개발자의 역량에 따른 메소드의 이름과 매개 변수 선언의 격차를 줄일 수 있습니다.
- 공통적인 인터페이스와 abstract 클래스를 선언해 놓으면 선언과 구현을 구분할 수 있습니다.

2. 인터페이스에서 private 메소드는 작성가능할까요? 어떻게 작성할까요?
- 자바9부터 private 메소드를 작성할 수 있고, body를 포함해야 합니다.
- static, non-static 모두 가능
```java
package vol1.interfaceex;

public interface Foo {

	default void bar() {
		System.out.print("Hello");
		baz();
	}

	private void baz() {
		System.out.println(" world!");
	}

	static void buzz() {
		System.out.print("Hello");
		staticBaz();
	}

	private static void staticBaz() {
		System.out.println(" static world!");
	}
}
```
```java
package vol1.interfaceex;

public class CustomFoo implements Foo {

	public static void main(String... args) {
		Foo customFoo = new CustomFoo();
		customFoo.bar();
		Foo.buzz();
	}
}
```
```text
Hello world!
Hello static world!
```
- 이와 같이 사용 가능합니다.
- 구현에 대한 세부 정보를 숨길 수 있습니다. 따라서 캡슐화가 가능합니다.
- 또 다른 이점은 다른 private 메소드처럼 유사한 기능을 가진 메서드의 인터페이스에 중복이 적고 재사용 가능한 코드가 더 많이 추가된다는 것입니다.
