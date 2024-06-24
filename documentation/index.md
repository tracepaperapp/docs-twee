---
template: home.html
title: Home
---

Introducing Tracepaper by Draftsman, a revolutionary tool designed to simplify the complexities of building business
applications. While the landscape is rich with low-code tools to aid development, the challenges of managing these tools
in production can be daunting. From maintaining performance to ensuring granular access control and backups, the
operational demands can strain businesses lacking a robust engineering department.

Enter serverless services offered by major cloud providers like AWS, promising to alleviate this operational burden. "
Serverless is how the cloud wants you to build applications," as Gregor Hohpe puts it. But is this approach practical?
In practice, serverless architecture introduces a high level of runtime granularity, where each line in your domain
model translates into network calls fraught with access control, latency, and security considerations. It requires a
shift from operational competence to distributed system design proficiency.

However, if your expertise lies in your domain rather than distributed systems intricacies, you likely prefer to reason
about your domain holistically—a "Monolith," in industry terms. This is where Tracepaper shines, drawing inspiration
from Domain Driven Design principles. We empower you to model your domain using distinct boxes connected by lines,
representing various elements such as API access points, domain behaviors, and materialized views. Our modeling tool
prioritizes data structures and the connections between these boxes, allowing you to focus on the essence of your
domain.

While we provide modeling concepts for the business logic within these boxes, the true strength lies in the ability to
inject custom Python code. Transitioning from pseudo-code to Python is seamless, freeing you from concerns about the
connections between your logic.

Let us handle the lines, so you can concentrate on the boxes—the core of your domain expertise. The model seamlessly
converts into a Python project defined with CloudFormation, AWS's Infrastructure as Code specification. This ensures
deployability within your AWS account, granting you full control over runtime parameters such as access and cost.
Additionally, both the model and the generated project are stored in your GitHub account, offering complete oversight
and ownership.

With Tracepaper, we streamline the process of modeling your domain as a distributed system. Let us handle the
intricacies, while you focus on what truly matters—your domain.
