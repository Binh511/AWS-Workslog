---
title: "Proposal"
date: 2026-03-26
weight: 2
chapter: false
pre: " <b> 2. </b> "
--------------------

In this section, you need to summarize the workshop contents that you **plan** to implement.

# Code Protector – IrisAuth

## A Multi-language Secure Code Protection and Distribution Platform on AWS Cloud

**Team Members:**

| Full Name         | Student ID | Role        |
| ----------------- | ---------- | ----------- |
| Võ Tấn Phát       | SE194484   | Team Leader |
| Bùi Minh Hiển     | SE190829   | Member      |
| Dương Nguyên Bình | SE194067   | Member      |
| Trần Vinh         | SE193927   | Member      |
| Nguyễn Duy Tùng   | SE196572   | Member      |
| Nguyễn Đức Trí    | SE194091   | Member      |

**AWS Services:** `Lambda` · `S3` · `DynamoDB` · `CloudFront` · `CloudWatch` · `SES`

---

### 1. Executive Summary

**Code Protector** (system name: **IrisAuth**) is a SaaS platform that allows developers to upload source code to a server and distribute it to end users through an encrypted loader mechanism. Instead of sharing source code directly, end users only receive a small 1–2 line loader. The server returns encrypted code only after the user passes all security checks, and the code exists only in RAM during execution — never written to disk.

The system is deployed on **AWS Cloud**, leveraging a fully serverless architecture and managed services to ensure high scalability, low global latency, and zero infrastructure management overhead.

---

### 2. Problem Statement

#### Current Problems

In modern software development, protecting source code is a critical challenge. When developers distribute Python or JavaScript scripts to end users, they face risks of code being copied, modified, or redistributed without proper control mechanisms.

Existing solutions such as simple obfuscation or packaging into `.exe` files have significant limitations: they are vulnerable to reverse engineering, cannot control who executes the code, and cannot revoke access when needed. The market lacks an integrated solution that both protects source code and manages distribution and access control comprehensively.

#### Solution

IrisAuth addresses these issues through:

* **Loader-based distribution**: End users only receive a 1–2 line loader and never access plaintext source code.
* **Dual encryption protocol**: Protocol v2 (XOR + SHA-256) and v3 (ECDH X25519 + AES-256-GCM) with perfect forward secrecy.
* **HWID Lock**: License binding to hardware to prevent account sharing.
* **AWS Serverless**: Auto-scaling, zero downtime, and pay-per-use cost model.

#### Benefits and ROI

* Comprehensive protection for Python and JavaScript source code against reverse engineering.
* Real-time access control: instantly enable/disable licenses without client updates.
* Full monitoring via CloudWatch: track who runs the code, where, and how often.
* Low infrastructure cost thanks to serverless architecture — pay only for actual usage.

---

### 3. Solution Architecture

The IrisAuth system is built using a **fully serverless architecture** on AWS, eliminating the need for managing physical servers.

**Typical request flow:**

```
Client (browser / loader)
  → CloudFront Distribution (SSL termination, cache layer)
    → Static assets: S3 bucket (HTML/CSS/JS)
    → API /api/*: API Gateway → AWS Lambda
      → DynamoDB (metadata: users, licenses, logs)
      → S3 (encrypted + gzip script content)
      → Amazon SES (email invitation)
  → CloudWatch Logs (auto stream from Lambda)
  → API Gateway WebSocket API (real-time sync)
```

#### AWS Services Used

| Layer          | Service                   | Details                                                                       |
| -------------- | ------------------------- | ----------------------------------------------------------------------------- |
| Runtime API    | AWS Lambda (Node.js v18+) | ES Module, Express.js wrapped with aws-serverless-express, cold start <1s     |
| Database       | Amazon DynamoDB           | NoSQL key-value, on-demand capacity, automatic TTL for rate_limits & sessions |
| Object Storage | Amazon S3                 | Stores encrypted + gzip scripts (key = UUID), versioning enabled, SSE-KMS     |
| CDN & Edge     | Amazon CloudFront         | Distributes static frontend, SSL termination, S3 caching, integrated WAF      |
| Email          | Amazon SES                | Sends workspace invitations, DKIM + SPF configured, sandbox → production      |
| Monitoring     | Amazon CloudWatch         | Log groups per Lambda, metric alarms (error rate, latency), dashboards        |
| Real-time      | API Gateway WebSocket API | Broadcast per-workspace updates                                               |
| Frontend       | HTML5, CSS3, Vanilla JS   | 5 static pages deployed on S3 and served via CloudFront                       |

#### Component Design

* **Client Layer**: Browser dashboard and loader clients (Python 3.7+, Node.js v14+, Tampermonkey userscript).
* **Edge Layer**: CloudFront Distribution + AWS WAF — SSL termination, static caching, bot protection at edge.
* **Compute Layer**: AWS Lambda — handles all business logic, auto-scales from 0 to ∞.
* **Data Layer**: DynamoDB (metadata, licenses, logs) + S3 (script content).
* **Notification Layer**: Amazon SES (email) + Discord Webhook (alerting).
* **Observability Layer**: CloudWatch Logs, Metrics, Alarms, Dashboard.

---

### 4. Technical Implementation

#### 4.1. Authentication System (Custom-built, no JWT libraries)

Token format: `v2.<payload_base64url>.<signature_base64url>`

* Tokens are signed using **HMAC-SHA256**, with a 48-byte random secret stored in DynamoDB (`app_config` table).
* Token TTL: 7 days. When a password changes, all old tokens are instantly invalidated (`iat` vs `password_changed_at`).
* Passwords are hashed using **PBKDF2-SHA256 with 210,000 iterations**, with a 16-byte random salt — OWASP compliant.
* Workspace PINs also use PBKDF2, generating a session token stored in DynamoDB with automatic TTL.

#### 4.2. Two Code Distribution Protocols

**Protocol v2 — GET /api/v5/execute (XOR)**

* Client sends: `id`, `license key`, `HWID`, `timestamp`, `nonce`, `HMAC-SHA256 signature`.
* Server verifies signature and timestamp (±5 minutes to prevent replay attacks).
* Response is encrypted using XOR with key = `SHA256(derivedSecret:hwid:nonce:id)`.
* Script content is retrieved from Amazon S3.

**Protocol v3 — POST /api/v5/handshake (ECDH + AES-GCM)**

* Client generates an **X25519 key pair**, sends public key with signature.
* Lambda computes shared secret using `diffieHellman()`, derives AES key via **HKDF-SHA256** (RFC 5869).
* Script is encrypted using **AES-256-GCM** — each session has a unique key (perfect forward secrecy).

#### 4.3. Security Algorithms and Standards

| Algorithm                          | Usage                                                 |
| ---------------------------------- | ----------------------------------------------------- |
| PBKDF2-SHA256 (210,000 iterations) | Password hashing, Workspace PIN hashing               |
| HMAC-SHA256                        | Token signing, request signing, response signing      |
| ECDH X25519                        | Key exchange in Protocol v3                           |
| HKDF-SHA256                        | AES key derivation from ECDH shared secret (RFC 5869) |
| AES-256-GCM                        | Script encryption (S3 + transmission in Protocol v3)  |
| XOR + SHA-256                      | Transmission encryption (Protocol v2)                 |
| Gzip (Pako)                        | Compress content before storing in S3                 |
| `crypto.timingSafeEqual`           | Secure comparison to prevent timing attacks           |
| Nonce + Timestamp ±5 minutes       | Replay attack protection                              |
| S3 SSE-KMS                         | At-rest encryption for all S3 objects                 |
| CloudFront + ACM                   | Enforce TLS 1.2+ for all connections                  |

#### 4.4. Database — Amazon DynamoDB

| Table                   | Partition Key / Sort Key                | Purpose                                                |
| ----------------------- | --------------------------------------- | ------------------------------------------------------ |
| `users`                 | PK: `userId`                            | User accounts, roles, `password_changed_at`            |
| `workspaces`            | PK: `workspaceId`                       | Workspace config, loader_key, encryption_key, PIN hash |
| `projects`              | PK: `workspaceId`, SK: `projectId`      | Project settings, execution count                      |
| `project_files`         | PK: `projectId`, SK: `fileId`           | Project file structure, entry point, ordering          |
| `licenses`              | PK: `workspaceId`, SK: `licenseKey`     | License info, HWID, expiration, usage count            |
| `access_lists`          | PK: `workspaceId`, SK: `ip#type`        | IP blacklist/whitelist                                 |
| `workspace_members`     | PK: `workspaceId`, SK: `userId`         | Workspace members and roles                            |
| `workspace_invitations` | PK: `token`                             | Email invitation tokens (SES), **TTL enabled**         |
| `pin_verifications`     | PK: `sessionToken`                      | PIN verification sessions, **TTL enabled**             |
| `logs`                  | PK: `workspaceId`, SK: `timestamp#uuid` | Event logs, GSI on `country`, `timestamp`              |
| `app_config`            | PK: `configKey`                         | System configs (HMAC secret, loader secret…)           |
| `rate_limits`           | PK: `rateLimitKey`                      | Sliding window rate limiting with **TTL**              |

> **Design Note**: TTL is enabled on `workspace_invitations`, `pin_verifications`, and `rate_limits` to automatically delete expired records without cron jobs. GSI on `country` and `timestamp` supports analytics queries.

---

### 5. Roadmap & Milestones

| Phase                 | Content                                                                       | Timeline   |
| --------------------- | ----------------------------------------------------------------------------- | ---------- |
| 1. Analysis & Design  | Requirements, DynamoDB schema, API design, CloudFront behavior rules          | Week 1–2   |
| 2. AWS Infrastructure | Provision Lambda, DynamoDB, S3, CloudFront, SES, CloudWatch (IaC via SAM/CDK) | Week 3     |
| 3. Backend Core       | Auth, Workspace, Project APIs — migrate SQLite → DynamoDB, filesystem → S3    | Week 4–6   |
| 4. Security & Loader  | ECDH handshake, AES encryption, Python/JS loader via CloudFront               | Week 7–8   |
| 5. License & Access   | License system, HWID, rate limiting, blacklist, whitelist                     | Week 9     |
| 6. Frontend & Email   | UI pages, WebSocket integration, SES invitations                              | Week 10–11 |
| 7. Obfuscator         | Python AST obfuscator, bundle generation                                      | Week 12    |
| 8. Testing & Deploy   | Unit, integration, load testing, CloudWatch tuning, production deploy         | Week 13–14 |

---

### 6. Cost Estimation

| Service           | Estimated Cost     | Notes                           |
| ----------------- | ------------------ | ------------------------------- |
| AWS Lambda        | ~$0.00/month       | Free tier: 1M requests/month    |
| Amazon DynamoDB   | ~$0.00–$1.00/month | On-demand, 25 GB free storage   |
| Amazon S3         | ~$0.10–$0.50/month | Script + static storage         |
| Amazon CloudFront | ~$0.00–$1.00/month | 1 TB free transfer (first year) |
| Amazon SES        | ~$0.10/month       | $0.10 per 1,000 emails          |
| Amazon CloudWatch | ~$0.00–$0.50/month | 30-day log retention            |
| API Gateway       | ~$0.01–$0.10/month | REST + WebSocket                |
| **Total**         | **~$1–3/month**    | Usage-based                     |

> Costs scale linearly with usage — ideal for development and demo phases. See details at AWS Pricing Calculator.

---

### 7. Risk Assessment

| Risk                      | Impact | Probability | Mitigation                             |
| ------------------------- | ------ | ----------- | -------------------------------------- |
| Lambda cold start latency | Medium | Medium      | Use Provisioned Concurrency            |
| DynamoDB throttling       | High   | Low         | On-demand scaling, CloudWatch alerts   |
| S3 PUT/GET failures       | High   | Low         | Retry logic, versioning enabled        |
| SES sandbox limitations   | Medium | High        | Request production access early        |
| Budget overrun            | Medium | Low         | AWS Budgets alerts, optimize TTL/cache |
| Replay attacks            | High   | Low         | Timestamp + nonce + HMAC required      |

**Contingency Plan:**

* If API Gateway fails → fallback to Lambda Function URL.
* If S3 is unavailable → retry with exponential backoff, log to CloudWatch.
* Use **AWS CloudFormation / CDK** to quickly recreate infrastructure.

---

### 8. Expected Outcomes

**Technical Outcomes:**

* Fully functional web platform deployed on AWS serverless architecture.
* Secure code protection system with dual encryption protocols (v2 XOR, v3 ECDH/AES-GCM).
* Loader clients for Python, Node.js, and Tampermonkey via CloudFront.
* Automated email invitations via Amazon SES.
* Full monitoring and alerting via CloudWatch.

**Long-term Value:**

* Expandable to support more programming languages.
* Reusable AWS cloud-native architecture for future projects.
* Complete technical documentation: architecture, API specs, deployment guides via SAM/CDK.
