---
title: "Phase 2: Deploy AWS Infrastructure"
date: 2026-03-26
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

In this phase you will use AWS SAM to build and deploy the full GuardScript infrastructure stack, including Lambda, DynamoDB tables, S3 buckets, CloudFront, WebSocket API, and CloudWatch.

## Step 1: Navigate to the infra Directory

```bash
cd code_protector_aws/infra
```

The SAM template is located at `infra/template.yaml`.

---

## Step 2: Build the SAM Application

```bash
sam build --template-file template.yaml
```

SAM validates the template and prepares the build artifacts. A successful build outputs:

```
Build Succeeded
```

---

## Step 3: Deploy the Stack

Run the guided deploy (first time only). SAM will prompt you for each parameter:

```bash
sam deploy --guided
```

Answer the prompts as follows:

| Prompt | Value |
|---|---|
| Stack Name | `guardscript-prod` |
| AWS Region | `REGION` (e.g. `ap-southeast-1`) |
| Parameter `ProjectName` | `code-protector-aws` |
| Parameter `Stage` | `prod` |
| Parameter `FrontendBucketName` | *(leave blank to auto-generate)* |
| Parameter `ContentBucketName` | *(leave blank to auto-generate)* |
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

SAM will show a **Change Set** preview before creating resources. Review and confirm.

> **Note:** Deployment typically takes 3–8 minutes. CloudFront distributions take the longest to provision.

---

## Step 4: Note the Stack Outputs

After a successful deploy, SAM prints the stack outputs. Record these values:

| Output Key | Description | Example |
|---|---|---|
| `DistributionDomainName` | CloudFront domain (your app URL) | `d1abc123.cloudfront.net` |
| `ApiFunctionUrl` | Lambda Function URL | `https://xxx.lambda-url.ap-southeast-1.on.aws/` |
| `WebSocketEndpoint` | WebSocket API URL | `wss://xxx.execute-api.ap-southeast-1.amazonaws.com/prod` |
| `FrontendBucketName` | S3 frontend bucket name | `code-protector-aws-prod-123456789012` |
| `ContentBucketName` | S3 content bucket name | `code-protector-aws-prod-content-123456789012` |

Save these to a local notes file — you will need them in the next phases.

---

## Step 5: Verify the CloudFormation Stack

In the AWS Console, navigate to **CloudFormation → Stacks → guardscript-prod**.

Verify:
- Status is `CREATE_COMPLETE`
- All 14 DynamoDB tables are listed under **Resources**
- The Lambda function `code-protector-aws-prod-api` exists under **Resources**
- CloudFront distribution is `Enabled` and `Deployed`

You can also verify via CLI:

```bash
aws cloudformation describe-stacks \
  --stack-name guardscript-prod \
  --query "Stacks[0].StackStatus"
# Expected: "CREATE_COMPLETE"
```

---

## Summary

At the end of this phase you have:

- [ ] Built the SAM application with `sam build`
- [ ] Deployed the full infrastructure stack (Lambda, 14 DynamoDB tables, S3, CloudFront, WebSocket, CloudWatch)
- [ ] Recorded the stack outputs (CloudFront domain, Lambda URL, WebSocket endpoint, bucket names)
- [ ] Verified the CloudFormation stack is `CREATE_COMPLETE`

Proceed to **Phase 3: Deploy Frontend**.
