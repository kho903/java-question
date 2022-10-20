### IO와 NIO

**IO는 JVM을 기준으로 읽을 때에는 Input을, 파일로 쓰거나 외부로 전송할 때에는 Output이라는 용어를 사용한다.**

- 여기서 Input과 Output은 JVM 기준이라는 것을 꼭 기억하자.

- 패키지에서는 바이트 기반의 데이터를 처리하기 위해서 여러 종류의 스트림이라는 클래스를 제공한다.
- 읽는 작업은 InputStream 을 통해서, 쓰는 작업은 OutputStream을 통해서 작업하도록 되어 있다.

- 바이트가 아닌 char 기반의 문자열로만 되어있는 파일은 Reader와 Writer라는 클래스로 처리한다.

**NIO는 JDK 1.4부터는 보다 빠른 I/O를 처리하기 위해서 NIO 라는 것이 추가되었다.**

- NIO는 스트림 기반이 아니라 버퍼와 채널을 기반으로 데이터를 처리한다.
- Java7에서는 NIO2라는 것이 추가되었다.

---

### 자바의 File와 Files 클래스

: Java7부터는 NIO2가 등장하면서 java.nio.file 패키지에 있는 Files라는 클래스에 있는 메소스들을 대체하여 제공한다.

- File 클래스는 객체를 생성하여 데이터를 처리하는데 반하여, Files 클래스는 모든 메소드가 static 으로 선언되어 있기 때문에 별도의 객체를 생성할 필요가 없다는 차이가 있다.

File 객체 생성자들.

| constructor | description |
| --- | --- |
| File(File parent, String child) | 이미 생성되어 있는 File 객체(parent)와 그 경로의 하위 경로 이름으로 새로운 File 객체를 생성한다. |
| File(String pathname) | 지정한 경로 이름으로 File 객체를 생성한다. |
| File(String parent, String child) | 상위 경로(parent)와 하위 경로(child)로 File 객체를 생성한다. |
| File(URI uri) | URI에 따른 File 객체를 생성한다. |

### 디렉터리에 있는 목록을 살펴보기 위한 list 메소드들.

| method name & parameters | return type | description |
| --- | --- | --- |
| listRoots() | static File[] | JVM이 수행되는 OS에서 사용자인 파일 시스템의 루트(root) 디렉터리 목록을 File 배열로 리턴한다. static 메소드이므로, File 객체를 별도로 생성할 필요는 없다. |
| list() | String[] | 현재 디렉터리의 하위에 있는 목록을 String 배열로 리턴한다. |
| list(FilenameFilter filter) | String[] | 현재 디렉터리의 하위에 있는 목록 중, 매개 변수로 넘어온 filter의 조건에 맞는 목록을 string 배열로 리턴한다. |
| listFiles() | File[] | 현재 디렉터리의 하위에 있는 목록을 File 배열로 리턴한다. |
| listFiles(FileFilter filter) | File[] | 현재 디렉터리의 하위에 있는 목록 중, 매개변수로 넘어온 filter 의 조건에 맞는 목록을 File 배열로 리턴한다. |
| listFiles(FilenameFilter filter) | File[] | 현재 디렉터리의 하위에 있는 목록 중, 매개 변수로 넘어온 filter 의 조건에 맞는 목록을 File 배열로 리천한다. |

FileFilter 인터페이스에 선언되어 있는 메소드

| method name & parameters | return type | description |
| --- | --- | --- |
| accept(File pathName) | boolean | 매개 변수로 넘어온 File 객체가 조건에 맞는지 확인한다. |

FilenameFilter 인터페이스

| method name & parameters | return type | description |
| --- | --- | --- |
| accept(File dir, String name) | boolean | 매개 변수로 넘어온 디렉터리(dir)에 있는 경로나 파일 이름(name)이 조건에 맞는지 확인한다. |

JDK7 이상의 버전을 사용하면, 이 File 클래스를 사용하는 것보다는 java.nio.file 패키지에 있는 Files 클래스를  

사용하는 것이 보다 효과적이다.

---

### InputStream과 OutputStream 그리고 Closeable interface

InputStream OutputStream

- 자바의 I/O는 기본적으로 InputStream과 OutputStream 이라는 abtract 클래스를 통해서 제공된다.
    - InputStream : 어떤 대상의 데이터를 읽을 때 InputStream의 자식 클래스를 통해서 읽으면 된다.
        - InputStream에서 꼭 기억해야 되는 메소드 read()와 close() 이다.
            - 많이 사용하는 stream 3가지
        
        | class | description |
        | --- | --- |
        | File | 파일을 읽는 데 사용한다. 주로 우리가 쉽게 읽을 수 있는 텍스트 파일을 읽기 위한 용도라기보다 이미지와 같이 바이트 코드로 된 데이터를 읽을 때 사용한다. |
        | FileInputStream | 이 클래스는 다른 입력 스트림을 포괄하여, 단순히 InputStream 클래스가 Override 되어 있다. |
        | ObjectInputStream | ObjectInputStream으로 저장한 데이터를 읽는데 사용한다. |
        
        FileInputStream과 ObjectInputStream 은 객체를 생성해서 데이터를 처리하면 된다. 
        
        하지만 FilterInpustream 클래스의 생성하는 protected 로 선언되어 있기 때문에, 상속 받은 클래스
        
    - OutputStream : 어떤 대상의 데이터를 쓸 때 OutputStream의 자식 클래스를 통해서 쓰면된다.
        - Closeable과 Flushable 이라는 두개의 인터페이스가 구현되어 있다.

Closeable interface?

- 이 인터페이수에는 close()라는 메소드만 선언되어 있는데, 어떤 리소스를 열었던 간에 이 인터페이스를 구현하며 해당 리소스를 close() 메소드를 이용하여 닫으라는 것을 의미한다.

> FileInputStream과 ObjectStream은 객체를 생성해서 데이터를 처리하면 된다.
> 

> FilterInputStream 클래스의 생성자는 protected로 선언되어 있기 때문에, 상속 받은 클래스에서만 객체를 생성할 수 있다. 따라서, 이 클래스를 확장한 클래스들을 주의 깊게 볼 필요가 있다.
> 

---

### Reader 와 wrtier

- stream은 byte를 다루기 위한 것.
- Reader와 writer는 char 기반의 문자열을 처리 하기 위한 클래스.

> InputStream 이나 OutputStream 클래스들처럼 이 Reader 클래스도 abstract으로 선언되어 있다.
> 

Reader 를 확장한 주요 클래스.

```java
BufferedReader, CharArrayReader, FilterReader, InputStreamReader, PipedReader, StringReader
```

여기 있는 클래스를 중에서 BufferedReader와 InputStreamReader가 많이 사용된다. 

Writer

```java
public abstract class Writer 
extends Object
implements Appendable, Closeable, Flushable
```

Appendable 인터페이스 : java 5부터 추가되었으며, 각종 문자열을 추가하기 위해 선언되었다.

CharSequence 인터페이스 를 구현한 대표적인 클래스 : String, StringBuilder, StringBuffer

---

### 텍스트 파일을 쓰기/읽기.

1) 텍스트 파일 

FileWriter 클래스는 거의 사용할 일이 없다.

write에 있는 write()나 append 메소드들 사용하여 데이터를 쓰면 메소드를 호출했을 때마다 파일에 쓰기 때문에 매우 비효율적이다. 

이러한 단점을 보관하기 위해 BufferedWriter라는 클래스가 있다.

| constructor | desciprtion |
| --- | --- |
| BufferedWrtier(Writer out) | Writer 객체를 매개 변수로 받아 객체를 생성한다. |
| BufferedWriter(Writer out, int size) | Writer 객체를 매개 변수로 받아 객체를 생성한다. 그리고, 두 번째 매개 변수에 있는 size 를 사용하여, 버퍼의 크기를 정한다. |

규칙.

- finally 에서 close를 해야 한다.
- FileWriter, BufferedWriter 순으로 객체를 생성하고, 생성한 객체를 닦아줄 때에는 BufferedWrtier, FileWriter 순으로 닫아야만 한다.

2) 텍스트 파일 읽기

ex) 데이터 읽는 부분. 

```java
String data; 
while( (data=bufferedReader.readLine()) != null){
		System.out.println(data);
}
```

refactoring

Java 7에서 제공하는 Files 라는 클래스를 사용하면 한줄로 작성이 가능하다. 

```java
String data = new String(Files.readAllBytes(Paths.get(fileName)));
```
