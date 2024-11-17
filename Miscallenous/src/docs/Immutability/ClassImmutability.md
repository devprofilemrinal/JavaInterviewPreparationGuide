# Class Immutability

An immutable class must respect several requirements, such as the following

- The class should be marked as final to suppress extensibility (other classes cannot extend this class; therefore, they cannot override methods)
- All fields should be declared private and final (they are not visible in
  other classes, and they are initialized only once in the constructor of this
  class)
- The class should contain a parameterized public constructor (or a
  private constructor and factory methods for creating instances) that initializes the fields
- The class should provide getters for fields
- The class should not expose setters

For example:
[Point](src/main/java/Immutability/Point.java)

