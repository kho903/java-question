# 변수
- 어떤 프로그래밍 언어든 내용을 어디엔가 담아 둬야 한다. 담아 두는 것을 변수(variable)라고 한다.
그리고 변수는 항상 이름을 정해 주어야 한다.

## 자바의 네가지 변수
- 지역 변수 (local variables) : 중괄호 내에서 선언된 변수
- 매개 변수 (parameters) : 메소드에 넘겨주는 변수
- 인스턴스 변수 (instance variables) : 메소드 밖, 클래스 안에 선언된 변수 (static이 없어야 함)
- 클래스 변수 (class variables) : 메소드 밖, 클래스 안에 선언된 변수. 타입 선언 앞에 static 예약어가 붙은 변수
```java
public class VariableTypes {
    int instanceVariable;
    static int classVariable;
    public void method(int paramter) {
        int localVariable;
    }
}
```
### 네가지 변수의 생명주기
- 지역 변수 : 지역 변수를 선언한 중괄호 내에서만 유효
- 매개 변수 : 메소드가 호출될 때 생명이 시작되고, 메소드가 끝나면 소멸
- 인스턴스 변수 : 객체가 생성될 때 생명이 시작되고, 그 객체를 참조하고 있는 다른 객체가 없으면 소멸
- 클래스 변수 : 클래스가 처음 호출될 때 생명이 시작되고, 자바 프로그램이 끝날 때 소멸

## 변수 이름 규칙
- 길이의 제한은 없다.
- 첫 문자는 유니코드 문자, 알파벳, `$`, `_`만 올 수 있다. 
but, 보통 변수 이름은 일반적으로 `$`, `_`로 시작하지 않는다.
- 두 번째 문자부터는 유니코드 문자, 알파벳, 숫자, `$`, `_` 중 아무거나 사용 가능
- 보통은 메소드 이름처럼 지정해서 사용. 첫 문자는 소문자, 두 번째 단어의 첫 문자만 대문자로 시작
- 상수(constant value)의 경우 모두 대문자로 지정. 단어와 단어 사이는 `_`로 구분.
  - 상수란 절대 변하지 않는 값을 이야기한다.

# 자바의 자료형 2가지
- 자바는 기본 자료형 (Primitive type)과 참조 자료형(Reference data type)으로 나뉜다.
- 일반적으로 new를 사용해서 초기화하는 것을 참조 자료형, 그렇지 않고 바로 초기화 가능한 것을 기본 자료형.
  (String은 참조 자료형이지만, new를 사용해서 객체를 생성하지 않아도 되는 유일한 타입)

## 기본 자료형은 8개
숫자와 boolean 타입 두 가지로 나뉜다. 그리고, 숫자는 정수형과 소수형으로 나뉜다.
- 정수형 : byte, short, int, long, char
- 소수형 : float, double
- 기타 : boolean

## 기본 자료형의 범위
- byte : -2<sup>7</sup> ~ 2<sup>7</sup> - 1
- short : -2<sup>15</sup> ~ 2<sup>15</sup> - 1
- int : -2<sup>31</sup> ~ 2<sup>31</sup> - 1
- long : -2<sup>63</sup> ~ 2<sup>63</sup> - 1
- char : 0 ~ 2<sup>16</sup> - 1

