---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---



### Mục tiêu tuần 8:

* Tạo và cấu hình WebSocket API trên AWS API Gateway.
* Thiết lập các routes WebSocket.
* Attach Integrations giữa WebSocket routes và các Lambda functions tương ứng.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu WebSocket API trên API Gateway<br>- Cách Lambda xử lý WebSocket events                                                                                             | 08/06/2026   | 08/06/2026      | https://docs.aws.amazon.com/|
| 3   | - Tạo WebSocket API trên API Gateway<br>- Tạo các routes                      | 09/06/2026   | 09/06/2026      | https://docs.aws.amazon.com/ | 
| 4   | - Tạo Integrations cho các routes WebSocket  | 10/06/2026   | 10/06/2026      | https://docs.aws.amazon.com/ | 
| 5   | - Lấy WebSocket URL và Connection URL                  | 11/06/2026   | 11/06/2026      | https://docs.aws.amazon.com/ | 
| 6   | - **Thực hành:**  <br>&emsp; + Test gửi message qua WebSocket <br>&emsp; + Kiểm tra Lambda logs để xác nhận xử lý thành công                                                                                       | 12/06/2026   | 12/06/2026      


### Kết quả đạt được tuần 8:

* Biết kiến thức về WebSocket API trên AWS API Gateway và cách Lambda xử lý các sự kiện WebSocket.
* Tạo và cấu hình hoàn chỉnh các routes.
* Tạo stage dev cho WebSocket API và lấy được WebSocket URL cũng như Connection URL.
* Thực hành test thành công WebSocket API với wscat: kết nối, gửi message và xác nhận Lambda xử lý đúng qua CloudWatch Logs.