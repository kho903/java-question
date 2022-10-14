[GC Collection Algorithm (Java 9)](https://howtodoinjava.com/java/garbage-collection/all-garbage-collection-algorithms/)

### Object life cycle
1. object creation.  ( **new** keyword)
2. Object in use  
3. Object destruction


---
### Garbage collection algorithms

####  Mark and sweep ( runs in two stages: )

1. Marking live object 
2. Removing unreachable objects
    - Normal deletion
    
        ![image](https://user-images.githubusercontent.com/46278436/195804300-8224b997-4ca7-4d67-99e4-7ecb514090ca.png)


    - Deletion with compacting
    
        ![image](https://user-images.githubusercontent.com/46278436/195804478-93e21002-3c23-4c2d-b25f-3f60bcbb24e2.png)

    - Deletion with copying
    
       ![image](https://user-images.githubusercontent.com/46278436/195804535-f9844794-c36a-40f7-9b26-ced096616656.png)


---
[GC Algorithm implementations](https://plumbr.io/handbook/garbage-collection-algorithms-implementations)

## GC Algorithm : implementations

> An important aspect to recognize first is the fact that, for most JVMs out there, two different GC algorithms are needed <br>
> – one to clean the Young Generation and another to clean the Old Generation.


1. Serial GC
2. Parallel GC
3. Concurrent Mark and Sweep
4. G1 – Garbage First
