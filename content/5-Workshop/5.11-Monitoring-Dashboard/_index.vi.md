---
title: "Giám sát CloudWatch & Dashboard"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

# Cấu hình Giám sát CloudWatch & Dashboard

Trong bước này, bạn sẽ hoàn thiện các thiết lập cuối cùng cho việc định tuyến sự kiện ngầm bằng cách tạo các **Quy tắc EventBridge Rules** (yêu cầu các hàm Lambda đích đã tạo ở phần trước) và xây dựng bảng giám sát **Amazon CloudWatch** để theo dõi hiệu năng hệ thống.

---

### 1. Cấu hình Quy tắc định tuyến EventBridge Rules

Bây giờ khi các hàm Lambda xử lý logic đã được tạo, tiến hành tạo các rule định tuyến trên Event Bus để tự động kích hoạt chúng khi có sự kiện chơi game gửi lên.

#### 1.1 Tạo Rule cho AnswerSubmitted (Nộp câu trả lời)
1. Mở **[Amazon EventBridge console](https://console.aws.amazon.com/events/)**.
2. Chọn mục **Rules** trên menu trái.
3. Chọn Event bus `webquiz-dev-game-events` từ menu thả xuống.
4. Nhấp chọn **Create rule**.
5. **Step 1 - Define rule detail:**
   * **Name:** Nhập `webquiz-dev-answer-submitted`
   * **Description:** Nhập `Trigger score calculator when answer is submitted`.
   * **Event bus:** Chọn `webquiz-dev-game-events`.
   * **Rule type:** Chọn **Rule with an event pattern**.
   * Nhấp chọn **Next**.
6. **Step 2 - Build event pattern:**
   * **Event source:** Chọn **Other**.
   * **Creation method:** Chọn **Custom pattern (JSON editor)**.
   * **Event pattern:** Dán đoạn JSON sau:
     ```json
     {
       "source": ["quiz.game"],
       "detail-type": ["AnswerSubmitted"]
     }
     ```
   * Nhấn **Next**.
7. **Step 3 - Select targets:**
   * **Target type:** Chọn **AWS service**.
   * **Select a target:** Chọn **Lambda function**.
   * **Function:** Chọn `webquiz-dev-score-calculator`.
   * Nhấn **Next**.
8. **Step 4 - Review and create:**
   * Nhấp chọn **Create rule**.

#### 1.2 Tạo Rule cho GameEnded (Kết thúc trận đấu)
1. Trong bus `webquiz-dev-game-events`, nhấp chọn **Create rule**.
2. **Step 1 - Define rule detail:**
   * **Name:** Nhập `webquiz-dev-game-ended`.
   * **Event bus:** Chọn `webquiz-dev-game-events`.
   * **Rule type:** Chọn **Rule with an event pattern**.
3. **Step 2 - Build event pattern:**
   * **Event source:** Chọn **Other**.
   * **Event pattern:**
     ```json
     {
       "source": ["quiz.game"],
       "detail-type": ["GameEnded"]
     }
     ```
4. **Step 3 - Select targets:**
   * **Target:** Chọn **Lambda function** → Chọn hàm `webquiz-dev-game-results-saver`.
5. Nhấp chọn **Create rule**.

---

### 2. Thiết lập Cảnh báo CloudWatch Alarms

Cấu hình các bộ alarms để gửi email hoặc SNS cảnh báo khi hệ thống xảy ra lỗi nghiêm trọng hoặc nghẽn cơ sở dữ liệu.

1. Mở **[Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch/)**.
2. Chọn **Alarms** ở menu bên trái → Click **All alarms** → Click **Create alarm**.

#### 2.1 Cảnh báo Lỗi Hàm Lambda
*   **Metric:** Chọn `AWS/Lambda` → `Errors` → Sum.
*   **Period:** `5 minutes`.
*   **Threshold:** Lớn hơn `5` lỗi.
*   **Name:** `webquiz-dev-lambda-errors`.

#### 2.2 Cảnh báo Quá tải Kết nối WebSocket
*   **Metric:** Chọn `AWS/ApiGateway` → `ConnectCount` (Lọc theo ApiId của WebSocket API vừa tạo).
*   **Period:** `1 minute`.
*   **Threshold:** Lớn hơn `10000` kết nối.
*   **Name:** `webquiz-dev-websocket-connections`.

#### 2.3 Cảnh báo Nghẽn Cơ sở dữ liệu DynamoDB
*   **Metric:** Chọn `AWS/DynamoDB` → `ThrottledRequests` (Lọc theo TableName = `webquiz-dev-game-state`).
*   **Period:** `1 minute`.
*   **Threshold:** Lớn hơn `1` request bị nghẽn.
*   **Name:** `webquiz-dev-dynamodb-throttle`.

---

### 3. Tạo Dashboard Giám sát CloudWatch

Xây dựng bảng theo dõi tổng quan trực quan hóa sức khỏe hệ thống theo thời gian thực.

1. Chọn mục **Dashboards** ở menu bên trái của CloudWatch console.
2. Nhấp chọn **Create dashboard**.
3. **Dashboard name:** Nhập `webquiz-dev-dashboard`.
4. Lần lượt thêm các Widget sau:
   * **Widget 1 (Tần suất gọi Lambda):** Hiển thị metric `Invocations` cho các hàm `webquiz-dev-ws-message`, `webquiz-dev-score-calculator`, và `webquiz-dev-quiz-crud`.
   * **Widget 2 (Độ trễ xử lý Lambda):** Hiển thị metric `Duration` (đo lường p99) cho hàm `webquiz-dev-ws-message` và `webquiz-dev-score-calculator`.
   * **Widget 3 (Số liệu WebSocket):** Hiển thị metric `ConnectCount` và `MessageCount` của WebSocket API Gateway.
   * **Widget 4 (Năng lực tiêu thụ DynamoDB):** Hiển thị metrics `ConsumedReadCapacityUnits`, `ConsumedWriteCapacityUnits`, và `ThrottledRequests` của bảng `webquiz-dev-game-state`.
5. Nhấp chọn **Save dashboard** để lưu lại cấu hình.
