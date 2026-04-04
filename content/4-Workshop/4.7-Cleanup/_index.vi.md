---
title: "Dọn dẹp tài nguyên"
date: 2026-03-26
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

Khi bạn hoàn thành workshop, hãy làm theo các bước sau để xóa toàn bộ tài nguyên AWS và tránh phát sinh chi phí.

> **Lưu ý quan trọng:** Các S3 bucket (`FrontendBucket` và `ContentBucket`) được cấu hình `DeletionPolicy: Retain` trong CloudFormation template. Bạn phải làm trống và xóa chúng **thủ công** sau khi xóa CloudFormation stack.

---

## Bước 1: Làm trống các S3 Bucket

Trước khi xóa stack, hãy làm trống cả hai application S3 bucket (bao gồm tất cả object versions, vì versioning đang bật):

```bash
# Xóa tất cả object versions từ frontend bucket
aws s3api delete-objects \
  --bucket FRONTEND_BUCKET_NAME \
  --delete "$(aws s3api list-object-versions \
    --bucket FRONTEND_BUCKET_NAME \
    --query '{Objects: Versions[].{Key:Key,VersionId:VersionId}}' \
    --output json)"

# Xóa tất cả object versions từ content bucket
aws s3api delete-objects \
  --bucket CONTENT_BUCKET_NAME \
  --delete "$(aws s3api list-object-versions \
    --bucket CONTENT_BUCKET_NAME \
    --query '{Objects: Versions[].{Key:Key,VersionId:VersionId}}' \
    --output json)"
```

Sau đó xóa các object còn lại:

```bash
aws s3 rm s3://FRONTEND_BUCKET_NAME --recursive
aws s3 rm s3://CONTENT_BUCKET_NAME --recursive
```

---

## Bước 2: Xóa CloudFormation Stack

Xóa infrastructure stack của GuardScript:

```bash
aws cloudformation delete-stack --stack-name guardscript-prod
```

Theo dõi tiến trình xóa:

```bash
aws cloudformation describe-stacks \
  --stack-name guardscript-prod \
  --query "Stacks[0].StackStatus"
```

Chờ đến khi status là `DELETE_COMPLETE` (thường mất 3–5 phút). Nếu stack bị kẹt, kiểm tra tab **Events** trên CloudFormation console để tìm lỗi.

---

## Bước 3: Xóa các S3 Bucket

Sau khi stack đã xóa, xóa các bucket đã được retain (lúc này đã rỗng):

```bash
aws s3 rb s3://FRONTEND_BUCKET_NAME
aws s3 rb s3://CONTENT_BUCKET_NAME
```

---

## Bước 4: Xóa Deployment Bucket

Xóa deployment bucket được tạo ở Giai đoạn 1:

```bash
aws s3 rm s3://DEPLOY_BUCKET --recursive
aws s3 rb s3://DEPLOY_BUCKET
```

---

## Bước 5: Kiểm tra Cleanup

Xác nhận các tài nguyên chính đã được xóa:

```bash
# Kiểm tra stack đã bị xóa
aws cloudformation describe-stacks --stack-name guardscript-prod 2>&1 | grep "does not exist"

# Kiểm tra Lambda function đã bị xóa
aws lambda get-function --function-name code-protector-aws-prod-api 2>&1 | grep "ResourceNotFoundException"

# Kiểm tra S3 buckets đã bị xóa
aws s3 ls | grep "code-protector-aws"
# (không có kết quả)
```

---

## Tóm tắt

| Tài nguyên | Phương pháp dọn dẹp |
|---|---|
| CloudFormation stack | `aws cloudformation delete-stack` |
| Frontend S3 bucket | Làm trống versions + `aws s3 rb` |
| Content S3 bucket | Làm trống versions + `aws s3 rb` |
| Deployment S3 bucket | `aws s3 rm --recursive` + `aws s3 rb` |
| Lambda function | Tự động xóa cùng stack |
| DynamoDB tables | Tự động xóa cùng stack |
| CloudFront distribution | Tự động xóa cùng stack |
| WebSocket API | Tự động xóa cùng stack |
| CloudWatch Alarms / Dashboard | Tự động xóa cùng stack |
| IAM Execution Role | Tự động xóa cùng stack |

> **Lưu ý:** CloudWatch Log Groups có thể được giữ lại tùy vào tham số `ManageApiLogGroup`. Xóa thủ công qua **CloudWatch → Log Groups** nếu cần.
