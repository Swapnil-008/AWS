# What is the Cloud?

## Cloud Computing

**Cloud computing** is the on-demand delivery of IT resources over the internet with **pay-as-you-go pricing**.

Instead of buying and maintaining physical infrastructure, organizations can use resources provided by cloud providers such as **AWS**.

### Traditional On-Premises

- Company buys physical servers, storage, and networking equipment.
- Company manages and maintains the infrastructure.
- Setting up new environments can take days or weeks.
- Requires significant upfront cost and infrastructure management.

### Cloud

- AWS owns and maintains the physical data centers.
- Customers use AWS resources over the internet.
- New environments can be created in minutes or seconds.
- Resources can be scaled up or down as needed.
- Customers pay for the resources they use.

## Undifferentiated Heavy Lifting

**Undifferentiated heavy lifting** means managing repetitive infrastructure tasks that do not directly differentiate a business.

Examples:
- Managing servers
- Maintaining hardware
- Installing virtual machines
- Managing backups
- Running data centers

Cloud computing lets organizations focus on their **business logic and applications** while AWS handles much of the underlying infrastructure.

## AWS

**AWS (Amazon Web Services)** provides cloud computing services that can be used to build **scalable, highly available, and cost-effective** applications.

## Six Benefits of Cloud Computing

### 1. Pay as You Go

Pay only for the computing resources you use instead of making large upfront investments in hardware.

### 2. Massive Economies of Scale

AWS serves a large number of customers, allowing it to operate at a large scale and provide lower pay-as-you-go costs.

### 3. Stop Guessing Capacity

You don't need to predict infrastructure capacity in advance.

- Scale up when demand increases.
- Scale down when demand decreases.
- Avoid expensive idle resources or insufficient capacity.

### 4. Increase Speed and Agility

Cloud resources can be provisioned in minutes instead of taking weeks to purchase and install physical infrastructure.

This enables faster development, testing, and experimentation.

### 5. Stop Maintaining Data Centers

AWS handles much of the physical infrastructure management, allowing organizations to focus on their customers, applications, and business.

### 6. Go Global in Minutes

AWS allows applications to be deployed across multiple **Regions** around the world.

Benefits include:
- Lower latency
- Better user experience
- Global availability

## Key Takeaways

- **Cloud computing:** On-demand IT resources over the internet with pay-as-you-go pricing.
- AWS provides cloud computing services.
- Cloud reduces infrastructure management and undifferentiated heavy lifting.
- Cloud makes it easier to scale resources and deploy applications quickly.
- AWS helps organizations build scalable, highly available, and cost-effective infrastructure.

---

# AWS Global Infrastructure

AWS Global Infrastructure consists of physical infrastructure such as **data centers and networking**, organized into **Regions** and **Availability Zones (AZs)**.

## AWS Regions

An **AWS Region** is a geographic location where AWS hosts its data centers.

Each Region has:
- A geographical name
- A Region code

### Examples

| Region Code | Geographic Name |
|---|---|
| `us-east-1` | N. Virginia |
| `ap-northeast-1` | Tokyo |

AWS Regions are **independent**. Data is not automatically replicated between Regions unless you explicitly configure it.

## Choosing an AWS Region

Consider these four factors:

1. **Latency** – Choose a Region close to your users to reduce network delay.
2. **Price** – Pricing can vary between Regions due to local operating costs.
3. **Service Availability** – Some AWS services may not be available in every Region.
4. **Compliance** – Some regulations require data to be stored in a specific geographic location.

## Availability Zones (AZs)

An **Availability Zone** is one or more data centers within an AWS Region.

Each AZ has:
- Redundant power
- Redundant networking
- Redundant connectivity
- High-speed, low-latency connections to other AZs

### AZ Naming

An AZ is identified by adding a letter to the Region code.

Examples:

- `us-east-1a` → AZ in `us-east-1`
- `sa-east-1b` → AZ in `sa-east-1`

```text
AWS Region
├── Availability Zone A
│   └── Data Center(s)
│
└── Availability Zone B
    └── Data Center(s)
```

## AWS Service Scope

AWS services can operate at different scopes:

- **Availability Zone (AZ) level**
- **Region level**
- **Global level**

The scope determines where the resources are deployed and how availability and resiliency are handled.

### Region-Scoped Services

You only select the Region. AWS generally manages availability and durability for the service.

### AZ-Scoped Services

You select a specific AZ. You may need to design for higher availability and durability by using multiple AZs.

## Maintaining Resiliency

For high availability:

- Prefer **Region-scoped managed services** when possible.
- If an AZ-specific service is used, replicate resources across **multiple AZs**.
- Use at least **two AZs** so the application can continue running if one AZ fails.

```text
Region
├── AZ 1 → Application
└── AZ 2 → Application

If AZ 1 fails → AZ 2 continues serving traffic
```

---

# Ways to Interact with AWS

Every action performed in AWS is an **API call** that must be **authenticated and authorized**.

AWS services and resources can be accessed through:

1. **AWS Management Console**
2. **AWS Command Line Interface (CLI)**
3. **AWS Software Development Kits (SDKs)**

## AWS Management Console

The **AWS Management Console** is a web-based interface used to create and manage AWS resources through a browser.

- Beginner-friendly and visual.
- AWS services are organized into categories such as Compute, Storage, Database, and Security.
- The **Region selector** allows you to choose the AWS Region where requests are made.

## AWS Command Line Interface (CLI)

The **AWS CLI** is a unified command-line tool used to manage AWS services.

- Manage multiple AWS services from the command line.
- Automate tasks using scripts.
- Useful for repetitive or scheduled tasks.
- Available for Windows, Linux, and macOS.
- Open-source.

Example:

```bash
aws ec2 describe-instances
```

This makes an API call to Amazon EC2 and returns information about EC2 instances.

## AWS Software Development Kits (SDKs)

**AWS SDKs** allow developers to make AWS API calls directly from application code.

AWS provides SDKs for popular languages such as Python, Java, JavaScript, C++, Go, .NET, PHP, and Ruby.

### Python Example

The AWS SDK for Python is called **Boto3**.

```python
import boto3

ec2 = boto3.client("ec2")

response = ec2.describe_instances()

print(response)
```

This code makes an API call to Amazon EC2 and retrieves instance information.

## Quick Comparison

| Method | How you use it | Best for |
|---|---|---|
| **Management Console** | Web browser | Beginners and visual management |
| **AWS CLI** | Command line | Automation and scripts |
| **AWS SDK** | Programming code | Integrating AWS into applications |

## Key Takeaway

> **Console, CLI, and SDKs are different ways to make API calls to AWS services.**
