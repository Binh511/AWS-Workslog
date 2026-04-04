---
title: "Proposal"
date: 2026-03-26
weight: 2
chapter: false
pre: " <b> 2. </b> "
--------------------

# GuardScript — Code Protector Platform

## Project Proposal

**Team Members:**

| Full Name         | Student ID | Role        |
| ----------------- | ---------- | ----------- |
| Võ Tấn Phát       | SE194484   | Team Leader |
| Bùi Minh Hiển     | SE190829   | Member      |
| Dương Nguyên Bình | SE194067   | Member      |
| Trần Vinh         | SE193927   | Member      |
| Nguyễn Duy Tùng   | SE196572   | Member      |
| Nguyễn Đức Trí    | SE194091   | Member      |

---

### 1. Executive Summary

**GuardScript** is a script distribution platform with loader-based access control. Instead of distributing source code directly, the system serves script content through controlled endpoints with signature checks, timestamp/nonce validation, license enforcement, HWID binding, and workspace access policies.

The solution uses a serverless AWS architecture to reduce operational overhead, support scale, and satisfy cloud workshop objectives.

---

### 2. Problem Statement

#### Current Problems

The team identified three key issues in script delivery workflows:

1. Source code can be easily leaked or copied when shared manually
2. Lack of fine-grained access control for users or teams
3. Difficulty in tracking versions, access, and usage history
4. Absence of centralized management leading to fragmented data
5. Limited auditing, monitoring, and protection mechanisms
6. Deployment and security systems are often complex and costly
 
#### Solution

The proposed system addresses these issues through:

1. Loader-based script retrieval instead of direct source distribution.
2. Dual transfer protocols: v2 (XOR + response verification) and v3 (X25519 ECDH + AES-GCM).
3. License/HWID controls with access list policies.
4. Cloud-native observability using CloudWatch and application logs.

#### Benefits and ROI

1. Stronger control over script usage and distribution.
2. Better protection against API abuse via signed requests and rate limits.
3. Cost-efficient operation for student/demo workloads with pay-per-use services.

---

### 3. Solution Architecture

The architecture follows a serverless model with edge delivery for frontend and Lambda-based API processing.

**Typical request flow:**

```
Client (browser / loader)
  → CloudFront Distribution (SSL termination, cache layer)
    → Static assets: S3 bucket (frontend)
    → API /api/*, /files/*: Lambda Function URL origin
      → DynamoDB (users, workspaces, projects, licenses, logs, rate_limits, ...)
      → S3 (script/content objects)
  → API Gateway WebSocket (real-time updates)
  → CloudWatch Logs / Alarms / Dashboard
```
![GuardScript System Architecture](/images/2-Proposal/architecture.jpg)

#### AWS Services Used

| Layer          | Service                   | Details                                                                       |
| -------------- | ------------------------- | ----------------------------------------------------------------------------- |
| Runtime API    | AWS Lambda (Node.js 20.x) | Modular monolith backend handlers |
| Database       | Amazon DynamoDB           | Multi-table model, PAY_PER_REQUEST, TTL for temporary records |
| Object Storage | Amazon S3                 | Frontend hosting and content objects |
| CDN & Edge     | Amazon CloudFront         | Static delivery and route behaviors for `/api/*`, `/files/*` |
| Edge Security  | AWS WAF                   | Protects against malicious web requests at CloudFront edge |
| TLS/SSL        | AWS Certificate Manager   | Manages SSL/TLS certificates for HTTPS |
| Notification   | Amazon SNS / SES          | Sends alerts and email notifications (invitations, alarms) |
| Monitoring     | Amazon CloudWatch         | Alarms for errors/throttles/p95 duration + operational dashboard |
| Real-time      | API Gateway WebSocket API | Workspace/user/admin event broadcasting |
| Delivery       | GitHub Actions + SAM      | Automated infrastructure and frontend deployment |

#### Component Design

1. **Client Layer**: web dashboard and loader clients (Python/Node/Lua).
2. **Edge Layer**: CloudFront routing and caching.
3. **Compute Layer**: Lambda handlers for auth, workspace, project, license, and loader protocols.
4. **Data Layer**: DynamoDB + S3.
5. **Observability Layer**: CloudWatch metrics/alarms/dashboard and application logs.

#### Monitoring & Observability

1. CloudWatch alarms are configured for `Errors`, `Throttles`, and `p95 Duration`.
2. A CloudWatch dashboard is provisioned for runtime visibility.
3. Application-level logs are stored by workspace for auditability.
4. Validation checklist includes auth, protocol, access-list, and alarm tests.

---

### 4. Technical Implementation

#### 4.1. Technical Requirements

**Functional Requirements:**
1. User registration, login, and session-based authentication.
2. Workspace creation and management with role-based access control (owner, admin, member).
3. Script project management: CRUD for projects, files, and bundled content.
4. Script distribution via controlled loader endpoints (Protocol v2 and v3).
5. License key management with HWID binding, expiration, and usage tracking.
6. IP access control per workspace (blacklist and whitelist policies).
7. Team collaboration via workspace invitations and membership management.
8. Audit logging for all critical user and admin actions.
9. Real-time event broadcasting via WebSocket API.
10. Admin console for platform-level oversight and user management.

**Non-Functional Requirements:**

| Category        | Requirement                                                                          |
| --------------- | ------------------------------------------------------------------------------------ |
| Security        | HMAC-signed requests, ECDH key exchange, replay protection via nonce + timestamp     |
| Performance     | p95 API response < 500ms; CloudWatch alarms for latency breaches                     |
| Scalability     | Serverless Lambda + DynamoDB on-demand auto-scales with load                         |
| Availability    | CloudFront edge delivery; S3 high-durability object storage (99.999999999%)          |
| Maintainability | Modular Lambda monolith; SAM/CloudFormation IaC for reproducible infrastructure      |
| Observability   | CloudWatch metrics, alarms, and dashboard; structured per-workspace application logs |

**Technical Stack:**

| Component  | Technology                                  |
| ---------- | ------------------------------------------- |
| Runtime    | Node.js 20.x on AWS Lambda                  |
| Database   | Amazon DynamoDB (PAY_PER_REQUEST)           |
| Storage    | Amazon S3                                   |
| Edge / CDN | Amazon CloudFront + AWS WAF                 |
| Real-time  | API Gateway WebSocket API                   |
| Auth       | Custom HMAC-SHA256 token system             |
| Encryption | AES-256-GCM, ECDH X25519, PBKDF2-SHA256     |
| IaC        | AWS SAM / CloudFormation                    |
| CI/CD      | GitHub Actions                              |
| Monitoring | Amazon CloudWatch (Logs, Alarms, Dashboard) |

---

#### 4.2. Implementation Phases

The project follows **Agile Scrum** methodology with 6 sprints (1 week each):

**Sprint 1 — Analysis & Architecture Design**
- Define system requirements
- Design AWS architecture
- Design database schema and APIs

**Sprint 2 — Backend Foundation**
- Set up Lambda project structure
- Implement Auth, User, Workspace APIs
- Integrate DynamoDB

**Sprint 3 — Script & File Management**
- CRUD for project, file, and content resources
- S3 integration for upload/download
- File tree structure and bundling

**Sprint 4 — Security & Access Control**
- Implement access control and licensing
- Integrate script protection loader (Protocol v2/v3)
- Build audit logging

**Sprint 5 — Frontend & Realtime**
- Develop dashboard UI
- Integrate APIs
- Configure WebSocket for real-time updates

**Sprint 6 — CI/CD & Deployment**
- Configure GitHub Actions pipeline
- Deploy via AWS SAM/CloudFormation
- Set up CloudWatch monitoring and alarms

---

### 5. Roadmap & Milestones

| Phase                                   | Content                                                                                      | Timeline    |
| --------------------------------------- | -------------------------------------------------------------------------------------------- | ----------- |
| 1. Analysis & Architecture Design       | AWS onboarding, GuardScript scope definition, architecture design, DynamoDB schema, API planning | Week 1–6 |
| 2. Backend Foundation                   | Project kickoff, auth, workspace, project, file, and license APIs                           | Week 7–8    |
| 3. Script, File & AWS Migration         | Encryption module, S3 integration, Lambda/DynamoDB/CloudFront setup, loader protocols       | Week 8–9    |
| 4. Security & Access Control            | Loader v2/v3 hardening, HWID lock, access-list policies, rate-limit validation               | Week 9–11   |
| 5. Frontend & Realtime                  | Dashboard/workspace UI, WebSocket integration, responsive design, overall testing            | Week 7–10   |
| 6. CI/CD & Documentation                | SAM/CloudFormation deployment, CloudWatch monitoring, validation checklist, final reporting  | Week 11–12  |

---

### 6. Cost Estimation

Typical monthly infrastructure cost (Free Tier / Small Scale): **~$4.32/month**

| Service               | Estimated Cost     | Notes                                      |
| --------------------- | ------------------ | ------------------------------------------ |
| AWS Lambda            | ~$0.00/month       | Free tier: 1M requests/month               |
| Amazon DynamoDB       | ~$0.60/month       | On-demand; 25 GB free storage              |
| Amazon S3             | ~$0.80/month       | Frontend hosting + content storage         |
| Amazon CloudFront     | ~$0.77/month       | 1 TB free transfer (first year)            |
| Amazon CloudWatch     | ~$0.50/month       | 30-day log retention, alarms, dashboard    |
| API Gateway WebSocket | ~$0.35/month       | WebSocket connections                      |
| Amazon SES            | ~$0.09/month       | Email notifications and invitations        |
| Amazon SNS            | ~$0.00/month       | Alert notifications (mostly free tier)     |
| AWS WAF               | ~$0.21/month       | Edge protection rules                      |
| AWS ACM               | ~$0.00/month       | SSL/TLS certificates (free for CloudFront) |
| AWS IAM               | ~$0.00/month       | No direct cost                             |
| **Total**             | **~$4.32/month**   | Usage-based, serverless pay-per-use        |

> Lambda and DynamoDB are mostly covered by the free tier at low usage levels.

---

### 7. Risk Assessment

| Risk                      | Impact | Probability | Mitigation                             |
| ------------------------- | ------ | ----------- | -------------------------------------- |
| Lambda cold start latency | Medium | Medium      | Optimize handlers and monitor p95      |
| DynamoDB throttling       | High   | Low         | On-demand scaling, CloudWatch alerts   |
| S3 PUT/GET failures       | High   | Low         | Retry logic, versioning enabled        |
| Budget overrun            | Medium | Low         | AWS Budgets alerts, optimize TTL/cache |
| Replay attacks            | High   | Low         | Timestamp + nonce + HMAC required      |

**Contingency Plan:**

* If primary endpoint behavior fails → fallback to Lambda Function URL.
* If S3 is unavailable → retry with exponential backoff, log to CloudWatch.
* Use SAM/CloudFormation stacks for quick infrastructure recovery.

---

### 8. Expected Outcomes

**Technical Outcomes:**

* End-to-end AWS serverless deployment with validated API and loader flow.
* Dual loader protocol support with security controls (signature, HWID, rate-limit).
* Baseline observability for runtime health and performance.
* Reproducible deployment process aligned with workshop requirements.

**Long-term Value:**

* Extendable architecture for additional loaders and governance features.
* Reusable cloud-native reference for future FCJ workshop projects.

---


### 9. Future Improvements

1. Add production-ready SES email workflow for invitations.
2. Add AWS WAF managed rules for edge protection.
3. Upgrade S3 encryption controls from SSE-S3 to SSE-KMS CMK where needed.
4. Add AWS Budgets and Cost Anomaly Detection.
5. Increase automated test coverage (unit/integration/security checks). 