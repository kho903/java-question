### [Shallow & Deep Copies](https://towardsdatascience.com/shallow-deep-copies-stop-making-these-slicing-mistakes-12d02ffa2f7f)

> Shallow and deep coopies are essential to understand when writing efficient code and managing a limited memory properly.


### [Deep / shallow / Lazy Copy With Java Examples](https://www.geeksforgeeks.org/deep-shallow-lazy-copy-java-examples/)

#### Three ways for creating copy object
- shallow 

![image](https://user-images.githubusercontent.com/46278436/196031307-8662eb93-763b-41de-bf7b-69bf0c5ef475.png)

```java
public class ShallowEx {
    private int[] data; 
    
    // make a shallow copy of values
    public shallowEx(int[] values){
        data = values;
    }
    
    public void showData(){
        System.out.println( Arrays.toString(data) );
    }
}
```
```java
public class ShallowMain{
    public static void main(String[] args){
         int[] vals = {3, 7, 9};
         ShallowEx e = new ShallowEx(vals);
         e.showData(); // prints out [3, 7, 9] 
         vals[0] = 13; 
         e.showData(); // prints out [13, 7, 9] 
    }
}
```

- deep 

![image](https://user-images.githubusercontent.com/46278436/196031438-caa50aa4-c173-4725-aaa6-b1880bfe378e.png)

```java
public class DeepCopy {
    private int[] data; 
     
    // altered to make a deep copy of values
    public DeepCopy(int[] values){
        data = new int[values.length];
        for(int i = 0; i < data.length; i++){
            data[i] = values[i];
        }
    }
    public void showData(){
        System.out.println(Arrays.toString(data));
    }
}
```
```java


public class DeepCopyMain{ 
  
    public static void main(String[] args) { 
        int[] vals = {3, 7, 9}; 
        DeepCopy e = new DeepCopy(vals); 
        e.showData(); // prints out [3, 7, 9] 
        vals[0] = 13; 
        e.showData(); // prints out [3, 7, 9] 
  
       // changes in array values will not be  
       // shown in data values.  
    } 
} 
```


- lazy copy ( shallow + deep )


---

### [An alternative to the deep copy technique](https://www.infoworld.com/article/2077578/java-tip-76--an-alternative-to-the-deep-copy-technique.html)


### [Cloneable interface in java](https://www.geeksforgeeks.org/cloneable-interface-in-java/)
> This interface allows the implementing class to have its objects to be cloned instead of using a new operator.



### [How to Make a Deep copy of an Object in java - Baeldung](https://www.baeldung.com/java-deep-copy)
> learn four methods to implement the deep copy.
1. Copy constructor
2. Cloneable Interface
"To achieve a deep copy. we can serialize an object and then deserialize it to a new object"
3. Apache commons Lang
4. JSON Serialization with gson / Json Serialization with Jackson

> Maven dependencies
- gson
- commons-lang
- jackson-databind
