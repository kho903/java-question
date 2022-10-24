
---

## **☑️자바에서 사용하는 대표적인 Network Layer**

![image](https://user-images.githubusercontent.com/46278436/197499224-758069ef-a8e2-4c7d-b858-721bb4212d57.png)


[그림 참조](https://www.imperva.com/learn/application-security/osi-model/)

**HTTP, FTP, Telnet 모두 TCP 통신을 한다.** 

### TCP(Transmission control protocal) 통신에 대해 알아보자 .

- 연결 기반 프로토콜 이라 불린다.
- TCP는 상대방이 데이터를 받았는지를 확실히 보장할 수 있다.

### UDP(User Datagram Protocal)

- 다른 장비가 데이터를 제대로 받았는지에 대한 보장을 못한다 .

### TCP   vs   UDP

- TCP는 UDP 보다 비싸고 느리며, 무겁다 .

> **이 세상에 있는 모든 데이터가 꼭 전송이 보장되어야 할 필요는 없다. 이번에 데이터를 받지 않아도 다음에 받는 데이터를 사용해도 될 수 있다. 그리고 100만개 중 몇 개 정도는 유실되어도 큰 문제가 되지 않는 시스템들이 있을 수 있는데 그러한 경우네는 무조건 무거운 TCP를 사용하여 데이터를 주고 받을 필요가 없다.**
> 

### PORT

- 기본적으로 80 포트를 사용한다.
- 99를 사용하고 싶다면 :99 를 쓰면 된다.

### SSL

- 안전한 통신을 하려면 443 포트를 사용한다.

> 0~ 1023 까지는 이미 사용 용도가 정해져 있어 사용하는 것이 제한된다.
포트는 16비트로 구성되어 있어 65,535까지 사용할 수 있으니 그 외의 값들은 임의로 사용하면 된다.
> 

---

## ☑️TCP

### Socket 클래스

자바에서 TCP 통신을하기 위해서는 Socket 클래스를 사용해야 한다.

Socket 클래스는 데이터를 보내는 쪽에서 객체를 생성하여 사용한다.

데이터를 받는 쪽에서 클라이언트 요청을 받으면, 요청에 대한 Socket 객체를 생성하여 데이터를 처리한다.

> Socket 클래스는 서버 쪽이 되었든, 클라이언트 쪽이 되었든 원격체 있는 장비와의 연결 상태를 보관하고 있다고 생각하면 된다.
> 

### Socket 생성자.

: ServerSocket 클래스의 API를 보면 매우 많은 메소드가 제공되는 것을 볼 수 있다. 

하지만 생성자 2개의 메소드만 알아도 충분히 네트워크 프로그래밍이 가능하다. 

| constructor | description |
| --- | --- |
| ServerSocket() | 서버 소켓 객체만 생성한다. |
| ServerSocket(int port) | 지정된 포트를 사용하는 서버 소켓을 생성한다. |
| ServerSocket(int port, int backlog) | 지정된 포트와 backlog 개수를 가지는 소켓을 생성한다. |
| ServerSocket(int port, int backlog, InetAddress bindAddr) | 지정된 포트와 backlog 개수를 가지는 소켓을 생성하며, bindAddr에 있는 주소에서의 접근만을 허용한다. |
- “backlog”라는 값이 있는데  큐의 개수라고 보면 된다.
    - ServerSocket 객체가 바빠서 연결 요청을 처리 못하고 대기시킬 때가 있는데, 그 때의 최대 대기 개수라고 보면 된다.

- backlog의 개수를 지정하지 않으면 50개로 지정된다.

> 어플리케이션의 접속이 원활하지 않다면, 이 개수를 적절하게 증가시키는 것이 좋다.
> 

- InetAddress 라는 클래스 객체인 bindAddr은 특정 주소에서만 접근이 가능하도록 지정할 때 사용한다.
    
<br><br>

**유사사항** : 

매개변수가 없는 ServerSocket 클래스를 제외한 나머지 클래스들은 객체가 생성되자 마자 연결을 대기할 수 있는 상태가 된다.

- ServetSocket 생성자는 별도로 연결작업을 해야만 대기가 가능하다.
- 객체 생성후 사용자의 요청을 대기하는 메소드가 바로 accept 메소드다.

| method | return type | description |
| --- | --- | --- |
| accept() | Socket | 새로운 소켓 연결을 기다리고, 연결이 되면 Socket  객체를 리턴.  |
| close() | void | 소켓 연결을 종료.  |

여기서 close() 메소드 처리를 하지 않고, JVM이 계속 동작중이라면, 해당 포트는 동작하는 서버나 PC에서 다른 프로그램이 사용할 수 없다.

데이터를 받는 서버에서는 클라이언트에서 접속을 하면 Socket 객체를 생성하지만,

 데이터를 보내는 클라이언트에서는 Socket 객체를 직접 생성해야 한다.

[java.net](http://java.net) package > Socket class consructor

| contructor | desciption |
| --- | --- |
| Socket() | 소켓 객체만 생성 |
| Socket(Proxy proxy) | 프록시 관련 설정과 함께 소켓 객체만 생성. |
| Socket(SocketImpl impl) | 사용자가 지정한 SocketImpl 객체를 사용하여 소켓 객체만 생성.  |
| Socket(InetAddress address, int port) | 소켓 객체 생성 후 address 와 port를 사용하는 서버에 연결 |
| Socket(InetAddress address, int port, InetAddress localAddr, int localPot) | 소켓 객체 생성 후 address 와 port 를 사용하는 서버에 연결하여, 지정한 localAddr 와 localPort에 접속.  |
| Socket(String host, int port) | 소켓 객체 생성 후 host와 port를 사용하는 서버에 연결.  |
| Socket(String host, int port, InetAddress localAddr, int localPort) | 소켓 객체가 생성 후 host 와 port를 사용하는 서버에 연결하며, 지정된 localAddr 와 localPort에 접속.  |


<br><br>

**유의사항** :

소켓 연결에 문제가 있을 경우 끊어주는 Timeout 관련 메소드는 꼭 직접 확인해보기 바란다 .

왜냐하면 운영용 시스템을 개발할 때에는 Timeout 이 매우 중요하기 때문이다. 

ㅇ예외 

- java.net.BindException
    
    : Address alreay in use 서버를 띄어놓고 또 띄었을 때 발생한다. 이미 지정된 port 번호를 사용하고 있기 때문에 동일한 port 번호를 사용할 수 없기 떄문이다. 
    
- java.net.ConnetException
    
    : Connection refused 서버를 띄어 놓지 않고 클라이언트 프로그램만 수행했을 때 발생한다. 왜냐하면 받을 서버가 없으니 던질 곳도 없기 때문이다. 
    

---

## ☑️UDP 통신을 하는 방법.

: 자바에서 UDP 통신을 하려면 TCP와 마찬가지로 데이터를 주고 받기 위한 클래스가 필요하다. 

그런데 TCP와는 다르게, 클래스 하나에서 보내는 역할과 받는 역할을 모두 수행할 수 있다. 

> **DatagramSocket 이다.**
> 
- TCP에서는 스트림 객체를 얻어 데이터를 주고 받았지만, UDP 통신을 할 때에는 스트림을 사용하지 않고 DatagramPacket 이라는 클래스를 사용한다.

### **DatagramSocket constuctor**

| constructor | description |
| --- | --- |
| DatagramSocket() | 소켓 객체 생성후 사용 가능한 포트로 대기  |
| DatagramSocket(DatagramSocketImpl impl) | 사용자가 지정된 SocketImpl 객체를 사용하여 소켓 객체만 생성.  |
| DatagramSocket(int port) | 소켓 객체 생성 후 지정된 port 로 대기  |
| DatagramSocket(int port, InetAddress address) | 소켓 객체 생성후 address 와 port를 사용하는 서버에 연결 |
| DatagramSocket(SocketAddress address) | 소켓 객체 생성 후 address 에 지정된 서버로 연결.  |
- Datagram class도 일반적인 연결하는 하는 클래스들처럼 close() 메소드를 제공한다.
    - 더 이상 사용할 필요가 없을 때에는 이 메소드를 호출해 주어야 한다.
        - 그리고, 데이터를 받기 위해서 대기할 때에는 receive() 메소드를 사용하고, 데이터를 보낼 때에는 send() 메소드를 사용한다.
            
            
            | method | return type | description |
            | --- | --- | --- |
            | receive(DatagramPacket packet) | void | 메소드 호출 시 요청을 대기하고, 만약 데이터를 받았을 때에는 packet 객체에 데이터를 저장 |
            | send(DatagramPacket packet) | void | packet 객체에 있는 데이터 전송  |
    

### DatagramPacket class

| constructor | description |
| --- | --- |
| DatagramPacket(byte[] buf, int length) | length 의 크기를 갖는 데이터를 “받기”위한 객체 생성.  |
| DatagramPacket(byte[] buf, int length, 
InetAddress address, int port) | 지정된 address 와 port로 데이터를 전송하기 위한 객체 생성 |
| DatagramPacket(byte[] buf, int offset, int length) | 버퍼의 offset이 할당되어 있는 데이터를 전송하기 위한 객체 생성.  |
| DatagramPacket(byte[] buf, int offset, int length, InetAddress address, int port) | 버퍼의 offset 이 할당되어 있고, 지정된 address 와 port로 데이터를 전송하기 위한 객체 생성 |
| DatagramPacket(byte[] buf, int offset, int length, SocketAddress address) | 버퍼의 offset이 할당되어 있고, 지정된 소켓 address 로 데이터를 전송하기 위한 객체 생성 |
| DatagramPacket(byte[] buf, int length, SocketAddress address) | 지정된 소켓 address 로 데이터를 전송하기 위한 객체 생성.  |

- 여기서 byte 배열은 전송되는 데이터다.
- 그리고 offset은 전송되는 byte 배열의 첫 위치이다.
    - 만약 offset 이 1이면 배열의 0번째부터가 아닌 1번째부터 데이터를 전송한다. 그리고 length는 데이터의 크기를 의미하는데, 이 값이 byte 배열의 크기보다 작으면 java.lang.IllegalArgumentException이 발생한다.

- DatagramPacket 클래스의 여러 메소드 중 꼭 알아야 하는 메소드는 getData()와 getLength()다.
- getData() 메소드 : byte[] 로 전송받은 데이터를 리턴
- getLength()  메소드 : 전송받은 데이터의 길이를 int 타입으로 리턴한다.

---

## ☑️자바에서 웹 페이지 요청하려면?

: 자바 API에서 제공하는 URL 이라는 클래스를 사용하면 아주 간단한 요청은 처리할 수 있다. 

- 만약 여러분들이 운영하는 시스템 내에서 웹 페이즈를 요청할 일이 있다면, 이 URL이라는 클래스를 사용하는 것은 권장하지 않는다.
    - 왜나하면 클래스는 연결에 대한 상세한 설정을 할 수가 없기 때문이다.
    
    > 그래서 일반적으로 Apache 의 Http Components를 많이 사용한다.
    > 

---

### ☑️조금 더 생각 해보자.

- 데이터가 유실되어도 되는 경우 ?
- Apache 의 http components ?
