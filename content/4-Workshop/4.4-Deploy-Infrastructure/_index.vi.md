---
title: "Giai đoạn 2: Triển khai hạ tầng AWS"
date: 2026-03-26
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

Trong giai đoạn này bạn sẽ dùng AWS SAM để build và deploy toàn bộ infrastructure stack của GuardScript, bao gồm Lambda, các bảng DynamoDB, S3 buckets, CloudFront, WebSocket API và CloudWatch.

## Bước 1: Di chuyển vào thư mục infra

```bash
cd code_protector_aws/infra
```

SAM template nằm tại `infra/template.yaml`.

---

## Bước 2: Build SAM Application

```bash
sam build --template-file template.yaml
```

SAM validate template và chuẩn bị build artifacts. Build thành công sẽ hiển thị:

```
Build Succeeded
```

---

## Bước 3: Deploy Stack

Chạy guided deploy (lần đầu tiên). SAM sẽ hỏi từng parameter:

```bash
sam deploy --guided
```

Trả lời các câu hỏi như sau:

| Câu hỏi | Giá trị |
|---|---|
| Stack Name | `guardscript-prod` |
| AWS Region | `REGION` (ví dụ: `ap-southeast-1`) |
| Parameter `ProjectName` | `code-protector-aws` |
| Parameter `Stage` | `prod` |
| Parameter `FrontendBucketName` | *(để trống để tạo tự động)* |
| Parameter `ContentBucketName` | *(để trống để tạo tự động)* |
| Parameter `LambdaCodeS3Bucket` | `DEPLOY_BUCKET` |
| Parameter `LambdaCodeS3Key` | `lambda.zip` |
| Parameter `LambdaMemorySize` | `1024` |
| Parameter `LambdaTimeout` | `30` |
| Parameter `LambdaLogRetentionDays` | `30` |
| Parameter `ManageApiLogGroup` | `true` |
| Parameter `EnableCloudWatchAlarms` | `true` |
| Confirm changes before deploy | `Y` |
| Allow SAM CLI IAM role creation | `Y` |
| Save arguments to samconfig.toml | `Y` |

SAM sẽ hiển thị **Change Set** preview trước khi tạo tài nguyên. Review và xác nhận.

> **Lưu ý:** Quá trình deploy thường mất 3–8 phút. CloudFront distribution mất thời gian lâu nhất để provision.

---

## Bước 4: Ghi lại Stack Outputs

Sau khi deploy thành công, SAM in ra các stack outputs. Ghi lại các giá trị này:

| Output Key | Mô tả | Ví dụ |
|---|---|---|
| `DistributionDomainName` | CloudFront domain (URL app của bạn) | `d1abc123.cloudfront.net` |
| `ApiFunctionUrl` | Lambda Function URL | `https://xxx.lambda-url.ap-southeast-1.on.aws/` |
| `WebSocketEndpoint` | WebSocket API URL | `wss://xxx.execute-api.ap-southeast-1.amazonaws.com/prod` |
| `FrontendBucketName` | Tên S3 frontend bucket | `code-protector-aws-prod-123456789012` |
| `ContentBucketName` | Tên S3 content bucket | `code-protector-aws-prod-content-123456789012` |

Lưu các giá trị này lại — bạn cần dùng ở các giai đoạn tiếp theo.

---

## Bước 5: Kiểm tra CloudFormation Stack

Trên AWS Console, điều hướng đến **CloudFormation → Stacks → guardscript-prod**.

Kiểm tra:
- Status là `CREATE_COMPLETE`
- Tất cả 14 bảng DynamoDB có trong mục **Resources**
- Lambda function `code-protector-aws-prod-api` có trong **Resources**
- CloudFront distribution ở trạng thái `Enabled` và `Deployed`

Bạn cũng có thể kiểm tra qua CLI:

```bash
aws cloudformation describe-stacks \
  --stack-name guardscript-prod \
  --query "Stacks[0].StackStatus"
# Kết quả mong đợi: "CREATE_COMPLETE"
```

---

## Tóm tắt

Kết thúc giai đoạn này bạn đã:

- [ ] Build SAM application với `sam build`
- [ ] Deploy toàn bộ infrastructure stack (Lambda, 14 bảng DynamoDB, S3, CloudFront, WebSocket, CloudWatch)
- [ ] Ghi lại stack outputs (CloudFront domain, Lambda URL, WebSocket endpoint, tên buckets)
- [ ] Xác nhận CloudFormation stack ở trạng thái `CREATE_COMPLETE`

Tiếp tục sang **Giai đoạn 3: Triển khai Frontend**.
