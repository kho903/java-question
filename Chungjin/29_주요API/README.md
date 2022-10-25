## ☑️ 네트워크 관련 클래스들.

: 자바에서 네트워크 통신을 위기 위해서 기본으로 제공되는 클래스는 TCP 통신을 위한 Socket 클래스와 UDP 통신을 위한 Datagram 관련 클래스로 나뉜다. 

> 간단하게 웹에 접속하고 데이터를 처리하는 URL 클래스가 있긴 하지만, 섬세한 설정이 불가능하기 때문에 Apache 에서 제공하는 HttpClient 클래스를 사용할 것을 권장한다.
> 

### TCP 처리를 위한 Socket

- Socket 클래스는 데이터를 전달하고, 받기 위해서 사용하는 클래스다.
- ServerSocket 클래스는 데이터를 수신하기 위해서 사용하는 클래스다. 클라이언트에서 데이터를 전달하면 Socket 객체로 받아 처리하면 된다 .
- 데이터를 주고 받는 것은 Socket 클래스를 통하여 Stream이나 Reader/Writer 객체로 처리하면 된다.

### UDP를 처리하기 위한 Datagram

- DatagramPacket 클래스는 데이터를 전달하고, 받기 위해서 사용하는 클래스이다 .
- DatagramSocket은 데이터를 수신하기 위해서 사용하는 클래스다. 클라이언트에서 데이터를 전달하면 DatagramPacket 객체로 받아 처리하면 된다.
- TCP 는 연결할 때부터 데이터를 받는 서버가 시작되어 있어야만 데이터를 전송할 수 있지만, UDP 는 데이터를 받는 서버가 시작 여부 및 데이터 송수신 가능 여부와 상관없이 데이터를 전송할 수 있다.

---

## ☑️ IO 관련 클래스

파일이나 네트워크로 데이터를 읽고 쓰는 것을 통틀어 IO라고 한다. 그래서 파일에 쓸 때에는 “File IO”, 네트워크 처리할 때에는 “Network IO”라고 한다. 

### IO의 기본 Stream

- 자바 프로그램에서 데이터를 읽을 때에는 InputStream을 사용한다 .
- 자바 프로그램에서 밖으로 데이터를 쓸 때에는 OutputStream을 사용한다.
- char 기반의 데이터를 읽을 때에는 Reader, 쓸 때에는 Writer를 사용한다.

### InputStream 과 Reader

- 주요 메서드
    - read()  : 스트림의 내용 읽기
    - close() : 스트림 닫기
- InputStream의 주요 자식 클래스
    - FileInputStream : 바이트 기반의 파일을 읽을 때 사용.
    - ObjectInputStream : 저장되어 있는 객체를 읽을 때 사용.
    - FilterInputStream : 기타 여러 형태의 스트림을 처리하는 클래스의 부모가 되는 클래스

### OutputStrema 과 Writer

- 주요 메서드
    - write() : 스트림에 내용 쓰기
    - flush() : 강제로 쓰기
    - close() : 스트림 닫기
- OutputStream 의 주요 자식 클래스
    - InputStream 의 주요 자식 클래스의 이름에 OutputStream만 붙여주면 됨.

### File 클래스

- 파일 및 경로를 관리하는 클래스
- 기본 기능.
    - exists() : 존재 여부 확인.
    - isFile9), isDirectory() : 파일인지 경로인지 여부를 확인.
    - isHidden() : 숨겨진 파일 및 경로인지 확인.
    - lastModified() : 마지막 수정 시간 확인.
    - delete9) : 파일 및 경로 삭제
- 파일일 경우
    - renameTo() : 파일 이름 변경.
    - createNewFile() : 파일 생성
    - canRead(), canWrite(), canExecute() : 읽기 및 쓰기, 실행 가능 여부 확인.
- 경로인 경우
    - list(), listFiles() : 파일 목록 확인.
    - mkdir(), mkdirs() : 경로 생성
    - 

### Serializable

- 객체를 다른 서버로 전송하거나, 파일에 쓰고, 읽기 위해서 사용하는 인터페이스
- implements 만 하며 되며, 별도로 구현해야 하는 클래스는 없다.
- static final long serialVersionUID : 객체에 대한 serial 버전 값을 지정

### transient

- Serializable로 선언한 객체 내에 전송하거나 저장하지 않는 변수를 선언할 때 사용
- 변수 선언시 transient라고 지정만 해주면 된다.

### NIO

- 보다 빠른 IO를 위해서 추가된 패키지
- Channel 기반으로 데이터와 연결하여
- Stream 대신 Buffer 를 사용하여 데이터를 읽고 쓴다.

### Buffer 클래스

- 데이터 읽기와 쓰기
    - get() : 데이터 읽기
    - put() : 데이터 쓰기
- 상태 및 속성 확인을 위한 주요 메소드
    
    
    | method | return type | description |
    | --- | --- | --- |
    | capacity() | int | 버퍼에 담을 수 있는 크기 리턴 |
    | limit() | int | 버퍼에서 읽거나 쓸 수 없는 첫 위치 리턴 |
    | position() | int | 현재 버퍼의 위치 리턴 |
    
- 위치 변경을 위한 메소드 .
    
    
    | method | return type | description |
    | --- | --- | --- |
    | flip() | Buffer | limit 값을 현재 position으로 지정한 후, position을 0으로 이동 |
    | mark() | Buffer | 현재 position을 mark |
    | reset() | Buffer | 버퍼의 position을 mark한 곳으로 이동 |
    | rewind() | Buffer | 현재 버퍼의 position을 0으로 이동 |
    | remaining | Buffer | limit-position 계산 결과를 리턴 |
    | hasRemaining() | Buffer | position과 limit 값에 차이가 있을 경우 true 리턴 |
    | clear() | Buffer | 버퍼를 지우고 현재 position을 0으로 이동하며, limit 값을 버퍼의 크기로 변경.  |

 

---

## ☑️스레드

### 스레드를 시작할 수 있는 인터페이스와 클래스

- java.lang.Runnable
- java.lang.Thread

### 스레드 간단 정리.

- Runnable 인터페이스나 Thread 클래스를 직/간접적으로 구현한 클래스만 스레드로 처리될 수 있다.
- 스레드를 선언시에는 public void run() 메소드를 꼭 선언해야 한다.
- 스레드를 시작하려면 thread 객체의 start() 메소드를 호출하면 선어노디어 있는 run() 메서드가 실행된다.
- 스레드는 run() 메소드가 종료되면 끝난다.
- 단, 스레드를 Daemon으로 선언한 경우에는 프로세스 내의 다른 스레드들이 종료되면 run() 메소드가 종료되지 않더라도 해당 스레드는 종료된다.
- 스레드를 실행한 메소드에서 스레드가 종료될 때까지 대기하려면, join() 메소드를 사용하면 된다.
- Object 클래스에 선언된 wait() 메소드를 호출하면 스레드는 대기 상태가 되며, notify()나 notifyAll() 메소드를 호출하면 깨어나게 된다.
- 수행중인 스레드를 종료시키려면 interrupt()  메소드를 호출하면 된다. interrupt() 메소드를 호출한다고 모든 스레드가 종료되는 것은 아니고, join(), sleep(), wait() 메소드가 호출된 상태에서는 중지된다.

### 스레드의 상태 종합

| 상태 | 의미 |
| --- | --- |
| NEW | 스레드 객체는 생성되었지만, 아직 시작되지는 않은 상태  |
| RUNNABLE | 스레드가 실행중인 상태 |
| BLOCKED | 스레드가 실행 중지 상태이며, 모니터 락이 풀리기를 기다리는 상태 |
| WAITING | 스레드가 대기중인 상태 |
| TIMED_WAITING | 특정 시간만큼 스레드가 대기중인 상태 |
| TERMINATED | 스레드가 종료된 상태.  |

### synchronized

- 이 예약어를 사용하면 synchronized 블록을 만들면 동일한 객체를 공유하는 블록은 하나의 스레드에서만 수행할 수 있다.
- 이 예약어를 메소드 선언시 사용하면, 하나의 객체를 공유하는 아무리 많은 스레드가 동시에 이 메소드에 접근한다고 하더라도 하나의 스레드만 해당 메소드를 수행할 수 있다 .

---

## ☑️ 제네릭

: 제네릭은 형반환시에 발생할 수 있는 문제들을 사전에 없애기 위해서 만든 것이다.

### **제네릭 타입 이름 명명 기본 규칙**.

| E | 요소, 자바 컬렉션에서 주로 사용됨 |
| --- | --- |
| K | 키 |
| V | 값 |
| N | 숫자 |
| T  | 타입 |
| S,U, V | 두 번째, 세 번째, 네 번째에 선언되는 타입.  |

### **제네릭의 wildcard 타입.**

- 메소드 선언시 제네릭 타입의 제한을 해소하기 위해서 특정 타입 대신 <?>을 사용.
- 해당 타입을 정확히 모르기 때문에 Object로 받음.
    
    ```java
    public void wildcardMethod(WildcardGeneric<?> c){
    		Object value = c.getWildcard();
    		System.out.println(value);
    }
    ```
    

### **제한이 있는 wildcard 타입**

- 아무런 제약 없는 <?>은 어떤 타입도 올 수 있으므로, 타입에 제한을 걸어둠
- <? extends TypeName> 과 같이 TypeName 클래스를 확장한 모든 클래스를 의미함.
- 선언 예 : TypeName이 Car인 경우
    
    ```java
    public void bundedWildcardMethod(WildcardGeneric<? extends Car> c){
    		Car value = c.getWildcard();
    		System.out.println(value);
    }
    ```
    

### 제네릭한 메소드 선언하기

- wildcard 사용시에는 매개 변수로 넘어온 타입을 변경할 수 없음.
- 메소드를 제네릭하게 선언하면 제네릭 타입의 값을 변경할 수 있음.
- 선언 예
    
    ```java
    public <T> void genericMethod(WildcardGeneric<T> c, T addValue){
    		c.setWildCard(addValue);
    		T value = c.getWildcard();
    		System.out.println(value);
    }
    ```
    

---

## ☑️예외처리 관련된 예약어들.

| type | description |
| --- | --- |
| try | 예외가 발생 가능한 코드의 범위 선언 |
| catch | try로 묶은 범위에서 예외가 발생할 때 처리 방법 선언 |
| finally | try ~ catch 수행 후 반드시 실행해야 하는 작업 선언 |
| throw | 예외를 발생 시키거나 호출한 클래스로 넘길 때 사용 |
| throw | 예외를 던질 수도 있다는 것을 선언할 때 사용.  |

## ☑️클래스, 메소드, 변수 선언, 객체 생성과 관련된 예약어들. (잘모르는 것만 정리)

| packaeg | description |
| --- | --- |
| import |  |
| interface |  |
| abstract |  |
| class |  |
| enum |  |
| implments |  |
| extends |  |
| private |  |
| protected | 같은 패키지 내에 있거나 상속받은 경우에만 접근하게 할 경우 사용.  |
| public |  |
| final | 변수에 사용할 경우 값을 변경하지 못하도록 선언하며, 클래스에 사용할 경우 확장을 못하도록 선언.  |
| synchronized | 동시 접근 제어자 |
| void |  |
| static | 하나의 인스턴스만 허용하는 제어자 |
| return |  |
| assert | 검증을 위한 로직 선언.  |
| native | 다른 언어로 구현된 것을 선언. 이 책에서는 다루지 않음.  |
| new |  |
| null | 참조되고 있는 객체가 없다는 것을 선언.  |
| strictfp | strict 소수 값 제어자 |
| super | 상위 클래스 참조 |
| this | 현재의 객체에 대한 참조를 명시적으로 나타낼 때 사용.  |
| transient | Serializable 할 때 저장되거나 전송되지 않는 객체를 선언.  |
| volatile | 하나의 변수를 여러 스레드가 참조할 때 모두 동일한 값을 바라보도록 할 때 사용 |
| instanceof  | 객체의 타입을 확인할 때 사용.  |
