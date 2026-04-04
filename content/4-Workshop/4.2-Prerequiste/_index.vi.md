---
title: "Chuẩn bị"
date: 2026-03-26
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

Trước khi bắt đầu triển khai, hãy đảm bảo bạn đã chuẩn bị đầy đủ các công cụ và quyền truy cập sau.

## 🔑 Quyền truy cập cần thiết

### Tài khoản AWS

- Tài khoản AWS với quyền **AdministratorAccess** hoặc tương đương.
- Quyền truy cập vào **AWS Management Console** và **AWS CLI**.

> Ghi lại các giá trị sau — bạn sẽ dùng chúng xuyên suốt workshop:
>
> | Placeholder | Mô tả | Ví dụ |
> |---|---|---|
> | `ACCOUNT_ID` | AWS Account ID 12 chữ số | `123456789012` |
> | `REGION` | Region triển khai | `ap-southeast-1` |
> | `DEPLOY_BUCKET` | S3 bucket lưu Lambda artifact | `guardscript-deploy-ap-southeast-1` |

### Quyền IAM cần thiết

User hoặc role AWS của bạn cần có quyền tạo và quản lý:

- AWS Lambda, Lambda Function URLs
- Amazon DynamoDB (tables, GSIs)
- Amazon S3 (buckets, objects, policies)
- Amazon CloudFront (distributions, cache policies, functions, OAC)
- API Gateway V2 (WebSocket API, routes, stages)
- AWS CloudFormation / AWS SAM
- AWS CloudWatch (log groups, alarms, dashboards)
- AWS IAM (tạo roles và policies cho Lambda execution)
- AWS Certificate Manager (tùy chọn — cho custom domain)
- AWS WAF (tùy chọn)

> **Tip:** Trong môi trường workshop, cách đơn giản nhất là dùng `AdministratorAccess`.

---

## 🛠️ Công cụ cần cài đặt

Cài đặt tất cả công cụ trước khi tiếp tục.

### 1. Node.js 20.x

GuardScript chạy trên **Node.js 20.x** (LTS). Cần thiết để cài dependencies cục bộ.

```bash
node --version   # Phải ra v20.x.x
npm --version
```

Tải về: [https://nodejs.org](https://nodejs.org)

### 2. AWS CLI v2

Dùng để đồng bộ frontend lên S3, seed DynamoDB và quản lý tài nguyên.

```bash
aws --version    # Phải ra aws-cli/2.x.x
```

Tải về: [https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

Cấu hình với thông tin đăng nhập:

```bash
aws configure
# AWS Access Key ID:     <access-key>
# AWS Secret Access Key: <secret-key>
# Default region name:   ap-southeast-1
# Default output format: json
```

### 3. AWS SAM CLI

Dùng để build và deploy stack CloudFormation/SAM.

```bash
sam --version    # Phải ra SAM CLI, version 1.x.x
```

Tải về: [https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html)

### 4. Git

Dùng để clone repository.

```bash
git --version
```

---

## 📁 Cấu trúc repository

Clone repository và điều hướng vào thư mục `code_protector_aws`:

```bash
git clone <repository-url>
cd code_protector_aws
```

Các thư mục quan trọng:

```
code_protector_aws/
  src/          ← Lambda source code (Node.js)
  frontend/     ← Static frontend assets
  infra/
    template.yaml  ← AWS SAM / CloudFormation template
```

---

## ✅ Checklist trước khi bắt đầu

Trước khi chuyển sang Giai đoạn 1, kiểm tra:

- [ ] `node --version` ra `v20.x.x`
- [ ] `aws --version` ra `aws-cli/2.x.x`
- [ ] `sam --version` ra phiên bản hợp lệ
- [ ] `aws sts get-caller-identity` trả về account ID và ARN của bạn
- [ ] Repository đã clone và thư mục `code_protector_aws/` có thể truy cập
