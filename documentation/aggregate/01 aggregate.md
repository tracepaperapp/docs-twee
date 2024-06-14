# The Aggregate Concept

An aggregate reflects a domain concept, a collection of data (state) and behavior to manipulate the data. The data is internally encapsulated, so the only way to induce a state change is by invoking behavior.

Modeling an aggregate involves two activities:
1. Modeling the data model.
2. Modeling behavior on the model.

----

> A concept from DDD opinionated to optimize cognitive load, focusing on the concepts rather than the underlying technology. 
> It abstracts the Command/Compute part of our CQRS architecture.