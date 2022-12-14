1. 생성자에 대해 설명해 주세요.
- 생성자는 자바 클래스의 객체를 생성하기 위해서 존재하고,
개수의 한계가 없지만, 알아보기 쉽게 필요한 만큼만 만드는 것이 좋습니다.

2. DTO가 무엇이고, 왜 사용할까요?
- Data Transfer Object의 약자로, 어떤 속성을 갖는 클래스를 만들고, 그 속성들을
쉽게 전달하기 위해서 만듭니다.
- 자바는 메소드 선언시 리턴 타입은 한 가지만 선언할 수 있는데, 복합적인 데이터를 리턴하기 위해 DTO를 사용합니다.

3. this는 무엇이고, 언제 사용할까요?
- 말 그대로 "이 객체"의 의미로, 생성자와 메소드에서 사용합니다.
- this가 없을 경우, 매개 변수의 이름과 인스턴스 변수를 구별할 수 없고 다른이름으로 만들어야 되는 등의 불편함이 있습니다.
- 간단하게 this를 사용하여 알아보기 쉽게 만들 수 있습니다.

4. static 메소드와 일반 메소드의 차이
- static 메소드는 객체를 생성하지 않고서도 메소드를 호출할 수 있다.
- static 메소드는 클래스 변수만 사용할 수 있다. 그렇다고 무작정 인스턴스 변수에 static을 붙여
클래스 변수를 만들면 안된다.
  - 클래스 변수가 되면 모든 객체에서 하나의 값을 바라보기 때문이다.

5. pass by value / pass by reference에 대해 설명해주세요.
- pass by value의 경우 값을 전달한다는 뜻으로, 원래 값은 놔두고, 전달되는 값이 진짜인 것처럼 보이게 하는 것을 말합니다.
- 그래서 매개 변수를 받은 메소드에서 그 값을 바꾸어도 원래의 값은 변하지 않습니다. 기본 자료형은 무조건 Pass by value로 데이터를 전달합니다.

- String이 아닌 참조 자료형의 경우, 매개 변수로 받은 참조 자료형 안에 있는 객체를 변경하면, 호출한 참조 자료형 안에 있는
객체는 호출된 메소드에서 변경이 되는데, 이를 Pass by Reference 라고 합니다.
- 하지만 자바는 Pass by value로 동작합니다.
- 매개 변수로 넘어온 Reference type의 객체를 new로 새로 생성해 바꾼다면 해당 메소드를 호출한 메소드에서는
변경 내용이 무시됩니다.
- new로 생성하게 되면 객체 위치를 가리키는 정확한 복사본이 힙 메모리에 생기게 되는데, 객체 내의 정보를 변경하면 주소값 자체는
변하지 않기 때문에 원래 호출한 메소드 내에서도 값이 변경되는 것입니다.
- 만약 new를 사용해서 새로운 객체를 할당하게 되면 같은 힙 메모리를 가리키던 것이 새로운 객체를 가리키게 되어 변경값이 반영되지 않는 것입니다.


call by value는 복사하여 처리하기 때문에 안전하고 원래 값이 보존 되지만, 복사를 하기 때문에 메모리 사용량이증가하게 됩니다.
call by reference 는 복사하지 않고 직접 참고하기 때문에 속도가 빠르지만 직접 참조 하기 때문에 원래 값이 영향을 받게 되는 단점이 있습니다.
