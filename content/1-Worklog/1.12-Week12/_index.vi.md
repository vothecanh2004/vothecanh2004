---
title: "Worklog Tuần 12"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.12 </b> "
---


### Mục tiêu tuần 12:

* Kiểm thử liên thông toàn diện kiến trúc End-to-End giữa Frontend và Backend, đảm bảo các luồng REST và WebSocket API chạy ổn định song song.
* Khảo sát, nghiệm thu toàn bộ kịch bản nghiệp vụ cốt lõi của ứng dụng (Xác thực, Tạo Quiz, Quản lý phòng thi và kết nối đa người chơi).
* Giám sát hệ thống qua CloudWatch và SQS để phát hiện, khắc phục triệt để các lỗi phát sinh (bugs).
* Tối ưu hóa hiệu năng truy vấn cơ sở dữ liệu (RDS), rà soát phân quyền an toàn (S3) và nghiệm thu tổng thể dự án trước khi bàn giao.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Kiểm tra kết nối giữa Frontend và Backend. <br> - Kiểm tra Amazon Cognito với API Gateway <br>- Kiểm tra Lambda kết nối Amazon RDS. <br>- Kiểm tra Lambda truy cập Amazon S3. <br>- Kiểm tra API Gateway REST và WebSocket hoạt động đồng thời.                                                                                         | 06/08/2026   | 06/08/2026      |
| 3   | - Kiểm thử luồng Đăng ký → Đăng nhập.<br>- Kiểm thử tạo Quiz.<br>- Kiểm thử tạo Room.<br>- Kiểm thử người chơi tham gia phòng                                    | 07/08/2026   | 07/08/2026      
| 4   | - Kiểm tra CloudWatch Metrics <br>- Kiểm tra CloudWatch Logs.<br>- Kiểm tra Amazon SQS xử lý hàng đợi. | 08/08/2026   | 08/08/2026      
| 5   | - Khắc phục các lỗi phát hiện trong quá trình kiểm thử.<br>- Tối ưu API và Database Query.<br>- Kiểm tra quyền truy cập S3.                 | 09/08/2026   | 09/08/2026      
| 6   | - Kiểm tra toàn bộ chức năng lần cuối.                                                                                 | 10/08/2026   | 10/08/2026      


### Kết quả đạt được tuần 12:

* Kiểm thử liên thông thành công toàn bộ kiến trúc End-to-End giữa Frontend và Backend, bao gồm việc tích hợp Cognito, API Gateway, Lambda, RDS và S3.
* Xác thực tính ổn định của hệ thống khi chạy song song cả hai luồng dịch vụ API Gateway REST và WebSocket API trong môi trường thực tế.
* Hoàn thiện kiểm thử nghiệm thu toàn bộ các luồng chức năng cốt lõi của ứng dụng: Đăng ký/Đăng nhập, Tạo bộ câu hỏi (Quiz), Tạo phòng và kết nối nhiều người chơi thời gian thực.
* Theo dõi và giám sát chặt chẽ hiệu năng, xử lý hàng đợi qua Amazon SQS, đồng thời phân tích sâu log lỗi bằng Amazon CloudWatch Metrics/Logs.
* Khắc phục triệt để các bug phát sinh, tối ưu hóa thành công các câu lệnh truy vấn Database (RDS) và phân quyền bảo mật S3, đảm bảo hệ thống sẵn sàng vận hành ổn định 100%.