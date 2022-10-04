# java.lang.Object
- 모든 클래스의 부모 클래스는 java.lang.Object 클래스이다. 이것을 알 수 있는 방법은 Object 내의 메소드를 사용하는 것.

# Object 클래스에서 제공하는 메소드
- protected Object clone() : 객체의 복사본을 만들어 리턴
- public boolean equals(Object obj) : 현재 객체와 매개 변수로 넘겨받은 객체가 같은지 확인한다. 같으면 true, 다르면 false
- protected void finalize() : 가비지 컬렉터에 의해 호출됨.
- public Class<?> getClass() : 현재 객체의 Class 클래스의 객체를 리턴
- public int hashCode() : 객체에 대한 해시 코드 값 리턴. 16진수로 제공되는 객체의 메모리 주소
- public String toString() : 객체를 문자열로 표현하는 값을 리턴

## 쓰레드 처리를 위한 메소드
- public void notify() : 이 객체의 모니터에 대기하고 있는 단일 쓰레드를 깨운다.
- public void notifyAll() : 이 객체의 모니터에 대기하고 있는 모든 쓰레드를 깨운다.
- public void wait() : 다른 쓰레드가 현재 객체에 대한 notify() 메소드나 notifyAll() 메소드를 호출할 때까지 현재 쓰레드가 대기하고
있도록 함
- public void wait(long timeout) : wait()와 동일, timeout 밀리초만큼 대기. (1ms = 1/1,000초)
- public void wait(long timeout, int nanos) : wait()와 동일, timeout 밀리초 + 나노초만큼 대기. (나노초=1/1,000,000,000초)

## toString() 메소드
- toString() 메소드는 Object 클래스의 메소드 중에서 가장 많이 사용된다. 해당 객체가 어떤 객체인지 쉽게 나타내는 메소드로 자동으로 호출되는 경우는
  - System.out.println() 메소드에 매개 변수로 들어가는 경우
  - 객체에 대하여 더하기 연산을 하는 경우
- toString()으로 출력된 결과는 뭘까? Object 클래스에 구현되어 있는 toString() 메소드는 다음과 같다.
```text
getClass().getName() + '@' + Integer.toHexString(hashCode)
```
- Object 클래스에 있는 getClass()의 결과에 getName() 메소드를 부르면 현재 클래스의 패키지 이름과 클래스 이름이 나온다.
- 그 다음에는 at (@)이 붙고, 객체의 해시 코드 값을 출력한다. int로 반환된 hashCode를 Integer 클래스의 toHexString() 메소드로
16진수로 변환한다.
- 이 toString()은 Overriding 하여 주로 사용. 특히 DTO 패턴에서는 필수로 오버라이드 할 것을 권장.

## ==, equals()
- 두 개의 값이 같은지 다른지를 비교하는 연산자는 ==와 !=이 있다. but, 기본 연산자에서만 사용가능하고, 값을 비교하는 것이 아니라
"주소값"을 비교한다.
```java
private void equalsMethod() {
    MemberDTO obj1 = new MemberDTO("Sangmin");
    MemberDTO obj2 = new MemberDTO("Sangmin");

    if (obj1 == obj2) {
        System.out.println("obj1 and obj2 is same");
    } else {
        System.out.println("obj1 and obj2 is different");
    }
}
```
```text
obj1 and obj2 is different
```
- obj1 과 obj2는 각각의 생성자를 사용하여 만들었기 때문에 주소값이 다르다. 그런데, name은 "Sangmin", phone, email은 모두 null로
동일하다. 그래서 이와 같이 참조 자료형은 equals()라는 메소드를 사용하여 두 객체를 비교해야 한다.
```java
public void equalsMethod2() {

    MemberDTO obj1 = new MemberDTO("Sangmin");
    MemberDTO obj2 = new MemberDTO("Sangmin");

    if (obj1.equals(obj2)) {
        System.out.println("obj1 and obj2 is same");
    } else {
        System.out.println("obj1 and obj2 is different");
    }
}
```
```text
obj1 and obj2 is different
```
- 하지만 여전히 다르다고 나온다. MemberDTO에서 아직 equals를 Override하지 않았다. 오버라이드하지 않았다면 equals() 메소드에서는
hashCode() 값을 비교한다. 즉, 해당 객체의 주소값을 비교하는 것이다. 그렇다면, equals()를 Overriding 해보자.
```java
public boolean equals(Object obj) {
    if (this == obj) return true; // 주소가 같으므로 당연히 true
    if (obj == null) return false; // obj가 null이므로 당연히 false
    if (getClass() != obj.getClass()) return false; // 클래스의 종류가 다르므로 false

    MemberDTO other = (MemberDTO)obj; // 같은 클래스이므로 형 변환 실행

    if (name == null) { // name이 null일 때
        if (other.name != null) return false; // 비교 대상의 name이 null이 아니면 false
    } else if (!name.equals(other.name)) return false; // 두 개의 email 값이 다르면 false

    if (email == null) {
        if (other.email != null) return false;
    } else if (!email.equals(other.email)) return false;

    if (phone == null) {
        if (other.phone != null) return false;
    } else if (!phone.equals(other.phone)) return false;

	// 모든 난관을 거쳐서 false를 리턴하지 않은 객체는 같은 값을 가지는 객체로 생각해서 true를 리턴한다.
    return true;
}
```
equals()메소드를 Overriding 할 때에 "반드시" 다음 다섯가지 조건을 만족시켜야 한다.
- 재귀 (reflexive) : null이 아닌 x라는 객체의 x.equals(x) 결과는 항상 true
- 대칭 (symmetric) : null이 아닌 x와 y 객ㅊ가 있을 떄, y.equals(x)가 true라면 x.equals(y)도 반드시 true 리턴
- 타동적 (transitive) : null이 아닌 x, y, z가 있을 때 x.equals(y)가 true를 리턴하고, y.equals(z)가 true를 리턴하면,
x.equals(z)는 반드시 true를 리턴해야만 한다.
- 일관 (consistent) : null이 아닌 x와 y가 있을 때 객체가 변경되지 않은 상황에서는 몇 번을 호출하더라도 x.equals(y)는
항상 true이거나, 항상 false여야만 한다.
- null과의 비교 : null이 아닌 x라는 객체의 x.equals(null) 결과는 항상 false여야만 한다.
```java
public int hashCode() {
    final int prime = 31;
    int result = 1;
    result = prime * result + ((email == null) ? 0 : email.hashCode());
    result = prime * result + ((name == null) ? 0 : name.hashCode());
    result = prime * result + ((phone == null) ? 0 : phone.hashCode());
    return result;
}
```
- equals() 메소드를 Overriding 할 때에는 hashCode() 메소드도 같이 Overriding 해야 한다.
- equals() 메소드를 Overriding해서 객체가 서로 같다고 이야기할 수는 있겠지만, 그 값이
같다고 해서 그 객체의 주소 값이 같지는 않기 때문이다.
- 따라서 해당 값 또한 같도록 하기 위해 hashCode() 또한 Overriding 해야 한다.

## 객체의 고유값을 나타내는 hashCode()
- hashCode() 메소드는 기본적으로 객체의 메모리 주소를 16진수로 리턴한다.
- 만약 어떤 두 개의 객체가 서로 동일하다면 hashCode() 값도 무조건 동일해야만 한다.
- 따라서 equals() 메소드를 override 하면, hashCode() 또한 override 해야 한다.

### hashCode() 구현 시 규약
- 자바 애플리케이션이 수행되는 동안에 어떤 객체에 대해서 이 메소드가 호출될 때에는 항상 동일한
int 값을 리턴해 주어야 한다. 하지만, 자바를 실행할 때마다 같은 값이어야 할 필요는 전혀 없다.
- 어떤 두 개의 객체에 대하여 equals() 메소드를 사용하여 비교한 결과가 true일 경우에, 두 객체의
hashCode() 메소드를 호출하면 동일한 int 값을 리턴해야만 한다.
- 두 객체를 equals() 메소드를 사용하고 비교한 결과 false를 리턴했다고 해서, hashCode()
메소드를 호출한 int값이 무조건 달라야 할 필요는 없다. 하지만, 이 경우에 서로 다른 int값을 제공하면
hashtable의 성능을 향상시키는 데 도움이 된다.

