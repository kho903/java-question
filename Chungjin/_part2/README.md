### jar

: jar는 여러 개의 클래스 파일을 하나의 파일로 묶기 위해서 사용한다. 

> jar 파일은 그냥 압축 파일이라고 생각하자.
> 

jar의 주요 옵션. 

- -c : jar 만들기.
- -u : jar 수정하기
- -x : jar 풀기
- -t : jar 내의 파일 목록 확인하기.

그리고 이 옵션들에 붙여서 사용할 수 있는 옵션은 다음과 같다. 

- -f : 파일명 지정.
- -v : verbose 로그 출력
- -m : manifest 파일 지정.

**jar 만들기**

```bash
$ cd : C:\godofjava
$ jar -cvf b.jar ./b
```

- jar뒤에 cvf 옵션을 붙이고, 묶을 jar 파일을 지정한 후 가장 마지막에는 묶을 디렉터리를 지정했다.
- 이렇게 c 옵션은 jar 파일을 만들고, v 옵션으로 어떤 파일들을 추가하는지 출력하고, f 옵션으로 저장할 파일을 지정하면 jar 파일을 만들 수 있다.

**jar 내의 파일 목록 확인하기.** 

```bash
jar -tvf b.jar
```

이렇게 c 대신 t 옵션을 사용하면 현재 jar 파일에 묶여 있는 파일들의 목록을 확인할 수 있다. 

**jar 수정하기.**

```bash
jar -uvf b.jar ./c
```

이처럼 u 옵션을 지정하고, 추가할 파일이 있는 c라는 이름의 디렉터리를 지정하면 디렉터리를 포함한 그 안에 있는 파일들이 b.jar 파일에 추가되어 저장된다. 

**jar 풀기.** 

```bash
$ mkdir temp
$ mv b.jar /temp
$ cd temp
$ jar -xvf b.jar
```

- temp라는 디렉터리를 만들고
- b.jar 파일을 temp로 옮긴 후
- 현재 작업 디렉터리를 temp로 바꾸고
- x 옵션을 사용하여 jar 파일을 푼다.

---

### classpath와 자바 옵션들.

자바로 개발을 하려면 classpath라는 것을 반드시 알아야만 한다. 

jar 파일 하나에 모든 라이브러리가 들어갈 수도 있지만, 여러 개의 jar 파일을 한꺼번에 사용해야 하는 경우가 생기기도 한다.

그러면 이러한 library를 어떻게 사용할 수 있을까?

- -classpath 라는 옵션을 컴파일할 때나 실행할 때 다음과 같이 추가해 주면 된다.
- -classpath를 줄여서 -cp 라고 써도 된다.

```bash
$ java -classpath c:\godofjava Calculator
$ java -cp c:\godofjava Calculator
```

패키지가 지정되지 않는 클래스를 실행한 경우.

```bash
$ java -classpath /godfojava Calculator
$ java -cp /godofjava Calculator
```

패키지가 있을 경우에는 다음과 같이 지정.

```bash
$ java -cp c:\godofjava b.variable.PrimitiveTypes
```

**다음과 같이 패키지까지 포함된 경로를 지정하면 안된다.** 

```bash
$ java -cp c:\godofjava\b\variable PrimitiveTypes
```

: 패키지가 지정되어 있을 때는 그 클래스가 있는 경로를 클래스 패스로 설정하면 안 된다. 

**여러 개의 jar 파일이나 경로를 클래스패스에 지정할 때는 어떻게 해야 할까?**

예) junit.jar와 apache-commons.jar라는 파일이 c:\godofjava\lib 라는 경로에 있을 경우에 다음과 같이 지정해 주면 된다. 

작성한 프로그램의 기본 경로 c:\godofjava

```bash
$ java -cp c:\godofjava\lib\junit.jar;c:\godofjava\lib\apache-commons.jar;c\godofjava\Calculator
```

그리고 여러 개의 jar 파일이 하나의 디렉터리에 존재하는 경우에는 다음과 같이 와일드 카드 문자인 *를 사용해도 된다. 

```bash
$ java -cp c:\godofjava\lib\*;c:\godofjava Calculator 
```

> 클래스 패스 경로 사이에는 공백이 있으면 안 된다.
> 

일반적인 클래스 패스의 환경 변수 이름은 CLASSPATH라고 지정한다. 

윈도우는 다음과 같이 지정. 

```bash
$ set CLASSPATH=C:\godofjava\lib\*;c:\godofjava;
```

유닉스는 export라는 명령을 실행. 

```bash
$ export CLASSPATH=/godofjava/lib/*:/godofjava
```

> set이나 export 와 같은 명령을 수행한 까만 화면을 닫으면, 그 설정은 없어진다. 

그 단점을 해결하기 위해서 윈도우 계열에는 OS의 환경 설정값에 지정하면 되고, 유닉스 계열에서는 프로파일 파일에 설정해 주면 된다.
> 

윈도우는 환경 변수의 앞과 뒤에 %를 붙이면 된다. 

```bash
$ java -cp %CLASSPATH% Calculator
```

유닉스 계열의 OS에서는 다음과 같이 환경 변수 이름 앞에 $를 붙여주면 된다.

```bash
$java -cp $CLASSPATH Calculator
```

---

### 자바를 컴파일할 때 알아 두면 좋은 옵션들.

: 자바를 컴파일하는 javac 명령에는 매우 많은 옵션들이 제공된다. 

- -d 디렉터리 : javac는 클래스가 있는 디렉터리에 클래스 파일을 생성한다. 이 옵션을 제공하면 해당 디렉터리도 생성하고, 관련된 패키지 디렉터리도 생성하여 클래스 파일을 만들어준다.
- -deprecation : deprecated 된 클래스에 대한 상세한 정보를 포함하여 컴파일한다.
- -g : 디버깅과 관련된 정보를 포함한 클래스 파일을 생성한다. 운영용으로 컴파일 할 때는 이 옵션을 사용하여 컴파일해야만 한다.

---

### 자바의 표준 실행 옵션은 종류가 아주 많다.

: 옵션은 “표준 옵션”과 “비 표준 옵션”으로 나뉜다. 

표준 옵션은 대부분 -sever 나 -client 와 같이 X가 없는 옵션들이고, -Xms, -Xms 와 같이 X가 붙은 옵션들은 비표준 옵션들이다. 

표준 옵션은 앞으로 변경되지 않을 옵션들이다. 

거꾸로 X나 XX가 붙은 옵션들은 변경되거나 삭제될 수 있는 옵션들이다. 

주요 표준 옵션부터 살펴보자.

- -client : “클라이언트” VM을 사용한다. Swing과 같이 클라이언트 UI를 처리할 때 유용하다.
- -server : “서버” VM를 사용한다. 대부분의 시스템에는 이 옵션이 적용된다.
- -cp 혹은 -classpath : 클래스 패스를 지정할 때 사용하며, 이 옵션의 공백 뒤에 경로를 연달아 지정하면 된다.
- -verbose : 클래스가 JVM에 로딩되는 정보를 출력한다.
- -version : JVM의 버전을 출력하고 프로세스를 종료한다.
- -showversion : JVM의 버전을 출력하고 자바 프로세스를 계속 수행한다.
- -d32 : 가능한 경우 32비트 데이터 모델을 사용한다.
- -d64 : 가능한 경우 64비트 모델을 사용한다.
- -D키=값 : System.getProperties() 메소드를 호출했을 때 제공되는 자바의 속성에 해당 키와 값을 추가한다.

이 옵션들을 여러분들의 전부 사용할 일은 없지만, 알아두면 언젠가 필요할 때가 있을 것이다.

마지막으로 비 표준 옵션 중 알아두어야 할 옵션들.

- -Xms크기 : JVM의 시작 크기를 지정한다. -Xms512m이면 512MB 로 시작하고, -Xmxlg이면 1GB로 시작한다.
- -Xms크기 : JVM의 최대 크기를 지정한다. 뒤에 크기 옵션을 지정하는 것은 -Xms 옵션과 동일하다.
- -Xss크기 : 스레드의 스택 크기를 지정한다. StackOverflowError 가 발생할 때 이 옵션을 지정하여 스택의 크기를 증가시킬 수 있다.

여기서 -Xms와 -Xms 옵션은 JVM을 시작할 때 반드시 지정해야 한다는 것을 잊지 말자. 

---

### javadoc

: 코드를 작성할 때 주석을 달아주면 그 주석내에 있는 내용들을 API 문서로 자동 생성할 수 있다. 

- 이렇게 자동으로 생성해주는 프로그램이 javadocs이다.
- javadocs를 위한 주석은 /**으로 시작하고 */으로 끝나야만 한다.

**javadoc 주석의 규칙들.** 

: javadoc 주석은 다음의 항목에 지정할 수 있다. 

- 패키지
- 클래스 및 인터페이스
- 필드 (인스턴스 및 클래스 변수)
- 생성자 및 메소드.

---

### Formatter.

자바에서 제공하는 Formatting 기능을 살펴보자.

- 숫자 및 통화
- 날짜와 시간
- 문자열.

**1) 숫자와 통화를 나타내기 위해서는 NumberFormat 클래스를 사용.**

: 숫자 및 통화를 쉽게 표현하는 NumberFormat 클래스에 대해 살펴보자.

- 이 클래스는 abstract 클래스로 선언되어 있다.
    - 필요에 따라서 이 클래스를 확장하여 사용할 수도 있다.
- 그리고 이 클래스의 생성자는 protected로 확장한 클래스와 클래스 내부에 있는 메소드에서만 사용할 수 있도록 되어 있기 때문에, 이 클래스의 객체를 생성하려면 다음과 같이 static으로 선언되어 있는 getInstance() 메소드를 사용해야 한다.

```java
NumberFormat formatter = NumberFormatter.getInstance();
```

**NumberFormat 의 객체를 생성할 때 사용하는 static 메소드들**

| 메소드 이름 | 용도 |
| --- | --- |
| getInstance() | 현재 JVM의 기본 지역(Locale)으로 일반적인 몬적의 숫자  format 제공. |
| getInstance(Locale inLocale) | 매개 변수로 제공된 지역으로 숫자 format 제공. |
| getCurrencyInstance() | 현재 jvm의 기본 지역(Locale)으로 통화 format 제공 |
| getCurrencyInstance(Locale inLocale) | 매개 변수로 제공된 지역으로 통화 format 제공 |
| getIntegerInstance() | 현재 JVM의 기본 지역(Locale)으로 정수 format 제공 |
| getIntegerInstance(Locale inLocale) | 매개 변수로 제공된 지역으로 정수 format 제공.  |

Type 값을 Decimal로?

DECIMAL로 출력할 때 소수점 3자리까지만 출력되는데 더 자세히 출력하길 원하면 확인해보자. 

| return type | method | description |
| --- | --- | --- |
| void | setMaximumFractionDigits(int newValue) | 소수점 이하의 최대 표시 개수 지정 |
| void | setMaximumIntegerDigits(int newValue) | 정수형의 최대 표시 개수 지정 |
| void | setMinmumFractionDigits(int newValue) | 소수점 이하의 최소 표시 개수 지정 |
| void | setMinimumIntegerDigits(int newValue) | 정수형의 최소 표시 개수 지정.  |

**날짜와 시간을 간단히 표현하려면 DateFormat**

: 숫자의 NumberFormat 처럼 날짜와 시간을 표현할 때에는 DtaeFormat을 사용하면 간단하게 처리할 수 있다. 

| 메소드 이름 | 용도 |
| --- | --- |
| getInstance() | 날짜와 시간을 현재 JVM의 기본 지역(Locale) 및 짧은 형식으로 생성하는 format 제공.  |
| getDateInstance() | 날짜를 기본 지역(Locale) 및 기본 형식 (Default)으로 생성하는 format 제공. |
| getDateInstance(int style) | 날짜를 기본 지역(Locale) 및 style에 저장된 형식으로 생성하는 format  제공 |
| getDateInstance(int style, Locale aLocale) | 날짜를 aLocale에 지정된 지역 및 style에 지정된 형식으로 format 제공 |
| getTimeInstance() | 시간을 기본 지역(Locale) 및 기본 형식(Default)으로 생성자 format 제공.  |
| getTimeInstance(int style) | 시간을 기본 지역(Locale) 및 style 지정된 형식으로 생성하는 format 제공.  |
| getTimeInstance(int style, Locale alocale) | 시간을 aLocale 에 지정된 지역 및 style에 지정된 형식으로 format 제공.  |
| getDateTimeInstance() | 날짜와 시간을 기본 지역(Locale) 및 기본 형식(Default) 으로 생성하는 format 제공 |
| getDateTimeInstance(int dateStyle, int timeStyle) | 기본 지역(Locale)에 날짜는 dateStyle에 시간은 timeStyle에 지정된 형식으로 생성하는 format 제공.  |
| getDateTimeInstance(int dateStyle, int timeStyle, Locale aLocale | aLocale에 지정된 지역, 날짜는 dateStyle에, 시간은 timeStyle에 지정된 형식으로 생성하는 format 제공.  |

```java
Date date = new Date();
DateFormat formatter = DateFormat.getInstance();
String str = formatter.format(date);
```

DateFormat 클래스에 제공하는 기본 타입. 

- DEFAULT
- FULL
- LONG
- MEDIUM
- SHORT

---

### Collection 다음으로 많이 쓰는 애들은 자바 유틸.

java.lang 다음으로 많이 사용되는 java.util 패키지. 

많이 클래스 중에서 꼭 알아야 하는 클래스 목록. 

- Date와 Calendar
- Collections
- Arrays
- StringTokenizer
- Properties
- Random
- Formatter

**날짜를 처리하기 위한 Date와 calendar** 

: JDK 1.0 버전에서는 날짜를 처리하기 위해서 Date 클래스를 사용했다. 

하지만 JDK 1.1 버전부터는 Calendar 클래스를 사용하여 날짜 처리 작업을 하도록 변경되었다. 

그래서 Date 클래스에는 매우 많은 미사용(deprecated된) 메소드들이 존재한다.

Date 클래스에서는 두 개의 생성자만 사용 가능하다. ( 나머지 생성자들은 deprecated 되었다. ) 

| 생성자 | 설명 |
| --- | --- |
| Date() | 객체가 생성된 시간을 갖는 Date 객체 생성 |
| Date(long date) | 매개 변수로 넘어온 long 타입 시간을 갖는 Date 객체 생성.  |

생성자와 마찬가지로 Date 클래스의 메소드들은 deprecated 된 것들이 매우 많다. 

사용가능한 메소드들 중에서 쓸 만한 것은 getTime() 메서드와 setTime() 메소드다. 

| 리턴 타입 | 메소드 이름 및 매개 변수 | 설명 |
| --- | --- | --- |
| long | getTime() | Date 객체의 시간을 long 타입으로 리턴 |
| void | setTime(long time) | Date 객체의 시간을 매개 변수로 받은 시간으로 변경.  |

**Calendar 클래스는 다음과 같이 선언되어 있다.** 

```java
public abstract class Calendar extends Object implements Serializable, Cloneable, Comparable<Calendar>
```

- abstract 클래스라는 것을 알 수 있다. 게다가 Calendar 클래스는 생성자들이 protected로 선언되어 있어 확장하여 구현하지 않으면 생성자를 만들어 낼 수 없다.

**Calendar 클래스에는 다음 4가지 getInstance() 메소드가 있다.** 

| 객체 생성 메소드 | 설명 |
| --- | --- |
| getInstance() | 기본 객체 생성 메소드, 모든 값은 기본값이며, 현재 시간으로 지정된 Calendar 객체를 생성 |
| getInstance(Locale locale) | 지정된 지역의 Calendar 객체를 생성 |
| getInstance(TimeZone zone) | 지정된 타임존의 Calendar 객체를 생성 |
| getInstance(TimeZone zone, Locale alocale) | 지정된 타임존과 지역의 Calendar 객체를 생성 |
- Locale 클래스와 TimeZone 클래스는 모두 java.util 패키지에 선언되어 있다.
- Locale 클래스는 지역에 대한 정보를 담는다. 여러분들이 자바를 설치할 때 OS에 있는 지역 정보를 확인하여 JVM 설저엥 저장되며, 여러분들의 기본 지역은 한국으로 되어 있을 것이다.
- TimeZone 클래스는 시간대를 나타내고 처리하기 위해서 사용된다.
- TimeZone 클래스도 마찬가지로 abstract 클래스이기 때문에, 이 클래스를 편하게 사용하려면 같은 패키지에 선언된 SimpleTimeZone 클래스를 사용하면 된다.

**Calendar 클래스를 제대로 사용하기 위해서는 API 문서에 있는 상수들을 눈여겨 봐야만 한다.** 

| 구분 | 상수 |
| --- | --- |
| 날짜 계산 관련 | ERA, YEAR, MONTH, WEEK_OF_YEAR, WEEK_OF_MONTH, DATE, DAY_OF_MONTH, DAY_OF_YEAR, DAY_OF_WEEK, DAY_OF_WEEK_IN_MONTH |
| 시간 관련 | AM_PM, HOUR_OF_DAY, MINUTE, SECOND, MILLISECOND, ZONE_OFFSET, DST_OFFSET, AM, PM |
| 요일 관련 | SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY |
| 월 관련  | JANUARY, FEBRUARY, MARCH, APRIL, MAY, JUNE, JULY, AUGUST, SEPTEMBER, OCTOBER, NOVEMBER, DECEMBER, UNDECIMBER |
| 기타 | FIELD_COUNT, ALL_STYLES, SHORT, LONG |

**Calendar 클래스에 대한 마지막 설명 add()와 roll() 메소드**

| 리턴 타입 | 메소드 이름 및 매개 변수 | 설명 |
| --- | --- | --- |
| void | add(int field, int amount) | 지정한 field의 값을 amount 만큼 더한다. |
| void | roll(int field, int amount) | 지정한 field의 값을 amount 만큼 더하는데, 그 상위 값은 변경하지 않는다.  |

### 컬렉션 객체들의 도우미 collections

| 구분 | 메소드 |
| --- | --- |
| 데이터 검색 | binarySearch(), min(), max(), indexOfSubList(), lastIndexOfSubList(), frequency() |
| 데이터 정렬 | sort() |
| 데이터 순서 변경 | reverse(), shuffle(), swap(), rotate(), reverseOrder() |
| 데이터 변경 및 추가 | fill(), replaceAll(), addAll() |
| 데이터 복사 | copy(), nCopies() |
| 데이터 삭제  | emptySet(), emptyList(), emptyMap() |
| 데이터 추출 | newSetFromMap() |
| 데이터 비교 | disjoint() |
| 타입 변환 | enumeration(), list(), asLifeQueue() |
| 변경 가능 여부 속성 변경 | unmodifiableCollection(), unmodifiableSet(), unmodifiableSortedSet(), unmodifiableList(), unmodifiableMap(), unmodifiableSortedMap() |
| 스레드 안전 여부 속성 추가  | synchronizedCollection(), synchronizedSet(), synchronizedSortedMap(), synchronizedList(), synchronizedMap(), synchronizedSortedMap() |
| 데이터의 타입 안전 여부 속성 추가. | checkedCollection(), checkedSet(), checkedSortedSet(), checkedList(), checkedMap(), checkedSortedMap() |
| singleton | singleton(), singletonList(), singletonMap() |
- 대부분의 Collection 클래스의 객체를 생성할 때에는 synchronized로 시작하는 메소드를 사용하면 스레드에 안전한 클래스가 된다.
- 따라서 안전하지 않은 Collection 클래스의 객체를 생성할 때에는 synchronized로 시작하는 메소드를 사용하면 스레드에 안전한 클래스가 된다.

예) ArrayList는 다음과 같이 객체를 생성해주면 스레드에 안전한 클래스가 된다. 

```java
List list = Collections.synchronizedList(new ArrayList(...));
```

그렇다고, 무조건 스레드에 안전하게 작성하려고 이렇게 변경해 버리면 성능에 좋지 않다. 

### 배열을 쉽게 처리해주는 Arrays

: Arrays 클래스도 Collection 클래스처럼 도우미 클래스이며, 배열을 쉽게 처리할 수 있도록 도움을 주는 클래스다. 

| 구분 | 메소드 |
| --- | --- |
| 정렬 | sort() |
| 검색 | binarySearch() |
| 비교 | equals(), deepEquals() |
| 데이터 변경 | fill() |
| 복사 | copyOf(), copyOfRange() |
| 변환 | asList() |
| 해시코드 | hashCode(), deepHashCode() |
| 문자열로 변환 | toString() |

### 문자열을 자르기 위한 StringTokenizer

: StringTokenizer 클래스는 다음의 3가지 생성자가 있다. 

| 생성자 | 설명 |
| --- | --- |
| StringTokenizer(String str) | 기본 구분자로 매개 변수로 넘어온 문자열(str)을 나눈다. 기본 구분자는 “ \t\n\r\f”이 있다(지면 상으로는 안보이겠지만, 맨 앞에 공백이 있다 ) , 캐리지 리턴, 폼 피드를 나타내는 char를 만나면 문자열을 분리한다. |
| StringTokenizer(String str, String delim) | 지정된 구분자(delim)로 문자열(str)을 나눈다. |
| StringTokenizer(String str, String delim, boolean returnDelims) | 마지막 매개 변수 값(returnDelims) 이 false일 때에는 매개 변수가 두 개인 생성자와 동일하지만, 이 값이 true이면, 구분자도 같이 리턴한다.  |

### 속성 파일들을 관리하기 위한 Properties

: java.util 패키지에 Properties 라는 클래스가 존재한다. 

이 클래스는 Hashtable 클래스를 확장한 클래스다. 

| 리턴 타입 | 메소드 이름 및 매개 변수 | 설명 |
| --- | --- | --- |
| String | getProperty(String key) | “키”를 갖는 속성 “값”을 받는다. |
| String | getProperty(String key, String defaultValue) | "키”를 갖는 속성 “값”을 받으며, 해당 “값”이 없을 경우에는 defaultValue를 받는다. |
| Object | setProperty(String key, String value) | “키”에 해당하는 “값”을 저장한다. |
| void | load(Reader reader) | reader 객체에 해당하는 파일을 읽는다. |
| void | store(Writer writer, String comments) | writer 객체에 해당하는 파일에 저장하고, 그 파일의 윗부분에 comments 값에 지정한 주석을 저장한다.  |

### Java.math 패키지의 BigDecimal 클래스.

: 돈 계산과 같이 중요한 작업을 할 때에는 BigDecimal 을 사용해 야한다. 

- 이 클래스는 java.math 패키지에 선언되어 있다.

BigDecimal에서 기본적인 사칙연산을 수행하는 메소드

| 리턴 타입 | 메소드 이름 및 매개 변수 | 설명 |
| --- | --- | --- |
| BigDecimal | add(BigDecimal augend)
add(BigDecimal augend, MathContext mc) | 더하기 연산 |
| BigDecimal | substract(BigDecimal subtrahed)
subtrract(BigDecimal subtrahed, MathContext mc) | 빼기 연산 |
| BigDecimal | multiply(BigDecimal multiplicand)
multiply(BigDecimal multiplicand, MathContext mc) | 곱하기 연산 |
| BigDecimal | substract(BigDecimal subtrahend)
substract(BigDecimal subtrahead, MathContext mc) | 나누기 연산 |

### 자바의 ThreadLocal

: 각 스레드에서  혼자 쓸 수 있는 값을 가지려면 ThreadLocal을 쓰면 된다. 

- 앞에서 여러 스레드에서 데이터를 공유할 때 발생하는 문제을 해결하기 위해서, synchronized 라는 구문을 사용했다.
    - 만약 스레드 별로 서로 다른 값을 처리해야 하는 필요가 있을 때에는
        - ThreadLcoal이라는 것을 사용하면 된다.

**ThreadLocal 에 대해서 정리.**

- ThreadLocal에 저장된 값은 해당 스레드에서 고유하게 사용할 수 있다.
- ThreadLocal 클래스의 변수는 private static final 로 선언한다.
- ThreadLocal 클래스에 선언되어 있는 메소드는 set(), get(), remove(), initialValue()가 있다.
- 사용이 끝난 후에는 remove() 메소드를 호출해 주는 습관을 가져야만 한다.

> 웹 기반 어플리케이션에서는 스레드를 재사용하기 위해서 ThreadPool이라는 것을 사용한다. 

스레드 풀을 사용하면, 스레드가 시작된 후에 그냥 끝나는 것이 아니기 때문에 remove() 메소드를 사용하여 값을 지워줘야지만 해당 스레드를 다음에 사용할 때 스레드 값이 들어 있지 않게 된다.
> 

### 자바의 volatile

- 스레드에 선언된 인스턴스 변수를 선언할 일이 있을 때, 이처럼 volatile로 선언하면
    - 해당 변수 값이 바뀌면 “내가 갖고 있는 volatile 변수가 바뀌었는데, 너도 이거 쓰니까 값을 바꿔”라고 이야기 해준다.
        - 그러므로 같은 객체에 있는 변수는 모든 스레드가 같은 값을 바라보게 된다.

volatile을 남발하면 성능상으로 저하가 발생한다. 

모든 스레드의 인스턴스 변수에 이렇게 volatile이라고 적어주지 않았다고 데이터가 꼬이는 일이 발생하는 것은 아니다. 

책의 예제는 volatile을 처리하지 않았을 때 문제가 발생한 이유는 JIT 컴파일러가 최적화 작업을 수행하기 때문이다. 

즉, 최적화가 되지 않아 캐시간에 데이터가 다른 값을 보지 않으면 volatile을 사용할 필요가 없다.
