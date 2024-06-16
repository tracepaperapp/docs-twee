# Aggregates

An aggregate reflects a domain concept, a collection of data (state) and behavior to manipulate the data. The data is
internally encapsulated, so the only way to induce a state change is by invoking behavior.

Modeling an aggregate involves two activities:

1. Modeling the data model.
2. Modeling behavior on the model.

----

> A concept from DDD opinionated to optimize cognitive load, focusing on the concepts rather than the underlying
> technology.
> It abstracts the Command/Compute part of our CQRS architecture.

# Aggregate Data Model

The data model consists of two parts, the actual part: the **events**, and the projection: the **document**.

## The Document

The document is a projection of the data, a mental model of the state so that we can implement validations in the
behavior to ensure data integrity. Because it is a projection, this model can evolve over time without manipulating the
underlying data. Events from the past are immutable by definition.

Thus, the document serves both as a mental model for ourselves on how we want to think about state and as a way to model
how we present data to the viewstore. It is the data contract between the aggregate and the viewstore.

**One of the document fields (String) will serve as the business key of the aggregate instance**

## The Events

The domain events are the recording of facts. They reflect a bundle of data that encompasses the delta between two
states. Therefore, the state is not stored in the database, merely a log of deltas that exist alongside each other on a
timeline. This is called the event log.

### Event Handling

To populate our mental model, the projection, we need event handlers. These model the mapping between the event log and
the document.

----

## Side Notes on Views

Views will be discussed later, but regarding the contract, the viewstore essentially models the external contract, the
GraphQL API. So while the document is a data contract, this contract remains within the domain. The viewstore can do
various things with the data:

### Store As Is

Essentially caching a snapshot of the aggregate state. In this case, the internal model becomes publicly accessible,
being queryable and read-optimized.

### Enrich and Store

Enrichment can take various forms, such as combining data from different aggregates into one document or modifying data
for storage, i.e., determining derived data and caching it in the viewstore.

### Enrich During Data Reading

Modifying the API response before it is sent to the client. This involves executing logic on the combination of request
data and cached data. Essentially, this is an on-the-fly projection where the view model is virtual. The logic has
access to the request data and a fluent API to the viewstore, allowing the creation of a response object using Python
scripting from this combination.

# Behavior Flows

## Overview

Behavior flows define the lifecycle and interactions of aggregates within a system. They encapsulate the business logic
required to handle events and commands, ensuring consistent state transitions and proper handling of complex workflows.
By defining how an aggregate responds to various inputs, behavior flows help maintain system integrity and align with
business rules.

## General

Behavior is a response to an event emited within the domain this may be an ActorEvent (Command) or a DomainEvent (From
an other entity), which can lead to changes in the state of an aggregate.

- **Name**: A unique identifier for the command.
- **Create Command**: (Optional) Indicates if this should be the initial behavior of a new instance of the aggregate. If
  the instance with the specific business key is already present in the database the execution will fail.

## Trigger

A trigger specifies the conditions under which a command is activated. It listens for specific events and uses the data
from these events to execute the corresponding behavior flow.

- **Source**: The event that activates the command.
- **Key Field**: The primary field used to correlate the incoming event with the correct aggregate instance.
- **Mapping**: Transfers data from the event to the flow variable fields. Optionally event-fields can be marked
  as `part of the idempotency key` giving you the ability to model functional idempotency.

## Processors

Processors are essential components within behavior flows that perform specific operations. They manage validations,
data transformations, event emissions, and other logical actions to ensure that the behavior executes correctly and that
aggregates maintain consistent state transitions. Each processor type serves a unique function and has specific
attributes to support its operations.

## Processor Types

## Emit Event

The `emit-event` processor is responsible for generating and emitting domain events, which signal that a significant
change or action has occurred within the system. This processor type includes a reference
specifying the event to be emitted, and a mapping mechanism that transfers flow-variables to the
event fields.

## Code

The `code` processor allows for custom logic to be executed within the behavior flow. This processor is used when
predefined processor types do not cover the required operation. It includes the code (inline) or script (global) to
be executed, and optionally, inputs or parameters required by the script.

## Validator

The `validator` processor checks specific conditions and ensures they are met before proceeding. If the condition fails,
an exception or error is triggered, preventing the behavior from executing further. This processor type includes
the condition to be checked, typically expressed as a logical expression, and the message or exception to be
raised if the condition is not met. If a functional exception is raised, no events will be persisted to the event store.
So a validator may also validate on the instance state after emit-event processors are already applied, it will still
prevent the state transition.

## Set Variable

The `set-variable` processor assigns values to variables (in memory) within the behavior flow. This can be used to store
intermediate results, configuration values, or other data needed for subsequent processing steps. This processor type
includes the name of the variable to be set, and the expression or value used to determine the variable's value.

## Update Key

The `update-key` processor updates the business key of an aggregate instance. This is useful for operations that require
changing
the identifier or key used to access an aggregate instance. This processor type includes the field
in the aggregate to be updated, and the new value to be assigned to the key field.

## Test Case

Test cases validate the behavior of a command by defining inputs, expected domain events, and resulting state changes.
They ensure that the command performs as intended and that the aggregate's state transitions correctly.

- **Name**: A unique identifier for the test case.
- **Trigger Event**: The event that initiates the test case.
- **Input**: Defines the input data for the command.
    - **Name**: The name of the input field.
    - **Value**: The value assigned to the input field.
    - **Type**: The data type of the input field.
- **Expected Domain Event**: Specifies the expected event to be emitted by the command.
    - **Field**: Defines the expected values for fields in the emitted event.
- **State**: (Optional) The initial state of the aggregate before executing the command.
- **Expected State**: The expected state of the aggregate after executing the command.
    - **Primary Key (pk)**: The key identifying the aggregate instance.
    - **State Data**: The expected state data of the aggregate.

## Summary

Behavior flows are essential for managing the lifecycle and interactions of aggregates. By defining commands, triggers,
mappings, processors, and test cases, behavior flows ensure that business logic is consistently applied, state
transitions are correctly managed, and the system's behavior aligns with the intended domain model. Understanding and
modeling behavior flows are crucial for implementing robust, maintainable, and scalable systems.

# Advanced Features of an Aggregate

## Event Store Time-to-Live (TTL)

The Event Store Time-to-Live (TTL) is an advanced configuration setting for aggregates that specifies the duration, in
seconds, for which events associated with the aggregate should be retained in the event store. This feature is
particularly useful in scenarios where certain events become irrelevant or obsolete after a specified period.
The default TTL is -1 seconds which translates to indefinitely.

The TTL setting allows system architects and developers to manage the lifecycle of events within the event store
effectively. By setting an expiration time for events related to an aggregate, the system can automatically purge older
events beyond the specified TTL. This helps in maintaining a lean and efficient event store by removing outdated data
that no longer contributes to the current state or history of the aggregate.

### Benefits

- **Optimized Event Storage**: Helps in managing storage space by automatically removing events that are no longer
  relevant.

- **Compliance and Governance**: Supports compliance requirements by ensuring that sensitive or outdated data is removed
  from the system in a timely manner.

- **Performance**: Improves performance by reducing the volume of data that needs to be processed and queried over time.

### Considerations

- **Event Retention Policies**: Define appropriate TTL values based on business needs and regulatory requirements.

- **Data Archival**: Consider integrating TTL with data archival strategies to maintain historical data beyond the event
  store.

### Summary

The Event Store Time-to-Live (TTL) configuration for aggregates enhances the management and efficiency of event-driven
architectures. By setting TTL values, systems can automatically manage event data retention, ensuring that only relevant
and current information is maintained in the event store while optimizing storage resources and improving overall system
performance.

## Snapshot Interval

The Snapshot Interval is an advanced configuration setting for aggregates that determines how frequently snapshots of
the aggregate's state should be taken. Snapshots capture the current state of an aggregate at a specific point in time,
providing a checkpoint that can optimize event sourcing and reconstruction processes.

In event sourcing architectures, aggregates can accumulate a large number of events over time, which can impact
performance during state reconstruction. Snapshots serve as efficient checkpoints by storing the aggregate's current
state periodically. Instead of replaying all events from the beginning to reconstruct the aggregate's state, the system
can load the latest snapshot and replay only the events that occurred after the snapshot was taken.

### Usage

When configuring an aggregate with Snapshot Interval:

- **Snapshot Interval**: Specifies the number of events after which a new snapshot of the aggregate's state should be
  taken. For example, a snapshot interval of 100 means that every 100 events, a snapshot of the
  aggregate's state will be captured.

### Benefits

- **Performance Optimization**: Reduces the time and computational resources required for state reconstruction by
  loading snapshots and applying only subsequent events.

- **Efficient Event Handling**: Improves overall system performance by minimizing the number of events that need to be
  processed during state recovery.

- **Scalability**: Facilitates scalability by reducing the processing overhead associated with large aggregates and long
  event histories.

### Considerations

- **Snapshot Size**: Evaluate the size and complexity of aggregate states to determine an appropriate snapshot interval.
- **Event Volume**: Adjust the snapshot interval based on the frequency and volume of events generated by aggregates.

### Default Value

The default Snapshot Interval is typically set to 100 events, providing a balance between capturing frequent snapshots
and minimizing overhead. This default value can be adjusted based on specific application requirements and performance
considerations.

### Summary

The Snapshot Interval configuration enhances the efficiency and performance of event-sourced aggregates by periodically
capturing snapshots of their states. By reducing the computational effort required for state reconstruction, snapshots
optimize event handling and support scalability in event-driven architectures. Adjusting the snapshot interval allows
systems to achieve optimal performance while effectively managing aggregate states and event histories.

## Backup to Blob Storage (S3)

The Backup to Blob Storage feature allows event-sourced aggregates to automatically back up their state to cloud
storage, specifically Amazon S3. This capability ensures that critical data remains secure and accessible, providing
resilience against data loss and supporting disaster recovery strategies.

In event sourcing architectures, ensuring data durability and recoverability is paramount. Backup to Blob Storage
leverages cloud infrastructure, such as Amazon S3, to store snapshots of aggregate states at regular intervals.
Additionally, it manages the retention of these backups based on specified policies, providing flexibility in data
management and compliance with retention requirements.

The event store (DynamoDB) has point-in-time recovery enabled, therefore, this feature is disabled by default
to reduce the area where data needs to be protected. If this feature should be enabled for a specific aggregate 
is a business consideration.

### Configuration

- **Interval**: Specifies the frequency, specified in days, at which snapshots of aggregate states should be backed up to S3. .
- **Retention Period**: Defines the duration for which backup snapshots should be retained in S3 storage, measured in
  days. After the retention period expires, older snapshots are automatically deleted.

### Usage

The Backup to Blob Storage feature offers several advantages:

- **Data Resilience**: Enhances data durability by securely storing aggregate state snapshots in Amazon S3, which is
  designed for high availability and redundancy.

- **Disaster Recovery**: Facilitates rapid recovery of aggregate states in the event of data corruption, system
  failures, or disasters by maintaining up-to-date backups. Although the required script to recover from cold storage 
  are not (yet) provided by Draftsman.

- **Compliance**: Supports compliance with regulatory requirements and data retention policies by managing backup
  retention periods effectively.

### Implementation Considerations

- **Security**: Ensure proper access controls and encryption mechanisms are in place to protect sensitive data stored in
  S3.
- **Cost Management**: Monitor storage costs associated with S3 backups and optimize usage based on storage needs and
  budget constraints.
- **Integration**: Integrate with existing backup and recovery processes to streamline data management and operational
  workflows.

### Summary

Backup to Blob Storage (S3) is a critical feature in event-driven architectures that ensures the resilience and
recoverability of event-sourced aggregates. By automatically backing up aggregate states to Amazon S3 at defined
intervals and managing retention periods, this feature enhances data durability, supports disaster recovery strategies,
and enables compliance with data retention policies. Configuring backup intervals and retention periods optimally
balances data protection with operational efficiency, thereby safeguarding business-critical information in event
sourcing environments.