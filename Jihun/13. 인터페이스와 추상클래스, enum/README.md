# interface
시스템을 만드는 절차가 있다. 어떤 시스템을 개발하든 간에 "방법론"이라는 것을 사용하여 개발한다. 방법론이라는 것은 시스템을 어떻게 만들 것인지에
대한 절차를 설명하고, 어떤 문서 (산출물)를 작성해야 하는지를 정리해 놓은 공동절차. 일반적인 절차는
- 분석
- 설계
- 개발 및 테스트
- 시스템 릴리즈

이다. 방법론도 종류가 매우 많다.

- 여기서 인터페이스와 abstract 클래스는 설계 단계에서 만들어 두면 개발할 때 어떤 클래스를 만들지, 어떤 메소드를 만들지, 어떤 변수를 만들지 정리하고,
메소드의 이름을 어떻게 할지, 매개 변수를 어떻게 할지를 일일이 고민하지 않아도 된다.
- 그리고, 개발자의 역량에 따라 다른 메소드 이름, 매개 변수의 이름을 명명하는 데에 격차를 줄일 수 있다.
- 또 한가지 중요한 점은 해당 메소드를 사용하는 사용자의 입장에서 내부 구현이 어떻게 되어 있는지는 별로 궁금하지 않고, 원하는 메소드를 호출하고 그 답을 받으면 된다.
이것이 인터페이스의 역할이다.
- 가장 일반적인 것이 DAO (Data Access Object) 패턴이다. 이 패턴은 데이터를 저장하는 저장소에서 원하는 값을 요청하고 응답을 받는다. (DBMS에 상관없이.)
    - 예를 들어, MemberDAO가 있을 때, 어떤 메소드가 필요할까? 회원의 전화번호를 매개 변수로 넘겨주면, 회원의 상세 정보를 제공한다든지, 이름을
    넘겨주면, 동명이인을 포함한 정보들의 목록을 보여준다든지 여러 종류의 메소드들이 존재할 것이다.
    - 이 메소드들은 어떠한 DBMS를 사용해도 상관 없도록 만들어져 있을 것이다. 이 인터페이스를 구현해서 Oracle, MySQL, MongoDB 무엇을 사용하든
    결과만 제대로 넘겨주면 된다. 
- 이렇게, 인터페이스를 정해 놓으면, 선언과 구현을 구분할 수 있다.

## 인터페이스와 abstract 클래스를 사용하는 이유 정리
- 설계시 선언해 두면 개발할 때 기능을 구현하는 데에만 집중할 수 있다.
- 개발자의 역량에 따른 메소드의 이름과 매개 변수 선언의 격차를 줄일 수 있다.
- 공통적인 인터페이스와 abstract 클래스를 선언해 놓으면, 선언과 구현을 구분할 수 있다.

# 인터페이스 구현
```java
package _13.service;

import _13.model.MemberDTO;

public interface MemberManager {
	public boolean addMember(MemberDTO member);
	public boolean removeMember(String name, String phone);
	public boolean updateMember(MemberDTO member);
}
```
- public interface로 시작. 메소드 몸통(body)이 있으면 안된다. 확장자는 그대로 .java
- 프로젝트마다 표준을 달리하는데, 보통 아무런 명시적인 단어를 지정하지 않거나, 클래스 이름 앞에 I를 붙여서 IMemberManager라고 할 수도 있다.

# 인터페이스 활용
```java
package _13.service;

import _13.model.MemberDTO;

public class MemberManagerImpl implements MemberManager {
}
```
- 위와 같이 implements 뒤에 인터페이스를 추가하여 나열할 수 있다. "구현하다"라는 의미로, 구현은 상속과 달리 해당 클래스에서
구현해야 하는 인터페이스들을 정의함으로써 클래스에 짐을 지어 주는 것이다. 즉, 상속이 아니므로, 여러 개를 implements 할 수 있다.
- 위의 클래스를 컴파일 해보면 abstract 클래스도 아니고, MemberManager에 정의되어 있는 updateMember()라는 abstrac 메소드도 구현하지 않았다는 
에러가 발생한다. 
- 여기서 abstract란 추상적인 이라는 뜻으로, 자바에서는 메소드가 구현되지 않은, 인터페이스에 있는 메소드 선언문들과 같이 몸통이 없이 선언한 것을
abstract 라는 용어로 표현한다.

```java
package _13.service;

import _13.model.MemberDTO;

public class MemberManagerImpl implements MemberManager {
	@Override
	public boolean addMember(MemberDTO member) {
		return false;
	}

	@Override
	public boolean removeMember(String name, String phone) {
		return false;
	}

	@Override
	public boolean updateMember(MemberDTO member) {
		return false;
	}
}
```
- 위와 같이 MemberManager에 정의된 메소드들을 모두 구현해야 컴파일이 정상적으로 수행된다. 위 메소드들, 즉, Member 정보를 저장하고, 삭제하고,
수정하는 일을 할 수 있도록 제대로 된 코드를 만드는 작업이 개발 프로세스 중에서 "개발"에 해당한다.

- 정리하면, 설계 단계에서 인터페이스만 만들어 놓고, 개발 단계에서 실제 작업을 수행하는 메소드를 만들면 설계 단계의 산출물과 개발 단계의 산출물이
보다 효율적으로 관리된다. 
- 그러나 이 이유 이외, 인터페이스의 용도는 외부에 노출되는 것을 정의해 놓고자 할 때 사용된다. 다시 말해서 MemberManagerImpl 이라는 클래스가 있는데,
이 클래스가 "저한테 직접 이야기하지 마시구요. 공식적인 것은 저의 대변인을 통해서 말씀하세요."에서의 대변인이 인터페이스이다.
- 그리고, 이 인터페이스는 구현된 코드가 없다면 .class 확장자라고 해서 인터페이스를 그대로 사용할 수는 없다. 생성자, 메소드 내용 아무것도 없기 때문이다.
```java
package _13;

import _13.service.MemberManager;

public class InterfaceExample {
	public static void main(String[] args) {
		MemberManager member = new MemberManager();
	}
}
```
- 위 코드는 에러가 발생하고 MemberManager가 abstrat이기 때문에 초기화가 되지 않는다는 메시지가 출력된다.
- 즉, 컴파일러가 "아무것도 구현해 놓지 않았는데, 왜 얘로 초기화하려는 것이냐"며 에러를 내뿜는 것이다.
- 위 코드는 아래와 같이 고쳐주어야 한다.
```java
MemberManager member = new MemberManagerImpl();
```
- 겉 보기에 member의 타입은 MemberManager이고, MemberManagerImpl 클래스에는 인터페이스에 선언되어 있는 모든 메소드들이 구현되어 있다.
따라서 실제 member의 타입은 MemberManager이기 때문에, member에 선언되 메소드들을 실행하면 MemberManagerImpl에 있는 메소드들이 실행된다.

### interface 유의점
- 인터페이스는 static이나 final 메소드가 선언되어 있으면 안된다.

# abstract 클래스
- abstract 클래스는 자바에서 마음대로 초기화하고 실행할 수 없도록 되어있다. 그래서, abstract 클래스를 구현해 놓은 클래스로 초기화 및 실행이
가능하다.
```java
package _13.service;

import _13.model.MemberDTO;

public abstract class MemberManagerAbstract {
	public abstract boolean addMember(MemberDTO member);
    
	public abstract boolean removeMember(String name, String phone);
    
	public abstract boolean updateMember(MemberDTO member);
    
	public void printLog(String data) {
		System.out.println("Data=" + data);
	}
}
```
- class 앞에 abstract를 붙이고, 몸통이 없는 메소드 선언문에도 abstract로 명시하였다.

## abstract 클래스 정리
- abstract 클래스는 클래스 선언시 abstract 라는 예약어가 클래스 앞에 추가되면 된다.
- abstract 클래스 안에는 abstract 으로 선언된 메소드가 0개 이상 있으면 된다. 
- abstract으로 선언된 메소드가 하나라도 있으면, 그 클래스는 반드시 abstract 으로 선언되어야만 한다.
- abstract 클래스는 몸통이 잇는 메소드가 0개 이상 있어도 전혀 상관 없으며, static이나 final 메소드가 있어도 된다.
  - (interface는 static이나 final 메소드가 선언되어 있으면 안된다.)

# abstract 구현
```java
package _13.service;

import _13.model.MemberDTO;

public class MemberManagerImpl2 extends MemberManagerAbstract {
}
```
- 위 코드는 interface에서 처럼 abstract으로 선언되어 있는 메소드들을 구현하지 않아, 에러가 발생한다.
```java
package _13.service;

import _13.model.MemberDTO;

public class MemberManagerImpl2 extends MemberManagerAbstract {
	@Override
	public boolean addMember(MemberDTO member) {
		return false;
	}

	@Override
	public boolean removeMember(String name, String phone) {
		return false;
	}

	@Override
	public boolean updateMember(MemberDTO member) {
		return false;
	}
}
```
- 위와 같이 구현 가능
- 왜 abstract 클래스를 만들었을까? 인터페이스를 선언하다 보니, 어떤 메소드는 미리 만들어 놓아도 전혀 문제가 없는 경우가 발생한다.
그렇다고 해당 클래스를 만들기는 좀 애매하고.... 특히 아주 공통적인 기능을 미리 구현해 놓으면 많은 도움이 된다.
이럴 때 사용하는 것이 abstract 클래스이다.

# 인터페이스, abstract 클래스, 클래스의 차이 표
|                     | 인터페이스     | abstract 클래스   | 클래스    |
|---------------------|-----------|----------------|--------|
| 선언 시 사용하는 예약어       | interface | abstract class | class  |
| 구현 안 된 메소드 포함 가능 여부 | 가능(필수)    | 가능             | 불가     |
| 구현된 메소드 포함 가능 여부    | 불가        | 가능             | 가능(필수) |
| static 메소드 선언 가능 여부 | 불가        | 가능             | 가능     |
| final 메소드 선언 가능 여부  | 불가        | 가능             | 가능     |
| 상속 (extends) 가능     | 불가        | 가능             | 가능     |
| 구현 (implements) 가능  | 가능        | 불가             | 불가     |

# final
- 클래스, 메소드, 변수에 선언할 수 있다.

## final 클래스
```java
package _13.util;

public final class FinalClass {

}
```
- 다음과 같이 final로 선언되어 있으면 상속을 해 줄 수 없다.
- 해당 클래스를 상속받고, 컴파일하면 final 인 FinalClass에서 상속을 받을 수는 없어요라며, 컴파일 에러가 발생.
- 이렇게 하는 이유는 뭘까? 대표적인 final 클래스인 String은 자바에서 아주 중요한 클래스이며, 이 클래스를 조금이라도 변경해서 작업을 하면 안 된다.
만약 String 클래스 상속을 받아서 toString() 메소드에서 무조건 1을 리턴하게 한다면 String이라는 클래스에 대한 기본 속성을 변경하는 것이다.
- 더 이상 확장해서는 안 되는 클래스, 누군가 이 클래스를 상속 받아서 내용을 변경해서는 안 되는 클래스를 선언할 때 final로 선언하면 된다.

## final 메소드
- 메소드에서도 클래스와 비슷하게, final로 선언하면 더 이상 Overriding 할 수 없다. 
```java
package _13.util;

public abstract class FinalMethodClass {
    public final void printLog(String data) {
        System.out.println("Data=" + data);
    }
}
```
```java
package _13.util;

public class FinalMethodChildClass extends FinalMethodClass {
    public void printLog(String data) {
        System.out.println("Data=" + data);
    }
}
```
- 위 코드를 컴파일 하면 "FinalMethodChild 클래스에 있는 printLog() 메소드는 final 이기 때문에 override 할 수 없다"라는 에러가 발생한다.
- 우리가 힘들게 만든 메소드를 누가 변경하지 못하도록 하려면 final로 만들어 다른 개발자가 그 메소드를 덮어 쓰는 것을 아주 간단하게 막을 수 있다.
- 하지만 final 클래스는 많이 쓰이지만 final 메소드는 그리 많이 보이진 않는다.

## final 변수
- 변수에 final을 사용하면 그 변수는 "더 이상 바꿀 수 없다."라는 말이다. 그래서, 인스턴스 변수나 static으로 선언되 클래스 변수는 선언과 함께
값을 지정해야만 한다.
```java
package _13.util;

public class FinalVariable {
    final int instanceVariable;
}
```
- 인스턴스 변수에 final이 붙어있다면, 초기화를 하지 않으면 컴파일 에러가 발생한다. 다음과 같이 선언해야만 한다.
```java
package _13.util;

public class FinalVariable {
    final int instanceVariable = 1;
}
```
- 변수 생성과 동시에 초기화를 해야만 컴파일시 에러가 발생하지 않는다. 생성자나 메소드에서 초기화하면 되지 않나? 라고 생각할 수도 있지만, 그러면
중복되어 변수값이 선언될 수도 있기 때문에 final의 기본 의도를 벗어난다. 이렇게 초기화를 하려면 그냥 지역변수를 선언해서 사용하면 된다.
- 인스턴스 변수나 클래스 변수는 생성과 동시에 초기화를 해야 된다.
```java
package _13.util;

public class FinalVariable {
    final int instanceVariable = 1;
    
    public void method(final int parameter) {
        final int localVariable;
    }
}
```
- 매개 변수나 지역 변수는 final로 선언하는 경우에는 반드시 선언할 때 초기화할 필요는 없다. 이미 매개 변수는 초기화가 되어 넘어왔고, 
지역 변수는 메소드를 선언하는 중괄호 내에서만 참조되므로 다른 곳에서 변경할 일이 없다.
- 하지만 다음과 같이 사용해서는 안된다.

```text
package _13.util;

public class FinalVariable {
    // ...
    public void method(final int parameter) {
        final int localVariable;
        localVariable = 2;
        localVariable = 3;
        parameter = 4;
    }
}
```
- localVariable 을 2로 선언할 때에는 문제가 없지만, 그 다음줄에서 2으로 다시 선언하면 안 되고, 매개 변수 또한 다시 값을 할당할 수 없다.

### final 변수 존재 이유
- 변수는 "변하는 수"인데, 왜 final로 변하지 못하게 할까? 예를 들어 월별 날짜와 같은 상황은 절대 바뀌지 않는 내용이다. 이러한 변수들에
final을 선언하면 매우 유용하게 사용할 수 있다. 하지만 남발해서는 안 된다.


## final reference type 
```java
package _13.util;

import _13.model.MemberDTO;

public class FinalReferenceType {
    final MemberDTO dto = new MemberDTO();
    
    public static void main(String[] args) {
        FinalReferenceType referenceType = new FinalReferenceType();
        referenceType.checkDTO();
    }
    
    public void checkDTO() {
        System.out.println(dto);
        dto = new MemberDTO();
    }
}
```
- 위와 같이 이미 new로 할당된 dto를 출력하고 다시 new로 객체를 생성하면 final이기 때문에 할당할 수 없다는 메시지와 함께 에러가 발생한다.
- 그런데, 아래와 같이 수정해보자.

```java
package _13.util;

import _13.model.MemberDTO;

public class FinalReferenceType {
    final MemberDTO dto = new MemberDTO();
    
    public static void main(String[] args) {
        FinalReferenceType referenceType = new FinalReferenceType();
        referenceType.checkDTO();
    }
    
    public void checkDTO() {
        System.out.println(dto);
        // dto = new MemberDTO();
        dto.name = "Sangmin";
        System.out.println(dto);
    }
}
```
- 이렇게 변경하면 아무 문제가 없고 name 값이 "Sangmin"으로 할당된다. 
- dto 객체, 즉 MemberDTO 클래스의 객체는 final이지만, 그 객체 안에 있는 객체들은 final로 선언된 것이 아니기 때문에 그러한 제약이 없다.
왜냐하면 MemberDTO에 선언되어 있는 name, phone, email 모두 final이 아니기 때문이다.
- 결론적으로, 해당 클래스가 final이라고 해서, 그 안에 있는 인스턴스 변수나 클래스 변수가 final은 아니다.

# enum 클래스 상수의 집합
- 어떤 클래스가 상수만으로 만들어져 잇을 경우 enum이라고 선언하면 "이 객체는 상수의 집합이다"라는 것을 명시적으로 나타낸다. 
- enumeration이라는 "셈, 계산, 열거, 목록, 일람표"라는 영어 단어의 앞부분이다.
```java
package _13.enums;

public enum OverTimeValues {
    THREE_HOUR,
    FOUR_HOUR,
    WEEKEND_FOUR_HOUR,
    WEEKEND_EIGHT_HOUR;
}
```
- 예를 들어 야근 수당에 관한 상수를 선언해놓고, 이 4개의 상수에 관련된 값을 할당할 수도 있지만, 이렇게 선언만 할 수도 있다.
- 타입지정, 값 지정이 필요 없다. 그냥 해당 상수들의 이름만 콤마로 구분하여 나열해주면 된다.
- enum 클래스 사용 코드는 다음과 같다.
```java
package _13.enums;

public class OverTimeManager {
	public static void main(String[] args) {
		OverTimeManager manager = new OverTimeManager();
		int myAmount = manager.getOverTimeAmount(OverTimeValues.THREE_HOUR);
		System.out.println(myAmount);
	}
	
    public int getOverTimeAmount(OverTimeValues value) {
		int amount = 0;
		System.out.println(value);
		switch (value) {
			case THREE_HOUR:
				amount = 18000;
				break;
			case FIVE_HOUR:
				amount = 30000;
				break;
			case WEEKEND_FOUR_HOUR:
				amount = 40000;
				break;
			case WEEKEND_EIGHT_HOUR:
				amount = 60000;
				break;
		}
		return amount;
	}
}
```
- enum 타입을 매개 변수로 받고, 변수명 value로 지정했다. switch문을 사용해 value를 조건으로 지정하고, case에는 OverTimeValues에
선언한 상수 값들로 분기하도록 했다.
- OverTimeValue enum 타입을 매개 변수로 전달하기 위해서 `OverTimeValues.THREE_HOUR` 같이 전달하였다.
- 즉, "enum 클래스이름.상수 이름"을 지정함으로써 클래스의 객체 생성이 완료된다고 생각하면 된다.

## enum 제대로 사용하기
```java
package _13.enums;

public enum OverTimeValues2 {
	THREE_HOUR(18000),
	FIVE_HOUR(30000),
	WEEKEND_FOUR_HOUR(40000),
	WEEKEND_EIGHT_HOUR(60000);
	private final int amount;

	OverTimeValues2(int amount) {
		this.amount = amount;
	}

	public int getAmount() {
		return amount;
	}
}
```
- 상수 아래 amount 라는 final 변수를 선언하고 그 다음에 생성자에서 매개 변수로 넘겨받은 값을 할당할 때 사용한다.
- enum 클래스의 생성자는 package-private과 private만 접근 제어자로 사용할 수 있다. 즉, enum 클래스 내에서 선언할 때에만 사용 가능.
- 일반 클래스와 마찬가지로, 컴파일할 때 생성자를 자동으로 만들어준다.
- 그 다음에 getAmount()라는 메소드는 enum 클래스의 변수로 선언한 amount 값을 리턴하기 위해 만들었다. 다음과 같이 enum도 
보통 클래스와 마찬가지로 메소드를 선언해서 사용 가능
- enum을 사용하는 두 가지 방법중에서 뭐가 더 나을까?
- 앞에서 했던 OverTimeValues로 선언하면 선언 자체는 매우 간단해지지만, 구현이 약간 복잡해진다. 그리고 메소드를 호출하여 switch 로 값을 간단히 할당하기는
했지만, 만약 해당 수당이 2000원 오르면 어떻게 될까? 이 값이 항상 바뀔 수 있는 경우에는 원격 서버에 있는 값을 읽어오도록 하면 큰 문제는 없을 것이다.
- 하지만 OverTimeValues2에서 수당이 2000원 오른다면 자바 프로그램을 수정한 후 다시 컴파일해서 실행중인 자바 프로그램을 중지했다가 다시 시작해야 한다는
단점이 존재한다. 하지만 성능은 OverTimeValues2이 훨씬 좋을 것이다.

## enum 클래스의 부모는 무조건 java.lang.Enum
- enum 클래스를 선언할 때 컴파일러가 알아서 extends java.lang.Enum을 추가해서 컴파일한다. 따라서, 마음대로 extends 할 수 없고, 누가
만들어 놓은 enum을 extends 하여 선언할 수도 없다.
- enum의 생성자는 다음과 같다.
- `protected Enum(String name, int ordinal)` : 컴파일러에서 자동으로 호출되도록 해 놓은 생성자이지만, 개발자가 이 생성자를 호출할 수는 없다.
- name은 enum 상수의 이름. ordinal은 enum의 순서이며, 상수가 선언된 순서대로 0부터 증가
- Enum 클래스의 부모 클래스는 Objedct 클래스이기 때문에, Object 클래스의 메소드들은 모두 사용가능하지만 개발자가 Override 하지 못하게 4개의
메소드를 막아 두었다. : clone(), finalize(), hashCode(), equals()
- hashCode(), equals()는 사용해도 무방하나, clone(), finalize()는 사용하면 안된다.

## enum 클래스 메소드
- compareTo(E e) : 매개 변수로 enum 타입과의 순서(ordinal) 차이를 리턴
- getDeclaringClass() : 클래스 타입의 enum 리턴
- name() : 상수의 이름 리턴
- ordinal() : 상수의 순서 리턴
- valueOf(Class<T> enumType, String name) : static 메소드, 첫 번째 매개 변수로는 클래스 타입의 enum, 두 번째 매개 변수로는 상수의 이름

### compareTo()
- 순서가 같은지, 다른지를 비교하는 데 사용. 같은 상수라면 0, 그렇지 않고 다르면 순서의 차이를 출력.
- 메소드의 매개 변수로 넘기는 상수 기준으로 앞에 있으면 음수(-), 뒤에 있으면 양수(+)를 리턴.

### values()
- enum 클래스에는 API 문서에 없는 특수한 메소드가 하나 있다. 바로 values() 메소드로 이 메소드를 호출하면 enum 클래스에 선언되어 있는 모든 상수를
배열로 리턴.
- 어떤 상수가 어떤 순서로 선언되었는지 확인하기가 어려운 경우에 이 메소드를 사용하면 도움이 된다.
```text
package _13.enums;

public class OverTimeManager3 {
    public static void main(String[] args) {
        OverTimeValues2[] valueList = OverTimeValues2.values();
        for (OverTimeValues2 value : valueList) {
            System.out.println(value);
        }
    }
}
```
```text
THREE_HOUR
FIVE_HOUR
WEEKEND_FOUR_HOUR
WEEKEND_EIGHT_HOUR
```
