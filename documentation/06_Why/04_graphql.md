# Why GraphQL

| Feature/Aspect          | GraphQL                                                                                                                                             | REST                                                                                                                                                                                                                           |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Alignment with CQRS** | Perfect fit with CQRS; commands (mutations) and queries are distinct, reducing cognitive load because no additional level of abstraction is needed. | Less aligned; REST has a good fit with CRUD, while we deal with commands (CUD) and queries (R), merging those concepts together increases complexity and cognitive load (adds just another thing that you should think about). |
| **Modeling**            | Simplifies modeling by mirroring existing structures; focus on behavior, not data.                                                                  | Requires explicit API modeling, adding a layer of abstraction and complexity.                                                                                                                                                  |
| **Client Interaction**  | Commands modeled as GraphQL mutations; async processing monitored via subscriptions.                                                                | CRUD operations on resources; requires inventing virtual resources for modeling, less intuitive.                                                                                                                               |
| **Learning Curve**      | Familiarity with GraphQL required, but benefits in aligning with CQRS and reducing overall complexity.                                              | REST is widely known but might not fit well with our implementation of CQRS, requiring additional abstractions.                                                                                                                |

### Benefits of Using GraphQL for Modeling:

1. **Conceptual Fit with CQRS**:
    - **Commands and Mutations**: In our modeling tool, commands represent client interactions or requests for changes,
      aligning perfectly with GraphQL mutations. The API accepts these commands, returning a trace ID that can be
      monitored via GraphQL subscriptions, allowing clients to track the asynchronous processing of these mutations.
    - **Materialized Views and Queries**: Our tool uses materialized views and projections to handle data viewing,
      aligning seamlessly with GraphQL queries.

2. **Reduced Cognitive Load**:
    - **Natural API Structure**: With GraphQL, the API mirrors the existing structure of the model. This means
      developers only need to decide what to expose and configure authentication/authorization methods, significantly
      reducing the cognitive load.
    - **Behavior Over Data**: Our approach focuses on behavior rather than just data manipulation. REST, being more
      resource-centric, would require us to invent virtual resources and simulate CRUD operations, which adds
      unnecessary complexity.

3. **Potential Challenges**:
    - **Less Common Usage**: While GraphQL is powerful, it may be less commonly used compared to REST. However, the
      benefits in reducing cognitive load and fitting naturally with our CQRS architecture make it a superior choice for our
      needs.

By choosing GraphQL, we ensure that our modeling tool aligns perfectly with our CQRS architecture, providing a more
intuitive, efficient, and effective way to design APIs, reducing the complexity and cognitive load on developers. This
alignment allows for a more straightforward configuration of commands, queries, and access patterns, leading to a
smoother and more productive development experience.