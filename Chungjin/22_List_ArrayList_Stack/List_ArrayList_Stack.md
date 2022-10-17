**“지금 배우는 컬렉션, 나중에 배울 쓰레드, IO, 네트워크 등의 관련 클래스를 사용할 때에는 한 번 정도 그 클래스의 상속 관계를 살펴보는 것이 좋다.**

**DATA CONSTRUCTOR**

- 순서가 있는 목록 LIST 형
- 순서가 중요하지 않은 셋 SET 형
- 먼저 들어온 것이 먼저 나가는 큐  Queue 형
- 키 -값 key - value 으로 저장되는 맵 Map 형

---

**Iterator () interface**

```java
public interface Collection<E> extends Iterable<E>
```

> **Iterator 라는 인터페이스에는 추가 데이터가 있는지 확인하는 hasNext() 메소드, <br>
>  현재 위치를 다음 요소로 넘기고 그 값을 리턴해주는 next() 라는 메소드, <br>
>  데이터를 삭제하는 remove() 메소드가 있다.**
> 

---

**List 인터페이스와 그 동생들.** 

> List 인터페이스를 구현한 클래스들은 매우 많다. 그 많은 클래스들 중에서 java.util 패키지에서는 ArrayList, Vector, Stack, LinkedList를 많이 사용한다.
> 

- ArrayList : Thread safe x
- Vector : Thread safe o
- Stack : Vector를 확장하여 만들어졌다.

---

### ArrayList 가 공통적으로 구현한 모든 인터페이스

```java
Serializable, Cloneable, Iterable<E>, Collection<E>, List<E>, RandomAccess
```

- Serializable : 원격으로 객체를 전송하거나, 파일에 저장할 수 있음을 지정.
- Cloneable : Object 클래스의 clone() 메소드가 제대로 수행될 수 있음을 지정. 즉, 복제가 가능한 객체임을 의미한다.
- Iterable<E> : 객체가 “foreach” 문장을 사용할 수 있음을 지정.
- Collection<E> : 여러 개의 객체를 하나의 객체에 담아 처리할 때의 메소드를 지정.
- List<E> : 목록형 데이터를 처리하는 것과 관련된 메소드 지정.
- RandomAccess : 목록형 데이터에 보다 빠르게 접근할 수 있도록 임의로(random)하게 접근하는 알고리즘이 적용된다는 것을 지정.

### ArrayList의 생성자.

> ArrayList는 앞 절에서 “확장 가능한” 배열이라고 말했다.
> 
- ArrayList() : 객체를 저장할 공간이 10개인 ArrayList를 만든다.
- ArrayList(Collection<? extends E> c) : 매개 변수로 넘어온 컬렉션 객체가 저장되어 있는 ArrayList를 만든다.
- ArrayList(int initialCapacity) : 매개 변수로 넘어온 initialCapacity 개수만큼의 저장 공간을 갖는 ArrayList를 만든다.

<br><br>

ArrayList 객체를 선언할 때 매개 변수를 넣지 않으면, 초기 크기가 10이다. <br>
따라서 10개 이상의 데이터가 들어가면 크기를 늘이는 작업이 ArrayList 내부에서 자동으로 수행된다.

이러한 작업이 수행되면, 애플리케이션 성능에 영향을 주게  된다. 만약 저장되는 데이터의 크게가 어느 정도 예측 가능하다면 다음과 같이 에측한 초기 크기를 지정할 것을 권장한다. 

<br><br>

> ArrayList는 대부분 서로 다른 종류의 객체를 하나의 배열에 넣지 않고, 한 가지 오류의 객체만 저장한다. <br>
여러 종류를 하나의 객체를 담을 때에도 되도록이면 필자가 계속 이야기하는 DTO라는 객체를 하나 만들어서 담는 것이 좋다. <br>
 그래서 컬렉션 관련 클래스의 객체들을 선언할 때에는 앞 장에서 배운 제네릭을 사용하여 선언하는 것을 권장한다.<br>
> 

---

### ArrayList 에 데이터를 담아보자.

> add(), addAll() 메소드만 알면 된다.
> 

| return type | method name and parameters | description  |
| --- | --- | --- |
| boolean | add(E e) | 매개변수로 넘어온 데이터를 가장 끝에 담는다. |
| void  | add(int index, E e) | 매개변수로 넘어온 데이터를 지정된 index 위치에 담는다. |
| boolean | addAll(Collection<? extends E> c | 매개변수로 넘어온 컬렉션 데이터를 가장 끝에 담는다. |
| boolean | addAll(int index, Collection<? extends E> e | 매개 변수로 넘어온 컬렉션 데이터를 데이터를 index에 지정된 위치부터 담는다. |

> ArrayList는 배열 확장된 배열 타입이기 때문에 배열처럼 순서가 매우 중요하다.
> 

---

### toArray() 메서드.

| return type | mehtod name and parameters | description |
| --- | --- | --- |
| Object[] | toArray() | ArrayList 객체에 있는 값들을 Object [] 태압의 배열로 만든다. |
| <T> T[] | toArray(T[] a) | ArrayList 객체에 있는 값들을 매개 변수로 넘어온 T 타입의 배열로 만든다. |

```java
public void checkArrayList6(){
		ArrayList<String> list = new ArrayList<String>();
		list.add("A");
		String[] strList = list.toArray(new String[0]));
		System.out.println(strList[0]);
}
```

---

### ArrayList and Vector

- ArrayList 는 Thread safe 하지 않다.
- Vector 는 Thread safe 하다.

**ArrayList를 Thread Safe 하게 만들고 싶다면? 아래와 같이 사용하자.** 

```java
List list = Collection.synchronizedList(new ArrayList(...));
```

---

### Stack 클래스와 ArrayDeque

속도 : stack < ArrayDeque 

Thread safe 

- stack  → 0
- ArrayDeque → x
