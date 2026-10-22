# Amazon VPC Security — Notes

## 1. VPC Security Overview

AWS provides security controls at different levels of the VPC.

```text
Internet
   ↓
VPC
   ↓
Network ACL        ← Subnet-level security
   ↓
Subnet
   ↓
Security Group     ← EC2-level security
   ↓
EC2 Instance
```

| Feature | Network ACL (NACL) | Security Group |
|---|---|---|
| Applied to | Subnet | EC2 instance / network interface |
| Type | Virtual firewall | Virtual firewall |
| Stateful? | ❌ Stateless | ✅ Stateful |
| Rules | Allow + Deny | Allow only |
| Inbound rules | Yes | Yes |
| Outbound rules | Yes | Yes |
| Main purpose | Control traffic at subnet level | Control traffic to/from EC2 |

---

## 2. Network Access Control List (NACL)

A **Network ACL (NACL)** is a virtual firewall that operates at the **subnet level**.

It controls:

- **Inbound traffic** → traffic entering the subnet
- **Outbound traffic** → traffic leaving the subnet

```text
                 VPC
                  │
              ┌───┴───┐
              │ Router│
              └───┬───┘
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
   Network ACL A        Network ACL B
        ↓                   ↓
     Subnet 1            Subnet 2
```

### Key point

Because a NACL works at the subnet level, its rules can affect **all resources inside that subnet**.

---

## 3. NACL Rules

A NACL has separate rule sets for:

### Inbound rules

Control traffic **coming into the subnet**.

### Outbound rules

Control traffic **leaving the subnet**.

A rule can specify:

- Rule number
- Protocol
- Port
- Source (for inbound)
- Destination (for outbound)
- Allow or Deny

Example:

```text
Rule #    Protocol    Port    Source       Action
---------------------------------------------------
100       All         All     0.0.0.0/0    ALLOW
*         All         All     0.0.0.0/0    DENY
```

---

## 4. NACL Rule Evaluation

NACL rules are evaluated in **ascending rule-number order**.

> **Only the first matching rule is applied.**

The remaining rules are **not evaluated** for that packet.

Example:

```text
Rule 100 → ALLOW
Rule 110 → DENY
Rule 120 → ALLOW
```

If traffic matches Rule 100:

```text
Traffic
   ↓
Rule 100?
   ↓
YES → ALLOW
   ↓
STOP
```

Rules 110 and 120 are not checked.

If Rule 100 does not match:

```text
Traffic
   ↓
Rule 100?
   ↓
NO
   ↓
Rule 110?
   ↓
YES → DENY
   ↓
STOP
```

### The `*` rule

The `*` rule is the final fallback.

If traffic does not match any preceding rule, the `*` rule is applied.

```text
Rule 100 → no match
     ↓
Rule 110 → no match
     ↓
Rule 120 → no match
     ↓
* → DENY
```

### Easy mental model

NACL evaluation works like:

```text
if Rule 100 matches:
    apply Rule 100

else if Rule 110 matches:
    apply Rule 110

else if Rule 120 matches:
    apply Rule 120

else:
    apply * rule
```

> **NACL = lowest rule number → first match → stop**

---

## 5. Default NACL

The default NACL allows all IPv4 traffic in and out of the subnet.

### Inbound

```text
Rule 100
All IPv4
All ports
0.0.0.0/0
→ ALLOW
```

The final fallback is:

```text
Rule *
→ DENY anything not already matched
```

### Outbound

The same basic structure applies:

```text
Rule 100
All IPv4
All ports
0.0.0.0/0
→ ALLOW
```

Therefore:

> **Default NACL = allows all IPv4 traffic in and out.**

You generally do not need to change the initial default NACL settings unless you need additional subnet-level security.

---

## 6. NACLs Are Stateless

A NACL is **stateless**.

That means:

> **Inbound and outbound traffic are evaluated independently.**

If you allow traffic in one direction, the response traffic is **not automatically allowed**.

### Example: HTTPS

A client sends an HTTPS request:

```text
Client
   │
   │ HTTPS → port 443
   ↓
Subnet
```

You might allow:

```text
Inbound:
TCP 443 → ALLOW
```

But this does **not automatically allow the response**.

The course example uses:

```text
Inbound:
TCP 443 → ALLOW

Outbound:
TCP 1025–65535 → ALLOW
```

### Why?

The client connects to the server's HTTPS port:

```text
Client → Server
          TCP 443
```

The server's response goes back to the client's **ephemeral port**.

Therefore, with a stateless NACL, both directions need appropriate rules.

```text
NACL
 ↓
Stateless
 ↓
Inbound and outbound evaluated independently
 ↓
Response traffic needs its own matching rule
```

---

## 7. Example: Web Server NACL

For a web server:

```text
Internet
    │
    │ HTTPS : 443
    ↓
 [NACL]
    │
    ↓
Web Server
```

### Inbound rule

```text
Source:    0.0.0.0/0
Protocol:  TCP
Port:      443
Action:    ALLOW
```

### Outbound rule

```text
Destination: 0.0.0.0/0
Protocol:    TCP
Port:        1025–65535
Action:      ALLOW
```

This allows HTTPS requests to reach the server and the corresponding response traffic to leave the subnet.

---

## 8. Security Groups

A **security group** is another virtual firewall, but it operates at the **EC2 instance / network interface level**.

```text
Subnet
   │
   ├── EC2 A
   │     └── Security Group A
   │
   ├── EC2 B
   │     └── Security Group B
   │
   └── EC2 C
         └── Security Group C
```

Security groups provide the next security layer for EC2 instances.

---

## 9. Default Security Group Behavior

By default, a security group:

### Inbound

```text
❌ Blocked
```

There are initially no inbound rules.

### Outbound

```text
✅ Allowed
```

All outbound traffic is allowed by default.

So:

```text
Inbound  → ❌ Blocked by default
Outbound → ✅ Allowed by default
```

To allow incoming traffic, you must explicitly create an **inbound rule**.

---

## 10. Security Groups Are Stateful

A security group is **stateful**.

That means it remembers the connection.

Example:

```text
Client
   │
   │ Request
   ↓
EC2
   │
   │ Response
   ↓
Client
```

If the connection is allowed, the response traffic is automatically allowed.

You do not need to create a separate rule just to allow the response traffic for that established connection.

### Easy memory trick

```text
NACL → Stateless → Both directions need rules

SG   → Stateful  → Response traffic is automatically allowed
```

---

## 11. Allowing Web Traffic with Security Groups

Suppose an EC2 instance is running a web server.

You want users on the internet to access:

- HTTP → port **80**
- HTTPS → port **443**

You can create inbound rules:

```text
Type       Port    Source
------------------------------
HTTP       80      0.0.0.0/0
HTTPS      443     0.0.0.0/0
```

### Address notation

`0.0.0.0/0` means:

> Any IPv4 address.

For IPv6:

```text
::/0
```

means any IPv6 address.

---

## 12. Security Groups and Application Tiers

Security groups can be used to isolate different application tiers.

```text
Internet
   │
   │ HTTPS : 443
   ↓
┌─────────────────┐
│    Web Tier     │
│ Security Group  │
└────────┬────────┘
         │
         │ HTTP : 80
         ↓
┌─────────────────┐
│   App Tier      │
│ Security Group  │
└────────┬────────┘
         │
         │ MySQL : 3306
         ↓
┌─────────────────┐
│  Database Tier  │
│ Security Group  │
└─────────────────┘
```

### Web Tier

```text
Inbound:
HTTPS : 443
Source: Internet
```

### Application Tier

```text
Inbound:
HTTP : 80
Source: Web Tier
```

### Database Tier

```text
Inbound:
MySQL : 3306
Source: Application Tier
```

This creates controlled communication between the tiers.

---

## 13. Why Use Security Groups This Way?

Instead of allowing unrestricted communication:

```text
Internet
   ↓
Everything
   ↓
Everything
   ↓
Database
```

you restrict communication to only what each tier needs:

```text
Internet
   │
   │ HTTPS 443
   ↓
Web Tier
   │
   │ HTTP 80
   ↓
Application Tier
   │
   │ MySQL 3306
   ↓
Database
```

This creates isolation between application tiers.

---

## 14. NACL vs Security Group

```text
                VPC
                 │
          ┌──────┴──────┐
          │             │
      Network ACL   Security Group
          │             │
       Subnet           EC2
```

| Feature | Network ACL | Security Group |
|---|---|---|
| Level | Subnet | EC2 / network interface |
| Stateful | ❌ No | ✅ Yes |
| Allow rules | ✅ Yes | ✅ Yes |
| Deny rules | ✅ Yes | ❌ No |
| Inbound default | Allows in default NACL | Blocks by default |
| Outbound default | Allows | Allows |
| Rule evaluation | Lowest rule number → first match | All applicable rules |
| Response traffic | Must be explicitly permitted | Automatically allowed |

---

## 15. Simple Mental Model

Think of a building:

```text
NACL
 ↓
Building security gate
 ↓
Controls traffic entering/leaving the building
```

Then:

```text
Security Group
 ↓
Individual apartment door
 ↓
Controls access to a particular apartment
```

In AWS:

```text
NACL
 ↓
Subnet-level firewall

Security Group
 ↓
EC2-level firewall
```

---

## 16. Key Interview Points

### Network ACL

- Works at the **subnet level**
- **Stateless**
- Supports **ALLOW and DENY**
- Has separate inbound and outbound rules
- Rules are evaluated in **ascending rule-number order**
- **First matching rule is applied**
- `*` is the final fallback rule
- Response traffic needs its own appropriate rule

### Security Group

- Works at the **EC2 / network interface level**
- **Stateful**
- Supports **ALLOW rules only**
- No explicit DENY rules
- Inbound traffic is blocked by default
- Outbound traffic is allowed by default
- Response traffic is automatically allowed for an allowed connection

---

## 17. One-Line Memory Trick

```text
NACL → Subnet → Stateless → Allow + Deny → First matching rule

SG   → EC2    → Stateful  → Allow only   → Response automatically allowed
```

> **NACL = subnet-level firewall**  
> **Security Group = EC2-level firewall**
