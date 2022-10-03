# 상속
```java
package vol1._10.inheritance;

public class Parent {
	public Parent() {
		System.out.println("Parent Constructor");
	}

	public void printName() {
		System.out.println("Parent printName()");
	}
}
```
```java
package vol1._10.inheritance;

public class Child extends Parent {
	public Child() {
		System.out.println("Child Constructor");
	}
}
```
```java
package vol1._10.inheritance;

public class InheritancePrint {
	public static void main(String[] args) {
		Child child = new Child();
		child.printName();
	}
}
```
```text
Parent Constructor
Child Constructor
Parent printName()
```
- 부모 클래스에서는 기본 생성자를 만들어 놓는 것 이외에는 상속을 위해서 아무런 작업을 할 필요는 없다.
- 자식 클래스는 클래스 선언시 extends 다음에 부모 클래스 이름을 적어준다.
- 자식 클래스에서는 부모 클래스에 있는 public, protected로 선언된 모든 인스턴스 및 클래스 변수와 메소드 사용 가능

## 상속 존재 이유
- 부모 클래스가 갖고 있는 변수와 메소드를 상속받음으로써, 개발할 때 이중, 삼중의 일을 안해도 된다.
- 즉, 상속이라는 개념이 없으면, 위 코드에서 모든 클래스에 공통적인 printName() 메소드를 만들어주어야 하고,
고쳐야 하는 상황에서 일일이 고쳐 주어야 하는 상황이 발생한다.

## 상속과 생성자
- 앞에서 부모 클래스에서는 기본 생성자를 만들어 놓는 것 이외에는 상속을 위해서 아무런 작업을 할 필요는 없다. 라고 했다.
- 정확히는 다른 매개 변수를 가지는 생성자가 없다면 기본 생성자는 자동으로 컴파일러가 만들어 주기 떄문이다.
- 매개 변수가 있는 생성자만 만들 경우, 기본 생성자가 없다는 에러가 발생한다. 두 가지 방법으로 해결할 수 있다.
1. 부모 클래스에 "매개 변수가 없는" 기본 생성자를 만든다.
2. 자식 클래스에서 부모 클래스의 생성자를 명시적으로 지정하는 super()를 사용한다.

```java
package vol1._10.inheritance;

public class ParentArg {
	public ParentArg(String name) {
		System.out.println("ParentArg(" + name + ") Constructor");
	}

	public void printName() {
		System.out.println("ParentArg printName()");
	}
}
```
```java
package vol1._10.inheritance;

public class ChildArg extends ParentArg {
	public ChildArg() {
		super("ChildArg");
		System.out.println("ChildArg Constructor");
	}
}
```
- super()로 사용하면 부모 클래스의 생성자를 호출한다는 것을 의미한다. super.printName() 처럼
메소드 호출 또한 가능하다.
- 기본 생성자가 없을 때, 실행오류가 발생하는 이유도 자식 클래스의 생성자에는 지정하지 않아도, 자식 클래스를
컴파일할 때 자동으로 super() 라는 문장이 들어가기 때문이다. 
- 위와 같이 super("ChildArg") 라고 지정하면 ParentArg 클래스의 생성자 중 String을 매개 변수로 받는 생성자를 찾아
호출된다. 여기서 참조 자료형을 매개 변수로 받는 생성자가 하나 더 있고, null을 호출한다면 어떻게 될까?
```java
package vol1._10.inheritance;

public class ParentArg {
	public ParentArg(String name) {
		System.out.println("ParentArg(" + name + ") Constructor");
	}

	public ParentArg(InheritancePrint obj) {
		System.out.println("ParentArg(InheritancePrint) Constructor");
	}

	public void printName() {
		System.out.println("ParentArg printName()");
	}
}
```
```java
package vol1._10.inheritance;

public class ChildArg extends ParentArg {
	public ChildArg() {
		// super("ChildArg");
		super(null);
		System.out.println("ChildArg Constructor");
	}
}
```
```text
_10/inheritance/ChildArg.java:6: error: reference to ParentArg is ambiguous
                super(null);
                ^
  both constructor ParentArg(String) in ParentArg and constructor ParentArg(InheritancePrint) in ParentArg match
```
- `reference to ParentArg is ambiguous` 즉, ParentArg 클래스로의 참조가 매우 모호하다 라는 의미이다.
- super()를 사용하여 생성자를 호출할 때에는 모호하게 null을 넘기는 것보다는 호출하고자 하는 생성자의 기본 타입을 
넘겨주는 것이 좋다. 위 코드에서는 null을 넘겨주면, String과 InheritancePrint 클래스 중 어떤 클래스를 찾아가야 하는지를
자바 컴파일러가 마음대로 정할 수가 없다.
- 그리고, 자식 클래스의 생성자에서 super()를 명시적으로 지정하지 않으면, 컴파일시 자동으로 super()가 추가된다. 그리고,
반드시 super()는 자식 클래스 생성자에서 가장 첫줄에 선언되어야만 한다.

# 메소드 Overriding
- 부모 클래스에 선언되어 있는 메소드와 동일한 메소드를 자식 클래스에서 선언해서 사용할 수 있다.
- 자식 클래스에서 부모 클래스에 있는 메소드와 동일하게 선언하는 것을 메소드 Overriding이라 말하며, 접근 제어자, 리턴 타입,
메소드 이름, 매개 변수 타입 및 개수가 모두 동일해야만 "메소드 Overriding"이라고 부른다. 
- 메소드 이름, 매개 변수의 타입 및 개수를 통틀어 시그니처라고 부른다. 즉, 동일한 시그니처를 가진다.
- 리턴 타입이 같지 않을 경우, 컴파일 에러 발생.
- 접근 제어자의 경우, 접근 권한이 확장되면 가능, 축소되면 불가능하다.(축소시, 컴파일 에러) 
(public > protected > package-private > private)

# 참조 자료형의 형 변환
```java
package vol1._10.inheritance;

public class ParentCasting {
	public ParentCasting() {
	}

	public ParentCasting(String name) {
	}

	public void printName() {
		System.out.println("printName() - Parent");
	}
}
```
```java
package vol1._10.inheritance;

public class ChildCasting extends ParentCasting {
	public ChildCasting() {
	}

	public ChildCasting(String name) {
	}

	public void printName() {
		System.out.println("printName() - Child");
	}

	public void printAge() {
		System.out.println("printAge() - 18 month");
	}
}
```
- 지금까지 객체를 생성할 떄에는 다음과 같이 만들었다.
```text
ParentCasting parent = new ParentCasting();
ChildCasting child = new ChildCasting();
```
- 그런데 상속 관계가 성립되면, 다음과 같이 객체 생성 가능. (반대는 불가능)
```text
ParentCasting obj = new ChildCasting();
```
- 자식 클래스인 ChildCasting 클래스에서는 부모 클래스인 ParentCasting 클래스에
있는 메소드와 변수들을 사용할 수 있지만, 반대는 불가능
- ChildCasting에 새로운 메소드나 변수가 있을 수 있기 때문.
- int에서 long으로 형 변환은 별도의 작업이 필요 없지만, long에서 int로의 변환은
  (int)라고 명시적으로 적어 주어야 했다. 데이터의 범위가 넓어지므로, 동일한 값이
된다는 보장이 없어 개발자가 "내가 이건 책임질게"라는 뜻이다.
- 참조 자료형 또한 자식 클래스의 타입을 부모 클래스의 타입으로 형 변환하면
부모 클래스에서 호출할 수 있는 메소드들은 자식 클래스에서도 호출할 수 있으므로 문제가 안된다.
따라서 명시적으로 해줄 필요 없다.
```java
private void objectCast() {
    ParentCasting parent = new ParentCasting();
    ChildCasting child = new ChildCasting();
    ParentCasting parent2 = child;
    ChildCasting child2 = parent;
}
```
- `ChildCasting child2 = parent;` 에서 incompatible types 컴파일 에러 발생.
- parent 객체는 ParentCasting 클래스의 객체이며, ChildCasting 클래스에 선언되어 있는 메소드나
변수를 완전히 사용할 수 없기 때문이다. 컴파일 오류를 피하려면 형 변환을 해야 한다.
```text
Exception in thread "main" java.lang.ClassCastException: class vol1._10.inheritance.ParentCasting cannot be cast to class vol1._10.inheritance.ChildCasting (vol1._10.inheritance.ParentCasting and vol1._10.inheritance.ChildCasting are in unnamed module of loader 'app')
	at vol1._10.inheritance.InheritanceCasting.objectCast(InheritanceCasting.java:13)
	at vol1._10.inheritance.InheritanceCasting.main(InheritanceCasting.java:6)
```
- 이와 같이 예외 발생. parent는 ParentCasting 클래스의 객체라서 ChildCasting 객체로 사용할 수 없다.
그렇다면 언제 형 변환을 해도 문제가 없을까?
```java
private void objectCast2() {
    ChildCasting child = new ChildCasting();
    ParentCasting parent2 = child;
    ChildCasting child2 = (ChildCasting)parent2;
}
```
- parent2는 child를 대입한 것이고, child는 원래 ChildCasting 클래스의 객체다.
- 겉모습은 ParentCasting 클래스의 객체인 것처럼 보이지만, 실제로는 ChildCasting
클래스의 객체이기 때문에 parent2를 ChildCasting 클래스로 형 변환해도 전혀 문제는 없다.
- 다음으로 instanceof이다.

## instanceof
```java
private void objectCastArray() {
    ParentCasting[] parentArray = new ParentCasting[3];
    parentArray[0] = new ChildCasting();
    parentArray[1] = new ParentCasting();
    parentArray[2] = new ChildCasting();
    objectTypeCheck(parentArray);
}

private void objectTypeCheck(ParentCasting[] parentArray) {
    for (ParentCasting tempParent : parentArray) {
        if (tempParent instanceof ChildCasting) {
            System.out.println("ChildCasting");
        } else if (tempParent instanceof ParentCasting) {
            System.out.println("ParentCasting");
        }
    }
}
```
```text
ChildCasting
ParentCasting
ChildCasting
```
- instanceof로 타입을 확인한 후에 명시적으로 형 변환을 하면 문제가 없다.
```java
private void objectTypeCheck(ParentCasting[] parentArray) {
    for (ParentCasting tempParent : parentArray) {
        if (tempParent instanceof ChildCasting) {
            System.out.println("ChildCasting");
            ChildCasting tempChild = (ChildCasting) tempParent;
            tempChild.printAge();
        } else if (tempParent instanceof ParentCasting) {
            System.out.println("ParentCasting");
        }
    }
}
```
- 단 여기서 유의점은, ChildCasting의 경우 ParentCasting 타입 검사에서도 true를 반환한다.
- 따라서 instanceof를 사용하여 타입을 점검할 때에는 가장 하위에 있는 자식 타입부터
확인을 해야 제대로 타입 점검이 가능하다.

# Polymorphism
- 다형성이라고 하는 Polymorphism은 형태가 다양하다라는 말이다. 

- 다형성은 형 변환을 하더라도, 실제 호출되는 것은 원래 객체에 있는 메소드가 호출된다는 의미이다.

```java
package vol1._10.inheritance;

public class Parent {
	public Parent() {
		System.out.println("Parent Constructor");
	}

	public void printName() {
		System.out.println("Parent printName()");
	}
}
```
```java
package vol1._10.inheritance;

public class Child extends Parent {
	public Child() {
		System.out.println("Child Constructor");
	}
}
```
```java
package vol1._10.inheritance;

public class ChildOther extends Parent {
	public ChildOther() {
		System.out.println("ChildOther Constructor");
	}

	@Override
	public void printName() {
		System.out.println("ChildOther - printName()");
	}
}
```
```java
package vol1._10.inheritance;

public class InheritancePoly {
	public static void main(String[] args) {
		InheritancePoly poly = new InheritancePoly();
		poly.callPrintName();
	}

	private void callPrintName() {
		Parent p1 = new Parent();
		Parent p2 = new Child();
		Parent p3 = new ChildOther();

		p1.printName();
		p2.printName();
		p3.printName();
	}
}
```
```text
Parent Constructor
Parent Constructor
Child Constructor
Parent Constructor
ChildOther Constructor
Parent printName()
Parent printName()
ChildOther - printName()
```
- 위 코드와 같이 모두 Parent 타입으로 선언되어 있지만, printName() 메소드의 결과는 상이하다.
선언시에는 모두 Parent 타입이지만, 실제 호출된 메소드는 생성자를 사용한 클래스에 있는 것이 호출되었다.
각 객체의 실제 타입은 다르기 때문이다.
- 이와 같이 "형 변환을 하더라도, 실제 호출되는 것은 원래 객체에 있는 메소드가 호출된다."는 것이 바로 다형성이다.
