---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# WebQuiz - Triển khai nền tảng trắc nghiệm thời gian thực Serverless trên AWS

#### Tổng quan

**WebQuiz** là một ứng dụng trắc nghiệm thời gian thực được thiết kế để mô phỏng các trò chơi tương tác trực tiếp (tương tự như Kahoot). Trong bài thực hành này, bạn sẽ tự tay xây dựng và triển khai một hạ tầng serverless hoàn chỉnh trên AWS, có khả năng co giãn linh hoạt và tối ưu hóa chi phí vận hành.

Bạn sẽ học cách:
* Cấu hình **Amazon Cognito** để đăng ký người dùng và đăng nhập quản trò bảo mật.
* Thiết kế và tạo các bảng **Amazon DynamoDB** lưu trữ thông tin và quản lý trạng thái trò chơi trực tiếp (thay thế cho Redis).
* Lập trình các hàm **AWS Lambda** để xử lý API REST, xử lý kết nối WebSocket, tính điểm và lưu kết quả chung cuộc.
* Cấu hình **Amazon API Gateway** (cả HTTP API và WebSocket API) cung cấp các endpoint kết nối độ trễ thấp giữa frontend và backend.
* Định tuyến các sự kiện chơi game bất đồng bộ sử dụng **Amazon EventBridge**.
* Triển khai frontend tĩnh lên **Amazon S3** phân phối toàn cầu qua **Amazon CloudFront** với cơ chế bảo mật **Origin Access Control (OAC)**.
* Thiết lập **Amazon CloudWatch** logs, chỉ số metrics, alarms cảnh báo lỗi và Dashboard trực quan để theo dõi sức khỏe hệ thống.

#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Xác thực Cognito](5.3-Cognito/)
4. [Bảng dữ liệu DynamoDB](5.4-DynamoDB/)
5. [Quyền IAM & EventBridge](5.5-IAM-EventBridge/)
6. [Hàm Lambda REST API](5.6-Lambda-REST/)
7. [Hàm Lambda Realtime](5.7-Lambda-Realtime/)
8. [API Gateway - HTTP API](5.8-APIGateway-HTTP/)
9. [API Gateway - WebSocket API](5.9-APIGateway-WebSocket/)
10. [Lưu trữ Frontend với S3 & CloudFront](5.10-Frontend-Hosting/)
11. [Giám sát CloudWatch & Dashboard](5.11-Monitoring-Dashboard/)
12. [Kiểm thử hệ thống](5.12-Testing/)
13. [Dọn dẹp tài nguyên](5.13-Cleanup/)