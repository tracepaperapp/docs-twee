# Views

Views are defined entities that represent a set of attributes and may contain transformation of data from underlying
aggregates (data sources). Each view is named and configured with specific parameters.

## Fields

Views represent collections of documents, fields are part of the document schema. Fields are configured with a name and
a type (String, Int, Float, Boolean, StringList).

## Relations

The document schema of a view may have relations to other views. These may be OneToOne, OneToMany, ManyToOne, and
ManyToMany. A special type of relation is the ObjectList. While the other relations point to other documents in the view
store, ObjectList represents a nested object in the document.

Relations are part of the document schema of a view and have a name, relation type, and referenced view-model as minimal
configuration. Depending on the relation type, it may be mandatory to configure the foreign key (this may be a canonical
key, an advanced feature explained later). Optionally, it is possible to require a specific role (authorization) to
access the relational data. The default authorization method is `inherit from the API query`, meaning if the client has
access to the view, they have access to the relational data.

## Business Key

One of the fields may be appointed to serve as the business key (also called the primary key). The field that is used as
the business key must be of the type `String`. Only when a business key is appointed, the view can be stored in the
view-store as a root document. Only root documents may have queries exposed in the GraphQL API.

If no business key is appointed, the view model can only serve as ObjectList relations in other views.

## Data Mapping

Views with an appointed business key need to be filled with data published by aggregates. There are two types of
event-handlers that are configured to listen for snapshot-events from a specific aggregate. A view may have multiple
event handlers observing different aggregates or multiple handlers observing the same aggregate. This is useful when the
aggregate instance has its business key updated, allowing one handler to create a new instance with the new key and
another handler to remove the old view instance.

A snapshot-event is streamed to the view-store every time an aggregate emits a domain-event.

### Mapper

The first type of event-handler is a simple mapper. It enables you to map aggregate fields and collections to
view-fields and ObjectList relations. Additionally, you must configure which aggregate field (String) serves as the
business-key of the view-instance. Note that the business-key for the view may differ from the business-key from the
aggregate. Furthermore, you can select a processing strategy, either `item` or `dictionary`.

- **Item**: The mapping is executed once for every aggregate snapshot.
- **Dictionary**: In this case, you select one of the aggregate's nested collections to be processed. This is useful
  when a nested collection in an aggregate must be autonomously queryable.

Additionally, you can configure multiple delete-expressions, causing deletion of the view instance when any of the
expressions evaluate to true.

### Custom Mapper

Instead of relying on the standard mapping, you can use Python code to convert aggregate data into the view instance.
This allows for advanced use cases like pre-filtering and enrichment beyond the capabilities of the standard mapper. For
example, you could create multilevel nesting in the document (ObjectList relation) which the standard mapper only
provides for one level. However, for maintainability, simpler is usually better.

## GraphQL Queries

Queries associated with views define access patterns for retrieving data via GraphQL. The GraphQL namespace is
configured at the view level, meaning all queries in a specific view are bundled in the same namespace. Each query has a
unique name in this namespace, and the needed access control method can be configured:

- **anonymous**: Requires an API key that can be configured in the client. Because this key may become publicly
  available, the API may rate limit these requests to prevent DDOS attacks.
- **authenticated**: Requires a user token to access the data.
- **user**: Requires a predefined query parameter to match the signed-in user, making the data accessible only to the
  current signed-in user.
- **role-based**: Requires the user to have a specific role to access the data.

There are three types of queries:

- **get**: Returns one instance of the view based on the business key. This type can be configured once per view.
- **filter**: Returns a list of instances based on defined filter clauses, e.g., `field x equals a query parameter`.
  During modeling, the possible filter clauses are defined. Filters may also provide canonical search, making the
  key_begins_with query parameter mandatory. This type can be configured only once per view.
- **named query**: Similar to the filter query but with a configurable name during modeling, and the filter clauses are
  mandatory for the client.

## Data Retention and Exclusion

Views may specify:

- **Data Retention**: Period for which data within the view remains accessible. Defined in days, the default is -1,
  translating to indefinitely.
- **Notification Exclusion**: By default, the view store will publish a GraphQL notification to which clients can
  subscribe. The notification contains the view name and the primary key, allowing the client to refetch the data when
  an update occurs. Subscriptions to the notification may be anonymous. This option prevents the publication of this
  notification, e.g., because the business key contains identifiable data like a username or an email address.

## Benefits

- **Data Consistency**: Views provide consistent and up-to-date representations of data entities.
- **Customization**: Custom handlers enable tailored logic and functionalities within views.
- **Integration**: Seamless integration with underlying aggregates ensures data accuracy and synchronization.
- **Security**: Authorization controls ensure data access is restricted as per defined policies.
