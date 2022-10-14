# java.lang 패키지는 특별하죠
- 자바의 패키지 중 유일하게 java.lang 패키지에 있는 클래스들은 import 안해도 사용 가능
- java.lang, java.util 패키지 안에는 아주 다양한 일을 하는 클래스와 인터페이스들이 혼재되어 있다.
- java.lang 패키지에서 제공하는 인터페이스, 클래스, 예외 클래스 등은 다음과 같이 분류 가능
  - 언어 관련 기본
  - 문자열 관련
  - 기본 자료형 및 숫자 관련
  - 쓰레드 관련
  - 예외 관련
  - 런타임 관련
- 추가로 java.lang 패키지에는 에러도 포함되는데, OutOfMemoryError(OOME), StackOverflowError는 알아두는 것이 좋다.
- OOME의 경우 매모리가 부족해서 발생하는 에러다. 자바는 가상 머신에서 메모리를 관리하지만 프로그램을 잘못 작성하거나 설정이 제대로 되어 있지 않을 경우 발생
- StackOverflowError는 호출된 메소드의 깊이가 너무 깊을 때 발생. 자바에서는 스택 (Stack)이라는 영역에 어떤 메소드가 어떤 메소드를 호출했는지에 대한 정보를 관리.
예를들어 재귀 메소드를 잘못 작성했다면 스택에 쌓을 수 있는 메소드 호출 정보의 한계를 넘어설 수 있다. 이럴 때 발생.
- 추가로, java.lang 패키지에는 Deprecated, Override, SuppressWarnings 기본 어노테이션이 선언되어 있다.

# 숫자를 처리하는 클래스들
- 대부분 계산시 기본 자료형 (Primitive Type)을 사용. 기본 자료형은 자바의 힙 영역이 아닌 스택 영역에 저장되어 관리된다. 따라서 보다 빠른 계산이 가능.
- 그런데 기본 자료형의 숫자를 객체로 처리해야 할 필요가 있을 수도 있다. 따라서 다음과 같은 기본 자료형으로 선언되어 있는 타입의 클래스들이 선언되어 있다.
  - Byte, Short, Integer, Long, Float, Double, Character, Boolean
- Character와 Boolean을 제외한 숫자를 처리하는 클래스들은 감싼(Wrapper) 클래스라고 불리며, 모두 Number 라는 abstract 클래스를 확장 (extends)한다.
그리고, 겉보기에는 참조 자료형이지만, 기본 자료형처럼 사용 가능. 자바가 자동으로 형 변환을 해준다.
- Character 클래스를 제외하고는 공통적인 메소드인 parse타입이름(), valueOf()를 제공한다.

```java
package vol2._20.lang;

public class JavaLangNumber {
	public static void main(String[] args) {
		JavaLangNumber sample = new JavaLangNumber();
		sample.numberTypeCheck();
	}

	public void numberTypeCheck() {
		String value1 = "3";
		String value2 = "5";
		byte byte1 = Byte.parseByte(value1);
		byte byte2 = Byte.parseByte(value2);
		System.out.println(byte1 + byte2);

		Integer refInt1 = Integer.valueOf(value1);
		Integer refInt2 = Integer.valueOf(value2);
		System.out.println(refInt1 + refInt2 + "7");
	}
}
```
- parse타입이름() 메소드는 기본 자료형을 리턴하고, valueOf() 메소드는 참조 자료형을 리턴한다.
```text
8
87
```
- 먼저 String 값을 parseByte() 메소드로 byte로 변환하고 두 값을 더한 결과는 8이 출력되었다.
- valueOf() 메소드를 사용하여 Integer 타입으로 변홚나 후 두 값을 더한 후 "7"이라는 String을 더한 값은 87이 출력되었다.
- 참조 자료형 중에서 Byte, Short, Integer, Long, Float, Double 타입들은 필요시 기본 자료형처럼 사용할 수 있다. 즉, 다음과 같이 new로 객체를
만들지 않아도 값을 할당할 수 있다.
```java
public void numberTypeCheck2() {
    Integer refInt1;
    refInt1 = 100;
    System.out.println(refInt1.doubleValue());
}
```
- 왜 이런 참조 자료형을 만들었을까 그 이유는 다음과 같다.
1. 매개 변수를 참조 자료형으로만 받는 메소드를 처리하기 위해서
2. 제네릭과 같이 기본 자료형을 사용하지 않는 기능을 사용하기 위해서
3. MIN_VALUE, MAX_VALUE와 같이 클래스에 선언된 상수 값을 사용하기 위해서
4. 문자열을 숫자로, 숫자열을 문자열로 쉽게 변환하고, 2, 8, 10, 16진수 변환을 쉽게 처리하기 위해서 이다.
- 각각의 숫자 참조 자료형에서는 매우 많은 메소드를 제공한다.
- 기본 자료형을 참조 자료형으로 만든 클래스들은 Boolean 제외 모두 MIN_VALUE, MAX_VALUE 상수를 가지고 있다.
- 해당 타입이 나타낼 수 있는 값의 범위를 확인하려면 static으로 선언된 상수들을 다음과 같이 사용하면 된다.
```java
public void numberMinMaxCheck() {
    System.out.println("Byte min=" + Byte.MIN_VALUE + " max=" + Byte.MAX_VALUE);
    System.out.println("Short min=" + Short.MIN_VALUE + " max=" + Short.MAX_VALUE);
    System.out.println("Integer min=" + Integer.MIN_VALUE + " max=" + Integer.MAX_VALUE);
    System.out.println("Long min=" + Long.MIN_VALUE + " max=" + Long.MAX_VALUE);
    System.out.println("Float min=" + Float.MIN_VALUE + " max=" + Float.MAX_VALUE);
    System.out.println("Double min=" + Double.MIN_VALUE + " max=" + Double.MAX_VALUE);
    System.out.println("Character min=" + (int)Character.MIN_VALUE + " max=" + (int)Character.MAX_VALUE);
}
```
```text
Byte min=-128 max=127
Short min=-32768 max=32767
Integer min=-2147483648 max=2147483647
Long min=-9223372036854775808 max=9223372036854775807
Float min=1.4E-45 max=3.4028235E38
Double min=4.9E-324 max=1.7976931348623157E308
Character min=0 max=65535
```
```java
public void integerLongMinMaxCheckBinary() {
    System.out.println("Integer BINARY min=" + Integer.toBinaryString(Integer.MIN_VALUE));
    System.out.println("Integer BINARY max=" + Integer.toBinaryString(Integer.MAX_VALUE));

    System.out.println("Integer HEX min=" + Integer.toHexString(Integer.MIN_VALUE));
    System.out.println("Integer HEX max=" + Integer.toHexString(Integer.MAX_VALUE));


    System.out.println("Long BINARY min=" + Long.toBinaryString(Long.MIN_VALUE));
    System.out.println("Long BINARY max=" + Long.toBinaryString(Long.MAX_VALUE));

    System.out.println("Long HEX min=" + Long.toHexString(Long.MIN_VALUE));
    System.out.println("Long BINARY max=" + Long.toHexString(Long.MAX_VALUE));

}
```
```text
Integer BINARY min=10000000000000000000000000000000
Integer BINARY max=1111111111111111111111111111111
Integer HEX min=80000000
Integer HEX max=7fffffff
Long BINARY min=1000000000000000000000000000000000000000000000000000000000000000
Long BINARY max=111111111111111111111111111111111111111111111111111111111111111
Long HEX min=8000000000000000
Long BINARY max=7fffffffffffffff
```
- 돈 계산과 같이 중요한 연산을 수행할 때, 정수형은 BigInteger, 소수형은 BigDecimal을 사용하여야 정확한 계산이 가능하다.
두 클래스 모두 java.lang.Number 클래스의 상속을 받고, java.math 패키지에 선언되어 있다.

# 각종 정보를 확인하기 위한 System 클래스
- System 클래스의 가장 큰 특징은 생성자가 없다. System 클래스의 3개의 static 변수가 선언되어 있고 다음과 같다.

| 선언 및 리턴 타입         | 변수명 | 설명                   |
|--------------------|-----|----------------------|
| static PrintStream | err | 에러 및 오류를 출력할 때 사용한다. |
| static InputStream | in  | 입력값을 처리할 때 사용한다.     |
| static PrintStream | out | 출력값을 처리할 떄 사용한다.     |
- System.out.println() 을 다시 살펴보자. System은 클래스 이름. out은 System 클래스에 static으로 선언된 변수 이름이다. out은 PrintStream
타입이다. 그러므로, println() 이라는 메소드는 PrintStream 클래스에 선언되어 있으며, static 메소드이다.
- out이라는 클래스 변수와 println() 이라는 메소드가 모두 static으로 선언되어 있기 때문에, 별도의 클래스 객체를 생성할 필요가 없었던 것이다. 그러므로,
관련된 메소드는 System 클래스가 아닌 PrintStream 클래스에서 찾아야만 한다. PrintStream, InputStream은 모두 java.io 패키지에 선언되어 있다.
- 처음에는 System 클래스는 출력만을 위한 클래스라고 생각할 수도 있다. 출력을 위한 부분들은 out과 err로 선언된 PrintStream과 연관되어 있다. 즉,
실제 System 클래스에 선언되어 있는 메소드들을 살펴보면 출력과 관련된 메소드들은 없다. System 클래스는 이름 그대로 시스템에 대한 정보를 확인하는 클래스이며,
이 클래스에서 제공되는 메소드를 분류해보면 다음과 같이 다양한 역할을 한다는 것을 알 수 있다.
  - 시스템 속성 (Property)값 관리
  - 시스템 환경 (Environment) 값 조회
  - GC 수행
  - JVM 종료
  - 현재 시간 조회
  - 기타 관리용 메소드들
- GC 수행과 JVM 종료 관련 메소드는 절대 수행해서는 안된다. 그리고 기타 관리용 메소드들은 거의 사용되지 않는다.

## 시스템 속성 (Property) 값 관리
| 리턴 타입             | 메소드 이름 및 매개 변수                        | 설명                                                        |
|-------------------|---------------------------------------|-----------------------------------------------------------|
| static String     | clearProperty(String key)             | key에 지정된 시스템 속성을 제거                                       |
| static Properties | getProperties()                       | 현재 시스템 속성을 Properties 클래스 형태로 제공                          |
| static String     | getProperty(String key)               | key에 지정된 문자열로 된 시스템 속성값(value)을 얻는다.                      |
| static String     | getProperty(String key, String def)   | key에 지정된 문자열로 된 시스템 속성값(value)을 얻고, 만약 없으면 def에 지정된 값을 리턴 |
| static void       | setProperties(Properties props)       | Properties 타입으로 넘겨주는 매개 변수에 있는 값들을 시스템 속성에 넣는다.           |
| static String     | setProperty(String key, String value) | key에 지정된 시스템 속성의 값을 value로 대체한다.                          |

- Properties 클래스는 java.util 패키지에 속하며, Hashtable의 상속을 받은 클래스다. 
- Hashtable은 key와 value의 쌍으로 이루어진 여러 개의 값을 갖는 Map 형태의 자료 구조. 순서는 거의 없고, key-value 쌍으로 되어 잇다.
- 우리의 필요 여부와 상관 없이 자바 프로그램을 실행하면 지금 이야기한 Properties 객체가 생성되며, 그 값은 언제, 어디서든지 같은 JVM 내에서는 꺼내서 사용할 수 있다.

```java
package vol2._20.lang;

public class JavaLangSystem {
	public static void main(String[] args) {
		JavaLangSystem sample = new JavaLangSystem();
		sample.systemPropertiesCheck();
	}

	public void systemPropertiesCheck() {
		System.out.println("java.version=" + System.getProperty("java.version"));
	}
}
```
```text
java.version=11.0.11
```
- System.getProperty() 메소드로 "java.version"이라는 키에 있는 값을 출력하도록 했다.

## 시스템 환경 (Environment)값 조회
| 리턴 타입                      | 메소드 이름 및 매개 변수 | 설명                              |
|----------------------------|----------------|---------------------------------|
| static Map<String, String> | getenv()       | 현재 시스템 환경에 대한 Map 형태의 리턴값을 받는다. |
| static String              | getenv()       | 지정한 name에 해당하는 값을 받는다.          |

- Properties라는 것은 추가할 수도 있고, 변경도 할 수 있다. 하지만 환경값 env 라는 것은 변경하지 못하고 읽기만 할 수 있다. 이 값들은 대부분 OS나 장비와
관련된 것들이다. 앞에서 사용한 systemPropertiesCheck() 메소드의 마지막 줄에 다음의 한 줄을 추가하고 결과를 확인해보자.
```text
System.out.println("JAVA_HOME=" + System.getenv("JAVA_HOME"));
```
- 자바를 사용할 때 JAVA_HOME은 JDK가 설치되어 있는 경로를 말한다. java.exe나 java 명령어가 있는 위치가 아닌 자바의 가장 상위 디렉터리이다.
- 설정되어 있지 않으면 null로 나올 수 있다.

## GC 수행
| 리턴 타입       | 메소드 이름 및 매개 변수    | 설명                                           |
|-------------|-------------------|----------------------------------------------|
| static void | gc()              | 가비지 컬렉터를 실행한다.                               |
| static void | runFinalization() | GC 처리를 기다리는 모든 객체에 대하여 finalize() 메소드를 실행한다. |
- 이 메소드들은 개발자가 직접 호출하면 안 된다.
- 자바는 메모리 처리를 개발자가 별도로 하지 않는다. System.gc() 라는 메소드를 호출하면 가비지 컬렉션을 명시적으로 처리할 수 있다. 그리고 Object 클래스에 선언되어
있는 finalize() 메소드를 명시적으로 수행하도록 하는 runFinalization() 메소드가 있다.
- 이 두개의 메소드들을 우리가 호출하지 않아도 알아서 JVM이 더 이상 필요 없는 객체를 처리하는 GC 작업과 finalization 작업을 실행한다. 만약 우리가 명시적으로 이 메소드를
호출하면 시스템은 하려던 일들을 멈추고 이 작업을 실행한다.
- GC는 JVMd이 알아서 필요로 할 때 한다. 절대 우리가 이 메소드를 호출해서는 안 된다.

## JVM 종료
| 리턴 타입       | 메소드 이름 및 매개 변수   | 설명                |
|-------------|------------------|-------------------|
| static void | exit(int status) | 현재 수행중인 JVM을 멈춘다. |

- 이 메소드 역시 절대 호출하면 안된다. 나중에 안드로이드 앱이나 웹 애플리케이션에서 이 메소드를 사용하면 해당 어플리케이션의 JVM이 죽어버린다. 그러면 바로
장애로 직결될 것이다. 절대 쓰면 안 된다. 매개 변수로 넘어가는 정수는 0일 경우에만 정상적인 종료를 의미하고, 그 외의 숫자는 비정상적인 종료를 의미한다.

## 현재 시간 조회
| 리턴 타입       | 메소드 이름 및 매개 변수      | 설명                |
|-------------|---------------------|-------------------|
| static long | currentTimeMillis() | 현재 시간을 밀리초 단위로 리턴 |
| static long | nanoTime()          | 현재 시간을 나노초 단위로 리턴 |

- currentTimeMillis() 메소드는 현재 시간을 나타낼 때 매우 유용. UTC라는 Universal time 기준으로 1970년 1.1 00:00 부터 지금까지의 밀로초 단위의
차이를 출력한다. 밀리초는 1/1,000초, 1,000 밀리초는 1,000ms 이며 1초이다.
- nanoTime() 메소드는 현재 시간을 알아내기 위해서가 아닌 시간의 차이를 측정하기 위한 용도의 메소드로 나노초를 제공하며, 1/1,000,000,000초를 의미한다.
```java
public void numberMinMaxElapsedCheck() {
    JavaLangNumber numberSample = new JavaLangNumber();
    long startTime = System.currentTimeMillis();
    long startNanoTime = System.nanoTime();
    numberSample.numberMinMaxCheck();
    System.out.println("Milli second=" + (System.currentTimeMillis() - startTime));
    System.out.println("Nano second=" + (System.nanoTime() - startNanoTime));
}
```
```text
Byte min=-128 max=127
Short min=-32768 max=32767
Integer min=-2147483648 max=2147483647
Long min=-9223372036854775808 max=9223372036854775807
Float min=1.4E-45 max=3.4028235E38
Double min=4.9E-324 max=1.7976931348623157E308
Character min=0 max=65535
Milli second=0
Nano second=14993292
```
- 밀리초는 0으로 출력되지만 나노초는 매우 큰 숫자로 출력된다. 따라서 시간을 측정할 필요가 있을 떄, 나노초를 이용하는 것을 권장한다. nanoTime() 메소드는 시간 측정을
위해 만든 것이다.

# System.out을 살펴보자.
- System 클래스의 out(System.out), err(System.err) 변수는 PrintStream 이라는 동일한 클래스의 객체다. 단지, 정상 출력인지 에러 출력인지의 차이만 존재.
- PrintStream 클래스는 static 하게 사용된다. 출력을 위한 주요 메소드의 이름은 다음과 같다.
  - print()
  - println()
  - format()
  - printf()
  - write()
- write()는 System.out 으로 사용할 떄 많이 사용되지는 않는다. 가장 많이 사용되는 메소드는 print(), println()이다. 두 메소드는 줄바꿈처리의 차이다. 그리고
println() 메소드는 매개 변수가 없는 메소드가 존재한다. 두 메소드 모두 기본 / 참조 자료형을 매개 변수로 가질 수 있다.
- 줄바꿈 처리를 위해서는 `println("")` 보다는 `println()`을 사용하는 것이 깔끔하다. "" 이라는 필요 없는 String 객체가 생기기 때문이다.
- print(), println()은 다음과 같은 매개 변수를 받는다.
  - boolean, char, char[], double, float, int, long, Object, String, (+ println()의 경우 매개 변수가 없는 경우)
- 잘 살펴보면 byte, short 타입을 매개 변수로 받는 메소드가 선언되어 있지 않다. 하지만, 두 개 모두 정수형이기 때문에 전혀 문제 없이 출력된다.
int 타입을 매개 변수로 받는 메소드에서 알아서 처리해준다.
```java
package vol2._20.lang;

public class JavaLangSystemPrint {
	public static void main(String[] args) {
		JavaLangSystemPrint sample = new JavaLangSystemPrint();
		sample.printStreamCheck();
	}

	public void printStreamCheck() {
		byte b = 127;
		short s = 32767;
		System.out.println(b);
		System.out.println(s);
		printInt(b);
		printInt(s);
	}

	private void printInt(int value) {
		System.out.println(value);
	}
}
```
- 이와 같이 컴파일도 제대로 되고, 실행도 아무런 이상 없이 된다.
- 다음을 null을 출력하는 메소드에서는 어떻게 될까?
```java
public void printNull() {
    Object obj = null;
    System.out.println(obj);
    System.out.println(obj + " is object's value.");
}
```
```text
null
null is object's value.
```
- null인 obj는 아무런 할당이 되어 있지 않기 때문에 메소드를 호출할 수가 없다. 따라서 toString()메소드를 호출할 수 없는데도 예외가 발생하지 않는다.
- print() println() 메소드에서는 단순히 toString() 메소드 결과가 아닌 String의 valueOf() 라는 static 메소드를 호출하여 결과를 받은 후 출력한다.
즉, String.valueOf(obj)가 호출된 것이다. 그러므로, 아무런 이상 없이 결과가 출력된 것이다. toString()으로 출력해보자.

```java
public void printNullToString() {
    Object obj = null;
    System.out.println(obj.toString());
}
```
```text
Exception in thread "main" java.lang.NullPointerException
	at vol2._20.lang.JavaLangSystemPrint.printNullToString(JavaLangSystemPrint.java:32)
	at vol2._20.lang.JavaLangSystemPrint.main(JavaLangSystemPrint.java:8)
```
- NullPointerException이 발생한다. obj 자체가 null이고, 이 객체의 toString() 이라는 메소드를 불러버렸으니, 예외가 발생할 수 밖에 없다.
- 따라서 객체를 출력할 때에는 toString() 보다 valueOf() 메소드를 사용하는 것이 훨씬 안전하다는 것을 기억해야 한다. 
- 두 번째 출력문인 `obj + " is object's value"` 에서도 예외가 발생하지 않는다. 이 더하기 문장을 컴파일러는 StringBuilder로 변환하기 때문이다.
즉 해당 코드는 `new StringBuilder().append(obj).append(" is object's value")`로 변환되며, append() 메소드에 null을 넣어버린 것과 
동일하므로 문제없이 null을 출력한다.
- 다음으로 format(), printf() 메소드는 자바 5 부터 추가되었다.
