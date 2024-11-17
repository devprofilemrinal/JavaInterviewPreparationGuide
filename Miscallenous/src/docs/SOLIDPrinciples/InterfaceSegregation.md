# Interface Segregation Principle (ISP)

## Core Concepts
1. **Split Large Interfaces**
    * Break down fat interfaces into smaller, focused ones
    * Each interface should serve a specific purpose
    * Clients only implement methods they need

2. **Benefits**
    * Reduces coupling between components
    * Prevents unnecessary dependencies
    * Makes system more maintainable
    * Improves code reusability

## Design Patterns that Implement ISP

1. **Adapter Pattern**
    * Converts one interface into another
    * Allows incompatible interfaces to work together
    * Clients only see methods they need

2. **Facade Pattern**
    * Provides simplified interface to complex subsystem
    * Clients interact with minimal necessary methods
    * Hides unnecessary complexity

3. **Bridge Pattern**
    * Separates abstraction from implementation
    * Allows independent variation of each
    * Each interface focuses on specific functionality

4. **Command Pattern**
    * Encapsulates requests as objects
    * Each command has single responsibility
    * Clients only know about relevant command interface

5. **Observer Pattern**
    * Defines minimal interface for notification
    * Subjects and observers are loosely coupled
    * Observers only implement needed update methods