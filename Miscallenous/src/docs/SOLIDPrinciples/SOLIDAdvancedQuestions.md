# SOLID Principles & Core Java - Advanced Interview Questions

---

## OCP

### 1. Explain how Java’s Comparator interface follows OCP.

The Comparator interface in Java is a great example of the Open-Closed Principle. It allows you to define custom
comparison logic for objects without modifying the existing classes.

- How it Follows OCP
    - The class being compared (e.g., Employee, Product) remains closed for modification. The Comparator provides a way
      to introduce new comparison logic by being open for extension through its implementation.
      For example :

```java
import java.util.*;

class Employee {
    String name;
    int age;

    Employee(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}

class AgeComparator implements Comparator<Employee> {
    @Override
    public int compare(Employee e1, Employee e2) {
        return Integer.compare(e1.age, e2.age);
    }
}

class NameComparator implements Comparator<Employee> {
    @Override
    public int compare(Employee e1, Employee e2) {
        return e1.name.compareTo(e2.name);
    }
}

public class Main {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
                new Employee("Alice", 30),
                new Employee("Bob", 25),
                new Employee("Charlie", 35)
        );

        Collections.sort(employees, new AgeComparator()); // Compare by age
        System.out.println(employees);

        Collections.sort(employees, new NameComparator()); // Compare by name
        System.out.println(employees);
    }
}

```

Here:

- The Employee class remains unchanged, adhering to the "closed for modification" principle.
- New comparison strategies can be added by implementing the Comparator interface.

### 2. How does OCP relate to Java’s enum types, and can they violate the principle?

Enums in Java define a fixed set of constants, which makes them inherently closed for modification. However, enums can
still follow OCP by allowing extension through additional methods or behavior.

**When Enums Violate OCP?**

Enums can violate OCP when you find yourself needing to add new constants to represent new states. Since enums are
immutable in terms of their constants, every addition requires modifying the enum class itself.

Example of OCP Violation:

```java
enum PaymentStatus {
    SUCCESS,
    FAILURE;
}
```

If you later need to add a new status (e.g., PENDING), you have to modify the PaymentStatus enum, violating OCP.

**How to Avoid Violations:**
Instead of modifying the enum, use a design that supports extension, like maintaining states in a configuration or
database.

Alternatively, you can extend behavior with methods inside the enum:

```java
enum Operation {
    ADD {
        @Override
        int apply(int x, int y) {
            return x + y;
        }
    },
    MULTIPLY {
        @Override
        int apply(int x, int y) {
            return x * y;
        }
    };

    abstract int apply(int x, int y);
}
```

Here, new operations can be added by extending the Operation enum behaviorally without changing existing constants.

### 3. Can OCP be applied to a final class in Java? Why or why not?

A final class in Java cannot be extended. This makes it inherently closed for modification but not open for extension.
OCP requires that a class should be extensible without modification. Since a final class cannot be extended, it cannot
directly adhere to OCP.

---

## LSP

### 1. How does LSP apply to Java’s Collection framework? Are there cases of violations?

Answer. The **Java Collection framework** adheres to the Liskov Substitution Principle (LSP) by allowing interchangeable
use of collections (e.g., `List`, `Set`, `Map`). You can use any implementation (e.g., `ArrayList`, `HashSet`) in place
of their parent interface (`List`, `Set`, etc.) without changing the program’s correctness.

#### Example:

```java
List<String> list = new ArrayList<>();
list.

add("item");

list =new LinkedList<>(); // Can replace ArrayList with LinkedList
        list.

add("another item");
```

This works seamlessly because `ArrayList` and `LinkedList` both adhere to the contract defined by the List interface.

A common LSP violation arises when certain subclass implementations introduce unexpected behavior. For instance,
UnsupportedOperationException in unmodifiable collections can break expectations.

For example:

```text
List<String> unmodifiableList = Collections.unmodifiableList(new ArrayList<>());
unmodifiableList.add("item"); // Throws UnsupportedOperationException

```

Here, `unmodifiableList` is a subclass of `List`, but it violates LSP because it doesn’t fully implement the `add`
behavior expected of a `List`.

### 2. What are the risks of violating LSP in dependency injection?

Dependency Injection (DI) is heavily reliant on polymorphism, which assumes LSP compliance. Violating LSP in DI can lead
to:

Risks:

1. **Incorrect Behavior**: If a substituted implementation of a dependency doesn’t behave as expected, the application
   logic might break. Example: A PaymentService interface with a pay() method might have a BankPaymentService and a
   MockPaymentService. If the mock behaves differently (e.g., skips error handling), it may cause unexpected results in
   production.

2. **Testing Challenges**: LSP violations in dependencies can lead to unreliable tests, as mocks or stubs might behave
   differently than real implementations.

3. **Coupling Issues**: Violations can lead to tightly coupled classes, negating the benefits of DI. For example, a
   service may expect specific behaviors from injected dependencies, forcing you to check types (e.g., instanceof),
   which defeats polymorphism.

### 3. What edge cases in method overriding could lead to LSP violations in Java?

Answer. Method overriding can cause LSP violations when:

1. Stricter Preconditions: A subclass imposes stricter conditions on its overridden method than the parent class.

```java
class Animal {
    void eat(String food) {
        System.out.println("Eating " + food);
    }
}

class Carnivore extends Animal {
    @Override
    void eat(String food) {
        if (!food.equals("meat")) {
            throw new IllegalArgumentException("Carnivores eat only meat!");
        }
        super.eat(food);
    }
}

Animal animal = new Carnivore();
animal.

eat("plants"); // Throws an exception, violating LSP.
```

2. Weaker Postconditions: A subclass provides a weaker result than its parent class. For example, if a parent method
   guarantees returning a valid result, but the overridden method in the subclass does not, LSP is violated.

3. Return Type Issues: Java allows covariant return types, but violating type expectations can break LSP.

```java
class Parent {
    Number getValue() {
        return 42;
    }
}

class Child extends Parent {
    @Override
    Integer getValue() {
        return null;
    } // Weaker postcondition violates LSP.
}
```

4. Exception Handling: Subclasses throw exceptions not allowed by the parent class. This violates the expected contract.

### 4. Explain why "design by contract" is important for LSP and how it relates to preconditions and postconditions.

Design by Contract ensures that:

- Preconditions (what must be true before a method executes) are not stricter in the subclass than in the parent class.
- Postconditions (what the method guarantees after execution) are at least as strong in the subclass as in the parent
  class.
- Why It Matters:
    - Preconditions: Subclasses must honor the parent’s expectations to remain interchangeable.
    - Postconditions: Subclasses must deliver results that meet or exceed the parent’s guarantees.
- Example:

```java


class Parent {
    int divide(int a, int b) {
        if (b == 0) throw new IllegalArgumentException("b cannot be zero");
        return a / b;
    }
}

class Child extends Parent {
    @Override
    int divide(int a, int b) {
        // Violates precondition: now b must be > 0, not just non-zero
        if (b <= 0) throw new IllegalArgumentException("b must be greater than zero");
        return a / b;
    }
}
``` 

Here, the child class introduces stricter preconditions, breaking the "contract."

---

## ISP

### 1. Do Marker Interfaces Violate ISP?

The Interface Segregation Principle (ISP) states that an interface should include only the methods that are relevant to
the specific client. Marker interfaces do not declare any methods, so technically, they do not force any class to
implement unused methods.

However:
Marker interfaces can violate the spirit of ISP because they do not provide actual behavior and may lead to confusion
about their purpose. Modern Java (from Java 5 onward) encourages using annotations (e.g., `@Deprecated,
@FunctionalInterface`) instead of marker interfaces, as annotations are more flexible and less intrusive.

### 2. How does ISP impact the design of callback mechanisms in Java?

Callback mechanisms in Java involve passing a reference (usually an interface or lambda) to another method or object to
execute specific code at a later time.

**How ISP Applies to Callbacks:**
When designing callback interfaces, adhering to ISP ensures that the interface contains only the methods relevant to the
specific callback operation. This avoids bloated callback interfaces that force the implementing class to define
unnecessary methods.

**Good Design (Follows ISP)**
```java
interface OnClickListener {
    void onClick();
}
```

**Bad Design (Violates ISP)**
```java
interface EventListener {
    void onClick();

    void onHover();

    void onFocus();
}
```

## General Java-Related Pitfalls and Edge Cases in SOLID
### Edge Case 1: 
**How would you apply SRP when a single class must handle multiple cross-cutting concerns like logging,
security, and transactions?**
Answer:
When a single class needs to handle **cross-cutting concerns** such as logging, security, and transactions, adhering to the **Single Responsibility Principle (SRP)** requires segregating these concerns into separate components. Here’s how this can be achieved:

#### **1. Identify the Cross-Cutting Concerns**
Cross-cutting concerns are functionalities that affect multiple parts of an application but are not directly related to the core responsibility of a class. Examples include:
- **Logging**: Tracking application behavior.
- **Security**: Authenticating and authorizing access.
- **Transactions**: Managing database transactions.

#### **2. Use Aspect-Oriented Programming (AOP)**
Aspect-Oriented Programming (AOP) allows you to separate cross-cutting concerns from business logic. In Java, frameworks like **Spring AOP** can help manage these concerns without violating SRP.

##### **Example: Using AOP for Logging**
```java
@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("Executing method: " + joinPoint.getSignature());
    }
}
```

### Edge Case 2:
**Discuss scenarios where adherence to SOLID principles could lead to overengineering in simple applications.**

Answer : Think by your selves

### Edge Case 3:
**How do SOLID principles interact with Java’s garbage collection? Does SRP lead to memory overhead due to many small objects?**
Answer:

The **SOLID principles** aim to improve code quality by promoting modularity, maintainability, and extensibility. However, their interaction with **Java's garbage collection (GC)** introduces considerations related to object lifecycles, memory overhead, and performance.

#### **1. Impact of SOLID on Object Creation**
SOLID principles encourage the use of modular and smaller objects, abstractions, and composition over inheritance. This can lead to:
- **More Objects in Memory**: Increased modularity often results in more objects being instantiated, as classes are broken into smaller, single-responsibility components.
- **Shorter Object Lifetimes**: Objects created for specific tasks (e.g., dependency injection, services) may have short lifetimes, making them quickly eligible for garbage collection.

#### **2. How GC Handles This**
Java’s garbage collector is designed to manage large numbers of short-lived objects effectively, particularly through the **Young Generation** of the heap:
- **Eden Space**: Most small, temporary objects are created here and collected quickly, minimizing the impact of increased object creation.
- **Survivor Spaces**: Objects that persist slightly longer are moved to these spaces before being promoted to the Old Generation.

#### **3. Example: Dependency Injection and Object Creation**
In adherence to the **Dependency Inversion Principle (DIP)**, frameworks like Spring create objects dynamically and manage their lifecycles. These objects are eligible for garbage collection when no longer referenced, assuming proper scope management.

---

#### Does SRP Lead to Memory Overhead Due to Many Small Objects?

#### **1. Why SRP Can Lead to More Objects**
The **Single Responsibility Principle (SRP)** encourages dividing a class into smaller, focused classes, each handling a single responsibility. While this improves modularity, it can lead to:
- **Increased Object Count**: More objects are created as responsibilities are split.
- **Indirect References**: Dependency injection or composition can result in more reference chains in memory.

#### **2. Memory Overhead Considerations**
- **Heap Usage**: A larger number of small objects increases the memory footprint, especially in applications with high concurrency or heavy object creation.
- **Pointer References**: Java's objects include metadata and pointer references, so even small objects consume additional memory.
- **Garbage Collection Load**: More frequent object creation leads to more frequent GC cycles, which may increase CPU load.

#### **3. Example: SRP and Small Object Creation**
```java
class Logger {
    public void log(String message) {
        System.out.println("Log: " + message);
    }
}

class EmailSender {
    private Logger logger;

    public EmailSender(Logger logger) {
        this.logger = logger;
    }

    public void sendEmail(String email, String message) {
        logger.log("Sending email to: " + email);
        System.out.println("Email sent to: " + email);
    }
}

public class Main {
    public static void main(String[] args) {
        Logger logger = new Logger();
        EmailSender emailSender = new EmailSender(logger);
        emailSender.sendEmail("example@example.com", "Hello!");
    }
}
```

### Edge Case 4:
**How does multi-threading affect compliance with SOLID principles, particularly SRP and DIP?**

### Edge Case 5:
**Discuss how SOLID principles might conflict with Java’s default serialization mechanisms.**

### Edge Case 6:
**How would you handle circular dependencies in a system adhering to DIP?**

### Edge Case 7:
**How does the use of reflection in Java challenge compliance with SOLID principles?**

### Edge Case 8:
**Discuss how adherence to ISP might conflict with backward compatibility in Java APIs.**

### Edge Case 9:
**How would you design a SOLID-compliant class hierarchy for file I/O operations in Java, given unpredictable
failures?**

### Edge Case 10:
**Can SOLID principles lead to slower performance in time-critical Java applications? How would you address
this?**






