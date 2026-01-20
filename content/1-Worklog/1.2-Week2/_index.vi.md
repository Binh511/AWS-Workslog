---
title: "Worklog Tuần 2"
date: 2026-01-18
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu tuần 2:

* Hiểu và nắm vững các dịch vụ Storage & Database trên AWS.
* Biết cách cấu hình, quản lý và áp dụng các dịch vụ lưu trữ trong thực tế.
* Thực hành xây dựng hệ thống lưu trữ dữ liệu đơn giản trên AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tổng quan về AWS Storage & Database <br> - Tìm hiểu vai trò và use case của S3, EBS, RDS | 18/01/2026 | 18/01/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Tìm hiểu Amazon S3 <br>&emsp; + Bucket & Object <br>&emsp; + Storage Classes <br>&emsp; + Lifecycle Policies | 18/01/2026 | 18/01/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Tìm hiểu Amazon EBS <br>&emsp; + Các loại volume <br>&emsp; + Gắn / tháo volume với EC2 <br>&emsp; + Snapshot & backup | 18/01/2026 | 18/01/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Tìm hiểu Amazon RDS <br>&emsp; + Các engine cơ sở dữ liệu <br>&emsp; + Cấu hình instance <br>&emsp; + Backup & security | 18/01/2026 | 18/01/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - **Thực hành:** <br>&emsp; + Tạo S3 bucket <br>&emsp; + Cấu hình lifecycle policy <br>&emsp; + Xây dựng hệ thống upload file đơn giản sử dụng S3 | 18/01/2026 | 18/01/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 2:

* Nắm được kiến thức tổng quan về các dịch vụ Storage & Database trên AWS, bao gồm:
  * Amazon S3
  * Amazon EBS
  * Amazon RDS

* Đã tạo và quản lý thành công Amazon S3 bucket:
  * Upload và quản lý object
  * Cấu hình lifecycle policy để tối ưu chi phí lưu trữ
  * Hiểu sự khác nhau giữa các storage class

* Hiểu và thực hành với Amazon EBS:
  * Tạo và gắn EBS volume vào EC2
  * Tạo snapshot để backup và phục hồi dữ liệu
  * Nắm được tính bền vững của dữ liệu trên EBS

* Đã thiết lập và quản lý Amazon RDS:
  * Lựa chọn database engine phù hợp
  * Cấu hình instance, storage và security group
  * Hiểu cơ chế backup tự động và bảo trì cơ bản

* Hoàn thành bài thực hành xây dựng hệ thống upload file đơn giản sử dụng Amazon S3, giúp hiểu rõ hơn về:
  * Cách tích hợp ứng dụng với dịch vụ lưu trữ đám mây
  * Phân quyền truy cập dữ liệu
  * Luồng hoạt động của hệ thống lưu trữ trên AWS

* Củng cố khả năng kết hợp các dịch vụ Storage, Database với các dịch vụ Compute trong kiến trúc AWS thực tế.
