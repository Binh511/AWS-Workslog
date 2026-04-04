---
title: "Tổng quan"
date: 2026-03-26
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

## Các thành phần hệ thống

**GuardScript** là nền tảng phân phối script serverless theo cơ chế loader kiểm soát truy cập. Thay vì chia sẻ source code trực tiếp, nội dung script được phục vụ qua các endpoint có kiểm soát với xác thực chữ ký, bảo vệ replay bằng timestamp/nonce, kiểm tra license, HWID binding và access policy theo workspace.

Nền tảng gồm năm lớp:

- **Client Layer**: Dashboard web (browser) và loader clients (Python, Node.js, Lua). Loader gọi endpoint execute hoặc handshake để lấy nội dung script đã mã hóa.
- **Edge Layer**: Amazon CloudFront xử lý SSL termination, cache static frontend từ S3, định tuyến `/api/*` và `/files/*` đến Lambda Function URL. CloudFront Function rewrite SPA routes và kiểm tra auth-cookie trước khi phục vụ trang protected.
- **Compute Layer**: Một Lambda function duy nhất (Node.js 20.x, modular monolith) xử lý toàn bộ API routes — xác thực, quản lý workspace, CRUD project/file, license, access lists, admin console và các loader protocols (v2/v3).
- **Data Layer**: Amazon DynamoDB lưu toàn bộ dữ liệu có cấu trúc trên 14 bảng PAY_PER_REQUEST. Amazon S3 lưu nội dung script đã mã hóa và static frontend.
- **Observability Layer**: Amazon CloudWatch cung cấp 3 alarm (Errors, Throttles, p95 Duration), dashboard vận hành và log group có thể cấu hình retention.

## Sơ đồ kiến trúc

![Kiến trúc hệ thống GuardScript](/images/2-Proposal/architecture.jpg)

## Luồng request

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

## Tóm tắt Loader Protocol

| Protocol | Endpoint | Mã hóa | Ứng dụng |
|---|---|---|---|
| v2 | `GET /api/v5/execute` | XOR + HMAC-SHA256 | Loader nhẹ |
| v3 | `POST /api/v5/handshake` | ECDH X25519 + AES-256-GCM | Loader bảo mật cao |

Cả hai protocol đều yêu cầu `license` key hợp lệ, `HWID`, `timestamp` (trong khoảng ±300s) và `nonce` để chống replay.

## Kết quả sau workshop

Sau khi hoàn thành workshop, bạn sẽ có:

1. GuardScript backend đầy đủ trên Lambda với 14 bảng DynamoDB.
2. CloudFront distribution phục vụ frontend từ S3, định tuyến API traffic đến Lambda.
3. WebSocket endpoint cho sự kiện dashboard thời gian thực.
4. CloudWatch alarms và dashboard vận hành được cấu hình sẵn.
5. Tài khoản admin đã khởi tạo và nền tảng sẵn sàng sử dụng.
