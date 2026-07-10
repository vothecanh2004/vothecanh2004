---
title: "API Gateway - HTTP API"
date: 2024-01-01
weight: 8
pre: " <b> 5.8. </b> "
---

# Thiết lập API Gateway - HTTP API

Trong bước này, bạn sẽ cấu hình **Amazon API Gateway HTTP API** để định tuyến các request REST đến các hàm xử lý Lambda tương ứng ở backend. Chúng ta cũng sẽ bảo vệ các endpoint nhạy cảm bằng bộ xác thực Cognito JWT Authorizer.

---

### 1. Tạo HTTP API

1. Mở **[Amazon API Gateway console](https://console.aws.amazon.com/apigateway/)**.
![Ảnh 1](/images/5-Workshop/5.8/5.8.1.png)
2. Nhấp chọn **Create API**.
![Ảnh 2](/images/5-Workshop/5.8/5.8.2.png)
3. Tại phần **Choose an API type**, tìm **HTTP API** và chọn **Build**.
4. **Step 1 - Create and configure integrations:**
   * **API name:** Nhập `webquiz-dev-http-api`.
   * **IP address type:** Chọn **IPv4**.
   * *(Không thêm tích hợp integration nào ở bước này)*.
   * Nhấn **Next**.
![Ảnh 3](/images/5-Workshop/5.8/5.8.3.png)
5. **Step 2 - Configure routes:**
   * Nhấn **Next** (chúng ta sẽ tự tạo routes sau).
![Ảnh 4](/images/5-Workshop/5.8/5.8.4.png)
6. **Step 3 - Define stages:**
   * **Stage name:** Nhập `dev`.
   * Tick chọn ✅ **Auto-deploy**.
   * Nhấn **Next**.
![Ảnh 5](/images/5-Workshop/5.8/5.8.5.png)
7. **Step 4 - Review and create:**
   * Nhấn **Create**.
![Ảnh 6](/images/5-Workshop/5.8/5.8.6.png)
![Ảnh 7](/images/5-Workshop/5.8/5.8.7.png)

---

### 2. Cấu hình CORS

Để cho phép frontend gọi các request API từ các domain khác hoặc từ localhost:

1. Trong menu bên trái của API vừa tạo, chọn **CORS**.
![Ảnh 8](/images/5-Workshop/5.8/5.8.8.png)
2. Nhấp chọn **Configure** và nhập các thông số sau:
   * **Access-Control-Allow-Origin:** Nhập `*`.
   * **Access-Control-Allow-Headers:** Nhập `Content-Type, Authorization`.
   * **Access-Control-Allow-Methods:** Nhập `GET, POST, PUT, DELETE, OPTIONS`.
   * **Access-Control-Max-Age:** Nhập `300`.
   * Nhấp chọn **Save**.
![Ảnh 9](/images/5-Workshop/5.8/5.8.9.png)
![Ảnh 10](/images/5-Workshop/5.8/5.8.10.png)



---

### 3. Tạo bộ xác thực Cognito JWT Authorizer

1. Chọn **Authorization** ở menu bên trái.
![Ảnh 11](/images/5-Workshop/5.8/5.8.11.png)
2. Chọn tab **Manage authorizers**, nhấp chọn **Create**.
![Ảnh 12](/images/5-Workshop/5.8/5.8.12.png)
3. Thiết lập thông số Authorizer:
   * **Authorizer type:** Chọn **JWT**.
   * **Name:** Nhập `CognitoAuthorizer`.
   * **Identity source:** Nhập `$request.header.Authorization`.
   * **Issuer URL:** Nhập `https://cognito-idp.ap-southeast-1.amazonaws.com/YOUR_USER_POOL_ID` (Thay thế `YOUR_USER_POOL_ID` bằng User Pool ID thực tế đã lưu ở phần Cognito).
![Ảnh 13](/images/5-Workshop/5.8/5.8.13.png)
   * **Audience:** Nhập **App Client ID** đã lưu ở phần Cognito.
![Ảnh 14](/images/5-Workshop/5.8/5.8.14.png)
   * Nhấp chọn **Create**.
![Ảnh 15](/images/5-Workshop/5.8/5.8.15.png)



---

### 4. Tạo các Tích hợp (Integrations)

Kết nối API Gateway với các hàm Lambda xử lý nghiệp vụ backend:

1. Chọn **Integrations** ở menu bên trái.
![Ảnh 16](/images/5-Workshop/5.8/5.8.16.png)
2. Nhấp chọn **Create** cho 2 integration sau:
   * **Integration 1 (Quiz CRUD):**
     * **Integration target:** Chọn **Lambda function**.
     * **Lambda function:** Chọn `webquiz-dev-quiz-crud`.
![Ảnh 17](/images/5-Workshop/5.8/5.8.17.png)
     * Nhấp chọn **Create**.
![Ảnh 18](/images/5-Workshop/5.8/5.8.18.png)
   * **Integration 2 (Room Management):**
     * Nhấp chọn **Create**.

     * **Integration target:** Chọn **Lambda function**.
     * **Lambda function:** Chọn `webquiz-dev-room-management`.
     * Nhấp chọn **Create**.
![Ảnh 19](/images/5-Workshop/5.8/5.8.19.png)
![Ảnh 20](/images/5-Workshop/5.8/5.8.20.png)

---

### 5. Tạo Định tuyến (Routes) & Gán Phân Quyền

Liên kết các route đường dẫn của API, tích hợp Lambda và gắn quyền xác thực tương ứng:

1. Chọn **Routes** ở menu bên trái.
![Ảnh 21](/images/5-Workshop/5.8/5.8.21.png)
2. Khởi tạo và thiết lập 5 route theo bảng dưới đây:

| STT | Phương thức (Method) | Đường dẫn (Path) | Tích hợp Target (Integration) | Bộ xác thực (Authorizer) |
| :--- | :--- | :--- | :--- | :--- |
| **Route 1** | `ANY` | `/quizzes` | `webquiz-dev-quiz-crud` | `CognitoAuthorizer` |
| **Route 2** | `ANY` | `/quizzes/{proxy+}` | `webquiz-dev-quiz-crud` | `CognitoAuthorizer` |
| **Route 3** | `POST` | `/rooms` | `webquiz-dev-room-management` | `CognitoAuthorizer` |
| **Route 4** | `GET` | `/rooms/{pin}` | `webquiz-dev-room-management` | **None** (Công khai) |
| **Route 5** | `POST` | `/rooms/{pin}/join` | `webquiz-dev-room-management` | **None** (Công khai) |

*Các bước cấu hình cho từng Route:*
1. Nhấp chọn **Create**, nhập **Method** và **Path** tương ứng.
![Ảnh 22](/images/5-Workshop/5.8/5.8.22.png)
![Ảnh 23](/images/5-Workshop/5.8/5.8.23.png)
2. Click vào route vừa tạo → Chọn **Attach integration** và gán hàm Lambda tương ứng.
![Ảnh 24](/images/5-Workshop/5.8/5.8.24.png)
![Ảnh 25](/images/5-Workshop/5.8/5.8.25.png)
![Ảnh 26](/images/5-Workshop/5.8/5.8.26.png)
![Ảnh 27](/images/5-Workshop/5.8/5.8.27.png)
![Ảnh 28](/images/5-Workshop/5.8/5.8.28.png)
![Ảnh 29](/images/5-Workshop/5.8/5.8.29.png)
![Ảnh 30](/images/5-Workshop/5.8/5.8.30.png)
![Ảnh 31](/images/5-Workshop/5.8/5.8.31.png)
![Ảnh 32](/images/5-Workshop/5.8/5.8.32.png)



3. Chọn **Attach authorizer** (nếu bảng yêu cầu) và gán `CognitoAuthorizer`.
![Ảnh 33](/images/5-Workshop/5.8/5.8.33.png)
![Ảnh 34](/images/5-Workshop/5.8/5.8.34.png)
![Ảnh 35](/images/5-Workshop/5.8/5.8.35.png)s



---

### 6. Lưu lại Endpoint REST API

1. Chọn mục **Stages** trên menu bên trái.
![Ảnh 36](/images/5-Workshop/5.8/5.8.36.png)
2. Click chọn stage tên là **`dev`**.
![Ảnh 37](/images/5-Workshop/5.8/5.8.37.png)
3. Sao chép và lưu trữ URL tại phần **Invoke URL**:
   ```
   https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/dev
   ```
*(Bạn sẽ cần dùng link REST endpoint này để cấu hình cho client frontend sau này).*
