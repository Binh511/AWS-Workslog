---
title: "Phase 4: Configure & Validate"
date: 2026-03-26
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

In this final setup phase you will seed the required initial configuration into DynamoDB and run smoke tests to validate the deployment end-to-end.

## Step 1: Seed App Configuration (DynamoDB)

The Lambda function reads its HMAC signing secret and loader secret from the `app_config` DynamoDB table. You must insert these before the platform is usable.

Replace `APP_CONFIG_TABLE` with your actual table name (from the CloudFormation output or using the pattern `code-protector-aws-prod-app-config`).

### 1a. Set the HMAC Secret (Token Signing)

```bash
aws dynamodb put-item \
  --table-name APP_CONFIG_TABLE \
  --item '{
    "key": {"S": "hmac_secret"},
    "value": {"S": "REPLACE_WITH_A_STRONG_RANDOM_SECRET_MIN_32_CHARS"}
  }'
```

> Generate a strong secret: `openssl rand -hex 32`

### 1b. Set the Loader Secret (Loader Request Signing)

```bash
aws dynamodb put-item \
  --table-name APP_CONFIG_TABLE \
  --item '{
    "key": {"S": "loader_secret"},
    "value": {"S": "REPLACE_WITH_A_DIFFERENT_STRONG_RANDOM_SECRET"}
  }'
```

Verify both items are stored:

```bash
aws dynamodb scan \
  --table-name APP_CONFIG_TABLE \
  --query "Items[*].key.S"
```

Expected output: `["hmac_secret", "loader_secret"]`

---

## Step 2: Register the First Admin User

Open your CloudFront domain in a browser and navigate to `/register`:

```
https://CLOUDFRONT_DOMAIN/register
```

Create an account using your email and a strong password. This first account will be promoted to admin in the next step.

---

## Step 3: Promote User to Admin

Retrieve the user ID from the `users` table:

```bash
aws dynamodb query \
  --table-name code-protector-aws-prod-users \
  --index-name EmailIndex \
  --key-condition-expression "email = :email" \
  --expression-attribute-values '{":email": {"S": "YOUR_EMAIL@example.com"}}' \
  --query "Items[0].id.S"
```

Then set the `role` attribute to `admin`:

```bash
aws dynamodb update-item \
  --table-name code-protector-aws-prod-users \
  --key '{"id": {"S": "USER_ID_FROM_ABOVE"}}' \
  --update-expression "SET #r = :admin" \
  --expression-attribute-names '{"#r": "role"}' \
  --expression-attribute-values '{":admin": {"S": "admin"}}'
```

---

## Step 4: Smoke Test — Auth & Dashboard

1. Log in at `https://CLOUDFRONT_DOMAIN/login` with your admin account.
2. Verify you are redirected to `/dashboard`.
3. Confirm the **Admin** menu is visible in the navigation.

---

## Step 5: Smoke Test — Loader Endpoint (Protocol v2)

Test that the loader execute endpoint responds correctly to an invalid request (it should return a 400 or 401 error — not a 500):

```bash
curl -X GET "https://CLOUDFRONT_DOMAIN/api/v5/execute?id=test&license=test&hwid=test&timestamp=0&nonce=test&signature=invalid"
```

Expected: A JSON error response (e.g. `{"error":"Invalid signature"}`) — **not** an unhandled exception.

---

## Step 6: Smoke Test — WebSocket API

Use a WebSocket client (e.g. `wscat`) to verify the WebSocket endpoint accepts connections:

```bash
npm install -g wscat
wscat -c "WSS_ENDPOINT"
```

Expected: Connection established (press Ctrl+C to close).

---

## Step 7: Verify CloudWatch

In the AWS Console, navigate to **CloudWatch → Alarms**. You should see three alarms:

- `code-protector-aws-prod-api-errors` — GREEN (OK)
- `code-protector-aws-prod-api-throttles` — GREEN (OK)
- `code-protector-aws-prod-api-duration-p95` — GREEN (OK)

Also check **CloudWatch → Dashboards → code-protector-aws-prod-ops** for the operational dashboard.

---

## Summary

At the end of this phase you have:

- [ ] Seeded `hmac_secret` and `loader_secret` into the `app_config` table
- [ ] Registered the first user and promoted them to admin role
- [ ] Verified login and dashboard navigation work
- [ ] Confirmed the loader endpoint returns structured error responses
- [ ] Verified the WebSocket endpoint accepts connections
- [ ] Checked CloudWatch alarms are in OK state

**The GuardScript platform is now fully deployed and operational.**

Proceed to **Cleanup** when you are done.
