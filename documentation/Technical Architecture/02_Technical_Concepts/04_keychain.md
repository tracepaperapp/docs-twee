# Understanding Keys and the Keychain Concept

In Draftsman-generated applications, the key concepts are crucial for efficient data management and access. 
The important thing to know, is that during modelling you only have to think about the meanigfull business-keys. 
We automate the management of the technical keys for you in our architecture. Here's a
detailed explanation of the concepts behind key management:

## Business Key

The business key is a meaningful identifier used in the view-store of the application. This key has semantic
significance, allowing clients to request data based on recognizable identities. For instance, in a system tracking
projects, the business key might encode project identifiers and related hierarchies, making it intuitive for users to
query specific projects. This key helps structure the data in a way that aligns with real-world relationships, ensuring
efficient querying and filtering in the read model.

## Technical Key

The technical key, unlike the business key, is an internal identifier used primarily in the write model and event
stores. It is typically a UUID (Universally Unique Identifier), which provides a unique, immutable reference for
entities regardless of their semantic meaning. This key is essential for ensuring the integrity and consistency of the
event log, as it remains unchanged even when the business context (and thus the business key) evolves. The technical key
supports robust state determination by replaying events, essential for behavior-driven data models.

## Keychain

The keychain is a mechanism introduced to map business keys to technical keys seamlessly. It abstracts the complexity of
translating user-friendly, meaningful identifiers into system-specific, immutable ones. This translation is critical
because while business keys might change (e.g., due to renaming or restructuring), the underlying technical keys must
remain consistent to maintain data integrity. The keychain ensures that users can interact with the system using
business keys while the system internally manages and references the stable technical keys.

## The Need for a Keychain

A keychain is necessary for several reasons:

1. **Data Integrity and Consistency**: By separating business and technical keys, the system maintains consistent
   references in the event log, even when business keys change.
2. **User-Friendly Interactions**: Users can work with meaningful identifiers (business keys) without worrying about
   internal key management complexities.
3. **Flexibility and Evolution**: As business requirements change, entities can be renamed or restructured without
   affecting the underlying system's stability.
4. **Efficient Data Access and Management**: The keychain enables efficient querying by leveraging meaningful keys for
   data access while ensuring the robustness of technical keys for internal operations.

In summary, the keychain concept in Draftsman applications bridges the gap between user-friendly, meaningful identifiers
and the system's need for consistent, immutable references, ensuring both usability and reliability. For more details,
refer to the original blog post on [Draftsman.io](https://papers.draftsman.io/why-we-need-a-keychain/).