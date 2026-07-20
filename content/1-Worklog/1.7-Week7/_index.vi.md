---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---



### Mục tiêu tuần 7:

* Tạo và cấu hình HTTP API trên AWS API Gateway.
* Tạo Authorizer (Cognito JWT) cho API để bảo vệ các endpoints.
* Tạo Integrations và Routes cho các chức năng Quiz và Room.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tạo HTTP API trên API Gateway <br> - Cấu hình API name và IP address type <br>enabled                                                                                        | 01/06/2026   | 01/06/2026      | https://docs.aws.amazon.com/ |
| 3   | - Cấu hình CORS cho HTTP API <br>                              | 02/06/2026   | 02/06/2026      | https://docs.aws.amazon.com/ | 
| 4   | - Tạo Authorizer loại JWT <br>- Liên kết Authorizer với Cognito User Pool <br>- Cấu hình audience (App Client ID) | 03/06/2026   | 03/06/2026      | https://docs.aws.amazon.com/ | 
| 5   | - Tạo Integrations cho các Lambda functions (quiz-crud, room-management) <br>- Tạo các routes                | 04/06/2026   | 04/06/2026      | https://docs.aws.amazon.com/ | 
| 6   | - **Thực hành:** <br>&emsp;  <br>&emsp; + Attach Authorizer vào các routes cần bảo vệ <br>&emsp; + Test HTTP API với Postman <br>&emsp; + Kiểm tra các endpoints hoạt động đúng                                                                                          | 05/06/2026   | 05/06/2026      


### Kết quả đạt được tuần 7:

* Tạo HTTP API trên AWS API Gateway 
* Tạo và cấu hình thành công Authorizer loại JWT
* Tạo Integrations giữa API Gateway và các Lambda functions
* Tạo tất cả các routes cần thiết, attach authorizer cho các routes cần xác thực.
* Test thành công các API endpoints bằng Postman, đảm bảo luồng xác thực JWT hoạt động đúng.
