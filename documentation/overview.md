# Concept overview

![image](https://file.notion.so/f/f/768f9acf-dd3f-42cb-a961-efe7917c26ac/5e085bb4-cdda-4578-b735-5e2d8f42cfff/devconf_2025.png?id=6dc08d44-b37a-4b0c-bd69-8ed352a727d5&table=block&spaceId=768f9acf-dd3f-42cb-a961-efe7917c26ac&expirationTimestamp=1718575200000&signature=WafdjPEs_7stxkGUN_IxOr436OyOKARwgjdI0qBXCx8&downloadName=devconf+2025.png)

Certainly! Here's a textual explanation of the concepts depicted in the architecture diagram:

In the architecture diagram:

**Commands**
Commands represent actions triggered by users or external systems. They encapsulate requests to perform specific
operations within the system.

**Aggregates and Behavior**

- **Event Sourcing**: This approach ensures that domain events, which capture immutable truths about past actions,
  remain unchanged over time. It separates the factual recording (event sourcing) from how we interpret and reason about
  these events (projection).
- **Technical Key**: This key is immutable and crucial for maintaining consistency in the system.

**Views and Queries**

- **Projection of Truth**: While projecting the truth to execute domain logic is essential, querying involves a
  different perspective. It allows deviations in the data model and supports querying based on functional keys that can
  change over time.
- **Materialized Views**: These are separate read models optimized for queries. They are decoupled from the write model
  and offer benefits like converting technical keys to functional ones, optimizing data aggregations, and establishing
  relationships between entities that are otherwise decoupled.

**Notifiers**
Notifiers orchestrate commands in response to domain events without maintaining state. They have the capability to
invoke a wide range of web APIs, including AWS, REST, GraphQL, and the platform's own API.

**Projections**
Projections enhance queries by combining data from APIs, materialized views, and custom Python logic. They facilitate
transformations such as calculations, advanced filtering, or generating time-sensitive attributes (tokens).

This structured approach ensures clarity and flexibility in handling actions, data querying, and system interactions
within the architectural context provided.