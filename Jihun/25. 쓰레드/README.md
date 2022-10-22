# 쓰레드가 도대체 뭘까?
- 지금까지는 모두 단일 쓰레드 프로그램이었다. 하지만, 자바 개발자라면 한 번 실행해 놓고, 계속 기능을 제공하는 프로그램을 만든다. 따라서 쓰레드를 알아야 한다.
- 자바 프로그램을 사용하여 실행시키면 JVM이 시작되고, JVM이 시작되면 자바 프로세스가 시작한다. 
- 하나의 프로세스 내에 여러 쓰레드가 수행된다. 하지만, 거꾸로 여러 프로세스가 공유하는 하나의 쓰레드가 수행되는 일은 절대 없다. 어떤 프로세스든 쓰레드가 하나
이상 수행된다.
- 정리하자면, java 명령어를 사용하여 클래스를 실행시키는 순간 자바 프로세스가 시작되고, main() 메소드가 수행되면서 하나의 쓰레드가 시작되는 것이다. 
- 만약 많은 쓰레드가 필요하다면 main() 메소드에서 쓰레드를 생성해주면 된다.
- 쓰레드는 왜 만들까? 프로세스가 하나 시작하려면 많은 자원이 필요하다. 만약 하나의 작업을 동시에 수행하려고 할 때 여러 프로세스를 띄워 실행하면 각각 메모리를 할당하여
주어야만 한다. OS마다 다르지만, JVM은 아무 옵션 없이 실행하면 적어도 32-64MB 의 물리 메모리를 점유하지만, 쓰레드를 하나 추가하면 1MB 이내의 메모리를 점유한다.
그래서 "경량 프로세스"라고도 부른다.
- 그리고, 요즘 PC급의 장비도 두 개 이상의 코어가 달려 있는 멀티 코어 시대로, 대부분 작업은 단일 쓰레드 보다 다중 쓰레드가 더 빠른 결과를 제공

# Runnabla 인터페이스와 Thread 클래스
- 쓰레드를 생성하는 2가지 방법을 Runnable 인터페이스를 사용하거나, Thread 클래스를 사용하는 것이다. Thread 클래스는 Runnable 인터페이스를 구현한 클래스이다.
둘 다 java.lang 패키지에 있다.
- Runnable 인터페이스에 선언된 메소드는 단 하나이다.

| 리턴 타입 | 메소드 이름 및 매개 변수 | 설명                 |
|-------|----------------|--------------------|
| void  | run()          | 쓰레드가 시작되면 수행되는 메소드 |

- run 메소드로 리턴값은 없다.
- 그에 반해 Thread 클래스는 매우 많은 생성자, 메소드를 제공
```java
package vol2._25.thread;

public class RunnableSample implements Runnable {
	@Override
	public void run() {
		System.out.println("This is RunnableSample's run() method.");
	}
}
```
```java
package vol2._25.thread;

public class ThreadSample extends Thread {
	@Override
	public void run() {
		System.out.println("This is ThreadSample's run() method.");
	}
}
```
```java
package vol2._25.thread;

public class RunThreads {
	public static void main(String[] args) {
		RunThreads threads = new RunThreads();
		threads.runBasic();
	}

	private void runBasic() {
		RunnableSample runnable = new RunnableSample();
		new Thread(runnable).start();

		ThreadSample thread = new ThreadSample();
		thread.start();
		System.out.println("RunThreads.runBasic() method is ended.");
	}
}
```
- 쓰레드가 수행되는 우리가 구현하는 메소드는 run() 메소드이다.
- 쓰레드를 시작하는 메소드는 start()이다.
- Runnable 인터페이스를 구현한 RunnableSample 클래스를 쓰레드로 바로 시작할 수는 없고, Thread 클래스의 생성자에 해당 객체를 추가하여 시작해 주어야만 한다.
- Thread 클래스를 바로 확장한 ThreadSample 클래스의 경우는 바로 start() 메소드를 호출할 수 있다.
- 두 가지 방법을 제공하는 이유는 뭘까? 자바에서는 하나의 클래스만 확장할 수 있다. 만약 어떤 클래스가 어떤 다른 클래스를 extends를 사용해 확장해야 하는 상황인데,
쓰레드로 구현해야 한다. 게다가 부모 클래스는 Thread를 확장하지 않았다. 어떻게 해야 할까?
- 자바에서 Thread 클래스를 확장 받아야만 쓰레드로 구현할 수 있는데, 다중 상속이 불가능하므로, 해당 클래스를 쓰레드로 만들 수 없다. 하지만, 인터페이스는
여러 개의 인터페이스를 구현해도 전혀 문제가 발생하지 않는다. 이러한 경우에는 Runnable 인터페이스를 구현해서 사용하면 된다.
- 정리하자면, 쓰레드 클래스가 다른 클래스를 확장할 필요가 있을 경우에는 Runnable 인터페이스를 구현하면 되며, 그렇지 않을 경우에는 쓰레드 클래스를 사용하는 것이 편하다.
- 위 메소드를 실행한 결과는 다음과 같다.
```text
This is RunnableSample's run() method.
This is ThreadSample's run() method.
RunThreads.runBasic() method is ended.
```
```text
This is RunnableSample's run() method.
RunThreads.runBasic() method is ended.
This is ThreadSample's run() method.
```
- 결과는 항상 같지 않고, 다를 것이다. 쓰레드라는 것을 start() 메소드를 통해서 시작했다는 것은 프로세스가 아닌 하나의 쓰레드를 JVM에 추가하여 실행한다는 것이다.
- runBasic()이라는 쓰레드를 가동시키는 메소드에서 runnable이라는 객체를 Thread 클래스 start()메소드로 시작한다. 이 때, 시작한 start() 메소드가 끝날때까지
기다리지 않고, 그 다음 줄에 있는 thread라는 객체의 start() 메소드를 실행한다. 이 줄도 마찬가지로, 새로운 쓰레드를 시작하므로 run() 메소드가 종료될 때까지
기다리지 않고, 바로 다음 줄로 넘어간다. 이처럼 쓰레드를 구현할 때, start() 메소드를 호출하면, 쓰레드 클래스에 있는 run() 메소드의 내용이 끝나든, 끝나지 않든
간에 쓰레드를 시작한 메소드에서는 그 다음 줄에 있는 코드를 실행한다.
```java
package vol2._25.thread;

public class RunMultiThreads {
	public static void main(String[] args) {
		RunMultiThreads sample = new RunMultiThreads();
		sample.runMultiThread();
	}

	private void runMultiThread() {
		RunnableSample[] runnable=  new RunnableSample[5];
		ThreadSample[] thread = new ThreadSample[5];
		for (int i = 0; i < 5; i++) {
			runnable[i] = new RunnableSample();
			thread[i] = new ThreadSample();

			new Thread(runnable[i]).start();
			thread[i].start();
		}

		System.out.println("RunMultiThreads.runMultiThread() method is ended.");
	}
}
```
```text
This is RunnableSample's run() method.
This is ThreadSample's run() method.
This is RunnableSample's run() method.
This is ThreadSample's run() method.
This is RunnableSample's run() method.
This is ThreadSample's run() method.
This is RunnableSample's run() method.
This is ThreadSample's run() method.
This is RunnableSample's run() method.
RunMultiThreads.runMultiThread() method is ended.
This is ThreadSample's run() method.
```
- 매 번 실행할 때마다 결과가 달라진다. 메소드 가장 마지막 줄에 있는 출력문이 가장 마지막에 실행되지 않는 것을 볼 수 있다.
- 새로 생성한 쓰레드는 run() 메소드가 종료되면 끝난다. 만약 run() 메소드가 끝나지 않으면, 실행한 애플리케이션은 끝나지 않는다. 단 여기서, 데몬 쓰레드는 예외이다.

# Thread 클래스의 생성자
| 생성자                                                                     | 설명                                                                                                                  |
|-------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| Thread()                                                                | 새로운 쓰레드를 생성한다                                                                                                       |
| Thread(Runnable target)                                                 | 매개 변수로 받은 target 객체의 run() 메소드를 수행하는 쓰레드를 생성                                                                        |
| Thread(Runnable target, String name)                                    | 매개 변수로 받은 target 객체의 run() 메소드를 수행하고, name 이라는 이름을 갖는 쓰레드를 생성                                                       |
| Thread(String name)                                                     | name이라는 이름을 갖는 쓰레드를 생성                                                                                              |
| Thread(ThreadGroup group, Runnable target)                              | 매개 변수로 받은 group의 쓰레드 그룹에 속하는 target 객체의 run() 메소드를 수행하는 쓰레드를 생성                                                     |
| Thread(ThreadGroup group, Runnable target, String name)                 | 매개 변수로 받은 group의 쓰레드 그룹에 속하는 target 객체의 run() 메소드를 수행하고, name이라는 이름을 갖는 쓰레드를 생성                                     |
| Thread(ThreadGroup group, Runnable target, String name, long stackSize) | 매개 변수로 받은 group의 쓰레드 그룹에 속하는 target 객체의 run() 메소드를 수행하고, name이라는 이름을 갖는 쓰레드를 생성. 단, 해당 쓰레드의 스택 크기는 stackSize 만큼만 가능 |
| Thread(ThreadGroup group, String name)                                  | 매개 변수로 받은 group의 쓰레드 그룹에 속하는 name이라는 이름을 갖는 쓰레드를 생성                                                                 |

- 모든 쓰레드는 이름이 있다. 아무 이름을 지정하지 않으면, "Thread-n"이다. n은 쓰레드 생성 순서에 따라 증가. 쓰레드 이르을 지정하면, 별도의 이름을 가지고, 
겹쳐도 예외나 에러가 발생하지 않는다. 
- 쓰레드를 묶어 놓을 수 있는 ThreadGroup. 여기서 제공하는 여러 메소드를 통해서 각종 정보를 얻을 수 있다. 
- stackSize의 경우 스택의 크기를 이야기한다. 쓰레드에서 얼마나 많은 메소드를 호출하는지, 얼마나 많은 쓰레드가 동ㅅ에 처리되는지는 JVM이 실행되는 OS의 플랫폼에 따라서
매우 다르다. 경우에 따라 무시될 수 있음
- 생성자들은 다음과 같이 호출할 수 있다.
```java
package vol2._25.thread;

public class NameThread extends Thread {

	public NameThread() {
		super("ThreadName");
	}

	public NameThread(String name) {
		super(name);
	}

	@Override
	public void run() {

	}
}
```
- 쓰레드를 실행할 떄에는 run() 메소드가 진입점이고, 쓰레드를 시작시킬 때에는 start() 메소드를 호출해야 한다. 이 메소드들은 매개 변수가 없는데, 값은
어떻게 넘겨줄까? 다음과 같이 인스턴스 변수로 사용한다면 값을 넘겨줄 수 있다.
```java
package vol2._25.thread;

public class NameCalcThread extends Thread {
	private int calcNumber;

	public NameCalcThread(String name, int calcNumber) {
		super(name);
		this.calcNumber = calcNumber;
	}

	@Override
	public void run() {
		calcNumber++;
	}
}
```
- 쓰레드 객체를 생성할 때 매개 변수를 받고, 인스턴스 변수로 사용한다면 다음과 같이 calcNumber라는 값을 동적으로 지정하여 쓰레드를 시작할 수 있다.

# 많이 사용되는 sleep() 메소드
- Thread 클래스에는 deprecated 된 메소드도 많고, static 메소드도 많이 있다. 
- static 메소드는 객체를 생성하지 않아도 사용할 수 있어, Thread에 있는 static 메소드는 대부분 해당 쓰레드를 위해서 존재하는 것이 아니라, JVM에 있는
쓰레드를 관리하기 위한 용도로 사용된다. 예외가 있는데, sleep() 메소드이다.

| 리턴 타입       | 메소드 이름 및 매개 변수                | 설명                                                                          |
|-------------|-------------------------------|-----------------------------------------------------------------------------|
| static void | sleep(long millis)            | 매개 변수로 넘어온 시간(1/1,000초)만큼 대기한다.                                             |
| static void | sleep(long millis, int nanos) | 첫 번째 매개 변수로 넘어온 시간(1/1,000초) + 두 번째 매개 변수로 넘어온 시간(1/1,000,000,000초)만큼 대기한다. |
- run() 메소드가 끝나지 않으면 애플리케이션이 끝나지 않는다. sleep() 메소드를 사용해 확인해보자.
```java
package vol2._25.thread;

public class EndlessThread extends Thread {
	@Override
	public void run() {
		while (true) {
			try {
				System.out.println(System.currentTimeMillis());
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}
}
```
```java
package vol2._25.thread;

public class RunEndlessThreads {
	public static void main(String[] args) {
		RunEndlessThreads sample = new RunEndlessThreads();
		sample.endless();
	}

	private void endless() {
		EndlessThread thread = new EndlessThread();
		thread.start();
	}
}
```
```text
1666336998064
1666336999068
1666337000073
1666337001078
...
```
- 즉, main() 메소드의 수행이 끝나더라도, main() 메소드나 다른 메소드에서 시작한 쓰레드가 종료하지 않으면 해당 자바 프로세스는 끝나지 않는다. (데몬 쓰레드는 예외)

# Thread 클래스의 주요 메소드
| 리턴 타입               | 메소드 이름 및 매개 변수       | 설명                              |
|---------------------|----------------------|---------------------------------|
| void                | run()                | 우리가 구현해야 하는 메소드                 |
| long                | getId()              | 쓰레드의 고유 id를 리턴. JVM에서 자동으로 생성해줌 |
| String              | getName()            | 쓰레드의 이름을 리턴                     |
| void                | setName(String name) | 쓰레드의 이름을 지정                     |
| int                 | getPriority()        | 쓰레드의 우선 순위를 확인                  |
| boolean             | isDaemon()           | 쓰레드가 데몬인지 확인                    |
| void                | setDaemon()          | 쓰레드를 데몬으로 설정할지 아닌지를 설정          |
| StackTraceElement[] | getStackTrace()      | 쓰레드의 스택 정보를 확인                  |
| Thread.State        | getThreadGroup()     | 쓰레드의 그룹을 확인                     |

- 쓰레드의 우선 순위는 대기하고 있는 상황에서 더 먼저 수행할 수 있는 순위. 대부분 이 값은 기본값으로 사용하는 것을 권장(장애로 연결 가능성.)
```java
package vol2._25.thread;

public class RunDaemonThreads {
	public static void main(String[] args) {
		RunDaemonThreads sample = new RunDaemonThreads();
		sample.checkThreadProperty();
	}

	private void checkThreadProperty() {
		ThreadSample thread1 = new ThreadSample();
		ThreadSample thread2 = new ThreadSample();
		ThreadSample daemonThread = new ThreadSample();

		System.out.println("thread1.getId() = " + thread1.getId());
		System.out.println("thread2.getId() = " + thread2.getId());

		System.out.println("thread1.getName() = " + thread1.getName());
		System.out.println("thread2.getName() = " + thread2.getName());

		System.out.println("thread1.getPriority() = " + thread1.getPriority());

		daemonThread.setDaemon(true);
		System.out.println("thread1.isDaemon() = " + thread1.isDaemon());
		System.out.println("daemonThread.isDaemon() = " + daemonThread.isDaemon());
	}
}
```
```java
thread1.getId() = 13
thread2.getId() = 14
thread1.getName() = Thread-0
thread2.getName() = Thread-1
thread1.getPriority() = 5
thread1.isDaemon() = false
daemonThread.isDaemon() = true
```
- 첫 두 줄에 쓰레드의 id 출력. 쓰레드 이름은 Thread-n으로 출력되었다. 우선순위는 기본 값이 5. 이처럼 쓰레드의 각종 상태 확인 가능.
- 우선 순위와 관계 있는 3개의 상수는 다음과 같다.

| 상수            | 값 및 설명                    |
|---------------|---------------------------|
| MAX_PRIORITY  | 가장 높은 우선 순위이며, 그 값은 10이다. |
| NORM_PRIORITY | 일반 쓰레드의 우선 순위이며, 5이다.     |
| MAX_PRIORITY  | 가장 낮은 우선 순위이며, 1이다.       |

- 우선 순위를 지정하는 것은 좋지 않지만, 만약 정할 일이 있다면 이 상수를 이용하는 것을 권장.
- 마지막줄의 daemonThread 라는 쓰레드 객체를 데몬 쓰레드를 지정하고 난 후 그 내용을 출력한 것을 볼 수 있다. 이렇게 쓰레드 수행 전에 데몬 여부를 지정해야만
그 쓰레드가 데몬 쓰레드로 인식된다.
- 데몬 쓰레드가 아닌 사용자 쓰레드는 JVM이 끝날 때까지 기다린다. 즉, 데몬 쓰레드로 지저앟면, 그 쓰레드가 수행되고 있든, 수행되지 않고 있든 상관 없이 JVM이
끝날 수 있따. 단, start() 메소드 호출 전에 데몬 쓰레드로 지정되어야만 한다.
```java
package vol2._25.thread;

public class DaemonThread extends Thread {
	@Override
	public void run() {
		try {
			Thread.sleep(Long.MAX_VALUE);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```
```java
public void runCommonThread() {
    DaemonThread thread = new DaemonThread();
    thread.start();
}
```
- 위와 같이 데몬 쓰레드로 지정하지 않고 실행하면 아무런 메시지도 뿌리지 않고, 해당 프로그램은 끝나지 않을 것이다.
- 다음으로 데몬 쓰레드로 지정해보자.
```java
public void runDaemonThread() {
    DaemonThread thread = new DaemonThread();
    thread.setDaemon(true);
    thread.start();
}
```
- 앞과 달리, 프로그램이 대기하지 않고, 그냥 끝나 버리는 것을 알 수 있다.
- 다시 말해, 데몬 쓰레드는 해당 쓰레드가 종료되지 않아도 다른 실행중인 일반 쓰레드가 없다면, 멈춰 버린다.
- 데몬 쓰레드를 만든 이유는 예를 들어 모니터링 하는 쓰레드를 별도로 띄워 모니터링하다가, 주요 쓰레드가 종료되면, 관련된 모니터링 쓰레드가 종료되어야 프로세스가 종료될
수 있다. 그런데, 모니터링 쓰레드를 데몬 쓰레드로 만들지 않으면 프로세스가 종료될 수 없게 된다. 이렇게 부가적인 작업을 수행하는 쓰레드를 선언할 때 데몬 쓰레드를 만든다.

# 쓰레드와 관련이 많은 synchronized
- 자바의 예약어로, 어떤 클래스나 메소드가 쓰레드에 안전하려면 synchronized를 사용해야 한다.
- 여러 쓰레드가 한 객체에 선언된 메소드에 접근하여 데이터를 처리하려고 할 때 동시에 연산을 수행하여 값이 꼬이는 경우가 발생할 수 있다. 단, 메소드에서 인스턴스
변수를 수정하려고 할 때에만 이러한 문제가 생긴다. 매개 변수나 메소드에서만 사용하는 지역변수만 다루는 메소드는 synchronized로 선언할 필요가 없다.
- synchronized는 두 가지 사용 방법이 있다.
  - 메소드 자체를 synchronized로 선언하는 방법 (synchronized methods)
  - 메소드 내의 특정 문장만 synchronized로 감싸는 방법 (synchronized statements)

## 메소드를 synchronized로 선언
```java
public void plus(int value) {
    amount += value;
}
```
- 위 메소드를 아래와 같이 변경하면 된다.
```java
public synchronized void plus(int value) {
    amount += value;
}
```
- synchronized 라는 단어가 메소드 선언부에 있으면, 동일한 객체의 이 메소드에 2개의 쓰레드가 접근하든, 100개의 쓰레드가 접근하든 간에 한 순간에는 하나의 쓰레드만
이 메소드를 수행하게 된다. 
```java
package vol2._25.thread;

public class CommonCalculate {
	private int amount;

	public CommonCalculate() {
		amount = 0;
	}

	public void plus(int value) {
		amount += value;
	}

	public void minus(int value) {
		amount -= value;
	}

	public int getAmount() {
		return amount;
	}
}
```
```java
package vol2._25.thread;

public class ModifyAmountThread extends Thread {
	private CommonCalculate calc;
	private boolean addFlag;

	public ModifyAmountThread(CommonCalculate calc, boolean addFlag) {
		this.calc = calc;
		this.addFlag = addFlag;
	}

	@Override
	public void run() {
		for (int i = 0; i < 10000; i++) {
			if (addFlag) {
				calc.plus(1);
			} else {
				calc.minus(1);
			}
		}
	}
}
```
- CommonCalculate 클래스의 객체를 받아서 addFlag 가 true이면 1을 더하고, addFlag가 false면 1을 빼는 연산을 수행한다. 덧셈이나 뺄셈 연산을 만번 수행하고
나서, 해당 쓰레드는 종료한다.

```java
package vol2._25.thread;

public class RunSync {
	public static void main(String[] args) {
		RunSync runSync = new RunSync();
		runSync.runCommonCalculate();
	}

	private void runCommonCalculate() {
		CommonCalculate calc = new CommonCalculate();
		ModifyAmountThread thread1 = new ModifyAmountThread(calc, true);
		ModifyAmountThread thread2 = new ModifyAmountThread(calc, true);

		thread1.start();
		thread2.start();
		try {
			thread1.join();
			thread2.join();
			System.out.println("Final value is " + calc.getAmount());
		} catch (InterruptedException e) {
			throw new RuntimeException(e);
		}
	}
}
```
```text
Final value is 17603
```
- RunSync 코드는 다음과 같다.
  - CommonCalculate라는 클래스의 객체를 calc 라는 이름으로 생성
  - ModifyAmountThread 라는 클래스의 객체를 생성할 때 calc를 매개 변수로 넘겨주고, plus() 메소드만 수행하도록 true를 두 번째 매개 변수로 넘겼다.
  - 각각의 쓰레드를 시작하고, try-catch 블록 내에 join() 메소드로 각각의 쓰레드가 종료될 때까지 기다린다.
  - join()이 끝나면 calc 객체의 getAmount() 메소드를 호출. 결과는 join() 메소드 수행 이후 이므로, 모든 쓰레드가 종료된 이후의 결과이다.
- 결과는 우리가 예상한 결과가 아닌 이상한 값이 출력되었다.
```java
public static void main(String[] args) {
    RunSync runSync = new RunSync();
    for (int i = 0; i < 5; i++)
        runSync.runCommonCalculate();
}
```
```text
Final value is 18524
Final value is 17973
Final value is 16349
Final value is 17006
Final value is 18234
```
- 왜 이렇게 나왓을까? plus() 메소드 때문이다. 다른 쓰레드에서 작업하고 있다고 하더라도, 새로운 쓰레드에서 온 작업도 같이 처리한다. 따라서, 데이터가 꼬일 수 있다.
```java
public void plus(int value) {
    amount += value;
}
```
- 이 `amount += value;` 를 풀어 쓰면 다음과 같다.
```java
amount = amount + value;
```
- 연산은 우측항의 결과를 좌측 항에 있는 amount에 담는다. 예를 들어 우측 항에 있는 amount가 1, value가 1이면 정상적인 경우 좌측 항의 결과에는 2가 된다.
- 그런데 좌측항에 2라는 값을 치환하기 전에 2가 안된 상태에서 amount 1 값을 가져와 1을 더하면 다시 amount가 2가 된다. 
- 이렇게 동시에 연산이 수행되기 때문에 우리가 원한 20,000이라는 값이 출력되지 않은 것이다. 이러한 문제를 해결하기 위한 것이 바로 synchronized이다.
```java
public synchronized void plus(int value) {
    amount += value;
}

public synchronized void minus(int value) {
    amount -= value;
}
```
```text
Final value is 20000
Final value is 20000
Final value is 20000
Final value is 20000
Final value is 20000
```
- 이렇게 수행한 결과는 우리가 원한 동일한 20,000이라는 결과를 출력한다.

# synchronized 블록
- 메소드에 간단히 synchronized를 추가해주면 성능상 문제점이 발생할 수 있다. 해당 synchronized가 필요한 부분이 아닌 다른 모든 줄까지 처리를 할 때,
필요 없는 대기 시간이 발생하게 된다. 이러한 경우에는 메소드 전체를 감싸면 안 되며, amount 라는 변수를 처리하는 부분만 synchronized 처리를 해 주면 된다.
```java
public void plus(int value) {
    synchronized (this) {
        amount += value;
    }
}

public void minus(int value) {
    synchronized (this) {
        amount -= value;
    }
}
```
- 이렇게 하면 synchronized(this) 이후에 있는 중괄호 내에 있는 연산만 동시에 여러 쓰레드에서 처리하지 않겠다는 의미다. 
- 일반적으로는 다음과 같이 별도의 객체를 선언하여 사용한다.

```java
Object lock = new Object();

public void plus(int value) {
    synchronized (lock) {
        amount += value;
    }
}

public void minus(int value) {
    synchronized (lock) {
        amount -= value;
    }
}
```
- synchronized를 사용할 때에는 하나의 객체를 사용하여 블록 내의 문장을 하나의 쓰레드만 수행하도록 할 수 있다. 쉽게 생각하자면, 여기서 사용한 lock 객체나
this는 모두 문지기라고 볼 수 있다. 그리고 그 문지기는 한 명의 쓰레드만 일을 할 수 있도록 허용해준다. 만약 블록에 들어간 쓰레드가 일을 다 처리하고 나오면,
문지기는 대기하고 있는 다른 쓰레드에게 기회를 준다.
- 이렇게 synchronized 블록을 사용할 때에는 lock이라는 별도의 객체를 사용할 수 있다. 그런데, 때에 따라서 이러한 객체는 하나의 클래스에서 두 개 이상 만들어 사용가능.
만약 amount 변수외에 interest 변수가 추가로 있고, lock을 하나만 사용하면 amount를 처리할 때, interest 변수를 처리하려는 부분도 처리를 못하게 된다.
따라서, 두 개의 lock 객체를 사용하면 보다 효율적인 프로그램이 된다.
- synchronized를 사용할 때 잘하는 실수 한 가지가 있다.
```text
CommonCalculate calc = new CommonCalculate();
ModifyThread thread1 = new ModifyThread(calc, true);
ModifyThread thread2 = new ModifyThread(calc, true);
```
- 메소드를 synchronized 할 때에는 이처럼 같은 객체를 참조할 때에만 유효하다. 
```text
CommonCalculate calc1 = new CommonCalculate();
ModifyThread thread1 = new ModifyThread(calc1, true);

CommonCalculate calc2 = new CommonCalculate();
ModifyThread thread2 = new ModifyThread(calc2, true);
```
- 이와 같이 두 개의 쓰레드가 동일한 calc가 아닌 서로 다른 객체를 참조한다면 synchronized로 선언된 메소드는 같은 객체를 참조하는 것이 아니므로, synchronized를
안쓰는 것과 동일하다고 보면 된다.
- 그리고, synchronized는 여러 쓰레드에서 하나의 객체에 있는 인스턴스 변수를 동시에 처리할 때 발생할 수 있는 문제를 해결하기 위해 필요한 것이라는 점이다.
즉, 인스턴스 변수가 선언되어 있다고 하더라도, 변수가 선언되어 있는 객체를 다른 쓰레드에서 공유할 일이 전혀 없다면 synchronized를 사용할 이유가 없다.
- StringBuffer와 StringBuilder라는 클래스가 있다. 여기서 StringBuffer는 쓰레드에 안전하고, StringBuilder는 그렇지 않다. 즉, StringBuffer는
synchronized 블록으로 주요 데이터 처리 부분을 감싸 두었고, StringBuilder는 synchronized라는 것이 사용되지 않았다. 따라서, StringBuffer는
하나의 문자열 객체를 여러 쓰레드에서 공유해야 하는 경우에 사용하고, StringBuilder는 여러 쓰레드에서 공유할 일이 없을 때 사용하면 된다.

# 쓰레드 통제 메소드
| 리턴 타입        | 메소드 이름 및 매개 변수               | 설명                                          |
|--------------|------------------------------|---------------------------------------------|
| Thread.State | getState()                   | 쓰레드의 상태 확인                                  |
| void         | join()                       | 수행중인 쓰레드가 중지할 때까지 대기                        |
| void         | join(long millis)            | 매개 변수에 지정된 시간만큼 (1/1,000초) 대기               |
| void         | join(long millis, int nanos) | 밀리초 (1/1,000초) + 나노초(1/1,000,000,000초)만큼 대기 |
| void         | interrupt()                  | 수행중인 쓰레드에 중지 요청을 한다.                        |

- Thread.State는 자바의 Thread 클래스에 있는 enum 클래스로, 이 클래스에 선언되어 있는 상수들의 목록은 다음과 같다.

| 상태           | 의미                                                 |
|--------------|----------------------------------------------------|
| NEW          | 쓰레드 객체는 생성되었지만, 아직 시작되지는 않은 상태                     |
| RUNNABLE     | 쓰레드가 실행중인 상태                                       |
| BLOCKED      | 쓰레드가 실행 중지 상태이며, 모니터 락(monitor lock)이 풀리기를 기다리는 상태 |
| WAITING      | 쓰레드가 대기중인 상태                                       |
| TIME_WAITING | 특정 시간만큼 쓰레드가 대기중인 상태                               |
| TERMINATED   | 쓰레드가 종료된 상태                                        |

- 이 클래스는 public static으로 선언되어 있다. 다시 말하면, Thread.State.NEW와 같이 사용할 수 있다는 의미다. 그리고, 어떤 쓰레드이건 간에 
"NEW -> 상태 -> TERMINATED"의 라이프 사이클을 가진다. 여기서 상태는 NEW, TERMINATED를 제외한 모든 다른 상태이다.

- join() 메소드의 경우 해당 쓰레드가 종료될 때까지 기다린다. 매개 변수가 없는 join() 메소드는 해당 쓰레드가 끝날 때까지 무한대로 대기한다.
- 만약, 특정 시간만큼만 기다리고 싶다면 매개 변수로 기다리고 싶은 시간을 지정하면 된다. 이 시간은 밀리초 단위, 두 개의 매개 변수로 할 경우 두 번째는 나노초 단위이다.
- 만약, 첫 번째 매개 변수가 음수이거나 두 번째 매개 변수가 0 ~ 999,999 값이 아니면 IllegalArgumentException 이라는 예외 발생.
- interrupt() 메소드는 현재 수행중인 쓰레드를 중단시킨다. 중단 시킬때, InterruptedException을 발생시키면서 중단시킨다. sleep(), join()에서 발생하는 예외로,
즉, 두 메소드와 같이 대기 상태를 만드는 메소드가 호출되었을 때에는 interrupt() 메소드를 호출할 수 있다. 이외에도 Object 클래스의 wait() 메소드가 호출된
상태에서도 이 메소드를 사용할 수 있다. 
- 만약 쓰레드 시작 전이나 종료 후 interrupt() 호출시 무시된다. 
- 또한 stop() 이라는 메소드는 안정상의 이유로 deprecated 되었으며, interrupt() 메소드를 사용하여 쓰레드를 멈추어야 한다.
```java
package vol2._25.thread.support;

public class RunSupportThreads {
	public static void main(String[] args) {
		RunSupportThreads sample = new RunSupportThreads();
		sample.checkThreadState1();
	}

	private void checkThreadState1() {
		SleepThread thread = new SleepThread(2000);
		try {
			System.out.println("thread state=" + thread.getState());
			thread.start();
			System.out.println("thread state(after start)=" + thread.getState());
			Thread.sleep(1000);
			System.out.println("thread state(after 1 sec)=" + thread.getState());

			thread.join();
			thread.interrupt();
			System.out.println("thread state(after join)=" + thread.getState());
		} catch (InterruptedException e) {
			throw new RuntimeException(e);
		}
	}
}
```
```java
package vol2._25.thread.support;

public class SleepThread extends Thread {
	long sleepTime;

	public SleepThread(long sleepTime) {
		this.sleepTime = sleepTime;
	}

	@Override
	public void run() {
		try {
			System.out.println("Sleeping " + getName());
			Thread.sleep(sleepTime);
			System.out.println("Stopping " + getName());
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}
```
- SleepThread 의 생성자 매개 변수에 2000이라는 값을 넘겨주고 2초동안 대기.
- getState()로 상태 출력
- sleep()으로 쓰레드가 시작하고 1초동안 대기한 후 상태 출력
- join()으로 메소드가 끝날 때까지 기다리도록 함
```text
thread state=NEW
thread state(after start)=RUNNABLE
Sleeping Thread-0
thread state(after 1 sec)=TIMED_WAITING
Stopping Thread-0
thread state(after join)=TERMINATED
```
- 쓰레드 시작 전에는 Thread.State 중 NEW 상태 출력.
- 쓰레드가 시작한 상황에 첫 출력문 전까지는 RUNNABLE
- 2초간 sleep 후 TIMED_WAITING
- 쓰레드가 종료되기를 join() 메소드에서 기다린 후에 TERMINATED 상태가 된다.

```java
public void checkJoin() {
    SleepThread thread = new SleepThread(2000);
    try {
        thread.start();
        thread.join(500);
        thread.interrupt();
        System.out.println("thread state(after join)=" + thread.getState());
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```
```text
Sleeping Thread-0
java.lang.InterruptedException: sleep interrupted
	at java.base/java.lang.Thread.sleep(Native Method)
	at vol2._25.thread.support.SleepThread.run(SleepThread.java:14)
```
- 잠자고 있던 쓰레드가 멈추고 InterruptedException 이라는 것이 발생한 것을 볼 수 있다.
- 매개 변수에 2초 이상시간을 넣으면 정상적으로 출력된다.
```text
Sleeping Thread-0
Stopping Thread-0
thread state(after join)=TERMINATED
```
- 이 메소드 외에 Thread 클래스에 선언되어 있는 상태 확인을 위한 메소드는 다음과 같다.

| 리턴 타입          | 메소드 이름 및 매개 변수  | 설명                                                                            |
|----------------|-----------------|-------------------------------------------------------------------------------|
| void           | checkAccess()   | 현재 수행중인 쓰레드가 해당 쓰레드를 수정할 수 있는 권한이 있는지를 확인. 만약 권한이 없다면 SecurityException 예외 발생 |
| boolean        | isAlive()       | 쓰레드가 살아 있는지를 확인. 해당 쓰레드의 run() 메소드가 종료되었는지 안 되었는지를 확인하는 것.                    |
| boolean        | isInterrupted() | run()이 정상적으로 종료되지 않고, interrupte() 메소드의 호출을 통해 종료되었는지를 확인하는 데 사용              |
| static boolean | interrupted()   | 현재 쓰레드가 중지되었는지를 확인                                                            |
- interrupted() 메소드는 static 메소드로 현재 쓰레드가 종료되었는지를 확인할 때 사용한다. isInterrupted() 메소드는 다른 쓰레드에서 확인할 때 사용하고,
interrupted() 메소드는 본인의 쓰레드를 확인할 때 사용된다는 점이 다르다.
- 지금까지 살펴본 Thread 클래스에서 제공하는 메소드는 쓰레드를 생성하고 처리하는 데 사용된다. JVM에서 사용되는 쓰레드의 상태를 확인하기 위해서는 Thread 클래스의
static 메소드들을 알아야만 한다. 

| 리턴 타입      | 메소드 이름 및 매개 변수  | 설명                                         |
|------------|-----------------|--------------------------------------------|
| static int | activeCount()   | 현재 쓰레드가 속한 쓰레드 그룹의 쓰레드 중 살아 있는 쓰레드의 개수를 리턴 |
| static     | currentThread() | 현재 수행중인 쓰레드의 객체를 리턴                        |
| static     | dumpStack()     | 콘솔 창에 현재 쓰레드의 스택 정보를 출력                    |

# Object 클래스에 선언된 쓰레드와 관련된 메소드들
- Thread 클래스에 선언된 메소드 외에 쓰레드의 상태를 통제하는 Object 클래스에 선언된 메소드들이 있다.

| 리턴 타입 | 메소드 이름 및 매개 변수                | 설명                                                                                                 |
|-------|-------------------------------|----------------------------------------------------------------------------------------------------|
| void  | wait()                        | 다른 쓰레드가 Object 객체에 대한 notify() 메소드나 notifyAll() 메소드를 호출할 때까지 현재 쓰레드에 대기하고 있도록 한다.                  |
| void  | wait(long timeout)            | wait() 메소드와 동일한 기능을 제공하며, 매개 변수에 지정한 시간만큼만 대기. 즉, 매개 변수 시간을 넘어서면 다시 깨어남. 밀리초로 1/1,000초 단위          |
| void  | wait(long timeout, int nanos) | wait() 메소드와 동일한 기능을 제공하며, 매개 변수에 지정한 시간만큼만 대기. 즉, 매개 변수 시간을 넘어서면 다시 깨어남. 밀리초+나노초(1,000,000,000초)단위 |
| void  | notify()                      | Object 객체의 모니터에 대기하고 있는 단일 쓰레드를 깨운다.                                                               |
| void  | notifyAll()                   | Object 객체의 모니터에 대기하고 있는 모든 쓰레드를 깨운다.                                                               |

```java
package vol2._25.thread.object;

public class StateThread extends Thread {
	private Object monitor;

	public StateThread(Object monitor) {
		this.monitor = monitor;
	}

	@Override
	public void run() {
		try {
			for (int i = 0; i < 10000; i++) {
				String a = "A";
			}
			synchronized (monitor) {
				monitor.wait();
			}
			System.out.println(getName() + " is notified.");
			Thread.sleep(1000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}
```
- monitor 라는 이름의 객체를 매개 변수로 받아 인스턴스 변수로 선언해 두었다.
- 쓰레드를 실행중인 상태로 만들기 위해서 간단하게 루프를 돌면서 String 객체를 생성한다.
- synchronized 블록 내에서 monitor 객체의 wait() 메소드를 호출
- wait() 끝나면 1초간 대기했다가 이 쓰레드는 종료
```java
package vol2._25.thread.object;

public class RunObjectThreads {
	public static void main(String[] args) {
		RunObjectThreads samp = new RunObjectThreads();
		samp.checkThreadState2();
	}

	private void checkThreadState2() {
		Object monitor = new Object();
		StateThread thread = new StateThread(monitor);
		try {
			System.out.println("thread state=" + thread.getState());
			thread.start();
			System.out.println("thread state(after start)=" + thread.getState());
			Thread.sleep(100);
			System.out.println("thread state(after sleep)=" + thread.getState());

			synchronized (monitor) {
				monitor.notify();
			}
			Thread.sleep(100);
			System.out.println("thread state(after notify)=" + thread.getState());
			thread.join();
			System.out.println("thread state(after join)=" + thread.getState());

		} catch (InterruptedException e) {
			throw new RuntimeException(e);
		}
	}
}
```
- StateThread의 매개 변수로 넘겨줄 montior 라는 Object 클래스 객체를 생성한다.
- 쓰레드 객체를 생성하고 시작한다.
- monitor 객체를 통해 notify() 메소드 호출
- 쓰레드 종료까지 기다리고 상태 출력
```text
thread state=NEW
thread state(after start)=RUNNABLE
thread state(after sleep)=WAITING
Thread-0 is notified.
thread state(after notify)=TIMED_WAITING
thread state(after join)=TERMINATED
```
- wait() 메소드 호출시, WAITING 상태. 누가 깨워줘야만 WAITING에서 풀림. interrupt()로 풀 수 있겠지만, notify()로 풀어야 InterruptedException이
발생하지 않고, wait() 이후의 문장도 정상적으로 수행하게 된다.

```java
private void checkThreadState3() {
    Object monitor = new Object();
    StateThread thread = new StateThread(monitor);
    StateThread thread2 = new StateThread(monitor);
    try {
        System.out.println("thread state=" + thread.getState());
        thread.start();
        thread2.start();
        System.out.println("thread state(after start)=" + thread.getState());
        Thread.sleep(100);
        System.out.println("thread state(after sleep)=" + thread.getState());

        synchronized (monitor) {
            monitor.notify();
        }
        Thread.sleep(100);
        System.out.println("thread state(after notify)=" + thread.getState());
        thread.join();
        System.out.println("thread state(after join)=" + thread.getState());
        thread2.join();
        System.out.println("thread2 state(after join)=" + thread2.getState());

    } catch (InterruptedException e) {
        throw new RuntimeException(e);
    }
}
```
- thread2라는 쓰레드 객체를 하나 더 생성후 동시에 실행하고, thread2.join()을 추가하여 thread2가 끝날때까지 대기 후 상태 출력.
```text
thread state=NEW
thread state(after start)=RUNNABLE
thread state(after sleep)=WAITING
Thread-0 is notified.
thread state(after notify)=TIMED_WAITING
thread state(after join)=TERMINATED
```
- thread2는 notify 되지 않았고, 끝나지도 않았다. 왜냐하면 자바에서 notify() 메소드를 호출하면, 먼저 대기하고 있는 것부터 그 상태를 풀어주기 때문이다.
- 따라서 무식하게 notify() 를 두번 호출하거나 notifyAll()로 모두 notify() 해 줄 수 있다.
```java
synchronized (monitor) {
    // monitor.notify();
    // monitor.notify();
    monitor.notifyAll();
}
```
```text
thread state=NEW
thread state(after start)=RUNNABLE
thread state(after sleep)=WAITING
Thread-0 is notified.
Thread-1 is notified.
thread state(after notify)=TIMED_WAITING
thread state(after join)=TERMINATED
thread2 state(after join)=TERMINATED
```
- 추가로 wait() 메소드는 join() 메소드처럼 매개 변수에 대기 시간을 지정하여 특정 기간 동안에만 대기하라고 할 수도 있다.

# ThreadGroup에서 제공하는 메소드들
- ThreadGroup는 쓰레드의 관리를 용이하게 하기 위한 클래스다. 하나의 애플리케이션에는 여러 종류의 쓰레드가 있을 수 있으며, 만약 ThreadGroup 클래스가 없으면
용도가 다른 여러 쓰레드를 관리하기 어려울 것이다. 쓰레드 그룹은 기본적으로 트리 구조를 가진다. 즉, 하나의 그룹이 다른 그룹에 속할 수도 잇고, 그 아래에 또 다른
그룹을 포함할 수도 있다.
- 주요 메소드는 다음과 같다.

| 리턴 타입       | 메소드 이름 및 매개 변수                                 | 설명                                                                                                   |
|-------------|------------------------------------------------|------------------------------------------------------------------------------------------------------|
| int         | activeCount()                                  | 실행중인 쓰레드의 개수를 리턴                                                                                     |
| int         | activeGroupCount()                             | 실행중인 쓰레드 그룹의 개수 리턴                                                                                   |
| int         | enumerate(Thread[] list)                       | 현재 쓰레드 그룹에 있는 모든 쓰레드를 매개 변수로 넘어온 쓰레드 배열에 담는다.                                                        |
| int         | enumerate(Thread[] list, boolean recurse)      | 현재 쓰레드 그룹에 있는 모든 쓰레드를 매개 변수로 넘어온 쓰레드 배열에 담고, 두 번째 매개 변수가 true면 하위에 있는 쓰레드 그룹에 있는 쓰레드 목록도 포함된다.       |
| int         | enumerate(ThreadGroup[] list)                  | 현재 쓰레드 그룹에 있는 모든 쓰레드 그룹을 매개 변수로 넘어온 쓰레드 그룹배열에 담는다.                                                   |
| int         | enumerate(ThreadGroup[] list, boolean recurse) | 현재 쓰레드 그룹에 있는 모든 쓰레드 그룹을 매개 변수로 넘어온 쓰레드 그룹배열에 담는다. 두 번째 매개 변수가 true면 하위에 있는 쓰레드 그룹에 있는 쓰레드 목록도 포함된다. |
| String      | getName()                                      | 쓰레드 그룹의 이름을 리턴                                                                                       |
| ThreadGroup | getParent()                                    | 부모 쓰레드 그룹 리턴                                                                                         |
| void        | list()                                         | 쓰레드 그룹의 상세 정보 출력                                                                                     |
| void        | setDaemon(boolean daemon)                      | 지금 쓰레드 그룹에 속한 쓰레드들을 데몬으로 지정                                                                          |

- enumerate()는 해당 쓰레드 그룹에 포함된 쓰레드나 쓰레드 그룹의 목록을 매개 변수로 넘어온 배열에 담는다. 이 메소드의 리턴값은 배열에 저장된 쓰레드 수이다.
- 따라서 쓰레드 그룹에 있는 모든 쓰레드의 객체를 제대로 담으려면 activeCount() 메소드를 통해 현재 실행중인 쓰레드의 개수를 정확히 파악 후, 그 개수만큼의 배열을 
생성하면 된다.

```java
package vol2._25.thread.group;

import vol2._25.thread.support.SleepThread;

public class RunGroupThreads {
	public static void main(String[] args) {
		RunGroupThreads samp = new RunGroupThreads();
		samp.groupThread();
	}

	private void groupThread() {
		try {
			SleepThread sleep1 = new SleepThread(5000);
			SleepThread sleep2 = new SleepThread(5000);

			ThreadGroup group = new ThreadGroup("Group1");
			Thread thread1 = new Thread(group, sleep1);
			Thread thread2 = new Thread(group, sleep2);

			thread1.start();
			thread2.start();
			Thread.sleep(1000);
			System.out.println("Group name=" + group.getName());
			int activeCount = group.activeCount();
			System.out.println("Active count=" + activeCount);
			group.list();

			Thread[] tempThreadList = new Thread[activeCount];
			int result = group.enumerate(tempThreadList);
			System.out.println("Enumerate result = " + result);
			for (Thread thread : tempThreadList) {
				System.out.println(thread);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```
```text
Sleeping Thread-1
Sleeping Thread-0
Group name=Group1
Active count=2
java.lang.ThreadGroup[name=Group1,maxpri=10]
    Thread[Thread-2,5,Group1]
    Thread[Thread-3,5,Group1]
Enumerate result = 2
Thread[Thread-2,5,Group1]
Thread[Thread-3,5,Group1]
Stopping Thread-0
Stopping Thread-1
```
- 쓰레드 그룹 이름과 실행중인 쓰레드 개수, list() 호출 결과를 알 수 있고, enumerate() 수행결과 2개의 데이터가 배열에 저장되었으며, 그 다음 두줄에는
각 쓰레드에 대한 정보가 출력되었다.
