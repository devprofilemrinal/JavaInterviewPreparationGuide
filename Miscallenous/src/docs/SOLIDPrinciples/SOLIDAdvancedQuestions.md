# SOLID Principles & Core Java - Advanced Interview Questions

## LSP

### 1. How does LSP apply to Java’s Collection framework? Are there cases of violations?

Answer. The **Java Collection framework** adheres to the Liskov Substitution Principle (LSP) by allowing interchangeable
use of collections (e.g., `List`, `Set`, `Map`). You can use any implementation (e.g., `ArrayList`, `HashSet`) in place
of their parent interface (`List`, `Set`, etc.) without changing the program’s correctness.

#### Example:

```java
List<String> list = new ArrayList<>();
list.add("item");

list =new LinkedList<>(); // Can replace ArrayList with LinkedList
list.add("another item");
```
This works seamlessly because `ArrayList` and `LinkedList` both adhere to the contract defined by the List interface.

A common LSP violation arises when certain subclass implementations introduce unexpected behavior. For instance, UnsupportedOperationException in unmodifiable collections can break expectations.

For example:
```text
List<String> unmodifiableList = Collections.unmodifiableList(new ArrayList<>());
unmodifiableList.add("item"); // Throws UnsupportedOperationException

```
Here, `unmodifiableList` is a subclass of `List`, but it violates LSP because it doesn’t fully implement the `add` behavior expected of a `List`.

### 2. What are the risks of violating LSP in dependency injection?
Dependency Injection (DI) is heavily reliant on polymorphism, which assumes LSP compliance. Violating LSP in DI can lead to:

Risks:
1. **Incorrect Behavior**: If a substituted implementation of a dependency doesn’t behave as expected, the application logic might break. Example: A PaymentService interface with a pay() method might have a BankPaymentService and a MockPaymentService. If the mock behaves differently (e.g., skips error handling), it may cause unexpected results in production.

2. **Testing Challenges**: LSP violations in dependencies can lead to unreliable tests, as mocks or stubs might behave differently than real implementations.

3. **Coupling Issues**: Violations can lead to tightly coupled classes, negating the benefits of DI. For example, a service may expect specific behaviors from injected dependencies, forcing you to check types (e.g., instanceof), which defeats polymorphism.

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
animal.eat("plants"); // Throws an exception, violating LSP.
```
2. Weaker Postconditions: A subclass provides a weaker result than its parent class. For example, if a parent method guarantees returning a valid result, but the overridden method in the subclass does not, LSP is violated.

3. Return Type Issues: Java allows covariant return types, but violating type expectations can break LSP.

```java
class Parent {
    Number getValue() { return 42; }
}

class Child extends Parent {
    @Override
    Integer getValue() { return null; } // Weaker postcondition violates LSP.
}
```
4. Exception Handling: Subclasses throw exceptions not allowed by the parent class. This violates the expected contract.

### 4. Explain why "design by contract" is important for LSP and how it relates to preconditions and postconditions.
Design by Contract ensures that:

- Preconditions (what must be true before a method executes) are not stricter in the subclass than in the parent class.
- Postconditions (what the method guarantees after execution) are at least as strong in the subclass as in the parent class.
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





