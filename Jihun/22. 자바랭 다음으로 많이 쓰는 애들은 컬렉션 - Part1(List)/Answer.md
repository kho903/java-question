1. 자바 컬렉션의 4가지 분류에 대해 말씀해 주세요.
- List, Set, Queue, Map
- List, Set, Queue 는 Collection 인터페이스를 구현했다.

2. Iterable 인터페이스에 선언된 메소드 1가지에 대해 설명해 주세요. 그리고 그 리턴타입에 대해 설명해주세요,
- Iterator iterator() 이고, Iterator 인터페이스에는 추가 데이터가 있는지 확인하는 hasNext(), 현재 위치를
다음 요소를 넘기고 그 값을 리턴해주는 next(), 데이터를 삭제하는 remove() 가 있습니다.
- 결론적으로, Collection 인터페이스가 Iterable 인터페이스를 확장했다는 의미는 Iterable 인터페이스를 사용하여
데이터를 순차적으로 가져올 수 있다는 의미

3. Collection을 확장한 다른 인터페이스와 List 인터페이스와 가장 큰 차이점은 무엇인가요?
- 배열처럼 "순서"가 있다는 것

4. ArrayList와 Vector는 거의 동일하고 기능도 거의 비슷한데 한가지 차이점이 무엇일까요?
- ArrayList의 경우 Thread safe 하지 않고, Vector는 Thread safe 합니다.

