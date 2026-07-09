---
title: "Quyền IAM & EventBridge"
date: 2024-01-01
weight: 5
pre: " <b> 5.5. </b> "
---

# Cấu hình Phân quyền IAM & EventBridge

Trong bước này, bạn sẽ thiết lập phân quyền bảo mật cho các tiến trình chạy ngầm và cấu hình trục hàng đợi thông điệp định tuyến sự kiện (Event Bus) cho hệ thống.

---

### 1. Tạo Lambda Execution Role

Đầu tiên, chúng ta cần tạo một vai trò IAM Role để cho phép các hàm Lambda giao tiếp an toàn với các dịch vụ khác của AWS.

1. Mở **[Amazon IAM console](https://console.aws.amazon.com/iam/)**.
2. Trên menu bên trái chọn **Roles**, nhấp chọn **Create role**.
3. **Step 1 - Select trusted entity:**
   * **Trusted entity type:** Chọn **AWS service**.
   * **Service or use case:** Chọn **Lambda** từ danh sách.
   * Nhấn **Next**.
4. **Step 2 - Add permissions:**
   * Tìm kiếm và tick chọn chính sách managed policy tên là **`AWSLambdaBasicExecutionRole`** (quyền này cho phép các hàm ghi log hoạt động ra CloudWatch).
   * Nhấn **Next**.
5. **Step 3 - Name, review, and create:**
   * **Role name:** Nhập `webquiz-dev-lambda-role`.
   * Nhấp chọn **Create role**.

---

### 2. Thiết lập Chính sách Phân quyền Custom (Inline Policy)

Tiếp theo, cấu hình phân quyền chi tiết cho role này truy cập vào bảng DynamoDB của dự án, quyền gửi sự kiện lên EventBridge và quyền quản lý kết nối client WebSocket.

1. Tìm kiếm và nhấp chọn role vừa tạo: `webquiz-dev-lambda-role`.
2. Tại tab **Permissions**, nhấp chọn **Add permissions** → Chọn **Create inline policy**.
3. Chọn tab **JSON** và dán chính sách phân quyền sau:
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "DynamoDBAccess",
               "Effect": "Allow",
               "Action": [
                   "dynamodb:GetItem",
                   "dynamodb:PutItem",
                   "dynamodb:UpdateItem",
                   "dynamodb:DeleteItem",
                   "dynamodb:Query",
                   "dynamodb:Scan",
                   "dynamodb:BatchGetItem",
                   "dynamodb:BatchWriteItem"
               ],
               "Resource": [
                   "arn:aws:dynamodb:ap-southeast-1:*:table/webquiz-dev-*",
                   "arn:aws:dynamodb:ap-southeast-1:*:table/webquiz-dev-*/index/*"
               ]
           },
           {
               "Sid": "EventBridgeAccess",
               "Effect": "Allow",
               "Action": [
                   "events:PutEvents"
               ],
               "Resource": "arn:aws:events:ap-southeast-1:*:event-bus/webquiz-dev-game-events"
           },
           {
               "Sid": "WebSocketManagement",
               "Effect": "Allow",
               "Action": [
                   "execute-api:ManageConnections"
               ],
               "Resource": "arn:aws:execute-api:ap-southeast-1:*:*/dev/*"
           }
       ]
   }
   ```
4. Nhấn **Next**.
5. **Policy name:** Nhập `webquiz-dev-lambda-policy`.
6. Nhấn **Create policy**.

---

### 3. Tạo Custom Event Bus trên EventBridge

Tạo Event Bus để định tuyến các sự kiện chơi game (nộp đáp án, tính điểm, kết thúc game) bất đồng bộ dưới nền.

1. Mở **[Amazon EventBridge console](https://console.aws.amazon.com/events/)**.
2. Trên menu bên trái chọn **Event buses**, nhấp chọn **Create event bus**.
3. Trong biểu mẫu khởi tạo:
   * **Name:** Nhập `webquiz-dev-game-events`.
   * **Encryption:** Chọn **Use AWS owned key** (mặc định).
   * Giữ nguyên các thiết lập khác, nhấp chọn **Create**.
