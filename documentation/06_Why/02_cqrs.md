# Why CQRS

## What is CQRS?

Command Query Responsibility Segregation (CQRS) is a design pattern that separates read (query) and write (command)
operations into distinct models. This segregation allows for optimized and scalable handling of commands and queries.

**Key Principles of CQRS:**

- **Commands**: Modify the state of the application. Each command represents an action or a change.
- **Queries**: Retrieve information without modifying the state. Queries are optimized for read operations.

## Benefits of CQRS

1. **Scalability**:
    - Separating reads and writes allows each to be independently scaled, which is particularly useful in high-load
      scenarios. For example, read operations can be scaled out with replicas, while write operations can be handled
      with dedicated resources.

2. **Performance Optimization**:
    - Queries can be optimized for fast read performance, while commands can be optimized for write efficiency. This
      leads to better overall system performance.

3. **Flexibility and Extensibility**:
    - CQRS allows for the use of different data models for reading and writing. This means you can use a highly
      normalized model for writes and a denormalized model for reads, optimizing each for their specific use cases.

4. **Simplified Complexity**:
    - By separating commands and queries, each part of the system becomes simpler and more focused. This separation
      reduces the cognitive load on developers, making the system easier to understand and maintain.

5. **Enhanced Security**:
    - The separation allows for more granular security controls. Commands can have strict validation and authentication,
      while queries can be more permissive.

## Why CQRS Fits Our Architecture

We draw inspiration from Domain-Driven Design (DDD) and event storming in our modeling tool. Adopting CQRS fits well
with those concepts and aiding the domain narative. Additionally, we can leverage the benefits of CQRS and the good
fit with event-driven architectures.

![image](../images/event-storming.png)

### Conclusion

Choosing CQRS allows us to build a robust, scalable, and maintainable system that leverages the strengths of both Event
Sourcing and modern cloud infrastructure. This decision aligns with our goal of simplifying complex system design for
domain experts, enabling them to focus on delivering business value.