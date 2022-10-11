1. StringBuffer와 StringBuilder 에 대해 자세히 설명해주세요. 차이점을 포함하여.
- String은 immutable한 객체이기 때문에 한 번 만들어지면 더 이상 값을 바꿀 수 없다.
- String 문자열을 더하면 새로운 String 객체가 생성되고, 기존 객체는 버려지므로, 계속 더하는 작업을 한다면 계속 쓰레기를 만들고 기존 객체는 GC의 대상이 된다.
- 이러한 단점을 보완하기 위해 나온 클래스가 StringBuffer, StringBuilder이다.
- StringBuffer는 Thread-safe하고, StringBuilder는 그렇지 않다. 대신에 속도가 더 빠르다.
- 모두 append()로 문자열을 더한다.
- 일반적으로 하나의 메소드 내에서 문자열을 생성하여 더할 경우에는 StringBuilder 를 사용해도 무관하다.
하지만 어떤 클래스에 문자열을 생성하여 더하기 위한 문자열을 처리하기 위한 인스턴스 변수가 선언되었고, 여러 쓰레드에서
이 변수를 동시에 접근할 가능성이 있는 경우 StringBuffer를 사용해야 한다.

