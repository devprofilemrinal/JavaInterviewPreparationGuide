# Memory Management in JVM: A Complete Guide
The JVM (Java Virtual Machine) memory management system is a cornerstone of Java's performance and reliability. It ensures efficient memory allocation, usage, and cleanup through features like automatic garbage collection and a well-defined memory structure.

## JVM Memory Architecture
The JVM divides memory into several regions, each serving a specific purpose. These regions include:

1. **Heap Purpose**:
   Stores all objects and class-level variables during runtime.
   The largest area of memory managed by the JVM.

   **Divisions**:
      - Young Generation:
         Subdivided into:
        -  Eden Space: Where all new objects are created.
        - Survivor Spaces (S0 and S1): Objects that survive garbage collection in Eden are moved here.
        Objects are promoted to the Old Generation if they survive multiple garbage collections.
        - Old Generation (Tenured Space):
        Stores long-lived objects (e.g., cached data, singletons).
        Managed by: Garbage collection (GC).
2. **Stack Purpose**:
   Stores method call frames, including:
   - Local variables.
   - Method parameters.
   - Return values.
   
   Memory is allocated for each thread independently.
   
   - Characteristics:
      - Thread-specific: Each thread gets its own stack.
      Memory is deallocated automatically when the method call completes.
   - Errors:
      - StackOverflowError: Occurs when the stack memory is exhausted, often due to infinite recursion.
      - NullPointerException: Occurs when referencing null objects.
3. **Method Area**
   - Purpose:
   Stores class-level information, including:
   Metadata about classes (e.g., methods, fields).
   Static variables.
   Constant pool (e.g., string literals).
   This region was previously part of the Permanent Generation (PermGen) in Java 7 and earlier but was replaced by Metaspace in Java 8.
   - Characteristics:
   Shared across all threads.
   Size can grow dynamically in Metaspace.
4. **Program Counter (PC) Register**
   - Purpose:
   Each thread has its own PC register to keep track of the current instruction being executed.
   - Characteristics:
   Stores the address of the currently executing JVM instruction.
   Essential for managing thread execution.
5. **Native Method Stack**
   - Purpose:
   Used for executing native methods (methods written in languages like C or C++).
   - Characteristics:
     - Works with native libraries outside the JVM.
     - Errors like OutOfMemoryError can occur if native memory is insufficient.
---
## Memory Allocation and Deallocation
1. **Memory Allocation**
   - Heap: Objects are allocated in the heap when the new keyword is used.
   - Stack: Method call frames (local variables and references) are allocated on the stack when methods are invoked.
   - Static variables: Allocated in the Method Area.
   

2. **Memory Deallocation**
   Managed automatically by the Garbage Collector (GC) for the heap.
   Stack memory is deallocated automatically when a method call completes.


### 1. Difference Between `StackOverflowError` and `OutOfMemoryError`

| **Aspect**              | **`StackOverflowError`**                                | **`OutOfMemoryError`**                             |
|--------------------------|--------------------------------------------------------|---------------------------------------------------|
| **Definition**           | Thrown when the **JVM stack** exceeds its limit.       | Thrown when the **heap memory** or **Metaspace** is exhausted. |
| **Cause**                | Caused by deep recursion or excessive method calls.    | Caused by creating too many objects, memory leaks, or insufficient heap size. |
| **Memory Region Affected** | JVM Stack (thread-specific memory).                   | Heap memory or Metaspace (shared memory).         |
| **Error Type**           | Relates to stack overflow during method calls.         | Relates to insufficient memory for objects or class metadata. |
| **Fix**                  | - Optimize recursive methods.                          | - Increase stack size with `-Xss`.               | - Optimize heap usage or increase heap size with `-Xmx`. |


