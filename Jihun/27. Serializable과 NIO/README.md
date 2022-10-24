# Serializable
- java.io 패키지의 인터페이스로 선언된 변수나 메소드가 없다.
- 개발을 하다 보면, 생성한 객체를 파일로 저장할 일이 있을 수도 있고, 저장한 객체를 읽을 일이 생길 수도 있다. 그리고, 객체를 다른 서버로 보낼 때도 있고, 다른
서버에서 생성한 객체를 받을 일도 생길 수 있다. 그럴 때 꼭 필요한 것이 Serializable 구현이다. 이 인터페이스를 구현하면 JVM에서 해당 객체는 저장하거나, 다른
서버로 전송할 수 있도록 해준다.
- Serializable 인터페이스를 구현한 후에는 다음과 같이 SerialVersionUID 라는 값을 지정하는 것을 권장. 안하면 컴파일시 자동 생성
```java
static final long serialVersionUID = 1L;
```
- 반드시 static final long으로 선언해야 하며, 변수명도 serialVersionUID로 선언해 주어야만 자바에서 인식한다. 이 값은 아무런 값이나 지정해주면
되지만, 필요에 따라 값을 변경해야 하는 경우가 발생.
- 이 값은 해당 객체의 버전을 명시하는 데 사용. A라는 서버에서 B라는 서버로 SerialDTO라는 클래스의 객체를 전송한다고 가정하자. 전송하는 A 서버에 SerialDTO라는
클래스가 있어야 하고, 전송받는 서버에도 있어야 한다. 그래야만 그 클래스의 객체임을 알아차리고 데이터를 받을 수 있다.
- 그런데 만약 A 서버의 SerialDTO에는 변수가 3개, B 서버에는 변수가 4개 있는 상황이 발생하면 자바에서는 제대로 처리를 못하게 된다. 따라서 각 서버가 쉽게 해당
객체가 같은지 다른지를 확인할 수 있도록 하기 위해서는 serialVersionUUID로 관리를 해주어야만 한다. 즉, 클래스 이름이 같더라도, 이 ID가 다르면 다른 클래스라고
인식한다. 게다가 같은 UID여도 변수 개수, 타입 등이 다르면 다른 클래스로 인식.

# 객체를 저장해보자
- 자바에서는 ObjectOutputStream이라는 클래스를 사용하면 객체를 저장할 수 있다. 반대로 ObjectInputStream 클래스로 저장해 놓은 객체를 읽을 수 있다.
```java
package vol2._27.io.object;

public class SerialDTO {
	private String bookName;
	private int bookOrder;
	private boolean bestSeller;
	private long soldPerDay;

	public SerialDTO(String bookName, int bookOrder, boolean bestSeller, long soldPerDay) {
		this.bookName = bookName;
		this.bookOrder = bookOrder;
		this.bestSeller = bestSeller;
		this.soldPerDay = soldPerDay;
	}

	@Override
	public String toString() {
		return "SerialDTO{" +
			"bookName='" + bookName + '\'' +
			", bookOrder=" + bookOrder +
			", bestSeller=" + bestSeller +
			", soldPerDay=" + soldPerDay +
			'}';
	}
}
```
- 보통은 다음과 같이 DTO를 만든다. (+ getter, setter)
- 저장하는 클래스는 다음과 같이 만든다.
```java
package vol2._27.io.object;

import static java.io.File.*;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

public class ManageObject {
	public static void main(String[] args) {
		ManageObject manager = new ManageObject();

		String fullPath =
			separator + "Users" + separator + "kimjihun" + separator + "dev"
				+ separator + "GodOfJavaEx" + separator + "text" + separator + "serial.obj";
		SerialDTO dto = new SerialDTO("GodOfJavaBook", 1, true, 100);
		manager.saveObject(fullPath, dto);
	}

	private void saveObject(String fullPath, SerialDTO dto) {
		FileOutputStream fos = null;
		ObjectOutputStream oos = null;

		try {
			fos = new FileOutputStream(fullPath);
			oos = new ObjectOutputStream(fos);
			oos.writeObject(dto);
			System.out.println("Write Success !");
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if (oos != null) {
				try {
					oos.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
			if (fos != null) {
				try {
					fos.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
	}
}
```

- 주요 부분은 다음과 같다.
```text
fos = new FileOutputStream(fullPath);
oos = new ObjectOutputStream(fos);
oos.writeObject(dto);
```
1. FileOutputStream 객체를 생성했다.
2. 객체를 저장하기 위해서 ObjectOutputStream 객체를 생성했다. 이 객체를 생성할 때, fos를 매개 변수로 넘기면 해당 객체는 파일에 저장된다.
3. writeObject() 메소드로 매개 변수로 넘어온 객체를 저장한다.
- ObjectOutputStream 에는 write() 메소드를 사용하여 int 값을 저장하고, writeByte() 메소드로 바이트 값을 저장하면 된다.
- 위 메소드를 실행하면 다음과 같은 예외가 발생한다.
```text
java.io.NotSerializableException: vol2._27.io.object.SerialDTO
	at java.base/java.io.ObjectOutputStream.writeObject0(ObjectOutputStream.java:1185)
	at java.base/java.io.ObjectOutputStream.writeObject(ObjectOutputStream.java:349)
	at vol2._27.io.object.ManageObject.saveObject(ManageObject.java:28)
	at vol2._27.io.object.ManageObject.main(ManageObject.java:18)
```
- Serializable이 되어 있지 않다는 NotSerializableException 이 발생하였다. SerialDTO 내에 Serializable을 구현하지 않았기 때문이다.
```java
package vol2._27.io.object;

import java.io.Serializable;

public class SerialDTO implements Serializable {
...
}
```
- 위와 같이 변경 후 재 컴파일후 실행하면 다음과 같은 메시지가 나타날 것이다.
```text
Write Success !
```
- 해당 위치에 serial.obj 가 생성되어 있을 것이다. 바이너리로 저장된 파일이기 때문에, 열어서 보기는 어렵다.

# 객체를 읽어보자.
```text
public void loadObject(String fullPath) {
    FileInputStream fis = null;
    ObjectInputStream ois = null;
    try {
        fis = new FileInputStream(fullPath);
        ois = new ObjectInputStream(fis);
        Object obj = ois.readObject();
        SerialDTO dto = (SerialDTO)obj;
        System.out.println(dto);
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        if (ois != null) {
            try {
                ois.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if (fis != null) {
            try {
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```
- ObjectInputStream에서는 read로 시작하는 메소드를 사용한다. 이와 같이 객체를 읽을 때에는 readObject() 메소드를 사용하면 된다. 이 메소드의 리턴 타입이
Object이므로 Object로 받은 뒤에 SerialDTO로 형 변환을 하면 된다. 
- 실행 결과는 SerialDTO의 toString() 결과대로 출력된다.
```text
SerialDTO{bookName='GodOfJavaBook', bookOrder=1, bestSeller=true, soldPerDay=100}
```
- 즉, 객체를 정상적으로 읽었다는 의미다.
- 다음으로 Serializable 객체가 변경되었을 때 어떤 상황이 연출될까? SerialDTO 클래스에 변수를 추가해보자.
```java
public class SerialDTO implements Serializable {
	private String bookType = "IT";
	...
}
```
- 그 다음에 다시 ManageObject 클래스를 다시 실행하면 다음과 같은 예외가 발생한다.
```text
java.io.InvalidClassException: vol2._27.io.object.SerialDTO; local class incompatible: stream classdesc serialVersionUID = 7107584891607873924, local class serialVersionUID = -2672808644013766540
	at java.base/java.io.ObjectStreamClass.initNonProxy(ObjectStreamClass.java:689)
	at java.base/java.io.ObjectInputStream.readNonProxyDesc(ObjectInputStream.java:2012)
	at java.base/java.io.ObjectInputStream.readClassDesc(ObjectInputStream.java:1862)
	at java.base/java.io.ObjectInputStream.readOrdinaryObject(ObjectInputStream.java:2169)
	at java.base/java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1679)
	at java.base/java.io.ObjectInputStream.readObject(ObjectInputStream.java:493)
	at java.base/java.io.ObjectInputStream.readObject(ObjectInputStream.java:451)
	at vol2._27.io.object.ManageObject.loadObject(ManageObject.java:58)
	at vol2._27.io.object.ManageObject.main(ManageObject.java:20)
```
- serialVersionUID가 다르다는 InvalidClassException 예외 메시지가 출력된다. 
- 즉, 변수가 추가되는 등 Serializable 객체의 형태가 변경되면 컴파일 시 serialVersionUID가 다시 생성되므로, 이와 같은 문제가 발생한다. 
- 이러한 상황에서 예외가 발생하지 않도록 하려면 SerialDTO 클래스에 serialVersionUID를 다음과 같이 추가하면 된다.
```java
package vol2._27.io.object;

import java.io.Serializable;

public class SerialDTO implements Serializable {
	static final long serialVersionUID = 1L;

	private String bookType = "IT";
	
	...
	...

	@Override
	public String toString() {
		return "SerialDTO{" +
			"bookType='" + bookType + '\'' +
			", bookName='" + bookName + '\'' +
			", bookOrder=" + bookOrder +
			", bestSeller=" + bestSeller +
			", soldPerDay=" + soldPerDay +
			'}';
	}
}
```
- toString() 또한 수정하였다. 그 이후 다시 saveObject() 메소드부터 실행하면, 예외가 발생하지 않을 것이다.
```text
Write Success !
SerialDTO{bookType='IT', bookName='GodOfJavaBook', bookOrder=1, bestSeller=true, soldPerDay=100}
```
- 이 상태에서 저장되어 있는객체와 읽는 객체가 다르면 어떻게 될까? bookType의 변수명을 다음과 같이 bookTypes로 바꾸자.
```java
package vol2._27.io.object;

import java.io.Serializable;

public class SerialDTO implements Serializable {
	static final long serialVersionUID = 1L;
	private String bookTypes = "IT";
	
	// ...
	
	@Override
	public String toString() {
		return "SerialDTO{" +
			"bookTypes='" + bookTypes + '\'' +
			", bookName='" + bookName + '\'' +
			", bookOrder=" + bookOrder +
			", bestSeller=" + bestSeller +
			", soldPerDay=" + soldPerDay +
			'}';
	}
}
```
```text
SerialDTO{bookTypes='null', bookName='GodOfJavaBook', bookOrder=1, bestSeller=true, soldPerDay=100}
```
- 변수의 이름이 바뀌면 저장되어 있는 객체에서 찾지 못하므로, 해당 값은 null로 처리된다. 
- 지금까지 살펴본 것처럼 Serializable을 구현해 놓은 상황에서 serialVersionUID를 명시적으로 지정하면 변수가 변경되더라도 예외는 발생하지 않는다. 하지만,
만약 이렇게 Serializable한 객체의 내용이 바뀌었는데도 아무런 예외가 발생하지 않으면 운영 상황에서 데이터가 꼬일 수 있기 때문에 절대 권장하는 코딩 방법이 아니다.
- 따라서 이렇게 데이터가 바뀌면 serialVersionUID 의 값을 변경하는 습관을 가져야만 데이터에 문제가 발생하지 않는다.

# transient라는 예약어는 Serializable과 떨어질 수 없는 관계다
- SerialDTO의 bookOrder 선언문 앞에 transient 예약어를 추가해보자.
```java
transient private int bookOrder;
```
- 그리고 saveObject() 호출 부분이 실행되도록 하고 실행해보면,
```text
Write Success !
SerialDTO{bookTypes='IT', bookName='GodOfJavaBook', bookOrder=0, bestSeller=true, soldPerDay=100}
```
- bookOrder의 값을 우리는 1로 지정하고 저장하였는데, 읽어낸 값을 살펴보면 0이 출력되었다.
- 객체를 저장하거나, 다른 JVM으로 보낼 떄, transient 라는 예약어를 사용하여 선언한 변수는 Serializable 대상에서 제외된다. 다시 말해서, 해당 객체는
저장 대상에서 제외되어 버린다.
- 해당 객체를 생성한 JVM 에서 사용할 때에는 이 변수를 사용하는 데 전혀 문제가 없다. 예를 들어 패스워드를 보관하고 있는 변수가 있다고 가정하자.
- 이 변수가 저장되거나 전송된다면 보안상 큰 문제가 발생할 수 있다. 따라서 이렇게 보안상 중요한 변수나 꼭 저장해야 할 필요가 없는 변수에 대해서는 
transient 키워드를 사용하면 된다.

# 자바 NIO란?
- JDK 1.4부터 NIO (New IO)가 추가되었다. NIO가 생긴 단 하나의 이유는 속도 때문이다.
- NIO는 지금까지 사용한 스트림을 사용하지 않고, 대신 채널(Channel)과 버퍼(Buffer)를 사용한다.
- 채널은 물건을 중간에서 처리하는 도매상, 버퍼는 도매상에서 물건을 사고, 소비자에게 물건을 파는 소매상이라고 생각하면 된다. 자바 NIO에서 데이터를 주고 받을 때에는
버퍼를 통해서 처리한다.

```java
package vol2._27.nio;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

public class NioSample {
	public static void main(String[] args) {
		NioSample sample = new NioSample();
		sample.basicWriteAndRead();
	}

	private void basicWriteAndRead() {
		String fileName = "/Users/kimjihun/dev/GodOfJavaEx/text/nio.txt";
		try {
			writeFile(fileName, "My first NIO sample");
			readFile(fileName);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	private void writeFile(String fileName, String data) throws Exception {
		FileChannel channel = new FileOutputStream(fileName).getChannel();
		byte[] byteData = data.getBytes();
		ByteBuffer buffer = ByteBuffer.wrap(byteData);
		channel.write(buffer);
		channel.close();
	}

	private void readFile(String fileName) throws Exception {
		FileChannel channel = new FileInputStream(fileName).getChannel();
		ByteBuffer buffer = ByteBuffer.allocate(1024);
		channel.read(buffer);
		buffer.flip();
		while (buffer.hasRemaining()) {
			System.out.print((char)buffer.get());
		}
		channel.close();
	}
}
```
- 모든 코드는 위와 같다.
```java
private void writeFile(String fileName, String data) throws Exception {
    FileChannel channel = new FileOutputStream(fileName).getChannel();
    byte[] byteData = data.getBytes();
    ByteBuffer buffer = ByteBuffer.wrap(byteData);
    channel.write(buffer);
    channel.close();
}
```
1. 파일을 쓰기 위한 FileChannel 객체를 만들려면, 이와 같이 FileOutputStream 클래스에 선언된 getChannel()을 호출
2. ByteBuffer 클래스에 static으로 선언된 wrap()을 호출하면, ByteBuffer 객체가 생성된다. 이 메소드의 매개 변수는 저장할 byte 배열
3. FileChannel 클래스에 선언된 write() 메소드에 buffer 객체를 넘겨 주면 파일에 쓰게 된다.

```java
private void readFile(String fileName) throws Exception {
    FileChannel channel = new FileInputStream(fileName).getChannel();
    ByteBuffer buffer = ByteBuffer.allocate(1024);
    channel.read(buffer);
    buffer.flip();
    while (buffer.hasRemaining()) {
        System.out.print((char)buffer.get());
    }
    channel.close();
}
```
1. 파일을 읽기 위한 객체도 FileInputStream 클래스에 선언된 getChannel() 메소드를 사용
2. ByteBuffer 클래스에 선언되어 있는 allocate() 메소드를 통해서 buffer 라는 객체를 만들었다. 여기서의 매개 변수는 데이터가 기본적으로 저장되는 크기를 의미
3. 채널의 read() 메소드에 buffer 객체를 넘겨줌으로써, 데이터를 이 버퍼에다 담으라고 알려준다. 이렇게 알려주기만 하면, buffer에는 데이터가 담기기 시작한다.
4. flip() 메소드는 buffer에 담겨있는 데이터의 가장 앞으로 이동한다.
5. get() 메소드를 호출하면 한 바이트씩 데이터를 읽는 작업을 수행한다.
6. ByteBuffer에 선언되어 있는 hasRemaining() 메소드를 사용하여 데이터가 더 남아 잇는지를 확인하면서 반복작업 수행.
- 마지막에는 다른 IO 관련 클래스들처럼 close() 메소드를 호출하여 Channel을 닫아야 한다.
```text
My first NIO sample
```
- 결과는 위와 같이 출력된다.
- 파일 데이터를 다룰 때에는 ByteBuffer 라는 버퍼와 FileChannel 이라는 채널을 사용하면 간단히 처리할 수 있다. Channel의 경우 그냥 간단하게 객체만
생성하여 read()나 write() 메소드만 불러주면 된다고 생각하면 된다. Buffer 클래스는 복잡하다.

# NIO의 Buffer 클래스
- NIO에서 제공하는 Buffer는 java.nio.Buffer 클래스를 확장하여 사용한다. ByteBuffer 외에도, CharBuffer, DoubleBuffer, FloatBuffer, IntBuffer, 
LongBuffer, ShortBuffer 등이 존재한다. 이러한 Buffer 클래스에 선언되어 있는 메소드는 flip() 만 있는 것이 아니다.
- 먼저 버퍼의 상태 및 속성을 확인하기 위한 메소드이다.

| 리턴 타입 | 메소드        | 설명                      |
|-------|------------|-------------------------|
| int   | capacity() | 버퍼에 담을 수 있는 크기 리턴       |
| int   | limit()    | 버퍼에서 읽거나 쓸 수 없는 첫 위치 리턴 |
| int   | position() | 현재 버퍼의 위치 리턴            |

- 여기서 position(위치)라는 말이 나오는데, 버퍼는 CD 처럼 위치가 있다. 버퍼에 데이터를 담거나, 읽는 작업을 수행하면 현재의 "위치"가 이동한다.
그래야 그 다음 "위치"에 있는 것을 바로 쓰거나 읽을 수 있기 때문이다. 따라서,
    - 현재의 "위치"를 나타내는 메소드가 position()
    - 읽거나 쓸 수 없는 "위치"를 나타내는 메소드가 limit()
    - 버퍼의 크기(capacity)를 나타내는 것이 capacity()
- 메소드이다. 이 3개 값의 관계는 다음과 같다.
```text
0 <= position <= limit <= 크기(capacity)
```
- 예제를 통해 살펴보자.
```java
package vol2._27.nio;

import java.nio.IntBuffer;

public class NioDetailSample {
	public static void main(String[] args) {
		NioDetailSample sample = new NioDetailSample();
		sample.checkBuffer();
	}

	private void checkBuffer() {
		try {
			IntBuffer buffer = IntBuffer.allocate(1024);
			for (int i = 0; i < 100; i++) {
				buffer.put(i);
			}
			System.out.println("Buffer capacity = " + buffer.capacity());
			System.out.println("Buffer limit    = " + buffer.limit());
			System.out.println("Buffer position = " + buffer.position());
			buffer.flip();
			System.out.println("Buffer flipped !!!");
			System.out.println("Buffer limit    = " + buffer.limit());
			System.out.println("Buffer position = " + buffer.position());
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```
- limit의 position을 별도로 지정하지 않았으므로, 이 값은 기본 크기인 1,024이다.
- 데이터를 추가한 후 버퍼의 position은 100이 된다.
- flip()이라는 메소드를 호출한 다음에는 limit 값은 100이 되고, position 값은 0이다. CD 플레이어에 되감기와 같은 맨 앞으로 이동하는 메소드이다.
```text
Buffer capacity = 1024
Buffer limit    = 1024
Buffer position = 100
Buffer flipped !!!
Buffer limit    = 100
Buffer position = 0
```
- mark를 포함한 관계는 다음과 같다.
```text
0 <= mark <= position <= limit <= 크기(capacity)
```
- 위치를 변경하는 메소드는 다음과 같다.

| 리턴 타입   | 메소드            | 설명                                                   |
|---------|----------------|------------------------------------------------------|
| Buffer  | flip()         | limit 값을 현재 position으로 지정한 후, position을 0(가장 앞)으로 이동 |
| Buffer  | mark()         | 현재 position을 mark                                    |
| Buffer  | reset()        | 버퍼의 position을 mark한 곳으로 이동                           |
| Buffer  | rewind()       | 현재 버퍼의 position을 0으로 이동                              |
| int     | remaining()    | limit-position 계산 결과를 리턴                             |
| boolean | hasRemaining() | position와 limit 값에 차이가 있을 경우 true 리턴                 |
| Buffer  | clear()        | 버퍼를 지우고 현재 position을 0으로 이동하며, limit 값을 버퍼의 크기로 변경   |

- flip()과 rewind() 가 비슷해 보이지만, flip()은 limit 값을 변경하지만, rewind는 limit 값을 변경하지 않는다. 그리고, remaining() 메소드나
hasRemaining() 메소드를 사용하면 limit 까지만 데이터를 읽을 수 있다. 그리고, mark() 메소드를 사용하여 특정 위치를 표시해두고 다시 읽을 필요가 있을 때,
rewind() 메소드를 사용한다. 
- 먼저 결과를 출력하는 다음의 메소드를 만들자
```java
public void printBufferStatus(String job, IntBuffer buffer) {
    System.out.println("Buffer " + job + " !!!");
    System.out.printf("Buffer position=%d remaining=%d limit=%d\n",
        buffer.position(), buffer.remaining(), buffer.limit());
}
```
- 어떤 작업을 한 이후에 position, remaining, limit 값들을 출력하도록 했다.
- 이제 checkBufferStatus()를 만들자.
```java
public void checkBufferStatus() {
    try {
        IntBuffer buffer = IntBuffer.allocate(1024);
        buffer.get();
        printBufferStatus("get", buffer);
        buffer.mark();
        printBufferStatus("mark", buffer);
        buffer.get();
        printBufferStatus("get", buffer);
        buffer.reset();
        printBufferStatus("reset", buffer);
        buffer.rewind();
        printBufferStatus("rewind", buffer);
        buffer.clear();
        printBufferStatus("clear", buffer);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
```text
Buffer get !!!
Buffer position=1 remaining=1023 limit=1024
Buffer mark !!!
Buffer position=1 remaining=1023 limit=1024
Buffer get !!!
Buffer position=2 remaining=1022 limit=1024
Buffer reset !!!
Buffer position=1 remaining=1023 limit=1024
Buffer rewind !!!
Buffer position=0 remaining=1024 limit=1024
Buffer clear !!!
Buffer position=0 remaining=1024 limit=1024
```
- 이와 같이 버퍼에서 제공하는 메소드를 사용하면 데이터를 읽는 데 큰 어려움이 없을 것이다.
- NIO는 단지 파일을 쓰고 읽을 때에만 사용하는 것이 아니라, 파일 복사를 하거나, 네트워크로 데이터를 주고 받을 때에도 사용할 수 있다.


