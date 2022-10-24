# 네트워크 프로그래밍이란?
- 바로 옆에 있는 장비와 데이터를 주고 받는 작업을 보통 네트워킹이라고 한다. 이러한 네트워킹은 다음과 같은 레이어로 구분하도록 되어 있다.
- 4 : 애플리케이션 레이어 (HTTP, ftp, telnet, ...)
- 3 : 트랜스포트 레이어 (TCP, UDP, ...)
- 2 : 네트워크 레이어 (IP, ...)
- 1 : 링크 레이어 (device driver, ...)
- 실제로는 OSI 7 Layer라고 해서 보다 복잡한 레이어가 존재. 
- 애플리케이션 레이어 중 가장 대표적인 HTTP (Hyper Transfer Protocol), FTP (File Transfer Protocol), Telnet 들은 모두 
TCP (Transmission Control Protocol) 통신을 한다. 만약 자바로 TCP 통신을 한다면 자바에서 제공하는 API를 사용하면 된다. 즉, 애플리케이션 레이어에서
프로그래밍만 하면 트랜스포트 레이어에서의 처리는 자바에서 알아서 한다.
- TCP 통신은 가장 대표적인 통신 방법으로 "연결 기반 프로토콜"이라고 불린다. 만약 TCP를 사용하여 바로 옆에 있는 장비나 지구 반대편에 있는 장비로 데이터를 보내는
경우에 전송이 성공되었다는 통보를 받는다. 다시말해, TCP는 상대방이 데이터를 받았는지를 확실히 보장할 수 있다.
- UDP는 User Datagram Protocol의 약자다. UDP와 TCP가 다른 점 중 하나는 UDP는 다른 장비가 데이터를 제대로 받았는지에 대한 보장을 못한다는 것이다.
- TCP를 사용하면 데이터가 전송된다는 보장을 받을 수 있지만 내부적으로 처리되는 절차가 매우 복잡하게 되어 있다. 그만큼 TCP는 UDP보다 비싸고 느리며, 무겁다.
- 이 세상에 있는 모든 데이터가 꼭 전송이 보장되어야 할 필요는 없다. 이번에 데이터를 받지 않아도 다음에 받는 데이터를 사용해도 될 수 있다. 그리고, 100만개 중
몇 개 정도는 유실되어도 큰 문제가 되지 않는 시스템들이 있을 수 있다. 그러한 경우에는 무조건(반드시) 무거운 TCP를 사용하여 데이터를 주고 받을 필요가 없다.
- 다음으로는 포트(port)이다. 일반적인 웹 애플리케이션에서는 80번 포트를 사용한다. 이건 정해져 있는 것이다. 우리가 99를 쓰고 싶다면 웹 서버에 99번을 사용한다고
지정해 놓고, 사용자가 웹 서버에 붙기 위해서 주소 뒤에 콜로(:)을 붙인 뒤 99라고 적어 주어야 한다. (즉, URL을 브라우저에 입력하면 자동으로 :80 이 붙은 것과 같다.)
- 또 다른 예로 SSL이라는 안전한 통신을 하려면 443이라는 포트를 사용하게 된다. 이렇게 정해져 있는 포트는 되도록이면 다른 용도로 사용하지 않는 것이 좋다. 따라서,
0 ~ 1023까지는 우리가 사용하는 것이 제한되어 있다. 하지만 포트는 16비트로 구성되어 65,535 까지 사용할 수가 있으니 그 외의 값들은 임의로 사용하면 된다.

# 소켓 통신을 하기 위해서 알아야 하는 Socket 클래스
- TCP 통신을 자바에서 수행하려면 Socket 클래스를 사용하면 된다. 이 java.net 패키지에는 이와 관련된 많은 클래스들이 선언되어 있다.
- 이 Socket 클래스는 데이터를 보내는 쪽 (보통 클라이언트)에서 객체를 생성해서 사용한다. 데이터를 받는 쪽(보통 서버)에서 클라이언트 요청을 받으면, 요청에 대한
Socket 객체를 생성하여 데이터를 처리. 즉, 이 Socket 클래스는 서버쪽이든, 클라이언트 쪽이든, 원격에 있는 장비와의 연결 상태를 보관하고 있다고 생각하면 된다.
- 그러면 서버에서는 ServerSocket이라는 클래스를 사용하여 데이터를 받는다. 우리가 별도로 new 키워드를 사용하여 만들 필요 없이 ServerSocket 클래스에서
제공하는 메소드에서 클라이언트 요청이 생기면 Socket 객체를 생성해서 전달해 준다.
- ServerSocket 클래스에는 많은 메소드가 제공되는데, 생성자와 2개의 메소드만 알아도 충분히 네트워크 프로그래밍이 가능.
- 생성자는 다음과 같다.

| 생성자                                                       | 설명                                                              |
|-----------------------------------------------------------|-----------------------------------------------------------------|
| ServerSocket()                                            | 서버 소켓 객체만 생성한다.                                                 |
| ServerSocket(int port)                                    | 지정된 포트를 사용하는 서버 소켓을 생성한다.                                       |
| ServerSocket(int port, int backlog)                       | 지정된 포트와 backlog 개수를 가지는 서버 소켓을 생성한다.                            |
| ServerSocket(int port, int backlog, InetAddress bindAddr) | 지정된 포트와 backlog 개수를 가지는 서버 소켓을 생성하며, bindAddr에 있는 주소에서의 접근만을 허용 |

- backlog는 쉽게 생각하면 큐의 개수라고 보면 된다. ServerSocket 객체가 바빠서 연결 요청을 처리 못하고 대기시킬 때가 있는데, 그 때의 최대 대기 개수라고 보면 된다.
- 두 번째 생성자는 backlog 개수를 지정하지 않는데, 이렇게 하면 50개가 된다. 만약 애플리케이션 접속이 원활하지 않다면 이 개수를 적절하게 증가시키는 것이 좋다.
- 그리고, InetAddress 라는 클래스의 객체인 bindAddr이 있는데, 이는 특정 주소에서만 접근이 가능하도록 지정할 때, 사용한다.
- 한 가지 유의점은 매개 변수가 없는 생성자를 제외하고는 객체가 생성되자 마자 연결을 대기할 수 있는 상태가 된다. 거꾸로 말하면 ServerSocket() 생성자는 별도로
연결작업을 해야만 대기가 가능하다. 객체 생성 후 사용자의 요청을 대기하는 메소드가 바로 accept() 메소드다. 그리고, 소켓 연결 끝난 이후 닫는 메소드는 clos()이다.

| 리턴 타입  | 메소드      | 설명                                    |
|--------|----------|---------------------------------------|
| Socket | accept() | 새로운 소켓 연결을 기다리고, 연결이 되면 Socket 객체를 리턴 |
| void   | close()  | 소켓 연결을 종료                             |

- 여기서 close() 메소드 처리를 하지 않고, JVM이 계속 동작중이라면, 해당 포트는 동작하는 서버나 PC에서 다른 프로그램이 사용할 수 없다.
- 데이터를 받는 서버에서는 클라이언트에서 접속을 하면 Socket 객체를 생성하지만, 데이터를 보내는 클라이언트에서는 Socket 객체를 직접 생성해야만 한다. java.net에
있는 Socket 클래스의 생성자는 다음과 같다.

| 생성자                                                                         | 설명                                                                    |
|-----------------------------------------------------------------------------|-----------------------------------------------------------------------|
| Socket()                                                                    | 소켓 객체만 생성                                                             |
| Socket(Proxy proxy)                                                         | 프록시 관련 설정과 함께 소켓 객체만 생성                                               |
| Socket(SocketImpl impl)                                                     | 사용자가 지정한 SocketImpl 객체를 사용하여 소켓 객체만 생성                                |
| Socket(InetAddress address, int port)                                       | 소켓 객체 생성 후 address와 port를 사용하는 서버에 연결                                 |
| Socket(InetAddress address, int port, InetAddress localAddr, int localPort) | 소켓 객체 생성 후 address와 port를 사용하는 서버에 연결하며, 지정한 loclaAddr와 localPort에 접속 |
| Socket(String host, int port)                                               | 소켓 객체 생성 후 host와 port를 사용하는 서버에 연결                                    |
| Socket(String host, int port, InetAddress localAddr, int localPort)         | 소켓 객체 생성 후 host와 port를 사용하는 서버에 연결하며, 지정된 localAddr와 localPort에 접속    |

- host와 port를 지정하는 생성자를 사용하는 것이 가장 편하고, 대부분 다른 생성자들은 별도의 용도가 있는 Socket 객체를 생성하는 것이라고 생각하면 된다. 그리고,
위에 3개의 생성자를 제외한 나머지 생성자들은 모두 객체 생성과 함께 지정된 서버에 접속을 한다.
- Socket 클래스도 ServerSocket 클래스와 마찬가지로 close() 메소드를 사용하여 소켓을 닫는다.
- 소켓 연결에 문제가 있을 경우 끊어주는 Timeout 관련 메소드는 운영용 시스템을 개발할 때 매우 중요하다.

# 간단하게 소켓 통신을 해보자
- 먼저 소켓을 대기하는 서버 코드 이다.
```java
package vol2._28.network;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

public class SocketServerSample {
	public static void main(String[] args) {
		SocketServerSample sample = new SocketServerSample();
		sample.startServer();
	}

	private void startServer() {
		ServerSocket server = null;
		Socket client = null;
		try {
			server = new ServerSocket(9999);
			while (true) {
				System.out.println("Server:Waiting for request.");
				client = server.accept();
				System.out.println("Server:Accepted");
				InputStream stream = client.getInputStream();
				BufferedReader in = new BufferedReader(
					new InputStreamReader(stream)
				);
				String data = null;
				StringBuilder receivedData = new StringBuilder();
				while ((data = in.readLine()) != null) {
					receivedData.append(data);
				}
				System.out.println("Received data : " + receivedData);
				in.close();
				stream.close();
				client.close();
				if (receivedData != null && "EXIT".equals(receivedData.toString())) {
					System.out.println("Stop SocketServer");
					break;
				}
				System.out.println("----------");
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if (server != null) {
				try {
					server.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
	}
}
```
```java
server = new ServerSocket(9999);
```
1. 포트 번호 9999를 이용하여 ServerSocket 객체를 생성하였다. 그러므로, 다른 프로그램에서 이 프로그램에서 띄운 서버로 접근하려면 9999 포트를 사용하면 된다.
```java
client = server.accept();
```
2. ServerSocket 클래스에 선언되어 잇는 accept() 메소드를 호출하면 다른 원격 호출을 대기하는 상태가 된다. 만약 연결이 완료되면 "Socket 객체를 리턴"하여 client
변수에 할당된다.
```java
InputStream stream = client.getInputStream();
```
3. 데이터를 받기 위해서는 Socket 클래스에 선언된 getInputStream() 메소드를 호출하여 InputStream 객체를 받았다. 만약 접속한 상대방에게 데이터를 전송하려면
getOutputStream() 메소드를 호출하여 OutputStream 객체를 받은 후 이 stream에 데이터를 전달하면 된다.
```java
client.close();
```
4. 모든 데이터 처리가 끝난 후 Socket 사용이 종료되었다는 것을 알리기 위해서 close() 메소드를 호출한다.
```java
server.close();
```
5. 더 이상 소켓 수신할 필요가 없을 때에는 이와 같이 ServerSocket의 close() 메소드를 호출하면 된다.
- 이 예제는 데이터가 올 때마다 해당 데이터 내용을 출력하고, 계속 대기 상태로 유지된다. 하지만, 만약 넘어오는 데이터가 "EXIT"이면 더 이상 대기하지 않고, 
프로그램이 종료된다.
- 데이터를 전송하는 클라이언트의 소스는 다음과 같다.
```java
package vol2._28.network;

import java.io.BufferedOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.net.Socket;
import java.util.Arrays;
import java.util.Date;

public class SocketClientSample {
	public static void main(String[] args) {
		SocketClientSample sample = new SocketClientSample();
		sample.sendSocketSample();
	}

	public void sendSocketSample() {
		for (int i = 0; i < 3; i++) {
			sendSocketData("I liked java at " + new Date());
		}
		sendSocketData("EXIT");
	}

	private void sendSocketData(String data) {
		Socket socket = null;
		try {
			System.out.println("Client:Connecting");
			socket = new Socket("127.0.0.1", 9999);
			System.out.println("Client:Connect status=" + socket.isConnected());
			Thread.sleep(1000);
			OutputStream stream = socket.getOutputStream();
			BufferedOutputStream out = new BufferedOutputStream(stream);
			byte[] bytes = data.getBytes();
			out.write(bytes);
			System.out.println("Client:Sent data " + new String(bytes));
			out.close();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if (socket != null) {
				try {
					socket.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
	}
}
```
```text
socket = new Socket("127.0.0.1", 9999);
```
1. 127.0.0.1 라는 IP는 같은 장비라는 것을 의미. 그리고 포트 번호는 서버쪽에 지정한 포트와 동일해야만 한다. 이 두개의 매개 변수를 갖는 Socket 생성자를 사용하여
객체를 생성하고, 접속을 했다.
```text
OutputStream stream = socket.getOutputStream();
```
2. 데이터를 서버에 전달하기 위해서 getOutputStream() 메소드를 사용하여 객체를 생성하였다.
```text
socket.close();
```
3. 데이터를 전달한 후 close() 메소드를 사용하여 소켓 연결을 닫는다.
- 이 예제는 sendSocketData() 메소드를 총 3회 호출하고, 마지막에 "EXIT"을 호출하여 작업을 마치도록 되어 있다. 따라서, "EXIT"이 호출된 이후에는 서버 프로그램이
종료되므로 사용한 서버에 다시 접근할 수는 없다.
- 그러면 실행해보자. 반드시 SocketServerSample 를 먼저 실행해야 한다. 정상적으로 시작되었을 경우에는 다음과 같은 메시지를 출력하고 대기한다.
```text
Server:Waiting for request.
```
- 이 서버 프로그램을 중단하지 말고, 새로운 커맨드 창을 띄워서 SocketClient 클래스를 실행하자. 이 클래스를 실행하면 서버에 접속 후 1초씩 대기하면서 데이터를
전송한다. 클라이언트 쪽에 출력되는 내용은 다음과 같다.
```text
Client:Connecting
Client:Connect status=true
Client:Sent data I liked java at Mon Oct 24 18:04:33 KST 2022
Client:Connecting
Client:Connect status=true
Client:Sent data I liked java at Mon Oct 24 18:04:34 KST 2022
Client:Connecting
Client:Connect status=true
Client:Sent data I liked java at Mon Oct 24 18:04:35 KST 2022
Client:Connecting
Client:Connect status=true
Client:Sent data EXIT
```
- 서버인 SocketServerSample 에는 다음과 같이 출력되었다.
```text
Server:Waiting for request.
Server:Accepted
Received data : I liked java at Mon Oct 24 18:04:33 KST 2022
----------
Server:Waiting for request.
Server:Accepted
Received data : I liked java at Mon Oct 24 18:04:34 KST 2022
----------
Server:Waiting for request.
Server:Accepted
Received data : I liked java at Mon Oct 24 18:04:35 KST 2022
----------
Server:Waiting for request.
Server:Accepted
Received data : EXIT
Stop SocketServer
```
- 당연한 이야기지만, 서버에 출력되는 데이터 내용과 클라이언트에 출력되는 데이터 내용은 동일하다. 만약 이 두 개의 데이터가 다르다면 프로그램에 문제가 있는 것이다.
- 추가로 이 예제에서 발생할 수 있는 예외는 다음고 같다.
  - java.net.BindException
    - Address already in use 서버를 띄워 놓고 또 띄웠을 때 발생. 이미 지정된 port 번호를 사용하고 있기 때문에 동일한 port 번호를 사용할 수 없기 때문이다.
  - java.net.ConnectException
    - Connection refused 서버를 띄워 놓지 않고 클라이언트 프로그램만 수행했을 때 발생한다. 왜냐하면 받을 서버가 없으니 던질 곳도 없기 때문이다.
- 참고로, 클라이언트에서 서버가 아닌 서버에서 클라이언트로 데이터를 전송하는 것도 상관없다.

# UDP 통신을 위해서 알아야 하는 Datagram 관련 클래스
- UDP 는 TCP와 달리 데이터가 제대로 전달되었다는 보장을 하지 않는다. 그러므로, UDP 관련 프로그램은 데이터의 유실이 있어도 문제가 없을 때에만 사용하는 것이 좋다.
- 자바에서 UDP 통신을 하려면 TCP와 마찬가지로 데이터를 주고 받기 위한 클래스가 필요하다. 그런데 TCP와 다르게, 클래스 하나에서 보내는 역할과 받는 역할을 모두
수행할 수 있다. 그 클래스는 바로 DatagramSocket이다. 
- 그리고 TCP에서는 스트림 객체를 얻어 데이터를 주고 받았지만, UDP 통신을 할 때에는 스트림을 사용하지 않고, DatagramPacket 이라는 클래스를 사용한다.
- DatagramSocket의 생성자는 Socket 클래스의 생성자와 비슷하며, 다음과 같다.

| 생성자                                           | 설명                                     |
|-----------------------------------------------|----------------------------------------|
| DatagramSocket()                              | 소켓 객체 생성 후 사용 가능한 포트로 대기               |
| DatagramSocket(DatagramSocketImpl impl)       | 사용자가 지정한 SocketImpl 객체를 사용하여 소켓 객체만 생성 |
| DatagramSocket(int port)                      | 소켓 객체 생성 후 지정된 port로 대기                |
| DatagramSocket(int port, InetAddress address) | 소켓 객체 생성 후 address와 port를 사용하는 서버에 연결  |
| DatagramSocket(SocketAddress address)         | 소켓 객체 생성 후 address에 지정된 서버로 연결         |

- 이 DatagramSocket 클래스도 일반적인 연결을 하는 클래스들처럼 close() 메소드 제공하며, 더 이상 필요가 없을 때 이 메소드를 호출해 주어야 한다.
- 그리고, 데이터를 받기 위해서 대기할 때에는 receive() 메소드를 사용하고, 데이터를 보낼 때에는 send() 메소드를 사용하면 된다. 이 두 개의 메소드는 

| 리턴타입 | 메소드                            | 설명                                                   |
|------|--------------------------------|------------------------------------------------------|
| void | receive(DatagramPacket packet) | 메소드 호출시 요청을 대기하고, 만약 데이터를 받았을 때에는 packet 객체에 데이터를 저장 |
| void | send(DatagramPacket packet)    | packet 객체에 있는 데이터 전송                                 |

- 이 외에도 여러 가지 메소드 존재.
- 이 두 개의 메소드의 매개 변수로 제공되는 DatagramPacket 클래스의 생성자는 다음과 같다. 생성자 중 단 하나만 데이터를 받기 위한 생성자이며, 나머지
생성자들은 데이터를 전송하기 위한 생성자들이다.

| 생성자                                                                               | 설명                                                         |
|-----------------------------------------------------------------------------------|------------------------------------------------------------|
| DatagramPacket(byte[] buf, int length)                                            | length 크기를 갖는 데이터를 "받기" 위한 객체 생성                           |
| DatagramPacket(byte[] buf, int length, InetAddress address, int port)             | 지정된 address와 port로 데이터를 전송하기 위한 객체 생성                      |
| DatagramPacket(byte[] buf, int offset, int length)                                | 버퍼의 offset이 할당되어 있는 데이터를 전송하기 위한 객체 생성                     |
| DatagramPacket(byte[] buf, int offset, int length, InetAddress address, int port) | 버퍼의 offset이 할당되어 있고, 지정된 address와 port로 데이터를 전송하기 위한 객체 생성 |
| DatagramPacket(byte[] buf, int offset, int length, SocketAddress address)         | 버퍼의 offset이 할당되어 있고, 지정된 소켓 address로 데이터를 전송하기 위한 객체 생성    |
| DatagramPacket(byte[] buf, int offset, SocketAddress address)                     | 지정된 소켓 address로 데이터를 전송하기 위한 객체 생성                         |

- byte 배열은 전송되는 데이터다. offset은 전송되는 byte 배열의 첫 위치이다. 만약 offset이 1이면, 배열의 0번째부터가 아닌 1번째 부터 데이터를 전송한다. 그리고,
length는 데이터의 크기를 의미하는데, 이 값이 byte 배열의 크기보다 작으면 java.lang.IllegalArgumentException이 발생한다.
- 이 DatagramPacket 클래스의 여러 메소드 중 꼭 알아야 하는 메소드는 getData()와 getLength()다. getData() 메소드는 byte[]로 전송받은 데이터를 리턴하며,
getLength()는 전송받은 데이터의 길이를 int 타입으로 리턴한다.

# UDP 통신
```java
package vol2._28.network;

import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class DatagramServerSample {
	public static void main(String[] args) {
		DatagramServerSample sample = new DatagramServerSample();
		sample.startServer();
	}

	private void startServer() {
		DatagramSocket server = null;
		try {
			server = new DatagramSocket(9999);
			int bufferLength = 256;
			byte[] buffer = new byte[bufferLength];
			DatagramPacket packet = new DatagramPacket(buffer, bufferLength);
			while (true) {
				System.out.println("Server:Waiting for request.");
				server.receive(packet);
				int dataLength = packet.getLength();
				System.out.println("Server:received. Data length=" + dataLength);
				String data = new String(packet.getData(), 0, dataLength);
				System.out.println("Received data:" + data);
				if (data.equals("EXIT")) {
					System.out.println("Stop DatagramServer");
					break;
				}
				System.out.println("--------------");
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if (server != null) {
				try {
					server.close();
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		}
	}
}
```

```java
server = new DatagramSocket(9999);
```
1. DatagramSocket 객체를 port 번호를 지정하여 생성한다.
```java
DatagramPacket packet = new DatagramPacket(buffer, bufferLength);
```
2. 데이터를 받기 위한 DatagramPacket 객체를 byte 배열과 크기로 지정하여 생성한다.
```java
server.receive(packet);
```
3. 데이터를 받기 위해서 대기하고 잇다가, 데이터가 넘어오면 packet 객체에 데이터를 담는다.
```java
int dataLength = packet.getLength();
```
4. 전송받은 데이터의 크기를 확인한다.
```java
String data = new String(packet.getData(), 0, dataLength);
```
5. String 클래스의 생성자를 이용하여 byte 배열로 되어 있는 데이터를 String 문자열로 변경한다.
```java
server.close();
```
6. 모든 처리가 끝나면 DatagramSocket 객체인 server를 닫는다.
- 다음으로 데이터를 전송하는 클라이언트 예제를 살펴보자.
```java
package vol2._28.network;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.util.Date;

public class DatagramClientSample {
	public static void main(String[] args) {
		DatagramClientSample sample = new DatagramClientSample();
		sample.sendDatagramSample();
	}

	private void sendDatagramSample() {
		for (int i = 0; i < 3; i++) {
			sendDatagramData("I like UDP " + new Date());
		}
		sendDatagramData("EXIT");
	}

	private void sendDatagramData(String data) {
		try {
			DatagramSocket client = new DatagramSocket();
			InetAddress address = InetAddress.getByName("127.0.0.1");
			byte[] buffer = data.getBytes();
			DatagramPacket packet =
				new DatagramPacket(buffer, 0, buffer.length, address, 9999);
			client.send(packet);
			System.out.println("Client:Sent data");
			client.close();
			Thread.sleep(1000);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```
```java
DatagramSocket client = new DatagramSocket();
```
1. DatagramSocket을 생성한다. 이렇게 아무런 매개 변수 없이 객체를 생성해도 전혀 문제 없다.
```java
InetAddress address = InetAddress.getByName("127.0.0.1");
```
2. InetAddress 클래스를 사용하여 데이터를 받을 서버의 IP를 설정한다.
```java
DatagramPacket packet = new DatagramPacket(buffer, 0, buffer.length, address, 9999);
```
3. 데이터를 전송하기 위한 DatagramPacket을 생성한다. 서버의 주소와 port 번호를 이렇게 지정하면 데이터를 받기 위한 객체가 아니라, 전송하기 위한 객체가 된다.
```java
client.send(packet);
```
4. 데이터를 전송한다.
```java
client.close();
```
5. 소켓 연결을 종료한다.

- 결과는 다음과 같다. 먼저 서버 클래스를 실행하면 다음과 같이 출력된 후 대기하고 있을 것이다.
```text
Server:Waiting for request.
```
- 이 프로그램을 중단하지 않고, 새로운 커맨드 창에서 클라이언트 프로그램을 실행하면 다음과 같이 출력된다.
```text
Client:Sent data
Client:Sent data
Client:Sent data
Client:Sent data
```
- 서버 프로그램의 로그는 다음과 같이 출력된다.
```text
Server:Waiting for request.
Server:received. Data length=39
Received data:I like UDP Mon Oct 24 20:31:57 KST 2022
--------------
Server:Waiting for request.
Server:received. Data length=39
Received data:I like UDP Mon Oct 24 20:31:58 KST 2022
--------------
Server:Waiting for request.
Server:received. Data length=39
Received data:I like UDP Mon Oct 24 20:31:59 KST 2022
--------------
Server:Waiting for request.
Server:received. Data length=4
Received data:EXIT
Stop DatagramServer
```
- 정상적으로 4번의 데이터를 받고 출력했으며, 마지막 데이터가 "EXIT"이기 때문에 프로세스를 종료한다.
- 그런데 UDP는 TCP와 다르게 데이터가 성공적으로 전송되지 않아도 예외가 발생하지 않는다. 서버 프로그램을 수행하지 않고 클라이언트 프로그램을 수행해도, 아무런
이상 없이 프로그램이 종료된다. 
- 이처럼 UDP로 통신을 할 때에는 서버에서 데이터를 받을 준비가 되어 있지 않더라도, 클라이언트에서는 아무런 오류를 내지 않고 그냥 수행하도록 되어 있다. 이와는 반대로,
TCP의 경우에는 서버에 접속하지 못하면,
```text
java.net.ConnectionException: Connection refused: connect
```
- 와 같이 ConnectException이 발생한다.

# 자바에서 웹 페이지를 요청을 하려면 어떻게 해야 하지?
- 자바에서 인터넷을 통해 웹 페이지 요청 가능. 자바 API에서 제공하는 URL이라는 클래스를 사용하며 아주 간단한 요청은 처리할 수 있다. 만약 운영하는 시스템 내에서 웹페이지를
요청할 일이 있다면, 이 URL 클래스를 사용하는 것을 권장하지 않는다. 왜냐하면 이 클래스에서는 연결에 대한 상세한 설정을 할 수 없기 때문이다.
- 그래서, 일반적으로 Apache의 Http Components를 많이 사용한다.

