# 자바 컬렉션
- 자바에서 컬렉션은 목록성 데이터를 처리하는 자료 구조를 통칭한다. 
- 자바에서의 데이터를 담는 자료 구조는 크게 다음과 같이 분류할 수 있다.
  - 순서가 있는 목록(List)형
  - 순서가 중요하지 않은 셋(Set)형
  - 먼저 들어온 것이 먼저 나가는 큐(Queue)형
  - 키-값(key-value)으로 저장되는 맵(Map)형
- 자바에서는 "목록"과 "셋", "큐"는 Collection 이라는 인터페이스를 구현하고 있다. 이 Collection 인터페이스는 java.util 패키지에 선언되어 있으며, 여러 개의
객체를 하나의 객체에 담아 처리할 때 공통적으로 사용되는 여러 메소드들을 선언해 놓았다.
- List, Set, Queue의 기본이 되는 Collection 인터페이스는 다음과 같이 선언되어 있다.
```java
public interface Collection<E> extends Iterable<E>
```
- Iterable 인터페이스에 선언되어 있는 메소드는 단 하나이다.

| 리턴 타입       | 메소드 이름 및 매개 변수 |
|-------------|----------------|
| Iterator<T> | iterator()     |
- Iterator 인터페이스에는 추가 데이터가 있는지 확인하는 hasNext(), 현재 위치를 다음 요소로 넘기고 그 값을 리턴해주는 next(), 데이터를 삭제하는 remove()가 있다.
- 결론적으로, Collection 인터페이스가 Iterable 인터페이스를 확장했다는 의미는 Iterable 인터페이스를 사용하여 데이터를 순차적으로 가져올 수 있다는 의미다.

## Collection 인터페이스에 선언된 주요 메소드
- 여기서 요소 라는 것은 Elemenet로 컬렉션에 저장되는 각각의 데이터를 말한다.

| 리턴 타입    | 메소드 이름 및 매개 변수          | 설명                                                                            |
|----------|-------------------------|-------------------------------------------------------------------------------|
| boolean  | add(E e)                | 요소를 추가                                                                        |
| boolean  | addAll(Collection)      | 매개 변수로 넘어온 컬렉션의 모든 요소를 추가                                                     |
| void     | clear()                 | 컬렉션에 있는 모든 요소 데이터를 지운다                                                        |
| boolean  | contains(Object)        | 매개 변수로 넘어온 객체가 해당 컬렉션에 있는지 확인. 동일한 값이 있으면 true 리턴                             |
| boolean  | containsAll(Collection) | 매개 변수로 넘어온 객체가 해당 컬렉션에 있는지 확인. 매개 변수로 넘어온 컬렉션에 있는 요소들과 동일한 값들이 모두 있으면 true 리턴 |
| boolean  | equals(Object)          | 매개 변수로 넘어온 객체와 같은 객체인지 확인                                                     |
| int      | hashCode()              | 해시 코드값을 리턴                                                                    |
| boolean  | isEmpty()               | 컬렉션이 비어있는지 확인. 비어있으면 true를 리턴                                                 |
| Iterator | iterator()              | 데이터를 한 건씩 처리하기 위한 Iterator 객체를 리턴                                             |
| boolean  | remove(Object)          | 매개 변수와 동일한 객체를 삭제                                                             |
| boolean  | removeAll(Collection)   | 매개 변수로 넘어온 객체들을 해당 컬렉션에서 삭제                                                   |
| boolean  | retainAll(Collection)   | 매개 변수로 넘어온 객체들만을 해당 컬렉션에 남겨 둔다.                                               |
| int      | size()                  | 요소의 개수 리턴                                                                     |
| Object[] | toArray()               | 컬렉션에 있는 데이터들을 배열로 복사                                                          |
| <T> T[]  | toArray(T[])            | 컬렉션에 있는 데이터들을 지정한 타입의 배열로 복사                                                  |

# List 인터페이스와 그 동생들
- 배열과 비슷한 "목록"은 List 인터페이스로부터 시작되며, Collection 인터페이스를 확장하였다. 따라서, 몇몇 추가된 메소드를 제외하고는 Collection에 선언된 메소드와
큰 차이는 없다. Collection을 확장한 다른 인터페이스와 List 인터페이스의 가장 큰 차이점은 배열처럼 "순서"가 있다는 것이다.
- List 인터페이스를 구현한 클래스는 매우 많은데, java.util 패키지에서는 ArrayList, Vector, Stack, LinkedList를 많이 사용한다.
- ArrayList와 Vector 클래스는 거의 동일하고 기능도 거의 비슷한다. 이 두 클래스는 "확장 가능한 배열"이라고 생각하면 된다. 두 클래스의 탄생 시기를 비교하면
Vector는 JDK 1.0, ArrayList는 JDK 1.2에서 추가되었다. 그리고, ArrayList는 Thread safe 하지 않고, Vector는 Thread safe하다.
- Stack은 Vector를 확장하여 만들었는데, LIFO를 지원하기 위함이 가장 큰 이유이다. Last In First Out의 약자로, 가장 마지막에 추가한 값을 가장 처음 빼내는 것이다.
- 보통 Vector의 대부분의 메소드가 ArrayList가 동일하고, 보통 Vector보다 ArrayList를 선호한다.

# ArrayList
- ArrayList 클래스의 상속 관계는 다음과 같다.
```text
java.lang.Object
    - java.util.AbstractCollection<E>
        - java.util.AbstractList<E>
            - java.util.ArrayList<E>
```
- ArrayList의 가장 상위 부모는 Object, 그 다음으로 AbstractCollection, AbstractList 순. Object 제외 모두 Abstract 클래스이다.
- ArrayList가 구현한 모든 인터페이스들은 다음과 같다.
```text
Serializable, Cloneable, Iterable<E>, Collection<E>, List<E>, RandomAccess
```
| 인터페이스         | 용도                                                              |
|---------------|-----------------------------------------------------------------|
| Serializable  | 원격으로 객체를 전송하거나, 파일에 저장할 수 있음을 지정                                |
| Cloneable     | Object 클래스의 clone() 메소드가 제대로 수행될 수 있음을 지정. 즉, 복제가 가능한 객체임을 의미   |
| Iterable<E>   | 객체가 "foreach" 문장을 사용할 수 있음을 지정                                  |
| Collection<E> | 여러 개의 객체를 하나의 객체에 담아 처리할 때의 메소드 지정                              |
| List<E>       | 목록형 데이터를 처리하는 것과 관련된 메소드 지정                                     |
| RandomAccess  | 목록형 데이터에 보다 빠르게 접근할 수 있도록 임의로 (random하게) 접근하는 알고리즘이 적용된다는 것을 지정 |

# ArrayList의 생성자는 3개다
- ArrayList는 "확장 가능한 배열"이다. 배열처럼 사용하지만 대괄호는 사용하지 않고, 메소드를 통해서 객체를 넣고, 빼고, 조회한다.

| 생성자                                  | 설명                                                         |
|--------------------------------------|------------------------------------------------------------|
| ArrayList()                          | 객체를 저장할 공간이 10개인 ArrayList를 만든다.                           |
| ArrayList(Collection<? extends E> c) | 매개 변수로 넘어온 컬렉션 객체가 저장되어 있는 ArrayList를 만든다.                 |
| ArrayList(int initialCapacity)       | 매개 변수로 넘어온 initialCapacity 개수만큼의 저장 공간을 갖는 ArrayList를 만든다. |

```java
package vol2._22;

import java.util.ArrayList;

public class ListSample {
	public static void main(String[] args) {
		ListSample sample = new ListSample();
		sample.checkArrayList1();
	}

	public void checkArrayList1() {
		ArrayList list1 = new ArrayList();
		list1.add(new Object());
		list1.add("ArrayListSample");
		list1.add(new Double(1));
	}
}
```
- list1 객체를 생성하면 다음과 같이 어떤 객체도 넣을 수 있다. 데이터를 넣을 때에는 add() 메소드를 사용.
- 그런데, 보통 ArrayList는 이렇게 사용하지 않는다. 대부분 서로 다른 종류의 객체를 하나의 배열에 넣지 않고, 한 가지 종류의 객체만 저장한다.
- 여러 종류를 하나의 객체를 담을 때에는 DTO 객체를 하나 만들어서 담는 것이 좋다.
- 그래서, 컬렉션 관련 클래스의 객체들을 선언할 때에는 제네릭을 사용하여 선언하는 것을 권장한다.
```java
ArrayList<String> list1 = new ArrayList<String>();
ArrayList<String> list1 = new ArrayList<>(); // jdk 7 ~ 생성자 호출시 <>만 사용 가능
```
- ArrayList 객체를 선언할 때 매개 변수를 넣지 않으면 초기 크기는 10이다. 따라서, 10개 이상의 데이터가 들어가면, 크기를 늘이는 작업이 ArrayList 내부에서
자동으로 수행된다. 이러한 작업이 수행되면, 애플리케이션 성능에 영향을 주게 된다. 만약 저장되는 데이터의 크기가 어느 정도 예측 가능하다면 다음과 같이 예측한
초기 크기를 지정할 것을 권장
```java
ArrayList<String> list = new ArrayList<>(100);
```

# ArrayList에 데이터를 담아보자
| 리턴 타입   | 메소드 이름 및 매개 변수                              | 설명                                       |
|---------|---------------------------------------------|------------------------------------------|
| boolean | add(E e)                                    | 매개 변수로 넘어온 데이터를 가장 끝에 담는다.               |
| void    | add(int index, E e)                         | 매개 변수로 넘어온 데이터를 지정된 index 위치에 담는다.       |
| boolean | addAll(Collection<? extends E> c            | 매개 변수로 넘어온 컬렉션 데이터를 가장 끝에 담는다.           |
| boolean | addAll(int index, Collection<? extends E> c | 매개 변수로 넘어온 컬렉션 데이터를 index에 지정된 위치부터 담는다. |

## add(E e)
- 배열의 가장 끝에 데이터를 담는다.
- 데이터를 추가했을 때 리턴되는 boolean 값은 제대로 추가가 되었는지 여부를 말한다. false가 떨어지는 경우는 거의 없다.

## add(int index, E e)
- 지정된 위치에 데이터를 담는다. 지정된 위치에 있는 기존 데이터들은 하나씩 뒤로 밀려난다. 
```java
public void checkArrayList2() {
    ArrayList<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    list.add("C");
    list.add("D");
    list.add("E");
    list.add(1, "A1");

    for (String tempData : list) {
        System.out.println(tempData);
    }
}
```
- 여기서 1을 전체 크기보다 큰 숫자를 지정하면 IndexOutOfBoundsException 발생
```text
A
A1
B
C
D
E
```
- 위치를 1로 지정하고 add() 했으므로 기존에 있던 B부터 그 이후 모든 요소는 그 위치가 하나씩 뒤로 밀린다.
```java
public void checkArrayList3() {
    ArrayList<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    list.add("C");
    list.add("D");
    list.add("E");
    list.add(1, "A1");
    ArrayList<String> list2 = new ArrayList<>();
    list2.add("0 ");
    list2.addAll(list);

    for (String tempData : list2) {
        System.out.println("List2 " + tempData);
    }
}
```
- list2 라는 ArrayList 객체를 새로 만들고 addAll() 메소드를 사용하여 값을 추가했다. 그러면 "0 "을 추가한 list2 객체에 그 다음으로 list의 값들이 들어갈 것이다.
- 만약 list의 값을 list2에 복사할 일이 생긴다면 다음과 같이 생성자를 사용하면 편리하다.
```java
ArrayList<String> list2 = new ArrayList<>(list);
```
- 그리고 Collection 을 매개 변수로 갖는 생성자와 메소드는 왜 있을까. 먼저 다음과 같이 치환하면 어떻게 될까?
```text
public void checkArrayList4() {
    ArrayList<String> list = new ArrayList<>();
    list.add("A");

    ArrayList<String> list2 = list;
    list.add("Ooops");
    for (String tempData : list2) {
        System.out.println("list2 " + tempData);
    }
}
```
```text
list2 A
list2 Ooops
```
- list2에 값을 추가한 적은 없는데 list2에 값이 추가되었다.
- `list2 = list`에서 list2가 list의 값만 사용하겠다는 것이 아니다. list라는 객체가 생성되어 참조되고 있는 주소까지도 사용하겠다는 말이다.
즉, 두 객체의 변수는 다르지만, 하나의 객체가 변경되면 다른 이름의 변수를 갖는 객체의 내용도 바뀐다. 이와 같은 복사를 Shallow copy라고 하고,
이와는 다르게, 객체의 모든 값을 복사하여 복제된 객체에 있는 값을 변경해도 원본에 영향이 없도록 할 때에는 Deep copy를 수행한다. 예를 들어 배열을 복사할 때,
Syste 클래스에 있는 arraycopy() 와 같은 메소드를 이용하면 Deep copy를 쉽게 처리할 수 있다.
- 따라서, 하나의 Collection 관련 객체를 복사할 일이 있을 떄에는 이와 같이 생성자를 사용하거나, addAll() 메소드를 사용할 것을 권장한다.

# ArrayList에서 데이터를 꺼내자
- 먼저 ArrayList 객체에 들어가 있는 데이터늬 개수를 가져오는 size() 메소드가 있다. 리턴 타입은 int이다. 배열은 배열.length, String 문자열은 length()
메소드이다.
- 배열.length는 배열의 저장 공간 개수를 의미하지만, size() 메소드의 결과는 ArrayList의 저장 공간 개수가 아닌 들어가 있는 데이터 개수를 의미.
```text
public void checkArrayList5() {
    ArrayList<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    int listSize = list.size();
    for (int i = 0; i < listSize; i++) {
        System.out.println("list.get(" + i + ")=" + list.get(i));
    }
}
```
- 객체에 있는 값을 가져올 때에는 get() 메소드 사용. i 는 데이터의 위치
- 반대로 데이터로 위치를 찾아내는 indexOf() 와 lastIndexOf() 메소드가 있다.
```java
public void checkArrayList5() {
    ArrayList<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    list.add("A");
    // int listSize = list.size();
    // for (int i = 0; i < listSize; i++) {
    // 	System.out.println("list.get(" + i + ")=" + list.get(i));
    // }
    System.out.println(list.indexOf("A"));
    System.out.println(list.lastIndexOf("A"));
}
```
```text
0
2
```
- ArrayList는 중복 데이터를 넣을 수 있기 떄문에 앞에서부터 찾을 때에는 indexOf(), 뒤에서부터 찾을 때에는 lastIndexOf()
- 간혹 ArrayList 객체에 있는 데이터들을 배열로 뽑아내고 싶으면 toArray() 메소드가 있다.

| 리턴 타입    | 메소드 이름 및 매개 변수 | 설명                                             |
|----------|----------------|------------------------------------------------|
| Object[] | toArray()      | ArrayList 객체에 있는 값들을 Object[] 타입의 배열로 만든다.     |
| <T> T[]  | toArray(T[] a) | ArrayList 객체에 있는 값들을 매개 변수로 넘어온 T 타입의 배열로 만든다. |

- 매개 변수가 없는 toArray()는 Object 배열로만 리턴을 하므로, 제네릭을 사용하여 선언한 ArrayList 객체를 배열로 생성할 떄에는 두 번째 메소드를 권장한다.
```java
public void checkArrayList6() {
    ArrayList<String> list = new ArrayList<>();
    list.add("A");
    String[] strList = list.toArray(new String[0]);
    System.out.println(strList[0]);
}
```
```text
A
```
- 여기서 매개변수 new String[0]을 유심히 보아야 한다. 매개 변수로 넘기는 배열은 그냥 이와 같이 의미 없이 타입만을 지정하기 위해 사용할 수 있다.
- 그런데, 실제로는 매개 변수로 넘긴 객체에 값을 담아준다. 하지만, ArrayList 객체의 데이터 크기가 매개 변수로 넘어간 배열 객체의 크기보다 클 경우에는
매개 변수로 배열의 모든 값이 null로 채워진다. 그러므로 이처럼 사용하는 것이 좋다.
```java
public void checkArrayList7() {
    ArrayList<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    list.add("C");
    String[] tempArray = new String[3];
    String[] strList = list.toArray(tempArray);
    for (String temp : strList) {
        System.out.println(temp);
    }
}
```
```text
A
B
C
```
- 정상적으로 결과가 출력된다.
- 이번에는 tempArray 배열 크기를 5로 지정하고 결과를 확인해보자.
```java
String[] tempArray = new String[5];
```
```text
A
B
C
null
null
```

# ArrayList에 있는 데이터를 삭제하자
| 리턴 타입   | 메소드 이름 및 매개 변수             | 설명                                        |
|---------|----------------------------|-------------------------------------------|
| void    | clear()                    | 모든 데이터를 삭제한다.                             |
| E       | remove()                   | 매개 변수에서 지정한 위치에 있는 데이터를 삭제하고, 삭제한 데이터를 리턴 |
| boolean | remove(Object o)           | 매개 변수에 넘어온 객체와 동일한 첫 번째 데이터를 삭제           |
| boolean | removeAll(Collection<?> c) | 매개 변수로 넘어온 컬렉션 객체에 있는 데이터와 동일한 모든 데이터를 삭제 |  

- clear() 메소드는 말 그대로 ArrayList의 데이터들을 깨끗이 지운다.
- remove() 메소드는 get() 메소드처럼 지정한 위치의 데이터를 리턴하지만 데이터를 지운다.
- 매개 변수가 있는 remove()와 removeAll() 은 거의 동일한 기능을 하는 것처럼 보일 수 있지만 약간 다르다. 객체를 넘겨주는 remove()는 매개 변수로
넘어온 객체와 동일한 첫 번째 데이터만 삭제한다. 하지만 removeAll() 메소드는 매개 변수로 넘어온 컬렉션에 있는 데이터와 동일한 모든 데이터를 삭제한다.
```java
public void checkArrayList8() {
    ArrayList<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    list.add("C");
    list.add("A");
    System.out.println("Removed " + list.remove(0));
    // System.out.println(list.remove("A"));
    // ArrayList<String> temp = new ArrayList<>();
    // temp.add("A");
    // temp.add("B");
    // System.out.println(list.removeAll(temp));

    for (int i = 0; i < list.size(); i++) {
        System.out.println("list.get(" + i + ")=" + list.get(i));
    }
}
```
```text
Removed A
list.get(0)=B
list.get(1)=C
list.get(2)=A
```
- index를 매개변수로 가지는 remove()는 0번째 값을 삭제하고 해당 값을 리턴한다.
```java
public void checkArrayList8() {
    ArrayList<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    list.add("C");
    list.add("A");
    // System.out.println("Removed " + list.remove(0));
    System.out.println(list.remove("A"));
    // ArrayList<String> temp = new ArrayList<>();
    // temp.add("A");
    // temp.add("B");
    // System.out.println(list.removeAll(temp));

    for (int i = 0; i < list.size(); i++) {
        System.out.println("list.get(" + i + ")=" + list.get(i));
    }
}
```
```text
true
list.get(0)=B
list.get(1)=C
list.get(2)=A
```
- remove(Object o)의 경우 해당 객체 "A"를 삭제하고 true값을 리턴한다. 가장 마지막에 있는 "A"는 삭제되지 않았다.
```java
public void checkArrayList8() {
    ArrayList<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    list.add("C");
    list.add("A");
    // System.out.println("Removed " + list.remove(0));
    // System.out.println(list.remove("A"));
    ArrayList<String> temp = new ArrayList<>();
    temp.add("A");
    temp.add("B");
    System.out.println(list.removeAll(temp));

    for (int i = 0; i < list.size(); i++) {
        System.out.println("list.get(" + i + ")=" + list.get(i));
    }
}
```
```text
true
list.get(0)=C
```
- "A"값을 가진 모든 데이터가 사라졌고, "B"값 또한 삭제되었다.
- 마지막으로 객체에 있는 값을 변경하는 메소드를 살펴보자

| 리턴 타입 | 메소드 이름 및 매개 변수            | 설명                                                              |
|-------|---------------------------|-----------------------------------------------------------------|
| E     | set(int index, E element) | 지정한 위치에 있는 데이터를 두 번째 매개 변수로 넘긴 값으로 변경한다. 그리고, 해당 위치에 있던 데이터를 리턴 |

- 이 메소드가 없다면 특정 위치에 있는 데이터를 삭제하고 (remove), 그 위치에 데이터를 넣어야 할 것이다(add). 이 메소드로 두 단계를 한 번에 끝낼 수 있다.
- 추가로 trimToSize() 메소드는 ArrayList 객체 공간의 크기를 데이터의 개수만큼으로 변경한다. 즉, String 클래스의 trim() 메소드가 앞뒤의 공백을 없애는 것처럼
저장할 수 있는 공간은 만들어 두었지만, 데이터가 저장되어 있지 않을 때 해당 공간을 없애버린다. 일반적으로 잘 사용하진 않지만, 이 객체를 원격으로 전송하거나
파일로 저장하는 일이 있을때, 데이터의 크기를 줄일 수 있다는 장점이 있다.
- 앞서 말했듯이 Vector는 Thread safe 하고, ArrayList는 그렇지 않은데, ArrayList를 여러 쓰레드에서 덤벼도 안전하게 만들려면 다음과 같이 객체를 
생성해야만 한다.
```text
List list = Collections.synchronizedList(new ArrayList(...));
```

# Stack 클래스
- List 인터페이스를 구현한 또 하나의 클래스인 Stack 클래스는 LIFO (Last In First Out) 기능을 구현하려고 할 때 필요한 클래스다.
- 하지만 LIFO 기능은 더 빠른 ArrayDeque 라는 클래스가 존재한다. 하지만 쓰레드에 안전하지 못하다. 쓰레드에 안전한 LIFO 기능을 원한다면 이 Stack 클래스를
사용하면 된다. 먼저 간단하게 Stack 클래스의 상속관계를 살펴보자
```text
java.lang.Object
    - java.util.AbstractCollection<E>
        - java.util.AbstractList<E>
            - java.util.Vector<E>
                - java.util.Stack<E>
```
- Stack 클래스의 부모 클래스는 Vector 이다.
- Stack에서 구현한 인터페이스는 Serializable, Cloneable, Iterable<E>, Collection<E>, List<E>, RandomAccess로 ArrayList와 동일하다.
- Stack 클래스는 자바에서 상속을 잘못 받은 클래스다. JDK 1.0부터 존재했기 때문에 원래의 취지인 LIFO를 생각한다면 Vector에 속해서는 안 된다. 하지만
자바의 하위 호환성을 위해 이 상속관계를 계속 유지하고 있다.
- 생성자는 단 하나다.

| 생성자     | 설명                        |
|---------|---------------------------|
| Stack() | 아무 데이터도 없는 Stack 객체를 만든다. |

- 메소드는 다음과 같다.

| 리턴 타입   | 메소드 이름 및 매개 변수   | 설명                        |
|---------|------------------|---------------------------|
| boolean | empty()          | 객체가 비어있는지를 확인             |
| E       | peek()           | 객체의 가장 위에 있는 데이터를 리턴      |
| E       | pop()            | 객체의 가장 위에 있는 데이터를 지우고, 리턴 |
| E       | push(E item)     | 매개 변수로 넘어온 데이터를 가장 위에 저장  |
| int     | search(Object o) | 매개 변수로 넝머온 데이터의 위치를 리턴    |

```java
package vol2._22.collection;

import java.util.Stack;

public class StackSample {
	public static void main(String[] args) {
		StackSample sample = new StackSample();
		sample.checkPeek();
	}

	public void checkPeek() {
		Stack<Integer> intStack = new Stack<Integer>();
		for (int i = 0; i < 5; i++) {
			intStack.push(i);
			System.out.println(intStack.peek());
		}
		System.out.println("size=" + intStack.size());
	}
}
```
```text
0
1
2
3
4
size=5
```
- pop()의 경우는 다음과 같다.
```text
public void checkPop() {
    Stack<Integer> intStack = new Stack<Integer>();
    for (int i = 0; i < 5; i++) {
        intStack.push(i);
        System.out.println(intStack.pop());
    }
    System.out.println("size=" + intStack.size());
}
```
```text
0
1
2
3
4
size=0
```
