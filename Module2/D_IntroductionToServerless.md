# Introduction to Serverless

## What is Serverless?

**Serverless computing** is a cloud computing model where AWS manages the **underlying servers and infrastructure**, allowing you to focus mainly on your application or workload.

> **Serverless does NOT mean there are no servers.**  
> It means **you don't have to provision, manage, patch, or scale the underlying servers yourself.**

---

## From EC2 to Serverless

The amount of infrastructure you manage depends on the service you choose.

### 1. Amazon EC2

With **EC2**, AWS manages the physical hardware, while you manage the **logical infrastructure** of the virtual machine.

You are responsible for:

- Guest operating system
- Software installation
- Security and patching
- Networking configuration
- Server capacity and scaling

```text
AWS
 ↓
Physical Hardware
 ↓
EC2
 ↓
Guest OS
 ↓
Application

       ↑
  You manage this
```

> **EC2 → More infrastructure management and more control.**

---

### 2. ECS/EKS on EC2

With **ECS or EKS**, AWS manages part of the container management, such as **container orchestration and cluster management**.

However, when they run on EC2, you still manage the **underlying EC2 instances**.

```text
ECS / EKS
    ↓
EC2 Instances      ← You manage
    ↓
Containers
    ↓
Application
```

> **ECS/EKS + EC2 → AWS manages container orchestration, but you still manage the servers.**

---

### 3. Serverless Compute

With serverless compute, AWS manages the **underlying compute infrastructure**, so you don't have to manage EC2 instances.

```text
Serverless Service
        ↓
   Application
```

For example:

```text
ECS + Fargate
        ↓
   Container
        ↓
  Application
```

Here, **Fargate manages the underlying compute infrastructure**, so you can focus on your containers instead of EC2 instances.

> **Serverless → Less infrastructure management and more focus on the application.**

---

# Undifferentiated Heavy Lifting

**Undifferentiated heavy lifting** means infrastructure work that is necessary to run an application but does not directly provide unique value to the application.

For example:

```text
Patch servers
Manage OS
Replace failed servers
Manage server capacity
Configure scaling
Maintain availability
```

These tasks are important, but they are not the application's actual business functionality.

Serverless services remove much of this work by letting **AWS manage the underlying infrastructure**.

> **Key idea:** Serverless lets you spend more time building your application and less time managing servers.

---

# Characteristics of Serverless

Serverless computing has four important characteristics:

### 1. No Servers to Provision or Manage

You don't have to:

- Provision EC2 instances
- Manage operating systems
- Patch servers
- Replace failed servers
- Manage server capacity

AWS manages the underlying infrastructure.

### 2. Scales with Usage

Serverless services can adjust compute resources according to workload or usage.

```text
Low usage  → Less compute
High usage → More compute
```

### 3. No Payment for Idle Resources

You don't have to keep your own servers continuously provisioned just in case they are needed.

Serverless services are designed around **usage-based resource consumption**.

### 4. Built-in Availability and Fault Tolerance

AWS manages much of the underlying infrastructure required for availability and fault tolerance.

You don't have to build the entire server infrastructure yourself to handle failures.

---

# EC2 vs ECS/EKS vs Serverless

| | EC2 | ECS/EKS + EC2 | Serverless |
|---|---|---|---|
| Physical hardware | AWS | AWS | AWS |
| Server/VM management | **You** | **You** | **AWS** |
| Container orchestration | — | **ECS/EKS** | Service-dependent |
| OS management | **You** | **You** | **AWS** |
| Infrastructure scaling | **You** | **You** | **AWS/service-managed** |
| Infrastructure control | High | High | Lower |
| Infrastructure management | High | Medium | Low |

---

# The Key Progression

```text
EC2
 ↓
You manage the VM infrastructure
 ↓
More control / More management

ECS/EKS + EC2
 ↓
AWS manages container orchestration
but you still manage EC2
 ↓
Less management than EC2 alone

ECS + Fargate
 ↓
AWS manages the underlying compute
 ↓
You focus on containers

Lambda
 ↓
AWS manages the underlying compute
 ↓
You focus mainly on your code
```

> **Remember:**
>
> **EC2 → You manage the VM**
>
> **ECS/EKS + EC2 → AWS manages containers, you manage EC2**
>
> **Fargate → AWS manages the underlying compute, you manage containers**
>
> **Lambda → AWS manages the underlying compute, you focus mainly on code**
>
> **Serverless = No servers for you to manage, not no servers at all.**
