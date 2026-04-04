---
title: "Workshop"
date: 2026-03-26
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# GuardScript — Nền tảng bảo vệ mã nguồn serverless

## Triển khai GuardScript trên AWS

**GuardScript** là nền tảng phân phối script theo cơ chế loader kiểm soát truy cập, xây dựng trên kiến trúc serverless hoàn toàn trên AWS. Trong workshop này, bạn sẽ triển khai toàn bộ nền tảng — từ source code đến hệ thống hoạt động thực tế — bằng AWS SAM và AWS CLI.

#### Những gì bạn sẽ triển khai

- **AWS Lambda** (Node.js 20.x) — API backend theo mô hình modular monolith
- **Amazon DynamoDB** — 14 bảng (PAY_PER_REQUEST) cho toàn bộ dữ liệu nền tảng
- **Amazon S3** — Lưu trữ frontend và nội dung script được mã hóa
- **Amazon CloudFront** — CDN với định tuyến edge và SPA URL rewriting
- **API Gateway WebSocket** — Phát sự kiện thời gian thực đến client
- **Amazon CloudWatch** — Alarms, dashboard vận hành và logs cấu trúc
- **AWS WAF** — Bảo vệ request tại edge (tùy chọn)

#### Kiến trúc

```
Client (browser / loader)
  → CloudFront Distribution (SSL, cache, SPA rewrite)
    → Static assets: S3 Bucket (frontend)
    → /api/*, /files/*: Lambda Function URL (API backend)
      → DynamoDB (14 bảng)
      → S3 (nội dung script)
  → API Gateway WebSocket API (sự kiện thời gian thực)
  → CloudWatch Logs / Alarms / Dashboard
```

![Kiến trúc hệ thống GuardScript](/images/2-Proposal/architecture.jpg)

#### Nội dung

1. [Tổng quan](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Giai đoạn 1: Chuẩn bị Lambda Artifacts](5.3-Prepare-Lambda/)
4. [Giai đoạn 2: Triển khai hạ tầng AWS](5.4-Deploy-Infrastructure/)
5. [Giai đoạn 3: Triển khai Frontend](5.5-Deploy-Frontend/)
6. [Giai đoạn 4: Cấu hình & Kiểm tra](5.6-Configure-Validate/)
7. [Dọn dẹp tài nguyên](5.7-Cleanup/)
