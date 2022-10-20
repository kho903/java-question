### **Serializable ?**

- **객체를 다른 서버로 보내거나 다른서버에서 생성한 객체를 받을 때 필요한 것이 “Serializable” 이다.**
- serializable 인터페이스를 구현하면 JVM에서 해당 객체는 저장하거나 다른 서버로 전송할 수 있도록 해준다 .

Serializable 인터페이스를 구현한 후에 아래와 같이 serialVersionUID 라는 값을 지정해 주는 것을 권장한다.

```java
static final long serialVersionUID = 1L; 
```

별도로 지정해줄 않으면 자바 소스가 컴파일 될 때 자동으로 생성해준다.

- 반드시 static final 으로 선언
- 반드시 변수명도 serialVersionUID로 선언

---

### **객체 저장/ 읽기**

1) 객체 저장

- ObjectInputStream / ObjectOutputStream
    - ObjectInputStream : ObjectInputStream 객체를 사용하면 저장해 놓은 객체를 읽을 수 있다.
    - ObjectOutputStream : ObjectOutputStream  객체를 저장할 수 있다.

DTO 부분에 Serializable이 구현되어 있지 않으면  NotSerializableException이 발생한다.

2) 객체 읽기

serialVersionUID가 다르면 InvalidClassException 예외가 출력된다.

**serializable을 구현해 놓은 상황에서 serialVersionUID를 명시적으로 지정하면 변수가 변경되더라도 예외는 발생하지 않는다.** 

```java
static final long serialVersionUID = 1L;
```

**유의사항**

: 만약 serializable 한 객체의 내용이 바뀌었는데도 아무런 예외가 발생하지 않으면 운영 상황에서 데이터가 꼬일 수 있기 때문에 절대 권장하는 방법은 아니다. 

이렇게 데이터가 바뀌면 serialVersionUID 의 값을 변경하는 습관을 가져야만 데이터에 문제가 발생하지 않는다. 

 * 추가로 요즘 자바 개발툴에서는 자동으로 serialVersionUID 를 생성하는 기능을 제공하기도 한다. 

---

 

### Transient 예약어.

```java
transient private int bookOrder; 
```

 객체를 저장하거나, 다른 JVM으로 보낼 때, transient 라는 예약어를 사용하여 선언한 변수는 Serializable의 대상에서 제외된다. 

> **무시될꺼면 무엇하러 이 변수를 만들어?**
> 

해당 객체를 생성한 JVM에서 사용할 때에는 이 변수를 사용하는데 전혀 문제가 없다. 

예를 들어 패스 워드를 보관하고 있는 변수가 있다고 생각해보자. 

이변수가 저장되거나 전송된다면 보안상 큰 문제가 발생할 수 있다. 

**따라서 이렇게 중요한 변수가 꼭 저장해야 할 필요가 없는 변수에 대해서는 transient를 사용할 수 있다.** 

> **serializable과 transient는 떨어질 수 없는 관계이다.**
> 

---

 

### 자바 NIO란?

: 속도 때문에 등장. 

**NIO는 지금까지 사용한 스트림을 사용하지 않고, 대신 채널(Channel)과 버퍼(Buffer)를 사용한다.**

- 채널 : 중간에서 처리하는 도매상.
- 버퍼 : 도메상에서 물건을 사고, 소비자에게 물건을 파는 소매상.

### NIO의 Buffer 클래스

NIO에서 제공하는 Buffer는 java.nio.Buffer 클래스를 확장하여 사용한다. 

버퍼의 상태 및 속성을 확인하기 위한 메소드

| method | return type | description |
| --- | --- | --- |
| capacity() | int | 버퍼에 담을 수 있는 크기를 리턴. |
| limit() | int | 버퍼에서 읽거나 쓸 수 없는 첫 위치 리턴. |
| position | int | 현재의 버퍼의 위치 리턴.  |

> **버퍼와 관련한 메서드 - 버퍼에 데이터를 담거나, 읽는 작업을 수행하면 현재의 “위치”가 이동한다. 
그래야 그 다음 “위치”에 있는 것을 바로 쓰거나, 읽을 수 있기 때문이다.**
> 
- 현재의 “위치”를 나타내는 메서드가 postion()이고,
- 읽거나 쓸 수 없는 “위치”를 나타내는 메소드가 limit()
- 버퍼의 크기(capacity)를 나타내는 것이 capacity()

```java
0 <= position <= limit <= 크기(capacity)
```

위치를 변경하는 메소드

| method | return type | description |
| --- | --- | --- |
| flip() | Buffer | limit 값을 현재 position으로 지정한 후, position을 0(가장 앞)으로 이동 |
| mark() | Buffer | 현재 position 을 mark |
| reset() | Buffer | 버퍼의 position 을 mark한 곳으로 이동 |
| rewind() | Buffer | 현재 버퍼의 position을 0으로 이동 |
| remaining() | int | limit-position 계산 결과를 리턴 |
| hasRemaining() | boolean | position와 limit 값에 차이가 있을 경우 true를 리턴 |
| clear() | Buffer | 버퍼를 지우고 현재 position 0으로 이동하며, limit 값을 버퍼의 크기로 변경. |

> **NIO는 단지 파일을 쓰고 읽을 때에만 사용하는 것이 아니라, 파일 복사를 하거나, 네트워크로 데이터를 주고 받을 때에도 사용할 수 있다.**
> 

---
