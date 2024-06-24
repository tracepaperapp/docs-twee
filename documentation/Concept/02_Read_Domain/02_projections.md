# Projections

The core concept of a projection involves dynamically transforming and combining data from various sources, such as
materialized views and user inputs, to generate specialized views in real-time. Projections facilitate tasks like
aggregating multiple materialized views, applying advanced filters based on user criteria, and performing calculations
using both user-provided data and precomputed views. This dynamic approach enables responsive and flexible querying,
ensuring that applications can deliver tailored data views efficiently without relying solely on precomputed results.

## API Configuration

To configure a projection, several key elements must be defined:

### Projection Name

A unique identifier for the projection, which helps in managing and referencing the projection.

### GraphQL Namespace

The namespace under which the projection's GraphQL queries will be bundled. This helps in organizing queries logically.

### GraphQL Method Name

The method name used to invoke the projection via GraphQL. This name should be unique within the specified namespace.

### Authorization Method

Specifies the access control for the projection. The following methods are supported:

- **anonymous**: Requires an API key that can be configured in the client. This key may be publicly available, and the
  API may rate limit these requests to prevent DDOS attacks.
- **authenticated**: Requires a user token to access the data.
- **role based**: Requires the user to have a specific role to access the data.

### Return Object

References a view model that defines the structure of the data returned by the projection.

### Return Type

Specifies whether the projection returns a single item or a result set. The possible values are:

- **item**: Returns a single instance of the view.
- **result_set**: Returns a list of instances.

## Arguments

Projections can accept various query variables, which are specified as follows:

### Name

The name of the query variable.

### Type

The data type of the query variable. Supported types include:

- String
- Int
- Float
- Boolean
- StringList

### Required

Specifies whether the query variable is mandatory for the projection to execute. The possible values are:

- **true**
- **false**

## Data Preparation Logic

The core logic for preparing data in a projection is written in Python. This logic involves accessing materialized
views, applying filters, and transforming data as needed. The following example demonstrates how to implement the data
preparation logic:

```python
from draftsman.ViewStoreApi import Query


def transform(arguments, username):
    # You have access to the username of the requestor and the arguments.
    print(f"Handle graph request [{arguments}/{username}]")

    # You have access to a fluent API to access materialized views
    # Here are some examples:

    # Access a specific object (most efficient method cost-wise)
    data = Query('ViewName').get_item("key").run()

    # Get a list of all objects of a specific type
    data = Query('ViewName').get_items().run()

    # Filter a list of objects based on type and key, part of the canonical key Concept
    data = Query('ViewName').get_items("key_starts_with_this_value").run()

    # Filter on content for a specific type (performs a scan on all views of type 'ViewName')
    data = Query('ViewName').get_items().equals('key', 'value').between('key', 0, 100).run()

    # Combine the two filter methods to filter within a subset
    data = Query('ViewName').get_items("key_starts_with_this_value").equals('key', 'value').between('key', 0, 100).run()

    # Switch to an "or" operator for filters (default is "and")
    data = Query('ViewName').get_items(key="key_starts_with_this_value", filter_chain_method="or").equals('key',
                                                                                                          'value').between(
        'key', 0, 100).run()

    # Program data transformations with Python
    # Ensure you add all fields that are defined in the return view object definition
    return {"field_name": "value"}
```

## Fluent API

The Fluent API, which allows you to query and filter
data from the DynamoDB view store in a flexible and efficient manner.

The Fluent API provides a convenient way to construct and execute queries against the DynamoDB view-store-table. It
supports various
filtering methods and allows for the retrieval of individual items or sets of items based on specified criteria.

### Initialization

To start using the Fluent API, you need to initialize the `Query` class with the type of view you want to query.

```python
from draftsman.ViewStoreApi import Query

# Initialize a query for a specific view name
query = Query("YourViewName")
```

### Methods

#### Retrieving Items

##### get_item

Retrieve a single item based on its key.

```python
query.get_item("your_item_key")
```

##### get_items

Retrieve a set of items, optionally filtered by a key prefix.

```python
query.get_items(key="key_prefix", filter_chain_method="and")
```

- `key` (optional): A prefix to filter items by their keys.
- `filter_chain_method` (optional): Determines how multiple filters are combined. Can be "and" or "or". Default is "
  and".

#### Adding Filters

Filters can be applied to the query to narrow down the results. The following filter methods are available:

##### equals

Filter items where the attribute equals a specified value.

```python
query.equals("attribute_name", "value")
```

##### less_than

Filter items where the attribute is less than a specified value.

```python
query.less_than("attribute_name", value)
```

##### less_than_equals

Filter items where the attribute is less than or equal to a specified value.

```python
query.less_than_equals("attribute_name", value)
```

##### greater_than

Filter items where the attribute is greater than a specified value.

```python
query.greater_than("attribute_name", value)
```

##### greater_than_equals

Filter items where the attribute is greater than or equal to a specified value.

```python
query.greater_than_equals("attribute_name", value)
```

##### begins_with

Filter items where the attribute begins with a specified value.

```python
query.begins_with("attribute_name", "value_prefix")
```

##### between

Filter items where the attribute is between two specified values.

```python
query.between("attribute_name", low_value, high_value)
```

##### not_equals

Filter items where the attribute does not equal a specified value.

```python
query.not_equals("attribute_name", "value")
```

##### exists

Filter items where the attribute exists.

```python
query.exists("attribute_name")
```

##### not_exists

Filter items where the attribute does not exist.

```python
query.not_exists("attribute_name")
```

##### contains

Filter items where the attribute contains a specified value.

```python
query.contains("attribute_name", "value")
```

##### attribute_type

Filter items where the attribute is of a specified type.

```python
query.attribute_type("attribute_name", "type")
```

##### is_in

Filter items where the attribute is in a specified list of values.

```python
query.is_in("attribute_name", ["value1", "value2", "value3"])
```

#### Running the Query

Once the query is constructed with the necessary filters, it can be executed using the `run` method.

```python
result = query.run()
```

- If `get_item` was used, `run` returns a single item.
- If `get_items` was used, `run` returns a list of items matching the query.

#### Method chaining

The Fluent API supports method chaining, allowing you to construct complex queries in a readable and concise manner by
chaining multiple method calls together. Method chaining enhances the clarity of query construction and reduces the need
for intermediate variables.

```python
resultset = query.get_items("key_prefix").equals("status", "active").greater_than("priority", 1).equals("user","j.doe").run()
```
