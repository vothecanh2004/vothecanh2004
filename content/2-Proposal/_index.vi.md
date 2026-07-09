---
title: "Bản đề xuất"
date: 2026-04-20
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# ĐỀ XUẤT DỰ ÁN

## Tên dự án
SyncQuiz - Hệ thống trắc nghiệm thời gian thực (Real-time Quiz System)

## Mô tả ngắn
SyncQuiz là nền tảng trắc nghiệm thời gian thực cho phép quản trò tạo các bộ câu hỏi, mở phòng đấu trực tiếp, mời người chơi tham gia bằng mã PIN và vận hành các phiên chơi đồng bộ. Người chơi tham gia từ thiết bị cá nhân, gửi đáp án ngay lập tức, nhận cập nhật điểm số và xem bảng xếp hạng.

---

## 1. Tóm tắt điều hành (Executive Summary)

### Giới thiệu dự án
SyncQuiz là nền tảng trắc nghiệm thời gian thực cho phép quản trò (host) tạo các câu hỏi trắc nghiệm, mở phòng đấu trực tiếp, mời người chơi tham gia bằng mã PIN phòng và vận hành các phiên chơi đồng bộ theo thời gian thực. Người chơi có thể tham gia từ thiết bị cá nhân của mình, gửi đáp án ngay lập tức, nhận cập nhật điểm số và xem bảng xếp hạng chung cuộc sau khi trò chơi kết thúc.

### Vấn đề cần giải quyết
- Thiếu giao tiếp thời gian thực giữa quản trò và người chơi.
- Gặp khó khăn trong việc quản lý trạng thái của các phòng đấu trực tiếp và người tham gia.
- Tính toán điểm số và cập nhật bảng xếp hạng bị chậm trễ.
- Khả năng mở rộng hạn chế khi có số lượng lớn người chơi tham gia cùng lúc.
- Chi phí hạ tầng cao do sử dụng các máy chủ chạy liên tục (always-on).
- Triển khai và bảo trì phức tạp đối với các đội ngũ nhỏ hoặc dự án sinh viên.
- Khả năng giám sát yếu, gây khó khăn cho việc phát hiện lỗi trong các phiên chơi trực tiếp.

### Giải pháp tổng quan
Dự án được thiết kế sử dụng kiến trúc không máy chủ (serverless) trên AWS để giảm thiểu việc quản lý hạ tầng, cải thiện khả năng mở rộng và tối ưu hóa chi phí. Frontend được lưu trữ trên Amazon S3 và phân phối qua CloudFront. Người dùng được xác thực qua Amazon Cognito. Dữ liệu được lưu trữ trong DynamoDB. Hệ thống sử dụng API Gateway HTTP API cho các hoạt động REST thông thường và API Gateway WebSocket API cho trải nghiệm chơi trực tiếp. AWS Lambda xử lý các logic backend. EventBridge được sử dụng để kích hoạt các tác vụ không đồng bộ.

### Lợi ích mang lại
- Trải nghiệm chơi trực tiếp thời gian thực mượt mà cho cả quản trò và người chơi.
- Kiến trúc serverless giảm thiểu tối đa nỗ lực vận hành hạ tầng.
- Mô hình thanh toán theo thực tế sử dụng (pay-as-you-go) giúp giảm thiểu chi phí khi lưu lượng truy cập thấp.
- Chế độ on-demand của DynamoDB hỗ trợ các mức tải linh hoạt.
- WebSocket API cho phép giao tiếp tức thời mà không cần tải lại trang.
- EventBridge tăng tính mô-đun của hệ thống và tách biệt rõ ràng logic nghiệp vụ.
- Hệ thống giám sát CloudWatch giúp nhanh chóng phát hiện lỗi, hiện tượng nghẽn (throttling) và các vấn đề về hiệu năng.
- Hệ thống dễ dàng mở rộng trong tương lai để phục vụ trường học, sự kiện, khóa học online và đào tạo doanh nghiệp.

---

## 2. Ý tưởng & Mục tiêu (Problem Statement)

### 2.1 Bối cảnh & Bài toán

#### Mục đích hệ thống
Cung cấp một nền tảng gọn nhẹ, có khả năng mở rộng cao và tiết kiệm chi phí phục vụ cho việc học tập tương tác, hoạt động lớp học, đào tạo trực tuyến, workshop và các buổi gắn kết nội bộ đội ngũ.

#### Đối tượng khách hàng
- Giáo viên, người hướng dẫn trong các lớp học, khóa học.
- Người tổ chức sự kiện, workshop.
- Các đội ngũ doanh nghiệp muốn tổ chức các hoạt động gắn kết nội bộ.
- Người học, người tham gia sự kiện, workshop.

#### Vấn đề cần giải quyết
- Thiếu giao tiếp thời gian thực giữa quản trò và người chơi.
- Gặp khó khăn trong việc quản lý trạng thái của các phòng đấu trực tiếp và người tham gia.
- Tính toán điểm số và cập nhật bảng xếp hạng bị chậm trễ.
- Khả năng mở rộng hạn chế khi có số lượng lớn người chơi tham gia cùng lúc.
- Chi phí hạ tầng cao do sử dụng các máy chủ chạy liên tục (always-on).
- Triển khai và bảo trì phức tạp đối với các đội ngũ nhỏ hoặc dự án sinh viên.
- Khả năng giám sát yếu, gây khó khăn cho việc phát hiện lỗi trong các phiên chơi trực tiếp.

### 2.2 Mục tiêu cụ thể

#### Output mong muốn
- **Output 1:** Hệ thống chạy ổn định, cho phép quản trò tạo quiz, mở phòng đấu với mã PIN, người chơi tham gia, chơi game thời gian thực.
- **Output 2:** Điểm số được tính toán tự động, bảng xếp hạng cập nhật liên tục, kết quả cuối cùng được lưu trữ.
- **Output 3:** Hệ thống được giám sát đầy đủ, chi phí vận hành tối ưu, có khả năng mở rộng.

#### Tiêu chí đánh giá thành công
- **KPI 1:** Hệ thống hỗ trợ ít nhất 50 người chơi đồng thời trong một phòng đấu mà không gặp hiện tượng nghẽn hoặc timeout.
- **KPI 2:** Thời gian phản hồi từ khi người chơi gửi đáp án đến khi nhận được cập nhật điểm số không vượt quá 2 giây.
- **KPI 3:** Tổng chi phí vận hành hàng tháng cho môi trường development không vượt quá $30.

### 2.3 Phù hợp với chương trình

#### Use-case
- **Use-case 1:** Giáo viên tạo quiz cho bài kiểm tra nhanh trong lớp học, mở phòng đấu, học sinh tham gia bằng mã PIN, thực hiện bài kiểm tra thời gian thực, xem kết quả ngay sau khi kết thúc.
- **Use-case 2:** Người tổ chức workshop tạo quiz để kiểm tra kiến thức của người tham gia, mở nhiều phòng đấu đồng thời, xem bảng xếp hạng tổng thể sau workshop.
- **Use-case 3:** Doanh nghiệp tổ chức hoạt động gắn kết nội bộ, tạo các quiz vui, chia đội chơi, xem bảng xếp hạng đội.

#### Phù hợp với Cloud/AWS
Dự án hoàn toàn phù hợp với mô hình Cloud và các dịch vụ AWS, đặc biệt là kiến trúc Serverless:
- Không cần quản lý máy chủ (EC2), giảm thiểu công sức vận hành.
- Thanh toán theo thực tế sử dụng (pay-as-you-go), tối ưu chi phí.
- Khả năng mở rộng tự động dựa trên lưu lượng truy cập.
- Tích hợp đầy đủ các dịch vụ cần thiết (xác thực, database, messaging, monitoring...).

#### Giải pháp đề xuất
- **Frontend:** Ứng dụng web Single Page Application (SPA), được lưu trữ trên Amazon S3 và phân phối toàn cầu qua Amazon CloudFront.
- **Backend:** Các hàm AWS Lambda xử lý logic nghiệp vụ, được tích hợp với Amazon API Gateway.
- **Database:** Amazon DynamoDB (NoSQL) lưu trữ toàn bộ dữ liệu của hệ thống.
- **Cache:** Sử dụng CloudFront cho caching frontend, có thể mở rộng thêm Elasticache cho caching dữ liệu nóng nếu cần.
- **Authentication:** Amazon Cognito quản lý xác thực người dùng.
- **Messaging:** Amazon API Gateway WebSocket API cho giao tiếp thời gian thực, Amazon EventBridge cho xử lý sự kiện không đồng bộ.
- **Monitoring:** Amazon CloudWatch cung cấp logs, metrics, alarms và dashboards.
- **Security:** AWS IAM quản lý phân quyền, AWS WAF (tùy chọn) cho bảo vệ web.
- **CI/CD:** Có thể mở rộng thêm AWS CodePipeline, CodeBuild, CodeDeploy để tự động hóa triển khai.

#### Lợi ích & ROI
- **Chi phí:** Tổng chi phí ước tính hàng tháng cho môi trường development từ $2 đến $30, thấp hơn nhiều so với việc sử dụng máy chủ EC2 chạy liên tục.
- **Hiệu năng:** Hệ thống phản hồi nhanh, hỗ trợ mở rộng tự động dựa trên lưu lượng truy cập.
- **Bảo mật:** Bảo mật tốt nhờ Cognito, IAM, HTTPS, OAC cho S3.
- **Khả năng mở rộng:** Dễ dàng mở rộng hệ thống để hỗ trợ nhiều người chơi hơn, thêm nhiều tính năng mới.

---

## 3. Kiến trúc giải pháp

### 3.1 Sơ đồ kiến trúc hệ thống
(Chèn sơ đồ kiến trúc ở đây)

### 3.2 Các dịch vụ AWS sử dụng

| Nhóm | Dịch vụ AWS | Vai trò |
| --- | --- | --- |
| Compute | AWS Lambda | Thực thi logic backend mà không cần quản lý máy chủ |
| Storage | Amazon S3, Amazon CloudFront | Lưu trữ frontend tĩnh và phân phối toàn cầu |
| Database | Amazon DynamoDB | Lưu trữ toàn bộ dữ liệu ứng dụng (users, quizzes, questions, rooms, connections, game state, results) |
| Networking | Amazon API Gateway (HTTP & WebSocket) | Cung cấp các endpoint REST và WebSocket cho frontend giao tiếp với backend |
| Security | Amazon Cognito, AWS IAM, AWS WAF (tùy chọn) | Quản lý xác thực người dùng và phân quyền truy cập |
| Monitoring | Amazon CloudWatch | Giám sát hệ thống, logs, metrics, alarms, dashboards |
| Messaging | Amazon EventBridge | Xử lý các luồng công việc hướng sự kiện không đồng bộ |
| CI/CD | (Tùy chọn) AWS CodePipeline, CodeBuild, CodeDeploy | Tự động hóa kiểm thử và triển khai |

---

## 4. Triển khai kỹ thuật

### Các giai đoạn triển khai

#### Giai đoạn 1: Thiết lập nền tảng (Weeks 1-3)
- Kế hoạch dự án, xác định phạm vi và kiến trúc.
- Thiết lập tài khoản AWS, cấu hình AWS CLI, chuẩn bị quyền IAM.
- Thiết lập Cognito User Pool, thiết kế và tạo các bảng DynamoDB.
- Tạo IAM role và policies, lập trình các hàm Lambda cho CRUD quiz và quản lý phòng.

#### Giai đoạn 2: Phát triển API & tính năng thời gian thực (Weeks 4-6)
- Tạo API Gateway HTTP API, cấu hình routes, tích hợp Lambda, thêm Cognito JWT Authorizer.
- Tạo API Gateway WebSocket API, lập trình các handler kết nối/ngắt kết nối/tin nhắn.
- Cấu hình EventBridge, lập trình quy trình gửi câu trả lời, bộ tính điểm, bộ lưu kết quả.

#### Giai đoạn 3: Triển khai Frontend & Giám sát (Weeks 7-8)
- Build ứng dụng frontend, deploy lên S3, cấu hình CloudFront.
- Kết nối frontend với các API HTTP và WebSocket.
- Tạo CloudWatch alarms và dashboard, thực hiện kiểm thử toàn trình (end-to-end).
- Tối ưu chi phí, sửa lỗi, hoàn thiện tài liệu.

### Yêu cầu chuẩn bị (Prerequisites)

#### Tài nguyên & Công cụ
- Tài khoản AWS.
- AWS CLI được cài đặt và cấu hình.
- Node.js và npm/yarn cho phát triển frontend.
- Công cụ kiểm thử API (Postman, curl...).
- Công cụ kiểm thử WebSocket (wscat...).

#### Kiến thức yêu cầu
- Kiến thức cơ bản về AWS (Lambda, API Gateway, DynamoDB, Cognito, S3, CloudFront, CloudWatch, EventBridge, IAM).
- Kiến thức về lập trình (JavaScript/TypeScript cho frontend và Lambda).
- Kiến thức về REST API và WebSocket.
- Kiến thức về NoSQL database (DynamoDB).

---

## 5. Lộ trình & Mốc triển khai

| Tuần | Mốc triển khai | Các công việc chính |
| --- | --- | --- |
| Tuần 1 | Kế hoạch dự án & Thiết lập AWS | Xác định phạm vi dự án, xem xét kiến trúc, tạo tài khoản AWS, cấu hình AWS CLI, chuẩn bị quyền IAM. |
| Tuần 2 | Xác thực & Thiết kế cơ sở dữ liệu | Thiết lập Cognito User Pool, thiết kế các bảng DynamoDB (users, quizzes, questions, rooms, connections, game state, results), tạo các bảng. |
| Tuần 3 | Xây dựng nền tảng Backend | Tạo IAM role, cấu hình quyền Lambda, lập trình các hàm Lambda cho CRUD quiz và quản lý phòng. |
| Tuần 4 | Phát triển HTTP API | Tạo API Gateway HTTP API, cấu hình routes, tích hợp Lambda, thêm Cognito JWT Authorizer, kiểm thử các API quản lý quiz và phòng. |
| Tuần 5 | Hệ thống thời gian thực WebSocket | Tạo WebSocket API, lập trình các handler kết nối/ngắt kết nối/tin nhắn, kiểm thử giao tiếp trực tiếp giữa host và player. |
| Tuần 6 | Logic game hướng sự kiện | Cấu hình EventBridge, lập trình quy trình gửi câu trả lời, bộ tính điểm, bộ lưu kết quả và logic bảng xếp hạng. |
| Tuần 7 | Triển khai Frontend | Build ứng dụng frontend, deploy lên S3, cấu hình CloudFront, kết nối frontend với các API HTTP và WebSocket. |
| Tuần 8 | Giám sát, Kiểm thử & Tối ưu | Tạo CloudWatch alarms và dashboard, thực hiện kiểm thử toàn trình (end-to-end), tối ưu chi phí, sửa lỗi, hoàn thiện tài liệu. |

---

## 6. Ước tính ngân sách

### 6.1 Môi trường Development / Workshop

| STT | Dịch vụ | Cấu hình | Chi phí (tháng) |
| --- | --- | --- | ---: |
| 1 | AWS Lambda | 1 triệu request/tháng, thời gian thực thi trung bình 1 giây, bộ nhớ 256 MB | $0 - $5 |
| 2 | Amazon DynamoDB | On-demand, 1 triệu RCU/WCU/tháng | $0 - $10 |
| 3 | Amazon API Gateway | HTTP API: 1 triệu request/tháng, WebSocket: 1 triệu tin nhắn/tháng | $1 - $5 |
| 4 | Amazon CloudFront | 10 GB data transfer/tháng | $0 - $5 |
| 5 | Amazon S3 | 5 GB storage, 100k request/tháng | ~$0.50 |
| 6 | Amazon EventBridge | 1 triệu sự kiện/tháng | $0 - $1 |
| 7 | Amazon CloudWatch | 10 GB log storage, 10 metrics | $0 - $3 |
| | | **Tổng chi phí ước tính** | **~$2 - $30** |

**Ghi chú tối ưu chi phí:**
- Sử dụng DynamoDB on-demand để tự động điều chỉnh theo tải thực tế.
- Sử dụng Lambda thay vì duy trì các máy chủ EC2 luôn chạy.
- Sử dụng bộ nhớ đệm CloudFront để phân phối frontend hiệu quả.
- Thiết lập TTL (Time to Live) trên các dữ liệu tạm thời để tự động dọn dẹp DynamoDB.
- Điều chỉnh thời gian lưu giữ logs (retention policy) của CloudWatch để tránh chi phí lưu trữ log không cần thiết.
- Tránh sử dụng NAT Gateway và các tài nguyên VPC không cần thiết trong môi trường dev.

### 6.2 Môi trường Production

| STT | Dịch vụ | Cấu hình | Chi phí (tháng) |
| --- | --- | --- | ---: |
| 1 | AWS Lambda | 10 triệu request/tháng, thời gian thực thi trung bình 1 giây, bộ nhớ 256 MB | $10 - $20 |
| 2 | Amazon DynamoDB | On-demand, 10 triệu RCU/WCU/tháng | $20 - $50 |
| 3 | Amazon API Gateway | HTTP API: 10 triệu request/tháng, WebSocket: 10 triệu tin nhắn/tháng | $10 - $30 |
| 4 | Amazon CloudFront | 100 GB data transfer/tháng | $5 - $15 |
| 5 | Amazon S3 | 50 GB storage, 1 triệu request/tháng | $1 - $5 |
| 6 | Amazon EventBridge | 10 triệu sự kiện/tháng | $1 - $5 |
| 7 | Amazon CloudWatch | 50 GB log storage, 30 metrics | $5 - $15 |
| 8 | AWS WAF | Bảo vệ web cơ bản | $10 - $20 |
| | | **Tổng chi phí ước tính** | **~$62 - $160** |

---

## 7. Đánh giá rủi ro

### Ma trận rủi ro

| Rủi ro | Khả năng xảy ra | Mức độ ảnh hưởng | Biện pháp giảm thiểu |
| --- | --- | --- | --- |
| Mất kết nối WebSocket giữa chừng trong phiên chơi | Trung bình | Cao | Sử dụng DynamoDB TTL để tự động xóa các phòng đấu, kết nối đã hết hạn; người chơi có thể kết nối lại bằng mã PIN và ID người chơi |
| Nghẽn DynamoDB khi nhiều người chơi cùng gửi đáp án | Trung bình | Trung bình | Phân tích lại mẫu truy cập để tối ưu hóa chỉ mục, chuyển sang chế độ provisioned capacity có tự động mở rộng |
| Lambda bị timeout khi gửi thông điệp broadcast đến quá nhiều kết nối | Trung bình | Cao | Thiết kế các hàm Lambda nhỏ gọn, chia nhỏ danh sách kết nối để gửi theo các lô nhỏ (batches) |
| Logic tính điểm bị sai lệch | Thấp | Cao | Lưu trữ tạm thời đáp án thô của người chơi để thực hiện tính toán lại sau; kiểm thử kỹ lượng logic tính điểm |
| Lỗi cấu hình Cognito chặn Host đăng nhập | Trung bình | Trung bình | Tạm thời mở cổng API công cộng trong môi trường thử nghiệm nội bộ để tiếp tục debug backend |
| Lỗi cấu hình route hoặc authorizer trên API Gateway | Trung bình | Trung bình | Kiểm thử kỹ lượng các API trước khi đưa vào sử dụng |
| Lỗi triển khai CloudFront/S3 khiến frontend không truy cập được | Thấp | Trung bình | Nhà phát triển có thể truy cập trực tiếp S3 tĩnh để kiểm tra trước |
| Chi phí CloudWatch tăng vọt do ghi log quá nhiều | Trung bình | Thấp | Điều chỉnh thời gian lưu giữ logs (retention policy) của CloudWatch; loại bỏ các log không cần thiết |
| Rủi ro bảo mật do cấp quyền IAM quá rộng | Trung bình | Cao | Áp dụng nguyên tắc đặc quyền tối thiểu (least privilege) khi cấu hình IAM policies |

### Kế hoạch dự phòng (Contingency Plan)

- **Kịch bản 1: Mất kết nối WebSocket**
  Người chơi có thể kết nối lại bằng cách sử dụng mã PIN phòng và ID người chơi hiện tại mà không mất điểm số.

- **Kịch bản 2: Nghẽn DynamoDB**
  Phân tích lại mẫu truy cập để tối ưu hóa chỉ mục hoặc chuyển sang chế độ provisioned capacity có tự động mở rộng.

- **Kịch bản 3: Lambda timeout khi broadcast**
  Chia nhỏ danh sách kết nối để gửi theo các lô nhỏ (batches) hoặc sử dụng hàng đợi bất đồng bộ.

- **Kịch bản 4: Logic tính điểm lỗi**
  Lưu trữ tạm thời đáp án thô của người chơi để thực hiện tính toán lại sau.

- **Kịch bản 5: Lỗi deploy CloudFront**
  Nhà phát triển có thể truy cập trực tiếp S3 tĩnh để kiểm tra trước.

- **Kịch bản 6: Chi phí AWS tăng bất thường**
  Sử dụng Cost Explorer để rà soát, giảm thời gian lưu logs và xóa bỏ các tài nguyên dư thừa.

---

## 8. Các luồng xử lý hệ thống & Đặc điểm kỹ thuật

### 8.1 Các luồng xử lý chính

#### Luồng xác thực (Authentication)
1. Người dùng (host) đăng ký/đăng nhập bằng email thông qua Amazon Cognito.
2. Cognito trả về token JWT.
3. Frontend sử dụng token JWT để gọi các API được bảo vệ (tạo quiz, tạo phòng...).
4. API Gateway xác thực token JWT với Cognito trước khi cho phép truy cập Lambda.

#### Luồng xử lý nghiệp vụ (Tạo quiz và phòng, tham gia phòng)
1. Host tạo quiz bằng cách gọi API `POST /quizzes` (được bảo vệ bởi JWT).
2. Lambda `quiz-crud` xử lý yêu cầu, lưu quiz và câu hỏi vào DynamoDB.
3. Host tạo phòng bằng cách gọi API `POST /rooms` (được bảo vệ bởi JWT).
4. Lambda `room-management` tạo mã PIN phòng ngẫu nhiên, lưu phòng vào DynamoDB.
5. Người chơi lấy mã PIN phòng từ host, gọi API `GET /rooms/{pin}` để lấy thông tin phòng.
6. Người chơi gọi API `POST /rooms/{pin}/join` để tham gia phòng, Lambda `room-management` lưu thông tin người chơi vào DynamoDB.

#### Luồng xử lý bất đồng bộ (Tính điểm, lưu kết quả)
1. Người chơi gửi đáp án qua WebSocket với action `SUBMIT_ANSWER`.
2. Lambda `ws-message` xử lý, lưu đáp án vào DynamoDB, sau đó gửi sự kiện `AnswerSubmitted` lên EventBridge.
3. EventBridge kích hoạt Lambda `score-calculator` để tính điểm dựa trên tính đúng đắn của đáp án, thời gian trả lời còn lại và streak.
4. Lambda `score-calculator` cập nhật điểm số của người chơi vào DynamoDB, gửi tin nhắn cập nhật điểm số đến tất cả người chơi qua WebSocket.
5. Khi host kết thúc game bằng action `END_GAME`, Lambda `ws-message` gửi sự kiện `GameEnded` lên EventBridge.
6. EventBridge kích hoạt Lambda `game-results-saver` để lưu kết quả chung cuộc vào DynamoDB.

#### Luồng dữ liệu
- Dữ liệu người dùng: Lưu vào bảng `users` (Cognito + DynamoDB).
- Dữ liệu quiz và câu hỏi: Lưu vào bảng `quizzes` và `questions`.
- Dữ liệu phòng đấu: Lưu vào bảng `rooms`.
- Dữ liệu kết nối WebSocket: Lưu vào bảng `connections`.
- Dữ liệu trạng thái game, người chơi, đáp án, điểm số: Lưu vào bảng `game-state`.
- Dữ liệu kết quả cuối cùng: Lưu vào bảng `game-results`.

#### Luồng thông báo
- Thông báo thời gian thực qua WebSocket: Bắt đầu game, chuyển câu hỏi, cập nhật điểm số, kết thúc game, cập nhật bảng xếp hạng.

### 8.2 Đặc điểm kiến trúc

#### Khả năng mở rộng (Scalability)
- Dễ dàng mở rộng hệ thống để hỗ trợ nhiều người chơi hơn nhờ kiến trúc serverless Lambda và DynamoDB on-demand.
- Không cần quản lý việc mở rộng máy chủ thủ công.

#### Tính sẵn sàng cao (High Availability)
- Các dịch vụ AWS (Lambda, API Gateway, DynamoDB, S3, CloudFront) đều có tính sẵn sàng cao được tích hợp sẵn.
- Dữ liệu được sao chép tự động trong nhiều AZ (Availability Zone).

#### Tính nhất quán dữ liệu (Data Consistency)
- DynamoDB hỗ trợ cả tính nhất quán mạnh (strong consistency) và cuối cùng (eventual consistency), phù hợp với các use-case khác nhau.
- Sử dụng EventBridge để đảm bảo các xử lý bất đồng bộ được thực hiện ít nhất một lần (at-least-once delivery).

#### Bảo mật (Security)
- Bảo mật tốt nhờ Cognito quản lý xác thực người dùng.
- Phân quyền chi tiết bằng IAM, áp dụng nguyên tắc đặc quyền tối thiểu.
- Giao tiếp giữa frontend và backend đều qua HTTPS/WSS.
- S3 bucket được bảo vệ bằng OAC, không cho phép truy cập công cộng.
- (Tùy chọn) Sử dụng AWS WAF để bảo vệ web khỏi các cuộc tấn công phổ biến.

#### Hiệu năng (Performance)
- Hệ thống phản hồi nhanh nhờ kiến trúc serverless, không cần khởi động máy chủ.
- Frontend được phân phối toàn cầu qua CloudFront CDN, giảm độ trễ cho người dùng ở các khu vực khác nhau.
- WebSocket API cho phép giao tiếp hai chiều thời gian thực với độ trễ thấp.

---

## 9. Kết quả kỳ vọng

### Cải tiến kỹ thuật
- **Kết quả 1:** Khả năng mở rộng linh hoạt nhờ các dịch vụ serverless Lambda và DynamoDB.
- **Kết quả 2:** Giảm thiểu công sức quản lý và vận hành hệ thống nhờ mô hình không máy chủ.
- **Kết quả 3:** Giao tiếp hai chiều thời gian thực hiệu quả cao bằng API Gateway WebSocket API.
- **Kết quả 4:** Tách biệt logic nghiệp vụ và xử lý bất đồng bộ thông qua kiến trúc hướng sự kiện với EventBridge.
- **Kết quả 5:** Bảo mật tốt hơn với Cognito quản lý user và phân quyền IAM role chi tiết.
- **Kết quả 6:** Tăng tốc độ phản hồi frontend toàn cầu nhờ CDN CloudFront.
- **Kết quả 7:** Tăng tính quan sát (observability) với metrics, logs, dashboard của CloudWatch.

### Giá trị lâu dài

#### Giá trị cho doanh nghiệp
- Cung cấp một nền tảng tương tác thời gian thực cho các hoạt động đào tạo, gắn kết nội bộ.
- Giảm thiểu chi phí vận hành hệ thống so với các giải pháp truyền thống.
- Dễ dàng tùy chỉnh và mở rộng theo nhu cầu thực tế.

#### Giá trị cho người dùng
- Trải nghiệm chơi quiz thời gian thực mượt mà, tương tác tốt.
- Dễ dàng tham gia mà không cần cài đặt ứng dụng phức tạp.
- Nhận được phản hồi và kết quả nhanh chóng.

#### Khả năng mở rộng trong tương lai
- Bổ sung trang quản trị (Admin Dashboard) để phân tích báo cáo và quản lý nâng cao.
- Hỗ trợ nhiều chế độ chơi phong phú hơn (chơi tự luyện, chơi thi đấu, chế độ thi cử).
- Phân tích sâu hiệu suất của người chơi và tỷ lệ trả lời đúng của từng câu hỏi.
- Chế độ chơi theo nhóm/đội (Team-based quiz).
- Tích hợp với các hệ thống quản lý học tập (LMS) hoặc nền tảng đào tạo doanh nghiệp.
- Hỗ trợ đa ngôn ngữ.
- Giao diện tối ưu hơn trên thiết bị di động.
- Triển khai môi trường production với tên miền riêng, AWS WAF và các chính sách bảo mật chặt chẽ.
- Tích hợp pipeline CI/CD hoàn chỉnh để tự động hóa kiểm thử và triển khai.
