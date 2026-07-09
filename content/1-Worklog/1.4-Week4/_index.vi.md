---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---





### Mục tiêu tuần 4:

* Hiểu rõ cơ chế lưu trữ đối tượng của Amazon S3 và cách tối ưu hóa chi phí thông qua các nhóm S3 Storage Classes.
* Nắm vững kiến thức nền tảng về cơ sở dữ liệu quan hệ Amazon RDS (SQL) và cơ sở dữ liệu phi quan hệ Amazon DynamoDB (NoSQL).
* Thực hành thành thạo trên AWS Console: Tạo S3 Bucket, cấu hình phân quyền Object và khởi tạo, kết nối thành công một cơ sở dữ liệu MySQL Instance trên Amazon RDS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu dịch vụ Amazon Simple Storage Service (Amazon S3).                                                                                              | 11/05/2026   | 11/05/2026      |<https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html>
| 3   | - Tìm hiểu các Storage Class của Amazon S3 và trường hợp sử dụng của từng loại.                                            | 12/05/2026    | 12/05/2026       | <https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html> |
| 4   | - Tìm hiểu dịch vụ Amazon Relational Database Service (Amazon RDS). <br> - Tìm hiểu các hệ quản trị cơ sở dữ liệu được hỗ trợ như MySQL... | 13/05/2026    | 13/05/2026       | <https://docs.aws.amazon.com/rds/> |
| 5   | - Tìm hiểu dịch vụ Amazon DynamoDB  <br> - Hiểu cơ sở dữ liệu NoSQL. <br> - So sánh sự khác nhau giữa Amazon RDS và Amazon DynamoDB                 | 14/05/2026   | 14/05/2026       | <https://docs.aws.amazon.com/dynamodb/> |
| 6   | - **Thực hành:** <br>&emsp; + Tạo Bucket trên Amazon S3. <br>&emsp; + Thiết lập quyền Public và Private cho Object. <br>&emsp; + Khởi tạo cơ sở dữ liệu MySQL trên Amazon RDS. <br>&emsp; + Kết nối và kiểm tra trạng thái hoạt động của RDS Instance.                                                                                      | 15/05/2026    | 15/05/2026       | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 4:

* Nắm vững kiến thức cốt lõi về Amazon Simple Storage Service (Amazon S3), cơ chế lưu trữ dạng đối tượng và các khái niệm liên quan như Bucket, Object, Key.
* Phân biệt rõ ràng các lớp lưu trữ của S3 và biết cách lựa chọn lớp lưu trữ tối ưu chi phí dựa trên tần suất truy cập dữ liệu.
* Hiểu sâu về dịch vụ cơ sở dữ liệu quan hệ Amazon RDS, các công cụ quản trị được hỗ trợ như MySQL, PostgreSQL, SQL Server... và cách AWS tự động hóa việc quản lý phần cứng, sao lưu.
* Làm quen với Amazon DynamoDB và các đặc trưng cốt lõi của cơ sở dữ liệu NoSQL , đồng thời phân biệt rõ các tiêu chí lựa chọn giữa Amazon RDS (SQL) và Amazon DynamoDB (NoSQL).
* Hoàn thành các bài thực hành thực tế trực tiếp trên AWS Console:
  * Khởi tạo thành công S3 Bucket, thực hiện cấu hình phân quyền bảo mật (Public/Private Access) linh hoạt cho các Object lưu trữ bên trong.
  * Khởi tạo hoàn chỉnh một cơ sở dữ liệu MySQL Instance trên dịch vụ Amazon RDS, thực hiện kết nối thành công và kiểm tra chính xác trạng thái hoạt động của RDS Instance.

