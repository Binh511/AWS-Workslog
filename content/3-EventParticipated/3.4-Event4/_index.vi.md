---
title: "Event 4"
date: 2026-04-10
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# Event 4: AWS Networking, Security , Identity & Access Management

### Event Information

- **Event Name:** AWS Networking, Security & IAM Workshop
- **Date & Time:** 9:00 sáng, ngày 11 tháng 4
- **Location:** Hội trường Academy, Đại học FPT
- **Role:** Attendee

---

## Event Description

Buổi workshop này cung cấp cái nhìn tổng quan về các khái niệm cốt lõi trong AWS, tập trung vào networking, bảo mật và quản lý truy cập.

Nội dung buổi học giúp người tham gia hiểu cách thiết kế kiến trúc cloud an toàn và có khả năng mở rộng bằng cách kết hợp các thành phần mạng, cơ chế kiểm soát truy cập và các dịch vụ bảo mật trong AWS.

---

## Main Content

### 1. Identity & Access Management (IAM)

Workshop giới thiệu **AWS IAM**, một dịch vụ dùng để kiểm soát quyền truy cập vào tài nguyên AWS.

#### Khái niệm chính

- Quản lý **user, group và role**
- Kiểm soát **xác thực (authentication)** và **phân quyền (authorization)**
- Áp dụng quyền thông qua **policies**

#### Best Practices

- Áp dụng nguyên tắc **least privilege (cấp quyền tối thiểu)**
- Không sử dụng tài khoản root cho các thao tác hằng ngày
- Bật **MFA (xác thực đa yếu tố)**
- Thay đổi thông tin xác thực định kỳ
- Hạn chế sử dụng access key lâu dài

#### Nội dung nâng cao

- **Single Sign-On (SSO):** Đăng nhập một lần cho nhiều hệ thống
- **Service Control Policies (SCP):** Giới hạn quyền tối đa giữa các account
- **Permission Boundaries:** Giới hạn quyền cho user/role cụ thể

---

### 2. AWS Networking

Phần này tập trung vào cách hệ thống mạng hoạt động trong AWS thông qua **VPC (Virtual Private Cloud)**.

#### Thành phần chính

- **VPC & CIDR:** Xác định dải địa chỉ mạng
- **Subnets:** Chia nhỏ hệ thống mạng
- **Internet Gateway (IGW):** Cho phép kết nối Internet
- **Route Tables:** Điều hướng traffic

#### NAT Gateway

- Cho phép instance trong private subnet truy cập Internet
- Ngăn chặn traffic từ Internet vào
- Sử dụng cơ chế **SNAT và PAT**

#### Các lớp bảo mật

- **Security Group (SG):**
  - Áp dụng ở cấp instance
  - Stateful
  - Chỉ cho phép rule dạng allow

- **Network ACL (NACL):**
  - Áp dụng ở cấp subnet
  - Stateless
  - Hỗ trợ cả allow và deny

---

### 3. AWS Security Services

Workshop cũng giới thiệu các dịch vụ bảo mật quan trọng trong AWS:

#### AWS WAF (Web Application Firewall)

- Bảo vệ ứng dụng khỏi các tấn công phổ biến như:
  - SQL Injection
  - Cross-Site Scripting (XSS)
- Lọc request HTTP/HTTPS

#### AWS Shield

- Bảo vệ khỏi các cuộc tấn công **DDoS**
- Gồm 2 phiên bản:
  - Standard (miễn phí)
  - Advanced (bảo vệ nâng cao)

#### AWS Network Firewall

- Cung cấp bảo mật ở cấp network trong VPC
- Hỗ trợ:
  - Lọc traffic stateful và stateless
  - Phát hiện và ngăn chặn xâm nhập

#### AWS Firewall Manager

- Quản lý bảo mật tập trung
- Tự động áp dụng rule cho nhiều account AWS

---

## Key Takeaways

Sau buổi workshop, tôi rút ra được rằng:

- IAM đóng vai trò quan trọng trong việc quản lý truy cập an toàn
- Các thành phần mạng như VPC, Subnet và NAT Gateway là nền tảng của kiến trúc cloud
- Bảo mật cần được triển khai ở nhiều lớp (application, network và access control)
- AWS cung cấp hệ sinh thái đầy đủ để bảo vệ hệ thống khỏi các mối đe dọa
- Áp dụng best practices giúp cải thiện đáng kể độ an toàn và độ tin cậy của hệ thống

---

## Personal Experience

Buổi workshop giúp tôi hiểu rõ hơn cách các thành phần trong AWS phối hợp với nhau để xây dựng một hệ thống an toàn.

Tôi đặc biệt thấy phần IAM và networking rất hữu ích vì liên quan trực tiếp đến dự án hiện tại khi làm việc với API, xác thực người dùng và hạ tầng cloud.

Ngoài ra, việc tìm hiểu về các dịch vụ bảo mật của AWS giúp tôi tự tin hơn khi thiết kế các hệ thống có thể xử lý các vấn đề bảo mật trong thực tế.

---

## Conclusion

Sự kiện này cung cấp nền tảng vững chắc về networking, bảo mật và quản lý truy cập trong AWS.

Nó giúp tôi nâng cao khả năng thiết kế hệ thống cloud an toàn, có khả năng mở rộng và hoạt động hiệu quả.

---

## Event Photos

![Event](/event41.jpg)
![Event](/event42.jpg)
![Event](/event43.jpg)
![Event](/event44.jpg)
![Event](/event45.jpg)
