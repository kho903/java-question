Java7까지 많은 변화가 있었고, 

이것을 문서화한 것이 JSR 336 이다.  (JSR : Java Specification Requirement의 약자) 

### switch- case 변화 .

- java 6 이하에서는 String을 매개 변수로 받는 메소드는 컴파일 및 실행이 안되었다.
- java 7부터 switch-case 에 String 문자열 바로 사용 가능.
- switch-case 에 사용하는 String 문자열이 NULL인 경우에는 NullPointerException이 발생한다.

### Non reifiable varargs 타입

- 자바의 제네릭을 사용하면, 발생 가능한 문제 중 하나가 ‘reifiable 하지 않은 varargs 타입’이다.

이 문제는 자바에서 제네릭을 사용하지 않는 버전과의 호환성을 위해서 제공했던 기능 때문에 발생한다. 

> reifiable은 실행시에도 타입 정보가 남아있는 타입을 의미하고, non reifiable은 컴파일시 타입 정보가 손실되는 타입을 말한다.
> 

addAll() 메소드 선언.

```kotlin
public static <T> boolean addAll(Collection<? super T> c, T... elements)
```

가변 매개 변수를 사용할 경우에 실제로 내부적으로는 다음과 같이 Object 의 배열로 처리된다. 

```kotlin
public static boolean addAll(java.util.Collection c, java.lang.Object[] elements)
```

이렇게 Object 의 배열로 처리된다면, 더 큰 문제는 발생하지는 않지만 잠재적으로 문제가 발생할 수 있다. 

따라서, 이러한 경고를 없애기 위해서는 @SafeVarargs 라는 어노테이션을 메소드 선언부에 추가하면 된다. 

이 Collections 클래스의 allAll() 메소드에는 Java7 부터는 이 어노테이션이 추가되어 있다. 해당 어노테이션은 다음의 경우에만 사용할 수 있다. 

- 가변 매개 변수를 사용하고
- final이나 static으로 선언되어 있어야 한다.

그리고, 해당 어노테이션은

- 가변 매개 변수가 reifiable 타입이고,
- 메소드 내에서 매개 변수를 다른 변수에 대입하는 작업을 수행하는 경우

에는 컴파일에서 경고가 발생한다. 

---

### 리소스와 함께 처리하는 try 문장.

Java7 에는 AutoCloseable이라는 인터페이스가 추가되었다. 

try-with-resource를 사용할 때에는 이 인터페이스를 구현한 클래스는 별도로 close()를 호출하지 않아도 된다. 

Java5부터 추가된 java.io.Closeable이라는 인터페이스가 있다. 이 인터페이스를 구현할 클래스들은 명시적으로 close() 메소드를 사용하여 닫아주어야 했다. 

하지만, Java7 부터는 해당 인터페이스는 다음과 같이 선언되어 있다. 

```java
public interface Closeable extends AutoCloseable 
```

AutoCloseable과 관련 있는 클래스

- java.nio.channels.FileLock
- java.beans.XMLEncoder
- java.beans.XMLDecoder
- java.io.ObjectInput
- java.io.ObjectOuput
- java.util.Scanner
- java.sql.Connection
- java.sql.ResultSet
- java.sql.Statement
