# Automations

## Overview

**Automations**, also referred to as **notifiers**, are predefined processes that are triggered in response to specific
events or conditions within a system. They are designed to perform actions automatically without direct user
intervention, often to notify users or other systems about important events or changes. Automations enhance system
responsiveness and improve user experience by providing timely and relevant information.

## Automation Definition

An automation is defined by specifying its name, trigger conditions, and actions to be performed. Automations react to
events occurring within the system, executing predefined actions when certain conditions are met.

## Attributes of an Automation

### Name

A unique identifier for the automation.

### Trigger Conditions

Conditions or events that initiate the automation. These triggers can be system events, changes in data, or specific
time-based conditions. Examples of trigger conditions include:

- **Event-based**: Automations can be triggered by specific events such as Commands (ActorEvents) originating from the
  GraphQL API. Or domain events published by aggregates.
- **Time-based**: Automations can be set to trigger at specific times or after certain intervals.
- **After-deployment**: Automations can trigger after the application is deployed.

### Actions

# Automation Activity Types

Automations in a system can perform a variety of tasks, known as activities. Each activity type represents a specific
action that the automation can execute when triggered. Below is a documentation of the possible activity types that can
be used within automations (notifiers).

## Overview

Automations consist of a series of activities that define the specific actions to be performed. These activities are
triggered by events or conditions within the system and can interact with various system components or external
services.

## Activity Types

### Identity and Access Management (IAM - AWS Cognito) Activities

- **create-iam-group**
    - **Description**: Creates a new IAM group within the system.
    - **Use Case**: Used when a new group of users with specific permissions needs to be created.

- **delete-iam-group**
    - **Description**: Deletes an existing IAM group.
    - **Use Case**: Used when an IAM group is no longer needed and should be removed.

- **add-user-to-iam-group**
    - **Description**: Adds a user to an IAM group.
    - **Use Case**: Used to grant a user the permissions associated with the IAM group.

- **remove-user-from-iam-group**
    - **Description**: Removes a user from an IAM group.
    - **Use Case**: Used to revoke a user's permissions associated with the IAM group.

- **retrieve-email-from-iam**
    - **Description**: Retrieves a user's email address from the IAM system.
    - **Use Case**: Used to get the email address of a user for communication purposes.

- **iam-create-systemuser**
    - **Description**: Creates a system user in the IAM system.
    - **Use Case**: Used to create a non-human user that interacts with the system programmatically.

- **iam-create-user**
    - **Description**: Creates a new user in the IAM system.
    - **Use Case**: Used for onboarding new users.

- **iam-delete-user**
    - **Description**: Deletes a user from the IAM system.
    - **Use Case**: Used when a user account needs to be permanently removed.

### Communication and Notification Activities

- **render-template**
    - **Description**: Renders a template with specified data.
    - **Use Case**: Used to create personalized messages or documents.

- **send-email**
    - **Description**: Sends an email to specified recipients. (AWS SES)
    - **Use Case**: Used for sending notifications, alerts, or other communications.

- **send-graphql-notification**
    - **Description**: Sends a notification via the GraphQL endpoint.
    - **Use Case**: Used for notifying clients or systems through the GraphQL API.

### File and Data Operations

- **write-file**
    - **Description**: Writes data to a file.
    - **Use Case**: Used to save data in a remote file system (AWS S3).

- **fetch-property**
    - **Description**: Fetches a specific property or data point from the system.
    - **Use Case**: Used to retrieve configuration settings or other data.

### Token Management Activities

- **get-token**
    - **Description**: Retrieves a token for authentication or authorization purposes.
    - **Use Case**: Used to get tokens required for accessing secured resources.

- **get-systemuser-token**
    - **Description**: Retrieves a token for a system user.
    - **Use Case**: Used for system-to-system authentication.

### Variable and State Management

- **set-variable**
    - **Description**: Sets a flow variable to a specified value.
    - **Use Case**: Used to store temporary data or state within an automation.

### API and External Service Interactions

- **call-internal-api**
    - **Description**: Calls an internal GraphQL API endpoint.
    - **Use Case**: Used to invoke internal system functions (Commands/Queries/Projections).

- **HTTP**
    - **Description**: Makes an HTTP request to an external service.
    - **Use Case**: Used to interact with external APIs or services.

### Miscellaneous Activities

- **code**
    - **Description**: Executes custom code.
    - **Use Case**: Used for complex logic that cannot be handled by predefined activities.

- **invalidate-cdn**
    - **Description**: Invalidates a CDN cache.
    - **Use Case**: Used to ensure that updated content is served from the AWS CloudFront CDN.

- **loop**
    - **Description**: Repeats a set of activities a specified number of times.
    - **Use Case**: Used for iterating over a collection of items or repeating an action multiple times.

## Summary

Automations leverage a diverse set of activity types to perform various tasks automatically. By combining these
activities, systems can create sophisticated workflows that enhance functionality, improve efficiency, and provide
timely responses to events and conditions. Understanding these activity types is essential for designing effective
automations that meet specific business needs.
