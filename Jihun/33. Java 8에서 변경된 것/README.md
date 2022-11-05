# Lambda 표현식(expression)
- 람다는 익명 클래스의 떨어지는 가독성을 보완하기 위해 만들어졌다. 대신, 이 표현식은 인터페이스에 메소드가 "하나"인 것들만 적용 가능하다. 그래서, 람다 표현식와 익명 클래스는 서로 전환 가능하다.
- Java에 있는 인터페이스 중, 메소드가 하나인 인터페이스는 다음과 같다.
  - java.lang.Runnable
  - java.util.Comparator
  - java.io.FileFilter
  - java.util.concurrent.Callable
  - java.security.PrivilegedAction
  - java.nio.file.PathMatcher
  - java.lang.reflect.InvocationHandler
- 가장 많이 사용되는 쓰레드를 처리하기 위한 Runnable이 있고, 값을 비교하기 위한 Comparator도 여기에 속한다. 물론, 이 인터페이스들만 존재하는 것은 아니고, 직접 만들어서 사용해도 된다.
- 다음의 기본 표현식을 보자.
```text
(int x, int y) -> x + y;
() -> 43;
(String s) -> { System.out.println(s); }
```
- 기본 람다 표현식은 3 부분으로 구성되어 있다.

| 매개 변수 목록        | 화살표 토큰(Arrow Token) | 처리 식  |
|-----------------|---------------------|-------|
| (int x, int y)) | ->                  | x + y |

- 좌측에는 넘겨지는 매개 변수들의 타입이 선언되고, 중간에는 화살표 연산자, 가장 우측에는 리턴되는 값을 표시. 즉, x와 y 값을 받아서 x + y를 리턴해 준다는 의미.
```java
package vol2._33.lambdaex;

public interface Calculate {
	int operation(int a, int b);
}
```
- 이렇게 Calculate라는 인터페이스가 있고, operation() 메소드가 선언되어 있다. 대신 a와 b로 무슨 작업을 하는지는 선언되어 있지 않다. 이 인터페이스를 익명 클래스로 구현하면 다음과 같다.
```java
package vol2._33.lambdaex;

public class CalculateSample {
	public static void main(String[] args) {
		CalculateSample sample = new CalculateSample();
		sample.calculateClassic();
	}

	private void calculateClassic() {
		Calculate calculateAdd = new Calculate() {
			@Override
			public int operation(int a, int b) {
				return a + b;
			}
		};

		System.out.println(calculateAdd.operation(1, 2));
	}
}
```
```text
3
```
- operation() 메소드에서는 간단하게 a와 b를 더하고 리턴해 준다.
- 이 calculateAdd라는 익명 클래스 객체를 람다 표현식으로 처리하려면 아래와 같다.
```text
private void calculateLambda() {
    Calculate calculate = (a, b) -> a + b;
    System.out.println(calculate.operation(1, 2));
}
```
```text
3
```
- 코드가 엄청 간단해진 것을 볼 수 있다. 메소드의 첫 번째 줄을 보면,
```text
(a, b) -> a + b;
```
- 라고 되어 있다. 이 메소드 내에는 a와 b라는 변수가 전혀 선언되어 있지도 않는데, 이렇게 a와 b를 사용하고 그 값을 더하기까지 하고 있다. 
- Calculate라는 인터페이스는 메소드가 하나만 선언되어 있기 때문에, (a, b)라고 되어 있는 부분은 operation() 메소드의 int a와 int b를 매개 변수로 받는 다는 의미다.
- 그리고 -> 옆에 a + b는 결과로 a와 b의 합을 리턴한다는 의미다. 따라서 이 메소드를 수행하면 3이라는 결과가 출력된다.
- 여기서 a, b처럼 변수 이름은 임의로 선언해도 전혀 문제 없다.
- 뺄셈을 처리하는 람다 표현식은 아래와 같이 작성할 수 있다.
```java
private void calculateLambda() {
    Calculate calculateAdd = (a, b) -> a + b;
    System.out.println(calculateAdd.operation(1, 2));
    Calculate calculateSubtract = (a, b) -> a - b;
    System.out.println(calculateSubtract.operation(1, 2));
}
```
- a 에서 b를 빼는 람다 표현식을 구현하였다.
- 다시 Calculate 인터페이스를 보자.
```java
public interface Calculate {
    int operation(int a, int b);
}
```
- 일반적인 인터페이스이지만, 이 인터페이스는 Functional(기능적) 인터페이스라고 부를 수 있다. 기능적 인터페이스는 이와 같이 하나의 메소드만 선언되어 있는 것을 의미.
- 그런데 이렇게만 선언해 두면 매우 혼동될 수도 있다. 왜냐하면, 같이 개발하는 다른 사람이 이 인터페이스의 선언이 모호하다며 operationAdd()와 operationSubtract() 메소드로 구분하여
두 개의 메소드를 선언할 수도 있기 때문이다.

```java
public interface Calculate {
    int operationAdd(int a, int b);
    int operationSubtract(int a, int b);
}
```
- 만약 이렇게 된다면 람다 표현식을 사용할 수 없고, 람다 표현식에 컴파일 오류가 발생한다. (`error: incompatible types: Calculate is not a functional interface`)
- 이러한 혼동을 피하기 위하여, 인터페이스 선언시 어노테이션을 사용할 수 있다.
```java
@FunctionalInterface
public interface Calculate {
	int operation(int a, int b);
}
```
- 명시적으로 이렇게 @FunctionalInterface 를 사용하면 이 인터페이스에는 내용이 없는 "하나"의 메소드만 선언할 수 있다. 만약 두 개의 메소드를 선언한다면 컴파일 오류가 발생.
  (`error: Unexpected @FunctionalInterface annotation`)
- 메소드가 두 개 선언되어 있기 떄문에 Functional 인터페이스가 아닌데 왜 어노테이션을 사용했냐고 이와 같이 컴파일 에러가 발생한다.
- 우리가 앞서 본 인터페이스 중에 쓰레드를 처리하기 위한 Runnable 인터페이스를 살펴보자.
```java
private void runCommonThread() {
    Runnable runnable = new Runnable() {
        @Override
        public void run() {
            System.out.println(Thread.currentThread().getName());
        }
    };

    new Thread(runnable).start();
}
```
- 익명 클래스로 Runnable 의 run() 메소드를 이와 같이 구현할 수 있다. Runnable은 run() 메소드밖에 없기 때문에 람다 표현식으로 처리가 가능하다.
```java
private void runThread() {
    new Thread(() -> {
        System.out.println(Thread.currentThread().getName());
    }).start();
}
```
- 그런데 이 메소드는 run() 메소드에서 처리하는 것이 한 줄이기 때문에 더 간단히 표현 가능하다.
```text
private void runThreadSimple() {
    new Thread(() -> System.out.println(Thread.currentThread().getName())).start();
}
```
- 중괄호를 없애고, 출력문 뒤의 세미콜론을 지웠다. 이렇게 해도 정상적으로 컴파일되고 수행된다.
- 정리하자면,
  - 메소드가 하나만 존재하는 인터페이스는 @FunctionalInterface로 선언할 수 있으며, 이 인터페이스를 람다 표현식으로 처리할 수 있다.
  - (매개 변수 목록) -> 처리식으로 람다를 표현하며, 처리식이 한 줄 이상일 때에는 처리식을 중괄호로 묶을 수 있다.

# java.util.function 패키지
- Java 8 에서 제공하는 주요 Functional 인터페이스는 java.util.function 패키지에 다음과 같이 있다.
  - Predicate
  - Supplier
  - Consumer
  - Function
  - UnaryOperator
  - BinaryOperator

## Predicate
- test() 라는 메소드가 있으며, 두 개의 객체를 비교할 때 사용하고 boolean을 리턴.
- 추가로 and(), negate(), or() 이라는 default 메소드가 구현되어 있으며, isEqual() 이라는 static 메소드도 존재.

## Supplier
- get() 이라는 메소드가 있으며, 리턴값은 generic 으로 선언된 타입을 리턴한다.
- 다른 인터페이스들과는 다르게 추가적인 메소드는 선언되어 있지 않다.

## Consumer
- accept() 라는 매개 변수를 하나 갖는 메소드가 있으며, 리턴값이 없다. 그래서, 출력을 할 때처럼 작업을 수행하고 결과를 받을 일이 없을 때 사용한다.
- 추가로 andThen() 이라는 default 메소드가 구현되어 있는데, 순차적인 작업을 할 때 유용하게 사용될 수 있다.

## Function
- apply() 라는 하나의 매개 변수를 갖는 메소드가 있으며, 리턴값도 존재한다. 이 인터페이스는 Function<T, R>로 정의되어 있어, Generic 타입을 두 개 갖고 있다.
- 앞에 있는 T는 입력 타입, 뒤에 있는 R은 리턴 타입을 의미. 즉, 변환을 할 필요가 있을 때 이 인터페이스를 사용.

## UnaryOperator: A unary operator from T -> T
- apply() 라는 하나의 매개 변수를 갖는 메소드가 있으며, 리턴값도 존재.
- 단, 한 가지 타입에 대하여 결과도 같은 타입일 경우 사용한다.

## BinaryOperator: A binary operator from (T, T) -> T
- apply() 라는 두 개의 매개 변수를 갖는 메소드가 있으며, 리턴값도 존재한다.
- 단, 한 가지 타입에 대하여 결과도 같은 타입일 경우 사용한다.


- Predicate는 다음과 같은 방법으로 선언하면 된다.
```java
Predicate<String> predicateLength5 = (a) -> a.length() > 5;
Predicate<String> predicateContains = (a) -> a.contains("God");
Predicate<String> predicateStart = (a) -> a.startsWith("God");
```
- predicateLength5 는 길이가 5보다 큰지 여부를 리턴
- predicateContains 는 "God"이라는 문자열이 포함되었는지 여부를 리턴
- predicateStart 는 "God"이라는 문자열로 시작하는지 여부를 리턴

```java
package vol2._33.functional;

import java.util.function.Predicate;

public class PredicateExample {
	public static void main(String[] args) {
		PredicateExample sample = new PredicateExample();

		Predicate<String> predicateLength5 = (a) -> a.length() > 5;
		Predicate<String> predicateContains = (a) -> a.contains("God");
		Predicate<String> predicateStart = (a) -> a.startsWith("God");

		String godOfJava = "GodOfJava";
		String goodJava = "GoodJava";

		sample.predicateTest(predicateLength5, godOfJava);
		sample.predicateTest(predicateLength5, goodJava);

		sample.predicateNegate(predicateLength5, godOfJava);
		sample.predicateNegate(predicateLength5, goodJava);

		sample.predicateAdd(predicateLength5, predicateContains, godOfJava);
		sample.predicateAdd(predicateLength5, predicateContains, goodJava);

		sample.predicateOr(predicateLength5, predicateStart, godOfJava);
		sample.predicateOr(predicateLength5, predicateStart, goodJava);


	}

	private void predicateOr(Predicate<String> p1, Predicate<String> p2, String data) {
		System.out.println(p1.or(p2).test(data));
	}

	private void predicateAdd(Predicate<String> p1, Predicate<String> p2, String data) {
		System.out.println(p1.and(p2).test(data));
	}

	private void predicateNegate(Predicate<String> p, String data) {
		System.out.println(p.negate().test(data));
	}

	private void predicateTest(Predicate<String> p, String data) {
		System.out.println(p.test(data));
	}

}
```
```text
true
true
false
false
true
false
true
true
```
- 각 메소드들에 대해서 살펴보면
  - predicateTest() : 데이터가 해당 조건에 맞는지를 확인.
  - predicateAnd() : 데이터가 두 개의 조건에 모두 맞는지 확인.
  - predicateOr() : 데이터가 두 개의 조건 중 하나라도 맞는지 확인.
  - predicateNegate() : 데이터가 조건과 다른지 확인.

- 반드시 이렇게 만들어져 있는 것만 존재하지 않기 때문에, 필요한 것은 만들어서 사용하면 된다.

# Stream
- Java 8에 "stream"(스트림)이 추가되었다. stream이라는 영어 단어는 `1.개울, 시내 2.(액체 기체의) 줄기 3. (사람, 차량들로 계속 이어진) 줄` 이라는 의미를 가진다.
- 세 가지의 의미를 통해 보면 뭔가가 줄줄이 이어져 있는 것을 나타낸다. 자바의 스트림은 언제 사용할까?
- 자바의 스트림은 "뭔가 연속된 정보"를 처리하는 데 사용한다. 어떤 것들아 "연속된 정보"를 처리했을까?
- 가장 기본적인 것은 배열이고, 그 다음에 배운 것 중 하나가 컬렉션이다. 컬렉션에는 스트림을 사용할 수 있지만, 아쉽게도 배열에는 스트림을 사용할 수 없다. 그렇지만 배열을 컬렉션의 List로
변환하는 방법은 여러 가지가 존재한다.
- 다음과 같이 1, 3, 5라는 값이 정수 배열로 있다면, Arrays 클래스의 asList() 메소드로 변환 가능하다.

```java
Integer[] values = { 1, 3, 5 };
List<Integer> list = new ArrayList<Integer>(Arrays.asList(values));
```
- 그러나 꼭 이렇게 할 필요는 없고, Arrays 클래스에 있는 stream() 이라는 메소드를 사용하면 된다. 이 메소드의 매개 변수로 배열을 넘겨주면 Stream 객체를 리턴해 준다.
```text
Integer[] values = { 1, 3, 5 };
List<Integer> list = Arrays.stream(values).collect(Collectors.toList());
```
- 스트림은 다음과 같은 구조를 가진다.
```text
list.stream().filter(x -> x > 10).count()
```
- stream(), 스트림 생성 : 컬렉션의 목록을 스트림 객체로 변환한다. 여기서 스트림 객체는 java.util.stream 패키지의 Stream 인터페이스를 말한다. 이 stream() 메소드는 당연히 
Collection 인터페이스에 선언되어 있다.
- filter(x -> x > 10), 중개 연산 : 생성된 스트림 객체를 사용하여 중개 연산 부분에서 처리한다. 하지만, 이 부분에서는 아무런 결과를 리턴하지 못한다. 그래서 중개 연산 (intermediate 
operation)이라고 한다.
- count(), 종단 연산 : 마지막으로 중개 연산에서 작업된 내용을 바탕으로 결과를 리턴해 준다. 그래서 이 부분을 종단 연산(terminal operation)이라고 한다.


- 이 저라는 꼭 기억해 두는 것이 좋다. 중개 연산은 반드시 있어야 하는 것이 아닌 0개 이상이 존재한다.
- 여기서 사용한 stream() 은 순차적으로 데이터를 처리한다. 만약 stream() 을 보다 빠르게 처리하려면 parallelStream()을 사용하면 되는데, 이는 병렬로 처리하기 때문에 CPU도 많이 사용하고,
몇개의 쓰레드로 처리할지가 보장되지 않는다. 따라서, 일반적인 웹 프로그램에는 stream() 만을 사용할 것을 권장한다.
- 스트림에서 제공하는 연산의 종류는 다음과 같다.

| 연산자                                 | 설명                          |
|-------------------------------------|-----------------------------|
| filter(pred)                        | 데이터를 조건으로 거를 때 사용           |
| map(mapper)                         | 데이터를 특정 데이터로 변환             |
| forEach(block)                      | for 루프를 수행하는 것처럼 각각의 항목을 꺼냄 |
| flatMap(flat-mapper)                | 스트림의 데이터를 잘게 쪼개서 새로운 스트림 제공 |
| sorted(comparator)                  | 데이터 정렬                      |
| toArray(array-factory)              | 배열로 변환                      |
| any / all / noneMatch(pred)         | 일치하는 것을 찾음                  |
| findFirst / Any(pred)               | 맨 처음이나 순서와 상관없는 것을 찾음       |
| reduce(binop) / reduce(base, binop) | 결과를 취합                      |
| collect(collector)                  | 원하는 타입으로 데이터를 리턴            |

# stream forEach()
```java
package vol2._33.stream;

public class StudentDTO {
	String name;
	int age;
	int scoreMath;
	int scoreEnglish;

	public StudentDTO(String name, int age, int scoreMath, int scoreEnglish) {
		this.name = name;
		this.age = age;
		this.scoreMath = scoreMath;
		this.scoreEnglish = scoreEnglish;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public int getScoreMath() {
		return scoreMath;
	}

	public void setScoreMath(int scoreMath) {
		this.scoreMath = scoreMath;
	}

	public int getScoreEnglish() {
		return scoreEnglish;
	}

	public void setScoreEnglish(int scoreEnglish) {
		this.scoreEnglish = scoreEnglish;
	}
}
```
- 이름, 나이, 점수를 갖는 학생에 대한 DTO 클래스.
```java
package vol2._33.stream;

import java.util.ArrayList;
import java.util.List;

public class StudentForEachSample {
	public static void main(String[] args) {
		StudentForEachSample sample = new StudentForEachSample();
		List<StudentDTO> studentList = new ArrayList<>();
		studentList.add(new StudentDTO("요다", 43, 99, 10));
		studentList.add(new StudentDTO("만두", 30, 71, 85));
		studentList.add(new StudentDTO("건빵", 32, 81, 75));
		sample.printStudentNames(studentList);
	}

	private void printStudentNames(List<StudentDTO> students) {
		students.stream().forEach(student -> System.out.println(student.getName()));
	}
}
```
```text
요다
만두
건빵
```
- 여기서 스트림 처리 문장은 아래와 같다.
```text
students.stream().forEach(student -> System.out.println(student.getName()));
```
- students.stream() 이후 forEach 메소드를 보면, student 이름을 출력하도록 했다.
- 여기서 student는 목록으로 넘어온 students 라는 List 객체에 담겨 있는 하나의 StudentDTO 객체를 의미한다. 그래서 이 문장은 아래와 같이 구현해도 동일하다.
```text
for (StudentDTO student : students) System.out.println(student.getName());
```
- 다음의 문장을 살펴보자.
```text
students.stream().map(student -> student.getName()).forEach(name -> System.out.println(name));
```
- 중간에 map()은 "데이터를 특정 데이터로 변환"한다. 위와 같이 map()을 사용하면, 앞으로 stream() 내에서는 StudentDTO 객체를 사용하는 것이 아니라, student.getName()의 결과인
String 값을 사용한다는 말이 된다. 그래서, map() 이후부터는 List<StudentDTO>의 스트림이 아닌 List<String> 의 스트림을 처리한다고 생각하면 된다.
- 그래서 마지막 forEach() 에서 student가 아닌 name이라고 명시를 한 것이다. 참고로
```text
forEach(name -> System.out.println(name));
```
- 이 문장은
```text
forEach(x -> System.out.println(x));
```
- 로 표현해도 문제가 안 된다.

## 메소드 참조
- 앞의 forEach 의 출력 문장은 다음과 같이 처리할 수도 있다.
```text
forEach(System.out::println()
```
- 이 더블 콜론은 정확하게 Method Reference 라고 부른다. 즉, 메소드 참조를 의미.
- 메소드 참조의 종류는 4가지 이다.

| 종류                            | 예                                 |
|-------------------------------|-----------------------------------|
| static 메소드 참조                 | ContainingClass::staticMethodName |
| 특정 객체의 인스턴스 메소드 참조            | containingObject::instanceMethod  |
| 특정 유형의 임의의 객체에 대한 인스턴스 메소드 참조 | ContainingType::methodName        |
| 생성자 참조                        | ClassName::new                    |

- 각각의 참조를 살펴보자.

## static 메소드 참조
- static 한 메소드를 참조할 때 사용된다.
```java
package vol2._33.stream;

import java.util.stream.Stream;

public class MethodReferenceSample {
	public static void main(String[] args) {
		MethodReferenceSample sample = new MethodReferenceSample();
		String[] stringArray = {"요다", "만두", "건빵"};
		sample.staticReference(stringArray);
	}

	private static void printResult(String value) {
		System.out.println(value);
	}

	private void staticReference(String[] stringArray) {
		Stream.of(stringArray).forEach(MethodReferenceSample::printResult);
	}
}
```
- staticReference() 메소드를 보면 forEach() 내에서 MethodReferenceSample::printResult로 호출하는 것을 볼 수 있다. 이 예제에서는 String의 스트림이기 때문에 forEach()
문장 안에서는 String을 제공한다. 그래서 printResult() 메소드에서는 String 값을 매개 변수로 받기 때문에 이처럼 참조해서 사용할 수 ㅇ씨다.

## 특정 객체의 인스턴스 메소드 참조
- 인스턴스 참조는 System.out::println 과 같이 System 클래스에 선언된 out 변수가 있고, 그 out 변수에 있는 println() 메소드를 호출하는 것처럼 "변수에 선언된 메소드 호출"을 의미

## 특정 유형의 임의의 객체에 대한 인스턴스 메소드 참조
```java
private void objectReference(String[] stringArray) {
    Arrays.sort(stringArray, String::compareToIgnoreCase); // 임의 객체 참조
    Arrays.asList(stringArray).stream().forEach(System.out::println); // 인스턴스 메소드 참조
}
```
- 이 코드에서는 String::compareToIgnoreCase와 같이 변수가 아니라 static 참조처럼 사용을 했다. 그런데 compareToIgnoreCase() 메소드는 다음과 같이 선언되어 있다.
```java
public int compareToIgnoreCase(String str) {
    return CASE_INSENSITIVE_ORDER.compare(this, str);
}
```
- static 메소드가 아니지만, 이와 같이 메소드 참조를 사용할 수도 있다.

## 생성자 참조
```java
interface MakeString {
    String fromBytes(char[] chars);
}

private void createInstance() {
    MakeString makeString = String::new;
    char[] chars = {'G', 'o', 'd', 'O','f','J','a','v','a'};
    String madeString = makeString.fromBytes(chars);
    System.out.println(madeString);
}
```
```text
GodOfJava
```
- MakeString 이라는 인터페이스에 char[] 배열을 받는 fromBytes() 라는 메소드를 선언해 놓았다.
- 그 다음에 createInstance() 메소드의 첫 줄을 보면 String::new 라고 되어 있다. 이렇게 사용이 가능한 이유는 String의 생성자 중에서 char[] 을 매개 변수로 받는 생성자가 있기 때문이다.
물론 String을 이렇게 사용하는 분들은 별로 없겠지만, 필요에 따라서 여러 가지 생성자를 입맛대로 인터페이스로 만들어 놓을 수 있다.

# Stream map()
- 스트림의 구조를 다시 보자.
- list.stream().filter(x -> x > 10).count()
    - stream() : 스트림 생성
    - filter() : 중개 연산
    - count() : 종단 연산
- 앞서 보앗던 forEach()는 종단 연산이다.
- 중개 연산을 살펴보자. 중개 연산의 종류는 매우 다양하지만, 일반적으로 가장 많이 사용되는 것이 map() 과 filter() 이다. 
- 다음과 같은 List가 있다.
```text
List<Integer> intList = Arrays.asList(1,2,3,4,5,6,7,8,9,10);
```
- 이 intList에 잇는 내용들은 모두 3배수로 변환해서 출력하려고 한다. 먼저 가장 기본적인 방법이 for 루프를 사용하는 것이다.
```java
package vol2._33.stream;

import java.util.Arrays;
import java.util.List;

public class StreamMapSample {
	public static void main(String[] args) {
		StreamMapSample sample = new StreamMapSample();
		List<Integer> intList = Arrays.asList(1,2,3,4,5,6,7,8,9,10);
		sample.multiplyWithFor(intList);
	}

	private void multiplyWithFor(List<Integer> intList) {
		for (Integer value : intList) {
			int tempValue = value * 3;
			System.out.println(tempValue);
		}
	}
}
```
- 위의 for문을 스트림으로 작성할 수 있다. 
```java
private void multiplyWithStream(List<Integer> intList) {
    intList.stream().forEach(value -> System.out.println(value * 3));
}
```
- 위와 같이 스트림으로 3배를 출력할 수 있다. 하지만 3배로 변환된 값들의 합을 구하려면 좀 복잡해진다. 이럴 때에는 스트림의 값 자체를 변환해 버리는 map()을 사용하자.
```java
private void multiplyWithMapStream(List<Integer> intList) {
    intList.stream().map(x -> x * 3).forEach(System.out::println);
}
```
- 중간에 map(x -> x * 3) 이라는 구문이 추가되었다. 이렇게 map()으로 변환이 되면 해당 스트림의 다음 구문에 있는 내용들이 바뀌어 버린다.
- `intList.stream().map(x ->` 까지는 {1,2,3,4,5,6,7,8,9,10}, `x * 3).forEach(System.out::println);` 부터는 {3,6,9,12,15,18,21,24,27,30} 으로 바뀐다.
- 즉, 이렇게 map()을 사용하면 스트림에서 처리하는 값들을 처리하는 값들을 중간에 변경할 수 있다.
- 이 map()은 숫자를 변경하는 것이 아니라 객체도 변경 가능하다. 다음과 같은 StudentDTO 리스트가 있다.
```java
List<StudentDTO> studentList = new ArrayList<>();
studentList.add(new StudentDTO("요다", 43, 99, 10));
studentList.add(new StudentDTO("만두", 30, 71, 85));
studentList.add(new StudentDTO("건빵", 32, 81, 75));
```
```text
List<String> nameList = studentList.stream()
    .map(student -> student.getName()).collect(Collectors.toList());
```
- 중간에 map을 사용하여 학생들의 이름을 뽑아냈고, 그 결과를 collect() 메소드를 사용하여 List로 변환하였다. 
- forEach()나 map()이 각각의 값들을 처리하는 데 비해, collect() 메소드는 모든 값들을 한곳으로 모으는 종단 연산이다. 그래서, 이 메소드를 사용하면 예제와 같이 List로 변환이 가능하다.

# stream filter()
- 이제 스트림 필터에 대해서 살펴보자. "필터 (filter)"라는 용어를 들어 보았을 것이다. 보통 일상생활의 필터는 불순물이나, 유해한 것들을 걸러내는 역할을 한다.
- 프로그래밍에서 사용하는 필터라는 용어도 이것과 동일하니 크게 걱정할 필요는 없다. 즉, 필요 없는 데이터나 웹 요청들을 걸러낼 때 사용하는 것이 이 "필터"다.
- forEach()에서 살펴본 StudentDTO 클래스에는 name, age, score 라는 변수가 있고, 각 변수의 값을 가져오는 getter와 setter가 있다. 이 클래스를 사용하는 다음 예제가 있다.
```java
package vol2._33.stream;

import java.util.ArrayList;
import java.util.List;

public class StudentFilterSample {
	public static void main(String[] args) {
		StudentFilterSample sample = new StudentFilterSample();
		List<StudentDTO> studentList = new ArrayList<>();

		studentList.add(new StudentDTO("요다", 43, 99, 10));
		studentList.add(new StudentDTO("만두", 30, 71, 85));
		studentList.add(new StudentDTO("건빵", 32, 81, 75));

		sample.filterWithScoreForLoop(studentList, 80);
	}

	private void filterWithScoreForLoop(List<StudentDTO> studentList, int scoreCutLine) {
		for (StudentDTO student : studentList) {
			if (student.getScoreMath() > scoreCutLine) {
				System.out.println(student.getName());
			}
		}
	}
}
```
```text
요다
건빵
```
- 이 filterWithScoreForLoop() 메소드를 스트림으로 구현해 보자.
```java
private void filterWithScoreStream(List<StudentDTO> studentList, int scoreCutLine) {
    studentList.stream()
        .filter(student -> student.getScoreMath() > scoreCutLine)
        .forEach(student -> System.out.println(student.getName()));
}
```
- 이와 같이 studentList를 스트림 처리하고, 개별 학생의 점수가 scoreCutLine보다 큰 학생만 그 밑에 forEach() 구문으로 이동한다. 그래서 결과는 앞서 for 루프를 사용한 것과 같다.
- 즉, filter는 if문 처럼 스트림내에서 필요한 데이터를 걸러서 처리할 때 사용한다.

# Stream 정리
- 스트림은 Collection과 같이 목록을 처리할 때 유용하게 사용된다.
  - 스트림 생성 - 중간 연산 - 종단 연산으로 구분된다.
    - 스트림 생성시에는 stream() 메소드를 호출하면 Stream 타입을 리턴한다.
    - 중간 연산은 데이터를 가공할 때 사용되며 연산 결과로 Stream 타입을 리턴한다. 따라서 여러 개의 중간 연산을 연결할 수 있다.
    - 종단 연산 스트림 처리를 마무리하기 위해서 사용되며, 숫자값을 리턴하거나 목록형 데이터를 리턴한다.
- 중간 연산의 종류에는 다음과 같은 것들이 있다.
  - filter()
  - map() / mapToInt() / mapToLong() / mapToDouble()
  - flatMap() / flatMapToInt() / flatMapToLong() / flatMapToDouble()
  - distinct()
  - sorted()
  - peek()
  - limit()
  - skip()
- 종단 연산의 종류에는 다음과 같은 것들이 있다.
  - forEach() / forEachOrdered()
  - toArray()
  - reduce()
  - collect()
  - min() / max() / count()
  - anyMatch() / allMatch() / noneMatch()
  - findFirst() / findAny()

  