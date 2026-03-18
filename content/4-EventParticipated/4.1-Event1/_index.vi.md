## title: "AWS re:Invent 2025 Recap (Vietnam)" date: 2026-01-27 weight: 1 chapter: false pre: " \<b\> 4.1. \</b\> "

{{% notice warning %}}
⚠️ **Note:** Thông tin dưới đây chỉ mang tính chất tham khảo. Vui lòng **không sao chép nguyên văn** vào báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}

# Summary Report: “AWS re:Invent 2025 Recap (Vietnam)”

### Thông tin sự kiện

  * **Thời gian:** 27 tháng 01, 2026
  * **Địa điểm:** Văn phòng AWS Việt Nam (Tầng 26 & 36), TP. Hồ Chí Minh
  * **Vai trò:** Người tham dự (FCJ Cloud Intern)

### Event Objectives

  - Cập nhật các công bố quan trọng nhất từ sự kiện AWS re:Invent 2025 (Las Vegas).
  - Tìm hiểu sâu về Generative AI, đặc biệt là Agentic AI và Amazon Bedrock.
  - Khám phá các tối ưu hóa mới về lưu trữ dữ liệu và hạ tầng (SageMaker, S3).
  - Kết nối với các chuyên gia kiến trúc giải pháp (SA) của AWS và cộng đồng công nghệ tại địa phương.

### Speakers

  - **Mr. Thi** – Solution Architect (Chủ đề: Generative AI & Agents)
  - **Mr. Tung** – Speaker (Chủ đề: OpenSearch & Agentic Search)
  - **Các Solution Architects và Account Managers khác từ AWS.**

### Key Highlights

#### Generative AI & Agents

  - **Amazon Nova Models:** Giới thiệu các mô hình nền tảng (Foundation Models) hiệu suất cao mới.
  - **Bedrock Agents:** Đi sâu vào khả năng điều phối (Orchestration), luồng xử lý (Flows) cùng các tính năng **Memory** và **Guardrails** mới.
  - **Agentic AI:** Xu hướng chuyển dịch từ chatbot đơn thuần sang các agent tự trị có khả năng thực thi các quy trình làm việc đa bước.

#### SageMaker Unified Studio & S3 Updates

  - **Unified Studio:** Một môi trường IDE duy nhất kết nối Data Engineers, Data Scientists và AI Engineers, giúp phá bỏ rào cản giữa các bộ phận.
  - **S3 Tables:** Hỗ trợ lưu trữ định dạng bảng Apache Iceberg trực tiếp trên S3.
  - **S3 Vector:** Tính năng mới cho phép lưu trữ vector gốc ngay trên S3, giúp giảm chi phí đáng kể so với các cơ sở dữ liệu vector chuyên dụng.

#### Search & Multimodal AI

  - **OpenSearch Serverless:** Tích hợp với MCP (Model Context Protocol) và bộ nhớ Agentic.
  - **Nova Multimodal Embeddings:** Chuyển đổi video và hình ảnh thành vector để phục vụ tìm kiếm.
  - **Bedrock Data Automation:** Tự động trích xuất thông tin chuyên sâu từ các nội dung đa phương tiện.

#### AI Infrastructure

  - **SageMaker HyperPod:** Quản lý nâng cao cho các cụm GPU quy mô lớn.
  - **SageMaker MLflow:** Quản lý toàn bộ vòng đời cho các dự án Machine Learning.

### Key Takeaways

#### Tương lai của Agentic AI

  - **Chuyển dịch tư duy:** Sự thay đổi từ "Prompt Engineering" sang "Agent Engineering". Các Agent có bộ nhớ giúp duy trì ngữ cảnh theo thời gian và thực hiện các tác vụ phức tạp mà không cần sự can thiệp liên tục của con người.
  - **Tầm quan trọng của Guardrails:** Khi các Agent trở nên tự trị hơn, các chính sách bảo mật và rào cản kỹ thuật là yếu tố bắt buộc để đảm bảo vận hành an toàn.

#### Tối ưu hóa Dữ liệu & Tính toán

  - **Hiệu quả chi phí:** S3 Vector là giải pháp thay đổi cuộc chơi cho các dự án RAG (Retrieval-Augmented Generation) với ngân sách hạn chế.
  - **Sự cộng tác:** SageMaker Unified Studio giúp tinh gọn quy trình làm việc, cho phép chuẩn bị dữ liệu và huấn luyện mô hình trong cùng một môi trường.

### Applying to Work

  - **Tích hợp dự án (Security Platform):**
      - Đánh giá việc sử dụng **Bedrock Agents** để tự động hóa quy trình "vulnerability scanning" (quét lỗ hổng bảo mật).
      - Triển khai **S3 Vector** để lưu trữ nhật ký (logs) và các chữ ký lỗ hổng bảo mật hiệu quả cho backend của dự án.
  - **Cải thiện kiến trúc:** Cân nhắc áp dụng **Cognito** để quản lý người dùng dựa trên các tư vấn từ SA.
  - **Best Practices:** Áp dụng tư duy **"Serverless first"** để giữ cho hạ tầng dự án tinh gọn và tối ưu.

### Event Experience

Việc tham dự **“AWS re:Invent 2025 Recap”** tại văn phòng AWS Việt Nam là một cột mốc quan trọng trong quá trình thực tập của tôi. Những trải nghiệm chính bao gồm:

#### Mở rộng tầm nhìn công nghệ

  - Các phiên thảo luận đã làm rõ rằng **Agentic AI** là tương lai gần. Việc chứng kiến demo một Flow Agent phân tích dữ liệu bán hàng đã gợi cảm hứng cho các tính năng báo cáo trong dự án của chúng tôi.
  - Hiểu về **Multimodal RAG** giúp tôi nhận ra tiềm năng phân tích ảnh chụp màn hình cho các giai đoạn tương lai của nền tảng bảo mật.

#### Xác thực kỹ thuật

  - Sự ra đời của **S3 Vector** đã xác nhận nhu cầu của nhóm về một giải pháp lưu trữ tiết kiệm chi phí cho việc phân tích log.
  - Kết nối với Mr. Thi và các SA khác giúp giải đáp các thắc mắc về **IAM Policies** và các phương pháp vận hành **Serverless** tốt nhất mà tôi từng gặp khó khăn trước đó.

#### Kết nối cộng đồng

  - Không khí tại văn phòng AWS (Tầng 26 & 36) rất năng động với các nhà phát triển cùng chia sẻ những thách thức thực tế.
  - Tôi đã có cơ hội thảo luận về dự án nhóm **“TheBois”** với các chuyên gia trong ngành và nhận được phản hồi giá trị về kiến trúc đề xuất.

#### Một số hình ảnh tại sự kiện

*Thêm hình ảnh sự kiện của bạn tại đây*

> Nhìn chung, sự kiện này không chỉ cập nhật kiến thức kỹ thuật mà còn cung cấp những công cụ cụ thể (Agents, S3 Vector) mà tôi có thể áp dụng ngay lập tức vào dự án **Website Security Baseline Assessment Platform**.

-----
