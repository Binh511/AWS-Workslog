---
title: "Cleanup"
date: 2026-03-26
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

When you are finished with the workshop, follow these steps to remove all AWS resources and avoid ongoing charges.

> **Important:** The S3 buckets (`FrontendBucket` and `ContentBucket`) are set to `DeletionPolicy: Retain` in the CloudFormation template. You must empty and delete them **manually** after deleting the CloudFormation stack.

---

## Step 1: Empty the S3 Buckets

Before deleting the stack, empty both application S3 buckets (including all object versions, because versioning is enabled):

```bash
# Empty all object versions from the frontend bucket
aws s3api delete-objects \
  --bucket FRONTEND_BUCKET_NAME \
  --delete "$(aws s3api list-object-versions \
    --bucket FRONTEND_BUCKET_NAME \
    --query '{Objects: Versions[].{Key:Key,VersionId:VersionId}}' \
    --output json)"

# Empty all object versions from the content bucket
aws s3api delete-objects \
  --bucket CONTENT_BUCKET_NAME \
  --delete "$(aws s3api list-object-versions \
    --bucket CONTENT_BUCKET_NAME \
    --query '{Objects: Versions[].{Key:Key,VersionId:VersionId}}' \
    --output json)"
```

Then delete any remaining delete markers:

```bash
aws s3 rm s3://FRONTEND_BUCKET_NAME --recursive
aws s3 rm s3://CONTENT_BUCKET_NAME --recursive
```

---

## Step 2: Delete the CloudFormation Stack

Delete the GuardScript infrastructure stack:

```bash
aws cloudformation delete-stack --stack-name guardscript-prod
```

Monitor deletion progress:

```bash
aws cloudformation describe-stacks \
  --stack-name guardscript-prod \
  --query "Stacks[0].StackStatus"
```

Wait until the status is `DELETE_COMPLETE` (typically 3–5 minutes). If the stack gets stuck, check the **Events** tab in the CloudFormation console for errors.

---

## Step 3: Delete the S3 Buckets

After the stack is deleted, remove the (now-empty) retained buckets:

```bash
aws s3 rb s3://FRONTEND_BUCKET_NAME
aws s3 rb s3://CONTENT_BUCKET_NAME
```

---

## Step 4: Delete the Deployment Bucket

Remove the deployment bucket created in Phase 1:

```bash
aws s3 rm s3://DEPLOY_BUCKET --recursive
aws s3 rb s3://DEPLOY_BUCKET
```

---

## Step 5: Verify Cleanup

Confirm all main resources are removed:

```bash
# Verify stack is gone
aws cloudformation describe-stacks --stack-name guardscript-prod 2>&1 | grep "does not exist"

# Verify Lambda function is gone
aws lambda get-function --function-name code-protector-aws-prod-api 2>&1 | grep "ResourceNotFoundException"

# Verify S3 buckets are gone
aws s3 ls | grep "code-protector-aws"
# (should return nothing)
```

---

## Summary

| Resource | Cleanup Method |
|---|---|
| CloudFormation stack | `aws cloudformation delete-stack` |
| Frontend S3 bucket | Empty versions + `aws s3 rb` |
| Content S3 bucket | Empty versions + `aws s3 rb` |
| Deployment S3 bucket | `aws s3 rm --recursive` + `aws s3 rb` |
| Lambda function | Deleted automatically with stack |
| DynamoDB tables | Deleted automatically with stack |
| CloudFront distribution | Deleted automatically with stack |
| WebSocket API | Deleted automatically with stack |
| CloudWatch Alarms / Dashboard | Deleted automatically with stack |
| IAM Execution Role | Deleted automatically with stack |

> **Note:** CloudWatch Log Groups may be retained depending on the `ManageApiLogGroup` parameter. Delete manually via **CloudWatch → Log Groups** if needed.
