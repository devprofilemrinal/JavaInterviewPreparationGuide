# Open Closed Principle

"Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification."

### Key Points

- **Open for Extension**: New behavior can be added to meet changing requirements
- **Closed for Modification**: Existing code remains unchanged
- **Promotes Stability**: Existing, tested code isn't touched
- **Reduces Risk**: Changes don't affect existing functionality

### Implementation Guidelines

Implementation Guidelines

1. Use Abstractions

- Define interfaces for behavior
- Program to interfaces, not implementations
- Use abstract classes for partial implementations


2. Apply SOLID Principles

- Combine with Single Responsibility Principle
- Use Dependency Inversion
- Follow Interface Segregation

3. Design Considerations

- Plan for extension points
- Use composition over inheritance
- Consider plugin architectures

4. Common Techniques

- Parameter injection
- Strategy injection
- Template methods
- Hook methods

### Benefits of Following OCP

1. Maintainability

- Easier to add new features
- Reduces risk of bugs in existing code
- Better code organization


2. Reusability

- Components can be reused in different contexts
- Promotes modular design
- Encourages interface-based programming


3. Flexibility

- Easy to adapt to changing requirements
- Supports multiple implementations
- Facilitates testing and mocking


4. Scalability

- New features don't require modifying existing code
- Easier to maintain as system grows
- Better support for parallel development

### Design Patterns Following OCP
1. Strategy Pattern
```java
interface SortStrategy {
    void sort(int[] array);
}

class QuickSort implements SortStrategy {
    public void sort(int[] array) { /* implementation */ }
}

class MergeSort implements SortStrategy {
    public void sort(int[] array) { /* implementation */ }
}
```
2. Template Method
```java
abstract class DataMiner {
    public final void mine() {
        openFile();
        extractData();
        parseData();
        closeFile();
    }
    
    abstract void extractData();
    abstract void parseData();
}
```
3. Observer Pattern
```java
interface Observer {
    void update(String message);
}

class Subject {
    private List<Observer> observers = new ArrayList<>();
    
    public void attach(Observer observer) {
        observers.add(observer);
    }
}
```
4. Decorator Pattern
```java
interface Coffee {
    double getCost();
}

class SimpleCoffee implements Coffee {
    public double getCost() { return 1.0; }
}

class MilkDecorator implements Coffee {
    private Coffee coffee;
    public MilkDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
    public double getCost() {
        return coffee.getCost() + 0.5;
    }
}
```
5. Factory Method
```java
abstract class LoggerFactory {
    abstract Logger createLogger();
}

class FileLoggerFactory extends LoggerFactory {
    Logger createLogger() {
        return new FileLogger();
    }
}
```
6. Bridge Pattern
```java
interface Color {
    String fill();
}

abstract class Shape {
    protected Color color;
    public Shape(Color color) {
        this.color = color;
    }
    abstract void draw();
}
```
7. Chain of Responsibility
```java
abstract class Handler {
    protected Handler successor;
    public void setSuccessor(Handler successor) {
        this.successor = successor;
    }
    abstract void handleRequest(Request request);
}
```