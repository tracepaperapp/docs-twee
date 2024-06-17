# AWS services

## DynamoDB

**Service Description**: Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable
performance with seamless scalability. It supports both document and key-value data models, making it suitable for a
wide range of applications.

**Selection Rationale**: We chose DynamoDB for its seamless scalability and managed nature, which allows us to focus on
application development rather than database management. Its integration with other AWS services and ability to handle
large-scale workloads with low latency were key factors in our decision.

**Optimization Goals**:

- **AWS Native & Serverless**: DynamoDB is a fully managed service provided by AWS, eliminating the need for server
  management.
- **Minimizing Management and Cost**: It offers pay-as-you-go pricing and automate tasks such as hardware provisioning,
  setup, and configuration,
  reducing operational overhead and potential costs.

**Documentation**: [AWS DynamoDB Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)

**Service Limits**: [DynamoDB Service Limits](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Limits.html)

## Amazon Cognito

**Service Description**: Amazon Cognito provides authentication, authorization, and user management for web and mobile
applications. It supports social identity providers, such as Facebook, Google, and Amazon, as well as enterprise
identity providers via SAML 2.0.

**Selection Rationale**: We selected Amazon Cognito for its ease of integration with other AWS services and its
comprehensive identity management features. It simplifies user authentication and access control while providing a
secure and scalable solution.

**Optimization Goals**:

- **AWS Native & Serverless**: Amazon Cognito is a fully managed service by AWS, reducing operational complexity and
  server management.
- **Minimizing Management and Cost**: It offers pay-as-you-go pricing and automates user management tasks, helping to
  lower operational costs.

**Documentation**: [Amazon Cognito Documentation](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)

**Service Limits**: [Amazon Cognito Limits](https://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)

## AWS Secrets Manager

**Service Description**: AWS Secrets Manager helps you protect access to your applications, services, and IT resources
without the upfront cost and complexity of managing your own infrastructure. It enables you to rotate, manage, and
retrieve secrets throughout their lifecycle.

**Selection Rationale**: We opted for AWS Secrets Manager to centrally manage and secure access to sensitive
information, such as database credentials and API keys. Its integration with AWS services were pivotal in our decision.

**Optimization Goals**:

- **AWS Native & Serverless**: AWS Secrets Manager is a fully managed service, eliminating the need for infrastructure
  management.

**Documentation**: [AWS Secrets Manager Documentation](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)

**Service Limits**: [AWS Secrets Manager Limits](https://docs.aws.amazon.com/secretsmanager/latest/userguide/limits.html)

## Amazon EventBridge

**Service Description**: Amazon EventBridge is a serverless event bus service that makes it easy to connect applications
using data from your own applications, integrated Software-as-a-Service (SaaS) applications, and AWS services.

**Selection Rationale**: We selected Amazon EventBridge for its seamless integration capabilities across AWS services.
It simplifies event-driven architectures by decoupling event producers from consumers and
supporting event filtering and transformation. Especially the content based routing is pivotal in our architecture.

**Optimization Goals**:

- **AWS Native & Serverless**: Amazon EventBridge is serverless, handling all infrastructure provisioning and scaling
  automatically.
- **Minimizing Management and Cost**: It reduces operational overhead by managing event routing and processing,
  optimizing costs through pay-as-you-go pricing.

**Documentation**: [Amazon EventBridge Documentation](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html)

**Service Limits**: [Amazon EventBridge Limits](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-quota.html)

## AWS Lambda

**Service Description**: AWS Lambda is a serverless compute service that lets you run code without provisioning or
managing servers. It automatically scales your application by running code in response to triggers and manages resources
on your behalf.

**Selection Rationale**: We chose AWS Lambda for its serverless architecture, which eliminates the need for server
management and optimizes resource utilization based on workload demands. Its seamless integration with other AWS
services facilitates event-driven computing models.

**Optimization Goals**:

- **AWS Native & Serverless**: AWS Lambda is fully managed by AWS, reducing operational complexity and administrative
  overhead.
- **Minimizing Management and Cost**: It optimizes cost by charging only for compute time consumed and automatically
  scaling resources as needed.

**Documentation**: [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

**Service Limits**: [AWS Lambda Limits](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html)

## AWS AppSync

**Service Description**: AWS AppSync simplifies application development by letting you create scalable APIs that
securely access data from multiple sources. It supports real-time and offline capabilities, making it suitable for
mobile and web applications.

**Selection Rationale**: We selected AWS AppSync for its managed GraphQL service, which reduces the complexity of API
development and data synchronization. Its ability to handle real-time updates aligns with our
application’s requirements.

**Optimization Goals**:

- **AWS Native & Serverless**: AWS AppSync is fully managed by AWS, minimizing operational overhead and infrastructure
  management.
- **Minimizing Management and Cost**: It optimizes cost by providing built-in scaling and pay-as-you-go pricing,
  ensuring efficient resource utilization.

**Documentation**: [AWS AppSync Documentation](https://docs.aws.amazon.com/appsync/latest/devguide/what-is-appsync.html)

**Service Limits**: [AWS AppSync Limits](https://docs.aws.amazon.com/general/latest/gr/appsync.html#limits_appsync)
