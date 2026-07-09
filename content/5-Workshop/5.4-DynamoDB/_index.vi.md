---
title: "Bảng dữ liệu DynamoDB"
date: 2024-01-01
weight: 4
pre: " <b> 5.4. </b> "
---

# Thiết kế & Tạo các bảng DynamoDB

Trong bước này, bạn sẽ xây dựng tầng cơ sở dữ liệu của dự án. Thay vì sử dụng SQL truyền thống hay cơ chế Redis tốn kém, **WebQuiz** tận dụng tối đa sức mạnh của **Amazon DynamoDB**—cơ sở dữ liệu NoSQL Serverless. Chúng ta sẽ cấu hình co giãn theo yêu cầu (**On-demand**) để tối ưu hóa chi phí.

---

### 1. Tiến hành Tạo các bảng Dữ liệu

Vui lòng tạo lần lượt 7 bảng DynamoDB theo cấu hình chi tiết dưới đây:

#### 1.1 Bảng Users (`webquiz-dev-users`)
1. Truy cập **[Amazon DynamoDB console](https://console.aws.amazon.com/dynamodb/)**.
2. Trên menu trái chọn **Tables**, nhấp chọn **Create table**.
3. Nhập các thông tin trong phần **Table details**:
   * **Table name:** `webquiz-dev-users`
   * **Partition key:** `userId` | **String** (Kiểu chuỗi)
   * Không tick chọn Sort key.
4. Tại phần **Table settings**, tick chọn **Customize settings**.
5. Đặt **Capacity mode** là **On-demand**.
6. Tại phần **Secondary indexes**, nhấp chọn **Create global index**:
   * **Partition key:** `email` | **String**
   * **Index name:** `email-index`
   * **Attribute projections:** Chọn **All** (Tất cả)
   * Nhấp chọn **Create index**.
7. Giữ các thông số mã hóa mặc định, nhấp chọn **Create table**.

#### 1.2 Bảng Quizzes (`webquiz-dev-quizzes`)
1. Nhấp chọn **Create table**:
   * **Table name:** `webquiz-dev-quizzes`
   * **Partition key:** `quizId` | **String**
2. Chọn **Customize settings** → Cấu hình co giãn **On-demand**.
3. Tại phần **Secondary indexes**, nhấp chọn **Create global index**:
   * **Partition key:** `userId` | **String**
   * **Sort key:** `createdAt` | **String**
   * **Index name:** `userId-createdAt-index`
   * **Attribute projections:** **All**
   * Nhấp chọn **Create index**.
4. Nhấp chọn **Create table**.

#### 1.3 Bảng Questions (`webquiz-dev-questions`)
1. Nhấp chọn **Create table**:
   * **Table name:** `webquiz-dev-questions`
   * **Partition key:** `quizId` | **String**
   * **Sort key:** Tick chọn checkbox, nhập `questionId` | **String**
2. Chọn **Customize settings** → Cấu hình co giãn **On-demand**.
3. Nhấp chọn **Create table**.

#### 1.4 Bảng Rooms (`webquiz-dev-rooms`)
1. Nhấp chọn **Create table**:
   * **Table name:** `webquiz-dev-rooms`
   * **Partition key:** `roomPin` | **String**
2. Chọn **Customize settings** → Cấu hình co giãn **On-demand**.
3. Tại phần **Secondary indexes**, nhấp chọn **Create global index** (tạo 2 chỉ mục GSI):
   * **GSI 1:**
     * **Partition key:** `hostId` | **String**
     * **Sort key:** `createdAt` | **String**
     * **Index name:** `hostId-createdAt-index`
     * **Attribute projections:** **All**
     * Nhấp chọn **Create index**.
   * **GSI 2:**
     * **Partition key:** `status` | **String**
     * **Sort key:** `createdAt` | **String**
     * **Index name:** `status-createdAt-index`
     * **Attribute projections:** **Keys only** (Chỉ chứa khóa)
     * Nhấp chọn **Create index**.
4. Nhấp chọn **Create table**.
5. Sau khi bảng đã tạo xong, bấm vào tên bảng `webquiz-dev-rooms`, chọn tab **Additional settings**:
   * Tại phần **Time to Live (TTL)**, nhấp chọn **Turn on**:
     * **TTL attribute name:** Nhập `ttl`.
     * Nhấp chọn **Turn on TTL**.

#### 1.5 Bảng Connections (`webquiz-dev-connections`)
1. Nhấp chọn **Create table**:
   * **Table name:** `webquiz-dev-connections`
   * **Partition key:** `connectionId` | **String**
2. Chọn **Customize settings** → Cấu hình co giãn **On-demand**.
3. Tại phần **Secondary indexes**, nhấp chọn **Create global index**:
   * **Partition key:** `roomPin` | **String**
   * **Index name:** `roomPin-index`
   * **Attribute projections:** **All**
   * Nhấp chọn **Create index**.
4. Nhấp chọn **Create table**.
5. Bấm vào bảng `webquiz-dev-connections`, chọn tab **Additional settings**, kích hoạt **Time to Live (TTL)** với tên thuộc tính là `ttl`.

#### 1.6 Bảng Game State (`webquiz-dev-game-state`)
Bảng này đóng vai trò quan trọng lưu trữ các số liệu thay đổi liên tục trong trận đấu (như điểm số người chơi, đáp án đã nộp, kết nối lobby).
1. Nhấp chọn **Create table**:
   * **Table name:** `webquiz-dev-game-state`
   * **Partition key:** `pk` | **String**
   * **Sort key:** Tick chọn checkbox, nhập `sk` | **String**
2. Chọn **Customize settings** → Cấu hình co giãn **On-demand**.
3. Tại phần **Secondary indexes**, nhấp chọn **Create global index**:
   * **Partition key:** `gsi1pk` | **String**
   * **Sort key:** `gsi1sk` | **String**
   * **Index name:** `gsi1`
   * **Attribute projections:** **All**
   * Nhấp chọn **Create index**.
4. Nhấp chọn **Create table**.
5. Kích hoạt thuộc tính **Time to Live (TTL)** của bảng này với thuộc tính `ttl`.

#### 1.7 Bảng Game Results (`webquiz-dev-game-results`)
1. Nhấp chọn **Create table**:
   * **Table name:** `webquiz-dev-game-results`
   * **Partition key:** `roomPin` | **String**
   * **Sort key:** Tick chọn checkbox, nhập `orderId` | **String**
2. Chọn **Customize settings** → Cấu hình co giãn **On-demand**.
3. Nhấp chọn **Create table**.

---

### 2. Bảng Tổng hợp Cơ sở Dữ liệu

Dưới đây là tóm tắt các thuộc tính của 7 bảng DynamoDB đã tạo:

| Tên bảng | Partition Key (Khóa phân vùng) | Sort Key (Khóa sắp xếp) | Global Secondary Index (GSI) | Kích hoạt TTL |
| :--- | :--- | :--- | :--- | :--- |
| **webquiz-dev-users** | `userId` (S) | - | `email-index` (PK: `email`) | Không |
| **webquiz-dev-quizzes** | `quizId` (S) | - | `userId-createdAt-index` (PK: `userId`, SK: `createdAt`) | Không |
| **webquiz-dev-questions** | `quizId` (S) | `questionId` (S) | - | Không |
| **webquiz-dev-rooms** | `roomPin` (S) | - | `hostId-createdAt-index`, `status-createdAt-index` | Có (`ttl`) |
| **webquiz-dev-connections** | `connectionId` (S) | - | `roomPin-index` (PK: `roomPin`) | Có (`ttl`) |
| **webquiz-dev-game-state** | `pk` (S) | `sk` (S) | `gsi1` (PK: `gsi1pk`, SK: `gsi1sk`) | Có (`ttl`) |
| **webquiz-dev-game-results** | `roomPin` (S) | `orderId` (S) | - | Không |
