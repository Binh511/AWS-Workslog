---
title: "Giai đoạn 3: Triển khai Frontend"
date: 2026-03-26
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

Trong giai đoạn này bạn sẽ upload các static frontend assets lên S3 frontend bucket, sau đó invalidate CloudFront cache để các file mới được phục vụ ngay lập tức.

## Bước 1: Cấu hình API Endpoint cho Frontend

Frontend JavaScript cần biết CloudFront domain (URL app của bạn) và WebSocket URL. Mở `frontend/script.client.js` (hoặc file config tương ứng) và cập nhật các constants endpoint với giá trị từ stack outputs ở Giai đoạn 2:

```js
// Ví dụ — cập nhật với CloudFront domain thực của bạn
const API_BASE = 'https://d1abc123.cloudfront.net';
const WS_URL   = 'wss://xxx.execute-api.ap-southeast-1.amazonaws.com/prod';
```

> Nếu frontend đọc các giá trị này từ biến môi trường hoặc file config riêng, cập nhật tương ứng.

---

## Bước 2: Đồng bộ Frontend lên S3

Dùng AWS CLI để upload toàn bộ file frontend lên S3 frontend bucket:

```bash
aws s3 sync frontend/ s3://FRONTEND_BUCKET_NAME \
  --delete \
  --cache-control "public, max-age=86400"
```

> Thay `FRONTEND_BUCKET_NAME` bằng giá trị `FrontendBucketName` output từ Giai đoạn 2.

Đối với các file **không nên** được cache (ví dụ: HTML pages):

```bash
aws s3 sync frontend/ s3://FRONTEND_BUCKET_NAME \
  --exclude "*" \
  --include "*.html" \
  --cache-control "no-cache, no-store, must-revalidate"
```

Kiểm tra upload:

```bash
aws s3 ls s3://FRONTEND_BUCKET_NAME --recursive | head -20
```

---

## Bước 3: Invalidate CloudFront Cache

Sau khi sync, buộc CloudFront phục vụ file mới bằng cách tạo invalidation:

```bash
aws cloudfront create-invalidation \
  --distribution-id DISTRIBUTION_ID \
  --paths "/*"
```

> Lấy `DISTRIBUTION_ID` từ AWS Console (**CloudFront → Distributions**) hoặc từ stack output.

Invalidation mất 30–60 giây để propagate toàn cầu. Kiểm tra trạng thái:

```bash
aws cloudfront list-invalidations \
  --distribution-id DISTRIBUTION_ID \
  --query "InvalidationList.Items[0].{Status:Status,Id:Id}"
```

Trạng thái sẽ chuyển từ `InProgress` sang `Completed`.

---

## Bước 4: Kiểm tra Frontend

Mở CloudFront domain trong trình duyệt:

```
https://CLOUDFRONT_DOMAIN/
```

Bạn sẽ thấy **trang landing của GuardScript**. Kiểm tra:

- [ ] Landing page tải không có lỗi
- [ ] Các link điều hướng hoạt động (Login, Register)
- [ ] Không có lỗi browser console liên quan đến thiếu assets

---

## Tóm tắt

Kết thúc giai đoạn này bạn đã:

- [ ] Cập nhật cấu hình API endpoint cho frontend
- [ ] Đồng bộ toàn bộ frontend assets lên `s3://FRONTEND_BUCKET_NAME`
- [ ] Tạo CloudFront invalidation và xác nhận trạng thái `Completed`
- [ ] Kiểm tra landing page tải được tại `https://CLOUDFRONT_DOMAIN/`

Tiếp tục sang **Giai đoạn 4: Cấu hình & Kiểm tra**.
