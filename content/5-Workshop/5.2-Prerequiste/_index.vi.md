---
title: "Chuẩn bị"
date: 2024-01-01
weight: 2
pre: " <b> 5.2. </b> "
---

# Chuẩn bị môi trường & Công cụ

Trước khi bắt đầu thực hành triển khai WebQuiz, hãy đảm bảo bạn đã chuẩn bị đầy đủ các tài nguyên và công cụ sau:

### 1. Tài khoản AWS & Quyền hạn

*   **Tài khoản AWS:** Bạn cần có một tài khoản AWS đang hoạt động. Nếu chưa có, đăng ký tại [aws.amazon.com](https://aws.amazon.com/).
*   **Quyền truy cập:** Đảm bảo tài khoản IAM của bạn có quyền Administrator hoặc đủ quyền tạo Cognito User Pools, DynamoDB Tables, Lambda Functions, IAM Roles, EventBridge rules, API Gateways, S3 Buckets, và CloudFront.

---

### 2. Cài đặt công cụ ở máy cục bộ (Local)

*   **Node.js & npm:** Cài đặt Node.js (khuyến nghị phiên bản 20.x). Kiểm tra phiên bản bằng lệnh:
    ```bash
    node -v
    npm -v
    ```
*   **AWS CLI:** Cài đặt AWS CLI và cấu hình tài khoản truy cập:
    ```bash
    aws configure
    ```
    Hãy đảm bảo khai báo Region mặc định (ví dụ: `us-east-1`).
*   **Git:** Cài đặt Git để tải và quản lý mã nguồn.
*   **IDE:** Sử dụng Visual Studio Code hoặc công cụ viết code ưa thích của bạn.

---

### 3. Công cụ kiểm thử API & WebSocket

*   **Postman hoặc curl:** Cần thiết để kiểm tra các API REST HTTP (ví dụ: tạo quiz, khởi tạo phòng đấu).
*   **wscat:** Công cụ command-line để kiểm thử kết nối WebSocket thời gian thực. Cài đặt thông qua npm:
    ```bash
    npm install -g wscat
    ```
    Chúng ta sẽ sử dụng wscat để mô phỏng tương tác kết nối giữa người chơi và quản trò trực tiếp trên terminal.