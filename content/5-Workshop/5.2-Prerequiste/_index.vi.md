---
title : "Các bước chuẩn bị"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

### Các bước chuẩn bị (Prerequisites)

Để hoàn thành bài thực hành workshop xây dựng ứng dụng trắc nghiệm trực tuyến **WebQuiz**, bạn cần chuẩn bị môi trường phát triển cục bộ và phân quyền tài khoản AWS tương ứng.

---

#### 1. Quyền hạn IAM (IAM Permissions)

Bạn cần sử dụng một tài khoản IAM User hoặc IAM Role có các quyền hạn để dọn dẹp và vận hành các tài nguyên trong workshop. 

{{% notice note %}}
Để thực hành lab cá nhân một cách thuận tiện nhất và tránh lỗi phân quyền (Access Denied), chúng tôi khuyên bạn nên sử dụng quyền **AdministratorAccess** cho tài khoản thực hành của mình.
{{% /notice %}}

Nếu bạn muốn cấu hình chính sách phân quyền tối thiểu (Least Privilege), hãy làm theo các bước sau để tạo IAM Policy trên AWS Console:

1. Đăng nhập vào **AWS Management Console** và tìm kiếm dịch vụ **IAM**.
2. Chọn **Policies** ở thanh điều hướng bên trái và nhấn **Create policy**.
3. Tại giao diện tạo policy, chuyển sang trình chỉnh sửa **JSON** (JSON editor), xóa hết mã mẫu cũ và dán đoạn chính sách JSON dưới đây vào:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DynamoDBCleanup",
      "Effect": "Allow",
      "Action": [
        "dynamodb:DeleteTable",
        "dynamodb:DescribeTable",
        "dynamodb:ListTables"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CognitoCleanup",
      "Effect": "Allow",
      "Action": [
        "cognito-idp:DeleteUserPool",
        "cognito-idp:DescribeUserPool",
        "cognito-idp:DeleteUserPoolClient"
      ],
      "Resource": "*"
    },
    {
      "Sid": "S3Cleanup",
      "Effect": "Allow",
      "Action": [
        "s3:DeleteBucket",
        "s3:DeleteObject",
        "s3:DeleteObjectVersion",
        "s3:ListBucket",
        "s3:ListBucketVersions",
        "s3:GetBucketLocation"
      ],
      "Resource": "*"
    },
    {
      "Sid": "APIGatewayCleanup",
      "Effect": "Allow",
      "Action": [
        "apigateway:DELETE",
        "apigateway:GET"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CloudFrontCleanup",
      "Effect": "Allow",
      "Action": [
        "cloudfront:DeleteDistribution",
        "cloudfront:GetDistribution",
        "cloudfront:UpdateDistribution"
      ],
      "Resource": "*"
    },
    {
      "Sid": "LambdaCleanup",
      "Effect": "Allow",
      "Action": [
        "lambda:DeleteFunction",
        "lambda:GetFunction",
        "lambda:RemovePermission"
      ],
      "Resource": "*"
    },
    {
      "Sid": "EventBridgeCleanup",
      "Effect": "Allow",
      "Action": [
        "events:DeleteEventBus",
        "events:DeleteRule",
        "events:RemoveTargets",
        "events:DescribeRule",
        "events:DescribeEventBus"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CloudWatchCleanup",
      "Effect": "Allow",
      "Action": [
        "cloudwatch:DeleteAlarms",
        "cloudwatch:DescribeAlarms",
        "logs:DeleteLogGroup",
        "logs:DescribeLogGroups"
      ],
      "Resource": "*"
    },
    {
      "Sid": "IAMCleanup",
      "Effect": "Allow",
      "Action": [
        "iam:DeleteRole",
        "iam:DeleteRolePolicy",
        "iam:DetachRolePolicy",
        "iam:ListRolePolicies",
        "iam:ListAttachedRolePolicies",
        "iam:GetRole"
      ],
      "Resource": "*"
    }
  ]
}
```

5. Nhấn **Next**.
6. Nhập tên cho chính sách (ví dụ: `WebQuizCleanupPolicy`), thêm mô tả tùy chọn, và nhấn **Create policy**.
7. Tiến hành gán chính sách này vào **IAM User** của bạn (truy cập menu **Users** -> chọn User của bạn -> **Add permissions** -> **Attach policies directly** và chọn chính sách `WebQuizCleanupPolicy` vừa tạo).

---

#### 2. Cài đặt môi trường phát triển cục bộ (Local Workspace Setup)

Hãy đảm bảo máy trạm của bạn đã được cài đặt sẵn các công cụ sau:

##### A. AWS CLI
*   Cài đặt AWS CLI và cấu hình tài khoản IAM của bạn bằng lệnh:
    ```bash
    aws configure
    ```
    Hãy đảm bảo khai báo Region mặc định (ví dụ: `ap-southeast-1`).

##### B. Node.js & npm
*   Mã nguồn WebQuiz backend (Lambda) và frontend được viết bằng Node.js. Bạn cần cài đặt Node.js phiên bản **18.x** hoặc **20.x** (LTS).
*   Kiểm tra cài đặt:
    ```bash
    node -v
    npm -v
    ```

##### C. Git & wscat
*   Sử dụng Git để tải mã nguồn dự án. Cài đặt thêm `wscat` để test WebSocket API.
    ```bash
    git --version
    npm install -g wscat
    ```

---

#### 3. Tải mã nguồn dự án (Project Source Code)

Mã nguồn của ứng dụng WebQuiz được chia làm 2 thư mục chính:
1.  **webquiz-frontend/**: Giao diện React tĩnh hiển thị bộ câu hỏi, phòng chờ và giao diện chơi thời gian thực.
2.  **webquiz-backend/** (Backend Lambda): Mã nguồn Node.js cho các hàm Lambda xử lý nghiệp vụ REST API (tạo phòng, tham gia), WebSocket (giao tiếp thời gian thực), và EventBridge (tính điểm, lưu kết quả).

Hãy clone dự án từ repository workshop của bạn (thay thế URL bằng link repo thực tế của bạn nếu có):
```bash
git clone https://github.com/USER/webquiz-workshop.git
cd webquiz-workshop
```

---

#### 4. Lựa chọn Phương thức Triển khai Hạ tầng

Trong workshop này, bạn có thể lựa chọn 1 trong 2 phương thức sau để xây dựng hệ thống:

*   **Lựa chọn A (Khuyên dùng - Nhanh chóng): Triển khai tự động bằng AWS CloudFormation**
    *   Hạ tầng sẽ được khởi tạo hoàn toàn tự động.
    *   Các chương tiếp theo sẽ đóng vai trò là hướng dẫn để bạn **Kiểm tra và Xác thực** cấu hình tài nguyên đã được tạo thay vì phải tự tạo lại.
*   **Lựa chọn B: Tự tay cấu hình thủ công từng bước (Manual)**
    *   Bạn sẽ **bỏ qua** bước 4 (chạy CloudFormation) dưới đây.
    *   Bắt đầu từ chương tiếp theo, bạn sẽ tự tay làm theo từng bước hướng dẫn trên AWS Console để tự xây dựng kiến trúc Serverless từ con số 0.

---

#### Hướng dẫn cho Lựa chọn A: Triển khai tự động bằng CloudFormation

Để chuẩn bị nhanh môi trường làm workshop, chúng ta có thể deploy CloudFormation template (đây là template mẫu tự động tạo tài nguyên).

{{% notice info %}}
**Lưu ý về phạm vi triển khai**: 
Nếu chọn triển khai tự động bằng CloudFormation, kiến trúc cơ bản như DynamoDB, Cognito, S3, CloudFront, Lambda, API Gateway và EventBridge sẽ được thiết lập tự động. Bạn có thể **bỏ qua bước tạo thủ công** và chỉ cần tập trung vào việc **xác minh cấu hình**, cập nhật biến môi trường, tải mã nguồn Frontend lên S3 và kiểm thử hệ thống.
{{% /notice %}}

1. Trình duyệt sẽ mở bảng điều khiển CloudFormation Console và tự động điền sẵn các cấu hình (sử dụng file `architecture.yaml` đi kèm).
2. Tại màn hình **Specify stack details**, kiểm tra các tham số rồi click **Next**.
3. Cuộn xuống cuối trang Review, tích chọn **I acknowledge that AWS CloudFormation might create IAM resources with custom names.** và click **Submit** để bắt đầu triển khai.
4. Đợi cho đến khi quá trình khởi tạo hoàn tất (Trạng thái chuyển sang `CREATE_COMPLETE`).

Sau khi CloudFormation Stack triển khai thành công, phần lớn hệ thống backend của bạn đã sẵn sàng! Ở các bài thực hành tiếp theo, bạn sẽ tiến hành cấu hình Frontend, tải code tĩnh lên S3, và thực hiện kiểm thử sự kết nối luồng chơi game theo thời gian thực (Realtime WebSocket) và kiến trúc sự kiện (EventBridge).
