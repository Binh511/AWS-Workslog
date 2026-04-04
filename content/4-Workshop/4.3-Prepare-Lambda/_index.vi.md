---
title: "Giai đoạn 1: Chuẩn bị Lambda Artifacts"
date: 2026-03-26
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

Trước khi deploy CloudFormation stack, bạn cần đóng gói source code Lambda và upload lên S3 bucket. AWS SAM sẽ tham chiếu artifact này trong quá trình deploy.

## Bước 1: Cài đặt Dependencies

Di chuyển vào thư mục `code_protector_aws` và cài đặt dependencies Node.js:

```bash
cd code_protector_aws
npm install
```

Kiểm tra `node_modules/` được tạo ra và không có lỗi cài đặt.

---

## Bước 2: Tạo S3 Bucket cho Deployment

Tạo một S3 bucket ở region đích để lưu Lambda deployment artifact. Bucket này tách biệt với các application bucket (SAM sẽ tạo sau).

```bash
aws s3 mb s3://DEPLOY_BUCKET --region REGION
```

> Thay `DEPLOY_BUCKET` và `REGION` bằng giá trị của bạn từ phần Chuẩn bị.
>
> Ví dụ: `aws s3 mb s3://guardscript-deploy-ap-southeast-1 --region ap-southeast-1`

Kiểm tra bucket tồn tại:

```bash
aws s3 ls | grep DEPLOY_BUCKET
```

---

## Bước 3: Đóng gói Lambda Code

Tạo file ZIP từ source code Lambda. File ZIP phải bao gồm thư mục `src/` và `node_modules/`.

**Trên Linux / macOS:**
```bash
zip -r lambda.zip src/ node_modules/ package.json
```

**Trên Windows (PowerShell):**
```powershell
Compress-Archive -Path src, node_modules, package.json -DestinationPath lambda.zip -Force
```

Kiểm tra file ZIP đã được tạo:

```bash
ls -lh lambda.zip   # Linux/macOS
# hoặc
Get-Item lambda.zip  # PowerShell
```

---

## Bước 4: Upload Lambda ZIP lên S3

Upload artifact lên deployment bucket:

```bash
aws s3 cp lambda.zip s3://DEPLOY_BUCKET/lambda.zip
```

Kiểm tra upload thành công:

```bash
aws s3 ls s3://DEPLOY_BUCKET/
```

Bạn sẽ thấy `lambda.zip` được liệt kê với kích thước và timestamp upload.

---

## Tóm tắt

Kết thúc giai đoạn này bạn đã:

- [ ] Cài đặt đầy đủ Node.js dependencies
- [ ] Tạo deployment S3 bucket (`DEPLOY_BUCKET`)
- [ ] Đóng gói `lambda.zip` từ `src/` + `node_modules/`
- [ ] Upload `lambda.zip` lên `s3://DEPLOY_BUCKET/lambda.zip`

Tiếp tục sang **Giai đoạn 2: Triển khai hạ tầng AWS**.
