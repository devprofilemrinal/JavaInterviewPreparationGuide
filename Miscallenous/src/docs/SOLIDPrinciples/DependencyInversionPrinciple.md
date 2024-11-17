# Dependency Inversion Principle

The Dependency Inversion Principle (DIP) is the "D" in the SOLID principles of object-oriented design. It focuses on
reducing the coupling between high-level modules and low-level modules by introducing abstractions.

## The Principle:

High-level modules should not depend on low-level modules. Both should depend on abstractions.

Abstractions should not depend on details. Details should depend on abstractions.

## What Does This Mean?

In traditional software design, higher-level components often directly depend on lower-level components. This creates a
tight coupling, making the system harder to change or extend. DIP flips this relationship by introducing abstractions
that both high- and low-level components depend on.

## Everyday Analogy: The Electrical Socket

Imagine you’re designing an electrical system:

- High-level module: Your device (e.g., a smartphone).
- Low-level module: The wall socket (provides power).
- Without DIP: Your smartphone would directly depend on the wall socket’s specifics (e.g., shape, voltage).
- With DIP: Instead of coupling directly to the socket, both the smartphone and the socket depend on a universal plug
  standard (abstraction). This way, any device with the correct plug can use the socket, and the socket design can
  evolve independently of the devices.

## Technical Explanation

In programming terms:

- High-level modules are policies or business rules (e.g., a payment processing system).
- Low-level modules are specific implementations (e.g., a PayPal payment gateway).

## Bad Example (Violates DIP):

Here, the OrderService directly depends on the concrete PayPalPaymentProcessor class:

```java
class PayPalPaymentProcessor {
    public void processPayment(double amount) {
        System.out.println("Processing payment of $" + amount + " via PayPal.");
    }
}

class OrderService {
    private PayPalPaymentProcessor paymentProcessor;

    public OrderService() {
        this.paymentProcessor = new PayPalPaymentProcessor();
    }

    public void placeOrder(double amount) {
        paymentProcessor.processPayment(amount);
    }
}
```

### Problems:

- If you need to switch to another payment processor (e.g., Stripe), you must modify OrderService.
- Testing OrderService in isolation becomes harder because it is tightly coupled to PayPalPaymentProcessor.

## Good Example (Adheres to DIP):

Use an abstraction (interface) to decouple OrderService from specific payment processors.

```java
interface PaymentProcessor {
    void processPayment(double amount);
}

class PayPalPaymentProcessor implements PaymentProcessor {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing payment of $" + amount + " via PayPal.");
    }
}

class StripePaymentProcessor implements PaymentProcessor {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing payment of $" + amount + " via Stripe.");
    }
}

class OrderService {
    private PaymentProcessor paymentProcessor;

    public OrderService(PaymentProcessor paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }

    public void placeOrder(double amount) {
        paymentProcessor.processPayment(amount);
    }
}

```

Now, OrderService depends on the PaymentProcessor interface, not a specific implementation. You can easily switch
between PayPalPaymentProcessor and StripePaymentProcessor.

## Key Components of DIP

- Abstraction (Interface or Abstract Class):

    - Serves as a contract that defines behavior without specifying how it’s implemented.
    - High-level modules rely on this abstraction, not on concrete implementations.
- Dependency Injection:

    - Dependencies are provided to a class, often through its constructor, setter methods, or fields, rather than being
      instantiated inside the class.
    - This makes swapping implementations easy.

- Inversion of Control (IoC):

    - Shifts the responsibility of creating dependencies from the class to a container or external code.



