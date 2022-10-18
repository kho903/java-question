**Collection을 확장한 3개의 인터페이스**

List, Set, Queue

---

### Set은 언제사용할까?

: 순서와 상관없이, 어떤 데이터가 존재하는지를 확인하기 위한 용도로 많이 사용한다.

- 중복을 방지하고 원하는 값을 확인하는 용도

<BR><BR>

◼ 자바에서 Set 인터페이스를 구현할 주요 클래스는

HashSet, TreeSet, LinkedHashSet 있다.

- HashSet : 순서가 전혀 필요 없는 데이터를 해시 테이블에 저장한다. Set 중 가장 성능이 좋다.
- TreeSet : 저장된 데이터 값에 따라서 정렬되는 데이터 셋이다. Red-black이라는 트리 타입으로 값이 저장되면, HashSet보다 약간 성능이 느리다.
- LinkedHashSet : 연결된 목록 타입으로 구현된 해시 테이블에 데이터를 저장한다. 저장된 순서에 따라서 값이 정렬된다. 대신 성능이 이 셋방에서 가장 나쁘다.

> 성능 차이가 발생하는 이유는 데이터 정렬 때문이다.
> 

*red black 트리란 ? 각 노드의 색을 붉은색 혹은 검정색으로 구분하여 데이터를 빠르고 쉽게 찾을 수 있는 구조의 이진 트리이다.

<BR><BR>  
  
### Set 과 equals()/ hashCode() ?

: Set 은 무엇보다도 데이터가 중복되는 것을 허용하지 않으므로, 데이터가 같은자리로 확인하는 작업은 Set의 핵심이다. 

- Equals()와 hashCode() 메소드를 구현하는 부분은 Set에서 매우 중요하다.

### HashSet interface?

| Interface | Description |
| --- | --- |
| serializable | 원격으로 객체를 전송하거나, 파일에 저장할 수 있음을 지정. |
| Cloneable | object 클래스의 clone() 메소드가 제대로 수행될 수 있음을 지정 즉, 복제가 가능한 객체임을 의미한다. |
| Iterable<E> | 객체가 foreach 문장을 사용할 수 있음을 지정. |
| Collection<E> | 여러 개의 객체를 하나의 객체에 담아 처리할 때의 메소드 지정. |
| Set<E> | 셋 데이터를 처리하는 것과 관련된 메소드 지정. |

### HashSet 의 4개 생성자

| Constructor | Description |
| --- | --- |
| HashSet() | 데이터를 저장할 수 있는 16개의 공간과 0.75의 로드 팩터를 갖는 객체를 생성한다. |
| HashSet<Collection<? Extends E> c | 매개 변수로 받은 컬렉션 객체의 데이터를 hashSet에 담는다. |
| HashSet(int initialCapacity) | 매개 변수로 받은 개수만큼의 데이터 저장 공간과 0.75 의 로드 팩터를 갖는 객체를 생성한다. |
| HashSet(int initialCapacity, float loadFactor) | 첫 매개 변수로 받은 개수만큼의 데이터 저장 공간과 두 번째 매개 변수로 받은 만큼의 로드 팩터를 갖는 객체를 생성한다. |

<br><br>
  
### 로드 팩터

*로드 팩터?

(데이터의 개수)/(저장공간)

만약 데이터 개수가 증가하여 로드 팩터보다 커지면, 저장 공간의 크기는 증가되고 해시 재정리 작업을 해야만 한다. 

- 데이터가 해시 재정리 작업에 들어가면, 내부에 갖고 있는 자료 구조를 다시 생성하는 단계를 거쳐야하므로 성능이 영향이 발생한다.
- 로드 팩터라는 값이 클수록 공간이 넉넉해지지만, 데이터를 찾는 시간은 증가한다. 따라서 초기 크기 공간 개수와 로드 팩터를 데이터의 크기를 고려하여 산정하는 것이 좋다.
- 왜냐하면 초기 크기가 (데이터의 개수)/ (로드 팩터) 보다 클 경우에는 데이터를 쉽게 찾기 위한 해시 재정리 작업이 발생하지 않기 때문이다. 따라서 대량의 데이터를 여기에 담아 처리할 때에는 초기 크기와 로드 팩터를 값을 조절해 가면서 가장 적당한 크기를 찾아야만 한다.

> 대부분의 초급 개발자분 들은 로드 팩터를 건드릴 필요가 거의 없으므로, 매개 변수가 없는 첫 번째 생성자를 사용하거나, 세 번째에 있는 초기 크기만 지정하는 생성자를 사용하면 된다.
> 

### HashSet의 주요 메소드

| method name / parameters | return type | description |
| --- | --- | --- |
| add(E e) | boolean | 데이터를 추가한다. |
| clear() | void | 모든 데이터를 삭제한다. |
| clone() | Object | HashSet 객체를 복제한다. 하지만, 담겨 있는 데이터들은 복제하지 않는다. |
| contains(Object o) | boolean | 지정한 객체가 존재하는지를 확인한다. |
| isEmpty() | boolean | 데이터가 있는지 확인한다. |
| Iterator() | Iterator<E> | 데이터를 꺼내기 위한 Iterator 객체를 리턴한다. |
| remove(Object o) | boolean | 매개 변수로 넘어온 객체를 삭제한다. |
| size() | int | 데이터의 개수를 리턴한다. |

---

### Queue는 왜 필요할까?

> LinkedList를 가장 쉽게 이해하려면 열차를 생각하자.
> 

- 배열의 중간에 있는 데이터가 지속적으로 삭제되고, 추가될 경우에는 LinkedList 배열보다 메모리 공간 측면에서 훨씬 더 유리하다.
- 그리고 LinkedList는 List 인터페이스뿐만 아니라 Queue와 Deque 인터페이스도 구현하고 있다. 즉 LinkedList 자체가 List이면서도 Queue, Deque도 된다.

### 웹 서버에서 100명의 사용자가 요청?

만약 LIFO로 처리해서 사용자에게 응답을 준다면, 가장 먼저 와서 줄 서 있는 사용자가 가장 마지막에 응답을 받게 될 것이다. 

### Deque

:LinkedList 클래스가 구현한 인터페이스 중에서 java6에서 추가된 Deque라는 것이 있다. 

- Deque는 Queue 인터페이스를 확장하였기 때문에 Queue의 기능을 전부 포함한다.
    - 맨 앞에 값을 넣거나 빼는 작업, 맨 뒤에 값을 넣거나 빼는 작업을 수행하는데 용이하도록 되어 있다.

### LinkedList 클래스가 구현한 인터페이스

| Interface | description |
| --- | --- |
| Serializable | 원격으로 객체를 전송하거나, 파일에 저장할 수 있음을 지정. |
| Clonable | Object 클래스의 clone() 메소드가 제대로 수행할 수 있음을 지정.
 즉, 복제가 가능한 객체임을 의미한다 |
| Iterable<E> | 객체가 ‘foreach’ 문장을 사용할 수 있음을 지정. |
| Collection<E> | 여러 개의 객체를 하나의 객체에 담아 처리할 때의 메소드를 지정 |
| Deque<E> | 맨 앞과 맨 뒤의 값을 용이하게 처리하는 큐와 관련된 메소드를 지정 |
| List<E> | 목록형 데이터를 처리하는 것과 관련된 메소드를 지정 |
| Queue<E> | 큘를 처리하는 것과 관련된 메소드 지정. |

### LinkedList의 생성자를 주요 메소드

| Constructor | description |
| --- | --- |
| LinkedList() | 비어 있는 LinkedList 객체를 생성한다. |
| LinkedList(Collection<? Extends E> c) | 매개 변수로 받은 컬렉션 객체의 데이터를 LInkedList에 담는다. |

---

### 정리

- Set : 중복된 데이터 처리
- Queue : 먼저 들어온 데이터를 먼저 처리해주는 FIFO 기능을 처리하기 위해 사용.
    
                  ( 여러 스레드에서 들어오는 작업을 순차적으로 처리할 때 많이 사용한다. )
