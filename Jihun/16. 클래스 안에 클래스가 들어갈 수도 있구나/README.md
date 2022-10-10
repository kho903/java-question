# 클래스 안의 클래스
- 자바에서 클래스 안에 클래스를 Nested 클래스라고 부른다. 가장 큰 이유는 코드를 간단하게 표현하기 위함.
- 자바 기반 UI 처리를 할 때, 사용자의 입력이나, 외부의 이벤트에 대한 처리를 하는 곳에서 가장 많이 사용됨
- "Static nested 클래스"와 "내부(inner) 클래스"로 구분된다. (static으로 선언되었는지 여부)
- 내부 클래스는 또, "로컬(혹은 지역) 내부 클래스"라고 하고, 이름이 없는 클래스를 "익명 내부 클래스"라고 부른다. (일반적으로 내부 클래스와 익명 클래스라 부름)

## Nested 클래스를 만드는 이유
- 한 곳에서만 사용되는 클래스를 논리적으로 묶어서 처리할 필요가 있을 때 - Static Nested
- 캡슐화가 필요할 때 (예를 들어 A라는 클래스에 private 변수가 있다. 이 변수에 접근하고 싶은 B라는 클래스를 선언하고, B 클래스를 외부에 노출시키고 싶지 않을 경우 속함)
즉, 내부 구현을 감추고 싶을 때를 말한다. - inner 클래스
- 소스의 가독성과 유지보수성을 높이고 싶을 떄

## Static Nested 클래스의 특징
- 내부 클래스는 감싸고 있는 외부 클래스의 private 포함 어떤 변수도 접근할 수 있다.
- 하지만 Static nested 클래스를 그렇게 사용하는 것은 불가능하다. 이름 그대로 Static 하기 때문이다.
```java
package vol1._16.inner;

public class OuterOfStatic {
	static class StaticNested {
		private int value = 0;

		public int getValue() {
			return value;
		}

		public void setValue(int value) {
			this.value = value;
		}
	}
}
```
- 위 클래스를 컴파일하면 두개의 .class 파일이 만들어 진다.
```text
OuterOfStatic.class
OuterOfStatic$StaticNested.class
```
- 이 클래스의 객체는 어떻게 생성할까?
```java
package vol1._16.inner;

public class NestedSample {
	public static void main(String[] args) {
		NestedSample sample = new NestedSample();
		sample.makeStaticNestedObject();
	}

	private void makeStaticNestedObject() {
		OuterOfStatic.StaticNested staticNested = new OuterOfStatic.StaticNested();
		staticNested.setValue(3);
		System.out.println(staticNested.getValue());
	}
}
```
- 클래스 이름뒤에 .(점)을 찍고 사용할 수 있다. 객체를 생성한 이후에 사용하는 방법은 일반 클래스와 동일하다.
- 일반적으로 Static Nested 클래스는 클래스를 묶기 위해 사용한다. 만약 학교 관리를 위한 School 클래스를 만들고, 대학을 관리하는 University
클래스가 있을 때, Student라는 클래스를 만들면 School의 학생인지 University의 학생인지가 불분명해진다. 하지만 만약 School 내에 static nested
클래스인 Student를 만든다면, 이 클래스의 용도가 보다 명확해진다.

## 내부 클래스와 익명 클래스
```java
package vol1._16.inner;

public class OuterOfInner {
	class Inner {
		private int value = 0;

		public int getValue() {
			return value;
		}

		public void setValue(int value) {
			this.value = value;
		}
	}
}
```
- 앞에서 봤던 StaticNested와 Inner 클래스는 static 키워드를 제외하고는 모두 같다. 하지만 객체를 생성하는 방법이 다르다.
```java
package vol1._16.inner;

public class InnerSample {
	public static void main(String[] args) {
		InnerSample sample = new InnerSample();
		sample.makeInnerObject();
	}

	private void makeInnerObject() {
		OuterOfInner outer = new OuterOfInner();
		OuterOfInner.Inner inner = outer.new Inner();
		inner.setValue(3);
		System.out.println(inner.getValue());
	}
}
```
- Inner 클래스의 객체를 생성하기 전에는 먼저 Inner 클래스를 감싸고 있는 OuterOfInner 라는 클래스의 객체를 만들어야만 한다. 여기서는 outer라는 객체다.
- 그리고 이 outer 객체를 통해서 Inner 클래스의 객체를 만들어 낼 수 있다.
- 하나의 클래스에서 어떤 공통적인 작업을 수행하는 클래스가 필요한데 다른 클래스에서는 그 클래스가 전혀 필요가 없을 때 이러한 내부 클래스를 만들어 사용한다.
- GUI에서 내부 클래스들이 많이 사용되는 부분은 리스너(Listener)라는 것을 처리할 때다. 사용자가 버튼 클릭, 키보드 입력 등의 이벤트가 발생할 때
해야하는 작업을 정의하기 위해서 내부 클래스를 만들어 사용하게 된다.
- 그런데 하나의 애플리케이션에서 어떤 버튼이 눌렸을 때 수행해야 하는 작업은 대부분 상이하다. 그러니, 하나의 별도 클래스를 만들어 사용하는 것보다, 내부 클래스를
만드는 것이 훨씬 편하다.
- 그리고 내부 클래스보다 간단한 "익명 클래스"도 존재한다.
```java
package vol1._16.inner;

public class MagicButton {
	public MagicButton() {
	}

	private EventListener listener;

	public void setListener(EventListener listener) {
		this.listener = listener;
	}

	public void onClickProcess() {
		if (listener != null) {
			listener.onClick();
		}
	}
}
```
```java
package vol1._16.inner;

public interface EventListener {
	public void onClick();
}
```
- MagicButton 클래스의 setListener() 메소드에서 EventListener라는 것을 매개 변수로 받아 인스턴스 변수에 지정한다. 
- 그리고 여기서 사용하는 EventListener 또한 만들어 주었다. 위 코드를 사용하는 코드를 만들어보자.
```java
package vol1._16.inner;

public class AnonymousSample {
	public static void main(String[] args) {
		AnonymousSample sample = new AnonymousSample();
		sample.setButtonListener();
	}

	private void setButtonListener() {
		MagicButton button = new MagicButton();
		MagicButtonListener listener = new MagicButtonListener();
		button.setListener(listener);
		button.onClickProcess();
	}
	
	class MagicButtonListener implements EventListener {
		@Override
		public void onClick() {
			System.out.println("Magic Button Clicked!!");
		}
	}

}
```
- MagicButtonListener를 만든 후 setListener의 매개 변수로 넣어 주었다. 위와 같이 새로운 내부 클래스를 만들어 줄 수도 있지만 아래와
같이 익명 클래스를 만들 수도 있다.
```java
public void setButtonListenerAnonymous() {
    MagicButton button = new MagicButton();
    button.setListener(new EventListener() {
        @Override
        public void onClick() {
            System.out.println("Magic Button Clicked!!");
        }
    });
    button.onClickProcess();
}
```
- setListener() 메소드를 보면 new EventListener()로 생성자를 호출한 후 바로 중괄호를 열고 onClick() 메소드를 구현한 후 중괄호를 닫았따.
이것이 익명 클래스. 클래스에 이름이 없지만 onClick()과 같은 메소드가 구현되어 있다.
- 이렇게 구현했을 때에는 클래스 이름도 없고, 객체 이름도 없기 때문에 다른 클래스나 메소드에서는 참조할 수가 없어 재사용하려면
다음과 같이 객체를 생성한 후 사용하면 된다.
```java
public void setButtonListenerAnonymousObject() {
    MagicButton button = new MagicButton();
    EventListener listener = new EventListener() {
        @Override
        public void onClick() {
            System.out.println("Magic Button Clicked!!");
        }
    };
    button.setListener(listener);
    button.onClickProcess();
}
```
- 익명 클래스를 만들었을 때의 장점은 클래스를 만들고, 그 클래스를 호출하면 그 정보는 메모리에 올라간다. 즉, 클래스를 많이 만들면 만들수록 
메모리는 많이 필요해지고, 애플리케이션을 시작할 때 더 많은 시간이 소요된다.
- 따라서, 자바에서는 이렇게 간단한 방법으로 객체를 생성할 수 있도록 해놓았다. 그렇다고 클래스를 하나 더 만든다고 애플리케이션 시작 시간이
1초씩 더 걸리는 것을 아니지만, 줄일 수 있다면 줄이는 것이 좋다는 것이다.
- 재사용 가능성이 없을 때 만들어야 하고, 오히려 가독성이 떨어질 수 있어 남용하면 안 된다.

## Nested 클래스의 특징
- 알고 있어야 하는 사항은 참조 가능한 변수들이다.
```java
package vol1._16.inner;

public class NestedValueReference {
	public int publicInt = 0;
	protected int protectedInt = 1;
	int justInt = 2;
	private int privateInt = 3;
	static int staticInt = 4;

	static class StaticNested {
		public void setValue() {
			staticInt = 14;
		}
	}

	class Inner {
		public void setValue() {
			publicInt = 20;
			protectedInt = 21;
			justInt = 22;
			privateInt = 23;
			staticInt = 24;
		}
	}

	public void setValue() {
		EventListener listener = new EventListener() {
			@Override
			public void onClick() {
				publicInt = 30;
				protectedInt = 31;
				justInt = 32;
				privateInt = 33;
				staticInt = 34;
			}
		};
	}
}
```
- Static Nested 클래스에서는 감싸고 있는 클래스의 static 변수만 참조할 수 있다. StaticNested 클래스가 static으로 선언되어 있기 때문에 부모 
클래스애 static하지 않은 변수를 참조할 수는 없다.
- 즉, 여기서 publicInt, protectedInt, justInt, privateInt는 StaticNested 에서 참조 불가능하다.
- 내부 클래스와 익명 클래스는 감싸고 있는 클래스의 어떤 변수라도 참조할 수 있다.
- 반대로 감싸고 있는 클래스에서 Static Nested 클래스의 인스턴스 변수나 내부 클래스의 인스턴스 변수로의 접근하는 것은 가능하다.
```java
package vol1._16.inner;

public class ReferenceAtNested {
	static class StaticNested {
		private int staticNestedInt = 99;
	}

	class Inner {
		private int innerValue = 100;
	}

	public void setValue(int value) {
		StaticNested nested = new StaticNested();
		nested.staticNestedInt = value;
		Inner inner = new Inner();
		inner.innerValue = value;
	}
}
```
- 값이 private 일지라도, 각 클래스의 객체를 생성한 후 그 값을 참조하는 것은 가능하다.
