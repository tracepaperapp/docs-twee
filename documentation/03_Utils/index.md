# Globaly available concepts

Utils are globally available concepts that are modeled once and used across the system. They include:

## Expressions

Expressions are reusable logical constructs used to transform data on-the-fly before executing logic. There are two
types of expressions:

### API Authorization Expression

These expressions are used in models that define an access path in the GraphQL API. They
convert query variables to specific roles, useful in multi-tenant systems where each tenant has a private set of roles.

### Trigger Key Expression

The key expression is usable in all identity-based triggers: Aggregate behavior flows & View data sources.
It is used to convert an event attribute into a functional key.

## Dependencies

Allows you to add [pip packages](https://pypi.org/) to you runtime, the package will be available for importing in
all custom python code in your project.

## Patterns

Patterns are [regular expressions](https://en.wikipedia.org/wiki/Regular_expression) that can be used while modelling 
commands to ensure a specific pattern for String fields e.g.

### LowercasedOnly
```regexp
^[a-z]+$
```

### Date
```regexp
^(?:20)\d{2}-\d{2}-\d{2}$
```

### Extending patters

Patterns may reference each-other e.g.
```regexp
^{{LowercasedOnly}}:arn:{{LowercasedOnly}}$
```
## Roles

Simply a list of role names available in the modeler, at this moment these roles are not automatically loaded into the
the IAM system by Draftsman. The application owner needs to define these roles manually, or model an automation (@afterDeployment)
that creates these roles.

## Python modules

Python modules are usable scripts that define methods that are accessible from:

- Behavior flows
- Automations

[//]: # (- View custom mappings)

[//]: # ()
[//]: # (### Behavior flow & Automation accessible method)

```python
def behavior_or_notifier_function(flow):
    # May set a flow variable
    flow.myVariable = "Hello World!"
    
    # And has access to flow variables
    print(flow.myVariable)

    # And has also access to the aggregate document
    print(flow.entity)
    print(flow.entity.entityField)
```

It is a simple function but it does provide you access to all variables available to the flow execution.

[//]: # (### View custom mapping method)

[//]: # ()
[//]: # (```python)

[//]: # (def view_updater_function&#40;entity_manager,snapshot,event&#41;:)

[//]: # (    # Has access to the entity manager to retrieve a view instance)

[//]: # (    entity = EntityManager.get&#40;type="ViewName", key=snapshot.businessKey&#41;)

[//]: # (    )
[//]: # ()
[//]: # (    # Or may mark the entity for deletion)

[//]: # (    entity.mark_for_deletion = True    # And may manipulate the data)

[//]: # (    entity.value = snapshot.value)

[//]: # (    )
[//]: # (```)