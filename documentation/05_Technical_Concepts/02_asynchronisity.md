# Asynchronous Command Handling

## Overview

Our application utilizes an asynchronous command handling system through AWS AppSync. When a client sends a command (via
GraphQL mutation), it is accepted and dispatched asynchronously, providing a trace ID for monitoring purposes. This
approach ensures robust and responsive user interactions, accommodating the inherent challenges of distributed systems.

## Process Flow

1. **Command Dispatch**: When a command is issued, it is accepted into the domain and dispatched asynchronously. AWS
   AppSync abstracts this process, handling the details of asynchronous communication.

2. **Trace ID**: Upon accepting a command, the system generates and returns a trace ID. This ID is crucial for clients
   to monitor the status of their commands.

## Monitoring with Trace ID

Clients can use the trace ID to monitor the status of their commands through GraphQL queries or subscriptions. The
information retrievable includes:

- Component name
- Timestamp
- Status (accepted, success, error)

## Query vs. Subscription

- **Query**: Used when client technology does not support subscriptions, or when viewing information after the command
  has been published. Queries are useful for retrospective checks and scenarios requiring fast processing.
- **Subscription**: Ideal for real-time monitoring, providing immediate updates on the command’s status.

## Error Handling

- **Retries**: For technical errors (e.g., network issues), the system retries the command up to three times.
- **Error Events**: If retries fail, an error event is thrown, and the context is stored in the deadletter table for
  further investigation and handling.

## Benefits of Asynchronous Handling

- **Performance**: Fast initial responses to clients improve user experience.
- **Robustness**: Handles the fallacies of distributed systems, ensuring reliable operation despite potential issues.
- **Real-time Updates**: Clients receive immediate feedback on the status of their commands, aiding in timely
  decision-making and interactions.

This asynchronous approach leverages the strengths of AWS AppSync to provide a responsive, robust, and user-friendly
system for handling commands and ensuring that clients are well-informed about the status and outcomes of their
requests.