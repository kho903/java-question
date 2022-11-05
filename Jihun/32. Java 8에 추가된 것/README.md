# Java 8의 새로운 것들
- Java 8에서 추가되거나 변경된 것들은 매우 많다. 그 중에서 꼭 알아둬야 하는 것은 다음과 같다.
  - Lambda(람다) 표현식
  - Functional(함수형) 인터페이스
  - Stream (스트림)
  - Optional (옵셔널)
  - 인터페이스의 기본 메소드 (default method)
  - 날짜 관련 클래스들 추가
  - 병렬 배열 정렬
  - StringJoiner 추가

# Optional
- Optional은 "선택적인"이라는 의미. 즉, 선택적으로 뭔가를 처리할 때 사용한다는 것을 예상 가능. 
- 이 Optional은 Functional 언어인 Haskell과 Scala에서 제공하는 기능을 따 온 것이다. 즉, 객체를 편리하게 처리하기 위해 만든 클래스.
- Optional 클래스는 java.util 패키지에 속해 있고 선언부는 다음과 같다.
```java
public final class Optional<T>
    extends Object
```
- Object 클래스를 확장했고 final 클래스로 선언되어 있으며, Generic한 클래스. 
- final 변수는 변경 불가능하지만, final 클래스는 추가적인 확장이 불가능하다. 즉, 자식 클래스를 만들 수 없다.
- Optional 클래스에 대해 이해하려면 Optional 클래스는 하나의 깡통이라고 생각하면 된다. 이 깡통에 물건을 넣을 수도 있고 아무 물건이 없을 수도 있다. 그래서 깡통을 만들기 위해
Optional 클래스는, `new Optional()`과 같이 객체를 생성하지 않는다. API 문서를 보면 Optional 클래스를 리턴하는 empty(), of(), ofNullable() 메소드들이 존재.
이 메소드들을 사용하여 객체를 생성하는 방법은 다음과 같다.
```java
private void createOptionalObjects() {
    Optional<String> emptyString = Optional.empty();  // 1
    String common = null;
    Optional<String> nullableString = Optional.ofNullable(common);  // 2
    common = "common";
    Optional<String> commonString = Optional.of(common);  // 3
}
```
1. 데이터가 없는 Optional 객체를 생성하려면 이와 같이 empty() 메소드를 사용한다.
2. 만약 null이 추가될 수 있는 상황이라면 ofNullable() 메소드를 사용한다.
3. 반드시 데이터가 들어갈 수 있는 상황에는 of() 메소드를 사용한다.
- 이와 같이 Optional 클래스의 객체를 생성하는 방법은 3가지 이다.
- Optional 클래스가 비어 있는지 확인하는 메소드는 isEmpty() 가 아닌 isPresent() 메소드
```java
private void checkOptionalData() {
    System.out.println(Optional.of("present").isPresent());
    System.out.println(Optional.ofNullable(null).isPresent());
}
```
```text
true
false
```
- 그러면 값을 꺼내는 방법은 어떻게 될까? API 문서에 "T"를 리턴하는 메소드를 살펴보면 된다.
- 바로 get(), orElse(), orElseGet(), orElseThrow()가 데이터를 리턴하는 메소드들이다.
```java
public static void main(String[] args) throws Exception {
    OptionalSample sample = new OptionalSample();
    Optional<String> data = Optional.ofNullable(null);
    sample.getOptionalData(data);
}
private void getOptionalData(Optional<String> data) throws Exception {
    String defaultValue = "default";
    // String result1 = data.get(); // 1
    String result2 = data.orElse(defaultValue); // 2
    System.out.println(result2);
    Supplier<String> stringSupplier = new Supplier<String>() {
        @Override
        public String get() {
            return "godOfJava";
        }
    };
    String result3 = data.orElseGet(stringSupplier); // 3
    System.out.println(result3);
    Supplier<Exception> exceptionSupplier = new Supplier<Exception>() {
        @Override
        public Exception get() {
            return new Exception();
        }
    };
    String result4 = data.orElseThrow(exceptionSupplier); // 4
    System.out.println(result4);
}
```
```text
default
godOfJava
Exception in thread "main" java.lang.Exception
	at vol2._32.OptionalSample$2.get(OptionalSample.java:46)
	at vol2._32.OptionalSample$2.get(OptionalSample.java:43)
	at java.base/java.util.Optional.orElseThrow(Optional.java:408)
	at vol2._32.OptionalSample.getOptionalData(OptionalSample.java:49)
	at vol2._32.OptionalSample.main(OptionalSample.java:14)
```
- 이와 같이 4가지의 데이터를 꺼내는 방법이 존재.
1. 가장 많이 사용되는 get() 메소드. 만약 데이터가 없을 경우에는 null이 리턴.
2. 만약 값이 없을 경우, orElse() 메소드를 사용해 기본값 지정 가능
3. Supplier<T> 라는 인터페이스를 활용하는 방법으로 orElseGet() 메소드 사용 가능
4. 만약 데이터가 없을 경우 예외를 발생시키고 싶다면, orElseThrow() 메소드를 사용한다. 여기서 Exception도 3과 마찬가지로 Supplier<T> 인터페이스를 사용한다.
- Supplier<T> 는 람다 표현식에서 사용하려는 용도로 만들어 졌으며, get() 메소드가 선언되어 있다.
- 언제 Optional 클래스가 필요할까? Optional은 null 처리를 보다 간편하게 하기 위해서 만들어졌다. 자칫 잘못하면 NullPointerException이 발생할 수도 있는데, 이 문제를
간편하고 명확하게 처리하려면 Optional을 사용하면 된다. 단, Optional 클래스에 값을 잘못 넣으면 NoSuchElementException이 발생할 수 있으니 유의해야 한다.

# Default method
- Java 8부터는 default 메소드가 추가되었다. DefaultStaticInterface를 보자. 
```java
package vol2._32.defaultmethod;

public interface DefaultStaticInterface {
	static final String name = "GodOfJavaBook";
	static final int since = 2013;
	String getName();
	int getSince();
	default String getEmail() {
		return name + "@godofjava.com";
	}
}
```
- Java 8 에서 default를 사용하여 인터페이스 내에 메소드를 구현할 수 있다.
- 이 인터페이스를 구현해보자.
```java
package vol2._32.defaultmethod;

public class DefaultImplementedChild implements DefaultStaticInterface {
	@Override
	public String getName() {
		return name;
	}

	@Override
	public int getSince() {
		return since;
	}
}
```
- abstract 클래스의 경우 extends를 사용해야 하지만, default를 사용한 인터페이스는 implements 키워드를 사용하여 구현하면 된다.
- default 메소드를 만든 이유는 바로 "하위 호환성" 때문이다. 예를 들어 만약 오픈 소스 코드를 만들었다고 가정하자. 그 오픈 소스가 엄청 유명해져서 전 세계 사람들이 다 사용하고 
있는데, 인터페이스에 새로운 메소드를 만들어야 하는 상황이 발생했다. 자칫 잘못하면 내가 만든 오픈 소스를 사용한 사람들은 전부 오류가 발생하고 수정을 해야 하는 일이 발생할 수도 있다.
이럴 때 사용하는 것이 바로 default 메소드이다.
- 이렇게 간단하게 라이브러리를 공유하지 않더라도, 비행기나 자율 주행차에 포함되는 프로그램이 수정되어야 한담녀 더 심각한 문제를 야기할 수도 있기 때문에 default 메소드가 나왔다고
기억하면 된다.

# 날짜 관련 클래스들
- Java 8에서 새롭게 추가된 클래스들 중에서 날짜 관련 클래스들이 있다. 이전에는 Date나 SimpleDateFormat라는 클래스를 사용해 날짜를 처리해 왔다. 하지만 이들 클래스들은
쓰레드에 안전하지도 않다. 그래서 하나의 클래스에 생성해 놓은 이들 클래스는 여러 쓰레드에서 접근할 때 예기지 못한 값들을 리턴할 수도 있었다. 그리고 불변 (immutable) 객체도 아니어서
지속적으로 값이 변경 가능했다.
- 게다가 API 구성도 복잡하게 되어 있어서 연도는 1990년부터 시작하도록 되어 있고, 달은 1부터 일은 0부터 시작한다. 그래서 1900년 1월 1일은 1900, 1, 0 을 매개변수로 넘겨줘야만
했다. 이러한 여러 가지 이슈들 때문에 Java 8에서는 java.time이라는 패키지를 만들었다.
- 기존 클래스와 신규 클래스들의 차이는 다음과 같다.

| 내용           | 패키지                                                               | 설명                                                                                               |
|--------------|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| 값 유지 예전 버전   | java.util,Date, java.util.Calendar                                | Date 클래스는 날짜 계산을 할 수 없다. Calendar 클래스는 불변 객체가 아니므로 연산시 객체 자체가 변경되었다.                             |
| 값 유지 Java 8  | java.tim.ZonedDateTime, java.time.LocalDate 등                     | ZonedDateTime과 LocalDate 등은 불변 객체이다. 모든 클래스가 연산용의 메소드를 갖고 있으며, 연산시 새로운 불변 객체를 돌려준다. 그리고 쓰레드에 안전. |
| 변경 예전 버전     | java.text.SimpleDateFormat                                        | SimpleDateFormat은 쓰레드 안전하지 않고 느리다.                                                               |
| 변경 Java 8    | java.time.format.DateTimeFormatter                                | DateimeFormatter는 쓰레드 안전하며 빠르다.                                                                  |
| 시간대 예전 버전    | java.util.TimeZone                                                | "Asia/Seoul"이나 "+9:00" 같은 정보를 가진다.                                                               |
| 시간대 Java 8   | java.time.ZoneId, java.time.ZoneOffset                            | ZoneId는 "Asia/Seoul"라는 정보를 갖고 잇고, ZoneOffset는 "+09:00"라는 정보를 가지고 있다.                             |
| 속성 관련 예전 버전  | java.util.Calendar                                                | Calendar.YEAR, Calendar.MONTH, Calendar.Date(또는 Calendar.DAY_OF_MONTH) 등 이들은 정수(int)이다.          |
| 속성 관련 java 8 | java.time.temporal.ChronoField (java.time.temporal.TemporalField) | ChronoField.YEAR, ChronoField.MONTH_OF_YEAR, ChronoField.DAY_OF_MONTH 등이 enum 타입이다.              |
| 속성 관련 java 8 | java.time.temporal.ChronoUnit (java.time.temporal.TemporalUnit)   | ChronoUnit.YEARS(연수), ChronoUnit.MONTHS(개월), ChronoUnit.DAYS(일) 등이 enum 타입이다.                    |

- 앞서 이야기한 이유 외에도 시간을 처리하는 편의를 위한 클래스와 enum들이 추가되었다.
- 추가로 시간을 나타내는 클래스는 Local, Offset, Zoned 로 3가지 종류가 존재한다.
  - Local : 시간대가 없는 시간. 예를 들어 "1시"는 어느 지역의 1시인지 구분되지 않는다.
  - Offset : UTC (그리니치 시간대)와의 오프셋(차이)을 가지는 시간. 한국은 "+09:00"
  - Zoned : 시간대 ("한국 시간"과 같은 정보)를 갖는 시간. 한국은 "Asia/Seoul"

- 여기서 Local 과 Locale은 다른 클래스다. Locale은 지역을 의미하는 클래스이며, Local은 시간을 이야기하는 것이다. 한국어로도 "로컬", "로케일"이다.
- 누군가 SNS에 글을 올리면 그 글이 저장되는 시간은 글쓴이의 Locale(지역) 정보와 함께 저장되어야 한다. 그렇지 않으면 24시간이 다른 전 세계의 시간이 모두 꼬일 것이다.
- 그래서 보통은, 글을 쓴 사람은 자신의 시간대로 글이 보이게 되며, 글을 읽는 사람도 자신의 시간대에 맞게 글이 올라간 시간이 보인다. 그래서 ZonedDateTime이라는 클래스와 
LocalDate가 추가된 것이다.
- Java 8 에 추가된 클래스 중에서 DayOfWeek이라는 클래스가 있다. 지금까지는 요일을 표현하기 위해 한글을 배열에 넣는 등의 작업이 필요했지만, 이제는 DayOfWeek 클래스를
사용하면 된다. 정확히는 DayOfWeek은 enum 이다.
```java
package vol2._32;

import java.time.DayOfWeek;
import java.time.format.TextStyle;
import java.util.Locale;

public class DayOfWeekSample {
	public static void main(String[] args) {
		DayOfWeekSample sample = new DayOfWeekSample();
		sample.printDayOfWeek();
	}

	private void printDayOfWeek() {
		DayOfWeek[] dayOfWeeks = DayOfWeek.values();
		Locale locale = Locale.getDefault();
		for (DayOfWeek day : dayOfWeeks) {
			System.out.print(day.getDisplayName(TextStyle.FULL, locale) + " ");
			System.out.print(day.getDisplayName(TextStyle.SHORT, locale) + " ");
			System.out.println(day.getDisplayName(TextStyle.NARROW, locale));
		}
	}
}
```
```text
월요일 월 월
화요일 화 화
수요일 수 수
목요일 목 목
금요일 금 금
토요일 토 토
일요일 일 일
```
- DayOfWeek 클래스는 MONDAY부터 SUNDAY 까지의 상수가 enum에 선언되어 있다. 그래서 각 요일을 가져다 쓸 때에는 DayOfWeek.MONDAY 처럼 사용하면 된다. 여기서는 values()
메소드를 사용해 모든 요일을 가져왔다.
- DayOfWeek 클래스에서는 getDisplay() 메소드를 사용해 해당 요일을 출력할 수 있다. 그런데, 이 메소드에는 TextStyle과 Locale 지역 정보를 전달해 줘야만 한다.
- TextStyle에는 Full, SHORT, NARROW 라는 이미 정의되어 있는 스타일들이 존재.
- Full은 "월요일"같이 "요일"도 붙는다. 한글에서는 SHORT나 NARROW의 차이가 없다. 다른 나라의 요일을 확인하는 메소드는 다음과 같다.
```text
private void printDayOfWeekOfLocales() {
    DayOfWeek day = DayOfWeek.SUNDAY;
    Locale[] locales = Locale.getAvailableLocales();
    for (Locale locale : locales) {
        System.out.print(locale.getCountry() + " ");
        System.out.print(day.getDisplayName(TextStyle.FULL, locale) + " ");
        System.out.print(day.getDisplayName(TextStyle.SHORT, locale) + " ");
        System.out.println(day.getDisplayName(TextStyle.NARROW, locale));
    }
}
```
```text
US Sunday Sun S
....
KR 일요일 일 일
...
GB Sunday Sun S
```
- 날짜와 관련된 내용은 아래 링크에 상세한 내용 확인 가능
- https://docs.oracle.com/javase/tutorial/datetime/iso/index.html

# 병령 배열 정렬 (Parallel array sorting)
- 자바를 사용하면서, 배열을 정렬하는 가장 간편한 방법은 java.util 패키지의 Arrays 클래스를 사용하는 것. 이 Arrays 클래스에는 다음과 같은 static 메소드 존재.
  - binarySearch() : 배열 내에서의 검색
  - copyOf() : 배열의 복제
  - equals() : 배열의 비교
  - fill() : 배열 채우기
  - hashCode() : 배열의 해시 코드 제공
  - sort() : 정렬
  - toString() : 배열 내용을 출력
- Java 8에서는 parallelSort() 라는 정렬 메소드가 제공되며, Java 7에서 소개된 Fork-Join 프레임웍이 내부적으로 사용된다.
- 사용법은 다음과 같다.
```java
int[] intValues = new int[10];
// 배열 값 지정
Arrays.parallelSort(intValues);
```
- 이렇게 해당 배열을 매개 변수로 집어 넣으면 정렬이 된다. 
- sort()의 경우 단일 쓰레드로 수행되며, parallelSort()는 필요에 따라 여러 개의 쓰레드로 나뉘어 작업이 수행된다. 따라서 parallelSort()가 CPU를 더 많이 사용하게 되겠지만,
처리 속도는 더 빠르다.
- 실제 두 개의 성능을 비교 테스트한 결과를 보면, 5,000 개 정도부터 parallelSort()의 성능이 더 빨라지는 것을 볼 수 있다.
- 따라서 개수가 많지 않은 배열에서는 굳이 parallelSort()를 사용할 필요는 없다고 봐도 무방하다.

# StringJoiner
- 앞서 문자열을 처리하는 방법에 String, StringBuilder, StringBuffer 등에 대해서 설명했다. 그리고 문자열을 예쁘게 처리하기 위한 Formatter 라는 클래스도 있다. 
- Java 8 에서는 StringJoiner 라는 클래스가 추가되었다. 이 클래스는 java.util에 포함되어 있으며, 순차적으로 나열되는 문자열을 처리할 때 사용한다.
```java
String[] stringArray = new String[]{"StudyHard", "GodOfJava", "Book"};
```
- 이와 같은 배열을 다음과 같이 변환하려고 하면 어떻게 해야 할까?
```text
{StudyHard,GodOfJava,Book}
```
- 이전 버전에서는 String 에 계속 더하거나, StringBuilder나 StringBuffer를 사용할 것이다.
- 게다가 기본적으로 콤마(,)에 대한 처리를 해주지 않으면, 다음과 같은 결과가 나올 것이다.
```text
{StudyHard,GodOfJava,Book,}
```
- 이러한 단점을 보완하기 위해 StringJoiner가 만들어졌다.
- 만약 배열의 구성요소 사이에 콤마만 넣고자 하려면 다음과 같이 사용하면 된다.
```java
package vol2._32;

import java.util.StringJoiner;

public class StringJoinerSample {
	public static void main(String[] args) {
		StringJoinerSample sample = new StringJoinerSample();
		String[] strings = {"study", "GodOfJava", "Book"};
		sample.joinStringOnlyComma(strings);
	}

	private void joinStringOnlyComma(String[] stringArray) {
		StringJoiner joiner = new StringJoiner(",");
		for (String string : stringArray) {
			joiner.add(string);
		}
		System.out.println(joiner);
	}
}
```
```text
study,GodOfJava,Book
```
- 아주 간단하게 배열의 구성 요소 사이 사이마다 콤마가 들어간 것을 볼 수 있다. 앞 뒤에 소괄호는 다음과 같이 넣을 수 있다.
```java
private void joinString(String[] stringArray) {
    StringJoiner joiner = new StringJoiner(",", "(", ")");
    for (String string : stringArray) {
        joiner.add(string);
    }
    System.out.println(joiner);
}
```
```text
(study,GodOfJava,Book)
```
- 생성자에 맨 앞에 들어갈 prefix와 뒤에 들어갈 suffix 값을 지정해 주면 된다.
- 이렇게 StringJoiner를 사용하는 방법도 있지만, 다음 장에서 배울 스트림과 람다 표현식을 사용하면 다음과 같이 코드를 작성할 수도 있다.
```java
private void withCollector(String[] stringArray) {
    List<String> stringList = Arrays.asList(stringArray);
    String result = stringList.stream()
        .collect(Collectors.joining(","));
    System.out.println(result);
}
```
