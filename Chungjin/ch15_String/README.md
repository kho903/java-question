### [QA / Why?]

- **“예전에는 UTF-8이나 EUC-KR을 많이 사용했지만, 요즘에는 대부분 UTF-16을 많이 사용한다.”** 
-<br>→ 왜 UTF-16을  많이 사용하는지?  [글1](https://redisle.tistory.com/14) , [글2](https://okky.kr/articles/991940?note=2419787)

- **한글이 깨지는 이유는 브라우저에서 생각하는 캐릭터 셋과 웹 페이지에 지정된 캐릭터 셋이 다르다.** 
<br> →  한글 캐릭터 셋?. [글1](https://sidepower.tistory.com/58), [글2(인코딩과 캐릭터셋의 차이)](https://dongwooklee96.github.io/post/2021/03/27/%EC%9D%B8%EC%BD%94%EB%94%A9%EA%B3%BC-%EC%BA%90%EB%A6%AD%ED%84%B0-%EC%85%8B%EC%9D%98-%EC%B0%A8%EC%9D%B4/)
- **“compareTo, ==, equals() 객체를 비교 해야 하는 상황에서 적절한 메소드는?"** →  [글1](https://www.nextree.co.kr/p11101/)

- 한글을 byte 배열로 만들 때 어떤 캐릭터 셋을 쓰느냐에 따라서 배열의 크기가 다르다. → Java Character Set ?  [글1](https://wrkbr.tistory.com/127)

---

> **public final class String extends object**
> 

### **1. 클래스가 final ?**

**Java 클래스가 final로 선언되어 있으면 더 이상 확장할 수 없다.** 

**String은 Serializable, Comparable<String>, CharSequence라는 인터페이스를 구현한 클래스이다.** 

- Serializable interaface : 구현해야 하는 메소드가 하나도 없다.  / 객체를 파일로 저장하거나 다른 서버에 전송 가능한 상태가 된다.
- comparable<String> - method : 이 메소드는 넘어가는 현재 객체가 같은지를 비교 하는데 사용한다.
- CharSequence - interface : 해당 클래스가 문자열을 다루기 위한 클래스라는 것을 명시적으로 나타내는데 사용한다.

ex) StringBuilder와 StringBuffer 클래스도 CharSequence 인터페이스를 구현해 놓았다. 

---

### 2. UTF-16 , EUC-KR

> **한글을 byte 배열로 만들 때 어떤 캐릭터 셋을 쓰느냐에 따라서 배열의 크기가 다르다.**
> 

: EUC-KR의 경우는 한글 두 글자를 표현하기 위해서 4바이트를 사용하지만, UTF-16은 6바이트를 사용한다는 점이다. 

---

### 3. 객체의 널 체크

: String 뿐만 아니라 모든 객체를 처리할 때에는 널 체크를 반드시 해야 한다. 

---

### **4. Immutable이란?**

**immutable이라는 말은 사전적인 의미로 “불변의”라는 의미다.** 

**다시 말해서 한 번 만들어지면 더 이상 값을 바꿀 수 없다.** 

---

### 5. CompareTo / Equals / ‘==’

compareTo 와 equals 와 ‘==’ 는 모두 같다 라는 의미이지만 차이가 있다.

- **CompareTo()  : 문자열 or 숫자의 비교**
    - return : -1(크다) 0(같다) 1(작다)
- **Equals() : 내용 자체 비교하여 결과를 Boolean 값으로 반환.**
- **== : 주소 값을 비교 결과를 Boolean 값으로 반환**
    - return : True(두 객체가 같은 주소값 가리킴) false

---

### 6. Equals    vs    ‘==’ 성능 차이

== 으로 비교하는 것이 훨씬 빠르다.
  
---

  
### 7. StringBuilder   vs  StringBuffer
  
 
속도는 Thread Safe 하지 않는 클래스가 더 빠르다.
  
StringBuffer : Thread Safe
StringBuilder : not Thread Safe
