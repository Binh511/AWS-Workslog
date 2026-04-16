---
title: "Event 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Event 3:Secure Hybrid Access to S3 using VPC Endpoints

### Event Information

- **Event Name:** Secure Hybrid Access to S3 using VPC Endpoints
- **Date & Time:** 9:00 sáng, ngày 4 tháng 4
- **Location:** Hội trường Academy, Đại học FPT
- **Role:** Attendee

---

## Event Description

Buổi workshop này tập trung vào cách truy cập Amazon S3 một cách an toàn từ nhiều môi trường khác nhau (cloud và on-premises) thông qua VPC Endpoints.

Nội dung buổi học nhấn mạnh cách các kiến trúc cloud hiện đại có thể tránh việc truyền dữ liệu qua Internet công cộng, đồng thời vẫn đảm bảo hiệu năng và tính bảo mật trong giao tiếp giữa các dịch vụ.

---

## Main Content

### 1. Tổng quan về kết nối riêng (Private Connectivity)

Workshop giới thiệu về **AWS PrivateLink**, một giải pháp cho phép kết nối riêng giữa VPC và các dịch vụ AWS mà không cần đi qua Internet công cộng.

Ý tưởng chính:

- Giữ toàn bộ lưu lượng trong mạng AWS
- Tăng cường bảo mật và giảm rủi ro bị lộ dữ liệu
- Đảm bảo kết nối ổn định và độ trễ thấp

---

### 2. Các loại VPC Endpoint

Hai loại endpoint chính được trình bày:

#### Gateway Endpoint

- Dùng cho **Amazon S3 và DynamoDB**
- Điều hướng traffic thông qua **route table**
- Không cần sử dụng IP public

#### Interface Endpoint

- Sử dụng **Elastic Network Interface (ENI)**
- Hỗ trợ nhiều dịch vụ AWS hơn
- Sử dụng cơ chế **DNS resolution**
- Phù hợp với hệ thống hybrid hoặc on-premises

---

### 3. Truy cập S3 từ VPC

Các bước được thực hiện trong workshop:

- Tạo **Gateway Endpoint**
- Cấu hình route table
- Kiểm tra kết nối từ EC2 đến S3

Kết quả:

- EC2 có thể truy cập S3 hoàn toàn nội bộ
- Không cần Internet Gateway

---

### 4. Truy cập S3 từ hệ thống On-Premises

Workshop cũng mô phỏng mô hình hybrid:

- Hệ thống on-premises kết nối với AWS thông qua VPC
- Sử dụng **Interface Endpoint**
- Cấu hình DNS để phân giải dịch vụ

Cách này giúp hệ thống bên ngoài có thể truy cập dịch vụ AWS một cách an toàn.

---

### 5. VPC Endpoint Policies

- Cho phép kiểm soát truy cập chi tiết
- Giới hạn tài nguyên được phép truy cập
- Áp dụng nguyên tắc **least privilege** để tăng cường bảo mật

---

## Key Takeaways

Sau buổi workshop, tôi rút ra được rằng:

- VPC Endpoint là thành phần quan trọng trong kiến trúc cloud an toàn
- Có thể truy cập dịch vụ AWS mà không cần sử dụng Internet công cộng
- Gateway và Interface Endpoint phục vụ các mục đích khác nhau
- Kiến trúc hybrid yêu cầu cấu hình DNS và network hợp lý
- Có thể tăng cường bảo mật bằng cách sử dụng endpoint policies

---

## Personal Experience

Buổi workshop giúp tôi hiểu rõ hơn về cách hoạt động của hệ thống mạng trong AWS, đặc biệt là trong các môi trường doanh nghiệp yêu cầu bảo mật cao.

Tôi thấy nội dung rất thực tế vì liên quan trực tiếp đến các hệ thống lớn, nơi mà bảo mật và quyền riêng tư dữ liệu là yếu tố quan trọng.

Những kiến thức này rất hữu ích cho dự án hiện tại của tôi, đặc biệt khi làm việc với các dịch vụ như S3 và thiết kế kiến trúc hệ thống an toàn.

---

## Conclusion

Sự kiện này cung cấp kiến thức thực tế về cách xây dựng kiến trúc hệ thống an toàn và có khả năng mở rộng bằng cách sử dụng VPC Endpoints.

Nó giúp tôi hiểu rõ hơn về networking trong AWS và có cái nhìn tổng quan hơn trong việc thiết kế hệ thống hiệu quả và bảo mật trên cloud.

---

## Event Photos

![Event](/event31.jpg)
![Event](/event32.jpg)
![Event](/event33.jpg)
![Event](/event34.jpg)
![Event](/event35.jpg)
![Event](/event36.jpg)
