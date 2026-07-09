---
title: "Giới thiệu"
date: 2024-01-01
weight: 1
pre: " <b> 5.1. </b> "
---

# Giới thiệu

### Giới thiệu về ứng dụng WebQuiz
**WebQuiz** là một ứng dụng trắc nghiệm trực tuyến tương tác thời gian thực đa nhiệm (tương tự như Kahoot) có khả năng co giãn linh hoạt, bảo mật tốt và tối ưu hóa chi phí vận hành. Mục tiêu chính của workshop này là hướng dẫn bạn từng bước tự xây dựng và triển khai **kiến trúc Serverless hoàn chỉnh** cho hệ thống này trên **AWS** bằng cách kết hợp các dịch vụ tính toán theo yêu cầu (**on-demand**), cơ sở dữ liệu **NoSQL tự động co giãn**, cơ chế **định tuyến sự kiện bất đồng bộ** và hệ thống truyền thông điệp **hai chiều độ trễ cực thấp**.

### Tổng quan về workshop
Trong workshop này, bạn sẽ thực hành xây dựng hệ thống với các thành phần cốt lõi:

*   **Tầng Web Frontend:** Ứng dụng giao diện tĩnh (**Single Page Application - SPA**) được lưu trữ trên **Amazon S3** (**Static Website Hosting**), phân phối toàn cầu thông qua mạng phân phối nội dung **Amazon CloudFront** sử dụng cơ chế bảo mật **Origin Access Control (OAC)** để chặn truy cập trực tiếp vào S3.
*   **Tầng Xác thực & Bảo mật:** Quản lý đăng ký người dùng và đăng nhập quản trò (host) bảo mật sử dụng **Amazon Cognito User Pool**. Các mã token **JSON Web Tokens (JWT)** do Cognito cấp được xác minh tự động ở **API Gateway** để bảo vệ tài nguyên.
*   **Tầng Giao tiếp & API Gateway:** Thiết lập **API Gateway HTTP API** cho các request REST thông thường (quản lý bộ câu hỏi, phòng chơi) và **API Gateway WebSocket API** để duy trì kết nối **hai chiều thời gian thực (real-time)** với độ trễ cực thấp giữa quản trò và người chơi trong trận đấu.
*   **Tầng Tính toán (Compute API):** Lập trình và cấu hình các hàm **AWS Lambda** thực thi mã nguồn backend theo yêu cầu (**Node.js runtime**) xử lý các nghiệp vụ CRUD câu hỏi, quản lý trạng thái phòng đấu, xử lý kết nối/ngắt kết nối WebSocket và phát tin nhắn thời gian thực.
*   **Tầng Cơ sở dữ liệu (Database Tier):** Sử dụng cơ sở dữ liệu NoSQL **Amazon DynamoDB** chạy ở chế độ **On-demand** (co giãn theo yêu cầu và tối ưu chi phí) để lưu trữ thông tin người dùng, câu hỏi, kết nối và quản lý trạng thái trò chơi trực tiếp thay thế cho **Redis**.
*   **Tầng Định tuyến Sự kiện bất đồng bộ (Event Routing):** Sử dụng **Amazon EventBridge Custom Event Bus** (`webquiz-dev-game-events`) để định tuyến các sự kiện chơi game (người chơi nộp bài, kết thúc game) bất đồng bộ sang các hàm Lambda xử lý nền (tính điểm, lưu kết quả chung cuộc).
*   **Dịch vụ Giám sát & Dashboard:** Thiết lập **Amazon CloudWatch** logs ghi nhận lỗi, CloudWatch metrics đo lường hiệu năng, **CloudWatch Alarms** gửi cảnh báo khi có sự cố và xây dựng **CloudWatch Dashboard** để giám sát toàn diện sức khỏe hệ thống theo thời gian thực.
