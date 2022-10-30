### ☑️**Java8 새로운 것들**

- Lomda 표현식
- Functional 인터페이스
- Stream
- Optional
- 인터페이스의 기본 메서드(Default method)
- 날짜 관련 클래스들 추가
- 병렬 배열 정렬
- StringJoiner 추가.

---

### ☑️Opitional

> optional이라는 단어는 “선택적인”이라는 의미다.
> 
- Optional은 Funtional 언어인 Haskell과 Scala에서 제공하는 기능을 따 온 것이다.
    - 객체를 편리하게 처리하기 위해서 만든 클래스라고 보면 된다.
    
    ```java
    public final class Optional<T> extends Object 
    ```
    

final 변수는 변경 불가능하지만, final 클래스로 선언했다고 해서 내용 변경이 불가능한 것은 아니다. 

대신 추가적인 확장이 불가능하다. 즉, 자식 클래스를 만들 수 없다는 의미다. 

Optinal 클래스는 하나의 깡통이라고 생각하면 된다. 

- Optional 클래스는 객체를 생성하지 않는다.

Optional 클래스는 null 처리를 보다 간편하게 하기 위해서 만들어졌다. 

Optional 클래스에 값을 잘못 넣으면 NoSuchElementException이 발생할 수도 있다. 

---

### ☑️Default method

: Java8부터는 default 메소드라는 것이 추가되었다. 

- abstract 클래스의 경우 extends를 사용해야 하지만, default를 사용한 인터페이스는 implements 키워드를 사용하여 구현하면 된다.

> **default 메소드를 왜 만들었을까?**
> 
- 하위 호환성 때문이다.
    - 오픈소스를 만들었다고 가정해보자.
        - 그 오픈 소스가 유명해져서 전 세계 사람들이 다 사용하고 있는데, 인터페이스에 메소드를 새로 만들어야 하는 상황이 발생.
            - 이 때 사용하는 것이 default 메소드이다.

---

### ☑️날짜 관련 클래스

Java8 이전에는 Date나 SimpleDateFormatter라는 클래스를 사용하여 날짜를 처리해 왔다. 

- 이 클래스들은 스레드에 안전하지 않다.
- 불변 객체가 아니어서 지속적으로 값이 변경 가능하다.

기존 클래스와 신규 클래스의 차이 

| 내용 | 버전 | 패키지 | 설명 |
| --- | --- | --- | --- |
| 값 유지 | 예전 버전 | java.util.Date java.util.Calendar | Date 클래스는 날짜 계산을 할 수 없다. Calendar 클래스는 불변 객체가 아니므로 연산시 객체 자체가 변경되었다.  |
|  | Java 8 | java.time.ZonedDateTime java.time.LocalDate 등.  | ZonedDateTime과 LocalDate 등은 불변 객체이다. <br> 모든 클래스가 연산용의 메소드를 갖고 있으며, 연산시 새로운 불변 객체를 돌려 준다. <br> 그리고 스레드에 안전하다. |
| 변경 | 예전 버전 | java.text.SimpleDateFormat | SimpleDateFormat는 스레드 안전하지도 않고 느리다. |
|  | Java 8 | java.util.format.DateTimeFormatter | DateTimeFormatter 는 스레드에 안전하며 빠르다. |
| 시간대 | 예전 버전 | java.util.Timezone | “Asia/seoul”이나 “+09 : 00”같은 정보를 가진다.  |
|  | Java 8 | java.time.ZoneId java.time.ZonOffset | ZoneId 는 “Asia/Seoul”라는 정보를 갖고 있고, ZoneOffset는 “+09:00” 라는 정보를 가지고 있다.  |
| 속성 관련 | 예전 버전 | java.util.Canlendar | Calendar.YEAR Calendar.MONTH Calendar.DATE(또는 Calendar.DAY_OF_MONTH) 등 이들은 정수(int)이다. |
|  | Java 8 | java.time.temporal.ChronoField java.time.temporal.ChronoUnit | ChronoField.YEAR ChronoField.MONTH_OF_YEAR ChronoField.DAY_OF_MONTH 등이 enum 타입이다.

ChronoUnit.YEARS (연수)
ChronoUnit.MONTHS(개월)
ChronoUnit.DAY(일) 등이 enum 타입이다.  |

<br>

추가로 시간을 나타내는 클래스는 Local, Offset, Zoned 로 3가지 종류가 존재한다. 

- Local : 시간대가 없는 시간, 예를 들어 “1시”는 어느 지역의 1시인지 구분되지 않는다.
- Offset : UTC와의 오프셋(차이)를 가지는 시간. 한국은 “+09:00”
- Zoned : 시간대 (”한국 시간”과 같은 정보)를 갖는 시간, 한국은 경우의 “Asia/Seoul”

 주의사항 : Local과 Locale는 다른 클래스다. Locale은 지역을 의미하는 클래스이며, Local는 시간을 이야기 하는 것이다.

**누군가가 SNS에 클을 올리면 , 그 글이 저장되는 시간은 언제로 표시되어야 할까?**

 - 그 글이 저장되는 시간은 글쓴이의 Locale(지역) 정보와 함께 저장되어야 한다. 그렇지 않으면 24시간이 다른 전 세계의 시간이 모두 꼬이게 될 것이다.  그렇지 않으면 24시간이 다른 전 세계의 시간이 모두 꼬이게 될 것이다. 

---

### ☑️병렬 배열 정렬 (Parallel array sorting)

: 자바를 사용하면서, 배열을 정렬하는 가장 간편한 방법은 java.util 패키지의 Arrays 클래스를 사용하는 것이다. 

이 Arrays 클래스에는 다음과 같은 static 메소드들이 존재한다.

- binarySearch() : 배열 내에서의 검색
- copyOf() : 배열의 복제
- equals() : 배열의 비교
- fill() : 배열 채우기
- hashCode9) : 배열의 해시코드 제공
- sort() : 정렬
- toString() : 배열 내용을 출력.

Java8 에서는 parallelSort() 라는 정렬 메소드가 제공되며, Java 7에서 소개된 Fork-Join 프레임웤이 내부적으로 사용된다. 

> sort()의 경우 단일 스레드를 수행되며, parallelSort() 는 필요에 따라 여러 개의 스레드로 나뉘어 작업이 수행된다. 
따라서 parallelSort()가 CPU를 더 많이 사용하게 되겠지만 처리 속도는 더 빠르다.
> 

---

### ☑️StringJoiner

- 앞서 문자열을 처리하는 방법에 Stirng, StringBuilder, StringBuffer 등이 있다.
- 또한 문자열을 예쁘게 처리하기 위한 java.util 패키지의 Formatter 라는 클래스도 있다.
    
    

> 이번에 Java 8에서는 StringJoiner라는 클래스가 새롭게 추가되었다. 이 클래스는 java.util에 포함되어 있으며
순차적으로 나열되는 문자열을 처리할 때 사용한다.
>
