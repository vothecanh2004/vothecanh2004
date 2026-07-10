---
title: "Lưu trữ Frontend với S3 & CloudFront"
date: 2024-01-01
weight: 10
pre: " <b> 5.10. </b> "
---

# Lưu trữ & Phân phối Frontend với S3 và CloudFront (OAC)

Trong bước này, bạn sẽ cấu hình lưu trữ trang web tĩnh cho ứng dụng frontend. Chúng ta sử dụng **Amazon S3** làm kho chứa và **Amazon CloudFront** để phân phối nội dung toàn cầu, đồng thời áp dụng cơ chế bảo mật **Origin Access Control (OAC)** để ngăn chặn truy cập công cộng trực tiếp vào S3.

---

### 1. Tạo S3 Bucket

1. Mở **[Amazon S3 console](https://console.aws.amazon.com/s3/)**.
2. Trên menu trái chọn **General purpose buckets**, nhấp chọn **Create bucket**.
3. Điền biểu mẫu thiết lập:
   * **Bucket name:** Nhập `webquiz-dev-frontend-YOUR_ACCOUNT_ID` (Thay `YOUR_ACCOUNT_ID` bằng AWS Account ID của bạn).
   * **Object Ownership:** Chọn **ACLs disabled / Bucket owner enforced** (mặc định).
   * **Block Public Access settings for this bucket:** Đảm bảo tick chọn ✅ **Block all public access** (Chặn toàn bộ truy cập công cộng).
   * **Bucket Versioning:** Chọn **Enable**.
   * Nhấp chọn **Create bucket**.

---

### 2. Tạo CloudFront Distribution sử dụng OAC

1. Mở **[Amazon CloudFront console](https://console.aws.amazon.com/cloudfront/)**.
2. Nhấp chọn **Create distribution**.
3. **Step 1 - Origin:**
   * **Origin domain:** Chọn S3 bucket vừa tạo ở bước trên (`webquiz-dev-frontend-YOUR_ACCOUNT_ID.s3.amazonaws.com`).
   * **Origin access:** Chọn **Origin access control settings (recommended)**.
   * Nhấp chọn **Create control setting**:
     * Giữ nguyên cấu hình tên mặc định và đảm bảo chọn **Sign requests (recommended)**.
     * Nhấp chọn **Create**.
4. **Step 2 - Default cache behavior:**
   * **Viewer protocol policy:** Chọn **Redirect HTTP to HTTPS** (định tuyến an toàn).
   * **Allowed HTTP methods:** Chọn **GET, HEAD, OPTIONS**.
5. **Step 3 - Settings:**
   * **Default root object:** Nhập `index.html`.
6. Nhấp chọn **Create distribution**.
7. Sau khi tạo xong, một thông báo màu vàng sẽ xuất hiện. Hãy copy đoạn mã **S3 Bucket Policy** do CloudFront tự động sinh ra để chuẩn bị dán vào cấu hình S3 ở bước tiếp theo.

---

### 3. Cấu hình S3 Bucket Policy

1. Quay lại trang chi tiết S3 bucket `webquiz-dev-frontend-YOUR_ACCOUNT_ID` trong S3 console.
2. Chọn tab **Permissions**.
3. Kéo xuống mục **Bucket policy** và nhấp chọn **Edit**.
4. Dán đoạn mã phân quyền do CloudFront sinh ra (cho phép CloudFront truy cập đọc đối tượng tĩnh):
   ```json
   {
       "Version": "2018-10-17",
       "Statement": [
           {
               "Sid": "AllowCloudFrontOAC",
               "Effect": "Allow",
               "Principal": {
                   "Service": "cloudfront.amazonaws.com"
               },
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::webquiz-dev-frontend-YOUR_ACCOUNT_ID/*",
               "Condition": {
                   "StringEquals": {
                       "AWS:SourceArn": "arn:aws:cloudfront::YOUR_ACCOUNT_ID:distribution/YOUR_DISTRIBUTION_ID"
                   }
               }
           }
       ]
   }
   ```
   *(Đảm bảo đã thay `YOUR_ACCOUNT_ID` và `YOUR_DISTRIBUTION_ID` bằng các mã thực tế của bạn).*
5. Nhấn **Save changes**.

---

### 4. Cấu hình định tuyến Single Page Application (SPA)

Để đảm bảo các bộ định tuyến phía client hoạt động mượt mà không gặp lỗi `403 Access Denied` khi người dùng tải lại trang (reload):

1. Quay lại trang chi tiết CloudFront Distribution.
2. Chọn tab **Error pages**.
3. Tạo 2 phản hồi lỗi tùy chỉnh bằng cách nhấp chọn **Create custom error response**:
   * **Lỗi thứ nhất:**
     * **HTTP error code:** Chọn **403: Forbidden**.
     * **Customize error response:** Chọn **Yes**.
     * **Response page path:** Nhập `/index.html`.
     * **HTTP response code:** Chọn **200: OK**.
     * Nhấp chọn **Create**.
   * **Lỗi thứ hai:**
     * **HTTP error code:** Chọn **404: Not Found**.
     * **Customize error response:** Chọn **Yes**.
     * **Response page path:** Nhập `/index.html`.
     * **HTTP response code:** Chọn **200: OK**.
     * Nhấp chọn **Create**.

---

### 5. Build và Deploy Frontend lên Cloud

1. Mở thư mục mã nguồn frontend ở máy cục bộ.
2. Cấu hình file môi trường frontend (ví dụ `.env` hoặc `config.js`) với các thông số AWS đã lưu:
   ```javascript
   const config = {
       COGNITO_USER_POOL_ID: "ap-southeast-1_xxxxxxxxx",
       COGNITO_APP_CLIENT_ID: "xxxxxxxxxxxxxxxxxxxxxxxxxx",
       REST_API_ENDPOINT: "https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/dev",
       WEBSOCKET_API_ENDPOINT: "wss://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/dev"
   };
   ```
3. Đóng gói mã nguồn tĩnh frontend:
   ```bash
   npm run build
   ```
4. Đẩy (upload) toàn bộ các file tĩnh trong thư mục đóng gói (thường là `dist/` hoặc `build/`) lên S3 bucket thông qua S3 Console hoặc sử dụng lệnh AWS CLI:
   ```bash
   aws s3 sync dist/ s3://webquiz-dev-frontend-YOUR_ACCOUNT_ID
   ```
5. Tại trang tổng quan CloudFront, copy địa chỉ **Distribution Domain Name** (ví dụ: `https://xxxxxxxxxx.cloudfront.net`) dán vào trình duyệt web và bắt đầu kiểm thử trò chơi!
