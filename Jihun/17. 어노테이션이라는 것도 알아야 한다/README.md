# 어노테이션
- 어노테이션은 클래스나 메소드 등의 선언시에 @를 사용하는 것을 말한다. 메타데이터라고도 불린다. JDK 5 부터 등장.
- 어노테이션은
  - 컴파일러에게 정보를 알려주거나
  - 컴파일할 때와 설치 (deployment)시의 작업을 지정하거나
  - 실행할 때 별도의 처리가 필요할 때 사용
- 매우 다양한 용도로 사용할 수 있는 어노테이션은 클래스, 메소드, 변수 등 모든 요소에 선언할 수 있다.
- 프로그램에 영향을 미치기도 하고, 아니기도 하다.

## 미리 정해져 있는 어노테이션 3개
- JDK 6 까지는 @Override, @Deprecated, @SuppressWarnings

### @Override
- 해당 메소드가 부모 클래스에 있는 메소드를 Override 했다는 것을 명시적으로 선언
- Override 할 때에는 부모 클래스의 메소드 이름과 매개 변수들을 동일하게 가져간다. 
- 만약 자식 클래스에 여러 개의 메소드가 있을 때, 어떤 메소드가 Override 되었는지 쉽게 알 수 없을 수도 있고, 실수로 매개 변수를 빠졌을 때,
이 메소드는 Override 된거니까 만약에 내가 잘못 코딩했으면 컴파일러 니가 알려줘야 해~ 라고 지정해 주는 것이다.

### @Deprecated
- 미리 만들어져 있는 클래스나 메소드가 더 이상 사용되지 않는 경우가 있다. 컴파일러에게 얘는 더 이상 사용하지 않으니까 사용하면 경고 메시지를 줘
라고 일러 주는 것이다.

### @SuppressWarnings
- 컴파일러에서 경고를 알리는 경우가 있다. 프로그램에는 문제가 없는데, 경고가 나타나면 불편할 때, 얘는 내가 일부러 이렇게 코딩한 거니까
니가 경고를 해 줄 필요가 없어 라고 이야기해주는 것이다.
```java
package vol1._17.inheritance;

public class Parent {
	public Parent() {
		System.out.println("Parent Constructor");
	}

	public Parent(String name) {
		System.out.println("Parent(String) Constructor");
	}

	public void printName() {
		System.out.println("Parent printName()");
	}
}
```
```java
package vol1._17.annotation;

import vol1._17.inheritance.Parent;

public class AnnotationOverride extends Parent {

	@Override
	public void printName() {
		System.out.println("AnnotationOverride ");
	}
}
```
- 위와 같이 printName() 이라는 Parent 클래스의 메소드를 AnnotationOverride 클래스에서 Override 했다.
- 여기에 매개 변수를 추가하면 컴파일 에러가 발생한다.
```java
package vol1._17.annotation;

import vol1._17.inheritance.Parent;

public class AnnotationOverride extends Parent {

	@Override
	public void printName(String args) {
		System.out.println("AnnotationOverride ");
	}
}
```
```text
vol1/_17/annotation/AnnotationOverride.java:7: error: method does not override or implement a method from a supertype
        @Override
        ^
1 error
```
- 어노테이션으로 Override 한다고 했는데 그런 메소드가 부모 클래스에는 없다. 
- 위와 같이 제대로 Override 했는지 확인하는 수단으로 사용할 수 있다.
- 다음으로 @Deprecated 이다.
```java
package vol1._17.annotation;

public class AnnotationDeprecated {
	@Deprecated
	public void noMoreUse() {
	}
}
```
```java
package vol1._17.annotation;

public class AnnotationSample {
	public void useDeprecated() {
		AnnotationDeprecated child = new AnnotationDeprecated();
		child.noMoreUse();
	}
}
```
```text
$ javac vol1/_17/annotation/AnnotationSample.java  
Note: vol1/_17/annotation/AnnotationSample.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
```
- 이처럼 Deprecated된 메소드를 컴파일하면 Deprecated API를 사용하는 메소드가 있으니, "-Xlint:deprecation"이라는 옵션을 추가하여 컴파일을 다시
해서 확인해보라는 메시지가 나온다.
```text
$ javac -Xlint:deprecation vol1/_17/annotation/AnnotationSample.java
vol1/_17/annotation/AnnotationSample.java:6: warning: [deprecation] noMoreUse() in AnnotationDeprecated has been deprecated
                child.noMoreUse();
                     ^
1 warning
```
- 이처럼 noMoreUse() 라는 메소드는 deprecated 되었으니 조심하라고 경고가 나타난다. 여기서 나타난 메시지를 잘 살펴보면 에러가 아닌 경고 뿐이다.
따라서 컴파일은 잘되고, 클래스 파일은 생성된다.
- 더 이상 사용되지 않는 메소드나 클래스에 @Deprecated 어노테이션만 추가해 주면 "그 메소드나 클래스는 더 이상 사용하지 않는다."고 정의할 수 있다.
- 만약 Deprecated 를 하지 않고 어떤 클래스나 메소드를 지워 버리면 다른 개발자가 만든 프로그램이 변경된 사항을 모른 샅애로 컴파일 에러가 발생할 수 있다.
하위 호환성을 위해서 Deprecated로 선언하는 것은 꼭 필요하다. 가장 좋은 방법은 계도 기간을 거쳐 알림을 준 후에 지우는 것이 바람직하다.
- 마지막으로 @SuppressWarnings이다. 경고가 발생할 때, "나도 알고 있느니까 그냥 눈 감아줘"라고 컴파일러에게 이야기해 주는 것이다.
```java
package vol1._17.annotation;

public class AnnotationSample {
	@SuppressWarnings("deprecation")
	public void useDeprecated() {
		AnnotationDeprecated child = new AnnotationDeprecated();
		child.noMoreUse();
	}
}
```
- 이 어노테이션은 지금까지 살펴본 다른 어노테이션과는 다르게 소괄호 속에 문자열을 넘겨 주었다. 다음과 같이 속성값을 지정하는 것도 가능하다.
- @SuppressWarnings 어노테이션을 지정해 준 다음에는 컴파일을 해도 아무런 메시지가 나타나지 않는다. 
- 하지만 너무 남용할 경우 Deprecated 된 메소드를 사용해도 모르고 넘어갈 수 있어서 유의해야 한다.

# 어노테이션을 선언하기 위한 메타 어노테이션
- 메타 어노테이션은 어노테이션을 우리가 선언할 때 사용. 4개가 존재
  - @Target
  - @Retention
  - @Documented
  - @Inherited

## @Target
- 어노테이션을 어떤 것에 적용할지를 선언할 때 사용. 적용방법은 다음과 같다.
```java
@Target(ElementType.METHOD)
```
- @Target() 괄호 내에 적용 대상을 지정하는데, 그 대상 목록은 다음과 같다.

| 요소 타입          | 대상                           |
|----------------|------------------------------|
| CONSTRUCTOR    | 생성자 선언시                      |
| FIELD          | enum 상수를 포함한 필드(field) 값 선언시 |
| LOCAL_VARIABLE | 지역 변수 선언시                    |
| METHOD         | 메소드 선언시                      |
| PACKAGE        | 패키지 선언시                      |
| PARAMETER      | 매개 변수 선언시                    |
| TYPE           | 클래스, 인터페이스, enum 등 선언시       |

## @Retention
- 얼마나 오래 어노테이션 정보가 유지되는지를 다음과 같이 선언한다.
```java
@Retention(RetentionPolicy.RUNTIME)
```
- 괄호 내에 지정하는 적용 가능한 대상은 다음과 같다.

| 요소 타입   | 대상                                                                        |
|---------|---------------------------------------------------------------------------|
| SOURCE  | 어노테이션 정보가 컴파일시 사라짐                                                        |
| CLASS   | 클래스 파일에 있는 어노테이션 정보가 컴파일러에 의해서 참조 가능함. 하지만, 가상 머신(Virtual Machine)에서는 사라짐 |
| RUNTIME | 실행시 어노테이션 정보가 가상 머신에 의해서 참조 가능                                            |

## @Documented
- 해당 "어노테이션에 대한 정보가 Javadocs(API) 문서에 포함된다는 것"을 선언한다.

## @Inherited
- 모든 자식 클래스에서 부모 클래스의 어노테이션을 사용 가능하다는 것을 선언한다.

## 어노테이션 선언하기
```java
package vol1._17.annotation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface UserAnnotation {
	public int number();
	public String text() default "This is first annotation";
}
```
- @Target은 해당 어노테이션 사용 대상을 지정한다. 여기서는 ElementType.METHOD를 소괄호 안에 넣어 줌으로써 이 어노테이션은 메소드에
사용할 수 있다고 지정된 것이다.
- @Retention은 어노테이션 유지 정보를 지정하는 데 사용. RetentionPolicy.RUNTIME이므로, 실행시에 이 어노테이션을 참조하게 된다.
- 어노테이션 이름인 UserAnnotation 앞에는 @interface로 이렇게 선언하면 @UserAnnotation이 사용 가능해진다.
- 어노테이션 선언 안에는 number() 라는 메소드와 text() 라는 메소드가 있다. number() 의 리턴 타입은 int이며, text()의 리턴 타입은 String이다.
이렇게 어노테이션 안에 메소드처럼 선언하면 이 어노테이션을 사용할 때, 해당 항목에 대한 타입으로 값 지정 가능
- text() 에 default 예약어로 문자열을 지정하였다. 이 어노테이션을 사용할 때의 기본 값 지정 가능
```java
package vol1._17.annotation;

public class UserAnnotationSample {
	@UserAnnotation(number = 0)
	public static void main(String[] args) {
		UserAnnotationSample sample = new UserAnnotationSample();
	}

	@UserAnnotation(number = 1)
	public void annotationSample1() {

	}

	@UserAnnotation(number = 2, text = "second")
	public void annotationSample2() {

	}

	@UserAnnotation(number = 3, text = "third")
	public void annotationSample3() {

	}
}
```
- 이렇게 각 메소드의 이름에 해당하는 값을 소괄호 안에 넣어 주어야만 한다.
- number()는 필수로 지정해주고, text()는 지정하지 않으면 default인 "This is first annotation"으로 지정된다.
- 생성자나 클래스에서도 사용할 수 있도록 바꾸려면
```text
@Target({ElementType.METHOD, ElementType.TYPE})
```
- 이렇게 선언해 두면, 메소드와 클래스에서 해당 어노테이션을 사용할 수 있다.

## 어노테이션에 선언한 값은 어떻게 확인하지?
```java
package vol1._17.annotation;

import java.lang.reflect.Method;

public class UserAnnotationCheck {
	public static void main(String[] args) {
		UserAnnotationCheck sample = new UserAnnotationCheck();
		sample.checkAnnotations(UserAnnotationSample.class);
	}

	private void checkAnnotations(Class useClass) {
		Method[] methods = useClass.getDeclaredMethods();
		for (Method method : methods) {
			UserAnnotation annotation = method.getAnnotation(UserAnnotation.class);
			if (annotation != null) {
				int number = annotation.number();
				String text = annotation.text();
				System.out.println(method.getName() + "() : number = " + number + " text = " + text);
			} else {
				System.out.println(method.getName() + "() : annotation is null.");
			}
		}
	}
}
```
- Class, Method 라는 것은 자바의 리플렉션이라는 API에서 제공하는 클래스들이다. Class 라는 클래스는 클래스의 정보를 확인하고,
Method는 메소드의 정보를 확인하는 데 사용한다. 코드의 주요 부분을 살펴보자.
1. Class 클래스에 선언되어 있는 getDeclareMethods() 메소드를 호출하면, 해당 클래스에 선언되어 있는 메소드들의 목록을 배열로 리턴
2. Method 클래스에 선언되어 있는 getAnnotation() 메소드를 호출하면, 해당 메소드에 선언되어 있는 매개 변수로 넘겨준 어노테이션이 있는지
확인하고, 있을 경우 그 어노테이션의 객체를 리턴해준다.
3. 어노테이션에 선언된 메소드를 호출하면, 그 값을 리턴해준다.

```text
main() : number = 0 text = This is first annotation
annotationSample1() : number = 1 text = This is first annotation
annotationSample2() : number = 2 text = second
annotationSample3() : number = 3 text = third
```
- 이렇게 리플렉션 API로 어노테이션에 대한 정보를 확인할 수 있다.

## 어노테이션도 상속이 안돼요
- 어노테이션 용도에 따라 다음과 같이 나눌 수 있다.
  - 제약사항 등을 선언하기 위해 : @Deprecated, @Override, @NotNull
  - 용도를 나타내기 위해 : @Entity, @TestCase, @WebService
  - 행위를 나타내기 위해 : @Stateful, @Transaction
  - 처리를 나타내기 위해 : @Column, @XmlElement
- 어노테이션이 있기 전 모든 자바 애플리케이션의 설정을 XML이나 properties 라는 파일에 지정해 왔다. 그러면서 설정이 복잡해지고, 어떤 설정이
어디에 쓰이는지 이해하려면 많은 시간이 소요되었고, 어노테이션이 생기고, 가독성이 매우 좋아졌다. 하지만 여전히 XML과 같은 설정파일은 필요한 부분이 있어
계속 쓰인다.
- 나중에는 롬복을 사용해 필요한 작업을 어노테이션만으로도 편하게 처리할 수 있도록 도와준다.
