1. 숫자를 처리하는 기본 자료형의 참조 자료형은 왜 만들었을까요? 4가지 이유
   1. 매개 변수를 참조 자료형으로만 받는 메소드를 처리하기 위해서
   2. 제네릭과 같이 기본 자료형을 사용하지 않는 기능을 사용하기 위해
   3. MIN_VALUE, MAX_VALUE 와 같이 클래스에 선언된 상수값을 사용하기 위해
   4. 문자열을 숫자로, 숫자를 문자열로 쉽게 변환하고, 각 진수별 변환을 쉽게 처리하기 위해

2. System 클래스에 3가지 static 변수에 대해 설명해 주세요.
   1. static PrintStream err : 에러 및 오류를 출력할 떄 사용
   2. static InputStream in : 입력값을 처리할 때
   3. static PrintStream out : 출력값을 처리할 때

3. System.out.println() 의 매개 변수로 null인 객체가 들어갈 경우 어떻게 되는지 설명해주세요.
- null인 객체이므로 toString() 메소드를 호출할 수 없는데도 예외가 발생하지 않고, null이라고 출력된다.
- 그 이유는 단순히 toString() 메소드 결과가 아닌 String의 valueOf()라는 static 메소드를 호출하여 결과를
받은 후 출력한다. null인 객체 obj에 obj.toString() 으로 호출할 경우 NPE가 발생한다.
- 따라서 객체를 출력할 때에는 toString() 보다는 valueOf() 메소드를 사용하는 것이 훨씬 안전하다.

4. OutOfMemoryError와 StackOverflowError에 대해 설명해 주세요.
- OOME의 경우 메모리가 부족해서 발생하는 에러입니다. 자바는 가상 머신에서 메모리를 관리하지만 프로그램을
잘못 작성하거나 설정이 제대로 되어 있지 않을 경우 발생합니다.
- StackOverflowError는 호출된 메소드의 깊이가 너무 깊을 때 발생합니다. 자바에서는 스택이라는
영역에 어떤 메소드가 어떤 메소드를 호출되었는지에 대한 정보를 관리하는데, 예를 들어, 재귀 메소드를
잘못 작성했다면 스택에 쌓을 수 있는 메소드 호출 정보의 한계를 넘어설 수 있습니다. 이럴 때 발생합니다.

