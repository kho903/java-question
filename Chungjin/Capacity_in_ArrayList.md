[Java ArrayList performance Improvements in java](https://javarevisited.blogspot.com/2014/07/java-optimization-empty-arraylist-and-Hashmap-cost-less-memory-jdk-17040-update.html#axzz7hgQljeQT)

[Performance Evaluation of Java Arraylist](https://medium.com/@malith.jayasinghe/a-performance-evaluation-of-java-arraylist-f097582b5c4d)

[what is the default inital capacity of Arraylist, how it is resized and size is increased in java.](https://www.javamadesoeasy.com/2015/12/what-is-default-initial-capacity-of_6.html)


[ArrayList size increase in Java](https://moicapnhap.com/arraylist-size-increase-in-java)

## ArrayList size increase in Java

> ArrayList is a resizable array implementation in java. 

ArrayList class in java has 3 constructors. (It's own version of readObject and writeObject methods.)
1. RandomAccess
2. Cloneable
3. java.io.Serializable

--- 

[How ArrayList works Internally](https://www.javatpoint.com/arraylist-implementation-in-java)

### How ArrayList Works Internally

#### 1. java 7 or previous version

```java
public ArrayList(){
    this(10);
}
```


From the below code, we can see the array size will be equal to the specified Array.
```JAVA
public ArrayList(int initialCapacity){  //ArrayList constructor invokes with the intialCapacity argument for the specified Array  
      super();  
      if (initialCapacity < 0)  //if the value is less than 0, it will throw IllegalArgumentException  
      throw new IllegalArgumentException("Illegal Capacity: "+   initialCapacity);  
      this.elementData = new Object[initialCapacity];  
}  
```


#### 2. java 8 or later version.

```JAVA
private static final Object[] EMPTY_ELEMENTDATA = {}; //Initiliazing the arraylist EMPTY_ELEMENTDATA  
```

```java
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {}; // Initializing it with the default capacity 10 

public ArrayList(){
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA; // here, this keyword is returning the instance of ArrayList  
    }  

```

From the below code, we cae see the array size will be equals to the specified Array.
```java
public ArrayList(int initialCapacity) //ArrayList Constructor invokes with the initialCapacity argument for the initial Array  
{
        if (initialCapacity > 0) {  //if the value is grater than 0, it will create a new object  
            this.elementData = new Object[initialCapacity]; // new object creation  
        } else if (initialCapacity == 0) {  
            this.elementData = EMPTY_ELEMENTDATA;  
        } else {  
            throw new IllegalArgumentException("Illegal Capacity: "+  
                                               initialCapacity); // if the value is <= 0, it will throw an IllegalArgumentException  
        }  
  }  
```

---

### How the ArrayList Grows Dynamically ( In Java 7 or later )

```java
    public boolean add(E e) {  
        ensureCapacityInternal(size + 1);  //  modCount!! increments  
        elementData[size++] = e;  
        return true;  
    }  
      
    private void ensureCapacityInternal(int minCapacity) { // Detemining the current size of occupied elements  
  
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {  
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity); // here, Math.max will  return the maximum or largest value from the given arguments DEFAULT_CAPACITY AND minCapacity  
        }  
   
        ensureExplicitCapacity(minCapacity);  
    }  
      
    private void ensureExplicitCapacity(int minCapacity) {  
        modCount++; //modCount increases  
   
        // overflow-conscious code  
        if (minCapacity - elementData.length > 0)  
            grow(minCapacity);  
    }  
      
    private void grow(int minCapacity) { // The grow method expands the size of the Array  
  
        // overflow-conscious code  
        int oldCapacity = elementData.length;  
        int newCapacity = oldCapacity + (oldCapacity >> 1);  
        if (newCapacity - minCapacity < 0)  
            newCapacity = minCapacity;  
        if (newCapacity - MAX_ARRAY_SIZE > 0)  
            newCapacity = hugeCapacity(minCapacity); //minCapacity will be close to the size of the array  
        elementData = Arrays.copyOf(elementData, newCapacity); // The Arrays.copyOf will copy the specified array  
    }  
```

---

[Java ArrayList MAX SIZE](https://www.javaprogramto.com/2021/12/java-list-or-arraylist-max-size.html)

> ArrayList in Java has a get(int dex) method. <br>
> int is a signed 32 bit value, with a maximum value of 2,147,483,647

