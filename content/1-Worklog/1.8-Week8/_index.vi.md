---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---



### Mục tiêu tuần 8:

* Tạo và cấu hình WebSocket API trên AWS API Gateway.
* Thiết lập các routes WebSocket ($connect, $disconnect, $default).
* Attach Integrations giữa WebSocket routes và các Lambda functions tương ứng.
* Thực hành test WebSocket API với công cụ wscat.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu WebSocket API trên API Gateway <br> - Hiểu về route selection expression <br>- Cách Lambda xử lý WebSocket events                                                                                             | 08/06/2026   | 08/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-websocket-api.html> |
| 3   | - Tạo WebSocket API trên API Gateway <br>- Cấu hình route selection expression: $request.body.action<br>- Tạo các routes: $connect, $disconnect, $default                          | 09/06/2026   | 09/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-websocket-api-create.html> | 
| 4   | - Tạo Integrations cho các routes WebSocket <br>- Attach $connect route với Lambda ws-connect<br>- Attach $disconnect route với Lambda ws-disconnect<br>- Attach $default route với Lambda ws-message | 10/06/2026   | 10/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-websocket-api-integrations.html> | 
| 5   | - Tạo stage dev cho WebSocket API <br>- Enable auto-deploy<br>- Lấy WebSocket URL và Connection URL                  | 11/06/2026   | 11/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-websocket-api-stages.html> | 
| 6   | - **Thực hành:** <br>&emsp; + Cài đặt wscat <br>&emsp; + Kết nối WebSocket với wscat <br>&emsp; + Test gửi message qua WebSocket <br>&emsp; + Kiểm tra Lambda logs để xác nhận xử lý thành công                                                                                       | 12/06/2026   | 12/06/2026      


### Kết quả đạt được tuần 8:

* Nắm vững kiến thức về WebSocket API trên AWS API Gateway và cách Lambda xử lý các sự kiện WebSocket.
* Tạo thành công WebSocket API với route selection expression `$request.body.action`.
* Tạo và cấu hình hoàn chỉnh các routes WebSocket: $connect, $disconnect và $default.
* Attach thành công Integrations giữa các WebSocket routes và các Lambda functions tương ứng (ws-connect, ws-disconnect, ws-message).
* Tạo stage dev cho WebSocket API và lấy được WebSocket URL cũng như Connection URL.
* Thực hành test thành công WebSocket API với wscat: kết nối, gửi message và xác nhận Lambda xử lý đúng qua CloudWatch Logs.