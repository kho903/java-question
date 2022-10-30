### Lamda 표현식.

익명 클래스를 사용하면 가독성도 떨어지고 불편한데, 이러한 단점을 보완하기 위해서 람다 표현식이 만들어졌다. 

이 표현식은 인터페이스에  메소드가 “하나”인 것들만 적용 가능하다. 그래서, 람다 표현식은 익명 클래스로 전환이 가능하며, 익명 클래스는 람다 표현식으로 전환이 가능하다.

Java에 있는 인터페이스 중, 메소드가 하나인 인터페이스에는 어떤 것들이 있을까?

- java.lang.Runnable
- java.util.Comparator
- java.io.FileFilter
- java.util.concurrent.Callable
- java.security.PrivilegedAction
- java.nio.file.PathMatcher
- java.lang.reflect.InvocationHandler

가장 많이 사용되는 스레드를 처리하기 위한 Runnable 이 있고, 값을 비교하기 위한 Comparator 도 여기에 속한다. 

기본 람다 표현식은 3 부분으로 구성되어 있다. 

| 매개 변수 목록 | 화살표 토큰(Arrow Token) | 처리 식 |
| --- | --- | --- |
| (int x, int y)  | - > | x  + y |

**정리** 

- 메소드가 하나만 존재하는 인터페이스는 @FunctionalInterface 로 선언할 수 있으며, 이 인터페이스를 람다 표현식으로 처리할 수 있다.
- (매개 변수 목록) → 처리식으로 람다를 표현하며, 처리식이 한 줄 이상일 때에는 처리식을 중괄호로 묶을 수 있다.

---

### java.util.function 패키지.

Java8에서 제공하는 주요 Functional 인터페이스 java.util.function 패키지

- Predicate
- Supplier
- Consumer
- Function
- UnaryOperator
- BinaryOperator

Predicate 

- test()라는 메소드가 있으며, 두 개의 객체를 비교할 때 사용하고 boolean을 리턴한다.
- 추가로 and(), negate(), or()이라는 default 메소드가 구현되어 있으며, isEqauls()이라는 static 메소드도 존재한다.

Supplier

- get() 메소드가 있으며, 리턴값 generic으로 선언된 타입을 리턴한다.
- 다른 인터페이스들과는 다르게 추가적인 메소드는 선언되어 있지 않다.

Consumer

- aceept()라는 매개 변수를 하나 갖는 메소드가 있으며, 리턴값이 없다.
- 그래서, 출력을 할 때 처럼 작업을 수행하고 결과를 받을 일이 없을 때 사용한다.
- 추라고 andThen()이라는 default 메소드가 구현되어 있는데, 순타적인 작업을 할 때 유용하게 사용될 수 있다.

Function

- apply()라는 하나의 매개 변수를 갖는 메소드가 있으며, 리턴값도 존재한다.
- 이 인터페이스는 Funtion<T, R>로 정의되어 있어, Generic 타입을 두개 갖고 있다. 앞에 있는 T는 입력 타입, 뒤에있는 R는 리턴 타입을 의미한다. 즉, 변환할 필요가 있을 때 이 인터페이스를 사용한다.

UnaryOperator : A unary operator from T → T

- apply()라는 하나의 매개 변수를 갖는 메소드가 있으며, 리턴값도 존재한다. 한 가지 타입에 대하여 결과도 같은 타입일 경우 사용한다.

---

### Stream

: Java 8에 스트림이 추가되었다. 

스트림은 다음과 같은 구조를 가진다. 

```java
list.stream().filter(x -> x>10).count()
```

- 스트림 생성 : 컬렉션의 목록을 스트림 객체로 변환한다. 여기서 스트림 객체는 [java.util.stream](http://java.util.stream) 패키지의 Stream 인터페이스를 말한다. 이 stream() 메소드는 당연히 Collection 인터페이스에 선언되어 있다.
- 중개 연산 : 생성된 스트림 객체를 사용하여 중개 연산 부분에서 처리한다. 하지만 이 부분에서는 아무런 결과를 리턴하지 못한다.
    - 그래서 중개 연산이라고 한다.
- 종단 연산 : 마지막으로 중개 연산에서 작업된 내용을 바탕으로 결과를 리턴해 준다. 그래서 이 부분을 종단 연산이라고 한다.

> stream()을 보다 빠르게 처리하려면 parallelStream()을 사용하면 되는데, 
이는 병렬로 처리하기 때문에 CPU도 많이 사용하고 몇 개의 스레드로 처리할지가 보장되지 않는다. 
따라서, 일반적인 웹 프로그램에는 stream()만을 사용할 것을 권장한다.
> 

| 연산자 | 설명 |
| --- | --- |
| filter(pred) | 데이터를 조건으로 거를 때 사용 |
| map(mapper) | 데이터를 특정 데이터로 변환 |
| forEach(block) | for 루프를 수행하는 것처럼 각각의 항목을 꺼냄. |
| flatMap(flat-mapper) | 스트림의 데이터를 잘게 쪼개서 새로운 스트림 제공. |
| sorted(comparator) | 데이터 정렬 |
| toArray(array-factory) | 배열로 변환 |
| any / all /noneMatch(pred) | 일치하는 것을 찾음.  |
| findFirst / Any(pred) | 맨 처음이나 순서와 상관없는 것을 찾음.  |
| reduce(binop)  / reduce(base, binop) | 결과를 취합 |
| collect(collector) | 원하는 타입으로 데이터를 리턴.  |

---

### 메소드 참조.

```java
forEach(System.out::println)
```

메소드 참조의 종류는 4가지다. 

| 종료 | 예 |
| --- | --- |
| static 메소드 참조 | ContainingClass:staticMethodName |
| 특정 객체의 인스턴스 메서드 참조.  | containingObject::instanceMethodName |
| 특정 유형의 임의의 객체에 대한 인스턴스 메소드 참조 | ContainingType::methodName |
| 생성자 참조.  | ClassName::new |

---

### 정리

스트림은 Collection 과 같이 목록을 처리할 때 사용된다. 

- 스트림 생성 - 중간 연산 - 종단 연산으로 구분된다.
    - 스트림 생성시에는 stream() 메소드를 호출하면 Stream 타입을 리턴한다.
    - 중간 연산은 데이터를 가공할 때 사용되며 연산 결과로 Stream 타입을 리턴한다. 따라서 여러 개의 중간 연산을 연결할 수 있다.
    - 종단 연산은 스트림 처리를 마무리하기 위해서 사용되며, 숫자값을 리턴하거나 목록형 데이터를 리턴한다.

중간 연산의 종류

- filter()
- map() / mapToInt()  / mapToLong() / mapToDouble()
- flatMap() / flatMapToInt() / flatMapToLong() / flatMapToDouble()
- distinct()
- sorted()
- peek()
- limit()
- skip()

종단 연산의 종류

- forEach() / forEachOrdered()
- toArray()
- reduce()
- collect()
- min() / max() / count()
- anyMatch() / allMatch() / noneMatch()
- findFirst() / findAny()
