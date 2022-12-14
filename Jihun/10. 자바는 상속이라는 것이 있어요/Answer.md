1. 상속을 하면 부모 클래스의 어떤 접근 제어자로 선언된 무엇을 사용가능한가요?
- public과 protected로 선언된 변수와 메소드를 사용가능합니다.

2. 상속 존재 이유에 대해 설명해주세요.
- 부모 클래스가 갖고 있는 변수와 메소드를 상속 받음으로써, 개발할 때 이중, 삼중의 일을 하지 않아도 됩니다.
- 즉, 상속이 없으면, 모든 클래스의 공통적인 일을 하는 메소드가 있을 때, 그 메소드를 고치기 위해 일일이 고쳐 주어야 하는 
상황이 발생할 수 있습니다.

3. super()에 대해 설명해주세요.
- super()로 사용하면 부모 클래스의 생성자를 호출한다는 것을 의미합니다. super.method이름()처럼 메소드 호출또한 가능합니다.
- 기본 생성자가 없을 때, 실행 오류가 발생하는 이유도 자식 클래스의 생성자에는 지정하지 않아도, 자식 클래스를 컴파일
  할 때, 자동으로 super()라는 문장이 들어가기 때문입니다.
- 기본 생성자 없이, 다른 매개 변수를 가진 생성자만 있을 때, 오류가 발생한다면, 기본 생성자를 추가해 주거나,
  명시적으로 매개변수가 있는 super()를 자식 클래스의 생성자에 추가해 주는 방법이 있습니다.
- 그리고, 반드시, super()는 자식 클래스 생성자에서 가장 첫줄에 선언되어야 합니다.

4. super()에 null로 넘겨주면 어떻게 될까요?
- 매개 변수가 한 개인 생성자가 한 개밖에 없으면 상관없지만, 매개 변수가 한 개인 생성자가 매개 변수 타입이 다른 상태로,
  여러 개 있을 경우, 어떤 생성자를 참조해야 될 지 모호하다는 오류가 발생한다.
- 따라서, null을 넘기는 것보다는 호출하고자 하는 생성자의 기본 타입을 넘겨주는 것이 좋다.

5. 메소드 Overriding

   1. 메소드 오버라이딩에 대해 설명해주세요.
   - 부모 클래스로부터 상속받은 메소드를 자식 클래스에서 재정의하여 사용하는 것입니다.
   - 부모 클래스의 메소드와 동일한 시그니처를 갖는 자식 클래스의 메소드가 존재할 때 성립됩니다.

   2. 메소드 Overriding에서 접근 제어자가 어떻게 되어야 하나요? 리턴 타입은요?
   - 리턴 타입은같아야 하며, 접근 권한은 동일하거나 확장되어야 합니다.
   - 축소될 경우 컴파일 에러가 발생합니다.


6. 형 변환에서 부모 객체를 자식 객체로 언제 변환 할 수 있나요?
- 부모 타입의 실제 객체는 자식 타입일 때에만 변환할 수 있습니다.
- 예를들면

```java
Child child = new Child();
Parent parent = child;
Child child2 = (Child)parent;
```
- 다음과 같이 parent의 실제 객체가 Child 생성자로 만들어진 Child 객체이기 때문에 다음과 같은 상황에서 형 변환이 가능합니다.

7. 다형성에 대해 설명해주세요.
- 형 변환을 하더라도, 실제 호출되는 것은 원래 객체에 있는 메소드가 호출된다 는 것이 바로 다형성이다.
- 예를 들어 보면, Parent 클래스에 printName() 메소드가 있고, ChildA와 ChildB 가 각각 Parent를 상속받고,
printName을 재정의 했을 때, 
```java
Parent parent = new Parent();
Parent childA = new ChildA();
Parent childB = new ChildB();
```
- 다음과 같이 Parent 객체로 선언하고 메소드를 호출 한다고 해도 childA, childB의 경우 각각 ChildA, ChildB에서
재정의한 메소드가 호출되는 것을 말합니다.
