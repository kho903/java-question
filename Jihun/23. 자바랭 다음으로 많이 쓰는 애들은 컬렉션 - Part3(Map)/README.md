# Map 이란?
- 키(Key)와 값(Value)로 이루어져 있다. Map에서 다른 데이터와 구분하기 위한 값의 이름을 키라고 하는 이유를 생각해보자. 예를들어 핸드폰 번호와 핸드폰 주인과의
관계를 생각해보면, 한 사람이 여러 대의 핸드폰을 보유하기도 하지만 한 핸드폰 번호를 사용하는 사람은 한 사람 뿐이다. 따라서 한 핸드폰 번호의 주인은 단지 한 명이다.
자바의 Map에서 전화번호가 바로 키에 해당하고, 주인이 바로 값이 해당한다. 즉, 자바의 Map은 키와 값이 1:1로 저장된다. 키는 해당 Map에서 중복되지 않는다.
키가 다르고, 값이 동일하다면 맵에서는 다른 것으로 간주한다.
- 정리하면,
1. 모든 데이터는 키와 값이 존재한다.
2. 키가 없이 값만 저장될 수는 없다.
3. 값 없이 키만 저장할 수도 없다.
4. 키는 해당 Map에서 고유해야만 한다.
5. 값은 Map에서 중복되어도 전혀 상관 없다. 왜냐하면, 여러 이유로 한 대 이상의 핸드폰을 개통하여 사용하는 사람은 존재할 가능성이 매우 많기 때문이다. 

- Map은 java.util 패키지의 Map 이라는 이름의 인터페이스로 선언되어 있고, 구현해 놓은 클래스들도 많이 있다. 주요 메소드는 다음과 같다.

| 리턴 타입               | 메소드 이름 및 매개 변수                          | 설명                                               |
|---------------------|-----------------------------------------|--------------------------------------------------|
| V                   | put(K key, V value)                     | 첫 번째 매개 변수인 키를 갖는, 두 번째 매개 변수인 값을 갖는 데이터를 저장한다.  |
| void                | putAll(Map<? extends K, ? extends V> m) | 매개 변수로 넘어온 Map의 모든 데이터를 저장한다.                    |
| V                   | get(Object key)                         | 매개 변수로 넘어온 키에 해당하는 값을 넘겨준다.                      |
| V                   | remove(Object key)                      | 매개 변수로 넘어온 키에 해당하는 값을 넘겨주며, 해당 키와 값은 Map에서 삭제한다. |
| Set<K>              | keySet()                                | 키의 목록을 Set 타입으로 리턴한다.                            |
| Collection<V>       | values()                                | 값의 목록을 Collection 타입으로 리턴한다.                     |
| Set<Map.Entry<K,V>> | entrySet()                              | Map 안에 Entry 라는 타입의 Set을 리턴한다.                   |
| int                 | size()                                  | Map의 크기를 리턴한다.                                   |
| void                | clear(0                                 | Map의 내용을 지운다.                                    |
- Map을 사용할 때 꼭 기억해야 하는 메소드는 다음과 같다.
  - Map에 데이터를 넣는 put()
  - 데이터를 확인하는 get()
  - 데이터를 삭제하는 remove()

# Map을 구현한 주요 클래스들
- Map 인터페이스를 구현한 클래스는 다양하고 많다. HashMap, TreeMap, LinkedHashMap 등이 가장 유명하고 애용한다. 그리고, Hashtable이라는 클래스도 있다.
- Hashtable은 Map 인터페이스를 구현하기는 했지만, Map 인터페이스를 구현한 다른 클래스들과는 다르다. 다른 점은 다음과 같다.
  - Map은 컬렉션 뷰(Collection view)를 사용하지만, Hashtable은 Enumeration 객체를 통해서 데이터를 처리한다.
  - Map은 키, 값, 키-값 쌍으로 데이터를 순환하여 처리할 수 있지만, Hashtable은 이 중에서 키-값 쌍으로 데이터를 순환하여 처리할 수 없다.
  - Map은 이터레이션을 처리하는 도중에 데이터를 삭제하는 안전한 방법을 제공하지만, Hashtable은 그러한 기능을 제공하지 않는다.
- 다음과 같은 중요한 차이가 있다.

| 기능                  | HashMap | Hashtable |
|---------------------|---------|-----------|
| 키와 값에 null 저장 가능 여부 | 가능      | 불가능       |
| 여러 쓰레드 안전 여부        | 불가능     | 가능        |

- 왜 이러한 차이가 발생했을까? Collection은 JDK 1.2 부터 추가되었다. HashMap, TreeMap이라는 클래스도 이 때 만들어졌다. LinkedHashMap은 JDK 1.4에서
추가되었다. 그런데 Hashtable이라는 클래스는 JDK 1.0 부터 만들어져 사용된 클래스이기 때문에 Map 인터페이스의 기능을 구현한 것은 JDK 1.2 부터이다.
즉, Map 에 맞추어 보완되었다고 보면 된다.
- 중요한 점은 Hashtable을 제외한 Map으로 끝나는 클래스들을 여러 쓰레드에서 동시에 접근하여 처리할 필요가 있을 때에는 다음과 같이 선언하여 사용해야만 한다.
```java
Map m = Collections.synchronizedMap(new HashMap(...));
```
- 추가로 JDK 1.0부터 제공되는 Hashtable, Vector는 클래스가 쓰레드에 안전하게 개발되어 있다. 그 외의 JDK 1.2부터 제공되는 대부분의 Collection 관련
클래스들은 이와 같은 처리를 하거나, Concurrent가 포함된 이름이어야만 쓰레드에 안전하게 사용할 수 있다. (예 : JDK 1.5에 추가된 ConcurrentHashMap,
CopyOnWriteArrayList 등, java.util.concurrent 패키지 소속)

# HashMap
- HashMap은 다음과 같은 상속 관계를 가진다.
```text
java.lang.Object
    - java.util.AbstractMap<K, V>
        - java.util.HashMap<K, V>
```
- HashMap 클래스는 AbstractMap 이라는 abstract 클래스를 확장했으며, 대부분의 주요 메소드는 AbstractMap 클래스가 구현해 놓앗다.

| 인터페이스        | 용도                                                            |
|--------------|---------------------------------------------------------------|
| Serializable | 원격으로 객체를 전송하거나, 파일에 저장할 수 있음을 지정                              |
| Cloneable    | Object 클래스의 clone() 메소드가 제대로 수행될 수 있음을 지정. 즉, 복제가 가능한 객체임을 의미 |
| Map<E>       | 맵의 기본 메소드 지정                                                  |

- 생성자는 4개가 있다.

| 생성자                                      | 설명                                                             |
|------------------------------------------|----------------------------------------------------------------|
| HashMap()                                | 16개의 저장 공간을 갖는 HashMap 객체를 생성한다.                               |
| HashMap(int initialCapacity)             | 매개 변수만큼의 저장 공간을 갖는 HashMap 객체를 생성한다.                           |
| HashMap(int initialCapacity)             | 첫 번째 매개 변수의 저장 공간을 갖고, 두 번째 매개 변수의 로드 팩터를 갖는 HashMap 객체를 생성한다. |
| HashMap(Map<? extends K, ? extends V> m) | 매개 변수로 넘어온 Map을 구현한 객체에 있는 데이터를 갖는 HashMap 객체를 생성한다.           |

- 매개 변수가 없는 생성자를 주로 사용한다. 하지만, 담을 데이터가 많을 경우 초기 크기를 지정해 주는 것을 권장한다.
- 추가로, HashMap에 저장하는 키가 되는 객체를 직접 만들었을 때에는 유의점이 있다. 키는 기본, 참조 자료형 모두 될 수 있다. 보통 int, long 같은 숫자나
String 클래스를 키로 많이 사용한다. 하지만 직접 어떤 클래스를 만들어 그 클래스를 키로 사용할 때에는 Object 클래스의 hashCode(), equals() 메소드를
잘 구현해 놓아야만 한다.
- HashMap에 객체가 들어가면 hashCode()의 결과 값에 따른 버켓(bucket)이라는 목록(list) 형태의 바구니가 만들어진다. 만약 서로 다른 키가 저장되었는데,
hashCode() 결과가 동일하다면, 이 버켓에 여러 개의 값이 들어갈 수 있다. 따라서, get() 메소드가 호출되면 hashCode() 결과를 확인하고, 버켓에 들어간
목록에 데이터가 여러 개일 경우 equals() 메소드를 호출하여 동일한 값을 찾게 된다. 따라서 개발툴에서 제공하는 hashCode(), equals()를 구현해 놓는 것을
권장한다. ("java map buckets" 검색 필)

# HashMap 객체에 값을 넣고 확인해보자.
- Collection에서 데이터를 추가하는 것은 add() 였다. 말 그래도 데이터가 추가되어서 add()이다.
- Map 에서는 추가한다고 표현하지 않고, 데이터를 넣는다고 표현하여 put() 이라는 메소드를 사용한다.
```java
package vol2._24;

import java.util.HashMap;

public class MapSample {
	public static void main(String[] args) {
		MapSample sample = new MapSample();
		sample.checkHashMap();
	}

	public void checkHashMap() {
		HashMap<String, String> map = new HashMap<>();
		map.put("A", "a");
		System.out.println(map.get("A"));
		System.out.println(map.get("B"));
	}
}
```
```text
a
null
```
- 키가 "A", 값이 "a"인 키-값을 put 해 주었다. get() 메소드로 A라는 키를 갖는 값과 B라는 키를 갖는 값을 출력하도록 했다. 존재하지 않는 키일 경우
null을 반환하였다. Collection에서는 해당 위치에 값이 없을 떄에는 ArrayIndexOutOfBoundsException 예외가 발생한다.
- 이미 존재하는 키로 put()을 할 경우, 기존의 값을 새로운 값으로 대치한다.
```java
public void checkHashMap() {
    HashMap<String, String> map = new HashMap<>();
    map.put("A", "a");
    map.put("A", "a1");
    System.out.println(map.get("A"));
    System.out.println(map.get("B"));
}
```
```text
a1
null
```

# HashMap 객체의 값을 확인하는 다른 방법들
```java
public void checkKeySet() {
    HashMap<String, String> map = new HashMap<>();
    map.put("A", "a");
    map.put("C", "c");
    map.put("D", "d");
    Set<String> keySet = map.keySet();
    for (String tempKey : keySet) {
        System.out.println(tempKey + "=" + map.get(tempKey));
    }
}
```
```text
A=a
C=c
D=d
```
- keySet() 메소드로 Set 타입으로 모든 키를 가져올 수 있다. Set의 제네릭 타입은 키의 제네릭 타입과 동일하게 지정
- 위 코드에서 keySet 이라는 Set 타입의 변수를 선언하고, map의 keySet() 메소드 수행 결과를 할당하고 for 루프로 get() 메소드를 사용해 출력하였다.
- 결과의 순서는 일정하지 않을 것이다. Set, Map은 데이터 추가 순서가 중요하지 안다. Set은 데이터가 중복되지 않는 것이 중요하고, Map은 키가 중복되지
않는 것이 중요하다. 따라서 데이터를 저장한 순서대로 결과가 출력되지는 않는다.
- 키가 아닌 값만 필요할 경우에는 keySet()이 아닌 values() 메소드로 바로 얻어올 수 있다.
```java
public void checkValues() {
    HashMap<String, String> map = new HashMap<>();

    map.put("A", "a");
    map.put("C", "c");
    map.put("D", "d");

    Collection<String> value = map.values();
    for (String tempValue : value) {
        System.out.println(tempValue);
    }
}
```
```text
a
c
d
```
- values() 메소드를 사용하면 HashMap에 담겨 있는 값의 목록을 Collection 타입의 목록으로 리턴해 준다.
- 이렇게 데이터를 꺼내는 방법 외에 entrySet() 메소드를 사용할 수도 있다. 이 메소드는 Map에 선언된 Entry 타입의 객체를 리턴한다.
Entry에는 단 하나의 키와 값만이 저장된다. 따라서, getKey()와 getValue() 라는 메소드를 사용하면 키와 값을 간단하게 가져올 수 있다.
```java
public void checkHashMapEntry() {
    HashMap<String, String> map = new HashMap<>();
    map.put("A", "a");
    map.put("B", "b");
    map.put("C", "c");
    map.put("D", "d");

    Set<Map.Entry<String, String>> entries = map.entrySet();
    for (Map.Entry<String, String> tempEntry : entries) {
        System.out.println(tempEntry.getKey() + "=" + tempEntry.getValue());
    }

}
```
```text
A=a
B=b
C=c
D=d
```
- map의 entrySet() 메소드를 호출하면 Set 타입으로 리턴하며, 그 Set 내에는 Entry 타입으로 데이터가 저장된다. 
- 이처럼 Map에 있는 데이터를 꺼낼 때에는 목적에 맞는 메소드를 적절히 선택하는 것이 중요하다.
```text
public void checkContains() {
    HashMap<String, String> map = new HashMap<>();

    map.put("A", "a");
    map.put("B", "b");
    map.put("C", "c");
    map.put("D", "d");

    System.out.println(map.containsKey("A"));
    System.out.println(map.containsKey("Z"));
    System.out.println(map.containsValue("a"));
    System.out.println(map.containsValue("z"));
}
```
```text
true
false
true
false
```
- Map에 어떤 키나 값이 존재하는지 확인하는 containsKey(), containsValue() 메소드도 존재한다. 모두 boolean 타입 리턴.
- 무작정 get() 메소드로 해당 키나 값이 존재하는지 확인하는 것보다는 이렇게 containsKey()나 containsValue() 메소드를 사용하는 것이 효과적이다.
```text
public void checkRemove() {
    HashMap<String, String> map = new HashMap<>();
    map.put("A", "a");
    map.remove("A");
    System.out.println(map.size());
}
```
```text
0
```
- 데이터를 삭제하는 remove() 메소드는 이와 같이 삭제하려는 데이터를 매개 변수로 넘겨주면 된다.

# 정렬된 키의 목록을 원한다면 TreeMap을 사용하자
- HashMap 객체의 키를 정렬하려면 가장 간단한 방법 중 하나가 Arrays 클래스를 사용하는 것이다. 하지만 불필요한 객체가 생긴다는 단점이 있다.
- 이러한 단점을 보완하는 TreeMap 이라는 클래스는 저장하면서 키를 정렬한다. 정렬 순서는 숫자 -> 알파벳 대문자 -> 소문자 -> 한글로 String과 같은
문자열과 같다.객체가 저장되거나 숫자가 저장될 때에는 그 순서가 달라진다.
```java
package vol2._24;

import java.util.Map;
import java.util.Set;
import java.util.TreeMap;

public class TreeMapSample {
	public static void main(String[] args) {
		TreeMapSample sample = new TreeMapSample();
		sample.checkTreeMap();
	}

	public void checkTreeMap() {
		TreeMap<String, String> map = new TreeMap<>();
		map.put("A", "a");
		map.put("가", "e");
		map.put("1", "f");
		map.put("a", "g");
		Set<Map.Entry<String, String>> entries = map.entrySet();
		for (Map.Entry<String, String> tempEntry : entries) {
			System.out.println(tempEntry.getKey() + "=" + tempEntry.getValue());
		}
	}
}
```
```text
1=f
A=a
a=g
가=e
```
- TreeMap은 키를 정렬하여 저장하고, 키의 목록을 가져와서 출력해보면 정렬된 순서대로 제공되는 것을 볼 수 있다. 매우 많은 데이터일 때는 HashMap보다 느리지만,
100건, 1000건 정도의 데이터를 처리하고, 정렬 필요성을 느낀다면 HashMap 보다는 TreeMap을 사용하는 것이 더 유리하다.
- TreeMap이 키를 정렬하는 것은 SortedMap 이라는 인터페이스를 구현했기 때문이다. SortedMap을 구현한 클래스들은 모두 키가 정렬되어 있어야만 한다. 키가 정렬
되었을 때의 장점은 가장 앞에 있는 키 (firstKey()), 가장 뒤에 있는 키(lastKey()), 특정 키 뒤에 있는 키(higherKey()), 특정 키 앞에 있는 키
(lowerKey()) 와 같은 메소드를 제공해 준다는 것이다.

# Map을 구현한 Properties 클래스
- 이 클래스는 Hashtable을 확장(extends) 하였다. 따라서, Map 인터페이스에서 제공하는 모든 메소드를 사용할 수 있다. 기본적으로, 자바에서는 시스템의 속성을
이 클래스를 사용하여 제공한다.
```text
package vol2._24;

import java.util.Properties;
import java.util.Set;

public class PropertiesSample {
	public static void main(String[] args) {
		PropertiesSample sample = new PropertiesSample();
		sample.checkProperties();
	}

	public void checkProperties() {
		Properties prop = System.getProperties();
		Set<Object> keySet = prop.keySet();
		for (Object tempObject : keySet) {
			System.out.println(tempObject + "=" + prop.get(tempObject));
		}
	}
}
```

```text
...
...
java.runtime.name=OpenJDK Runtime Environment
file.encoding=UTF-8
java.vm.name=OpenJDK 64-Bit Server VM
java.vendor.version=Zulu11.48+21-CA
java.vendor.url.bug=http://www.azulsystems.com/support/
...
... 
```
- 실제 결과를 출력해보면 매우 많은 데이터가 출력된다. Hashtable을 확장한 클래스이기 떄문에 키/값 형태로 데이터가 저장되어 있다. 
- 시스템 여러 속성 중 자주 사용할 값들은 다음과 같다.

| 속성                      | 설명                |
|-------------------------|-------------------|
| user.language           | 사용자의 사용 언어        |
| user.dir                | 현재 사용중인 기본 디렉터리   |
| user.home               | 사용자 계정의 홈 디렉터리    |
| java.io.tmpdir          | 자바에서 사용하는 임시 디렉터리 |
| file.encoding           | 파일의 기본 인코딩        |
| sun.io.unicode.encoding | 유니코드 인코딩          |
| path.separator          | 경로 구분자            |
| file.separator          | 파일 구분자            |
| line.separator          | 줄(line) 구분자       |

- 대부분 디렉터리나 파일과 관련된 값으로, IO 관련 클래스의 상수로 정해져 있다. 
- Properites 클래스를 사용하는 이유는 추가로 제공하는 메소드를 사용하기 위해서이다.

| 리턴 타입 | 메소드 이름 및 매개 변수                                               | 설명                     |
|-------|--------------------------------------------------------------|------------------------|
| void  | load(InputStream inStream)                                   | 파일에서 속성을 읽는다.          |
| void  | load(Reader reader)                                          |                        |
| void  | loadFromXML(InputStream in)                                  | XML로 되어 있는 속성을 읽는다.    |
| void  | store(OutputStream out, String comments)                     | 파일에 속성을 저장한다.          |
| void  | store(Writer writer, String comments)                        |                        |
| void  | storeToXML(OutputStream os, String comment)                  | XML로 구성되는 속성 파일을 생성한다. |
| void  | storeToXML(OutputStream os, String comment, String encoding) |                        |
- comments라고 되어 있는 매개 변수들은 저장되는 속성 파일에 주석으로 저장된다.
- XML은 태그로 구성되어 있는 텍스트 문서를 의미한다. 서로 데이터를 주고 받을 때 표준을 정하기가 쉽다. 
- API 서비스와 같은 써비스는 어떤 데이터를 정해져 있는 XML 형태로 요청하면 정해져 있는 형태의 XML로 데이터를 받을 수 있는 기능을 제공한다.
  (요즘엔 json을 주로 사용).
- Properties 객체에는 우리가 사용하는 애플리케이션 속성값들의 데이터를 넣고, 빼고, 저장하고, 읽어들일 수 있다.
```java
public void saveAndLoadProperties() {
    try {
        String fileName = "test.properties";
        File propertiesFile = new File(fileName);
        FileOutputStream fos = new FileOutputStream(propertiesFile);
        Properties prop = new Properties();
        prop.setProperty("Writer", "Sangmin, Lee");
        prop.setProperty("WriterHome", "http://www.godOfJava.com");
        prop.store(fos, "Basic Properties ㄹile.");
        fos.close();

        FileInputStream fis = new FileInputStream(propertiesFile);
        Properties propLoaded = new Properties();
        propLoaded.load(fis);
        fis.close();
        System.out.println(propLoaded);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
```text
{WriterHome=www.godOfJava.com, Writer=Sangmin, Lee}
```
- test.properties
```text
#Basic Properties file.
#Mon Oct 17 16:52:13 KST 2022
WriterHome=http\://www.godOfJava.com
Writer=Sangmin, Lee
```
- .propeties 파일은 각종 설정 파일들이다. 이러한 파일들은 모두 이와 같은 형식의 파일로 되어 있다. 키는 왼쪽, 값은 오른쪽에 =으로 나누어 표시된다.
- 위에 두 줄은 #으로 시작하는 주석이다. store() 메소드를 사용할 때 지정한 주석이 여기에 저장된다. 다음으로는 XML로 지정해보자
```java
public void saveAndLoadPropertiesXML() {
    try {
        String fileName = "test.xml";
        File propertiesFile = new File(fileName);
        FileOutputStream fos = new FileOutputStream(propertiesFile);
        Properties prop = new Properties();
        prop.setProperty("Writer", "Sangmin, Lee");
        prop.setProperty("WriterHome", "http://www.godOfJava.com");
        prop.storeToXML(fos, "Basic XML file.");
        fos.close();

        FileInputStream fis = new FileInputStream(propertiesFile);
        Properties propLoaded = new Properties();
        propLoaded.loadFromXML(fis);
        fis.close();
        System.out.println(propLoaded);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
<comment>Basic XML file.</comment>
<entry key="WriterHome">http://www.godOfJava.com</entry>
<entry key="Writer">Sangmin, Lee</entry>
</properties>
```
- properties 태그 내에 다른 태그들이 감싸여 있다. 주석은 comments 태그, 각 키와 값은 entry 태그 내에 선언되어 있다.
- Propeties 클래스를 사용하면 여러 속성을 저장하고, 읽어 들이는 작업을 보다 쉽게 할 수 있다.

# 자바의 자료 구조 정리
- Collection을 구현한 것은 List, Set, Queue이며, Map은 별도의 인터페이스이다.
- 배열을 목록처럼 처리하기 위한 List의 대표적인 클래스는 ArrayList, LinkedList가 있으며 보통 ArrayList를 많이 사용한다.
- List처럼 목록을 처리하기는 하지만, 데이터의 중복이 없고, 순서가 필요 없는 Set의 대표적인 클래스는 HashSet, TreeSet, LinkedHashSet이 있다.
- 데이터가 들어온 순서대로 처리하기 위해서 사용하는 Queue의 대표적인 클래스는 LinkedList와 PriorityQueue 등이 있으며, LinkedList는 List에도
속하고, Queue에도 속하는 특이한 클래스이다.
- Map의 대표적인 클래스에는 HashMap, TreeMap, LinkedHashMap이 있으며, 사용용도에 따라 다르지만 HashMap을 주로 사용
- Map의 키 목록은 keySet() 으로 Set 타입의 데이터를 얻고, 값 목록은 values()로 Collection 타입의 데이터를 얻는다.
- Collection의 데이터를 처리하기 위해서는 for루프를 사용할 수 있지만, iterator() 메소드로 Iterator 객체를 얻어 각 데이터를 처리할 수도 있다.
- 