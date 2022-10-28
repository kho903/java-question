**FORK/JOIN** 

: CPU를 더 쉽게, 효율적으로 사용하기 위해서 만들어짐. 

Java7에서 추가된 클래스 중에는 Fork/Join과 관련된 클래스들이 존재한다. 

### ✔️Fork/Join이란

- 어떤 계산 작업을 할 때 “여러 개로 나누어 계산한 후 결과를 모으는 작업”을 의미한다.
- Fork는 여러 개로 나누는 것을 의미하고,
- Join은 나누어서 작업한 결과를 모으는 것을 말한다.

Java 7에서 추가된 Fork/Join은 단순하게 작업을 쪼개고 그 결과를 받는 단순한 작업만을 포함하지 않는다. 

- 여기서는 Work stealing이라는 개념이 포함되어 잇다.
    - 이것은 Fork/Join을 사용하면 별도로 여러분들이 구현하지 않아도 해당 라이브러리에서 Work steal 작업을 알아서 수행한다.
        - CPU가 많이 있는 장비에서 계산 위주의 작업을 매우 빠르게 해야 할 필요가 있을 때 매우 유용하게 사용될 수 있다.
        
    - Fork/Join이 실행되기 때문에 보통 이 연산은 회귀적으로 수행될때 많이 사용된다.

Fork/Join 기능은 java.util.concurrent 패키지의 RecursiveAction과 RecusiveTask라는 abstract 클래스를 사용해야 한다. 

각 클래스는 다음과 같이 선언되어 있다. 

| RecursiveAction | RecursiveTask |
| --- | --- |
| public abstract class RecursiveAction extends ForkJoinTask<Void> | public abstract class RecursiveTask<V> extends ForkJoinTask<V> |

두 클래스를 비교해 보면

| 구분 | RecusiveTask | RecursiveAction |
| --- | --- | --- |
| Generic | 0 | X |
| 결과 리턴 | 0 | X |

즉, RecursiveTask 클래스는 V라는 타입으로 결과가 리턴된다. 

두 클래스 모두 ForkJoinTask라는 abstract클래스를 확장한 것을 볼 수 있다. 

```java
public abstract class ForkJoinTask<V> extends Object implements Future<V>, Serializable
```

ForkJoinTask  라는 클래스는 Future라는 인터페이스를 구현했다. 

여기서 Future 라는 인터페이스는 Java5부터 추가된 인터페이스로 “비동기적인”요청을 하고 응답을 기다릴 때 사용”된다.

- Fork/Join 작업을 수행하려면 RecursiveTask 클래스나 RecursiveAction 클래스를 확장하여 개발하면 된다.
    - 두 클래스 모두 compute()라는 메소드가 있고, 이 메소드가 재귀 호출도고, 연산을 수행한다.

작업을 수행하는 클래스를 만든 후에는 ForkJoinPool 클래스를 사용하여 작업을 시작한다. 

|  | Fork/Join 클라이언트 밖에서 호출 | Fork/Join 클라이언트 내에서 호출 |
| --- | --- | --- |
| 비 동기적 호출 수행시  | execute(ForkJoinTask) | ForkJoinTask.fork() |
| 호출 후 결과 대기 | invoke(ForkJoinTask) | ForkJoinTest.invoke() |
| 호출 후 Future 객체 수신 | submit(ForkJoinTask) | ForkJoinTask.fork() |

---

### ✔️NIO 도 잘 모르는데 NIO2 가 나왔다.

- 자바의 NIO는 New I/O의 약자이며 JDK 1.4부터 제공되었다.
- NIO는 데이터를 보다 빠르게 일고 쓰는 데 주안점을 두고 각종 API를 제공한다.
- NIO와 이름은 비슷하지만 크게 관련은 없는 NIO2라는 것이 Java7부터 제공된다.

- 자바에서 파일을 다루기 위해서 제공한 [java.io](http://java.io) 패키지의 File 클래스에 미흡한 부분이 많았다.
    - 그래서 NIO2는 이를 보완하는 내용이 매우 많이 포함되어 있다.

Java6까지 사용된 File 클래스의 단점들.

- 심볼릭 링크, 속성 파일 권한 드엥 대한 기능이 없음.
- 파일을 삭제하는 delete() 메소드는 실패시 아무런 예외를 발생시키지 않고, boolean 타입의 결과만 제공해줌.
- 파일이 변경되었는지 확인하는 방법은 lastModifed()라는 메소드에서 제고앻주는 long 타입의 결과로 이전 시간과 비교하는 수밖에 없었으며, 이 메소드가 호출되면 연계되어 호출되는 클래스가 다수 존재하며 성능상 문제도 많음.

NIO2 클래스르 대체하는 클래스들. 

| class | description |
| --- | --- |
| Paths | 이 클래스에서 제공하는 static한 get() 메서드를  사용하면 Path라는 인터페이스의 객체를 얻을 수 있다. 여기서 Path라는 인터페이스는 파일과 경로에 대한 정보를 갖고 있다. |
| Files | 기존 File 클래스에서 제공되는 클래스의 단점들을 보완한 클래스다. 매우 많은 메서드들을 제공하며, Path 객체를 사용하여 파일을 통제하는데 사용된다. |
| FileSystems | 현재 사용중인 파일 시스템에 대한 정보를 처리하는데 필요한 메소드를 제공한다. Paths와 마찬가지로 이 클래스에서 제공되는 static한 getDefault() 메소드를 사용하면 현재 사용중인 기본 파일 시스템에 대한 정보를 갖고 있는 FileSystem이라는 인터페이스의 객체를 얻을 수 있다.  |
| FileStore | 파일을 저장하는 디바이스, 파티션, 볼륨 등에 대한 정보들을 확인하는데 필요한 메소드를 제공한다.  |

여기에 나열된 모든 클래스들은 java.nio.file이라는 새로 추가된 패키지에 위치하고 있다. 

이 중 Paths 클래스를 먼저 살펴보자. 

| method | return type |
| --- | --- |
| get(String first, String… more) | static path |
| get(URI uri) | static path |

디렉터리의 경로를 문자열로 지정하여 Path객체를 얻을 수도 있고 URI 정보를 갖고 Path 객체를 얻을 수도 있다. 

- Java7부터는 File클래스의 toPath()라는 메소드를 통해서도 Path 객체를 얻을 수 있다.

---

### ✔️Files 클래스는 파일을 다루기 위한 클래스.

- Files 클래스는 기존의 File 클래스에서 제공되는 기능보다 더 많은 기능을 제공한다.

| 기능 | 관련 메소드 |
| --- | --- |
| 복사 및 이동 | copy(), move() |
| 파일, 디렉터리 등 생성 | createDirectories(), createDirectory(), createFile(), createLink(), createSymbolicLink(), createTemDirecotry(), createTempFile() |
| 삭제 | delete(), deleteIfExists() |
| 읽기와 쓰기 | readAllBytes(), readAllLines(), readAttributes(), readSymbolicLink(), write() |
| Stream 및 객체 생성 | newBufferedReader(), newBufferedWriter(), newByteChannel, newDirectoryStream(), newInputStream(), newOutputStream() |
| 각종 확인 | get으로 시작하는 메소드와 is로 시작하는 메소드들로 파일의 상태를 확인함(매우 많음)  |

---

### ✔️파일과 관련된 다른 새로운 API

NIO2에서 추가된 클래스

- SeekableByteChannel ( random access)
    - Java7에 새로벡 추가되었고 java.nio.channels 패키지에 선언되어 있으며, 바이트 기반의 채널을 처리하는데 사용된다.
- NetworkChannel 및 MultiChannel
    - NetworkChannel : 네트워크 소켓을 처리하기 위한 채널.
        - 네트워크 연결에 대한 바인딩, 소켓 옵션을 셋팅하고, 로컬 주소를 알려주는 인터페이스이다.
    - MulticastChannel : IP 멀티캐스트를 지원하는 네트워크 채널이다.
        - 멀티 캐스트라는 것은 네트워크 주소인 IP를 그룹으로 묶고, 그 그룹에 데이터를 전송하는 방식을 말한다.
- Asynchronous I/O (AsynchronousFileChannel)
    - 비동기 처리를 의미한다. 자바스크립트를 사용할 때 사용하는 AJAX도 여기에 속한다.
    - AsychronousFileChannel을 처리한 결과는 java.util.concurrent 패키지의 Future 객체로 받게 된다.
        - Future 인터페이스로 결과를 받지 않으면, CompletionHandler라는 인터페이스를 구현한 객체로 받을 수 있다.
            - 이 CompletionHandler는 모든 데이터가 성공적으로 처리되었을 떄 수행되는 completed() 메소드와, 실패했을 때 처리되는 failed() 메소드를 제공하므로 필요에 따라 적절한 구현체를 사용하면 된다.
    - 이 외에도 AsynchronousChannelGroup이라는 것이 제공되는데
        - 이 그룹은 비동기 적인 처리를 하는 스레드풀을 제공하여 보다 안정적인 비동기적인 처리가 가능하다.
    

### ✔️FORK/JOIN과 NIO2 외에 추가 및 변경된 것들을 간단히 살펴보자.

Fork/Join과 NIO2 이외에 Java7에서 변경된 사항에 대해서 정리해 보면 

- JDBC 4.1
    - 주목할 만은 것은 RowSetFactory, RowSetProvider라는 클래스가 추가된 것이다.
    - JDK 1.4부터 제공되었는데, 이 인터페이스를 사용하면 Connection 및 Statement 객체를 생성할 필요 ㅇ벗이 sql query를 수행할 수 있다.
    - RowSetFactory와 RowSetProvider를 사용하면 아주 쉽게 이 RowSet 의 객체를 생성할 수 있다.
- TransferQueue 추가
    - java.util.concurrent 패키지에 있다.
        - 이 인터페이스는 어떤 메서드를 처리할 때 유용하게 사용할 수 있다.
    - 스레들와 관련된 공부를 더 자세히 하면 Producer/ Consumer라는 패턴을 만나게 된다. 이 패턴은 특정 타입의 객체를 처리하는 스레드 풀을 미리 만들어 놓고, 해당 풀이 객체들을 받으면 처리하도록 하는 구조를 의미한다.
        - 이 기능을 보다 일반화하여 SynchronousQueue의 기능을 인터페이스로 끌어올리면서 좀 더 일반화해서 BlockingQueue로 확장한 것이다.
- Objects 클래스 추가.
    - 이 클래스에는 compare(), equals(), hash(), hashCode(), toString() 등의 static한 메소드들은 제공한다.
        - 이 클래스는 매개변수로 넘어오는 객체가 null이라고 할지라도 예외를 발생시키지 않도록 구현해 놓은 것이 특징이다.
