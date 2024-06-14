# Trigger Key Expression

The key expression is usable in all identity-based triggers:

- Aggregate behavior flows
- View data sources

It is used to convert an event attribute into a functional key. The name of the expression, e.g., **truncateArn**, is used to access this expression from a behavior or data-source model.

You can model input for the expression, e.g., `arn;length`.

The expression itself is written in pure Python, e.g.,
```python
':'.join(arn.split(':')[:int(length)])
```

You can access it by configuring a method call inside the key field of behavior or data-source. The input parameters will reference a trigger attribute. You can use literals, but they can only be strings. In our case, the expression will evaluate the string to an integer.
```plaintext
#global.truncateArn(childArn, '2')
```

Let's say that the trigger contains an attribute `childArn=draftsmanid:workspace:project`, then the resulting key for the flow will be `draftsmanid:workspace`.