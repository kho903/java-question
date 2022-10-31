# Java 7에서 달라진 것들
## Java 7에서는 ...
- Java 7은 지금까지의 버전에서는 제공하지 않은 많은 변화가 있었다.
- 정리되어 있는 문서는 JSR336. JSR는 Java Specification Requirement로 자바에서 사용할 기술들을 나열한 문서
- 대표적인 변경 사항은 다음과 같다.
  - 숫자 표시 방법 보완
  - switch문에서 String 사용
  - 제네릭을 쉽게 사용할 수 있는 Diamond
  - 예외 처리시 다중 처리 기능

## 달라진 숫자 표현법...
- byte, long, int, long과 같은 정수형은 8, 10, 16진수로 표현할 수 있었다. 아무런 접두사를 숫자 앞에 넣지 않으면 10진수로 인식. 0을 숫자 앞에 넣어주면 8진수, 0x를 넣으면 16진수로 인식

```java
package vol2._30.number;

public class JDK7Numbers {
	public static void main(String[] args) {
		JDK7Numbers numbers = new JDK7Numbers();
		numbers.jdk6();
	}

	private void jdk6() {
		int decVal = 1106;
		int octVal = 02122;
		int hexVal = 0x452;
		System.out.println(decVal);
		System.out.println(octVal);
		System.out.println(hexVal);
	}
}
```
```text
1106
1106
1106
```
- 모두 1106이 나온다.
- 그런데 Java 7 부터는 2진수로 나타내기 위해서는 0b를 앞에 추가해주면 된다. 
```java
private void jdk7() {
    int binaryVal = 0b10001010010;
    System.out.println(binaryVal);
}
```
```text
1106
1106
1106
1106
```
- 2진수의 정수도 표현 가능하다.
- 추가로, 돈 단위나 숫자 단위를 표시할 때 1000 단위에 쉼표를 붙히는 것처럼 `_`를 사용할 수 있다.
```java
public void jdk7UnderScore() {
    int binaryVal = 0b0100_0101_0010;
    int million = 1_000_000;
    System.out.println(binaryVal);
    System.out.println(million);
}
```
```text
1106
1000000
```
- 보통 2진수는 4자리 단위로 잘라서 표시한다. 그리고, 백만을 표시하기 위해 이와 같이 _를 붙혀 훨씬 가독성이 좋아졌다.
- Java 7 이후부터 사용가능하고, 유의점은 이 _를 사용할 때에는 반드시 "숫자 사이에만" 넣어주어야 한다.
- 이진수 0b_101010처럼 b 뒤에 사용하는 것도 안 된다. 즉, 0x 뒤에 _도 와서는 안되고, Long을 나타내는 L과 그 앞의 숫자 사이에 두어도 안 된다.
- 하지만 8진수를 나타내는 0과 그 뒤 숫자 사이에 0_101010처럼 넣는 것은 가능하다. 0도 숫자이기 때문에 아무리 8진수를 나타내는 0이라 할지라도 그 뒤의 숫자와의 사이에 _가 있어도 상관 없다.

## Switch 문장 확장
- Java 6 까지는 switch-case 문장에 정수형만 사용할 수 있었다. 그런데, Java 7부터는 switch 문장에 String 도 사용할 수 있다.
```java
package vol2._30.switchcase;

public class JDK7Switch {
	public static void main(String[] args) {
		JDK7Switch switchSample = new JDK7Switch();
		System.out.println(switchSample.salaryIncreaseAmount(3));
	}

	private double salaryIncreaseAmount(int employeeLevel) {
		switch (employeeLevel) {
			case 1:
				return 10.0;
			case 2:
				return 15.0;
			case 3:
				return 100.0;
			case 4:
				return 20.0;
		}
		return 0.0;
	}
}
```
- 이 예제처럼 JDK 6까지는 숫자로만 switch-case문을 사용할 수 있었다. 보통은 이렇게 숫자를 지정하는 것보다는 상수를 선언하여 사용한다. 예를 들면 다음과 같이 클래스에 선언해 놓는 방식이다.
```text
private final int ENGINEER = 3;
```
- 혹은 enum을 사용해도 된다.
- 만약 이렇게 코드성 데이터를 미리 선언해 놓은 것이 있으면 어렵지 않게 코드를 작성할 수 있을 것이지만 이러한 String을 switch-case 에 사용함으로써 매우 간편해졌다.
- String을 매개 변수로 받는 salaryIncreaseAmount() 메소드를 추가해 보자. 
```text
public double salaryIncreaseAmount(String employeeLevel) {
    switch (employeeLevel) {
        case "CEO":
            return 10.0;
        case "Manager":
            return 15.0;
        case "Engineer":
        case "Developer":
            return 100.0;
        case "Designer":
        case "Planner":
            return 20.0;
    }
    return 0.0;
}
```
```text
100.0
100.0
```
- 한 가지 유의할 점은 만약 switch-case 에 사용하는 String 문자열이 null인 경우에는 NullPointerException이 발생하니 switch-case에서 사용하는 문자열이 null인지를 꼭
확인해야 한다.

## 제네릭은 다이아몬드를 쓰면 쉬워요
- 앞서 배운 제네릭을 사용할 때에는 반드시 다음과 같이 생성자에도 해당 타입들을 상세하게 명시했어야 했다. 예를 들면 다음과 같은 것을 말한다.
```java
HashMap<String, Integer> map = new HashMap<String, Integer>();
Map<String, List<String>> map2 = new HashMap<String, List<String>>();
ArrayList<String> list = new ArrayList<String>();
```
- 하지만 Java 7 부터는 이와 같이 생성자에는 일일이 타입들을 명시해 줄 필요가 없다. 왜냐하면, 이미 변수를 선언할 때 필요한 타입들을 지정해 놓았기 때문이다. 그러므로, 다음과 같이 명시해
주면 된다.
```text
HashMap<String, Integer> map = new HashMap<>();
Map<String, List<String>> map2 = new HashMap<>();
ArrayList<String> list = new ArrayList<>();
```
- 이름 그대로 <> 로 표시한 것을 다이아몬드라고 명명했으며, 이렇게 다이아몬드를 사용하면 컴파일러에서 알아서 해당 생성자의 타입을 지정해버린다.
- 이처럼 편리한 다이아몬드는 편한 만큼 제약이 따르며, 다은과 같은 제약들이 있다.
  - 다이아몬드 미 지정시 컴파일 경고 발생
  - 다이아몬드 생성시 유의 사항 1 - 메소드 내에서 객체 생성시
  - 다이아몬드 생성시 유의 사항 2 - 제네릭하면서도 제네릭하지 않은 객체 생성시

### 다이아몬드 생성시 유의사항 - 제네릭하면서도 제네릭하지 않은 객체 생성시
- 다음과 같은 제네릭 클래스와 실행 코드가 있을때,
```java
package vol2._30.generic;

public class GenericClass<X> {
	private X x;
	private Object o;

	public <T> GenericClass(T t) {
		this.o = t;
		System.out.println("T type=" + t.getClass().getName());
	}

	public void setValue(X x) {
		this.x = x;
		System.out.println("X type=" + x.getClass().getName());
	}
}
```
```java
package vol2._30.generic;

public class TypeInference {
	public static void main(String[] args) {
		TypeInference type = new TypeInference();
		type.makeObject();
	}

	private void makeObject() {
		GenericClass<Integer> generic1 = new GenericClass<>("String");
		generic1.setValue(999);
	}
}
```
```text
T type=java.lang.String
X type=java.lang.Integer
```
- T의 타입은 String 이고, X의 타입은 Integer이다. 선언한 대로 타입들이 지정된 것을 볼 수 있다. 
- 다음으로 makeObject2() 메소드를 다음과 같이 만들어 보자.
```java
private void makeObject2() {
    GenericClass<Integer> generic1 = new GenericClass("String");
    generic1.setValue(999);
}
```
```text
Note: vol2/_30/generic/TypeInference.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
```
```text
T type=java.lang.String
X type=java.lang.Integer
```
- 생성자에서 아무런 타입에 대한 명시를 안했기 때문에 다음과 같은 경고가 발생하기는 하지만, 실행하는 데에 큰 문제가 없다.
- 그렇다면 이번에는 다이아몬드를 안쓰고 명시적으로 지정할 때를 살펴보자. 
```java
public void makeObject3() {
    GenericClass<Integer> generic1 = new <String> GenericClass<Integer>("String");
    generic1.setValue(999);
}
```
```text
T type=java.lang.String
X type=java.lang.Integer
```
- 명시적으로 이렇게 지정하면 큰 문제가 없을 것이다. 당연히 컴파일 시에는 오류도 없고, 실행도 정상적으로 된다.
- 그런데 마지막으로 다음과 같이 X 타입을 다이아몬드로 선언해보자.
```java
public void makeObject4() {
    GenericClass<Integer> generic1 = new <String> GenericClass<>("String");
    generic1.setValue(999);
}
```
```text
vol2/_30/generic/TypeInference.java:28: error: cannot infer type arguments for GenericClass<X>
                GenericClass<Integer> generic1 = new <String> GenericClass<>("String");
                                                                          ^
  reason: cannot use '<>' with explicit type parameters for constructor
  where X is a type-variable:
    X extends Object declared in class GenericClass
Note: vol2/_30/generic/TypeInference.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
```
- 이렇게 명시적으로 타입 T에 대해 선언한 상태에서, 타입 X에 대해서는 다이아몬드로 선언하여 컴파일러에게 맡겨 버리면 컴파일이 정상적으로 되지 않으니 유의해야 한다.
- 따라서, 생성자에 있는 new와 클래스 이름 사이에 타입 이름을 명시적으로 두려면, 다이아몬드를 사용하면 안된다.

## Non reifiable varargs 타입
- 자바의 제네릭을 사용하면서, 발생 가능한 문제 중 하나가 "reifiable" 하지 않은 varargs 타입 (non reifiable varargs type)이다. 이 문제는 자바에서 제네릭을 사용하지 않는
버전과의 호환성을 위해서 제공했던 기능 때문에 발생한다.
- reifiable은 실행시에도 타입 정보가 남아 있는 타입을 의미하고, non reifiable은 컴파일시 타입 정보가 손실되는 타입을 말한다. 
- Collections 클래스에는 addAll() 이라는 메소드가 있으며 다음과 같이 선언되어 있다.
```java
public static <T> boolean addAll(Collection<? super T> c, T ... elements)
```
- 첫 번째 매개변수에는 추가되는 데이터가 저장되는 Collection 을 구현한 클래스의 객체가, 두 번째 매개 변수에는 추가되는 객체들이 나열되도록 되어 있다. 이 메소드를 활용하는 예제는 다음과 같다.
```java
package vol2._30.reifiable;

import java.util.ArrayList;
import java.util.Collections;

public class ReifiableSample {
	public static void main(String[] args) {
		ReifiableSample sample = new ReifiableSample();
		sample.addData();
	}

	private void addData() {
		ArrayList<ArrayList<String>> list = new ArrayList<>();
		ArrayList<String> newDataList = new ArrayList<>();
		newDataList.add("a");
		Collections.addAll(list, newDataList);
		System.out.println(list);
	}
}
```
- addData() 메소드를 살펴보자.
1. ArrayList 내에 ArrayList<String>이 저장되는 list라는 객체를 만들었다.
2. list에 저장할 newDataList라는 객체를 생성하고, 값을 하나 추가하였다.
3. Collection의 addAll() 메소드를 사용하여 list에 newDataList 라는 객체를 저장하도록 했다.
- 그냥 보기에 큰 문제는 없어보인다. 하지만 Java 6 에서 컴파일하면 경고와 함께 -Xlint:unchecked 옵션을 추가해서 다시 컴파일해 보라는 메시지가 나타난다.
- Collections의 addAll() 메소드에서 위험하다는 메시지가 나타나지만 실행해봐도 큰 문제는 없다.
- 실제 API에 있는 addAll() 메소드 선언을 보자.
```java
public static <T> boolean addAll(Collection<? super T> c, T... elements)
```
- 가변 매개 변수를 사용할 경우에 실제로 내부적으로는 다음과 같이 Object의 배열로 처리된다.
```text
public static boolean addAll(java.util.Collection c, java.lang.Object[] elements)
```
- 이렇게 Object의 배열로 처리되면, 큰 문제는 발생하지는 않지만 잠재적으로 문제가 발생할 수도 있다. 따라서 이러한 경고를 없애기 위해서 @SafeVarargs 라는 어노테이션을 메소드 선언부에
추가하면 된다. 이 Collections 클래스의 addAll() 메소드에도 Java 7부터는 이 어노테이션이 추가되어 있다. 해당 어노테이션은 다음의 경우에만 사용할 수 있다.
  - 가변 매개 변수를 사용하고,
  - final이나 static 으로 선언되어 있어야 한다.
- 그리고 해당 어노테이션은
  - 가변 매개 변수가 reifiable 타입이고,
  - 메소드 내에서 매개 변수를 다른 변수에 대입하는 작업을 수행하는 경우
- 에는 컴파일러에서 경고가 발생하게 된다.

## 예외도 이렇게 보완되었다
- Java 7 변경사항에 대한 마지막으로는 예외이다. 지금까지는 예외를 처리하기 위해 여러 개의 catch로 잡아주는 코드의 가독성이 떨어졌다. 예를들어 아래 코드와 같다.
```java
package vol2._30.trycatch;

import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class TryWithResource {
	public void scanFile(String fileName, String encoding) {
		Scanner scanner = null;
		try {
			scanner = new Scanner(new File(fileName), encoding);
			System.out.println(scanner.nextLine());
		} catch (IllegalArgumentException iae) {
			iae.printStackTrace();
		} catch (FileNotFoundException ffe) {
			ffe.printStackTrace();
		} catch (NullPointerException npe) {
			npe.printStackTrace();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if (scanner != null) {
				scanner.close();
			}
		}
	}
}
```
- 여기서 Scanner 클래스의 생성자에 대해 잠깐 살펴보면, 첫 번째 매개 변수는 읽을 파일 이름이고 두 번째 매개변수는 파일의 인코딩 타입이다. 특히, 한글일 경우 이 타입을 잘 명시해 줘야만 한다.
- 이 경우에 인코딩 타입이 존재하지 않을 경우 IllegalArgumentException이 발생하고, 파일이 존재하지 않을 경우 FileNotFoundException이 발생한다. 
- 파일명이나 인코딩 타입이 null일경우 NullPointerException이 발생할 수 있다. 
- 그래서, 지금까지는 이 부분을 처리할 때 예제처럼 일일이 각각의 catch 블록을 만들어서 처리하거나, 가장 마지막에 있는 Exception 클래스처럼 Exception 클래스 하나로 catch를 해줬다.
- 그런데, Java 7부터는 만약 예제처럼 catch 블록에서 처리하는 방식이 동일하다면 다음과 같이 간단하게 처리 가능.
```java
public void newScanFile(String fileName, String encoding) {
    Scanner scanner = null;
    try {
        scanner = new Scanner(new File(fileName), encoding);
        System.out.println(scanner.nextLine());
    } catch (IllegalArgumentException | FileNotFoundException | NullPointerException exception) {
        exception.printStackTrace();
    } finally {
        if (scanner != null) {
            scanner.close();
        }
    }
}
```
- 각 Exception을 조건식에서 or를 나타내는 파이프(|)로 연결하여 이처럼 간단하게 처리 가능.
- 그리고 자바의 예외와 관련하여 추가된 것이 하나 더 있따. 바로 "리소스와 함께 처리하는 try 문장" 인데, 영어로 "try-with-resource"라고 이야기한다.
- Java 7 에는 AutoCloseable이라는 인터페이스가 추가되었다. try-with-resource를 사용할 때에는 이 인터페이스를 구현한 클래스는 별도로 close()를 호출해 줄 필요가 없다.
- 즉, finally 문장에서 close()를 처리하기 위해 코드의 가독성을 떨어지게 할 필요가 없다. 앞서 사용한 예제는 다음과 같이 보다 더 간단히 처리할 수 있다.
```java
public void newScanFileTryWithResource(String fileName, String encoding) {
    try (Scanner scanner = new Scanner(new File(fileName), encoding)) {
        System.out.println(scanner.nextLine());
    } catch (IllegalArgumentException | FileNotFoundException | NullPointerException exception) {
        exception.printStackTrace();
    }
}
```
- 이와 같이 try 블록의 시작 부분에 catch 처럼 소괄호 안에 필요한 문장을 포함할 수 있다. 이렇게 try의 소괄호 내에 예외가 발생할 수 있는 객체에서 close()를 이용해 닫아야 할 필요가
있을 때 AutoCloseable을 구현한 객체는 finally 문장에서 별도로 처리할 필요가 없는 것을 볼 수 있다.
- 추가로 만약 try의 소괄호 내에서 두 개의 객체를 생성할 필요가 있을 때에는 당황할 필요 없이 "세미콜론"으로 구분하여 두 객체를 생성해주면 된다.

## Java 7 부터는 꼭 안닫아도 되는 애들이 있어요
- Java 5부터 추가된 java.io.Closeable이라는 인터페이스가 있다. 이 인터페이스를 구현한 클래스들은 명시적으로 close() 메소드를 사용하여 닫아주어야만 했었다.
- 하지만 Java 7 부터는 해당 인터페이스는 다음과 같이 선언되어 있다.
```java
public interface Closeable extends AutoCloseable
```
- AutoCloseable이라는 인터페이스를 상속받은 것을 볼 수 있으며, 이 인터페이스를 구현한 클래스는 앞 절에서 살펴본 try-with-resource 문장에서 사용할 수 있다. 많이 사용하는 클래스 중
이 AutoCloseable과 관련있는 클래스들은 다음과 같다.
  - java.nio.channels.FileLock
  - java.beans.XMLEncoder
  - java.beans.XMLDecoer
  - java.io.ObjectInput
  - java.io.ObjectOutput
  - java.util.Scanner
  - java.sql.Connection
  - java.sql.ResultSet
  - java.sql.Statement
- 이 클래스들의 목록은 극히 일부에 속하며, Java 7 API에 있는 클래스 중 매우 많은 클래스가 이 인터페이스를 구현해 놓았기 때문에, 관련된 클래스를 살펴보려면 java.lang 패키지에 선언되어
있는 AutoCloseable 인터페이스의 API를 확인해 보면 된다.

## 정리
- Java 7에서는 숫자 표시 방식이 조금 변경되었고, switch에서 String을 사용할 수 있게 기능이 추가되었다. 
- 그리고, 제네릭을 사용할 때, 다이아몬드를 사용하여 코딩할 때의 불편함도 해소되었고, Java 5, 6에서 발생 가능헀던 non-reifiable 한 타입에 대해서도 조금 더 상세하게 정리되었다.
- 마지막으로 try-catch 문장도 보다 코딩을 간단하게 할 수 있도록 변경된 것을 알 수 있다.
- 이처럼 Java 7은 개발의 편의를 위해 많은 기능들이 추가되었는데, 그 외에 추가된 클래스들도 다수 존재한다.


