1. Static Nested 클래스와 로컬 내부 클래스 생성 방식의 차이를 코드로 표현해주세요.
- Static Nested 클래스
```java
OuterOfStatic.StaticNested staticNested =  new OuterOfStatic.StaticNested();
```

- 로컬 내부 클래스
```java
OuterOfInner outer = new OuterOfInner();
OuterOfInner.Inner inner = outer.new Inner();
```

2. Nested 클래스를 만드는 이유가 뭘까요?
- 한 곳에서만 사용되는 클래스를 논리적으로 묶어서 처리할 필요가 있을 때 Static Nested 클래스를 사용합니ㅏㄷ.
- 캡슉화가 필요할 때, 즉, 내부 구현을 감추고 싶을 때 inner 클래스를 사용합니다.
- 소스의 가독성과 유지보수성을 높이고 싶을 때 사용합니다.

3. Static Nested 클래스와 로컬 내부 클래스에서 참조할 수 있는 변수 범위를 말해주세요.
- Static Nested 클래스 : static으로 선언된 클래스 변수만 참조 가능
- 로컬 내부 클래스 : private 으로 선언된 변수를 포함하여 모두 참조 가능합니다.

4. 반대로 감싸고 있는 클래스에서 Static Nested 클래스와 내부 클래스의 접근할 수 있는 변수 범위는?
- private 으로 선언된 변수를 포함하여 모두 참조 가능합니다.

