---
title: "Sự kiện 1"
date: 2026-01-27
weight: 3
chapter: false
pre: " <b> 4.1. </b> "
---

# Báo cáo Tổng kết: “AWS RE:INVENT 2025 RECAP (VIETNAM)”

### Chi tiết Sự kiện
- **Ngày & Giờ:** 27 tháng 1, 2026
- **Địa điểm:** Văn phòng AWS Việt Nam (Tầng 26 & 36), TP. Hồ Chí Minh
- **Vai trò:** Người tham dự (Thực tập sinh Cloud tại FCJ)

### Mục tiêu Sự kiện

- Cập nhật các công bố quan trọng nhất từ AWS re:Invent 2025 (Las Vegas).
- Tìm hiểu sâu về Generative AI, cụ thể là Agentic AI và Amazon Bedrock.
- Khám phá các tối ưu hóa mới về lưu trữ dữ liệu và hạ tầng (SageMaker, S3).
- Kết nối với các Kiến trúc sư Giải pháp (Solution Architects) của AWS và cộng đồng công nghệ tại địa phương.

### Diễn giả

- **Mr. Thi** – Solution Architect (Chủ đề: Generative AI & Agents)
- **Mr. Tung** – Speaker (Chủ đề: OpenSearch & Agentic Search)
- **Đội ngũ AWS Solution Architects & Quản lý khách hàng**

### Các nội dung nổi bật

#### Phiên 1: Generative AI & Agents
- **Amazon Nova Models**: Giới thiệu các mô hình nền tảng (foundation models) hiệu suất cao mới.
- **Bedrock Agents**: Đi sâu vào khả năng Điều phối (Orchestration), Luồng công việc (Flows), cùng các tính năng mới về **Bộ nhớ (Memory)** và **Hàng rào bảo vệ (Guardrails)**.
- **Agentic AI**: Chuyển dịch từ các chatbot đơn giản sang các tác nhân tự hành (autonomous agents) có khả năng thực hiện quy trình công việc đa bước.

#### Phiên 2: SageMaker Unified Studio & Cập nhật S3
- **Unified Studio**: Một môi trường phát triển (IDE) duy nhất giúp kết nối các Kỹ sư Dữ liệu, Nhà khoa học Dữ liệu và Kỹ sư AI.
- **S3 Tables**: Hỗ trợ định dạng bảng Apache Iceberg nguyên bản ngay trong S3.
- **S3 Vector**: Tính năng mới cho phép lưu trữ vector trực tiếp trên S3, giúp giảm đáng kể chi phí.

#### Phiên 3 & 4: Tìm kiếm & AI Đa phương thức (Multimodal)
- **OpenSearch Serverless**: Tích hợp với MCP (Model Context Protocol) và Bộ nhớ Tác nhân (Agentic Memory).
- **Nova Multimodal Embeddings**: Chuyển đổi video và hình ảnh thành vector để phục vụ tìm kiếm.
- **Bedrock Data Automation**: Tự động trích xuất thông tin chuyên sâu từ các nội dung đa phương tiện.

#### Phiên 5: Hạ tầng AI
- **SageMaker HyperPod**: Quản lý nâng cao cho các cụm GPU quy mô lớn.
- **SageMaker MLflow**: Quản lý toàn bộ vòng đời cho các dự án Machine Learning.

### Các bài học kinh nghiệm

#### Tương lai thuộc về "Agentic"
- **Quy trình tự hành**: Sự chuyển dịch từ "Kỹ thuật Prompt" sang "Kỹ thuật Tác nhân" (Agent Engineering). Các tác nhân có bộ nhớ có thể duy trì ngữ cảnh và thực hiện các nhiệm vụ phức tạp mà không cần can thiệp liên tục.
- **Hàng rào bảo vệ (Guardrails) là thiết yếu**: Khi các tác nhân trở nên tự hành hơn, việc thiết lập các chính sách nghiêm ngặt và hàng rào bảo mật là bắt buộc.

#### Tối ưu hóa Dữ liệu & Tính toán
- **Hiệu quả chi phí**: S3 Vector là một yếu tố thay đổi cuộc chơi cho các dự án yêu cầu tìm kiếm vector (như RAG) nhưng có ngân sách hạn chế.
- **Sự cộng tác**: SageMaker Unified Studio giúp tinh giản quy trình làm việc giữa khâu chuẩn bị dữ liệu và huấn luyện mô hình.

### Ứng dụng vào công việc

- **Tích hợp dự án (Nền tảng Bảo mật)**:
    - Đánh giá **Bedrock Agents** để tự động hóa quy trình "quét lỗ hổng" (ví dụ: một tác nhân tự chạy quét, phân tích log và dự thảo báo cáo).
    - Triển khai **S3 Vector** để lưu trữ log và các dấu hiệu lỗ hổng (signatures) một cách hiệu quả cho backend của dự án.
- **Cải thiện kiến trúc**: Cân nhắc áp dụng **Cognito** để quản lý người dùng dựa trên các thảo luận với đội ngũ SA.
- **Thực hành tốt nhất (Best Practices)**: Áp dụng tư duy "Ưu tiên Serverless" (Serverless first) học được từ các phiên kết nối.

### Trải nghiệm Sự kiện

Tham dự **“AWS re:Invent 2025 Recap”** tại văn phòng AWS Việt Nam là một cột mốc quan trọng trong quá trình thực tập của tôi:

#### Mở rộng tầm nhìn
- Các phiên thảo luận đã làm rõ rằng **Agentic AI** chính là tương lai gần. Việc xem demo về Flow Agent đã gợi cảm hứng cho các tính năng báo cáo trong dự án của chúng tôi.
- Hiểu về **Multimodal RAG** đã giúp tôi nhận ra các khả năng vượt xa việc xử lý văn bản thuần túy—điều này rất cần thiết cho các giai đoạn tương lai của Nền tảng Bảo mật khi cần phân tích ảnh chụp màn hình.

#### Xác thực kỹ thuật
- Sự ra đời của **S3 Vector** đã xác nhận nhu cầu của nhóm chúng tôi về một giải pháp lưu trữ chi phí thấp cho việc phân tích log.
- Kết nối với **Mr. Thi** và các SA khác đã giúp làm rõ những thắc mắc về **Chính sách IAM** và các thực hành **Serverless** tốt nhất mà tôi từng gặp khó khăn trong Tuần 3.

#### Cộng đồng & Kết nối
- Không khí tại văn phòng AWS rất năng động với các nhà phát triển chia sẻ về những thách thức thực tế.
- Tôi đã có cơ hội thảo luận về dự án nhóm **“TheBois”** với các chuyên gia trong ngành và nhận được những phản hồi giá trị về kiến trúc đề xuất.


> Tóm lại, sự kiện này không chỉ cập nhật kiến thức kỹ thuật mà còn cung cấp các công cụ cụ thể (Agents, S3 Vector) mà tôi có thể áp dụng ngay lập tức vào **Nền tảng Đánh giá Cơ sở Bảo mật Website (Website Security Baseline Assessment Platform)**.