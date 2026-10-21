# AWS Lambda — Core Concepts

## 1. Lambda Function

A **Lambda function** is an AWS resource that contains code that Lambda can execute when the function is invoked.

### Ways to create a Lambda function

- **From scratch** — write your own function code.
- **Blueprint** — start from an AWS-provided template.
- **Container image** — deploy the function as a container image.
- **Serverless Application Repository** — use a published serverless application.

---

## 2. Trigger

A **trigger** defines when a Lambda function should run.

A trigger integrates Lambda with another AWS service or event source so that the function can execute automatically.

### Example

```text
Image uploaded to S3
        ↓
    S3 Trigger
        ↓
Lambda Function
```

> **Trigger = What causes the Lambda function to run?**

---

## 3. Event

An **event** is a JSON-formatted document containing information that the Lambda function needs to process.

The runtime passes the event to the function when it is invoked.

For an S3-triggered Lambda, the event can contain:

- Bucket name
- Object key
- Event type

Example:

```json
{
  "Records": [
    {
      "eventName": "ObjectCreated:Put",
      "s3": {
        "bucket": {
          "name": "my-image-resizing-bucket-123"
        },
        "object": {
          "key": "input/image.jpg"
        }
      }
    }
  ]
}
```

> **Event = Data describing what happened and what the function needs to process.**

---

## 4. Application Environment

An **application environment** is the secure and isolated runtime environment in which a Lambda function executes.

It provides the processes and resources required to run the function.

```text
Lambda Function
      ↓
Application Environment
      ↓
Code executes
```

AWS manages this execution environment for you.

---

## 5. Deployment Package

A **deployment package** contains the code and dependencies required by a Lambda function.

Lambda supports two deployment package types.

### ZIP file

A ZIP package can contain:

```text
lambda_function.py
Dependencies
```

AWS provides the operating system and Lambda runtime.

### Container image

A container image contains the function code and dependencies and must also include the operating system and Lambda runtime.

### Comparison

| ZIP package | Container image |
|---|---|
| Code + dependencies | Code + dependencies + OS/runtime |
| AWS provides OS/runtime | You include OS/runtime in the image |
| Simple and common for many functions | Useful when container-based packaging is preferred |

---

## 6. Runtime

The **runtime** provides the language-specific environment used to execute the Lambda code.

Examples include:

- Python
- Node.js
- Java
- Go
- Ruby
- .NET

For example, if the runtime is **Python 3.14**, Lambda provides the Python 3.14 environment required to execute the Python function.

```text
Lambda Function
      ↓
Python 3.14 Runtime
      ↓
Python code executes
```

> **Runtime = The language environment used to run the function.**

---

## 7. Lambda Function Handler

The **handler** is the entry point of a Lambda function. It identifies the function that Lambda calls when the function is invoked.

For Python:

```python
def handler_name(event, context):
    # function logic
    return some_value
```

### Example

If the file is:

```text
lambda_function.py
```

and it contains:

```python
def lambda_handler(event, context):
    print("Processing event")
```

the Lambda handler configuration is:

```text
lambda_function.lambda_handler
```

Here:

```text
lambda_function
      ↓
lambda_function.py

lambda_handler
      ↓
Function inside the file
```

### Handler parameters

```python
def lambda_handler(event, context):
```

- **`event`** — contains the input/event data.
- **`context`** — provides information about the current Lambda execution.

When the handler returns, the invocation is complete.

---

## 8. Billing Granularity

Lambda uses a **pay-for-use** model.

You are charged based on:

1. **Number of requests/invocations**
2. **Duration of function execution**

Lambda measures execution duration in milliseconds and rounds the duration to the nearest **1 millisecond**, with no minimum run time.

This allows Lambda to be cost-effective for workloads where code runs only when needed.

---

## 9. Serverless

Lambda is a **serverless** compute service.

Serverless does **not** mean that servers do not exist.

It means AWS manages the underlying server infrastructure, while you focus on your function code.

### Traditional server-based approach

```text
You
 ↓
Server
 ↓
OS / Software / Capacity Management
 ↓
Application
```

### Lambda approach

```text
You
 ↓
Lambda Function
 ↓
AWS manages underlying infrastructure
 ↓
Your code executes
```

The main benefit is that you don't need to provision or manage servers for the function.

---

# Complete Lambda Flow

```text
Something happens
      ↓
   Trigger
      ↓
    Event
      ↓
Lambda Function
      ↓
   Runtime
      ↓
   Handler
      ↓
   Your Code
      ↓
    Result
```

### Example: S3 Image Resizing

```text
User uploads image
        ↓
S3 Trigger
        ↓
S3 Event (JSON)
        ↓
Lambda Function
        ↓
Python Runtime
        ↓
lambda_handler(event, context)
        ↓
Resize image
        ↓
Store thumbnail in S3
```
