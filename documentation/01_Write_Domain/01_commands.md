# Commands

## Overview

**Commands** are actions triggered by users or external systems to request the system to perform a specific operation.
Commands encapsulate all the necessary information required to carry out the action, including authorization details and
data fields. They are the primary means through which external entities interact with the system.

## Command Definition

A command is defined by specifying its name, authorization requirements, and associated data fields. Commands are
triggered by actors (users or external systems) and may lead to state changes within the system.

## Attributes of a Command

**Name**: A unique identifier for the command.

**Authorization**: Defines who can trigger the command. This includes:

- **Authorization Type**: Indicates if the command requires a specific role, can be triggered by any user (`anonymous`),
  or requires the user to be authenticated (`authenticated`).

- **Role**: In case role-based access is selected, specifies the user role required (e.g., `administrator`).

## GraphQL Configuration

Commands are exposed via a GraphQL API, allowing clients to interact with the system using standard GraphQL operations.
The configuration for GraphQL exposure includes:

**GraphQL Namespace**: Organizes related commands within a logical grouping.

**GraphQL Name**: Defines the specific mutation name for the command.

This configuration enables seamless integration and interaction with the system, providing a clear and structured API
for external clients.

## Fields

Fields represent the data required to execute the command. Each field is defined with the following attributes:

**Name**: The name of the field.

**Type**: The data type of the field. Possible types include:

- **String**: Textual data.
- **Int**: Integer values.
- **Boolean**: True/false values.
- **Float**: Floating-point numbers.

**Pattern**: (Optional) A [regular expression](https://en.wikipedia.org/wiki/Regular_expression) pattern that the field
value must match (e.g., DateTime input in a String field).

**Default**: (Optional) Default value for the field if none is provided. When a default is provided, the field will
become optional in the GraphQL schema.

**Auto-fill**: (Optional) Instructions for automatically generating the field value (e.g., `uuid` for unique identifiers
or `username`). Using auto-fill will remove this field from the GraphQL schema.

## Nested Collections

Commands can have complex data structures, which can be represented using collections of nested objects. Nested objects
allow the inclusion of sub-structures within a command, providing a way to encapsulate related data. A nested collection
has a name and a set of fields.

## Summary

Commands in an event-driven architecture encapsulate actions triggered by actors, specifying the necessary data and
authorization requirements. By defining commands with fields, nested collections, and GraphQL configurations, systems
can ensure a robust, secure, and structured interaction model for handling user and system interactions.