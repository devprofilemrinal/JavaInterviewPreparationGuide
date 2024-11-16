# Advanced Questions

### Question 1: What is the purpose of `WeakHashMap` in Java?

**Answer**:
`WeakHashMap` is a type of `Map` that uses **weak references** for its keys. If a key is no longer strongly referenced elsewhere in the application, it becomes eligible for garbage collection, and its entry is automatically removed from the map.

### **Use Case**:
- Caching data where entries should be removed when the key is no longer referenced.

### **Example**:
```java
import java.util.WeakHashMap;

public class WeakHashMapExample {
    public static void main(String[] args) {
        WeakHashMap<Object, String> map = new WeakHashMap<>();
        Object key = new Object();
        map.put(key, "Value");

        System.out.println("Before GC: " + map);
        key = null; // Remove strong reference
        System.gc();
        System.out.println("After GC: " + map);
    }
}
```
### Question 2: What is the default garbage collector in Java 8, Java 11, and Java 17?

**Answer**:

**Java 8**:
- **Default GC**: Parallel GC (for throughput).

**Java 11**:
- **Default GC**: G1 Garbage Collector.

**Java 17**:
- **Default GC**: G1 Garbage Collector.

