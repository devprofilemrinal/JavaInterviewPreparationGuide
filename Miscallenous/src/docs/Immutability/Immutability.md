# Immutability

---

## Immutable Object

An immutable object is an object that cannot be changed (its state is fixed) once it is
created.
In Java, the following applies:

- Primitive types are immutable.
- The famous Java String class is immutable (other classes are immutable as well, for example, Pattern, and LocalDate)
- Arrays are not immutable.
- Collections can be mutable, unmodifiable, or immutable.

> **Important Note:**
> An unmodifiable collection is not automatically immutable. It depends on which
> objects are stored in the collection:
> - If the stored objects are mutable, then the collection is mutable and unmodifiable
> - But if the stored objects are immutable, then the collection is effectively immutable

## String Immutability

In Java, strings are not represented by a primitive type like int, long, and float.
They are represented by a reference type named String. Almost any Java application uses strings, for example, the main()
method of a Java application gets as an argument an array of the String type.

Because of its wide usage String is made immutable in Java.

### Pros of String Immutability

1. **String constant pool or cached pool**
   One of the reasons in favor of string immutability is represented by the string
   constant pool (SCP) or cached pool.
   The SCP is a special area in memory (not the normal heap memory) used for the
   storage of string literals. Let's assume the following three String variables:
   ```text
   String x = "book";
   String y = "book";
   String z = "book";
   ```

**How many String objects have been created?**
It is tempting to say three, but actually Java creates only one String object with the "book" value. The idea is that
everything between quotes is considered as a string literal, and Java stores string literals in this special area of
memory called the SCP, by following an algorithm like this (this algorithm is known as string interning):

* When a string literal is created (for example, String x = "book"), Java inspects the SCP to see whether this string
  literal exists.
* If the string literal is not found in the SCP, then a new string object for the string literal is created in the SCP
  and the corresponding variable, x, will point to it.
* If the string literal is found in the SCP (for example, String y = "book", String z = "book"), then the new variable
  will point to the String object (basically, all variables that have the same value will point to the same String
  object):

![Object Creation](src/docs/Immutability/images/img.png "Object Creation")

* If you change x from "book" to "cook", y and z will still remain unchanged and this behaviour is provided by
  immutability. So, string immutability permits the caching of string literals, which allows applications to use a large
  number of string literals with a minimum impact on the heap memory and garbage collector. In a mutable context, a
  modification of a string literal may lead to corrupted variables.

2. **Security:**

   Another benefit of string immutability is its security aspect. Commonly, a lot of
   sensitive information (usernames, passwords, URLs, ports, databases, socket
   connections, parameters, properties, and so on) are represented and passed around as
   strings. By having this information immutable, the code becomes secure to a wide range of security threats (for
   example, modifying the references accidentally or deliberately).
3. **Thread Safety:**

   Imagine an application using thousands of mutable String objects and dealing with
   thread-safety code. Fortunately, in this case, our imagined scenario will not become a
   reality, thanks to immutability. Any immutable object is thread-safe by its nature.
   This means that strings can be shared and manipulated by multiple threads, with no
   risk of corruption and inconsistency.
4. **HashCode Caching:**
   The equals() and hashCode() section discussed equals() and hashCode(). Hash codes
   should be calculated every time they are involved in hashing specific activities (for
   example, searching an element in a collection). Since String is immutable, every
   string has an immutable hash code that can be cached and reused as it cannot be
   changed after string creation. This means that hash codes of strings can be used from
   the cache instead of recalculating them at each usage. For example, HashMap hashes
   its keys for different operations (for example, put(), get()), and if these keys are of
   the String type, then hash codes will be reused from the cache instead of
   recalculating them.
5. **Class Loading:**
   A typical approach for loading a class in memory relies on calling
   the Class.forName(String className) method. Notice the String argument
   representing the class name. Thanks to string immutability, the class name cannot be
   changed during the loading process. However, if String is mutable, then imagine
   loading class A (for example, Class.forName("A")), and, during the loading
   process, its name will get changed to BadA. Now, the BadA objects can do bad things!

### Cons Of String Immutability

1. **String cannot be extended:**
   An immutable class should be declared final to avoid extensibility. However,
   developers need to extend the String class in order to add more features, and this
   limitation can be considered a drawback of immutability.


2. **Sensitive data in memory for a long time:**
   Sensitive data in strings (for example, passwords) may reside in memory (in SCP) for
   a long time. Being a cache, the SCP takes advantage of special treatment from the
   garbage collector. More precisely, the SCP is not visited by the garbage collector with
   the same frequency (cycles) as other memory zones. As a consequence of this special
   treatment, sensitive data is kept in the SCP for a long time, and can be prone to
   unwanted usages.
   In order to avoid this potential drawback, it is advisable to store sensitive data (for
   example, passwords) in char[] instead of String.


3. **OutOfMemoryError:**
   The SCP is a small memory zone in comparison with others and can be filled pretty
   quickly. Storing too many string literals in the SCP will lead to OutOfMemoryError.