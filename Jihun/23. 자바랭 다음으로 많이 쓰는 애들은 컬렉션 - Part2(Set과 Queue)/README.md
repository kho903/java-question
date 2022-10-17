# Set이 왜 필요하지?
- Set은 순서에 상관 없이, 어떤 데이터가 존재하는지를 확인하기 위한 용도로 많이 사용된다. 중복을 방지하고, 원하는 값이 포함되어 있는지를 확인하는 것이 주용도.
- 어떤 값이 존재하는지, 없는지 여부만 중요할 때 Set을 사용하면 된다. Set 인터페이스를 구현한 주요 클래스는 HashSet, TreeSet, LinkedHashSet
  - HashSet : 순서가 전혀 필요 없는 데이터를 해시 테이블에 저장한다. Set 중 가장 성능이 좋다.
  - TreeSet : 저장된 데이터의 값에 따라서 정렬되는 셋. red-black 트리 타입으로 값이 저장되며, HashSet 보다 약간 느림
  - LinkedHashSet : 연결된 목록 타입으로 구현된 해시 테이블에 데이터를 저장. 저장된 순서에 따라서 값이 정렬된다. 대신 성능이 셋 중 제일 느리다.
- 데이터의 장렬때문에 성능 차이가 발생한다.
- red-black 트리는 각 노드의 색을 붉은 색 혹은 검은색으로 구분하여 데이터를 빠르고, 쉽게 찾을 수 있는 구조의 이진 트리.

# HashSet
- 상속 관계
```text
java.lang.Object
    - java.util.AbstractCollection<E>
        - java.util.AbstractSet<E>
            - java.util.HashSet<E>
```
- AbstractCollection을 확장한 것은 ArrayList와 동일. 하지만, AbstractSet을 확장했다. 구현되어 있는 메소드는 Object 클래스에 선언되어 있는
equals(), hashCode() 메소드와 이 클래스에서 추가한 removeAll()
- Set은 무엇보다도 데이터가 중복되는 것을 허용하지 않으므로, 데이터가 같은지를 확인하는 작업은 Set의 핵심이다. 그리고, equals() 는 hashCode()는 
Set에서 매우 중요하다. removeAll()은 컬렉션을 매개 변수로 받아 매개 변수 컬렉션에 포함된 모든 데이터를 지울 때 사용
- HashSet 은 다음과 같은 인터페이스를 구현했다.

| 인터페이스         | 용도                                                            |
|---------------|---------------------------------------------------------------|
| Serializable  | 원격으로 객체를 전송하거나, 파일에 저장할 수 있음을 지정                              |
| Cloneable     | Object 클래스의 clone() 메소드가 제대로 수행될 수 있음을 지정. 즉, 복제가 가능한 객체임을 의미 |
| Iterable<E>   | 객체가 "foreach" 문장을 사용할 수 있음을 지정                                |
| Collection<E> | 여러 개의 객체를 하나의 객체에 담아 처리할 때의 메소드 지정                            |
| Set<E>        | 셋 데이터를 처리하는 것과 관련된 메소드 지정                                     |

- Set은 순서가 없다. 따라서 순서가 매개 변수로 넘어가는 메소드나, 수행 결과가 데이터의 위치와 관련된 메소드는 Set 인터페이스에서 필요가 없다. 그러므로,
get(int index)나, indexOf(Object o)와 같은 메소드들은 Set에 존재하지 않는다.

# HashSet의 생성자
| 생성자                                | 설명                                                                  |
|------------------------------------|---------------------------------------------------------------------|
| HashSet()                          | 데이터를 저장할 수 있는 16개의 공간과 0.75 로드 팩터(load factor)를 갖는 객체를 생성한다.        |
| HashSet(Collection<? extends E> c) | 매개 변수로 받은 컬렉션 객체의 데이터를 HashSet에 담는다.                                |
| HashSet(int initialCapacity)       | 매개 변수로 받은 개수만큼의 데이터 저장 공간과 0.75 로드 팩터를 갖는 객체를 생성한다.                 |
| HashSet(int initialCapacity)       | 첫 매개 변수로 받은 개수만큼의 데이터 저장 공간과 두 번째 매개 변수로 받은 만큼의 로드 팩터를 갖는 객체를 생성한다. |

- 로드 팩터는 뭘까? (데이터의 개수) / (저장 공간)을 의미한다. 만약 데이터의 개수가 증가하여 로드 팩터보다 커지면, 저장 공간의 크기는 증가되고 해시 재정리 작업
  (rehash)을 해야만 한다. 데이터가 해시 재정리 작업에 들어가면, 내부에 갖고 있는 자료 구조를 다시 생성하는 단계를 거쳐야 하므로 성능에 영향이 발생
- 로드 팩터라는 값이 클수록 공간은 넉넉해지지만, 데이터를 찾는 시간은 증가한다. 따라서 초기 공간 개수와 로드 팩터는 데이터의 크기를 고려하여 산정하는 것이 좋다.
- 초기 크기가 (데이터의 개수) / (로드 팩터) 보다 클 경우에는 데이터를 쉽게 찾기 위한 해시 재정리 작업이 발생하지 않기 때문이다. 따라서 대량의 데이터를
여기에 담아 처리할 때에는 초기 크기, 로드 팩터의 값을 조절해 가면서 가장 적당한 크기를 찾아야만 한다.

# HashSet의 주요 메소드를 살펴보자
- 부모 클래스인 AbstractSet과 AbstractCollection에 선언/구현 되어 있는 메소드를 그대로 사용하는 경우가 많다.

| 리턴 타입       | 메소드 이름 및 매개 변수     | 설명                                           |
|-------------|--------------------|----------------------------------------------|
| boolean     | add(E e)           | 데이터를 추가한다.                                   |
| void        | clear(0            | 모든 데이터를 삭제한다.                                |
| Object      | clone()            | HashSet 객체를 복제한다. 하지만, 담겨 있는 데이터들은 복제하지 않는다. |
| boolean     | contains(Object o) | 지정한 객체가 존재하는지를 확인한다.                         |
| boolean     | isEmpty()          | 데이터가 있는지 확인한다.                               |
| Iterator<E> | iterator()         | 데이터를 꺼내기 위한 Iterator 객체를 리턴한다.               |
| boolean     | remove(Object o)   | 매개 변수로 넘어온 객체를 삭제한다.                         |
| int         | size()             | 데이터의 개수를 리턴한다.                               |

```java
package vol2._23.collection;

import java.util.HashSet;
import java.util.Set;

public class SetSample {
	public static void main(String[] args) {
		SetSample sample = new SetSample();
		String[] cars = new String[] {
			"Tico", "Sonata", "BMW", "Benz",
			"Lexus", "Mustang", "Grandeur",
			"The Beetle", "Mini Cooper", "i30",
			"BMW", "Lexus", "Carnival", "SM5",
			"SM7", "SM3", "Tico"
		};
		System.out.println(sample.getCarKinds(cars));
	}

	private int getCarKinds(String[] cars) {
		if (cars == null) return 0;
		if (cars.length == 1) return 1;
		Set<String> carSet = new HashSet<>();
		for (String car : cars) {
			carSet.add(car);
		}
		return carSet.size();
	}
}
```
```text
14
```
- HashSet과 같은 Set을 이용하면 여러 중복되는 값들을 걸러내는 데 매우 유용하다. 
- HashSet에 저장된 값은 다음과 같이 꺼낼 수 있다.
```text
public void printCarSet(Set<String> carSet) {
    for (String temp : carSet) {
        System.out.print(temp + " ");
    }
    System.out.println();
}
```
```text
Mustang Carnival Lexus Tico i30 Grandeur Sonata BMW Benz SM3 The Beetle SM5 Mini Cooper SM7 
14
```
- 결과를 보면 데이터를 Set에 저장한 순서대로 출력되지 않고, 뒤죽박죽이다. Set은 데이터가 보관되어 있는 순서가 중요하지 않을 때 사용해야 한다. 
- 이외 Iterator를 이용해 출력할 수도 있다.
```java
public void printCarSet2(Set<String> carSet) {
    Iterator<String> iterator = carSet.iterator();
    while (iterator.hasNext()) {
        System.out.print(iterator.next() + " ");
    }
    System.out.println();
}
```
```text
Mustang Carnival Lexus Tico i30 Grandeur Sonata BMW Benz SM3 The Beetle SM5 Mini Cooper SM7 
14
```
- Iterator 객체를 iterator() 메소드로 가져온 뒤, while문을 통해 hasNext() 로 다음 데이터가 존재하는지 지속적으로 확인하고,
next() 메소드를 사용하여 다음 값을 얻어낸다.
- HashSet에 존재하는지를 확인하기 위해서는 contains() 메소드, 해당 객체를 HashSet에서 삭제하기 위해서는 remove() 메소드를 사용하면 된다.

# Queue는 왜 필요할까?
- LinkedList는 A-B-C 가 있을 때, A는 뒤에 있는 값이 B라는 것만 알고, C가 있다는 것은 생각도 안한다. C역시 앞에 B라는 것만 기억한다.
- 이렇게 복잡한 LinkedList를 왜 쓸까? 간단하게 배열과 같이 데이터를 담아서 순차적으로 뺄 경우에는 필요가 없지만, 배열의 중간에 있는 데이터가
지속적으로 삭제되고, 추가될 경우에는 LinkedList가 배열보다 메모리 공간 측면에서 훨씬 유리하다. 배열(ArrayList or Vector)는 각 위치가 정해져 있고,
그 위치로 데이터를 찾는다. 그런데, 맨 앞의 값을 삭제하면, 그 뒤에 있는 값들은 하나씩 앞으로 위치를 이동해야 제대로 된 위치의 값을 가지게 된다.
- 그에 반해 LinkedList는 중간에 있는 데이터를 삭제하면, 지운 데이터의 앞의 데이터와 뒤의 데이터를 연결하면 그만이다. 위치를 맞추기 위해 값을 이동하는
단계를 거칠 필요가 없다는 것이다.
- 그리고, LinkedList는 List 인터페이스뿐만 아니라 Queue, Deque 인터페이스도 구현하고 있다.
- 큐(Queue)는 FIFO (First In First Out)으로 먼저 들어온 애가 먼저 나가는 것을 말한다. 큐는 사용자들의 요청을 들어온 순서대로 처리할 때 사용한다.

# LinkedList
- 상속 관계
```text
java.lang.Object
    - java.util.AbstractCollection<E>
        - java.util.AbstractList<E>
            - java.util.AbstractSequentialList<E>
                - java.util.LinkedList<E>
```
- ArrayList 클래스는 Vector와 상속 관계는 비슷하지만, AbstractSequentialList가 부모이다. AbstractList / AbstractSequentialList 차이점은
add(), set(), remove() 등의 메소드에 대한 구현 내용이 상이하다는 정도이다.
- LinkedList 클래스가 구현한 인터페이스의 목록은 다음과 같다.

| 인터페이스         | 용도                                                            |
|---------------|---------------------------------------------------------------|
| Serializable  | 원격으로 객체를 전송하거나, 파일에 저장할 수 있음을 지정                              |
| Cloneable     | Object 클래스의 clone() 메소드가 제대로 수행될 수 있음을 지정. 즉, 복제가 가능한 객체임을 의미 |
| Iterable<E>   | 객체가 "foreach" 문장을 사용할 수 있음을 지정                                |
| Collection<E> | 여러 개의 객체를 하나의 객체에 담아 처리할 때의 메소드 지정                            |
| Dequeue<E>    | 맨 앞과 맨 뒤의 값을 용이하게 처리하는 큐와 관련된 메소드 지정                          |
| List<E>       | 목록형 데이터를 처리하는 것과 관련된 메소드 지정                                   |
| Queue<E>      | 큐를 처리하는 것과 관련된 메소드 지정                                         |

# LinkedList의 생성자와 주요 메소드
- LinkedList는 일반적인 배열 타입의 클래스와 다르게 생성자로 객체를 생성할 때 처음부터 크기를 지정하지 않는다. 각 데이터들이 앞 뒤로 연결되는 구조이기
때문에, 미리 공간을 만들어 놓을 필요가 없어, 두 가지 생성자만 존재.

| 생성자                                   | 설명                                      |
|---------------------------------------|-----------------------------------------|
| LinkedList()                          | 비어 있는 LinkedList 객체를 생성한다.              |
| LinkedList(Collection<? extends E> c) | 매개 변수로 받은 컬렉션 객체의 데이터를 LinkedList에 담는다. |

- 객체에 추가하는 메소드는 다음과 같다.

| 리턴 타입   | 메소드 이름 및 매개 변수           | 설명                                                         |
|---------|--------------------------|------------------------------------------------------------|
| void    | addFirst(Object)         | LinkedList 객체의 가장 앞에 데이터를 추가한다.                            |
| boolean | offerFirst(Object)       |                                                            |
| void    | push(Object)             |                                                            |
| boolean | add(Object)              | LinkedList 객체의 가장 뒤에 데이터를 추가한다.                            |
| void    | addLast(Object)          |                                                            |
| boolean | offer(Object)            |                                                            |
| boolean | offerLast(Object)        |                                                            |
| void    | add(int, Object)         | LinkedList 객체의 특정 위치에 데이터를 추가한다.                           |
| Object  | set(int, Object)         | LinkedList 객체의 특정 위치에 있는 데이터를 수정한다. 그리고, 기존에 있던 데이터를 리턴한다. |
| boolean | addAll(Collection)       | 매개 변수로 넘긴 컬렉션의 데이터를 추가한다.                                  |
| boolean | addAll(int, Collection)  | 매개 변수로 넘긴 컬렉션의 데이터를 지정된 위치에 추가한다.                          |

```java
package vol2._23.collection;

import java.util.LinkedList;

public class QueueSample {
	public static void main(String[] args) {
		QueueSample sample = new QueueSample();
		sample.checkLinkedList();
	}

	public void checkLinkedList() {
		LinkedList<String> link = new LinkedList<>();
		link.add("A");
		link.addFirst("B");
		System.out.println(link);
		link.offerFirst("C");
		System.out.println(link);
		link.addLast("D");
		System.out.println(link);
		link.offer("E");
		System.out.println(link);
		link.offerLast("F");
		System.out.println(link);
		link.push("G");
		System.out.println(link);
		link.add(0, "H");
		System.out.println(link);
		System.out.println("EX=" + link.set(0, "I"));
		System.out.println(link);
	}
}
```
```text
[B, A]
[C, B, A]
[C, B, A, D]
[C, B, A, D, E]
[C, B, A, D, E, F]
[G, C, B, A, D, E, F]
[H, G, C, B, A, D, E, F]
EX=H
[I, G, C, B, A, D, E, F]
```
- 이 중에서 어떤 메소드를 대표적으로 사용하는 것이 좋을까? 자바의 LinkedList 소스를 보면 맨 앞에 추가하는 메소드는 동일한 기능을 수행하는 어떤 메소드를
호출해도 addFirst() 메소드를 호출한다. 맨 뒤에 추가하는 메소드는 동일한 기능을 수행하는 offer() 관련 메소드를 호출하면 add(), addLast() 메소드를
호출하도록 되어 있다. 따라서, 여러 메소드를 혼용하여 사용하면 이해하기 힘들기 때문에, 한가지만 선정하여 사용하는 것을 권장하며, add가 붙은 메소드를
사용하는 것이 오해의 소지가 가장 적다.
- 특정 위치의 데이터를 꺼내는 메소드는 다음과 같다.

| 리턴 타입  | 메소드 이름 및 매개 변수 | 설명                                   |
|--------|----------------|--------------------------------------|
| Object | getFirst()     | LinkedList 객체의 맨 앞에 있는 데이터를 리턴한다.    |
| Object | peekFirst()    |                                      |
| Object | peek()         |                                      |
| Object | element()      |                                      |
| Object | getLast()      | LinkedList 객체의 맨 뒤에 있는 데이터를 리턴한다.    |
| Object | peekLast()     |                                      |
| Object | get(int)       | LinkedList 객체의 지정한 위치에 있는 데이터를 리턴한다. |

- 맨 앞 데이터를 가져오는 메소드는 모두 내부적으로 getFirst() 메소드를 호출하므로, 이 메소드를 사용할 것을 권장한다.
- 추가로 LinkedList에 어떤 객체가 포함되어 있는지를 확인하는 메소드는 다음과 같다.


| 리턴 타입   | 메소드 이름 및 매개 변수      | 설명                                              |
|---------|---------------------|-------------------------------------------------|
| boolean | contains(Object)    | 매개 변수로 넘긴 데이터가 있을 경우 true를 리턴한다.                |
| int     | indexOf(Object)     | 매개 변수로 넘긴 데이터의 위치를 앞에서부터 검색하여 리턴한다. 없을 경우 -1 리턴 |
| int     | lastIndexOf(Object) | 매개 변수로 넘긴 데이터의 위치를 끝에서부터 검색하여 리턴한다. 없을 경우 -1 리턴 |

- 데이터를 삭제하는 메소드 목록은 다음과 같다. 여기에 있는 대부분의 삭제 관련 메소드들은 LinkedList 객체에서 삭제하고, 데이터를 리턴해주기 때문에 조회용
메소드보다는 이 메소드를 많이 사용한다.

| 리턴 타입   | 메소드 이름 및 매개 변수                | 설명                                                   |
|---------|-------------------------------|------------------------------------------------------|
| Object  | remove()                      | LinkedList 객체의 가장 앞에 있는 데이터를 삭제하고 리턴한다.              |
| Object  | removeFirst()                 |                                                      |
| Object  | poll()                        |                                                      |
| Object  | pollFirst()                   |                                                      |
| Object  | pop()                         |                                                      |
| Object  | pollLast()                    | LinkedList 객체의 가장 끝에 있는 데이터를 삭제하고 리턴한다.              |
| Object  | removeLast()                  |                                                      |
| Object  | remove(int)                   | 매개 변수에 지정된 위치에 잇는 데이터를 삭제하고 리턴한다.                    |
| boolean | remove(Object)                | 매개 변수로 넘겨진 객체와 동일한 데이터 중 앞에서부터 가장 처음에 발견된 데이터를 삭제한다. |
| boolean | removeFirstOccurrence(Object) |                                                      |
| boolean | removeLastOccurrence(Object)  | 매개 변수로 넘겨진 객체와 동일한 데이터 중 끝에서부터 가장 처음에 발견된 데이터를 삭제한다. |

- 삭제 관련 메소드들은 특정 데이터를 삭제하는 메소드를 제외하고는 모두 삭제된 데이터를 리턴한다. 맨 앞에 있는 데이터를 삭제하는 많은 메소드들은 모두
removeFirst() 메소드를 내부적으로 호출한다. 반대로 맨 뒤에 있는 데이터를 삭제하는 메소드들은 모두 removeLast() 메소드를 내부적으로 호출한다. 따라서
혼동을 피하려면 remove 가 붙은 메소드를 사용할 것을 권장.
- 이 외에도 크기를 확인하는 size(), 모든 데이터를 삭제하는 clear(), 데이터를 복제하는 clone(), 배열로 만드는 toArray() 가 있다.
- 다음으로 객체를 하나씩 검색하기 위한 Iterator 가 있다.

| 리턴 타입        | 메소드 이름 및 매개 변수       | 설명                                                   |
|--------------|----------------------|------------------------------------------------------|
| ListIterator | listIterator(int)    | 매개 변수에 지정된 위치부터의 데이터를 검색하기 위한 ListIterator 객체를 리턴한다. |
| Iterator     | descendingIterator() | LinkedList의 데이터를 끝에서부터 검색하기 위한 Iterator 객체를 리턴한다.    |

- ListIterator는 Iterator 인터페이스가 다음 데이터만을 검색할 수 있다는 단점을 보완하여, 이전 데이터도 검색할 수 있는 이터레이터다.
따라서 next() 외에도 previous() 메소드를 사용하면 이전 데이터를 확인할 수 있다.
