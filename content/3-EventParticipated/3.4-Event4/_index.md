---
title: "Event 4"
date: 2026-04-10
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# Event 4: AWS Networking, Security and Identity & Access Management

### Event Information

- **Event Name:** AWS Networking, Security & IAM Workshop
- **Date & Time:** 9:00 a.m 11st April
- **Location:** Academy Hall FPT university
- **Role:** Attendee

---

## Event Description

This workshop provided a comprehensive overview of core AWS concepts, focusing on networking, security, and identity management.

The session aimed to help participants understand how to design secure and scalable cloud architectures by combining networking components, access control, and security services in AWS.

---

## Main Content

### 1. Identity & Access Management (IAM)

The workshop introduced **AWS IAM**, a service used to control access to AWS resources.

#### Key Concepts

- Manage **users, groups, and roles**
- Control **authentication and authorization**
- Apply permissions through policies

#### Best Practices

- Apply the **least privilege principle**
- Avoid using root account for daily operations
- Enable **MFA (Multi-Factor Authentication)**
- Rotate credentials regularly
- Avoid using long-term access keys

#### Advanced Topics

- **Single Sign-On (SSO):** One login for multiple systems
- **Service Control Policies (SCP):** Define maximum permissions across accounts
- **Permission Boundaries:** Limit permissions for specific users/roles

---

### 2. AWS Networking

This section focused on how networking works in AWS using **VPC (Virtual Private Cloud)**.

#### Core Components

- **VPC & CIDR:** Define network range
- **Subnets:** Divide network into smaller segments
- **Internet Gateway (IGW):** Allow internet access
- **Route Tables:** Control traffic routing

#### NAT Gateway

- Allows private instances to access the internet
- Prevents inbound traffic from the internet
- Uses **SNAT and PAT mechanisms**

#### Security Layers

- **Security Group (SG):**
  - Works at instance level
  - Stateful
  - Allow-only rules

- **Network ACL (NACL):**
  - Works at subnet level
  - Stateless
  - Supports allow & deny rules

---

### 3. AWS Security Services

The workshop also introduced important AWS security services:

#### AWS WAF (Web Application Firewall)

- Protects applications from attacks like:
  - SQL Injection
  - Cross-Site Scripting (XSS)
- Filters HTTP/HTTPS requests

#### AWS Shield

- Protects against **DDoS attacks**
- Two versions:
  - Standard (free)
  - Advanced (enhanced protection)

#### AWS Network Firewall

- Provides network-level protection inside VPC
- Supports:
  - Stateful & stateless filtering
  - Intrusion prevention

#### AWS Firewall Manager

- Centralized security management
- Automatically applies security rules across multiple accounts

---

## Key Takeaways

From this workshop, I learned that:

- IAM is essential for managing secure access to AWS resources
- Networking components such as VPC, Subnet, and NAT Gateway form the foundation of cloud architecture
- Security must be implemented at multiple layers (application, network, and access control)
- AWS provides integrated services to protect systems from common threats
- Applying best practices can significantly improve system security and reliability

---

## Personal Experience

This event helped me gain a deeper understanding of how different components in AWS work together to build a secure system.

I found the IAM and networking sections particularly useful, as they are directly related to my current project when working with APIs, authentication, and cloud infrastructure.

Additionally, learning about AWS security services gave me more confidence in designing systems that can handle real-world security challenges.

---

## Conclusion

This workshop provided a solid foundation in AWS networking, security, and identity management.

It enhanced my ability to design secure, scalable, and efficient cloud-based systems and strengthened my overall understanding of AWS architecture.

---

## Event Photos

![Event](/event41.jpg)
![Event](/event42.jpg)
![Event](/event43.jpg)
![Event](/event44.jpg)
![Event](/event45.jpg)
