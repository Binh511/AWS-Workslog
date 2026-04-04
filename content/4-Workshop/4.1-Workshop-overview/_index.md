---
title: "Overview"
date: 2026-03-26
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

## System Components

**GuardScript** is a serverless script distribution platform with loader-based access control. Instead of sharing source code directly, script content is served through controlled endpoints with signature verification, timestamp/nonce replay protection, license enforcement, HWID binding, and workspace access policies.

The platform is composed of five layers:

- **Client Layer**: Web dashboard (browser) and loader clients (Python, Node.js, Lua). Loaders call the execute or handshake endpoints to retrieve encrypted script content.
- **Edge Layer**: Amazon CloudFront handles SSL termination, caches static frontend assets from S3, and routes `/api/*` and `/files/*` requests to the Lambda Function URL. A CloudFront Function rewrites SPA routes and enforces auth-cookie checks before serving protected pages.
- **Compute Layer**: A single AWS Lambda function (Node.js 20.x, modular monolith) handles all API routes — authentication, workspace management, project/file CRUD, license management, access lists, admin console, and loader protocols (v2/v3).
- **Data Layer**: Amazon DynamoDB stores all structured data across 14 PAY_PER_REQUEST tables. Amazon S3 stores encrypted script content objects and frontend static assets.
- **Observability Layer**: Amazon CloudWatch provides three alarms (Errors, Throttles, p95 Duration), an operational dashboard, and log group with configurable retention.

## Architecture Diagram

![GuardScript System Architecture](/images/2-Proposal/architecture.jpg)

## Request Flow

```
Client (browser / loader)
  → CloudFront Distribution
      → [Static] S3 Frontend Bucket (HTML, CSS, JS)
      → [/api/*, /files/*] Lambda Function URL
            → DynamoDB (users, workspaces, projects, files, licenses, ...)
            → S3 Content Bucket (script objects)
  → API Gateway WebSocket API
        → Lambda (→ DynamoDB WebSocket connections table)
  → CloudWatch Logs / Alarms / Dashboard
```

## Loader Protocol Summary

| Protocol | Endpoint | Encryption | Use Case |
|---|---|---|---|
| v2 | `GET /api/v5/execute` | XOR + HMAC-SHA256 signature | Lightweight loaders |
| v3 | `POST /api/v5/handshake` | ECDH X25519 + AES-256-GCM | High-security loaders |

Both protocols require a valid `license` key, `HWID`, `timestamp` (within ±300s), and a `nonce` to prevent replay.

## What You Will Build

By the end of this workshop you will have:

1. A fully deployed GuardScript backend on Lambda with 14 DynamoDB tables.
2. A CloudFront distribution serving the frontend from S3, routing API traffic to Lambda.
3. A WebSocket endpoint for real-time dashboard events.
4. CloudWatch alarms and an operational dashboard configured.
5. An initialized admin account and the platform ready to use.
