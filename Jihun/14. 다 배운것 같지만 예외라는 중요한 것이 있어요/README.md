# 예외
- 자바에는 예상을 했든, 하지 않았건, 예외적인 일이 발생하게 되면 "예외"라는 것을 던져버린다. 가장 일반적인 예가 null 객체에 메소드 호출,
5개 배열에서 6번째 값을 읽으라고 하든지 등의 경우가 있따.

## try-catch
```java
package vol1._14.exception;

public class ExceptionSample {
	public static void main(String[] args) {
		ExceptionSample sample = new ExceptionSample();
		sample.arrayOutOfBounds();
	}

	private void arrayOutOfBounds() {
		int[] intArray = new int[5];
		System.out.println(intArray[5]);
	}
}
```
- 5개의 공간을 가지는 intArray에서 6번째 값을 호출하면 컴파일은 정상적으로 되지만 아래와 같은 예외가 발생한다.
```text
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 5
	at vol1._14.exception.ExceptionSample.arrayOutOfBounds(ExceptionSample.java:11)
	at vol1._14.exception.ExceptionSample.main(ExceptionSample.java:6)
```
- ArrayIndexOutOfBoundsException는 배열의 범위 밖에 있는 위치를 요청한 예외 라는 의미이다.
- 예외의 첫 줄에는 어떤 예외가 발생했다고 출력됨. 그 다음 줄부터는 앞에 탭을 주고 at으로 시작하는 스택 호출 추적 (call stack trace)라고 하고,
보통 스택 트레이스라고 부르는 문장들이 출력된다.
- 이와 같은 예외 메시지가 발생하지 않도록 try-catch로 묶어줄 수 있다.
```java
private void arrayOutOfBoundsTryCatch() {
    try {
        int[] intArray = new int[5];
        System.out.println(intArray[5]);
    } catch (Exception e) {

    }
}
```
- 이것이 try-catch 블록으로 try 뒤에 중괄호로 예외가 발생하는 문장들을 묶어주고, catch 괄호 안에 예외가 발생했을 때 처리를 해준다.
- 모든 문장을 try로 묶어줄 필요는 없고, 예외가 발생하는 부분만 묶어주면 된다.
```java
private void arrayOutOfBoundsTryCatch() {
    int[] intArray = new int[5];
    try {
        System.out.println(intArray[5]);
        System.out.println("This code should run.");
    } catch (Exception e) {

    }
}
```
- 이렇게 메소드를 만들면 아무런 메시지를 출력하지 않는다.
- 자바에서는 try 블록 안에서 예외가 발생되면 그 이하의 문장은 실행되지 않고, 바로 catch 블록으로 넘어간다.
```java
private void arrayOutOfBoundsTryCatch() {
    int[] intArray = new int[5];
    try {
        System.out.println(intArray[5]);
        System.out.println("This code should run.");
    } catch (Exception e) {
        System.err.println("Exception occured");
    }
    System.out.println("This code must run.");
}
```
```text
This code must run.
Exception occurred
```
- try-catch 사용시, try 블록 내에서 예외가 발생하면, 예외가 발생한 줄 이후에 있는 try 블록 내의 코드들은 수행되지 않고,
해당하는 예외의 catch 블록 내의 문장이 실행된다. 그 다음에, try-catch 구문 밖에 있는 문장이 실행된다.

### try-catch문 실행 정리
- try-catch에서 예외가 발생하지 않을 경우 <br>
try 내에 있는 모든 문장이 실행되고 try-catch 문장 이후의 내용이 실행된다.
- try-catch에서 예외가 발생하는 경우 <br>
try 내에서 예외가 발생한 이후의 문장들은 실행되지 않는다.<br>
catch 내에 있는 문장은 반드시 실행되고, try-catch 문장 이후의 내용이 실행된다.

## try-catch 변수 선언
- try-catch를 사용할 때 가장 하기 쉬운 변수 지정 실수가 있다. try 블록은 말 그대로 중괄호로 쌓여 있는 블로이다. 그래서, try 내에서 선언한 변수를
catch에서 사용할 수 없다.

```java
private void checkVariable() {
    int[] intArray = new int[5];
    try {
        System.out.println(intArray[5]);
    } catch (Exception e) {
        System.out.println(intArray.length);
    }
    System.out.println("This code must run.");
}
```
```text
5
This code must run.
```
- 위 메소드는 아무런 이상이 없다. 아래와 같이 바꿔보자.
```java
private void checkVariable() {
    try {
        int[] intArray = new int[5];
        System.out.println(intArray[5]);
    } catch (Exception e) {
        System.out.println(intArray.length);
    }
    System.out.println("This code must run.");
}
```
```text
vol1/_14/exception/ExceptionVariable.java:14: error: cannot find symbol
                        System.out.println(intArray.length);
                                           ^
  symbol:   variable intArray
  location: class ExceptionVariable
1 error
```
- intArray가 try 블록 내에 선언되어 catch에서는 intArray가 누군지 모른다.
- 이러한 문제를 해결하기 위해서 일반적으로 catch 문장에서 사용할 변수에 대해서는 다음과 같이 try 앞에 미리 선언해 놓는다.
```java
private void checkVariable() {
    int[] intArray = null;
    try {
        intArray = new int[5];
        System.out.println(intArray[5]);
    } catch (Exception e) {
        System.out.println(intArray.length);
    }
    System.out.println("This code must run.");
}
```
- 이렇게 변수만 미리 선언해 놓으면 정상적으로 컴파일도 되고 실행도 된다.
- 예외가 발생해서 catch 블록이 실행된다고 해서 try 블록 내에서 실행된 모든 문장이 무시되는 것은 전혀 아니고, 위와 같은 경우는
intArray[5]를 호출하는 순간 발생한다. 앞에서 실행된 크기를 지정하는 문장은 전혀 문제 업시 실행된다.

## finally
- try-catch 구문에 추가로 finally는 예외를 처리할 때 "어떠한 경우에도 넌 반드시 실행해~~"라는 의미다.
```java
private void finallySample() {
    int[] intArray = new int[5];
    try {
        // System.out.println(intArray[4]);
        System.out.println(intArray[5]);
    } catch (Exception e) {
        System.out.println(intArray.length);
    } finally {
        System.out.println("Here is finally");
    }
    System.out.println("This code must run");
}
```
```text
// 0 (intArray[4])의 경우
5
Here is finally
This code must run
```
- 예외 발생 여부와 관계없이 finally는 항상 실행된다. 
- finally 블록은 코드의 중복을 피하기 위해서 반드시 필요하다.

## 두 개 이상의 catch
- catch 블록이 시작되기 전에 있는 소괄호에는 예외의 종류를 명시한다. 즉, 항상 Exception e만 쓰지 않는다.
```java
private void multiCatch() {
    int[] intArray = new int[5];
    try {
        System.out.println(intArray[5]);
    } catch (ArrayIndexOutOfBoundsException e) {
        System.err.println("ArrayIndexOutOfBoundsException occurred");
    } catch (Exception e) {
        System.err.println(intArray.length);
    }
}
```
```text
ArrayIndexOutOfBoundsException occurred
```
- catch 블록의 순서를 바꾸어 보자.
```java
private void multiCatch() {
    int[] intArray = new int[5];
    try {
        System.out.println(intArray[5]);
    } catch (Exception e) {
        System.err.println(intArray.length);
    }catch (ArrayIndexOutOfBoundsException e) {
        System.err.println("ArrayIndexOutOfBoundsException occurred");
    }
}
```
```text
vol1/_14/exception/MultiCatchSample.java:15: error: exception ArrayIndexOutOfBoundsException has already been caught
                }catch (ArrayIndexOutOfBoundsException e) {
                 ^
1 error
```

- 모든 예외의 부모 클래스는 java.lang.Exception 클래스다. 예외는 부모 예외 클래스가 이미 catch를 하고, 자식 클래스가 그 아래에서
catch를 하도록 되어 있을 경우에는 자식 클래스가 예외를 처리할 기회가 없다.
- 그래서 "왜 필요 없는 것을 만들고 그래?"라면서 컴파일러가 친절하게 에러를 던진 것이다.
- 그렇다면 왜 여러 개가 가능할까?
```java
private void multiCatchThree() {
    int[] intArray = new int[5];
    try {
        System.out.println(intArray[5]);
    } catch (NullPointerException e) {
        System.err.println("NullPointerException occurred");
    } catch (ArrayIndexOutOfBoundsException e) {
        System.err.println("ArrayIndexOutOfBoundsException occurred");
    } catch (Exception e) {
        System.err.println(intArray.length);
    }
}
```
```text
ArrayIndexOutOfBoundsException occurred
```
- 현재 예제에선 null인 객체를 가지고 어떤 작업을 하진 않는다. 그렇다면 발생하도록 하면
```java
private void multiCatchThree() {
    int[] intArray = new int[5];
    try {
        intArray = null;
        System.out.println(intArray[5]);
    } catch (NullPointerException e) {
        System.err.println("NullPointerException occurred");
    } catch (ArrayIndexOutOfBoundsException e) {
        System.err.println("ArrayIndexOutOfBoundsException occurred");
    } catch (Exception e) {
        System.err.println(intArray.length);
    }
}
```
```text
NullPointerException occurred
```
- 왜 이런 결과가 나올까?
- intArraydml 5번째 값을 찾는 작업을 하기 전에 null인 객체를 확인하는 작업은 반드시 먼저 선행되어야 한다.
- 두 번째에 있는 ArrayIndexOutOfBoundsException 보다 NullPointerException 가 먼저 발생해 버린 것이다.
- 예외가 발생해버리면 나머지 try 블록에 있는 내용은 모두 무시되므로 ArrayIndexOutOfBoundsException이 발생하지 않는다.
- 또 한가지, catch 문을 사용할 떄에는 지금과 같이 Exception 클래스로 catch 하는 것을 가장 아래에 추가할 것을 권장한다.
- 예상치 못한 예외가 발생했을 때 제대로 처리가 되지 않는 경우를 방지해 예외들이 빠져 나가지 않게 꽁꽁 묶어두는 것이 좋다.
- 만약 감싸지 않는다면, 
```java
private void multiNoCatch() {
    int[] intArray = new int[5];
    try {
        intArray = null;
        System.out.println(intArray[5]);
    } catch (ArrayIndexOutOfBoundsException e) {
        System.err.println("ArrayIndexOutOfBoundsException occurred");
    }
}
```
```text
Exception in thread "main" java.lang.NullPointerException
	at vol1._14.exception.MultiCatchSample.multiNoCatch(MultiCatchSample.java:40)
	at vol1._14.exception.MultiCatchSample.main(MultiCatchSample.java:8)
```
- 다음과 같이 try-catch로 묶은 것이 허사가 되면서 예외 로그를 발생시킨다.

### try-catch 정리
- try 다음에 오는 catch 블록은 1개 이상 올 수 있다.
- 먼저 선언한 catch 블록의 예외 클래스가 다음에 선언한 catch 블록의 부모에 속하면, 자식에 속하는 catch 블록은 절대 실행될 일이 
없으므로 컴파일 되지 않는다.
- 하나의 try 블록에서 예외가 발생하면 그 예외와 관련이 있는 catch 블록을 찾아서 실행한다.
- catch 블록 중 발생한 예외와 관련있는 블록이 없으면, 예외가 발생되면서 해당 쓰레드는 끝난다. 따라서 마지막 catch 블록에는 
Exception 클래스로 묶어주는 버릇을 들여 놓아야 안전한 프로그램이 될 수 있다.

# 예외의 종류
- checked exception
- error
- runtime exception 혹은 unchecked exception


- error, unchecked exception을 제외한 모든 예외는 checked exception이다.

### error 
- 에러는 자바 프로그램 밖에서 발생한 예외. (하드웨어, 서버의 디스크 또는 메인보드가 고장 등)
- 자바 프로그램에 오류가 발생했을 떄, 오류의 이름이 Error 로 끝나면 에러, Exception으로 끝나면 예외이다.
- 가장 큰 차이는 프로그램 안 / 밖에서 발생했는지 여부이다. 더 큰 차이는 프로그램이 멈추어 버리느냐 계속 실행할 수 있느냐의 차이다.
- 더 정확하게는 Error는 프로세스에 영향을 주고, Exception은 쓰레드에만 영향을 준다.

### runtime exception 
- 런타임 예외는 예외가 발생할 것을 미리 감지하지 못했을 때, 발생.
- 모든 런타임 예외는 RuntimeException을 확장한 예외들이다.
- Exception을 바로 확장한 클래스는 Checked 예외이고, Exception을 확장한 RuntimeException을 확장한 클래스들이 런타임 예외이다.

# 모든 예외의 할아버지는 java.lang.Throwable 클래스다
- 당연히 Exception, Error의 공통 부모 클래스는 Object 클래스이다. 그리고 한가지 추가로 공통 부모 클래스가 java.lang.Throwable 이다.
- 그래서 Exception, Error를 처리할 때 Throwable로 처리해도 무관하다.
- 상속 관계가 이렇게 되어 있는 이유는 성격은 다르지만 모두 동일한 이름의 메소드를 사용하여 처리할 수 있도록 하기 위함이다.

## Throwable 생성자
- Throwable()
- Throwable(String message) : message는 예외 메시지
- Throwable(String message, Throwable cause) : Throwable cause는 예외의 원인
- Throwable(Throwable cause)

## Throwable 클래스에 선언되어 있는 주요 메소드
### getMessage()
- 예외 메시지를 String 형태로 제공 받음. 출력되었을 때, 어떤 예외가 발생되었는지 확인할 때 유용

### toString()
- 예외 메시지를 String 형태로 제공 받는다. getMessage() 메소드보다 더 자세하게, 예외 클래스 이름도 같이 제공

### printStackTrace()
- 가장 첫 줄에는 예외 메시지를 출력하고, 두 번째 줄부터는 예외가 발생하게 된 메소드들의 호출 관계(스택 트레이스)를 출력해준다.

```java
private void throwable() {
    int[] intArr = new int[5];
    try {
        intArr = null;
        System.out.println(intArr[5]);
    } catch (Throwable t) {
        System.out.println(t.getMessage());
        System.out.println(t.toString());
        t.printStackTrace();
    }
}```
```text
null
java.lang.NullPointerException
java.lang.NullPointerException
	at vol1._14.exception.ThrowableSample.throwable(ThrowableSample.java:13)
	at vol1._14.exception.ThrowableSample.main(ThrowableSample.java:6)
```
- 결과는 다음과 같다.
- printStackTrace()는 개발할 때에만 사용 권장. 운영할 시스템에 이 메소드를 사용하면 엄청나게 많은 양의 로그가 쌓일 것이다. 따라서
꼭 필요한 곳에만 이 메소드를 사용할 것을 권장.

# 예외 던지기 - throws
- 예외를 발생시킬(던질) 수 있다.
```java
package vol1._14.exception;

public class ThrowSample {
	public static void main(String[] args) {
		ThrowSample sample = new ThrowSample();
		sample.throwException(13);
	}

	private void throwException(int number) {
		try {
			if (number > 12) {
				throw new Exception("Number is over than 12");
			}
			System.out.println("Number is " + number);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```
```text
java.lang.Exception: Number is over than 12
	at vol1._14.exception.ThrowSample.throwException(ThrowSample.java:12)
	at vol1._14.exception.ThrowSample.main(ThrowSample.java:6)
```
- 1년 12달을 처리하는 로직에서 13월이 넘어오면 예외를 발생시키는 코드이다.
- try 내에서 throw 라고 명시하고, 예외 클래스의 객체를 생성할 수 있다. throw 이후 try 블록 문장은 수행되지 않고, catch 블록으로 이동한다.
- 해당하는 예외가 catch 내에 없다면 메소드 밖으로 던져버린다. 즉, 예외가 발생된 메소드를 호출한 메소드로 던진다는 의미. 이럴 때
사용하는 것이 throws 구문

```java
private void throwException(int number) throws Exception {
    if (number > 12) {
        throw new Exception("Number is over than 12");
    }
    System.out.println("Number is " + number);
}
```
- 이렇게 메소드 선언에 해 놓으면, try-catch로 묶지 않아도 되지만, 개발이 어려워진다. 
- 이 throwsException()에 Exception을 던진다고 메소드 선언부에 throws를 하였기 떄문에, throwsException() 메소드를 호출한 메소드에서는
반드시 try-catch 블록으로 throwsException() 메소드를 감싸주어야만 한다. 그렇지 않을 경우 컴파일 에러 발생
```text
vol1/_14/exception/ThrowSample.java:6: error: unreported exception Exception; must be caught or declared to be thrown
                sample.throwException(13);
                                     ^
1 error
```
- 컴파일 에러 메시지에는 "caught 되거나 throw 한다고 선언되어야만 한다"이다.
- 해당 컴파일 에러는 try-catch로 묶거나 main에서도 다시 throws를 해버리면 된다.
```java
public static void main(String[] args) {
    ThrowSample sample = new ThrowSample();
    try {
        sample.throwException(13);
    } catch (Exception e) {
        throw new RuntimeException(e);
    }
}
```
```java
public static void main(String[] args) throws Exception {
    ThrowSample sample = new ThrowSample();
    sample.throwException(13);
}
```
- 하지만 이미 throws 한 것을 다시 throws 하는 방법은 그리 좋은 습관이 아니다. 가장 좋은 방법은 try-catch로 처리하는 것.

### throws와 throw 정리
- 메소드를 선언할 때 매개 변수 소괄호 뒤에 throws 라는 예약어를 적어 준 뒤 예외를 선언하면, 해당 메소드에서 선언한 예외가 발생했을 때 호출한
메소드로 예외가 전달된다. 만약 메소드에서 두 가지 이상의 예외를 던질 수 있다면 implements 처럼 콤마로 구분하여 예외 클래스 이름을 적어주면 된다.
- try 블록 내에서 예외를 발생시킬 경우에는 throw 라는 예약어를 적어 준 뒤 예외 객체를 생성하거나, 생성되어 있는 객체를 명시해준다.
throw한 예외 클래스가 catch 블록에 선언되어 있지 않거나, throws 선언에 포함되어 있지 않으면 컴파일 에러가 발생한다.
- catch 블록에서 예외를 throw 할 경우에도 메소드 선언의 throws 구문에 해당 예외가 정의되어 있어야만 한다.

## 예외 직접 만들기
- Throwable을 직접 상속 받는 클래스는 Exception, Error가 있다.
- Exception을 처리하는 예외 클래스는 임의로 추가해서 만들 수 있다. 한 가지 조건은 Throwable이나 그 자식 클래스의 상속을 받아야만 한다.
- Throwable 클래스의 상속을 받아도 되지만, java.lang.Exception 클래스의 상속을 받는 것이 좋다.

```java
package vol1._14.exception;

public class MyException extends Exception {
	public MyException() {
		super();
	}

	public MyException(String message) {
		super(message);
	}
}
```
```java
package vol1._14.exception;

public class CustomException {
	public static void main(String[] args) {
		CustomException sample = new CustomException();
		try {
			sample.throwMyException(13);
		} catch (MyException mye) {
			mye.printStackTrace();
		}
	}

	private void throwMyException(int number) throws MyException {
		try {
			if (number > 12) {
				throw new MyException("Number is over than 12");
			}
		} catch (MyException e) {
			e.printStackTrace();
		}
	}
}
```
- 이와 같이 직접 만든 예외를 던지고, catch 블록에서 사용하면 된다.
- 이 메소드는 호출하는 메소드에서는 MyException으로 catch할 필요는 없다.
- 여기서 만약 Throwable 클래스의 자식 클래스가 되어야 하는 MyException에서 관련 클래스를 확장하지 않으면 제대로 컴파일 되지 않는다.
```java
package vol1._14.exception;

public class MyException /*extends Exception*/ {
	public MyException() {
		super();
	}

	public MyException(String message) {
	}
}
```
```text
vol1/_14/exception/CustomException.java:8: error: incompatible types: MyException cannot be converted to Throwable
                } catch (MyException mye) {
                         ^
vol1/_14/exception/CustomException.java:9: error: cannot find symbol
                        mye.printStackTrace();
                           ^
  symbol:   method printStackTrace()
  location: variable mye of type MyException
vol1/_14/exception/CustomException.java:13: error: incompatible types: MyException cannot be converted to Throwable
        private void throwMyException(int number) throws MyException {
                                                         ^
vol1/_14/exception/CustomException.java:16: error: incompatible types: MyException cannot be converted to Throwable
                                throw new MyException("Number is over than 12");
                                ^
vol1/_14/exception/CustomException.java:18: error: incompatible types: MyException cannot be converted to Throwable
                } catch (MyException e) {
                         ^
vol1/_14/exception/CustomException.java:19: error: cannot find symbol
                        e.printStackTrace();
                         ^
  symbol:   method printStackTrace()
  location: variable e of type MyException
6 errors
```
- 따라서 임의로 예외 클래스를 만들 때에는 반드시 Throwable 직계 자손 클래스들을 상속받아 만들어야만 한다.

## 자바 예외 처리 전략
- 자바 예외 처리는 표준을 정해 두어야만 한다.
- 먼저, 우리가 직접 Exception을 확장하여 예외 클래스를 만들었는데, 이 예외가 항상 발생하지 ㅇ낳고, 실행시에 발생할 확률이 매우 높은 경우에는
런타임 예외로 만드는 것이 나을 수도 있다. (extends RuntimeException) 이렇게 되면, throw를 하더라도 try-catch로 묶지 않아도 된다.
- 하지만, 이 경우, 예외가 발생할 경우 해당 클래스를 호출하는 다른 클래스에서 예외를 처리하도록 구조적인 안전 장치가 되어 있어야만 한다.
  (try-catch로 묶지 않은 메소드를 호출하는 메소드에서 예외를 처리하는 try-catch가 되어 있는 것을 의미)
```text
public void methodCaller() {
    try {
        methodCallee();
    } catch(Exception e) {
        // 예외 처리
    }
}

public void methodCallee() {
    // RuntimeException 예외 발생 가능성 있는 부분
}
```
- 이와 같이 RuntimeException이 발생하는 코드가 있다면, 그 메소드를 호출하는 메소드는 try-catch 로 묶어주지 않더라도 컴파일할 때
문제가 발생하지는 않지만, 예외가 발생할 확률은 높으므로, try-catch로 묶어주는 것이 좋다.
- 그리고 catch 문장에서 처리를 해 줄때 아래와 같이 처리하는 것은 반드시 피해야만 한다.
```text
try {
    // 예외 발생 가능한 코드
} catch(SomeException e) {
    // 아무 코드 없음
}
```
- catch 문장 내에서 아무런 처리를 하지 않으면 나중에 예외가 발생했을 경우 어디서 문제가 발생했는지 전혀 찾을 수 없다.
- 따라서, 개발 표준을 잡을 때 catch 문 내에서 어떻게 처리할지를 명시적으로 선언해 두어야만 한다. 다시 말해서 catch문에서 로그를 남기는 등의 작업을 하고
예외를 throw를 이용하여 던져 주어야 문제가 발생한 정확한 원인을 찾을 수 있다.

### 자바 예외 처리 전략 정리
- 임의의 예외 클래스를 만들 때에는, 반드시 try-catch로 묶어줄 필요가 있을 경우에만 Exception 클래스를 확장한다. 일반적으로
실행시 예외를 처리할 수 있는 경우에는 RuntimeException 클래스를 확장하는 것을 권장한다.
- catch 문 내에 아무런 작업 없이 공백을 놔두면 예외 분석이 어려워지므로 꼭 로그 처리와 같은 예외처리를 해줘야만 한다.
