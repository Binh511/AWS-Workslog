---
title: "Giai đoạn 4: Cấu hình & Kiểm tra"
date: 2026-03-26
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Trong giai đoạn cuối này bạn sẽ seed cấu hình khởi tạo vào DynamoDB và chạy smoke tests để kiểm tra toàn bộ deployment.

## Bước 1: Seed App Configuration (DynamoDB)

Lambda function đọc HMAC signing secret và loader secret từ bảng DynamoDB `app_config`. Bạn phải insert các giá trị này trước khi nền tảng hoạt động.

Thay `APP_CONFIG_TABLE` bằng tên bảng thực của bạn (từ CloudFormation output hoặc theo pattern `code-protector-aws-prod-app-config`).

### 1a. Thiết lập HMAC Secret (Ký Token)

```bash
aws dynamodb put-item \
  --table-name APP_CONFIG_TABLE \
  --item '{
    "key": {"S": "hmac_secret"},
    "value": {"S": "THAY_BANG_SECRET_NGAU_NHIEN_MANH_TOI_THIEU_32_KY_TU"}
  }'
```

> Tạo secret mạnh: `openssl rand -hex 32`

### 1b. Thiết lập Loader Secret (Ký Loader Request)

```bash
aws dynamodb put-item \
  --table-name APP_CONFIG_TABLE \
  --item '{
    "key": {"S": "loader_secret"},
    "value": {"S": "THAY_BANG_MOT_SECRET_KHAC_CUNG_MANH_TUONG_TU"}
  }'
```

Kiểm tra cả hai item đã được lưu:

```bash
aws dynamodb scan \
  --table-name APP_CONFIG_TABLE \
  --query "Items[*].key.S"
```

Kết quả mong đợi: `["hmac_secret", "loader_secret"]`

---

## Bước 2: Đăng ký tài khoản Admin đầu tiên

Mở CloudFront domain trên trình duyệt và điều hướng đến `/register`:

```
https://CLOUDFRONT_DOMAIN/register
```

Tạo tài khoản với email và mật khẩu mạnh. Tài khoản đầu tiên này sẽ được cấp quyền admin ở bước tiếp theo.

---

## Bước 3: Cấp quyền Admin cho User

Lấy user ID từ bảng `users`:

```bash
aws dynamodb query \
  --table-name code-protector-aws-prod-users \
  --index-name EmailIndex \
  --key-condition-expression "email = :email" \
  --expression-attribute-values '{":email": {"S": "YOUR_EMAIL@example.com"}}' \
  --query "Items[0].id.S"
```

Sau đó set thuộc tính `role` thành `admin`:

```bash
aws dynamodb update-item \
  --table-name code-protector-aws-prod-users \
  --key '{"id": {"S": "USER_ID_TU_BUOC_TREN"}}' \
  --update-expression "SET #r = :admin" \
  --expression-attribute-names '{"#r": "role"}' \
  --expression-attribute-values '{":admin": {"S": "admin"}}'
```

---

## Bước 4: Smoke Test — Auth & Dashboard

1. Đăng nhập tại `https://CLOUDFRONT_DOMAIN/login` với tài khoản admin.
2. Kiểm tra bạn được redirect đến `/dashboard`.
3. Xác nhận menu **Admin** hiển thị trong navigation.

---

## Bước 5: Smoke Test — Loader Endpoint (Protocol v2)

Test loader execute endpoint phản hồi đúng với request không hợp lệ (phải trả về lỗi 400 hoặc 401 — không phải 500):

```bash
curl -X GET "https://CLOUDFRONT_DOMAIN/api/v5/execute?id=test&license=test&hwid=test&timestamp=0&nonce=test&signature=invalid"
```

Kết quả mong đợi: JSON error response (ví dụ `{"error":"Invalid signature"}`) — **không** phải unhandled exception.

---

## Bước 6: Smoke Test — WebSocket API

Dùng WebSocket client (ví dụ `wscat`) để kiểm tra WebSocket endpoint chấp nhận kết nối:

```bash
npm install -g wscat
wscat -c "WSS_ENDPOINT"
```

Kết quả mong đợi: Kết nối được thiết lập (nhấn Ctrl+C để đóng).

---

## Bước 7: Kiểm tra CloudWatch

Trên AWS Console, điều hướng đến **CloudWatch → Alarms**. Bạn sẽ thấy 3 alarms:

- `code-protector-aws-prod-api-errors` — XANH (OK)
- `code-protector-aws-prod-api-throttles` — XANH (OK)
- `code-protector-aws-prod-api-duration-p95` — XANH (OK)

Cũng kiểm tra **CloudWatch → Dashboards → code-protector-aws-prod-ops** để xem operational dashboard.

---

## Tóm tắt

Kết thúc giai đoạn này bạn đã:

- [ ] Seed `hmac_secret` và `loader_secret` vào bảng `app_config`
- [ ] Đăng ký user đầu tiên và cấp quyền admin
- [ ] Xác nhận đăng nhập và điều hướng dashboard hoạt động
- [ ] Kiểm tra loader endpoint trả về structured error response
- [ ] Xác nhận WebSocket endpoint chấp nhận kết nối
- [ ] Kiểm tra CloudWatch alarms ở trạng thái OK

**Nền tảng GuardScript đã được deploy và hoạt động đầy đủ.**

Tiếp tục sang phần **Dọn dẹp tài nguyên** khi bạn hoàn thành.
