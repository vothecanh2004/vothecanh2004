---
title: "Worklog Tuần 10"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---


### Mục tiêu tuần 10:

* Thiết lập và tổ chức cấu trúc Postman Collections khoa học cho toàn bộ hệ thống API dự án.
* Tiến hành kiểm thử End-to-End quy trình xác thực (Cognito, JWT), các tác vụ CRUD dữ liệu, upload tệp tin lên S3 và xử lý kịch bản lỗi.



### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tạo Collection cho Authentication API. <br> - Tạo Collection cho Quiz API. <br>- Tạo Collection cho Question API.                                                                               | 22/06/2026   | 22/06/2026      |
| 3   | - Tạo Collection cho Room API.   <br>- Tạo Collection cho Upload API.                                 | 23/06/2026   | 23/06/2026      
| 4   | - Kiểm thử Register và Login bằng Cognito. <br> - Kiểm thử JWT Authorization. <br>- Kiểm thử CRUD Quiz. <br>- Kiểm thử CRUD Question.<br>- Kiểm thử Upload ảnh lên Amazon S3. <br>- Kiểm thử các trường hợp lỗi |24/06/2026  | 24/06/2026      
| 5   | - Tìm hiểu cách Newman trả về kết quả Pass/Fail để hỗ trợ kiểm thử tự động.         | 25/06/2026   | 25/06/2026      
| 6   | - **Thực hành:** <br>&emsp; + Chạy toàn bộ Postman Collection. <br>&emsp; + Xuất báo cáo kết quả kiểm thử. <br>&emsp; + Sửa các API bị lỗi. <br>&emsp; + Kiểm tra lại toàn bộ Authentication, CRUD, Upload và Realtime API.                                                                                        | 26/06/2026   | 26/06/2026      


### Kết quả đạt được tuần 10:

* Khởi tạo và tổ chức thành công các Postman Collection chi tiết cho toàn bộ hệ thống API.
* Thực hiện kiểm thử toàn diện quy trình đăng ký/đăng nhập qua Cognito, xác thực JWT, các luồng CRUD dữ liệu và upload tệp tin lên Amazon S3; phát hiện và xử lý kịp thời các kịch bản lỗi .
* Chạy nghiệm thu toàn bộ Postman Collection, xuất báo cáo kết quả kiểm thử hoàn chỉnh và khắc phục triệt để các lỗi phát sinh.
* Đảm bảo hệ thống API chạy ổn định, đồng bộ từ khâu xác thực, xử lý dữ liệu cho đến các kết nối thời gian thực (Realtime API).