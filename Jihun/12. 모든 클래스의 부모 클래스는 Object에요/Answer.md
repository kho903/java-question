1. toString() 메소드가 자동으로 호출되는 2가지 경우는?
- System.out.println() 메소드에 매개 변수로 들어가는 경우
- 객체에 대하여 더하기 연산을 하는 경우

2. Object 클래스에 구현되어 있는 toString() 메소드는?
- getClass().getName() + '@' + Integer.toHexString(hashCode())

3. equals() 메소드를 override 할 때, 지켜야 할 5가지 규약은?
- 재귀 (reflective) : null이 아닌 x 객체의 x.equals(x)는 항상 true여야 한다.
- 대칭 (symmetric) : null이 아닌 x와 y 객체가 있을 때, x.equals(y)가 true이면 y.equals(x)도 반드시 true를 리턴해야 한다.
- 타동적 (transitive) : null이 아닌 x, y, z가 있을 때 x.equals(y)가 true, y.equals(z)가 true이면 x.equals(z)도 true를 리턴해야만 한다.
- 일관 (consistent) : null이 아닌 x, y가 있을 때, 객체가 변경되지 않은 상황에서는 몇 번을 호출하더라도 x.equals(y)의 결과는 항상 같아야 한다.
- null과의 비교 : null이 아닌 x 객체의 x.equals(null) 는 항상 false여야만 한다.

4. hashCode() 메소드 override 할 때, 지켜야 할 3가지 조건은?
- 자바 애플리케이션이 수행되는 동안에 어떤 객체에 대해서 이 메소드가 호출될 때에는 항상 동일한 int 값을 리턴해 주어야 한다. 하지만, 자바를 실행할 때마다
같은 값이어야 할 필요는 전혀 없다.
- 어떤 두 개의 객체에 대하여 equals() 메소드를 사용하여 비교한 결과가 true일 경우에, 두 객체의 hashCode() 메소드를 호출하면 동일한 int값을 리턴해야
한다.
- 두 객체를 equals() 메소드를 사용하여 비교한 결과 false를 리턴했다고 해서, hashCode() 메소드를 호출한 int 값이 무조건 달라야 할 필요는 없다.
하지만, 이 경우에 서로 다른 int 값을 제공하면 hashtable 의 성능을 향상시키는 데 도움이 된다.
