# Introduction to AWS Networking

## 1. Networking

**Networking** is the process of connecting computers and other devices so they can communicate and exchange data.

In AWS, networking is important because it controls how applications and resources communicate with each other and with the internet.

### Networking and AWS

When you launch an EC2 instance, you select a **VPC (Virtual Private Cloud)**.

AWS creates a **default VPC** in each Region, which makes it easy to launch EC2 instances and access applications over the internet.

However, default VPCs should be used carefully because resources placed in them can have internet connectivity.

For more control and security, you can create a **custom VPC** and configure its networking components according to your application's requirements.

> **Important:** The networking concepts in this section are especially relevant to **EC2-based architectures**. Some compute services, such as Lambda, may not require you to configure a network directly.

---

## 2. Networking Basics

A simple way to understand networking is to compare it with sending a letter.

A letter needs:

- **Sender address** — where the letter comes from
- **Recipient address** — where the letter is going
- **Message** — the actual information being sent

Similarly, computers need addresses so that data can be delivered to the correct destination.

The process of determining where data should be sent is called **routing**.

### Key idea

> **Address → identifies the destination**  
> **Routing → determines where the data should go**

---

## 3. IP Addresses

An **IP address (Internet Protocol address)** identifies a computer or network interface so that data can be routed to the correct destination.

An IPv4 address consists of **32 bits**, represented internally as binary values (`0` and `1`).

Example:

```text
11000000 10101000 00000001 00011110
```

This is a **32-bit IPv4 address**.

### Why IP addresses are needed

Just as a physical address identifies a destination for a letter, an IP address identifies a destination in a computer network.

---

## 4. IPv4 Notation

Although IPv4 addresses can be represented in binary, they are normally written in **decimal notation** for readability.

The 32 bits are divided into **four groups of 8 bits**, called **octets**.

```text
11000000 10101000 00000001 00011110
    ↓        ↓        ↓        ↓
   192      168       1        30
```

Therefore:

```text
192.168.1.30
```

is the decimal representation of the binary address above.

### IPv4 structure

```text
192 . 168 . 1 . 30
 8     8    8    8   bits
```

Each octet contains **8 bits**, giving:

```text
8 × 4 = 32 bits
```

---

## 5. CIDR Notation

An individual IP address identifies one address, such as:

```text
192.168.1.30
```

But when working with networks, we often need to represent a **range of IP addresses**.

**CIDR (Classless Inter-Domain Routing)** provides a compact way to represent an IP address range.

### CIDR format

```text
IP-address/prefix-length
```

Example:

```text
192.168.1.0/24
```

The number after `/` indicates how many bits are **fixed**.

---

## 6. Understanding `/24`

Consider:

```text
192.168.1.0/24
```

IPv4 contains **32 bits**.

```text
32 total bits
-24 fixed bits
----------------
 8 flexible bits
```

So:

- **24 bits** are fixed.
- **8 bits** are flexible.
- Each flexible bit can be either `0` or `1`.

Therefore, the number of possible addresses is:

```text
2⁸ = 256
```

So:

```text
192.168.1.0/24
```

represents a range containing **256 IP addresses**.

---

## 7. CIDR Prefix Length

The prefix length determines the size of the IP range.

### General rule

> **Larger prefix length → smaller IP range**  
> **Smaller prefix length → larger IP range**

For example:

```text
192.168.1.0/24
```

is a smaller range than:

```text
192.168.1.0/16
```

because `/16` leaves more bits available for addresses.

### Quick comparison

| CIDR | Fixed Bits | Flexible Bits | Address Range Size |
|---|---:|---:|---:|
| `/16` | 16 | 16 | 65,536 |
| `/24` | 24 | 8 | 256 |
| `/28` | 28 | 4 | 16 |

---

## 8. CIDR in AWS

When creating a network in AWS, you specify its size using **CIDR notation**.

According to the course material:

- Smallest supported IP range: **`/28`**
- Largest supported IP range: **`/16`**

Therefore:

```text
/28 → 16 IP addresses
/16 → 65,536 IP addresses
```

### Important mental model

```text
CIDR prefix
     ↓
Number of fixed bits
     ↓
Number of flexible bits
     ↓
Size of IP address range
```
