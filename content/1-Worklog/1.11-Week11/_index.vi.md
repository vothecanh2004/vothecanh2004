---
title: "Worklog Tuần 11"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---



### Mục tiêu tuần 11:

* Hiểu sâu về vòng đời và cơ chế tái sử dụng môi trường thực thi (Execution Environment) của AWS Lambda để giảm thiểu Cold Start.
* Thành thạo cách sử dụng Amazon CloudWatch Metrics và Logs để giám sát chỉ số hiệu năng và truy vết lỗi hệ thống.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu cách Lambda tái sử dụng Execution Environment.                                                                                  | 27/06/2026   | 27/06/2026      |
| 3   | - Tối ưu Static Initialization. <br>- Chọn Runtime phù hợp                        | 28/06/2026   | 28/06/2026     
| 4   | - Tìm hiểu Provisioned Concurrency <br>- Cấu hình Provisioned Concurrency cho Lambda | 29/06/2026   | 29/06/2026    
| 5   | - Tìm hiểu CloudWatch Metrics.<br>- Phân tích Logs trên CloudWatch.         | 30/06/2026   | 30/06/2026      
| 6   | - **Thực hành:** <br>&emsp; + Đo thời gian phản hồi trước khi tối ưu. <br>&emsp; + So sánh kết quả trước và sau tối ưu.                                                          | 31/06/2026   | 31/06/2026     


### Kết quả đạt được tuần 11:

* Hiểu rõ cơ chế hoạt động và vòng đời của AWS Lambda.
* Làm chủ công cụ giám sát Amazon CloudWatch, biết cách phân tích chuyên sâu các chỉ số (Metrics) và truy vết hệ thống thông qua Logs để chủ động phát hiện lỗi.
* Hoàn thành thực hành đo lường, phân tích và so sánh hiệu năng hệ thống; chứng minh được thời gian phản hồi của API được cải thiện rõ rệt sau khi tối ưu.