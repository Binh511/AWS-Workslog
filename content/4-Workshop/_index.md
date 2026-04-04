---
title: "Workshop"
date: 2026-03-26
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

# GuardScript — Serverless Script Protection Platform

## Deploying GuardScript on AWS

**GuardScript** is a script distribution platform with loader-based access control, built on a fully serverless AWS architecture. In this workshop, you will deploy the complete platform — from source code to a live, operational system — using AWS SAM and the AWS CLI.

#### What you will deploy

- **AWS Lambda** (Node.js 20.x) — Modular monolith API backend
- **Amazon DynamoDB** — 14 tables (PAY_PER_REQUEST) for all platform data
- **Amazon S3** — Frontend hosting + encrypted script content storage
- **Amazon CloudFront** — CDN with edge routing and SPA URL rewriting
- **API Gateway WebSocket** — Real-time event broadcasting to clients
- **Amazon CloudWatch** — Alarms, operational dashboard, and structured logs
- **AWS WAF** — Edge request protection (optional)

#### Architecture

```
Client (browser / loader)
  → CloudFront Distribution (SSL termination, cache, SPA rewrite)
    → Static assets: S3 Bucket (frontend)
    → /api/*, /files/*: Lambda Function URL (API backend)
      → DynamoDB (14 tables)
      → S3 (script content objects)
  → API Gateway WebSocket API (real-time events)
  → CloudWatch Logs / Alarms / Dashboard
```

![GuardScript Architecture](/images/2-Proposal/architecture.jpg)

#### Content

1. [Overview](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequiste/)
3. [Phase 1: Prepare Lambda Artifacts](5.3-Prepare-Lambda/)
4. [Phase 2: Deploy AWS Infrastructure](5.4-Deploy-Infrastructure/)
5. [Phase 3: Deploy Frontend](5.5-Deploy-Frontend/)
6. [Phase 4: Configure & Validate](5.6-Configure-Validate/)
7. [Cleanup](5.7-Cleanup/)
