---
title: "API Gateway - WebSocket API"
date: 2024-01-01
weight: 9
pre: " <b> 5.9. </b> "
---

# Thiết lập API Gateway - WebSocket API

Trong bước này, bạn sẽ cấu hình **Amazon API Gateway WebSocket API** nhằm duy trì các kết nối hai chiều liên tục thời gian thực giữa máy khách frontend và các hàm Lambda xử lý nghiệp vụ.

---

### 1. Tạo WebSocket API

1. Mở **[Amazon API Gateway console](https://console.aws.amazon.com/apigateway/)**.
2. Nhấp chọn **Create API**.
3. Tại phần **Choose an API type**, tìm **WebSocket API** và nhấp chọn **Build**.
4. Cấu hình các thông số chi tiết của API:
   * **API name:** Nhập `webquiz-dev-websocket-api`.
   * **Route selection expression:** Nhập `$request.body.action` (cấu hình này giúp hệ thống trích xuất tên action trong payload JSON gửi lên để định tuyến thông điệp chính xác).
   * Nhấn **Next**.

---

### 2. Thiết lập Định tuyến (Routes)

Kết nối WebSocket sử dụng các key định tuyến đặc thù. Chúng ta sẽ kích hoạt 3 route mặc định của hệ thống:

1. Tại bước **Add routes**:
   * Tick chọn ✅ **$connect**
   * Tick chọn ✅ **$disconnect**
   * Tick chọn ✅ **$default**
2. Nhấn **Next**.

---

### 3. Gán các Tích hợp (Integrations)

Liên kết từng route đường dẫn WebSocket với hàm Lambda xử lý tương ứng:

1. Thiết lập các integration:
   * Đối với route **`$connect`**:
     * **Integration type:** Chọn **Lambda**.
     * **Lambda function:** Chọn `webquiz-dev-ws-connect`.
   * Đối với route **`$disconnect`**:
     * **Integration type:** Chọn **Lambda**.
     * **Lambda function:** Chọn `webquiz-dev-ws-disconnect`.
   * Đối với route **`$default`**:
     * **Integration type:** Chọn **Lambda**.
     * **Lambda function:** Chọn `webquiz-dev-ws-message`.
2. Nhấn **Next**.

---

### 4. Khởi tạo và Deploy Stage

1. **Step 4 - Add stages:**
   * **Stage name:** Nhập `dev`.
   * Tick chọn ✅ **Auto deploy**.
   * Nhấn **Next**.
2. Nhấp chọn **Create and deploy**.

---

### 5. Sao chép các Endpoint WebSocket

Truy cập tab **Stages** ở menu trái và chọn stage **`dev`**. Ghi lại hai đường dẫn URL sau:

*   **WebSocket URL:**
    ```
    wss://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/dev
    ```
    *(Sử dụng phía client React Frontend để kết nối thời gian thực).*
*   **Connections URL:**
    ```
    https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/dev
    ```
    *(Sử dụng phía hàm Lambda backend để thực hiện phát/broadcast tin nhắn).*

---

### 6. Cập nhật Biến Môi trường cho Lambda

Khi đã có endpoint kết nối chính thức, chúng ta phải quay lại cập nhật biến môi trường cho các hàm Lambda để chúng có quyền phát tin nhắn ngược về client.

1. Mở **AWS Lambda console**.
2. Cập nhật hàm **`webquiz-dev-ws-message`**:
   * Vào trang chi tiết hàm → Chọn tab **Configuration** → Chọn mục **Environment variables**.
   * Nhấp chọn **Edit** và thiết lập:
     * `WEBSOCKET_ENDPOINT` = `https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/dev` (Sử dụng URL kết nối dạng **`https://`**, **không** dùng tiền tố `wss://`).
   * Nhấn **Save**.
3. Cập nhật hàm **`webquiz-dev-score-calculator`**:
   * Vào trang chi tiết hàm → Chọn tab **Configuration** → Chọn mục **Environment variables**.
   * Nhấp chọn **Edit** và thiết lập:
     * `WEBSOCKET_ENDPOINT` = `https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/dev`
   * Nhấn **Save**.
