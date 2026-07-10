---
title: "Worklog Tuần 11"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---



### Mục tiêu tuần 11:

* Cấu hình WAF (Web Application Firewall) cho API Gateway để tăng cường bảo mật.
* Cấu hình API Gateway logging và monitoring chi tiết.
* Viết tài liệu tích hợp (integration docs) cho toàn bộ hệ thống API.
* Tổng hợp và hoàn thiện tất cả tài liệu liên quan đến API và WebSocket.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu AWS WAF và cách tích hợp với API Gateway <br>- Tạo WAF Web ACL <br>- Thêm rules cơ bản (AWSManagedRulesCommonRuleSet)                                                                                  | 27/06/2026   | 27/06/2026      | <https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html> |
| 3   | - Cấu hình detailed logging cho API Gateway (log full request/response) <br>- Cấu hình X-Ray tracing cho API Gateway để trace request <br>- Kiểm tra logs trên CloudWatch                        | 28/06/2026   | 28/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-logging.html> |
| 4   | - Viết tài liệu tích hợp chi tiết: <br>&emsp; + Tổng quan kiến trúc API <br>&emsp; + Cách xác thực (Cognito + JWT) <br>&emsp; + Các HTTP endpoints <br>&emsp; + WebSocket protocol <br>&emsp; + Ví dụ code gọi API | 29/06/2026   | 29/06/2026     | <https://www.mkdocs.org/> |
| 5   | - Tổng hợp tất cả tài liệu: <br>&emsp; + API reference <br>&emsp; + Postman Collection docs <br>&emsp; + Hướng dẫn setup môi trường <br>&emsp; + Troubleshooting guide         | 30/06/2026   | 30/06/2026       |
| 6   | - **Thực hành:** <br>&emsp; + Demo hệ thống API hoàn chỉnh cho nhóm <br>&emsp; + Huấn luyện nhóm sử dụng API và Postman Collection <br>&emsp; + Đưa hệ thống API vào production-ready state                                                          | 31/06/2026   | 31/06/2026     


### Kết quả đạt được tuần 11:

* Tích hợp thành công AWS WAF với API Gateway để bảo vệ hệ thống khỏi các cuộc tấn công phổ biến.
* Cấu hình hoàn chỉnh logging và X-Ray tracing cho API Gateway, dễ dàng debug và monitor trên CloudWatch.
* Viết xong tài liệu tích hợp chi tiết, bao gồm kiến trúc, xác thực, HTTP endpoints, WebSocket protocol và ví dụ code.
* Tổng hợp hoàn chỉnh tất cả tài liệu liên quan đến API và WebSocket, sẵn sàng cho việc chuyển giao và bảo trì.
* Demo thành công hệ thống API hoàn chỉnh cho nhóm và đảm bảo hệ thống ở trạng thái production-ready.