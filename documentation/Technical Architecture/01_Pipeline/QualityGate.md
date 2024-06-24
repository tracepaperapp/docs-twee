# Quality Gate

The testing concept in the modeling tool ensures that the application acts as expected by validating the modeled
behavior, data access, and authorizations before deploying to production. This section covers how to define, structure,
and execute functional scenarios using the provided DSL.

## Purpose

The primary purpose of the test concept is to ensure application functionality, data integrity, and proper authorization
by running tests against the GraphQL API of a deployed version of the application. These tests help verify that the
application's behavior aligns with the modeled specifications.

## Functional Scenario Definition

Functional scenarios are defined and describe a sequence of actions
and validations to be executed against the GraphQL API. Each scenario consists of various activities that can include
mutations, queries, and validation steps.

## Available Actions

The model supports the following actions:

- **Grant Role to Test User**: Assigns a specific role to the current test user.
- **Execute Mutation**: Performs a mutation (command) to change the state of the application.
- **Validate Behavior and Automations**: Checks that expected behavior flows and automations execute with a defined
  status (success/error).
- **Validate View Queries**: Ensures that queries return the expected data and allows extraction of data for use in
  later actions.
- **Define Variables**: Sets variables for use in other actions within the scenario.

## GraphQL Queries and Mutations

All modeled commands are accessible as mutations, and all modeled queries and projections are accessible for testing
purposes. This ensures comprehensive coverage of the application's functionality. The test runner has access to Track &
Trace data,
meaning that you can instruct the test-runner to validate if a defined set of behavior-flows and automations are
executed, and are finished in an success or error state.

## Handling Authentication and Authorization

For testing purposes, the tool creates test users in the staging environment. Users can insert actions to grant specific
roles to the current test user or refresh the user's API token to ensure proper authorization during the test flow.

## Results and Outputs

Tests provide a pass or fail result based on:

- **Unexpected Behavior**: Deviation from the expected behavior.
- **Data Results**: Mismatch in expected data results.
- **Coverage**: Low event or view coverage, indicating insufficient test coverage.

A JSON report is generated and stored in the database. If a test fails, the application will not be deployed to
production.

## Validating Responses

Engineers can model response validations to ensure the application's responses meet the expected criteria. This includes
checking the presence and values of specific fields in the response data.

## Managing Test Data

Before tests are executed, the database is cleared to ensure tests run in a consistent and repeatable manner, avoiding
interference from residual data.
