# java, javac 명령어
- java 명령을 실행하면 어떤 프로그램을 실행하라는 지시. 지시를 하지 않으면 실행하는 데 필요한 각종 옵션들이 출력됨
- 윈도우 기준으로 확장자가 exe, com, bat 중 하나로 끝나야 실행이 가능한데, 확장자를 붙이지 않아도 실행을 하려면 
해당 확장자가 있는 파일이르 Path 라는 경로에 추가하면 된다. Path는 `set %PATH%`로 확인 가능
- 에디터로 코드를 작성하고, 콘솔에서 컴파일 및 실행하면 된다. 컴파일은 javac 명령어, 실행은 java 명령어 사용.

## 컴파일
- 대부분의 프로그래밍 언어들은 텍스트로 된 파일로 실행할 수가 없다. 우리가 만든 텍스트 파일들을 컴파일이라는 단계를 거쳐야 프로그래밍 언어를
실행하는 프로그램이 실행을 할 수 있다.
- compile 단어 뜻은 '엮다'로, 내가 만든 프로그램 코드를 컴퓨터가 이해할 수 있도록 엮어주는 작업이 바로 컴파일.
- .java 소스 파일을 컴파일하면 .class 파일이 생성되어 디스크에 저장됨.
- 이 .class 파일은 바이너리 파일로 되어 있기 때문에 사람이 알아보기 어렵다.
- 컴파일을 하는 프로그램은 컴파일러, 자바에서는 javac.exe(맥/리눅스 : javac)가 그 역할.
- 컴파일을 할 때 규칙을 지키지 않았다면 오류를 내보내고, 컴파일이 되지 않음
- 컴파일을 마친 클래스 파일은 JVM에서 읽어서 운영체제에서 실행됨

## 바이너리(binary) 파일이란
- 바이너리 : 0과 1로 구성되어 있는 2진법 
- 바이너리 파일은 2진수로 채워져 있는 파일.
- 컴퓨터는 2진 파일을 읽는 것이 훨씬 빨라, 컴퓨터가 읽기 위한 파일들은 대부분 바이너리로 되어 있음

## 컴파일 예
- HelloGodOfJava.java
```java
public class HelloGodOfJava {
	
}
```
```shell
$ javac HelloGodOfJava.java

$ 
```
- 아무런 메시지가 뜨지 않는 것이 정상이고, 만약 존재하지 않는 파일을 컴파일하면 다음과 같은 메시지가 나타난다.
```shell
$ javac HelloBasic.java 
error: file not found: j.java
Usage: javac <options> <source files>
use --help for a list of possible options
```
- javac 명령어를 사용하면 텍스트로 만든 자바 파일을 컴파일러가 읽어서 오류를 확인하고, 모든 것이 정상이라면
해당 .java 파일의 이름으로 .class 파일이 생성될 것이다.
- 그리고 java 명령어로 실행한다.
```text
Exception in thread "main" java.lang.noSuchMethodError: main
```
- 그렇다면 noSuchMethodError : main 예외가 발생할 것이다.
- 만약 존재하지 않는 클래스 파일을 지정하면 java.lang.NoClassDefFoundError 예외가 발생할 것이다.
```shell
$ java HelloBasic
Exception in thread "main" java.lang.NoClassDefFoundError: HelloBasic
    ....
```
- 위에서 `Exception in thread "main" java.lang.noSuchMethodError: main`를 해결해보자.
- "실행을 목적으로 하는" 모든 자바 클래스는 main() 메소드가 반드시 있어야 한다.
```java
public static void main(String[] args) {

}
```
- public : 접근 제어자에 해당.
- static : "정적인"이라는 뜻. static으로 선언하면 객체를 생성하지 않아도 호출할 수 있다.
- void : 메소드 이름 바로 앞에 반환 타입을 지정해 주는데, 돌려줄 것이 없을 때, void
- main : 메소드 이름.
- (String[] args) : 매개 변수. main() 메소드에 전달되는 매개 변수는 반드시 String[] args 여야 한다.
- HelloGodOfJava에 해당 main을 추가하면 예외가 사라질 것이다.

# 메소드 정리
하나의 메소드는 여섯 부분으로 나뉠 수 있다.
1. 제어자(modifier) : main() 메소드에 있는 public static과 같은 메소드의 특성을 정하는 부분
2. 리턴 타입 (return type) : 메소드가 끝났을 떄 돌려주는 타입
3. 메소드 이름 (method name) : 소괄호 앞에 있는 메소드 이름
4. 매개 변수 목록 (parameter list) : 소괄호 안에 있는 매개 변수의 목록
5. 예외 목록 (exception list) : 메소드의 소괄호가 끝나는 부분과 중괄호가 시작하는 부분 사이에 예외 목록을 선언 가능
6. 메소드 내용 (method body) : 중괄호 안에 있는 내용
- 이 중 반드시 있어야 하는 것은 리턴타입, 메소드 이름, 메소드 내용이다.
