# 참조 자료형
- 기본 자료형 (Primitive type) 8개 byte, short, int, long, float, double char, boolean을 제외하고는
모두 참조 자료형이다.

# 생성자
- 생성자는 자바 클래스의 객체 (또는 인스턴스)를 생성하기 위해서 존재
- 생성자는 개수의 한계가 없지만, 알아보기 쉽게 필요한 만큼만 만드는 것이 좋다.

## DTO
- Data Transfer Object의 약자로, 어떤 속성을 갖는 클래스를 만들고, 그 속성들을 쉽게 전달하기 위해서 만든다.
비슷한 클래스로 VO(Value Object)가 있다.
- 자바는 메소드 선언시 리턴 타입은 한 가지만 선언할 수 있는데, 복합적인 데이터를 리턴하기 위해 DTO를 사용

## this
- this 예약어는 말 그대로 "이 객체"의 의미로, 생성자와 메소드에서 사용가능
```java
public class MemberDTO {
    public String name;
    public String phone;
    public String email;
    public MemberDTO(String name) {
        this.name = name;
    }
}
```
- this 가 없을 경우, 컴파일러는 중괄호 안에 있는 name은 모두 매개 변수로 넘겨준 name이라고 생각할 것이다.
- 이름을 다르게 하는 것보다, 간단한 방법이 this를 사용하는 것.
- 즉, 위 코드에서 this.name 의 경우, "이 객체의 name"이라고 명시적으로 지정해 주는 것이다.

# 메소드 Overloading
- 메소드의 이름을 같도록 하고, 매개 변수의 타입, 개수를 다르게 하는 것.
- 같은 역할을 하는 메소드는 같은 메소드 이름을 가져야 한다는 모토로 사용하는 것

# static 메소드와 일반 메소드의 차이
- static 메소드는 객체를 생성하지 않고서도 메소드를 호출할 수 있다.
- static 메소드는 클래스 변수만 사용할 수 있다. 그렇다고 무작정 인스턴스 변수에 static을 붙여 클래스 변수를 만들면 안된다.
  - 클래스 변수가 되면 모든 객체에서 하나의 값을 바라보기 때문이다.

# static 블록
- 객체는 여러 개 생성하지만, 한 번만 호출되어야 하는 코드가 있다면 static 블록을 사용하면 된다.
- 클래스 내에 선언되어 있어야 하며, 메소드 내에서는 선언할 수가 없고, 인스턴스 변수나 클래스 변수와 같이 어떤 메소드나 생성자에 속해 있으면 안 된다.
- 클래스를 초기화할 때 꼭 수행되어야 하는 작업이 있을 경우 유용
- 추가로, static한 것들만 호출 가능

# Pass by value, Pass by reference
- Pass by Value의 경우 값을 전달한다는 뜻으로, 원래 값은 놔두고, 전달되는 값이 진짜인 것처럼 보이게 한다.
그래서 매개 변수를 받은 메소드에서 그 값을 바꾸어도 원래의 값은 변하지 않는다. 기본 자료형은 무조건 Pass by Value로 데이터를 전달
```java
public class ReferencePass {
	public static void main(String[] args) {
		ReferencePass reference = new ReferencePass();
		reference.callPassByValue();
	}

	private void callPassByValue() {
		int a = 10;
		String b = "b";
		System.out.println("before passByValue");
		System.out.println("a=" + a);
		System.out.println("b=" + b);
		passByValue(a, b);
		System.out.println("after passByValue");
		System.out.println("a=" + a);
		System.out.println("b=" + b);

	}

	private void passByValue(int a, String b) {
		a = 20;
		b = "z";
		System.out.println("in passByValue");
		System.out.println("a=" + a);
		System.out.println("b=" + b);
	}
}
```
```text
before passByValue
a=10
b=b
in passByValue
a=20
b=z
after passByValue
a=10
b=b
```
- 여기서 String은 참조 자료형인데, 값이 바뀐 변경되지 않은 이유는, String의 경우 큰 따옴표로 할당하면 new를 사용하여 객체를
생성한 것과 같다. String이 아닌 다른 참조 자료형들도, 호출된 메소드에서 다른 객체로 대체하여 처리한다면, 값이 변경되지 않는다.
- String이 아닌 참조 자료형의 경우, 매개 변수로 받은 참조 자료형 안에 있는 객체를 변경하면, 호출한 참조 자료형 안에 있는 객체는
호출된 메소드에서도 변경이 된다. 이를 Pass by Reference라고 한다.

```java
package vol1._8;

public class ReferencePass {
	public static void main(String[] args) {
		ReferencePass reference = new ReferencePass();
		// reference.callPassByValue();
		reference.callPassByReference();
	}

    // ...
	public void callPassByReference() {
		MemberDTO member1 = new MemberDTO("Sangmin");
		System.out.println("Before passByReference");
		System.out.println("member.getName() = " + member1.getName());
		passByReference(member1);
		System.out.println("After passByReference");
		System.out.println("member.getName() = " + member1.getName());
	}

	private void passByReference(MemberDTO member) {
		member.name = "Kim";
		System.out.println("in passByReference");
		System.out.println("member.getName() = " + member.getName());
	}
}
```
```text
Before passByReference
member.getName() = Sangmin
in passByReference
member.getName() = Kim
After passByReference
member.getName() = Kim
```
- 하지만 자바는 Pass by Value 이다. 무슨 말일까? 
- 위 passByReference() 메소드 아래에 코드 한줄을 추가하고 실행해보았다.
```java
private void passByReference(MemberDTO member) {
    member.name = "Kim";
    System.out.println("in passByReference");
    System.out.println("member.getName() = " + member.getName());
    member = new MemberDTO("Lee");
}
```
```text
Before passByReference
member.getName() = Sangmin
in passByReference
member.getName() = Kim
After passByReference
member.getName() = Kim
```
- new로 새로운 객체를 생성해 Lee로 바꾼 결과는 해당 메소드를 호출한 메소드에서는 무시되었다.
- 위 코드에서 MemberDTO reference 타입이 passByReference에 member1라는 이름으로 넘어간다. 
- 이렇게 되면 member1의 객체 위치를 가리키는 정확한 복사본이 힙 메모리에 생기게 된다. (모든 객체는 힙 메모리에 저장됨)  
위 코드에서는 passByReference() 내의 member.
- 따라서 passByReference() 메소드에서 member 내의 객체를 변경하게 되면 같은 값을 가리키기 때문에, 원래 호출한 메소드 내에서도
값이 변경되는 것이다.
- 여기서, 만약 `member = new MemberDTO("Lee");`와 같이 새로운 객체를 할당하게 되면 같은 힙 메모리를 가리키던 것이 
새로운 객체를 가리키게 되어 변경값이 반영되지 않는 것이다.
