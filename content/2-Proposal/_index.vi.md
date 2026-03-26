---
title: "Bản đề xuất"
date: 2026-03-26
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

Tại phần này, bạn cần tóm tắt các nội dung trong workshop mà bạn **dự tính** sẽ làm.

# Code Protector – IrisAuth
## Nền tảng bảo vệ mã nguồn và phân phối script an toàn đa ngôn ngữ trên AWS Cloud

**Thông tin nhóm thực hiện:**

| Họ tên | MSSV | Vai trò |
|---|---|---|
| Võ Tấn Phát | SE194484 | Nhóm trưởng |
| Bùi Minh Hiển | SE190829 | Thành viên |
| Dương Nguyên Bình | SE194067 | Thành viên |
| Trần Vinh | SE193927 | Thành viên |
| Nguyễn Duy Tùng | SE196572 | Thành viên |
| Nguyễn Đức Trí | SE194091 | Thành viên |

**AWS Services:** `Lambda` · `S3` · `DynamoDB` · `CloudFront` · `CloudWatch` · `SES`

---

### 1. Tóm tắt điều hành

**Code Protector** (tên hệ thống: **IrisAuth**) là nền tảng SaaS cho phép developer upload mã nguồn lên server và phân phối đến người dùng qua cơ chế loader được mã hóa. Thay vì chia sẻ source code trực tiếp, người dùng cuối chỉ nhận một đoạn loader nhỏ 1–2 dòng. Server trả về code đã mã hóa chỉ khi người dùng vượt qua toàn bộ các kiểm tra bảo mật, và code chỉ tồn tại trong RAM khi thực thi — không bao giờ được lưu ra file.

Hệ thống được triển khai trên nền tảng **AWS Cloud**, tận dụng kiến trúc serverless và các dịch vụ managed để đảm bảo khả năng mở rộng cao, độ trễ thấp toàn cầu, và vận hành không cần quản lý hạ tầng thủ công.

---

### 2. Tuyên bố vấn đề

#### Vấn đề hiện tại

Trong môi trường phát triển phần mềm hiện đại, việc bảo vệ mã nguồn là một thách thức cấp thiết. Khi các developer phân phối script Python hoặc JavaScript đến người dùng cuối, họ đối mặt với rủi ro source code bị sao chép, chỉnh sửa, hoặc tái phân phối trái phép mà không có cơ chế kiểm soát hiệu quả.

Các giải pháp hiện có như obfuscation đơn thuần hay đóng gói file `.exe` còn nhiều hạn chế: dễ bị reverse engineering, không kiểm soát được ai đang chạy code, không thu hồi được quyền truy cập khi cần thiết. Thị trường thiếu một giải pháp tích hợp vừa bảo vệ mã nguồn, vừa quản lý phân phối và kiểm soát truy cập một cách toàn diện.

#### Giải pháp

IrisAuth giải quyết vấn đề này thông qua:

- **Loader-based distribution**: Người dùng cuối chỉ nhận loader 1–2 dòng, không bao giờ tiếp xúc source code dạng plaintext.
- **Dual encryption protocol**: Protocol v2 (XOR + SHA-256) và v3 (ECDH X25519 + AES-256-GCM) với perfect forward secrecy.
- **HWID Lock**: Khóa license theo phần cứng, chống chia sẻ tài khoản.
- **AWS Serverless**: Auto-scaling, zero-downtime, chi phí theo mức sử dụng thực tế.

#### Lợi ích và ROI

- Bảo vệ toàn diện mã nguồn Python và JavaScript khỏi reverse engineering.
- Kiểm soát truy cập theo thời gian thực: bật/tắt license tức thì, không cần cập nhật phía client.
- Monitoring đầy đủ qua CloudWatch: biết chính xác ai đang chạy code, từ đâu, bao nhiêu lần.
- Chi phí hạ tầng thấp nhờ kiến trúc serverless — chỉ tính tiền khi có request thực tế.

---

### 3. Kiến trúc giải pháp

Hệ thống IrisAuth được triển khai theo mô hình **serverless thuần túy** trên AWS, loại bỏ hoàn toàn việc quản lý server vật lý.

**Luồng request tiêu biểu:**
```
Client (browser / loader)
  → CloudFront Distribution (SSL termination, cache layer)
    → Static assets: S3 bucket (HTML/CSS/JS)
    → API /api/*: API Gateway → AWS Lambda
      → DynamoDB (metadata: users, licenses, logs)
      → S3 (script content đã mã hóa + gzip)
      → Amazon SES (email invitation)
  → CloudWatch Logs (auto stream từ Lambda)
  → API Gateway WebSocket API (real-time sync)
```

#### Dịch vụ AWS sử dụng

| Tầng | Dịch vụ | Chi tiết |
|---|---|---|
| Runtime API | AWS Lambda (Node.js v18+) | ES Module, Express.js wrapped bằng aws-serverless-express, cold start <1s |
| Database | Amazon DynamoDB | NoSQL key-value, on-demand capacity, TTL tự động cho rate_limits & sessions |
| Object Storage | Amazon S3 | Lưu script đã mã hóa + gzip (key = UUID), versioning bật, SSE-KMS |
| CDN & Edge | Amazon CloudFront | Phân phối static frontend, terminate SSL, cache S3 assets, WAF tích hợp |
| Email | Amazon SES | Gửi workspace invitation email, cấu hình DKIM + SPF, sandbox → production |
| Monitoring | Amazon CloudWatch | Log Groups cho từng Lambda, metric alarms (error rate, latency), dashboard |
| Real-time | API Gateway WebSocket API | Broadcast per-workspace updates |
| Frontend | HTML5, CSS3, Vanilla JS | 5 trang static, deploy lên S3, phân phối qua CloudFront |

#### Thiết kế thành phần

- **Client Layer**: Browser (dashboard) và loader client (Python 3.7+, Node.js v14+, Tampermonkey userscript).
- **Edge Layer**: CloudFront Distribution + AWS WAF — terminate SSL, cache static, chặn bad bots tại edge.
- **Compute Layer**: AWS Lambda — xử lý toàn bộ business logic, auto-scale từ 0 đến ∞.
- **Data Layer**: DynamoDB (metadata, licenses, logs) + S3 (script content).
- **Notification Layer**: Amazon SES (email) + Discord Webhook (alerting).
- **Observability Layer**: CloudWatch Logs, Metrics, Alarms, Dashboard.

---

### 4. Triển khai kỹ thuật

#### 4.1. Hệ thống xác thực (tự xây dựng, không dùng thư viện JWT)

Token được tự implement theo cấu trúc `v2.<payload_base64url>.<signature_base64url>`:

- Ký token bằng **HMAC-SHA256**, secret sinh ngẫu nhiên 48 bytes lưu trong DynamoDB (bảng `app_config`).
- Token TTL: 7 ngày. Khi đổi mật khẩu, toàn bộ token cũ bị vô hiệu hóa tức thì (so sánh `iat` với `password_changed_at`).
- Mật khẩu băm bằng **PBKDF2-SHA256, 210.000 iterations**, salt ngẫu nhiên 16 bytes — đạt chuẩn OWASP.
- Workspace PIN cũng dùng PBKDF2, sinh pin session token riêng lưu DynamoDB với TTL tự động.

#### 4.2. Hai protocol phân phối code

**Protocol v2 — GET /api/v5/execute (XOR)**

- Client gửi: `id`, `license key`, `HWID`, `timestamp`, `nonce`, `HMAC-SHA256 signature`.
- Server xác thực signature, kiểm tra timestamp ±5 phút (chống replay attack).
- Mã hóa response bằng XOR với `key = SHA256(derivedSecret:hwid:nonce:id)`.
- Script content được đọc từ Amazon S3.

**Protocol v3 — POST /api/v5/handshake (ECDH + AES-GCM)**

- Client tạo cặp khóa **X25519**, gửi public key lên server cùng signature.
- Lambda tính shared secret bằng `diffieHellman()`, dẫn xuất AES key bằng **HKDF-SHA256** (RFC 5869).
- Mã hóa script bằng **AES-256-GCM** — mỗi phiên có session key hoàn toàn khác nhau (perfect forward secrecy).

#### 4.3. Thuật toán và chuẩn bảo mật

| Thuật toán | Ứng dụng cụ thể |
|---|---|
| PBKDF2-SHA256 (210.000 iter) | Băm mật khẩu người dùng, băm Workspace PIN |
| HMAC-SHA256 | Ký auth token, ký request loader, ký response server |
| ECDH X25519 | Trao đổi khóa Protocol v3 (handshake) |
| HKDF-SHA256 | Dẫn xuất AES key từ ECDH shared secret (RFC 5869) |
| AES-256-GCM | Mã hóa script lưu S3 + mã hóa truyền tải Protocol v3 |
| XOR + SHA-256 | Mã hóa truyền tải Protocol v2 |
| Gzip (Pako) | Nén content trước khi lưu S3 |
| `crypto.timingSafeEqual` | So sánh token/signature — chống timing attack |
| Nonce + Timestamp ±5 phút | Chống replay attack trên mọi request loader |
| S3 SSE-KMS | Mã hóa at-rest mọi object trên S3 |
| CloudFront + ACM | TLS 1.2+ enforce trên toàn bộ kết nối |

#### 4.4. Cơ sở dữ liệu — Amazon DynamoDB

| Bảng DynamoDB | Partition Key / Sort Key | Chức năng |
|---|---|---|
| `users` | PK: `userId` | Tài khoản người dùng, role, `password_changed_at` |
| `workspaces` | PK: `workspaceId` | Workspace, loader_key, encryption_key, PIN hash |
| `projects` | PK: `workspaceId`, SK: `projectId` | Project (script), cài đặt bảo mật, execution count |
| `project_files` | PK: `projectId`, SK: `fileId` | File/folder trong project, entry point, sort_order |
| `licenses` | PK: `workspaceId`, SK: `licenseKey` | License key, HWID, ngày hết hạn, usage count |
| `access_lists` | PK: `workspaceId`, SK: `ip#type` | IP blacklist / whitelist theo workspace |
| `workspace_members` | PK: `workspaceId`, SK: `userId` | Thành viên được mời, vai trò |
| `workspace_invitations` | PK: `token` | Token mời qua email (SES), **TTL tự động** |
| `pin_verifications` | PK: `sessionToken` | Session token sau xác thực PIN, **TTL tự động** |
| `logs` | PK: `workspaceId`, SK: `timestamp#uuid` | Nhật ký sự kiện, GSI trên `country`, `timestamp` |
| `app_config` | PK: `configKey` | Cấu hình hệ thống (HMAC secret, loader secret…) |
| `rate_limits` | PK: `rateLimitKey` | Sliding window với **TTL tự động** dọn dẹp |

> **Lưu ý thiết kế**: TTL được bật trên `workspace_invitations`, `pin_verifications`, và `rate_limits` để tự động xóa records hết hạn mà không cần cronjob. GSI trên `country` và `timestamp` của bảng `logs` hỗ trợ truy vấn phân tích.

---

### 5. Lộ trình & Mốc triển khai

| Giai đoạn | Nội dung | Thời gian dự kiến |
|---|---|---|
| 1. Phân tích & Thiết kế | Yêu cầu chi tiết, DynamoDB schema, API design, CloudFront behavior rules | Tuần 1–2 |
| 2. Hạ tầng AWS | Provisioning Lambda, DynamoDB, S3, CloudFront, SES, CloudWatch (IaC bằng AWS SAM / CDK) | Tuần 3 |
| 3. Backend Core | Auth, Workspace, Project, File management API — migrate từ SQLite → DynamoDB, filesystem → S3 | Tuần 4–6 |
| 4. Bảo mật & Loader | ECDH handshake, AES encryption, Python/JS loader hoạt động qua CloudFront endpoint | Tuần 7–8 |
| 5. License & Access | License system, HWID, Rate limit (DynamoDB TTL), Blacklist, IP Whitelist | Tuần 9 |
| 6. Frontend & Email | 5 trang giao diện, WebSocket integration (API GW), SES email invitation | Tuần 10–11 |
| 7. Obfuscator | Python AST obfuscator, bundle generation | Tuần 12 |
| 8. Testing & Deploy | Unit test, integration test, load test, CloudWatch alarm tuning, deploy production | Tuần 13–14 |

---

### 6. Ước tính ngân sách

| Dịch vụ | Chi phí ước tính | Ghi chú |
|---|---|---|
| AWS Lambda | ~$0.00/tháng | On-demand, free tier 1M requests/tháng |
| Amazon DynamoDB | ~$0.00–$1.00/tháng | On-demand capacity, miễn phí 25 GB storage |
| Amazon S3 | ~$0.10–$0.50/tháng | Script storage (gzip+encrypted), static assets |
| Amazon CloudFront | ~$0.00–$1.00/tháng | 1 TB data transfer free/tháng đầu |
| Amazon SES | ~$0.10/tháng | $0.10 per 1,000 emails |
| Amazon CloudWatch | ~$0.00–$0.50/tháng | Log retention 30 ngày, basic metrics miễn phí |
| API Gateway | ~$0.01–$0.10/tháng | REST + WebSocket API |
| **Tổng ước tính** | **~$1–3/tháng** | Theo mức sử dụng thực tế |

> Chi phí tăng tuyến tính theo số request — phù hợp giai đoạn phát triển và demo. Có thể xem chi tiết tại [AWS Pricing Calculator](https://calculator.aws/).

---

### 7. Đánh giá rủi ro

| Rủi ro | Mức ảnh hưởng | Xác suất | Chiến lược giảm thiểu |
|---|---|---|---|
| Cold start Lambda làm tăng latency | Trung bình | Trung bình | Provisioned Concurrency cho endpoint quan trọng; Node.js 18 cold start <1s |
| DynamoDB throttling khi burst traffic | Cao | Thấp | On-demand capacity tự động scale; CloudWatch Alarm cảnh báo sớm |
| S3 object PUT/GET lỗi | Cao | Thấp | Retry logic trong Lambda; S3 versioning bật để phục hồi |
| SES sandbox hạn chế gửi email | Trung bình | Cao (ban đầu) | Verify domain và request production access sớm trong sprint đầu |
| Vượt ngân sách AWS | Trung bình | Thấp | AWS Budgets alert, tối ưu TTL và cache CloudFront |
| Replay attack trên loader | Cao | Thấp | Timestamp ±5 phút + nonce + HMAC signature bắt buộc |

**Kế hoạch dự phòng:**
- Nếu API Gateway gặp sự cố: chuyển sang Lambda Function URL trực tiếp.
- Nếu S3 không truy cập được: Lambda retry với exponential backoff, log lỗi vào CloudWatch.
- Dùng **AWS CloudFormation / CDK** để tái tạo toàn bộ hạ tầng nhanh chóng khi cần.

---

### 8. Kết quả kỳ vọng

**Kết quả kỹ thuật:**
- Nền tảng web hoạt động đầy đủ, deploy trên AWS với kiến trúc serverless (Lambda + DynamoDB + S3 + CloudFront).
- Hệ thống bảo vệ mã nguồn với 2 protocol mã hóa (v2 XOR và v3 ECDH/AES-GCM), script content lưu an toàn trên S3 với SSE-KMS.
- Loader client tương thích Python 3.7+, Node.js v14+, và Tampermonkey — hoạt động qua CloudFront endpoint.
- Email invitation tự động qua Amazon SES — thành viên nhận link trực tiếp vào hộp thư.
- Monitoring & alerting đầy đủ qua CloudWatch: log tập trung, alarm tự động, dashboard KPI.

**Giá trị dài hạn:**
- Nền tảng có thể mở rộng cho các ngôn ngữ lập trình khác ngoài Python và JavaScript.
- Kiến trúc serverless là nền tảng thực hành AWS Cloud-native có thể tái sử dụng cho các dự án tương lai.
- Tài liệu kỹ thuật đầy đủ: kiến trúc AWS, API spec, hướng dẫn deploy bằng AWS SAM/CDK.