# Expressions

## API Authorization Expression

The authorization expression can be used in all components exposed in the API:

- Commands
- View queries

It is used to convert an input parameter (command field or query filter field, e.g., key, key_begins_with, or a custom
filter attribute) to a technical role that the API resolver will validate to determine if the requester has the specific
role. This is useful for providing role-based access in a multi-tenant system.

The expression has a **name**, e.g., `extractRoleFromArn`, which is used to access the expression from command models or
view models.

You model inputs for this function separated with a **;** e.g.,

```
arn;role
```

And then use Velocity Template Language (VTL) syntax with basic JavaScript to model the logic:

```
${arn.split(':')[0]}:${arn.split(':')[1]}:role
```

In a command or view query, you can use this expression in the role field when you select role-based access:

```plaintext
#global.extractRoleFromArn(key, 'viewer')
```

When you execute, for example, a query where this is implemented, it will evaluate to:

```plaintext
#foreach($group in $context.identity.claims.get("cognito:groups"))
    #if($group == "${ctx.args.key.split(':')[0]}:${ctx.args.key.split(':')[1]}:viewer")
        #set($inCognitoGroup = true)
    #end
#end
#if($inCognitoGroup){
    "version": "2018-05-29",
    "operation": "GetItem",
    "key": {
        "type": $util.dynamodb.toDynamoDBJson("ViewName"),
        "key": $util.dynamodb.toDynamoDBJson($ctx.args.key)
    }
}
#else
$utils.unauthorized()
#end
```

## Trigger Key Expression

The key expression is usable in all identity-based triggers:

- Aggregate behavior flows
- View data sources

It is used to convert an event attribute into a functional key. The name of the expression, e.g., **truncateArn**, is
used to access this expression from a behavior or data-source model.

You can model input for the expression, e.g., `arn;length`.

The expression itself is written in pure Python, e.g.,

```python
':'.join(arn.split(':')[:int(length)])
```

You can access it by configuring a method call inside the key field of behavior or data-source. The input parameters
will reference a trigger attribute. You can use literals, but they can only be strings. In our case, the expression will
evaluate the string to an integer.

```plaintext
#global.truncateArn(childArn, '2')
```

Let's say that the trigger contains an attribute `childArn=draftsmanid:workspace:project`, then the resulting key for
the flow will be `draftsmanid:workspace`.