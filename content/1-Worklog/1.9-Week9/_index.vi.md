---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---



### Mục tiêu tuần 9:

* Phân biệt cơ chế hoạt động giữa HTTP REST API và WebSocket API; nắm vững cách API Gateway và Lambda quản lý trạng thái kết nối qua DynamoDB.
* Hiểu rõ nguyên lý hoạt động của hàng đợi Amazon SQS trong việc giảm tải hệ thống và xử lý bất đồng bộ luồng nộp đáp án.
* Thực hành khởi tạo WebSocket API, tích hợp SQS/RDS và kiểm thử hoàn chỉnh tính năng phòng thi đa người chơi theo thời gian thực.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu về WebSocket <br> - So sánh HTTP REST API và WebSocket API          <bt>- Tìm hiểu cách Lambda xử lý WebSocket và lưu ConnectionId vào DynamoDB                                                                      | 15/06/2026   | 15/06/2026      |
| 3   | - Phân tích luồng tạo phòng <br>- Luồng tham gia phòng <br>- Luồng bắt đầu trò chơi <br>- Luồng chuyển câu hỏi <br>- Luồng đếm ngược thời gian <br>- Luồng cập nhật bảng xếp hạng <br>- Thiết kế dữ liệu lưu trong DynamoDB                                  | 16/06/2026   | 16/06/2026      
| 4   | - Tìm hiểu SQS cơ bản <br>- Khi nào nên sử dụng SQS trong hệ thống Quizizz | 17/06/2026   | 17/06/2026      
| 5   | - Tạo WebSocket API.<br>- Xây dựng Lambda xử lý Connect/Disconnect. <br>- Lưu ConnectionId vào DynamoDB.<br>- Tạo Queue xử lý Submit Answer.<br>- Lưu kết quả bài làm vào Amazon RDS sau khi xử lý.                | 18/06/2026   | 18/06/2026      
  | 6   | - **Thực hành:** <br>&emsp; + Tạo phòng thi và cho nhiều người tham gia. <br>&emsp; + Kiểm thử gửi câu hỏi realtime qua WebSocket. <br>&emsp; + Người chơi gửi đáp án qua WebSocket. <br>&emsp; + Cập nhật điểm theo thời gian thực cho tất cả người chơi.                                                                                       | 19/06/2026   | 19/06/2026      

### Kết quả đạt được tuần 9:

* Phân biệt rõ ràng cơ chế hoạt động giữa HTTP REST API và WebSocket API, nắm vững cách AWS Lambda quản lý trạng thái kết nối bằng `ConnectionId` trong Amazon DynamoDB.
* Thiết lập cấu trúc dữ liệu tối ưu trên DynamoDB và làm chủ các luồng xử lý realtime phức tạp: Tạo/tham gia phòng, bắt đầu, đếm ngược thời gian, chuyển câu hỏi và cập nhật bảng xếp hạng.
* Hiểu sâu về hàng đợi Amazon SQS và ứng dụng thành công để giảm tải cho hệ thống, xử lý bất đồng bộ luồng nộp đáp án (Submit Answer) trước khi lưu vào RDS.
* Khởi tạo và cấu hình hoàn chỉnh API Gateway WebSocket API kết hợp các Lambda function xử lý sự kiện `$connect` và `$disconnect`.
* Thực hành và kiểm thử thành công tính năng phòng thi trực tuyến đa người chơi: Gửi câu hỏi, nhận đáp án và tính toán, cập nhật điểm số theo thời gian thực (realtime) cho toàn bộ người chơi.