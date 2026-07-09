---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# WebQuiz - Hệ thống trắc nghiệm thời gian thực
## Giải pháp AWS Serverless cho hệ thống chơi trắc nghiệm đồng bộ thời gian thực

### 1. Tóm tắt điều hành
**WebQuiz** là một nền tảng trắc nghiệm thời gian thực cho phép quản trò tạo các bộ câu hỏi, mở phòng đấu trực tiếp, mời người chơi tham gia bằng mã số PIN và vận hành các phiên chơi đồng bộ. Hệ thống được thiết kế đặc biệt dựa trên kiến trúc không máy chủ (Serverless) trên AWS nhằm giải quyết bài toán giao tiếp thời gian thực độ trễ thấp và tự động co giãn linh hoạt khi có lượng lớn người chơi tham gia đồng thời.

---

### 2. Ý tưởng & Mục tiêu (Tuyên bố vấn đề)

#### 2.1 Bối cảnh & Bài toán
*   **Mục đích hệ thống:**
    **WebQuiz** được phát triển như một nền tảng trắc nghiệm trực tuyến gọn nhẹ, có khả năng co giãn linh hoạt và tối ưu chi phí, phục vụ cho việc học tập tương tác, hoạt động lớp học, đào tạo trực tuyến, workshop và các buổi gắn kết nội bộ đội ngũ.
*   **Đối tượng khách hàng:**
    Nền tảng hướng tới hai nhóm đối tượng chính: quản trò (giáo viên, giảng viên, người tổ chức sự kiện/workshop, đội ngũ nhân sự doanh nghiệp) cần vận hành phòng chơi mượt mà; và người tham gia (học sinh, người tham gia workshop, nhân viên) yêu cầu giao diện tham gia trực quan, phản hồi tức thì trên thiết bị cá nhân.
*   **Vấn đề giải quyết:**
    Các nền tảng trắc nghiệm truyền thống thường gặp hạn chế về tương tác thời gian thực, cập nhật bảng xếp hạng chậm trễ và tốn chi phí duy trì máy chủ chạy liên tục (always-on). **WebQuiz** giải quyết bài toán này bằng kiến trúc không máy chủ (Serverless) kết hợp WebSocket API để duy trì kết nối hai chiều độ trễ thấp và cơ sở dữ liệu DynamoDB có hiệu năng cao, đem lại trải nghiệm chơi game tối ưu với ngân sách tối giản.

#### 2.2 Mục tiêu cụ thể
*   **Output mong muốn:**
    * Hạ tầng đám mây Serverless hoàn chỉnh trên AWS hỗ trợ quản lý bộ câu hỏi (quiz) và phòng chơi trực tiếp.
    * Hệ thống giám sát (Monitoring): Bảng điều khiển giám sát (CloudWatch Dashboards), ghi nhật ký tập trung (CloudWatch Logs) và hệ thống cảnh báo tự động (CloudWatch Alarms) để theo dõi hiệu năng.
    * Tài liệu hướng dẫn triển khai ứng dụng Serverless chi tiết từ đầu đến cuối (end-to-end).
*   **Tiêu chí đánh giá thành công:**
    * Hệ thống hỗ trợ lượng lớn người chơi tham gia đồng thời trong một phòng đấu mà không gặp hiện tượng nghẽn hoặc hết thời gian chờ (timeout).
    * Thời gian phản hồi từ khi người chơi gửi đáp án đến khi nhận được cập nhật điểm số diễn ra tức thì trong thời gian thực.
    * Tổng chi phí vận hành hàng tháng cho môi trường phát triển (development) được tối ưu hóa ở mức tối thiểu.

#### 2.3 Phù hợp với chương trình
*   **Use-case gắn với FCAJ / AWS:**
    Dự án mô phỏng hoạt động tương tác trong giáo dục và đào tạo thực tế, áp dụng trực tiếp các dịch vụ Serverless và tích hợp ứng dụng cốt lõi của AWS (Lambda, API Gateway, DynamoDB, Cognito, EventBridge, CloudFront, S3) nằm trong chương trình học.
*   **Đúng trọng tâm Cloud:**
    Giải pháp tập trung sâu vào tính hiệu quả của mô hình Serverless, High Availability, Security và Scalability, hoàn toàn phù hợp với các tiêu chí đánh giá kiến trúc ứng dụng đám mây hiện đại và tối ưu chi phí.
*   **Giải pháp đề xuất:**
    Xây dựng hệ thống trắc nghiệm thời gian thực không máy chủ trên AWS:
    1.  **Frontend:** Được đóng gói dưới dạng Single Page Application (SPA), lưu trữ tĩnh trên **Amazon S3** và phân phối toàn cầu qua **Amazon CloudFront** để tải trang nhanh và tối ưu chi phí.
    2.  **API Gateway (HTTP API):** Cung cấp các REST endpoints phục vụ nghiệp vụ (CRUD bộ câu hỏi, quản lý phòng đấu) được bảo vệ bởi xác thực JWT của **Cognito User Pool**.
    3.  **API Gateway (WebSocket API):** Thiết lập kết nối hai chiều liên tục giữa máy khách và backend, đảm bảo truyền tải thông điệp chơi game thời gian thực với độ trễ thấp.
    4.  **Tầng Tính toán (AWS Lambda):** Thực thi logic nghiệp vụ (tạo phòng, kiểm tra kết nối, tính điểm, cập nhật trạng thái) theo nhu cầu thực tế, tự động co giãn và chỉ tính tiền khi chạy.
    5.  **Database & Tích hợp sự kiện:** Lưu trữ toàn bộ dữ liệu ứng dụng trong **Amazon DynamoDB** ở chế độ On-demand, sử dụng **Amazon EventBridge** để định tuyến sự kiện bất đồng bộ (tính điểm, lưu kết quả chung cuộc).
    6.  **Xác thực & Bảo mật:** Tích hợp **Cognito** cho tính năng đăng ký/đăng nhập của quản trò và cấu hình quyền hạn qua **AWS IAM Roles** tuân thủ nguyên tắc đặc quyền tối thiểu.
*   **Lợi ích và ROI (Hiệu quả đầu tư):**
    *   **Tối ưu hóa tài nguyên & Chi phí:** Mô hình Serverless pay-as-you-go giúp giảm chi phí xuống mức tối thiểu dựa trên lưu lượng sử dụng thực tế và tự động giảm về 0 khi không có người dùng truy cập.
    *   **Độ tin cậy cao & Độ trễ thấp:** WebSocket giúp cập nhật dữ liệu dưới 1 giây. DynamoDB cung cấp tốc độ đọc ghi mili-giây, kết hợp Lambda đảm bảo ứng dụng luôn sẵn sàng trên nhiều Availability Zones.
    *   **Bảo mật tối đa:** Phân quyền IAM nghiêm ngặt, xác thực token Cognito JWT và cơ chế CloudFront OAC bảo vệ an toàn cho dữ liệu lưu trữ S3.

---

### 3. Kiến trúc giải pháp

Hệ thống sử dụng kiến trúc Serverless hướng sự kiện trên AWS nhằm tối đa hóa khả năng co giãn và tối ưu hóa chi phí vận hành.

#### 3.1 Sơ đồ kiến trúc hệ thống

![Sơ đồ kiến trúc hệ thống](/images/2-Proposal/webquiz_architecture.png)

#### 3.2 Các dịch vụ AWS sử dụng

| Phân nhóm | Dịch vụ AWS | Lý do lựa chọn & Vai trò |
| :--- | :--- | :--- |
| **Compute** | AWS Lambda | Chạy runtime Node.js 20 thực thi logic backend theo nhu cầu (CRUD quiz, quản lý kết nối, tính điểm), tự động co giãn theo số lượng request và chỉ tính phí khi chạy. |
| **Storage** | Amazon S3 & CloudFront | **Amazon S3** lưu trữ mã nguồn tĩnh frontend. **Amazon CloudFront** phân phối nội dung toàn cầu qua giao thức HTTPS bảo mật, kết hợp cơ chế Origin Access Control (OAC) ngăn chặn truy cập trực tiếp vào S3 bucket. |
| **Database** | Amazon DynamoDB | Cơ sở dữ liệu NoSQL Serverless cấu hình ở chế độ On-Demand, lưu trữ toàn bộ dữ liệu của hệ thống: thông tin người dùng, bộ câu hỏi, phòng đấu, trạng thái game và kết quả chung cuộc. |
| **Networking** | Amazon API Gateway | Khởi tạo hai loại API: HTTP API cho các REST endpoint độ trễ thấp (CRUD quiz, tạo phòng) và WebSocket API để duy trì kết nối hai chiều thời gian thực giữa máy khách và máy chủ. |
| **Security** | Amazon Cognito, IAM, WAF | **Cognito** quản lý đăng ký/đăng nhập của người dùng và cấp mã JWT. **IAM Role** phân quyền theo nguyên tắc đặc quyền tối thiểu. **AWS WAF** (tùy chọn) bảo vệ API khỏi các cuộc tấn công web. |
| **Monitoring** | Amazon CloudWatch | Thu thập log ứng dụng, đo lường các chỉ số hiệu năng (metrics), kích hoạt cảnh báo tự động qua SNS khi xảy ra lỗi và trực quan hóa qua CloudWatch Dashboard. |
| **Messaging** | Amazon EventBridge | Định tuyến sự kiện không đồng bộ nhằm phân rã các tiến trình nghiệp vụ phức tạp (ví dụ: người chơi nộp đáp án sẽ kích hoạt tính điểm và lưu kết quả dưới nền). |
| **CI/CD** | AWS CodePipeline, CodeBuild | (Tùy chọn) Tự động hóa quy trình kiểm thử và phát hành mã nguồn lên môi trường chạy thực tế. |

---

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai
Dự án được phân chia thành 2 giai đoạn cụ thể diễn ra trong vòng 2 tuần:
1.  **Thiết lập nền tảng, Cơ sở dữ liệu & Phát triển API (Tuần 1):** Thiết lập tài khoản AWS, cấu hình AWS CLI, chuẩn bị quyền IAM, khởi tạo nhóm người dùng Cognito User Pool, thiết kế và tạo các bảng dữ liệu DynamoDB, lập trình hàm Lambda cho tính năng quản lý (CRUD) bộ câu hỏi và phòng chơi. Xây dựng API Gateway HTTP và WebSocket kết hợp các bộ xử lý kết nối.
2.  **Tích hợp sự kiện, Triển khai giao diện & Giám sát (Tuần 2):** Cấu hình EventBridge định tuyến luồng nộp câu trả lời, tính toán điểm số và lưu trữ kết quả. Đóng gói mã nguồn giao diện (SPA), triển khai lên S3 cùng phân phối CloudFront CDN, thiết lập CloudWatch logs, cảnh báo (alarms) và bảng điều khiển (dashboard) giám sát, thực hiện kiểm thử toàn trình.

#### Yêu cầu chuẩn bị (Prerequisites)
*   **Tài nguyên & Công cụ:** Tài khoản AWS với quyền hạn IAM cần thiết, AWS CLI đã cấu hình, môi trường chạy Node.js, công cụ kiểm thử API (Postman) và kiểm thử WebSocket (wscat).
*   **Kiến thức cơ bản:** Kiến trúc không máy chủ (Serverless), AWS Lambda, thiết kế NoSQL DynamoDB, API Gateway (HTTP & WebSocket), EventBridge và tích hợp ứng dụng giữa máy khách (frontend) và máy chủ (backend).

---

### 5. Lộ trình & Mốc triển khai
*   **Tuần 1: Thiết lập nền tảng, Cơ sở dữ liệu & Phát triển API**
    *   Xác định phạm vi dự án, cấu hình bộ công cụ AWS CLI ở máy cục bộ và chuẩn bị quyền IAM.
    *   Khởi tạo Amazon Cognito User Pool để phục vụ xác thực người dùng quản trò.
    *   Thiết kế và triển khai cấu trúc bảng đơn (single-table schema) trên DynamoDB (quizzes, questions, rooms, connections, game states, results).
    *   Lập trình các hàm Lambda cơ bản phục vụ CRUD bộ câu hỏi và logic tạo phòng đấu.
    *   Khởi tạo API Gateway HTTP và WebSocket API, cấu hình định tuyến REST/WebSocket và tích hợp Cognito JWT Authorizer.
    *   Kiểm thử tính ổn định của luồng kết nối hai chiều giữa quản trò và người chơi.
*   **Tuần 2: Tích hợp sự kiện, Triển khai Frontend & Nghiệm thu**
    *   Cấu hình các quy tắc định tuyến sự kiện trên Amazon EventBridge.
    *   Lập trình hàm Lambda `score-calculator` tính điểm và `game-results-saver` lưu kết quả chung cuộc dưới nền.
    *   Đóng gói mã nguồn frontend (SPA), triển khai lên S3 và cấu hình CDN phân phối Amazon CloudFront.
    *   Tích hợp ứng dụng client với các API Gateway HTTP và WebSocket.
    *   Thiết lập CloudWatch Logs, cấu hình các bộ alarms cảnh báo lỗi và dashboard giám sát.
    *   Thực hiện kiểm thử chịu tải (load testing), khắc phục lỗi và hoàn thiện tài liệu bàn giao.

---

### 6. Ước tính ngân sách
Dưới đây là bảng tính chi phí chi tiết hàng tháng của hệ thống dựa trên công cụ ước tính AWS Pricing Calculator (đã cấu hình sẵn cho kiến trúc WebQuiz trong môi trường Workshop):

| STT | Dịch vụ AWS | Cấu hình & Tham số tính toán | Chi phí/Tháng |
| :--- | :--- | :--- | :--- |
| 1 | AWS Lambda | 1 triệu requests/tháng, bộ nhớ 256MB, thời gian chạy 1s | ~ $5.00 |
| 2 | Amazon DynamoDB | Chế độ On-demand, 1 triệu RCU/WCU mỗi tháng | ~ $10.00 |
| 3 | Amazon API Gateway | HTTP API: 1 triệu reqs; WebSocket: 1 triệu messages | ~ $5.00 |
| 4 | Amazon CloudFront | 10GB dung lượng truyền dữ liệu, CloudFront Free Tier | ~ $5.00 |
| 5 | Amazon S3 | 5GB Standard Storage, 100,000 requests GET/PUT | ~ $0.50 |
| 6 | Amazon EventBridge | 1 triệu sự kiện tùy chỉnh được định tuyến | ~ $1.00 |
| 7 | Amazon CloudWatch | 10GB lưu trữ logs, 10 metrics tùy chỉnh và dashboards | ~ $3.00 |
| **Tổng** | **Ước tính chi phí hàng tháng** | | **~ $29.50** |

*Lưu ý:* Chi phí hạ tầng cloud có thể giảm đáng kể hơn nữa trong môi trường phát triển (Development) bằng cách:
*   Tận dụng tối đa chương trình AWS Free Tier cho Lambda, S3 và CloudFront.
*   Tối ưu hóa các thao tác đọc/ghi trên DynamoDB, giữ kích thước payload dữ liệu nhỏ gọn để giảm tiêu hao dung lượng.
*   Điều chỉnh thời gian lưu trữ log của CloudWatch xuống mức ngắn hạn thay vì lưu vô hạn để tiết kiệm dung lượng lưu trữ logs.

---

### 7. Đánh giá rủi ro

#### Ma trận rủi ro

| Rủi ro xác định | Khả năng xảy ra | Mức độ ảnh hưởng | Chiến lược giảm thiểu & Phòng ngừa |
| :--- | :--- | :--- | :--- |
| **Mất kết nối WebSocket giữa trận** | Trung bình | Cao | Sử dụng cơ chế DynamoDB TTL để tự động xóa kết nối ảo; cho phép người chơi kết nối lại qua mã PIN phòng và ID người chơi. |
| **Nghẽn cơ sở dữ liệu DynamoDB** | Trung bình | Trung bình | Tối ưu hóa chỉ mục (Index) và cấu trúc bảng đơn, sẵn sàng bật chế độ Provisioned Capacity co giãn tự động khi chịu tải lớn. |
| **Lambda bị timeout khi broadcast tin nhắn** | Trung bình | Cao | Thiết kế các hàm Lambda nhỏ gọn, thực hiện gửi tin nhắn theo các lô (batches) hoặc xử lý qua hàng đợi bất đồng bộ. |
| **Logic tính điểm bị sai lệch** | Thấp | Cao | Lưu giữ lịch sử câu trả lời thô của người chơi để cho phép tính toán lại điểm số nếu phát hiện lỗi logic hệ thống. |
| **Lỗi cấu hình chặn quản trò đăng nhập** | Trung bình | Trung bình | Duy trì môi trường staging độc lập và tài khoản kiểm thử dự phòng để tiếp tục gỡ lỗi ngoại tuyến (offline). |
| **Lạm dụng đặc quyền IAM gây mất an toàn** | Trung bình | Cao | Áp dụng nghiêm ngặt nguyên tắc đặc quyền tối thiểu (Least Privilege) khi thiết lập các IAM policies cho dịch vụ. |

#### Kế hoạch dự phòng (Contingency Plan)
*   **Khôi phục kết nối người chơi:** Trường hợp người chơi bị ngắt kết nối WebSocket giữa trận, client có thể tự động thực hiện reconnect bằng cách gửi mã PIN phòng và ID người chơi để phục hồi trạng thái game mà không mất điểm.
*   **Co giãn cơ sở dữ liệu khẩn cấp:** Nếu DynamoDB vượt ngưỡng tải cho phép gây nghẽn, quản trị viên có thể chuyển đổi nhanh chế độ từ On-Demand sang Provisioned và thiết lập co giãn tự động để đảm bảo thông luồng.
*   **Chia luồng gửi tin nhắn:** Nếu quá trình phát tin nhắn (broadcast) tới nhiều người chơi cùng lúc vượt quá thời gian chạy của Lambda, hệ thống sẽ tự động chia nhỏ danh sách kết nối và gửi bất đồng bộ theo từng đợt.

---

### 8. Các Luồng Xử Lý Hệ Thống & Đặc Điểm Kỹ Thuật

Hệ thống ứng dụng kiến trúc Serverless hướng sự kiện nhằm tách biệt các tương tác người dùng phía trước với các tác vụ xử lý nền. Dưới đây là các luồng hoạt động chính và đặc điểm kiến trúc:

#### 8.1 Các Luồng Xử Lý Cốt Lõi (Core Processing Flows)

*   **Luồng Xác thực (Authentication):**
    Tích hợp với Amazon Cognito để quản lý định danh người dùng. Các yêu cầu API được xác thực bằng middleware xác thực mã token JWT, đảm bảo cơ chế phân quyền phi trạng thái (stateless) an toàn giữa client và backend.
*   **Quản lý Phòng chơi & Vòng đời Kết nối:**
    Khi quản trò khởi tạo một phòng đấu, một mã PIN ngẫu nhiên được sinh ra và lưu vào DynamoDB. Người chơi tham gia phòng bằng cách xác thực mã PIN này. Mã kết nối WebSocket (Connection ID) được đăng ký trong bảng chuyên biệt để theo dõi trạng thái.
*   **Xử lý Sự kiện Bất đồng bộ:**
    Sử dụng Amazon EventBridge để tách biệt các tác vụ chơi game nặng khỏi luồng xử lý WebSocket chính. Khi người chơi gửi câu trả lời, sự kiện sẽ kích hoạt EventBridge để chuyển tiếp thông điệp tính toán điểm sang hàm Lambda xử lý nền.
*   **Cập nhật Bảng xếp hạng & Dọn dẹp trạng thái:**
    Sau khi điểm số được tính toán, thông điệp cập nhật được phát (broadcast) qua WebSocket tới tất cả máy khách để cập nhật bảng xếp hạng tức thì. Khi game kết thúc, tiến trình tự động dọn dẹp kết nối cũ và cập nhật bảng kết quả chung cuộc.

#### 8.2 Đặc Điểm Kiến Trúc (Architectural Strengths)

*   **Khả năng mở rộng & Phân rã (Scalability & Decoupling):**
    Các lớp Frontend, REST API và WebSocket hoạt động độc lập. AWS Lambda tự co giãn tức thì theo lượng request mà không cần vận hành máy chủ, kết hợp với EventBridge giúp giảm tải cho cơ sở dữ liệu trong các phiên chơi cao điểm.
*   **Tính nhất quán dữ liệu:**
    Sử dụng các cơ chế transaction và cơ chế đọc nhất quán (consistent read) trên DynamoDB giúp dữ liệu phòng chơi, danh sách kết nối và câu hỏi luôn đồng bộ, tránh trùng lặp thông tin người chơi.
*   **Công nghệ Frontend Hiện đại:**
    Xây dựng dưới dạng Single Page Application (SPA) tích hợp trực tiếp với API Gateway REST và WebSocket, giúp cập nhật trạng thái bảng điểm cục bộ mượt mà và tối ưu hóa trải nghiệm người dùng.

---

### 9. Kết quả kỳ vọng
*   **Cải tiến kỹ thuật:**
    *   Hệ thống co giãn không máy chủ hiệu năng cao, tự động xử lý các đợt lưu lượng truy cập đột biến của người chơi mà không cần can thiệp hạ tầng thủ công.
    *   Gửi nhận thông điệp hai chiều độ trễ cực thấp sử dụng API Gateway WebSocket, đảm bảo cập nhật trạng thái game và bảng điểm tức thì.
    *   Tách biệt rõ ràng giữa các luồng nghiệp vụ và quy trình xử lý bất đồng bộ bằng cách định tuyến các sự kiện trò chơi qua EventBridge.
*   **Giá trị lâu dài:**
    *   Xây dựng một kiến trúc Serverless mẫu có tính sẵn sàng cao, sẵn sàng phục vụ các ứng dụng thực tế về trò chơi tương tác trực tiếp hoặc truyền thông điệp thời gian thực.
    *   Tối giản hóa nỗ lực bảo trì hệ thống và tối ưu hóa ngân sách vận hành cloud khi ứng dụng không hoạt động.
    *   Cung cấp một kiến trúc hệ thống có khả năng mở rộng, sẵn sàng tích hợp với các Hệ thống Quản lý Học tập (LMS) bên thứ ba hoặc hỗ trợ các tính năng nhiều trò chơi nâng cao.

