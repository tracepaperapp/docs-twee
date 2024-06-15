# Aggregate Data Model

The data model consists of two parts, the actual part: the **events**, and the projection: the **document**.

## The Document

The document is a projection of the data, a mental model of the state so that we can implement validations in the
behavior to ensure data integrity. Because it is a projection, this model can evolve over time without manipulating the
underlying data. Events from the past are immutable by definition.

Thus, the document serves both as a mental model for ourselves on how we want to think about state and as a way to model
how we present data to the viewstore. It is the data contract between the aggregate and the viewstore.

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