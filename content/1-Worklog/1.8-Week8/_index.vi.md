---

title: "Worklog Tuần 8"
date: 2024-03-18
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
----------------------

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu tuần 8:

* Hiểu về Monitoring & Logging trong AWS.
* Nắm cách sử dụng CloudWatch, CloudTrail và X-Ray.
* Thực hành thiết lập hệ thống giám sát cơ bản.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                          |
| --- | --------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------- |
| 2   | - Tìm hiểu tổng quan Monitoring & Logging <br> - Hiểu khái niệm observability trong hệ thống                                      | 18/03/2026   | 18/03/2026      |                                         |
| 3   | - Tìm hiểu CloudWatch: <br>  + Metrics <br>  + Logs <br>  + Alarms                                                                | 18/03/2026   |  18/03/2026      | https://cloudjourney.awsstudygroup.com/ |
| 4   | - **Thực hành CloudWatch:** <br>  + Tạo metrics <br>  + Tạo alarm (CPU, status check)                                             | 18/03/2026   | 18/03/2026      | https://cloudjourney.awsstudygroup.com/ |
| 5   | - Tìm hiểu CloudTrail: <br>  + Event history <br>  + Audit logging                                                                | 18/03/2026   | 18/03/2026      | https://cloudjourney.awsstudygroup.com/ |
| 6   | - Tìm hiểu X-Ray: <br>  + Distributed tracing <br>  + Phân tích luồng request <br> - **Thực hành:** thiết lập monitoring tổng thể | 18/03/2026   | 18/03/2026      | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 8:

* Hiểu các khái niệm Monitoring & Logging trong AWS:

  * Metrics
  * Logs
  * Tracing
  * ...

* Sử dụng CloudWatch để giám sát hệ thống:

  * Theo dõi các metrics quan trọng (CPU, status check, ...)
  * Tạo cảnh báo (alarm) khi hệ thống có dấu hiệu bất thường
  * Làm quen với việc phân tích log
  * ...

* Nắm được cách sử dụng CloudTrail:

  * Xem lịch sử sự kiện (event history)
  * Theo dõi các hoạt động API trong hệ thống
  * Hỗ trợ audit và kiểm tra bảo mật
  * ...

* Hiểu và áp dụng distributed tracing với X-Ray:

  * Quan sát luồng request giữa các service
  * Phân tích đường đi của request trong hệ thống
  * Xác định các điểm gây độ trễ (latency)
  * ...

* Xây dựng được hệ thống giám sát cơ bản bằng cách kết hợp nhiều dịch vụ AWS:

  * Kết hợp CloudWatch + CloudTrail + X-Ray
  * Cải thiện khả năng theo dõi và phát hiện sự cố
  * ...

* ...
