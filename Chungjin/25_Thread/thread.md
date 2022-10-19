### 스레드는 어떻게 시작될까?

자바 프로그램을 사용하여 뒤에클래스 이름을 붙이고, 엔터프라이즈 치면 적어도 하나의 JVM이 시작된다.

보통 이렇게 JVM이 시작되면 자바 프로세스가 시작한다. 이 프로세스라고 울타리안에서 여러 개의 스레드라는 것이 아등바등 살게 된다.

즉, 하나의 프로세스 내에 여러 스레드가 수해된다. 하지만, 거꾸로 여러 프로세스가 공유하는 하나의 스레드가 수행될 일은 절대 없다. 어떤 프로세스는 간에 스레드가 하나 이상 수행된다.

java 명령어를 사용하여 클래스를 실행시키는 순간 자바 프로세스가 실행되고, main()메소드가 수행되면서 하나의 스레드에서 시작된다.

만약 많은 스레드에서 필요하다면, 이 main() 메소드에서 스레드를 생성해주면 된다.

*자바에서 웹을 제공할 때에는 Tomcat과 같은 Was를 사용한다.이 WAS도 똑같이 main() 메서드에서 생성한 스레드들이 수행된다.

### 스레드는 왜 만들어졌을까?

프로세스가 하나 시작하려면 많은 자원이 필요하다. 만약 하나의 작업을 동시에 수행하려고 할 때 여러 개의 프로세스를 띄어서 실행하면 각각 메모리를 할당하여 주어야 만 한다.

JVM은 기본적으로 아무런 옵션 없이 실행하면 os마다 다르지만, 적어도 32MB ~ 64MB의 물리 메모리를 점유한다. 그에 반해서, 스레드를 하나 추가하면 1MB 이내의 메모리를 점유한다.

---

**Thead 구현 방법 두가지 (Runnable interface / Thread class)**

- Runnable interface.
    - Runnable 인터페이스는 선언되어 있는 메소드를 단지 하나다. - run()
- Thread class.
    - Thread 클래스는 매우 많은 생성자와 메소드가 제공된다.

### 두가지 방법을 사용하는 이유?

자바에서 하나의 클래스만 확장할 수 있다. 

여러개의 인터페이스의 구현이 필요한 경우는 Runnable 인터페이스를 구현해서 사용한다.

### start() 메소드.

여러분들이 스레드라는 것을 start() 메소드를 통해서 시작했다는 것은, 프로세스가 아닌 하나의 스레드를 JVM에 추가하여 실행한다는 것이다.

---

### Thread 8개 생성자.

| 생성자 | 설명 |
| --- | --- |
| Thread() | 새로운 스레드를 생성한다. |
| Thread(Runnable target) | 매개 변수로 받은 target 객체의 run() 메소드를 수행하는 스레드를 생성한다. |
| Thread(Runnable target, String name) | 매개 변수로 받은 target 객체의 run() 메소드를 수행하고, name이 라는 이름을 갖는 스레드를 생성한다. |
| Thread(String name) | name이라는 이름을 갖는 스레드를 생성한다. |
| Thread(ThreadGroup group, Runnable target) | 매개 변수로 받은 group의 스레드 그룹에 속하는 target 객체의 run() 메소드를 수행하는 스레드를 생성한다. |
| Thread(ThreadGroup group, Runnable target, String name) | 매개 변수로 받은 group 스레드 그룹에 속하는 target 객체의 run() 메소드를 수행하고, name 이라는 이름을 갖는 스레드를 생성한다. |
| Thread(ThreadGroup group, Runnable target, String name, long stackSize) | 매개 변수로 받은 group 스레드 그룹에 속하는 target 객체의 run() 메소드를 수행하고, name이라는 이름을 갖는 스레드를 생성한다.단, 해당 스레드의 스택의 크기는 stackSize 만큼만 가능하다. |
| Thread(ThreadGroup group, String name) | 매개 변수로 받은 group 스레드 그룹에 속하는 name 이라는 이름을. 갖는 스레드를 생성한다. |
- Thread 의 이름을 지정하지 않으면 “Thread-n” 이다.

 Stacksize 라는 값은 스택의 크기를 이야기 한다. 스레드에서 얼마나 많은 메소드를 호출하는데, 얼마나 많은 스레드가 동시에 처리되는지는 jvm이 실행되는 os의 플랫폼에 따라 매우 다르다. 경우에 따라서 이 값이 무시될 수 있다.

> 스택은 Collection 설명 시 이야기한 stack이라는 클래스의 전혀 상관 없다. 자바 프로세스가 시작되면 실행 데이터 공간(Runtime data area)이 구성된다. 그 중 하나가 스 책이라는 공간이며, 스레드가 생성될 때마다 별도의 스택이 할당된다.
> 

---

## 많이 사용되는**Sleep method**

: Thread 클래스에는 deprecated 된 메소드도 많고, static 메소드도 많이 있다.

- deprecated 된 메서드 “ 더 이상 사용하지 않는 것”이라는 의미다.
- static method : 객체를 생성하지 않아도 사용할 수 있는 메소드.

Thread에 있는 static 메소드는 대부분 해당 스레드를 위해서 존재하는 것이 아니라 , jvm에 있는 스레드를 관리하기 위한 용도로 사용된다. 물론 예외도 있다.그중 하나가 sleep() 메소드이다.

| method name & parameters | Return type | Description |
| --- | --- | --- |
| sleep(long millis) | Static void | 매개 변수로 넘어온 시간(1/1,000)초 만큼 대기한다. |
| sleep(long Millais, int nanos) | Static void | 첫 번째 매개 변수로 넘어온 시간(1/1000초) + 두 번째 매개변수로 넘어온 시간(1/1,000,000,000)만큼 대기한다 |

> run() 메소드가 끝나지 않으면, 애플리케이션은 끝나지 않는다. But 데몬 스레드는 예외다.
> 

---

### Thread 클래스의 주요 메소드

: Thread 클래스에 선언된 static도 아니고 deprecated도 아닌 메소드 중 주요 메소드에 대해 살펴보자.

- 스레드의 속성을 확인하고 지정하기 위한 메소드.
- 스레드의 상태를 통제하기 위한 메소드.

| Method name & parameters | Return type | description |
| --- | --- | --- |
| run() | void | 더 이상 설명이 필요 없는 여러분들이 구현해야 하는 메소드다. |
| getId() | long | 스레드에서 고유 id를 리턴한다. jvm에서 자동으로 생성해준다. |
| getName() | String | 스레드의 이름을 리턴한다. |
| getName(String name) | void | 스레드의 이름을 지정한다. |
| getPriority() | int | 스레드의 우선 순위를 확인한다. |
| setPriority(int newPriority) | void | 스레드의 우선 순위를 지정한다. |
| isDaemon() | boolean | 스레드가 데몬인지 확인한다. |
| setDaemon(boolean on) | void | 스레드를 데몬으로 설정할지 아닌지를 설정한다. |
| getStackTrace() | StackTraceElement[] | 스레드의 스택 정보를 확인한다. |
| getState() | Thread.State | 스레드의 상태를 확인한다. |
| getThreadGroup | ThreadGroup | 스레드의 그룹을 확인한다. |

스레드의 우선순위 라는 것이 등장하는데 대부분 이 값은 기본값으로 사용하는 것을 권장한다. (마음대로 우선순위를 정했다가는 잘못해서 장애로 연결된다. ) 

### 스레드 API

| 상수 | 값 및 설명 |
| --- | --- |
| MAX_PRIORITY | 가장 높은 우선 순위임, 그 값은 10이다. |
| NORM_PRIORITY | 일반 스레드의 우선 순위임, 그 값은 5다. |
| MIN_PRIORITY | 가장 낮은 우선 순위이며, 그 값은 1이다. |

우선순위를 정할 일이 있다면, 숫자로 정하는 것보다 이 상수를 이용할 것을 권장한다. 하지만, 우선 순위는 되도록 지정하지 않는 것이 좋다. 

---

### **Daemon Thread.**

- 데몬 스레드가 아닌 사용자 스레드는 JVM이 해당 스레드가 끝날 때까지 기다친다고 했다.
- Daemon Thread는 해당 쓰레드가 종료되지 않아도 다른 실행중인 일반 쓰레드가 없다면 멈춰 버린다.

when ? 

데몬 스레드는 가비지 수집, 사용하지 않는 개체의 메모리 해제 및 캐시에서 원하지 않는 항목 제거와 같은 백그라운드 지원 작업에 유용 하다. 대부분의 JVM 스레드는 데몬 스레드입니다.

why ? 왜 만들었을까

예를 들어 모니터링하는 스레드를 별도로 띄어 모니터링하다가, 주요 스레드가 종료되면 관련된 모니터링 스레드가 종료되어야 프로세스가 종료될 수 있다. 그런데, 모니터링 스레드를 데몬 스레드로 만들지 않으면 프로세스가 종료될 수 없게 된다.이렇게 부가적인 작업을 수행하는 스레드를 선언할 때는 데몬 스레드를 만든다.

---

**Synchronized** 

> **쓰레드에 안전하다는 것은 무엇일까?**
> 

: 어떤 클래스나 메서드가 쓰레드에 안전하려면, synchronized 를 사용해야 한다. 

**synchronized 사용 방법 2가지**

- 메서드 자체를 synchronized 로 선언하는 방법. (synchronized methods)
- 다른 하나는 메서드 내의 특정  문장만 synchronized로 감싸는 방법. (synchronized statemenets)

> synchronized는 여러 쓰레드에서 하나의 객체에 있는 인스턴스 변수를 동시에 처리할 때 발생하는 있는 문제를 해결하기 위해서 필요하다.
> 

- 인스턴스 변수가 선언되어 있다고 하더라도, 변수가 선언되어 있는 객체를 다른 쓰레드에서 공유할 일이 전혀 없다면 synchronized를 사용할 이유가 전혀 없다.
    - StringBuffer → synchronized 블록으로 주요 데이터 처리부를 감싼다.→ thread safe
    - Stringbuilder → 여러 쓰레드에서 공유할 일이 없을 때 사용한다.

---

### Thread control method

: 여러가지의 이유로 스레드를 통제해야 하는 경우가 있을 수 있다. 스레드의 상태를 통제하는 메소드.

| method name & parameters | return type | description |
| --- | --- | --- |
| getState() | Thread.State | 스레드의 상태를 확인한다. |
| join() | void | 수행자인 스레드가 중지할 때까지 기다린다. |
| join(long millis) | void | 매개 변수에 지정된 시간만큼(1/1,000초) 대기한다. |
| join(long millis, int nanos) | void | 첫 번째 매개 변수에 지정된 시간 (1/1000초) + 두 번 째 매개변수에 지정된 시간(1/1000,000,000초)만큼 대기한다. |
| interrupt() | void | 수행자인 스레드에서 중지 요청을 한다. |

먼저 getState() 메소드에서 리턴하는 Thread.State에 대해서 알아보자. 

자바의 Thread 클래스에는 State라는 enum 클래스가 있다.

| 상태 | 의미 |
| --- | --- |
| NEW | 스레드 객체는 생성되었지만, 아직 시작되지는 않은 상태 |
| RUNNABLE | 스레드가 실행중인 상태 |
| BLOKCED | 스레드가 실행 중지 상태이며, 모디너 각이 풀리기를 기다리는 상태 |
| WAITING | 스레드가 대기중인 상태 |
| TIMED_WAITING | 특정 시간만큼 스레드가 대기중인 상태 |
| TERMINATED | 스레드가 종료된 상태.  |

> JVM에서 사용되는 쓰레드의 상태들을 확인하기 위해서는 Thread 클래스의 static 메소드을 알아야 한다.
> 

---

### Thread 클래스에 선언되어 있는 상태 확인 메소드.

| method name & parameteres | return type | description |
| --- | --- | --- |
| checkAccess() | void | 현재 수행자인 스레드가 해당 스레드를 수정할 수 있는 권한이 있논지를 확인한다. 만약 권한이 없다면 SecurityException이라는 예외를 발생시킨다. |
| isAlive() | boolean | 스레드가 살아 있는지를 확인한다. 해당 스레드의 run() 메소드가 종료되었는지 안 되었는지를. 확인하는 것이다.  |
| isInterrupted() | boolean | run() 메소드가 정상적으로 종료되지 않고, interupt() 메소드의 호출을 통해서 종료되었는지를 확인하는데 사용한다. |
| interrupted() | static boolean | 현재 스레드가 중지되었는지를 확인한다. |

JVM에서 사용되는 스레드의 상대들을 확인하기 위해서는 Thread 클래스의 static 메소드들을 알아야 한다. 

| method name & paratmeters | return type | description |
| --- | --- | --- |
| activeCount() | static int  | 현재 스레드가 속한 스레드 그룹의 스레드 중 살아 있는 스레드의 개수를 리턴한다. |
| currentThread() | static Thread | 현재 수행자인 스레드의 객체를 리턴한다. |
| dumpStack() | static void  | 콘솔 창에 현재 스레드의 스택 정보를 리턴한다.  |

Thread 클래스에 선언되어 있는 메소드를 외에 Object 클래스에 있는 메소드들도 스레드를 통제하는데 사용한다.

### Object 클래스에 선언된 스레드의 관련있는 메소드들.

: wait() 메소드를 사용하면 스레드가 대기 상태가 되며, notify() 나 notifyAll() 메소드를 사용하면 스레드의 대기 상태가 해제된다 .

| method name & parameters | return type | description |
| --- | --- | --- |
| wait() | void | 다른 스레드가 Object 객체에 대한 notify() 메소드나 notifyAll 메소드를 호추ㄹ할 때까지 현재 스레드가 대기하고 있도록 한다. |
| wait(long timeout) | void | wait() 메소드의 동이란 기능을 제공하며, 매개 변수에 지정한 시간만큼만 대기한다. 즉, 매개 변수 시간을 넘어 섰을 때에는 현재 스레드는 다시 깨어난다. 여기서의 시간은 밀리오레 1/1,000초 단위다. 만약 1초간 기다리게 할 경우에는 1000을 매개 변수로 넘겨주면 된다. |
| wait(long timeout, int nanos) | void | wait() 메소드의 동일한 기능을 제공한다.하지만, wait(timeout)에서 밀리고 단위의 대치 시간을 기다린다면, 이 메소드는 보다 자세한 밀리고 + 나노초(1/1,000,000,000초) 만큼만 대기한다. 뒤에 있는 나노초의 값은 0~999,999 사이의 값만 지정할 수 있다. |
| notify() | void | Object 객체의 모니터에 대기하고 있는 단일 스레드를 깨운다. |
| notifyAll() | void | Object 객체의 모니터에 대기하고 있는 모든 스레드를 깨운다.  |

---

### ThreadGroup에서 제공하는 메소드들.

: ThreadGroup은 스레드의 관리를 용이하게 하기 위한 클래스다. 

- 하나의 애플리케이션에서 여러 종류의 스레드가 있을 수 있으며, 만약 ThreadGroup 클래스가 없으면 용도가 다른 여러 스레드를 관리하기 어려울 것이다.
- 스레드 그룹은 기본적으로 운영체제의 폴더처럼 뻗어나가는 트리구조를 가진다.

| method name & parameters | return type | description |
| --- | --- | --- |
| activeCount() | int | 실행중인 스레드의 개수를 리턴한다. |
| activeGroupCount() | int | 실행중인 스레드 그룹의 개수를 리턴한다 |
| enumerate(Thread[] list) | int | 현재 스레드 그룹에 있는 모든 스레드를 매개 변수로 넘어온 스레드 배열에 담는다. |
| enumerate(Thread[] list, boolean recurse) | int | 현재 스레드 그룹에 있는 모든 스레드를 매개 변수로 넘어온 스레드 배열에 담는다.두 번째 매개 변수가 true이면 하위에 있는 스레드 그룹에 있는 스레드 목록도 포함한다. |
| enumerate(ThreadGroup[] list) | int | 현재 스레드 그룹에 있는 모든 스레드 그룹을 매개 변수로 넘어온 스레드 그룹 배열에 담는다. |
| enumerate(ThreadGroup[] list, boolean recurse) | int | 현재 스레드 그룹에 있는 모든 스레드 그룹을 매개 변수로 넘어온 스레드 그룹 배열에 담는다.두 번 째 매개변수가 true이면 하위에 있는 스레드 그룹목록도 포함한다. |
| getName() | String | 스레드 그룹의 이름을 리턴한다 |
| getParent() | ThreadGroup | 부모 스레드 그룹을 리턴한다. |
| list() | void | 스레드 그룹의 상세 정보를 출력한다. |
| setDaemon(boolean daemon) | void | 지금 스레드 그룹에 속한 스레드들을 데몬으로 지정한다. |

> Thread에는 ThreadLocal이라는 클래스의 volatile이라는 예약어가 있다.
> 

