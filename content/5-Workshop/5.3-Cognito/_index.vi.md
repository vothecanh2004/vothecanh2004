---
title: "Xác thực Cognito"
date: 2024-01-01
weight: 3
pre: " <b> 5.3. </b> "
---

# Thiết lập Xác thực với Cognito

Trong bước này, bạn sẽ cấu hình dịch vụ **Amazon Cognito** để quản lý việc đăng ký, đăng nhập và cấp quyền bảo mật cho người dùng quản trò (host). Chúng ta sẽ thao tác trên giao diện Console mới tập trung vào ứng dụng (application-centric) của Cognito.

### 1. Tạo Amazon Cognito User Pool cho ứng dụng Web (SPA)

1. Mở **[Amazon Cognito console](https://console.aws.amazon.com/cognito/)**.
![Ảnh 1](/images/5-Workshop/5-3-1.jpg)
2. Nhấn **Create user pool**.
![Ảnh 2](/images/5-Workshop/5-3-2.jpg)
3. **Step 1 - Define your application:**
   * **Application type:** Chọn **Single-page application (SPA)**.
   * Nhấn **Next**.
![Ảnh 3](/images/5-Workshop/5-3-3.jpg)
4. **Step 2 - Name your application:**
   * **Application name:** Nhập `webquiz-dev-web-client`.
   * Nhấn **Next**.
![Ảnh 4](/images/5-Workshop/5-3-4.jpg)
5. **Step 3 - Configure options:**
   * **Options for sign-in identifiers:** Tick chọn ✅ **Email**.
   * **Required attributes for sign-up:** Mặc định là `email`.
   * Nhấn **Next**.
![Ảnh 5](/images/5-Workshop/5-3-5.jpg)
6. **Step 4 - Add a return URL:**
   * **Return URL:** Nhập `http://localhost:3000/callback` (sẽ dùng để kiểm thử ứng dụng chạy frontend ở local).
   * Nhấn **Next**.
![Ảnh 6](/images/5-Workshop/5-3-6.jpg)
7. **Step 5 - Review and create:**
   * Kiểm tra kỹ lại toàn bộ cấu hình.
   * Nhấn **Create your application**.
8. **Step 6 - Set up your application:**
   * Giao diện sẽ hiển thị ví dụ mẫu tích hợp code.
   * Nhấn **Go to overview** để quay lại trang tổng quan User Pool.
1. **Mở Amazon Cognito Console**
   * Truy cập Amazon Cognito User Pools: [https://console.aws.amazon.com/cognito/v2/idp/user-pools](https://console.aws.amazon.com/cognito/v2/idp/user-pools)
   * Nhấn **Create user pool** hoặc **Get started for free in less than five minutes**.

2. **Define your application**
   * Tại màn hình Define your application:
   * **Application type:** Chọn **Single-page application (SPA)**
   * Nhấn **Next**.

3. **Name your application**
   * Nhập thông tin:
   * **Application name:** `webquiz-dev-web-client`
   * Nhấn **Next**.

4. **Configure options**
   * Thiết lập các tùy chọn đăng nhập:
   * **Options for sign-in identifiers:** Chọn **Email**
   * **Required attributes for sign-up:** Giữ mặc định là `email`
   * Nhấn **Next**.

5. **Add a return URL**
   * Nhập Callback URL của ứng dụng: `http://localhost:3000/callback` *(Nếu triển khai lên môi trường Production, hãy thay bằng URL thực tế của ứng dụng)*.
   * Nhấn **Next**.

6. **Review and create**
   * Kiểm tra lại các thiết lập:
     * **Application type:** Single-page application (SPA)
     * **Application name:** `webquiz-dev-web-client`
     * **Sign-in identifier:** Email
     * **Callback URL:** `http://localhost:3000/callback`
   * Sau đó nhấn **Create your application**.

7. **Set up your application**
   * Sau khi tạo thành công, Amazon Cognito sẽ tự động tạo:
     * User Pool
     * App Client (Public Client dành cho SPA)
   * Trang Set up your application sẽ hiển thị các đoạn mã mẫu (Quick setup guide) để tích hợp với ứng dụng.
   * Nếu chưa cần tích hợp ngay, chọn **Go to overview** để chuyển đến trang quản lý User Pool.

---

### 2. Cấu hình các thiết lập bổ sung

Sau khi tạo xong User Pool, cần đảm bảo các cấu hình bảo mật được thiết lập chuẩn xác:

1. Tại trang chi tiết User Pool, chọn tab **Authentication methods**:
   * Kiểm tra cấu hình **Password policy**:
     * Độ dài tối thiểu: `8` ký tự.
     * Tick chọn ✅ **Requires numbers** (yêu cầu số).
     * Tick chọn ✅ **Requires lowercase** (yêu cầu chữ thường).
     * Tick chọn ✅ **Requires uppercase** (yêu cầu chữ hoa).
     * Bỏ chọn ☐ **Requires symbols** (yêu cầu ký tự đặc biệt - tùy chỉnh theo nhu cầu).
2. Chọn tab **Sign-up**:
   * **Self-service sign-up:** Chọn ✅ **Enabled** (cho phép người dùng tự đăng ký tài khoản).
   * **Cognito-assisted verification:** Chọn ✅ **Enabled**.
   * **Verification method:** Chọn **Send email message** để gửi mã xác nhận qua email.
3. Chọn tab **App clients**:
   * Nhấp chuột vào app client tên là `webquiz-dev-web-client`.
   * Đảm bảo **Client secret** là **No client secret** (đặc thù cho ứng dụng SPA chạy phía client).
   * Đảm bảo mục **Authentication flows** đã kích hoạt các flow:
     * ✅ `ALLOW_USER_SRP_AUTH`
     * ✅ `ALLOW_REFRESH_TOKEN_AUTH`

---

### 3. Lưu lại thông tin cấu hình

Vui lòng sao chép và lưu trữ hai thông số quan trọng sau từ trang tổng quan:
*   **User Pool ID:** `ap-southeast-1_xxxxxxxxx`
*   **App Client ID** (tìm thấy trong tab App clients): `xxxxxxxxxxxxxxxxxxxxxxxxxx`

> [!IMPORTANT]
> Bạn sẽ cần dùng 2 giá trị này để cấu hình bộ xác thực Authorizer trên API Gateway và thiết lập biến môi trường cho ứng dụng frontend sau này.
