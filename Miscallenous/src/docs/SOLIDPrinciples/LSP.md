# Liskov Substitution Principle (LSP) Explained

The **Liskov Substitution Principle (LSP)** is one of the five SOLID principles of object-oriented design. It states:

**"Objects of a superclass should be replaceable with objects of a subclass without altering the correctness of the
program."**

Let me explain this with a simple analogy, followed by a technical example.

### Everyday Analogy: The Vehicle Example

Imagine you have a program that works with different types of vehicles. The program assumes all vehicles can move.
Here's how LSP applies:

* **Superclass**: Think of a generic "Vehicle." A Vehicle has a method called `move()` that allows it to drive forward.
* **Subclass**: Now imagine you have specific types of vehicles, like a "Car" and a "Bicycle." Both are subclasses of "
  Vehicle" and can replace a "Vehicle" without breaking anything because they can move.

But what if you introduce a new subclass called "Boat"? If you try to replace the "Vehicle" with a "Boat" in your
program, and your program is on a road, the program will break because a boat cannot drive on a road.

The Liskov Substitution Principle tells us: **Don't create a subclass that doesn't behave like its parent class**.

---

### Key Points

- Behavioral Subtyping

    - Subtypes must behave like their base types
    - Clients should not need to know about the specific subtype
    - No unexpected behavior changes in derived classes


- Contract Conditions

    - Preconditions cannot be strengthened in subtypes
    - Postconditions cannot be weakened in subtypes
    - Invariants must be preserved in subtypes

### LSP Rules

- Method Arguments

```java
// Base class
class Animal {
    void feed(Food food) { /* implementation */ }
}

// Derived class - accepts same or more general types
class Dog extends Animal {
    @Override
    void feed(Food food) { /* implementation */ }
}
```

- Return Types

```java
// Base class
class Bird {
    Animal hunt() {
        return new Animal();
    }
}

// Derived class - returns same or more specific type
class Eagle extends Bird {
    @Override
    Fish hunt() {
        return new Fish();
    }  // Fish is an Animal
}
```

- Exception Handling

```java
// Base class
class FileProcessor {
    void process() throws IOException { /* implementation */ }
}

// Derived class - throws same or more specific exceptions
class ImageProcessor extends FileProcessor {
    @Override
    void process() throws FileNotFoundException { /* implementation */ }
}
```

The above rules are similar to what you can observe in case of inheritance behaviour between and parent and a child
class.

---

### Common LSP Violations and Solutions

1. Empty Implementations

```java
// Violation
class Bird {
    void fly() { /* implementation */ }
}

class Ostrich extends Bird {
    @Override
    void fly() {
    } // Empty implementation violates LSP
}

// Solution
interface Bird {
    void move();
}

interface FlyingBird extends Bird {
    void fly();
}
```

2. Type Checking

```java
// Violation
void processShape(Shape shape) {
    if (shape instanceof Square) {
        // Special handling for squares
    }
}

// Solution
interface Shape {
    void process();
}
```

---

### Design Guidelines

1. Tell, Don't Ask

```java
// Bad
if(employee instanceof Manager){
        ((Manager)employee).

assignWork();
}

// Good
interface Employee {
    void handleWork();
}
```

2. Use Composition Over Inheritance

```java
// Instead of inheritance
class Flying {
    void fly() { /* implementation */ }
}

class Bird {
    private Flying flying;  // Composition

    void performFly() {
        flying.fly();
    }
}
```

3. Program to Interfaces

```java
// Good practice
List<Shape> shapes = new ArrayList<>();  // Interface
shapes.

add(new Circle());
        shapes.

add(new Square());
```

### Benefits of LSP

1. **Code Reusability**

* Components are truly interchangeable
* Reduces code duplication
* Promotes modular design

2. **Maintenance**

* Easier to extend functionality
* Reduced coupling between classes
* Better error handling

3. **Testing**

* Simplified unit testing
* Better test coverage
* More reliable mocking

4. **Design Patterns Supporting LSP**

* Strategy Pattern
* Template Method Pattern
* Factory Pattern
* Decorator Pattern

