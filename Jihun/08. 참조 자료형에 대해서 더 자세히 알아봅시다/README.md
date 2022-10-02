# 참조 자료형
- 기본 자료형 (Primitive type) 8개 byte, short, int, long, float, double char, boolean을 제외하고는
모두 참조 자료형이다.

# 생성자
- 생성자는 자바 클래스의 객체 (또는 인스턴스)를 생성하기 위해서 존재
- 생성자는 개수의 한계가 없지만, 알아보기 쉽게 필요한 만큼만 만드는 것이 좋다.

## DTO
- Data Transfer Object의 약자로, 어떤 속성을 갖는 클래스를 만들고, 그 속성들을 쉽게 전달하기 위해서 만든다.
비슷한 클래스로 VO(Value Object)가 있다.
- 자바는 메소드 선언시 리턴 타입은 한 가지만 선언할 수 있는데, 복합적인 데이터를 리턴하기 위해 DTO를 사용

## this
- this 예약어는 말 그대로 "이 객체"의 의미로, 생성자와 메소드에서 사용가능
```java
public class MemberDTO {
    public String name;
    public String phone;
    public String email;
    public MemberDTO(String name) {
        this.name = name;
    }
}
```
- this 가 없을 경우, 컴파일러는 중괄호 안에 있는 name은 모두 매개 변수로 넘겨준 name이라고 생각할 것이다.
- 이름을 다르게 하는 것보다, 간단한 방법이 this를 사용하는 것.
- 즉, 위 코드에서 this.name 의 경우, "이 객체의 name"이라고 명시적으로 지정해 주는 것이다.

# 메소드 Overloading
- 메소드의 이름을 같도록 하고, 매개 변수의 타입, 개수를 다르게 하는 것.
- 같은 역할을 하는 메소드는 같은 메소드 이름을 가져야 한다는 모토로 사용하는 것

# static 메소드와 일반 메소드의 차이
- static 메소드는 객체를 생성하지 않고서도 메소드를 호출할 수 있다.
- static 메소드는 클래스 변수만 사용할 수 있다. 그렇다고 무작정 인스턴스 변수에 static을 붙여 클래스 변수를 만들면 안된다.
  - 클래스 변수가 되면 모든 객체에서 하나의 값을 바라보기 때문이다.

# static 블록
- 객체는 여러 개 생성하지만, 한 번만 호출되어야 하는 코드가 있다면 static 블록을 사용하면 된다.
- 클래스 내에 선언되어 있어야 하며, 메소드 내에서는 선언할 수가 없고, 인스턴스 변수나 클래스 변수와 같이 어떤 메소드나 생성자에 속해 있으면 안 된다.
- 클래스를 초기화할 때 꼭 수행되어야 하는 작업이 있을 경우 유용
- 추가로, static한 것들만 호출 가능

# Pass by value, Pass by reference
