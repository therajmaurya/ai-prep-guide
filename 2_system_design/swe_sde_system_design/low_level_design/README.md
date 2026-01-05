---
title: "Low Level Design (LLD)"
category: system-design
levels: ["mid", "senior", "principal"]
skills: [lld, oop, design-patterns, clean-code]
questions:
  "mid": ["Explain SOLID principles.", "What is the Singleton pattern?", "Difference between Abstract Class and Interface."]
  "senior": ["Design a Parking Lot system.", "Implement a Thread-Safe Singleton.", "Explain Dependency Injection."]
  "principal": ["Design a Rate Limiter (Token Bucket).", "Design an extensible Plugin Architecture.", "Refactor a monolithic class into smaller components."]
---

# Low Level Design (LLD)

LLD focuses on the internal logic of components: Class diagrams, API signatures, and Algorithms. It's about writing clean, maintainable, and extensible code.

## Core Concepts

### 1. SOLID Principles
*   **S - Single Responsibility**: A class should have only one reason to change.
*   **O - Open/Closed**: Open for extension, closed for modification.
*   **L - Liskov Substitution**: Subtypes must be substitutable for their base types.
*   **I - Interface Segregation**: Clients shouldn't depend on interfaces they don't use.
*   **D - Dependency Inversion**: Depend on abstractions, not concretions.

### 2. Object-Oriented Programming (OOP)
*   **Encapsulation**: Hiding internal state.
*   **Inheritance**: Code reuse (Is-A relationship).
*   **Polymorphism**: Overriding (runtime) and Overloading (compile time).
*   **Abstraction**: Hiding complexity.

### 3. Design Patterns
*   **Creational**: Singleton, Factory, Builder.
*   **Structural**: Adapter, Decorator, Facade.
*   **Behavioral**: Observer, Strategy, Command.

## Advanced Topics

### 1. Concurrency
*   **Thread Safety**: Protecting shared resources (Locks, Semaphores, Atomic Variables).
*   **Deadlock**: Two threads waiting for each other forever.
*   **Race Condition**: Outcome depends on timing of threads.

### 2. API Design
*   **REST**: Resource-based (GET /users/1). Stateless.
*   **GraphQL**: Query language. Client asks for exactly what it needs.
*   **gRPC**: High performance RPC using Protocol Buffers.

### 3. UML Diagrams
*   **Class Diagram**: Static structure.
*   **Sequence Diagram**: Dynamic interaction over time.
*   **Use Case Diagram**: User interactions.

## Interview Questions

### Mid Level
1.  **Explain the Factory Pattern.**
    *   *Answer*: A creational pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created. Decouples creation logic from business logic.
2.  **What is the difference between Composition and Inheritance?**
    *   *Answer*: Inheritance is "Is-A". Composition is "Has-A". Composition is generally preferred ("Favor Composition over Inheritance") because it's more flexible and avoids the "Fragile Base Class" problem.
3.  **What is a Deep Copy vs Shallow Copy?**
    *   *Answer*: Shallow copy copies references. Deep copy copies the actual objects recursively.

### Senior Level
1.  **Design a Parking Lot System.**
    *   *Discussion*:
        *   **Classes**: `ParkingLot`, `Level`, `Spot`, `Vehicle` (Car, Bike, Truck), `Ticket`.
        *   **Enums**: `SpotType` (Compact, Large), `VehicleType`.
        *   **Logic**: `parkVehicle()` finds first available spot. `unparkVehicle()` calculates fee based on duration.
        *   **Concurrency**: Use locks when assigning a spot to prevent double booking.
2.  **Implement a Thread-Safe Singleton in Java/Python.**
    *   *Answer*: Use "Double-Checked Locking". Check if instance is null, synchronize (lock), check again, then create.
3.  **Explain the Strategy Pattern.**
    *   *Answer*: Defines a family of algorithms, encapsulates each one, and makes them interchangeable. Example: `PaymentStrategy` (CreditCard, PayPal, Bitcoin). Context class uses the strategy interface.

### Principal Level
1.  **Design a Rate Limiter.**
    *   *Discussion*:
        *   **Algorithms**: Token Bucket, Leaky Bucket, Fixed Window, Sliding Window Log.
        *   **Implementation**: Redis Lua script for atomicity.
        *   **Class Structure**: `RateLimiter` interface, `TokenBucket` implementation.
2.  **Refactor a "God Class" that does everything.**
    *   *Discussion*: Identify distinct responsibilities. Extract methods. Move methods to new classes. Use Dependency Injection to wire them back together. Apply SRP.
3.  **Design an Undo/Redo system.**
    *   *Discussion*: Use the **Command Pattern**. Each action is an object with `execute()` and `undo()` methods. Store executed commands in a Stack.
