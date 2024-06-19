# Why Event Sourcing

Event Sourcing is a powerful architectural pattern that has become an integral part of our system design. Here’s why we
chose Event Sourcing:

## Benefits of Event Sourcing

1. **Historical Accuracy and Auditing**:
    - **Complete History**: Every change to the application state is stored as an event, preserving a complete history
      of actions.
    - **Audit Trail**: This provides an accurate and immutable audit trail, crucial for compliance and debugging.

2. **Improved Consistency and Reliability**:
    - **Consistency**: Event Sourcing ensures that state changes are consistently recorded and applied in the order they
      occur.
    - **Reliability**: By replaying events, systems can recover from failures and ensure reliable state reconstruction.

3. **Enhanced Scalability and Performance**:
    - **Scalability**: Events can be processed asynchronously and in parallel, enhancing system scalability.
    - **Performance**: Read models can be optimized for query performance without affecting the write models, thanks to
      the separation of concerns.

4. **Flexibility and Extensibility**:
    - **Flexibility**: Event Sourcing supports evolving requirements by allowing new views or projections to be created
      from the event log.
    - **Extensibility**: New functionality can be added without changing existing events, enabling seamless system
      evolution.

5. **Compatibility with CQRS**:
    - **CQRS Alignment**: Event Sourcing naturally fits with the Command Query Responsibility Segregation (CQRS)
      pattern, enhancing our ability to separate read and write concerns.
    - **Behavior Modeling**: Commands capture intent and behavior, while events capture the results, aligning perfectly
      with our DDD approach.

## What is Event Sourcing?

Event Sourcing is an architectural pattern where all changes to an application's state are stored as a sequence of
events. Instead of just storing the current state, every state-changing action (event) is saved, providing a complete
history. This allows for the reconstruction of any past state by replaying these events.

Key aspects of Event Sourcing include:

- **Immutable Events**: Each event is immutable and represents a single change in the state.
- **Event Log**: A central log where all events are stored in the order they occurred.
- **State Reconstruction**: The current state can be rebuilt by replaying the stored events.

### Example:

Consider a bank account:

- **Event**: "Deposit of $100" and "Withdrawal of $50" are recorded as events.
- **Reconstruction**: To get the current balance, the system replays these events starting from an initial balance.
  - initial: $0
  - replay deposit -> $0 + $100
  - replay withdrawal -> $100 - $50
  - balance: $50

Event Sourcing is especially powerful when combined with Command Query Responsibility Segregation (CQRS) and
Domain-Driven Design (DDD), providing a robust foundation for complex applications.

## Conclusion

Event Sourcing provides a robust framework for building reliable, scalable, and maintainable systems. Its alignment with
CQRS and DDD principles makes it an ideal choice for
our architecture. This pattern ensures that our systems are not only capable of handling current demands but are also
well-prepared for future growth and changes.
