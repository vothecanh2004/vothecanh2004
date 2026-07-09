---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---



### Mục tiêu tuần 8:

* Phân tích và thiết kế cấu trúc REST API, sơ đồ quan hệ dữ liệu cho các thực thể Quiz, Question và Room của dự án Quizizz.
* Nắm vững cách xây dựng, thiết lập định tuyến và kết nối liên thông mô hình kiến trúc API Gateway → Lambda → RDS.
* Hiểu rõ cơ chế xử lý dữ liệu truyền thông đa phương tiện (Image/Audio) và tích hợp thành công Lambda với Amazon S3.
* Thực hành kiểm thử tự động/thủ công toàn bộ các đầu API Backend cơ bản thông qua công cụ Postman.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Phân tích kiến trúc Backend Quizizz. <br> - Xác định các module chính <br>- Tìm hiểu mô hình API Gateway → Lambda → RDS. <br>- Xây dựng danh sách REST API cần phát triển                                                                                             | 08/06/2026   | 08/06/2026      |
| 3   | - Thiết kế REST API <br>- Thiết kế các bảng Quiz, Question, Room.<br>- Xây dựng sơ đồ quan hệ dữ liệu                          | 09/06/2026   | 09/06/2026      | 
| 4   | - Tạo Lambda Function cho Quiz API.<br>- Tạo API Gateway Routes.<br>- Kết nối Lambda với RDS.<br>- Xây dựng các API: Create Quiz, Get Quiz List. | 10/06/2026   | 10/06/2026      | 
| 5   | - Xây dựng CRUD Question.<br>- Tìm hiểu quy trình upload Image/Audio lên Amazon S3.<br>- Tích hợp Lambda với S3.                  | 11/06/2026   | 11/06/2026      | 
| 6   | - **Thực hành:** <br>&emsp; + Kiểm thử các API bằng Postman. <br>&emsp; + Kiểm tra CRUD Quiz. <br>&emsp; + Kiểm tra CRUD Question.<br>&emsp; + Upload ảnh lên S3.  <br>&emsp; + Hoàn thiện các API Backend cơ bản.                                                                                       | 12/06/2026   | 12/06/2026      


### Kết quả đạt được tuần 8:

* Phân tích thành công kiến trúc mô hình API Gateway → Lambda → RDS và xác định rõ các module, danh sách REST API cần phát triển cho Backend Quizizz.
* Hoàn thiện thiết kế hệ thống REST API cùng sơ đồ quan hệ dữ liệu cho các thực thể Quiz, Question và Room.
* Xây dựng và kết nối thành công AWS Lambda với RDS, triển khai xong các API cốt lõi: Create Quiz và Get Quiz List qua API Gateway.
* Hoàn thành bộ tính năng CRUD Question và tích hợp thành công Lambda với Amazon S3 phục vụ luồng upload tệp tin hình ảnh/âm thanh.
* Kiểm thử (Test) toàn bộ hệ thống API Backend cơ bản bằng Postman, đảm bảo các luồng xử lý dữ liệu và upload file lên S3 hoạt động ổn định.