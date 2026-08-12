# AWS Shared Responsibility Model

When working with AWS Cloud, **security and compliance are a shared responsibility** between AWS and the customer.

AWS describes this using the **Shared Responsibility Model**, which separates responsibilities into:

- **Security of the cloud** → AWS responsibility
- **Security in the cloud** → Customer responsibility

## AWS Responsibility: Security of the Cloud

AWS is responsible for protecting and securing the **infrastructure that runs AWS services**.

This includes:

- Physical security of AWS data centers
- AWS Regions and Availability Zones
- Physical servers and hardware
- Host operating systems
- Virtualization layers
- AWS networking components
- Underlying software and infrastructure

In simple terms:

> **AWS is responsible for the security of the infrastructure that provides the cloud.**

## AWS Responsibility Based on Service Type

The amount of infrastructure AWS manages depends on the type of AWS service.

| Service Category | Example | AWS Responsibility |
|---|---|---|
| **Infrastructure services** | Amazon EC2 | AWS manages the underlying infrastructure and foundation services. |
| **Container services** | Amazon RDS | AWS manages the infrastructure, foundation services, operating system, and application platform. |
| **Abstracted services** | Amazon S3 | AWS manages the infrastructure, operating system, platforms, server-side encryption, and data protection. |

### Container Services

Here, **container services** means AWS abstracts the application platform behind the scenes. It does **not** specifically refer to Docker containers.

This reduces the amount of platform management required from the customer.

---

# Customer Responsibility: Security in the Cloud

Customers are responsible for **security within the AWS environment they use**.

This includes properly configuring AWS services and applications and protecting their data.

The exact responsibility depends on the AWS service being used.

> **The more infrastructure AWS manages, the less infrastructure the customer needs to manage.**

## Customer Responsibility Based on Service Type

| Service Category | AWS Responsibility | Customer Responsibility |
|---|---|---|
| **Infrastructure services** | Infrastructure and foundation services | Operating system, application platform, data, encryption, and protection |
| **Container services** | Infrastructure, operating system, and application platform | Customer data, encryption, firewalls, and backups |
| **Abstracted services** | Infrastructure, operating system, platforms, server-side encryption, and data protection | Customer data and client-side encryption |

## Customer Responsibilities in Practice

Customers are responsible for:

- **Data protection** – Protecting customer data using appropriate security mechanisms.
- **Encryption** – Encrypting data when required.
- **Access control** – Controlling who can access data and AWS resources.
- **Backups** – Managing backups where the service requires customer responsibility.
- **Configuration** – Properly configuring AWS services and applications.
- **Data sovereignty** – Choosing an appropriate AWS Region when regulations require data to remain in a specific geographic location.

## Important Concept

The Shared Responsibility Model does **not** mean AWS is responsible for all security.

AWS secures the **cloud infrastructure**, while the customer is responsible for securing what they **put and configure in the cloud**.

```text
              AWS Shared Responsibility
                       │
          ┌────────────┴────────────┐
          │                         │
   Security of the Cloud      Security in the Cloud
          │                         │
        AWS                    Customer
          │                         │
   Infrastructure            Data & Access
   Data Centers              Configuration
   Hardware                  Encryption
   Networking                Backups
   Virtualization            Applications
```

## Key Takeaway

> **AWS is responsible for security OF the cloud, while the customer is responsible for security IN the cloud.**

The exact division of responsibility depends on the AWS service. More **abstracted/managed services** generally require less infrastructure security management from the customer.
