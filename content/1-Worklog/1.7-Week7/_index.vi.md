---
title: "Worklog Tuần 7"
date: 2024-03-02
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

## Tuần 7: Containers & Orchestration

### Mục tiêu tuần 7

* Hiểu khái niệm Container và sự khác biệt giữa Container với Virtual Machine.
* Làm quen với Docker và quy trình đóng gói ứng dụng.
* Tìm hiểu các dịch vụ container trên AWS.
* Triển khai một ứng dụng web dạng container lên môi trường cloud.

---

## Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|----------|-------------|----------------|---------------|
| 2 | Tìm hiểu kiến thức cơ bản về Docker: Image, Container, Dockerfile, Volume, Port Mapping | 02/03/2026 | 02/03/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | Thực hành build Docker image và chạy container trên máy local | 02/03/2026 | 02/03/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Tìm hiểu Amazon ECR (Elastic Container Registry) | 02/03/2025 | 02/03/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Tìm hiểu Amazon ECS (Cluster, Task Definition, Service) | 02/03/2026 | 02/03/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Thực hành triển khai ứng dụng web dạng container trên ECS | 02/08/2026 | 02/03/2026 | https://cloudjourney.awsstudygroup.com/ |

---

## Nội dung đã học và thực hành

### 1. Docker cơ bản

* Hiểu cơ chế hoạt động của container.
* Phân biệt Container và Virtual Machine.
* Tìm hiểu các thành phần chính:
  * Docker Image
  * Docker Container
  * Dockerfile
  * Volume
  * Port Mapping
* Thực hành:
  * Build image từ Dockerfile
  * Chạy container trên máy local
  * Kiểm tra log và trạng thái container

---

### 2. Amazon ECR (Elastic Container Registry)

* Tạo repository lưu trữ Docker image.
* Gắn tag và push image từ local lên ECR.
* Quản lý version image trong registry.

---

### 3. Amazon ECS (Elastic Container Service)

* Hiểu kiến trúc ECS:
  * Cluster
  * Task Definition
  * Task
  * Service
* Phân biệt giữa EC2 Launch Type và Fargate.
* Tạo Cluster và Service để chạy container trên AWS.

---

## Kết quả đạt được tuần 7

* Nắm vững quy trình đóng gói ứng dụng bằng Docker.
* Build và chạy thành công container trên máy local.
* Push Docker image lên Amazon ECR thành công.
* Tạo ECS Cluster và triển khai service chạy container.
* Triển khai thành công ứng dụng web dạng container trên AWS.
* Hiểu cơ bản về cơ chế orchestration và quản lý container trong môi trường cloud.

---

## Nhận xét cá nhân

Tuần 7 giúp mình hiểu rõ cách các ứng dụng hiện đại được đóng gói và triển khai bằng container. Việc sử dụng Docker kết hợp với ECS giúp việc triển khai ứng dụng trở nên linh hoạt, dễ mở rộng và phù hợp với mô hình DevOps hiện đại.