---
title: "Phase 3: Deploy Frontend"
date: 2026-03-26
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

In this phase you will upload the static frontend assets to the S3 frontend bucket, then invalidate the CloudFront cache so the new assets are served immediately.

## Step 1: Configure the Frontend API Endpoint

The frontend JavaScript needs to know the CloudFront domain (your app URL) and the WebSocket URL. Open `frontend/script.client.js` (or the relevant config file) and update the endpoint constants with the values from your Phase 2 stack outputs:

```js
// Example — update with your actual CloudFront domain
const API_BASE = 'https://d1abc123.cloudfront.net';
const WS_URL   = 'wss://xxx.execute-api.ap-southeast-1.amazonaws.com/prod';
```

> If your frontend reads these values from environment variables or a config file, update accordingly.

---

## Step 2: Sync Frontend to S3

Use the AWS CLI to upload all frontend files to the frontend S3 bucket:

```bash
aws s3 sync frontend/ s3://FRONTEND_BUCKET_NAME \
  --delete \
  --cache-control "public, max-age=86400"
```

> Replace `FRONTEND_BUCKET_NAME` with the `FrontendBucketName` output from Phase 2.

For files that should **not** be cached (e.g. HTML pages), set a shorter cache-control:

```bash
aws s3 sync frontend/ s3://FRONTEND_BUCKET_NAME \
  --exclude "*" \
  --include "*.html" \
  --cache-control "no-cache, no-store, must-revalidate"
```

Verify the upload:

```bash
aws s3 ls s3://FRONTEND_BUCKET_NAME --recursive | head -20
```

---

## Step 3: Invalidate the CloudFront Cache

After syncing, force CloudFront to serve the fresh files by creating an invalidation:

```bash
aws cloudfront create-invalidation \
  --distribution-id DISTRIBUTION_ID \
  --paths "/*"
```

> Get `DISTRIBUTION_ID` from the AWS Console (**CloudFront → Distributions**) or from the stack output.

The invalidation takes 30–60 seconds to propagate globally. Check status:

```bash
aws cloudfront list-invalidations \
  --distribution-id DISTRIBUTION_ID \
  --query "InvalidationList.Items[0].{Status:Status,Id:Id}"
```

Status will change from `InProgress` to `Completed`.

---

## Step 4: Verify the Frontend

Open your CloudFront domain in a browser:

```
https://CLOUDFRONT_DOMAIN/
```

You should see the **GuardScript landing page**. Verify:

- [ ] Landing page loads without errors
- [ ] Navigation links work (Login, Register)
- [ ] No browser console errors related to missing assets

---

## Summary

At the end of this phase you have:

- [ ] Updated frontend API endpoint configuration
- [ ] Synced all frontend assets to `s3://FRONTEND_BUCKET_NAME`
- [ ] Created a CloudFront invalidation and confirmed `Completed` status
- [ ] Verified the landing page loads at `https://CLOUDFRONT_DOMAIN/`

Proceed to **Phase 4: Configure & Validate**.
