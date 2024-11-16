# Single Responsibility Principle
## Core Concept:

- "A class should have one, and only one, reason to change"
- Each class should be responsible for a single part of the functionality
- The class should be entirely encapsulated around that responsibility


## Key Benefits:

- Easier to maintain and modify code
- Better code organization
- Reduced coupling between classes
- Easier to test
- More reusable components
- Better code readability

## How to Identify Responsibilities:

- Ask "What is the primary purpose of this class?"
- Look for words like "and" in class descriptions
- Consider different actors/stakeholders who might want changes
- Think about different reasons the code might need to change


## Signs of SRP Violations:

- Large classes with many methods
- Methods that don't use most of the class's fields
- Classes with multiple unrelated methods
- Classes that change for multiple reasons
- God classes that do everything

## Practical Application:
Instead of one large class handling multiple responsibilities:

- Separate data objects from behavior
- Create specific service classes for different operations
- Use composition to combine functionality
- Focus on cohesive functionality


## Real-world Analogy:
Think of a restaurant:

- Chef (cooking responsibility)
- Waiter (serving responsibility)
- Cashier (billing responsibility)
- Manager (staff management responsibility)

Each role has a single, well-defined responsibility.


## Guidelines for Implementation:

- Keep classes small and focused
- Name classes according to their responsibility
- Extract responsibilities to new classes when needed
- Use interfaces to define contracts
- Consider dependency injection for flexibility


## Common Mistakes to Avoid:

- Creating utility classes with unrelated methods
- Mixing business logic with data access
- Combining UI logic with business logic
- Having classes that do "and" things


## Refactoring Tips:

- Extract Method: Split large methods into smaller ones
- Extract Class: Move related methods to new classes
- Move Method: Relocate methods to more appropriate classes
- Interface Extraction: Define clear contracts


## Testing Benefits:

- Easier to write unit tests
- Better test coverage
- More focused test cases
- Reduced test setup complexity
- Clearer test intentions



## Remember:

- SRP doesn't mean a class can only do one thing
- It means a class should only have one reason to change
- Focus on cohesion and encapsulation
- Consider the impact of changes
- Think about maintenance and future modifications

```java
class Employee {
    private String name;
    private double salary;
    private String address;

    // Employee data management
    public void saveEmployee() {
        // Code to save employee to database
        String sql = "INSERT INTO employees (name, salary, address) VALUES (?, ?, ?)";
        // Database operations here
    }
    
    // Salary calculation
    public void calculateTax() {
        // Tax calculation logic
        double tax = salary * 0.2;
        // More tax calculations
    }
    
    // Report generation
    public void generateReport() {
        // Code to generate employee report
        String report = "Employee Report for: " + name;
        // More report generation code
    }
}
```
The above code has too many responsibilities like data mangagement, salary calculation,and report generation.
If you translate this into business, you have to change your class if requested by HR(data management), finance team(salary calculation) and delivery team(report generation)
giving the class more than one reason to change.

```java
// Good Example - Single Responsibility
class Employee {
    private String name;
    private double salary;
    private String address;

    // Only employee data management
    public String getName() { return name; }
    public double getSalary() { return salary; }
    public String getAddress() { return address; }
    
    public void setName(String name) { this.name = name; }
    public void setSalary(double salary) { this.salary = salary; }
    public void setAddress(String address) { this.address = address; }
}

class EmployeeRepository {
    public void save(Employee employee) {
        // Database operations
        String sql = "INSERT INTO employees (name, salary, address) VALUES (?, ?, ?)";
        // Save employee to database
    }

    public Employee findById(int id) {
        // Fetch employee from database
        return new Employee();
    }
}

class TaxCalculator {
    public double calculateTax(Employee employee) {
        // Tax calculation logic
        return employee.getSalary() * 0.2;
    }
}

class EmployeeReportGenerator {
    public String generateReport(Employee employee) {
        // Report generation logic
        return "Employee Report for: " + employee.getName();
    }
}

```
