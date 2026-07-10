---
title: "Xác thực Cognito"
date: 2024-01-01
weight: 3
pre: " <b> 5.3. </b> "
---

# Thiết lập Xác thực Cognito

Trong bước này, bạn sẽ cấu hình **Amazon Cognito** để quản lý việc đăng ký người dùng, xác thực và bảo mật các access token cho người dùng đóng vai trò host. Chúng ta sẽ sử dụng giao diện điều khiển Cognito mới tập trung vào ứng dụng (application-centric).

### 1. Tạo Amazon Cognito User Pool cho Ứng dụng Web (SPA)

1. Mở **[Amazon Cognito console](https://console.aws.amazon.com/cognito/)**.
![Image 1](/images/5-Workshop/5.3/5.3.1.png)
2. Nhấp chọn **Create user pool**.
![Image 2](/images/5-Workshop/5.3/5.3.2.png)
3. **Bước 1 - Định nghĩa ứng dụng của bạn (Define your application):**
   * **Application type:** Chọn **Single-page application (SPA)**.
   * Nhấp chọn **Next**.
![Image 3](/images/5-Workshop/5.3/5.3.3.png)
4. **Bước 2 - Đặt tên cho ứng dụng (Name your application):**
   * **Application name:** Nhập `webquiz-dev-web-client`.
   * Nhấp chọn **Next**.
![Image 4](/images/5-Workshop/5.3/5.3.4.png)
5. **Bước 3 - Cấu hình các tùy chọn (Configure options):**
   * **Options for sign-in identifiers:** Tích chọn ✅ **Email**.
   * **Required attributes for sign-up:** `email` (mặc định).
   * Nhấp chọn **Next**.
![Image 5](/images/5-Workshop/5.3/5.3.5.png)
6. **Bước 4 - Thêm URL phản hồi (Add a return URL):**
   * **Return URL:** Nhập `http://localhost:3000/callback` (bạn sẽ sử dụng URL này để tích hợp với frontend ở môi trường local).
   * Nhấp chọn **Next**.
![Image 6](/images/5-Workshop/5.3/5.3.6.png)
7. **Bước 5 - Xem lại và tạo (Review and create):**
   * Kiểm tra lại toàn bộ thông tin cấu hình.
   * Nhấp chọn **Create your application**.
8. **Bước 6 - Thiết lập ứng dụng của bạn (Set up your application):**
   * Cognito sẽ hiển thị các ví dụ về tích hợp mã nguồn (code).
   * Nhấp chọn **Go to overview** để đi đến trang tổng quan User Pool của bạn.

---

### 2. Cấu hình các Thiết lập bổ sung cho User Pool

Sau khi User Pool được tạo, bạn phải cấu hình các chính sách bảo mật:

1. Trong trang chi tiết User Pool của bạn, chọn tab **Authentication methods**:
   * Xác minh cấu hình **Password policy** (Chính sách mật khẩu):
     * Độ dài tối thiểu (Minimum length): `8` ký tự.
     * Tích chọn ✅ **Requires numbers** (Yêu cầu chữ số).
     * Tích chọn ✅ **Requires lowercase** (Yêu cầu chữ thường).
     * Tích chọn ✅ **Requires uppercase** (Yêu cầu chữ hoa).
     * Bỏ tích ☐ **Requires symbols** (Yêu cầu ký tự đặc biệt - hoặc giữ tích chọn nếu bạn muốn).
2. Chọn tab **Sign-up**:
   * **Self-service sign-up:** Tích chọn ✅ **Enabled** (để cho phép người dùng tự đăng ký tài khoản).
   * **Cognito-assisted verification:** Tích chọn ✅ **Enabled**.
   * **Verification method:** Chọn **Send email message** (Gửi tin nhắn email).
3. Chọn tab **App clients**:
   * Nhấp vào client có tên là `webquiz-dev-web-client`.
   * Xác minh **Client secret** được đặt thành **No client secret** (vì đây là public client dành cho SPA).
   * Xác minh **Authentication flows** (Luồng xác thực) có:
     * ✅ `ALLOW_USER_SRP_AUTH`
     * ✅ `ALLOW_REFRESH_TOKEN_AUTH`

---

### 3. Lưu lại thông tin chi tiết của Cognito

Hãy đảm bảo bạn đã sao chép và lưu lại các thông tin xác thực sau từ trang **User Pool Overview**:
*   **User Pool ID:** `ap-southeast-1_xxxxxxxxx`
*   **App Client ID** (tìm thấy trong tab App Clients): `xxxxxxxxxxxxxxxxxxxxxxxxxx`

> [!IMPORTANT]
> Bạn sẽ cần hai giá trị này khi cấu hình authorizer cho API Gateway và các biến môi trường cho ứng dụng frontend ở local sau này.