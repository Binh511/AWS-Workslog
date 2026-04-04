---
title: "Phase 1: Prepare Lambda Artifacts"
date: 2026-03-26
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

Before deploying the CloudFormation stack, you need to package the Lambda source code and upload it to an S3 bucket. AWS SAM will reference this artifact during deployment.

## Step 1: Install Dependencies

Navigate to the `code_protector_aws` directory and install Node.js dependencies:

```bash
cd code_protector_aws
npm install
```

Verify that `node_modules/` is created and no installation errors occur.

---

## Step 2: Create a Deployment S3 Bucket

Create an S3 bucket in your target region to store the Lambda deployment artifact. This bucket is separate from the application buckets (which SAM will create).

```bash
aws s3 mb s3://DEPLOY_BUCKET --region REGION
```

> Replace `DEPLOY_BUCKET` and `REGION` with your values from the Prerequisites section.
>
> Example: `aws s3 mb s3://guardscript-deploy-ap-southeast-1 --region ap-southeast-1`

Verify the bucket exists:

```bash
aws s3 ls | grep DEPLOY_BUCKET
```

---

## Step 3: Package the Lambda Code

Create a ZIP archive of the Lambda source code. The archive must include the `src/` folder and `node_modules/`.

**On Linux / macOS:**
```bash
zip -r lambda.zip src/ node_modules/ package.json
```

**On Windows (PowerShell):**
```powershell
Compress-Archive -Path src, node_modules, package.json -DestinationPath lambda.zip -Force
```

Verify the ZIP file was created:

```bash
ls -lh lambda.zip   # Linux/macOS
# or
Get-Item lambda.zip  # PowerShell
```

---

## Step 4: Upload the Lambda ZIP to S3

Upload the artifact to the deployment bucket:

```bash
aws s3 cp lambda.zip s3://DEPLOY_BUCKET/lambda.zip
```

Verify the upload:

```bash
aws s3 ls s3://DEPLOY_BUCKET/
```

You should see `lambda.zip` listed with its size and upload timestamp.

---

## Summary

At the end of this phase you have:

- [ ] Installed all Node.js dependencies
- [ ] Created a deployment S3 bucket (`DEPLOY_BUCKET`)
- [ ] Packaged `lambda.zip` from `src/` + `node_modules/`
- [ ] Uploaded `lambda.zip` to `s3://DEPLOY_BUCKET/lambda.zip`

Proceed to **Phase 2: Deploy AWS Infrastructure**.
