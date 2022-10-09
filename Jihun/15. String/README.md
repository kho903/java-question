# 자바에서 가장 많이 사용하는 String 클래스
- String 클래스는 다음과 같이 선언되어 있다.
```text
public final class String extends Object
    implements Serializable, Comparable<String>, CharSequence
```
- public final 클래스로, public은 누구나 사용할 수 있는 클래스, final은 더 이상 확장할 수 없는 클래스이다. 
- implements는 Serializable, Camparable<String>, CharSequence
  - Serializable은 구현해야 하는 메소드가 하나도 없는 아주 특이한 인터페이스로, 이렇게 명시하면, 해당 객체를 파일로 저장하거나
    다른 서버에 전송 가능한 상태가 된다.
  - Comparable 인터페이스는 compareTo()라는 메소드 하나만 선언되어 있다. 이 메소드는매개 변수로 넘어가는 객체와 현재 개체가 같은지
    비교하는 데 사용됨. 같으면 0, 현재 객체가 매개 변수보다 순서 상으로 앞에 있으면 -1, 뒤에 있으면 1을 리턴한다.
  - CharSequence 인터페이스는 해당 클래스가 문자열을 다루기 위한 클래스라는 것을 명시적으로 나타내는 데 사용

## String의 생성자
| 생성자                                                              | 설명                                                                       |
|------------------------------------------------------------------|--------------------------------------------------------------------------|
| String()                                                         | String 객체 생성. but, 이렇게 생성하는 것은 전혀 의미가 없어 `String name = null;`이 더 효율적이다. |
| String(byte[] bytes)                                             | 현재 사용중인 플랫폼의 캐릭터 셋을 사용하여 제공된 byte 배열을 디코딩한 String 객체를 생성한다.              |
| String(byte[] bytes, Charset charset)                            | 지정된 캐릭터 셋을 사용하여 제공된 byte 배열을 디코딩한 String 객체를 생성한다.                       |
| String(byte[] bytes, String charsetName)                         | 지정한 이름을 갖는 캐릭터 셋을 사용하여 지정한 byte 배열을 디코딩한 String 객체를 생성한다.                |
| String(byte[] bytes, int offset, int length)                     | 현재 사용중인 플랫폼의 기본 캐릭터 셋을 사용하여 지정한 byte 배열의 일부를 디코딩한 String 객체를 생성한다.       |
| String(byte[] bytes, int offset, int length, Charset charset)    | 지정된 캐릭터 셋을 사용하여 byte 배열의 일부를 디코딩한 String 객체를 생성한다.                       |
| String(byte[] bytes, int offset, int length, String charsetName) | 지정한 이름을 갖는 캐릭터 셋을 사용하여 byte 배열의 일부를 디코딩한 String 객체를 생성한다.                |
| String(char[] value)                                             | char 배열의 내용들을 붙여 String 객체를 생성한다.                                        |
| String(char[] value, int offset, int count)                      | char 배열의 일부 내용들을 붙여 String 객체를 생성한다.                                     |
| String(int[] codePoints, int offset, int count)                  | 유니코드 코드 위치로 구성되어 있는 배열의 일부를 새로운 String 객체로 생성한다.                         |
| String(String original)                                          | 매개 변수로 넘어온 String과 동일한 값을 갖는 String 객체를 생성한다. 다시 말해, 복제본 생성              |
| String(StringBuffer buffer)                                      | 매개 변수로 넘어온 StringBuffer 클래스에 정의되어 있는 문자열의 값과 동일한 String 객체를 생성한다.        |
| String(StringBuilder builder)                                    | 매개 변수로 넘어온 StringBuilder 클래스에 정의되어 있는 문자열의 값과 동일한 String 객체를 생성한다.       |

- 여기서 많이 사용되는 생성자는 String(byte[] bytes), String(byte[] bytes, String charset) 이다.
- 이 생성자들은 한글을 사용하는 우리나라에서는 자주 사용할 수밖에 없다. 대부분 언어에서는 문자열을 변환할 때 기본적으로 영어로 해석하려고 하기 때문이다.

## String 문자열을 byte로 변환하기
- getBytes() 메소드로 현재의 문자열 값을 byte 배열로 변환할 수 있다.

| 리턴 타입  | 메소드 이름 및 매개 변수               | 설명                              |
|--------|------------------------------|---------------------------------|
| byte[] | getBytes()                   | 기본 캐릭터 셋의 바이트 배열을 생성한다.         |
| byte[] | getBytes(Charset charset)    | 지정한 캐릭터 셋 객체 타입으로 바이트 배열을 생성한다. |
| byte[] | getBytes(String charsetName) | 지정한 이름의 캐릭터 셋을 갖는 바이트 배열을 생성한다. |

- 보통 캐릭터 셋을 잘 알고 있거나, 같은 프로그램 내에서 문자열을 byte로 만들 때에는 getBytes(), 다른 시스템에서 전달 받았을 경우에는 아래 것을 사용하면 좋다.
문자열이 다른 캐릭터 셋으로 되어 있을 수도 있기 때문이다.
- 캐릭터 셋. 자바뿐 아니라 다른 프로그래밍 언어를 써도 특수 문자를 표시할 일이 생긴다. 여기서 특수 문자는 알파벳을 제외한 나라의 문자를 의미. 한글 역시
고유의 캐릭터 셋을 가진다. 가끔 웹페이지 내의 한글이 깨지는 이유는 브라우저에서 생각하는 캐릭터 셋과 웹 페이지에 지정된 캐릭터 셋이 다르기 때문이다.
- java.nio.Charset 클래스 API에는 표준 캐릭터 셋이 정해져 있다. 

| 캐릭터 셋 이름   | 설명                                                      |
|------------|---------------------------------------------------------|
| US-ASCII   | 7비트 아스키                                                 |
| ISO-8859-1 | ISO 라틴 알파벳                                              |
| UTF-8      | 8비트 UCS 변환 포맷                                           |
| UTF-16BE   | 16비트 UCS 변환 포맷, big-endian 바이트 순서를 가진다.                 |
| UTF-16LE   | 16비트 UCS 변환 포맷, little-endian 바이트 순서를 가진다.              |
| UTF-16     | 16비트 UCS 변환 포맷, 바이트 순서는 byte-order mark 라는 것에 의해서 정해진다. |
| EUC-KR     | 8비트 문자 인코딩으로 EUC의 일종으로 대표적인 "한글 완성형" 인코딩                |
| MS949      | Microsoft에서 만든 "한글 확장 완성형" 인코딩                          |

- 한글 처리를 위해 사용하는 캐릭터 셋은 UTF-16이다. 예전에는 UTF-8, EUC-KR을 많이 썼지만, 요즘에는 대부분 UTF-16를 사용

```java
private void convert() {
    try {
        String korean = "한글";
        byte[] array1 = korean.getBytes();
        for (byte data : array1) {
            System.out.print(data + " ");
        }
        System.out.println();
        String korean2 = new String(array1);
        System.out.println(korean2);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
- 기본 캐릭터셋이 EUC-KR이라면
```text
-57 -47 -79 -37 
한글
```
- 기본 캐릭터셋이 UTF-8이라면 다음과 같은 결과가 나온다.
```text
-19 -107 -100 -22 -72 -128 
한글
```

```text
private void convertUTF16() {
    try {
        String korean = "한글";
        byte[] array1 = korean.getBytes("UTF-16");
        printByteArray(array1);
        String korean2 = new String(array1);
        System.out.println(korean2);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
```text
-2 -1 -43 92 -82 0 
���\� 
```
- 잘못된 캐릭터 셋으로 변환을 하면 다음과 같이 알아볼 수 없는 문자로 표시된다.
- 다음과 같이 변경해서 해결해보자.
```java
private void convertUTF16() {
    try {
        String korean = "한글";
        byte[] array1 = korean.getBytes("UTF-16");
        printByteArray(array1);
        String korean2 = new String(array1, "UTF-16");
        System.out.println(korean2);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
```text
-2 -1 -43 92 -82 0 
한글
```

### EUC-KR vs UTF-16
- 한글을 byte 배열로 만들 때 어떤 캐릭터 셋을 쓰느냐에 따라서 배열의 크기가 다르다.
- EUC-KR은 한글 두 글자가 4바이트, UTF-16은 6바이트를 사용한다. korean 값을 변경해 보면서 확인해보면 글자 수와 상관 없이, 무조건 2바이트의 차이가 발생한다.
- 자바에서 한글이 몇 바이트를 점유하는지 알아 두는 것은 중요하다.

### 위 코드에서 try-catch로 감싼 이유
1. byte 배열과 String 타입의 캐릭터 셋을 받는 생성자
2. getBytes() 메소드 중에서 String 타입의 캐릭터 셋을 받는 메소드
- 위 두가지는 UnsupportedEncodingException을 발생시킬 수 있다. 존재하지 않는 캐릭터 셋의 이름을 지정할 경우에는 이 예외가 발생하게 되므로, 반드시
try-catch 또는 throws 구문을 사용해야 한다.

## 객체의 널체크
- String 뿐만 아니라 모든 객체를 처리할 때에는 널 체크를 해야만 한다.
- 객체가 null 이라는 것은 객체가 아무런 초기화가 되어 있지 않으며, 클래스에 선언되어 있는 어떤 메소드도 사용할 수 없다는 것을 의미
- null인 객체의 메소드를 호출하는 순간 예외를 발생시킨다.
```java
private boolean nullCheck(String text) {
    int textLength = text.length();
    System.out.println(textLength);
    if (text == null) return true;
    else return false;
}
```
```text
Exception in thread "main" java.lang.NullPointerException
	at vol1._15.string.StringNull.nullCheck(StringNull.java:10)
	at vol1._15.string.StringNull.main(StringNull.java:6)
```
- null인 객체의 메소드에 접근하면 NullPointerException이 발생한다. null 체크를 해보자.
```java
private boolean nullCheck2(String text) {
    if (text == null) {
        return true;
    } else {
        int textLength = text.length();
        System.out.println(textLength);
        return false;
    }
}
```
- 이러한 null 체크는 매우 중요하다. 널 체크를 하지 않아 애플리케이션이 비정상으로 작동하여 장애로 이어질 수도 있다.
- 메소드의 매개 변수로 넘어오는 객체가 널이 될 확률이 조금이라도 있다면 반드시 한 번씩 확인하는 습관을 가지고 있어야 한다.

## String 비교, 검색 메소드
- 문자열 길이를 확인하는 메소드
- 문자열이 비어 있는지 확인하는 메소드
- 문자열이 같은지 비교하는 메소드
- 특정 조건에 맞는 문자열이 있는지를 확인하는 메소드

### 문자열 길이를 확인하는 메소드
- `int length()` : 문자열의 길이를 리턴한다.

### 문자열이 비어 있는지 확인하는 메소드
- `boolean isEmpty()` : 문자열이 비어 있는지를 확인한다. 비어 있으면 true를 리턴

### 문자열이 같은지 비교하는 메소드
1. boolean equals(Object anObject)
2. boolean equalsIgnoreCase(String anotherStr)
3. int compareTo(String anotherStr)
4. int compareToIgnoreCase(String str)
5. boolean contentEquals(CharSequence cs)
6. boolean contentEquals(StringBuffer sb)

- equals, compareTo, contentEquals 로 시작하는 3가지로 분류할 수 있다. 이름은 상이하지만, 매개 변수로 넘어온 값과 String 객체가 같은지를 비교하기 위한 메소드이다.

```java
private void checkCompare() {
    String text = "Check value";
    String text2 = "Check value";
    if (text == text2) {
        System.out.println("text == text2 result is same.");
    } else {
        System.out.println("text == text2 result is different.");
    }
    if (text.equals("Check value")) {
        System.out.println("text.equals(text2) result is same.");
    }
}
```
```text
text == text2 result is same.
text.equals(text2) result is same.
```
- 기본적으로 객체는 == 이 아닌 equals() 메소드를 사용해서 비교해야만 하는데, 위와 같이 둘다 같다고 나오는 이유는 자바에는 Constant Pool 이라는 것이 존재하기 때문이다.
- 자바에서는 객체들을 재사용하기 위해서 Constant Pool이 있고, String의 경우 동일한 값을 갖는 객체가 있으면, 이미 만든 객체를 재사용한다. 따라서 text, text2는 같은 객체다.
- 그렇다면 new를 사용해 새로 할당하면 다음과 같다.
```java
// String text2 = "Check value";
String text2 = new String("Check value");
```
```text
text == text2 result is different.
text.equals(text2) result is same.
```
- new를 사용해 객체를 생성하면 값이 같은 String 객체를 생성한다고 하더라도 Constant Pool의 값을 재활용하지 않고 별도의 객체를 생성한다.
- 추가로, equalsIgnoreCase()는 대소문자를 구분하지 않고, 같은지 다른지만 확인한다.

- 다음으로 CompareTO()
```java
public void checkCompareTo() {
    String text = "a";
    String text2 = "b";
    String text3 = "c";
    System.out.println(text2.compareTo(text));
    System.out.println(text2.compareTo(text3));
    System.out.println(text.compareTo(text3));
}
```
```text
1
-1
-2
```
- "b"가 a보다 뒤에 있으므로 1 양수 리턴. 나머지 "b", "c"는 -1 , "a", "c"는 -2, 음수를 리턴한 것을 볼 수 있다.
- compareToIgnoreCase() 메소드는 대소문자 구분을 하지 않고, compareTo() 메소드를 수행하는 것과 같다.
- 마지막으로 contentEquals() 메소드는 매개 변수로 넘어오는 CharSequence와 StringBuffer 객체가 String 객체와 같은지를 비교하는 데 사용된다.

### 특정 조건에 맞는 문자열이 있는지를 확인하는 메소드
1. boolean startsWith(String prefix)
2. boolean startsWith(String prefix, int toffset)
3. boolean endWith(String suffix)
4. boolean contains(CharSequence s)
5. boolean matches(String regex)
6. boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)
7. boolean regionMatches(int toffset, String other, int ooffset, int len)

- 이 중에서 startsWith() 메소드를 가장 많이 사용한다. 이름 그대로 매개 변수로 넘어온 값으로 시작하는 지 확인.
- endWith()는 말 그대로 매개 변수로 넘어온 값으로 끝나는 지 확인.
```java
package vol1._15.string;

public class StringCheck {
	public static void main(String[] args) {
		StringCheck sample = new StringCheck();
		String[] addresses = new String[] {
			"서울시 구로구 신도림동",
			"경기도 성남시 분당구 정자동 개발공장",
			"서울시 구로구 개봉동"
		};
		sample.checkAddress(addresses);
	}

	private void checkAddress(String[] addresses) {
		int startCount = 0, endCount = 0;
		String startText = "서울시";
		String endText = "동";
		for (String address : addresses) {
			if (address.startsWith(startText)) {
				startCount++;
			}
			if (address.endsWith(endText)) {
				endCount++;
			}
		}
		System.out.println(startText +  " startCount = " + startCount);
		System.out.println(endText + " endCount = " + endCount);
	}
}
```
```text
서울시 startCount = 2
동 endCount = 2
```
- 중간에 있는 값은 contains() 메소드로 확인한다.
```text
public void containsAddress(String[] addresses) {
    int containsCount = 0;
    String containText = "구로";
    for (String address : addresses) {
        if (address.contains(containText)) {
            containsCount++;
        }
    }
    System.out.println("Contains " + containText + " count is " + containsCount);
}
```
```text
Contains 구로 count is 2
```
- 다음으로 regionMatches() 메소드는 문자열 중에서 특정 영역이 매개 변수로 넘어온 문자열과 동일한지를 확인하는 데 사용된다.
1. boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)
2. boolean regionMatches(int toffset, String other, int ooffset, int len)

- 매개 변수는 다음과 같다.
1. ignoreCase : true 일 경우 대소문자 구분을 하지 않고, 값을 비교한다.
2. toffset : 비교 대상 문자열의 확인 시작 위치를 지정한다.
3. other : 존재하는지를 확인할 문자열을 의미한다.
4. ooffset : other 객체의 확인 시작 위치를 지정한다.
5. len : 비교할 char의 개수를 지정한다.

```java
public void checkMatch() {
    String text = "This is a text";
    String compare1 = "is";
    String compare2 = "this";
    System.out.println(text.regionMatches(2, compare1, 0, 1));
    System.out.println(text.regionMatches(5, compare1, 0, 2));
    System.out.println(text.regionMatches(true, 0, compare2, 0, 4));
}
```
```text
true
true
true
```
- text.regionMatches(2, compare1, 0, 1)
  - 먼저 text 문장에 "is"는 2, 5번째에 위치한다.
  - 첫 번째 매개 변수인 2라는 위치 값은 제대로 맞았다. 그런데, 세 번째 매개 변수가 0, 네 번째 매개 변수가 1이기 때문에 비교하려는 것은 "i"인지만 확인하면 된다.
- regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)
- regionMatches() 메소드는 잘못 사용하기 쉽다. 다음과 같은 경우에는 항상 false이다.
  - toffset이 음수일 때
  - ooffset이 음수일 때
  - toffset + len 이 비교 대상의 길이보다 클 때
  - ooffset + len이 other 객체의 길이보다 클 때
  - ignoreCase가 false인 경우에는 비교 범위의 문자들 중 같은 위치(index)에 있는 char가 다를 때
  - ignoreCase가 true인 경우에는 비교 범위의 문자들을 모두 소문자로 변경한 후 같은 위치(index)에 있는 char가 달라야 한다.

## String 내에서 위치 찾아내기
- 리턴 타입은 모두 int이다.
1. indexOf(int ch)
2. indexOf(int ch, int fromIndex)
3. indexOf(String str)
4. indexOf(String str, int fromIndex)
5. lastIndexOf(int ch)
6. lastIndexOf(int ch, int fromIndex)
7. lastIndexOf(String str)
8. lastIndexOf(String str, int fromIndex)
- indexOf()는 앞에서부터 (가장 왼쪽부터) 문자열이나 char를 찾고,
- lastIndexOf()는 뒤에서부터 (가장 오른쪽부터) 찾는다.
- int를 넘겨주는 것은 char를 넘겨주면 자동으로 형 변환이 일어난다.

```java
public void checkIndexOf() {
    String text = "Java technology is both a programming language and a platform";
    System.out.println(text.indexOf('a'));
    System.out.println(text.indexOf("a "));
    System.out.println(text.indexOf('a', 20));
    System.out.println(text.indexOf("a ", 20));
    System.out.println(text.indexOf('z'));
}
```
```text
1
3
24
24
-1
```
```text
public void checkLastIndexOf() {
    String text = "Java technology is both a programming language and a platform";
    System.out.println(text.lastIndexOf('a'));
    System.out.println(text.lastIndexOf("a "));
    System.out.println(text.lastIndexOf('a', 20));
    System.out.println(text.lastIndexOf("a ", 20));
    System.out.println(text.lastIndexOf('z'));
}
```
```text
55
51
3
3
-1
```
- lastIndexOf() 메소드의 검색 시작 위치는 왼쪽부터의 위치를 말한다.

## String의 값의 일부를 추출하기 위한 메소드
| 리턴 타입 | 메소드 이름 및 매개 변수                                               | 설명                                                                                  |
|-------|--------------------------------------------------------------|-------------------------------------------------------------------------------------|
| char  | charAt(int index)                                            | 특정 위치의 char 값을 리턴                                                                   |
| void  | getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin) | 매개 변수로 넘어온 dst라는 char 배열 내에 srcBegin 에서 srcEnd에 있는 char를 저장. dst 배열의 시작위치는 dstBegin |
| int   | codePointAt(int index)                                       | 특정 위치의 유니 코드 값을 리턴. 리턴 타입은 int지만, 이 값을 char로 형 변환하면 char 값을 출력가능.                   |
| int   | codePointBefore(int index)                                   | 특정 위치 앞에 있는 char의 유니코드 값을 리턴. 리턴 타입은 int지만, 이 값을 char로 형 변환하면 char 값을 출력가능.         |
| int   | codePointCount(int beginIndex, int endIndex)                 | 지정한 범위에 있는 유니코드 개수를 리턴                                                              |
| int   | offsetByCodePoints(int index, int codePointOffset)           | 지정된 index 부터 오프셋(offset)이 설정된 인덱스를 리턴                                               |
- offsetByCodePoints()는 문자열 인코딩과 관련된 문제를 해결하기 위해 사용됨

### char 배열의 값을 String으로 변환하는 메소드
| 리턴 타입         | 메소드 이름 및 매개 변수                                  | 설명                                                              |
|---------------|-------------------------------------------------|-----------------------------------------------------------------|
| static String | copyValueOf(char[] data)                        | char 배열에 있는 값을 문자열로 변환한다.                                       |
| static String | copyValueOf(char[] data, int offset, int count) | char 배열에 있는 값을 문자열로 변환한다. 단, offset 위치부터 count까지의 개수만큼만 문자열로 변환 |

```java
char values[] = new char[] {'J','a','v','a'};
String javaText = String.copyValueOf(values):
```

### String의 값을 char 배열로 변환하는 메소드
| 리턴 타입  | 메소드 이름 및 매개 변수 | 설명                     |
|--------|----------------|------------------------|
| char[] | toCharArray()  | 문자열을 char 배열로 변환하는 메소드 |

### 문자열의 일부 값을 잘라내는 메소드
| 리턴 타입        | 메소드 이름 및 매개 변수                            | 설명                                                      |
|--------------|-------------------------------------------|---------------------------------------------------------|
| String       | substring(int beginIndex)                 | beginIndex부터 끝까지 대상 문자열을 잘라 String으로 리턴.                |
| String       | substring(int beginIndex, int endIndex)   | beginIndex부터 endIndex까지 대상 문자열을 잘라 String으로 리턴          |
| CharSequence | subSequence(int beginIndex, int endIndex) | beginIndex부터 endIndex까지 대상 문자열을 잘라 CharSequence 타입으로 리턴 |

```java
public void checkSubstring() {
    String text = "Java technology";
    String technology = text.substring(5);
    System.out.println(technology);
    String tech = text.substring(5, 9);
    System.out.println(tech);
}
```
```text
technology
```
- text.substring(5); 의 경우 5번째부터 text 문자열의 끝까지 잘라낸다.
- String tech = text.substring(5, 9); 와 같이, 끝나는 위치 또한 지정해 줄 수 있다.

### 문자열을 여러 개의 String 배열로 나누는 split 메소드
| 리턴 타입    | 메소드 이름 및 매개 변수                 | 설명                                                                                |
|----------|--------------------------------|-----------------------------------------------------------------------------------|
| String[] | split(String regex)            | regex에 있는 정규 표현식에 맞추어 문자열을 잘라 String의 배열로 리턴                                      |
| String[] | split(String regex, int limit) | regex에 있는 정규 표현식에 맞추어 문자열을 잘라 String의 배열로 리턴. 이 때 String 배열의 크기는 limit보다 커서는 안된다. |
- 자바에서 문자열을 여러 개의 문자열의 배열로 나누는 방법은 split() 메소드와 java.util.StringTokenizer 라는 클래스를 사용하는 것이다.
- 만약 정규 표현식을 사용하여 문자열을 나누려고 한다면 split(), 그냥 특정 String 으로 문자열을 나누려고 한다면 StringTokenizer 클래스를 사용하는 것이 편하다.
- 특정 알파벳, 기호로 나누는 것은 둘 다 상관없다.
```java
public void checkSplit() {
    String text = "Java technology is both a programming language and a platform";
    String[] splitArr = text.split(" ");
    for (String str : splitArr) {
        System.out.println(str);
    }
}
```
```text
Java
technology
is
both
a
programming
language
and
a
platform
```

## String의 값을 바꾸는 메소드
- 문자열을 합치는 메소드와 공백을 없애는 메소드
- 내용을 교체(replace)하는 메소드
- 특정 형식에 맞춰 값을 치환하는 메소드
- 대소문자를 바꾸는 메소드
- 기본 자료형을 문자열로 변환하는 메소드

### 문자열을 합치는 메소드와 공백을 없애는 메소드
| 리턴 타입  | 메소드 이름 및 매개 변수     | 설명                                                |
|--------|--------------------|---------------------------------------------------|
| String | concat(String str) | 매개 변수로 받은 str을 기존 문자열의 우측에 붙인 새로운 문자열 객체를 생성하여 리턴 |
| String | trim()             | 문자열의 맨 앞과 맨 뒤에 있는 공백들을 제거한 문자열 객체를 리턴             |

- concat() 보다는 보통 StringBuffer, StringBuilder 클래스를 사용하여 문자열을 더한다.
- trim() 메소드는 공백을 제거할 때 매우 유용하게 사용된다.

```java
public void checkTrim() {
    String[] strings = new String[] {
        " a", "b ", "       c", "d     ", "e    f", "    "
    };
    for (String string : strings) {
        System.out.println("["+ string + "]");
        System.out.println("["+ string.trim() + "]");
    }
}
```
```text
[ a]
[a]
[b ]
[b]
[       c]
[c]
[d     ]
[d]
[e    f]
[e    f]
[    ]
[]
```
- trim()의 용도는 매우 많지만 작업하려는 문자열이 공백만으로 이루어진 값인지, 아니면 공백을 제외한 값이 있는지 확인하기에 매우 편리하다.
```java
String text = " a ";
if (text !=null) {
    System.out.println("OK");
}
```
- 위와 같이 if문을 통과하면 해당 문자열은 공백을 제외한 char 값이 하나라도 존재한다는 의미이다.
- 만약 null 체크를 하지 않고, 값이 null인 객체의 메소드를 호출하면 NullPointerException이 발생한다. 따라서 이와 같이 String을 조작하기 전에 
null을 체크하는 습관을 갖자.

### 내용을 교체(replace)하는 메소드
| 리턴 타입  | 메소드 이름 및 매개 변수                                         | 설명                                                              |
|--------|--------------------------------------------------------|-----------------------------------------------------------------|
| String | replace(char oldChar, char newChar)                    | 해당 문자열에 있는 oldChar 값을 newChar로 대치한다.                            |
| String | replace(CharSequence target, CharSequence replacement) | 해당 문자열에 있는 target과 같은 값을 replacement로 대치한다.                     |
| String | replaceAll(String regex, String replacement)           | 해당 문자열의 내용 중 regex에 표현된 정규 표현식에 포함되는 모든 내용을 replacement로 대치한다.  |
| String | replaceFirst(String regex, String replacement)         | 해당 문자열의 내용 중 regex에 표현된 정규 표현식에 포함되는 첫번째 내용을 replacement로 대치한다. |

```java
public void checkReplace() {
    String text = "The String class represents character strings.";
    System.out.println(text.replace('s', 'z'));
    System.out.println(text);
    System.out.println(text.replace("tring", "trike"));
    System.out.println(text.replaceAll(" ", "|"));
    System.out.println(text.replaceFirst(" ", "|"));
}
```
```text
The String clazz reprezentz character ztringz.
The String class represents character strings.
The Strike class represents character strikes.
The|String|class|represents|character|strings.
The|String class represents character strings.
```
- replace(), replaceFirst()의 경우 가장 첫번째 나타는 target(oldChar)만 바꾸고, replaceAll()은 모두 변경한다.

### 특정 형식에 맞춰 값을 치환하는 메소드
| 리턴 타입         | 메소드 이름 및 매개 변수                                  | 설명                                                                                      |
|---------------|-------------------------------------------------|-----------------------------------------------------------------------------------------|
| static String | format(String format, Object... args)           | format에 있는 문자열의 내용 중 변환해야 하는 부분을 args의 내용으로 변경                                          |
| static String | format(Locale l, String format, Object... args) | format에 있는 문자열의 내용 중 변환해야 하는 부분을 args의 내용으로 변경. 단 첫 매개 변수인 Locale 타입의 l에 선언된 지역에 맞추어 출력 |
- Locale은 지역적으로 다른 표현 형식을 제공하기 위한 것이다. 보통 Locale을 지정하지 않으면 기본적으로 자바 프로그램이 수행되는 OS의 지역 정보를 기본으로 따른다.
- %s는 String, %d는 정수형, %f는 소수점이 있는 숫자, %%는 %%를 의미.
```java
public void checkFormat() {
    String text = "제 이름은 %s입니다. 지금까지 %d권의 책을 썼고, 하루에 %f %%의 시간을 책을 쓰는데 할애하고 있습니다.";
    String realText = String.format(text, "이상민", 7, 10.5);
    System.out.println(realText);
}
```
```text
제 이름은 이상민입니다. 지금까지 7권의 책을 썼고, 하루에 10.500000 %의 시간을 책을 쓰는데 할애하고 있습니다.
```
- System.out.printf()를 활용해 바로 출력할 수 있다. 
- 주의할 점은 대치해야 할 문자열보다 적게 나열하면 MissingFormatArgumentException이 발생한다.

### 대소문자를 바꾸는 메소드
| 리턴 타입  | 메소드 이름 및 매개 변수             | 설명                                 |
|--------|----------------------------|------------------------------------|
| String | toLowerCase()              | 모든 문자열의 내용을 소문자로 변경                |
| String | toLowerCase(Locale locale) | 지정한 지역 정보에 맞추어 모든 문자열의 내용을 소문자로 변경 |
| String | toUpperCase()              | 모든 문자열의 내용을 대문자로 변경                |
| String | toUpperCase(Locale locale) | 지정한 지역 정보에 맞추어 모든 문자열의 내용을 대문자로 변경 |

### 기본 자료형을 문자열로 변환하는 메소드
| 리턴 타입         | 메소드 이름 및 매개 변수                              |
|---------------|---------------------------------------------|
| static String | valueOf(boolean b)                          |
| static String | valueOf(char c)                             |
| static String | valueOf(char[] data)                        |
| static String | valueOf(char[] data, int offset, int count) |
| static String | valueOf(double d)                           |
| static String | valueOf(float f)                            |
| static String | valueOf(int i)                              |
| static String | valueOf(long l)                             |
| static String | valueOf(Object obj)                         |

- 이 valueOf()를 사용하여 문자열로 변경해도 되지만 다음과 같이 변환해도 된다.
```java
byte b = 1;
String byte1 = String.valueOf(b);
String byte2 = b + "";
```
- 대부분 기본 자료형을 String으로 변환할 때에는 String과 합치는 과정을 거친다. 그럴 경우에는 별도로 valueOf() 메소드를 사용할 필요까지는 없다.
- 하지만 String으로 변환만 해놓고 별도의 문자열과 합치는 과정이 없을 경우에는 valueOf() 메소드 사용을 권장한다.
- valueOf() 매개 변수로 객체(Object)가 넘어 왔을 경우, toString()을 구현한 객체나 정상적인 객체를 valueOf() 메소드에 넘겨주면 toString()의
결과를 리턴해준다. 하지만 null의 경우 toString()을 사용할 수 없다. (NPE 발생). 그러한 경우를 방지하기 위해 객체를 출력할 때 valueOf() 메소드를
사용하면 좋은데, 객체가 null이면 "null"이라는 문자열을 리턴해준다. 만약 null이 아니면 toString() 메소드를 호출한 결과가 리턴된다.

## 절대로 사용하면 안 되는 intern() 메소드
- C로 구현된 native 메소드인데, 시스템의 심각한 성능 저하를 발생시킬 수도 있다.
```java
public void internCheck() {
    String text1 = "Java Basic";
    String text2 = "Java Basic";
    String text3 = new String("Java Basic");
    System.out.println(text1 == text2);
    System.out.println(text1 == text3);
    System.out.println(text1.equals(text3));
}
```
```text
true
false
true
```
- 위 결과에서 intern() 메소드를 중간에 추가하면
```java
public void internCheck() {
    String text1 = "Java Basic";
    String text2 = "Java Basic";
    String text3 = new String("Java Basic");
    text3 = text3.intern();
    System.out.println(text1 == text2);
    System.out.println(text1 == text3);
    System.out.println(text1.equals(text3));
}
```
```text
true
true
true
```
- new String()으로 생성한 문자열 객체여도 풀에 해당 값이 있으면, 풀에 있는 값을 참조하는 객체를 리턴한다. 만약 동일한 문자열이 존재하지 않으면
풀에 해당 값을 추가한다. 따라서 equals() 말고 ==으로도 동일한지 비교할 수가 있다.
- equals()와 ==으로 비교하는 것의 성능 차이는 많다. 그런데 왜 사용하면 안 될까?
- 새로운 문자열을 쉴새 없이 만드는 프로그램에서 intern() 메소드를 사용하여 억지로 문자열 풀에 값을 할당하도록 만들면, 저장되는 영역은 한계가 있기
때문에 그 영역에 대해서 별도로 메모리를 청소하는 단계를 거치게 된다. 따라서 작은 연산 하나를 빠르게 하기 위해 전체 자바 시스템의 성능에 악영향을 주게 된다.

## immutable한 String의 단점을 보완하는 클래스에는 StringBuffer와 StringBuilder가 있다.
- String은 immutable한 객체다. 한 번 만들어지면 더 이상 그 값을 바꿀 수 없다.
- String 문자열을 더하면 새로운 String 객체가 생성되고, 기존 객체는 버려진다. 그러므로, 계속 더하는 작업을 한다면 계속 쓰레기를 만들게 된다.
- 원래 있던 객체는 더 이상 사용할 수 없고 GC(가비지 컬렉션)의 대상이 된다.
- 이러한 단점을 보완하기 위해서 나온 클래스가 StringBuffer, StringBuilder이다.
- StringBuffer는 Thread safe하고 StringBuilder는 그렇지 않다. 속도는 StringBuilder가 더 빠르다.
- 이 둘 모두 append()를 사용해 문자열을 더한다.
- 세미콜론이 나오기 전에 계속 append()를 붙여도 상관 없다.
- 추가로 JDK 5 이상에서는 String의 더하기 연산을 할 경우, 컴파일할 떄 자동으로 해당 연산을 StringBuilder로 변환해준다. 따라서
일일이 더하는 작업을 변환해 줄 필요는 없지만 for루프와 같은 반복 연산에서는 자동으로 변환을 해 주지 않아, 꼭 필요하다.
- 이 두 클래스의 공통점은 모두 문자열을 다루고, CharSequence 인터페이스를 구현했다는 점이다. 따라서 매개 변수로 받는 작업을 할 떄, String, StringBuilder 타입보다
CharSequence 타입으로 받는 것이 좋다.
- 그러면 언제 StringBuilder, StringBuffer를 사용해야 할까?
  - 일반적으로 하나의 메소드 내에서 문자열을 생성하여 더할 경우에는 StringBuilder를 사용해도 전혀 상관없다.
  - 어떤 클래스에 문자열을 생성하여 더하기 위한 문자열을 처리하기 위한 인스턴스 변수가 선언되었고, 여러 쓰레드에서 이 변수를 동시에 접근하는 일이 있을 경우에는
    반드시 StringBuffer를 사용해야 한다.

