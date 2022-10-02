# ArrayInitValue
```java
public class ArrayInitValue {
	
    public void referenceTypesSetValue() {
        ArrayInitValue[] array = new ArrayInitValue[2];
        arr[0] = new ArrayInitValue();
        System.out.println(arr[0]);
        System.out.println(arr[1]);
    }
}
```
```text
ArrayInitValue@c17164
null
```
- 참조 자료형을 초기화하지 않으면 null이 출력된다.
- toString()을 오버라이딩 하지 않고, 참조 자료형을 출력하면 타입이름@고유번호가 출력된다.

# 배열을 그냥 출력
```java
public class ArrayPrint {
	public static void main(String[] args) {
		ArrayPrint array = new ArrayPrint();
		array.printString();
	}

	public void printString() {
		System.out.println("strings=" + new String[0]);
		System.out.println("array=" + new ArrayPrint[0]);
	}
}
```
```text
strings=[Ljava.lang.String;@2f7a2457
array=[LArrayPrint;@6108b2d7
```
- `[L` : `[`는 해당 객체가 배열이라는 의미. `L`은 해당 배열은 참조 자료형이라는 뜻
- java.lang.String : 해당 배열이 어떤 타입의 배열인지를 보여준다.
- @2f7a2457 : 해당 배열의 고유번호
```java
public void printPrimitiveArray() {
    System.out.println("byteArray=" + new byte[1]);
    System.out.println("shortArray=" + new short[1]);
    System.out.println("intArray=" + new int[1]);
    System.out.println("longArray=" + new long[1]);
    System.out.println("floatArray=" + new float[1]);
    System.out.println("doubleArray=" + new double[1]);
    System.out.println("charArray=" + new char[1]);
    System.out.println("booleanArray=" + new boolean[1]);
}
```
```text
byteArray=[B@edf4efb
shortArray=[S@566776ad
intArray=[I@1554909b
longArray=[J@6cd8737
floatArray=[F@13969fbe
doubleArray=[D@3498ed
charArray=[C@3d8c7aca
booleanArray=[Z@21bcffb5
```
- `[` 다음의 알파벳이 모두 다르다.

| boolean | byte | char | double | float | int | long | short |
|---------|------|------|--------|-------|-----|------|-------|
| Z       | B    | C    | D      | F     | I   | J    | S     |

