### Map 에 선언되어 있는 map의 주요 메소드.

| Method name & parameters | Return type | Description |
| --- | --- | --- |
| put(K key, V value) | V | 첫 번째 매개 변수인 키를 갖는 |
| putAll(Map ? Extends K, ? Extends V> m) | Void | 매개 변수로 넘어온 Map의 모든 데이터를 저장한다. |
| get(Object key) | V | 매개 변수로 넘어온 키에 해당하는 값을 넘겨준다. |
| remove(Object key) | V | 매개 변수로 넘어온 키에 해당하는 값을 넘겨주면, 해당 키와 값은 Map에서 삭제한다. |
| KeySet() | Set<K> | 키의 목록을 Set타입으로 리턴한다. |
| values() | Collection<K> | 값의 목록을 Collection 타입으로 리턴한다. |
| EntrySet() | Set<Map.Entry<K,V>> | Map 안에 Entry 라는 타입의 Set을 리턴한다. |
| Size() | int | Map의 크기를 리턴한다. |
| Clear() | void | Map의 내용을 지운다. |
- Map에 데이터를 넣는 Put()
- 데이터를 확인하는 get()
- 데이터를 삭제하는 remove()

---

### Map을 구현한 주요 클래스,

: HashMap, TreeMap, LinkedHashMap, Hashtable 등.

- Map은 컬렉션 뷰를 사용하지만, Hashtable은 Enumeration 객체를 통해서 데이터를 처리한다.
- Map은 키, 값, 키-값 쌍으로 데이터를 순환하여 처리할 수 있지만, Hashtable은 이중에서 키-값 쌍으로 데이터를 순환하여 처리할 수 없다.
- Map은 이터레이션을 처리하는 도중에 데이터를 삭제하는 안전한 방법을 제공하지만, Hashtable은 그러한 기능을 제공하지 않는다.

---

### HashMap.    vs.     Hashtable

| 기능 | HashMap | Hashtable |
| --- | --- | --- |
| 키나 값에 null 저장 가능 여부 | 가능 | 불가능 |
| 여러 스레드 안전 여부 | 불가능 | 가능 |

Hashtable을 제외한 Map으로 끝나는 클래스들을 여러 스레드에서 동시에 접근하여 처리할 필요가 있을 때는 아래와 사용 한다.

```java
Map m = Collections.synchronizedMap(new HashMap(...));
```

- 자바 기본 api 중에 jdk 1.0부터 제공되는 Hashtable, vector 는 클래스가 스레드에서 안전하게 개발되어 있다.
- 그 외의 jdk1.2부터 제공되는 대부분의 collection 관련 클래스들은 이와 같은 처리를 해약하거나 이름에 concurrent가 포함되어 있어야만 스레드에서 안전하게 사용

---

### HashMap 클래스

| Interface | description |
| --- | --- |
| serializable | 원격으로 객체를 전송하거나 파일에 저장할 수 있음을 지정 |
| Cloneable | Object 클래스의 clone() 메소드가 제대로 수행될 수 있음을 지정 즉, 복제가 가능한 객체임을 의미한다. |
| Map<E> | 맵의 기본 메소드 지정. |

### HashMap 생성자 4개

| 생성자 | 설명 |
| --- | --- |
| HashMap() | 16개의 저장 공간을 갖는 HashMap 객체를 생성한다. |
| HashMap(int initialCapacity) | 매개 변수만큼의 저장 공간을 갖는 HashMap 객체를 생성한다. |
| HashMap(int initialCapacity, float loadFactor) | 첫 매개 변수의 저장 공간을 갖고, 두 번째 매개 변수의 로드 팩터를 갖는 HashMap 객체를 생성한다. |
| HashMap(Map<? Extends K, ? Extends V> m) | 매개 변수로 넘어온 Map 을 구현한 객체에 있는 데이터를 갖는 HashMap 객체를 생성한다. |
- 대부분의 HashMap 객체를 생성할 때에는 매개변수가 없는 생성자를 사용한다. 하지만
- HashMap에 담을 데이터의 개수가 많은 경우에는 초기 크기를 지정해 주는 것을 권장한다.

### HashMap 사용 시 유의사항 ( java map buckets)

- HashMap에 저장하는 키가 되는 객체를 직접 만들었을 때에는 유의해야 하는 점이 있다 .
    - HashMap의 키는 기본 자료형과 참조자료형 모두 될 수 있다. 그래서, 보통은 int 나 long 같은 숫자나 string 클래스를 키로 많이 사용한다. 하지만, 여러분들이 직접 어떤 클래스를 만들어 그 클래스를 키로 사용할 때에는 Object 클래스의 hashCode() 메소드가 equals() 메소드를 잘 구현해 놓아야만 한다.
- HashMap에 객체가 들어가면 hashCode() 메소드를 결과 값에 따른 버켓이라는 목록 형태의 바구니가 만들어진다.
    - 만약 서로 다른 키가 저장되었는데, hashCode() 메소드의 결과가 동일하다면, 이 마켓에 여러 개의 값이 들어갈 수 있다.
        - 따라서 get() 메소드가 호출되면 hashCode()의 결과를 확인하고, 버켓에 들어간 목록에 데이터가 여러 개일 경우 equals() 메소드를 호출하여 동일한 값을 찾게 된다.
            - 따라서 키가 되는 객체를 직접 생성할 때에는 개발원에서 제공하는 hashCode()와 equals()를 자동으로 생성해줘요 기능을 사용하여 해당 메소드를 구현해 좋아야 한다.

---

### 정렬된 키의 목록을 원한다면 TreeMap

- 많은 데이터를 TreeMap을 이용하여 보관하여 처리할 때에는 HashMap보다 느릴 것이다.
    - 하지만 100, 1000건 정도의 데이터를 처리하고, 정렬을 해야 할 필요가 있다면, HashMap 보다는 TreeMap을 사용하는 것이 유리하다.

---

### Map을 구현한 Properties 클래스

: System 클래스에 대해서 살펴보면, Properties 라는 클래스가 있다. 

- 이 클래스는 Hashtable을 확장(extends)하였다. 따라서 Map 인터페이스에서 제공하는 모든 메소드를 사용할 수 있다.

시스템에서 제공하는 여러 속성중에서 앞으로 개발하면서 많이 사용할 값들. 

| 속성 | 설명 |
| --- | --- |
| user.language | 사용자의 사용 언어 |
| user.dir | 현재 사용자인 기본 디렉터리 |
| user.home | 사용자 계정의 홈 디렉터리 |
| java.io.tmpdir | 자바에서 사용하는 임시 디렉터리 |
| file.encoding | 파일의 기본 인코딩 |
| sun.io.Unicode.encoding | 유니코드 인코딩 |
| path.separator | 경로 구분자 |
| file.separator | 파일 구분자 |
| line.separator | 줄 구분자 |

### Properties 클래스를 왜 사용할까?

---

### 정리

- Collection을 구현한 것은 List, Set, Queue이며, Map은 별도의 인터페이스로 되어 있다.
- 배열처럼 목록을 처리하기 위한 List의 대표적인 클래스 : ArrayList, LinkedList
- List처검 목록을 처리하기는 하지만, 데이터의 중복이 없고, 순서가 필요 없는 Set의 대표 클래스 : HashSet, TreeSet, LinkedHashSet
- 데이터가 들어온 순서대로. 처리하기 위해서 사용하는 Queue의 대표적인 클래스는 LinkedList와 PriorityQueue 등이 있으며, LinkedList 는 List
- Map의 대표적인 클래스에는 HashMap, TreeMap, LinkedHashMap이 있으며, 사용 용도에 따라 다르겠지만 대부분 HashMap을 많이 사용한다.
- Collection 의 데이터를 처리하기 위해서는 for 루프를 사용할 수 있지만, iterator() 메소드를 통하여 iterator 객체를 얻어 각 데이터를 처리할 수도 있다.
