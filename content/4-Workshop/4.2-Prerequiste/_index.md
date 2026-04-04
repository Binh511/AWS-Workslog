---
title: "Prerequisites"
date: 2026-03-26
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

Before starting the deployment, ensure you have the following tools and access ready.

## 🔑 Required Access

### AWS Account

- An AWS account with **AdministratorAccess** or equivalent permissions.
- Access to the **AWS Management Console** and the **AWS CLI**.

> Record these values — you will use them throughout the workshop:
>
> | Placeholder | Description | Example |
> |---|---|---|
> | `ACCOUNT_ID` | Your 12-digit AWS Account ID | `123456789012` |
> | `REGION` | Deployment region | `ap-southeast-1` |
> | `DEPLOY_BUCKET` | S3 bucket for Lambda deployment artifact | `guardscript-deploy-ap-southeast-1` |

### Required IAM Permissions

Your AWS user or role must have permissions to create and manage:

- AWS Lambda, Lambda Function URLs
- Amazon DynamoDB (tables, GSIs)
- Amazon S3 (buckets, objects, policies)
- Amazon CloudFront (distributions, cache policies, functions, OAC)
- API Gateway V2 (WebSocket API, routes, stages)
- AWS CloudFormation / AWS SAM
- AWS CloudWatch (log groups, alarms, dashboards)
- AWS IAM (create roles and policies for Lambda execution)
- AWS Certificate Manager (optional — for custom domains)
- AWS WAF (optional)

> **Tip:** The simplest approach for a workshop environment is `AdministratorAccess`.

---

## 🛠️ Required Tools

Install all tools before proceeding.

### 1. Node.js 20.x

GuardScript runs on **Node.js 20.x** (LTS). Required for installing dependencies locally.

```bash
node --version   # Should output v20.x.x
npm --version
```

Download: [https://nodejs.org](https://nodejs.org)

### 2. AWS CLI v2

Required for syncing frontend to S3, seeding DynamoDB, and managing resources.

```bash
aws --version    # Should output aws-cli/2.x.x
```

Download: [https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

Configure with your credentials:

```bash
aws configure
# AWS Access Key ID:     <your-access-key>
# AWS Secret Access Key: <your-secret-key>
# Default region name:   ap-southeast-1
# Default output format: json
```

### 3. AWS SAM CLI

Required for building and deploying the CloudFormation/SAM infrastructure stack.

```bash
sam --version    # Should output SAM CLI, version 1.x.x
```

Download: [https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html)

### 4. Git

Required for cloning the repository.

```bash
git --version
```

---

## 📁 Repository Structure

Clone the project repository, then navigate to the `code_protector_aws` folder:

```bash
git clone <repository-url>
cd code_protector_aws
```

Key directories:

```
code_protector_aws/
  src/          ← Lambda source code (Node.js)
  frontend/     ← Static frontend assets
  infra/
    template.yaml  ← AWS SAM / CloudFormation template
```

---

## ✅ Pre-flight Checklist

Before proceeding to Phase 1, verify:

- [ ] `node --version` outputs `v20.x.x`
- [ ] `aws --version` outputs `aws-cli/2.x.x`
- [ ] `sam --version` outputs a valid version
- [ ] `aws sts get-caller-identity` returns your account ID and ARN
- [ ] Repository cloned and `code_protector_aws/` folder is accessible
