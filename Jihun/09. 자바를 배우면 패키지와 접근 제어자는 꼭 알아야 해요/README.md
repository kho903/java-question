# 패키지
## package c.javapackage;
- 소스의 가장 첫 줄에 있어야 한다.
- 소스 하나에는 하나만 있어야 한다.
- 패키지 이름과 위치한 폴더 이름이 같아야만 한다. (다를시, 컴파일러가 찾지 못함)
- java 로 패키지 이름을 시작해서는 안된다.

## 패키지 이름
- java : 자바 기본 패키지 (Java 벤더에서 개발)
- javax : 자바 확장 패키지 (Java 벤더에서 개발)
- org : 일반적으로 비 영리단체 (오픈 소스)의 패키지
- com : 일반적으로 영리단체 (회사)의 패키지

### 패키지 이름 유의점
- 패키지 이름은 모두 소문자로 지정. 반드시는 아니지만 약속
- 자바의 예약어 사용 금지. int, static 등 X

## import
- 자바에서는 패키지가 같은지 다른지에 따라 import 여부가 결정된다.
- 폴더 구조상 상위 패키지에 있는 클래스와 하위 패키지에 있는 클래스의 상관관계는 자바 언어 상에서 상관 없다.
- 그냥 같은 패키지에 있는지, 다른 패키지에 있는지 여부만 중요하다. 
- 같은 패키지 또는 java.lang 패키지에 있으면 import 할 필요가 없다.

### import static
- static 변수에 접근하거나 static 메소드를 사용할 때 import static을 사용한다.
- 예를 들어 SubStatic 클래스에 선언된 subStaticMethod() 메소드를 사용할 떄 명시적으로
SubStatic.subStaticMethod() 처럼 사용해야 하고, 클래스 변수인 CLASS_NAME을 사용한다고
컴파일러에게 이야기하기 위해서는 SubStatic.CLASS_NAME으로 사용해야만 한다. 하지만 import static을
사용할 때는 다음과 같이 지정한다.

```java
package vol1._9.javapackage;

import vol1._9.javapackage.sub.SubStatic;
import static vol1._9.javapackage.sub.SubStatic.subStaticMethod;
import static vol1._9.javapackage.sub.SubStatic.CLASS_NAME;

public class PackageStatic {
	public static void main(String[] args) {
		// SubStatic.subStaticMethod();
		// System.out.println(SubStatic.CLASS_NAME);
		subStaticMethod();
		System.out.println(CLASS_NAME);
	}
}
```

# 자바의 접근 제어자
- public : 누구나 접근 가능
- protected : 같은 패키지 내에 잇거나 상속받은 경우에만 접근 가능
- package-private : 아무런 접근 제어자를 적어주지 않을때 이며, 같은 패키지 내에 있을 때만 접근 가능
- private : 해당 클래스 내에서만 접근 가능

## 접근 제어자를 만든 이유
- 중요한 계산을 하는 로직이 어떤 메소드에 만들었다고 가정하자. 그 메소드는 클래스내에서만 호출할 수 있고, 다른 클래스에서
절대 가져다 쓰면 안 될 때 유용하다.
- 어떤 메소드를 구현했는데, 다른 개발자가 그 메소드를 마음대로 호출하면 안 될 경우 통제하면 된다.
- 변수의 경우 직접 접근해서 변수를 변경 못하게 하고, 꼭 메소드를 통해서 변경이나 조회만 할 수 있도록 할 떄 많이 사용

## 클래스 접근 제어자 선언할 때 유의점
```java
package c.javapackage;

class A {
    public static void main(String[] args) {
		
    }
}

class B {
	
}
```
- A.java 라는 파일이 있을 때, 다음과 같을 때에는 정상적으로 컴파일 된다. 단, package private 이기 떄문에 같은
패키지 내에 있는 클래스들만 이 클래스의 객체 생성 / 사용 가능
```java
package c.javapackage;

public class A {
    public static void main(String[] args) {
    
    }
}

class B {

}
```
- 클래스 파일 이름 A.java 에서 A 클래스만이 public 이기 때문에 문제없다. 하지만 아래와 같이 선언하면 안 된다.
```java
package c.javapackage;

public class A {
    public static void main(String[] args) {
    
    }
}

public class B {

}
```
- public 으로 선언된 클래스가 소스 내에 있다면, 그 소스 파일의 이름은 public인 클래스 이름과 동일해야만 한다.

