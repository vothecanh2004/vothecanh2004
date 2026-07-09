---
title: "Dọn dẹp tài nguyên"
date: 2024-01-01
weight: 13
pre: " <b> 5.13. </b> "
---

# Dọn dẹp tài nguyên sau bài lab

Để tránh phát sinh chi phí ngoài ý muốn trên tài khoản AWS của bạn, hãy đảm bảo dọn dẹp sạch sẽ toàn bộ các tài nguyên đã khởi tạo trong suốt quá trình thực hành.

Vui lòng tiến hành dọn dẹp theo thứ tự tuần tự dưới đây để đảm bảo an toàn:

---

### 1. Dọn dẹp Tài nguyên Frontend

#### 1.1 Xóa dữ liệu trong S3 Bucket
1. Truy cập **[Amazon S3 console](https://console.aws.amazon.com/s3/)**.
2. Chọn bucket của bạn: `webquiz-dev-frontend-YOUR_ACCOUNT_ID`.
3. Bấm chọn **Empty** để xóa hoàn toàn các đối tượng tĩnh lưu bên trong bucket.
4. Sau khi bucket đã rỗng hoàn toàn, chọn **Delete**, nhập tên bucket để xác nhận và thực hiện xóa.

#### 1.2 Xóa CloudFront Distribution
1. Truy cập **[Amazon CloudFront console](https://console.aws.amazon.com/cloudfront/)**.
2. Chọn distribution của bạn, nhấp chọn **Disable** để vô hiệu hóa nó.
3. Đợi vài phút cho trạng thái chuyển hẳn sang `Disabled`.
4. Chọn lại distribution đó một lần nữa và nhấp chọn **Delete**.

---

### 2. Xóa các Endpoint API Gateway

1. Truy cập **[Amazon API Gateway console](https://console.aws.amazon.com/apigateway/)**.
2. Chọn HTTP API `webquiz-dev-http-api`, click **Actions** (hoặc nút 3 chấm) → chọn **Delete**.
3. Chọn WebSocket API `webquiz-dev-websocket-api`, click **Actions** → chọn **Delete**.

---

### 3. Xóa các Hàm Lambda & Phân quyền IAM

1. Truy cập **[AWS Lambda console](https://console.aws.amazon.com/lambda/)**.
2. Tại mục **Functions**, tìm kiếm các hàm bắt đầu bằng tiền tố `webquiz-dev-`.
3. Lần lượt chọn cả 7 hàm Lambda:
   * `webquiz-dev-quiz-crud`
   * `webquiz-dev-room-management`
   * `webquiz-dev-ws-connect`
   * `webquiz-dev-ws-disconnect`
   * `webquiz-dev-ws-message`
   * `webquiz-dev-score-calculator`
   * `webquiz-dev-game-results-saver`
4. Chọn **Actions** → **Delete**.
5. Truy cập **[Amazon IAM console](https://console.aws.amazon.com/iam/)**:
   * Tại mục **Roles**, tìm kiếm `webquiz-dev-lambda-role` và xóa role này.

---

### 4. Xóa Cơ sở dữ liệu & Tích hợp sự kiện

1. Truy cập **[Amazon DynamoDB console](https://console.aws.amazon.com/dynamodb/)**.
2. Tại mục **Tables**, chọn 7 bảng bắt đầu bằng tiền tố `webquiz-dev-`:
   * `webquiz-dev-users`
   * `webquiz-dev-quizzes`
   * `webquiz-dev-questions`
   * `webquiz-dev-rooms`
   * `webquiz-dev-connections`
   * `webquiz-dev-game-state`
   * `webquiz-dev-game-results`
3. Nhấp chọn **Delete table**, nhập `delete` vào ô xác nhận và click **Delete**.
4. Truy cập **[Amazon EventBridge console](https://console.aws.amazon.com/events/)**:
   * Chọn mục **Event buses**, click chọn bus `webquiz-dev-game-events` của bạn và nhấn **Delete**.

---

### 5. Xóa Cognito User Pool

1. Truy cập **[Amazon Cognito console](https://console.aws.amazon.com/cognito/)**.
2. Chọn User Pool có tên `webquiz-dev-users`.
3. Nhấp chọn **Delete**, nhập mã ID của User Pool đó để xác nhận và chọn xóa.
