---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---



### Mục tiêu tuần 5:

* Nắm vững kiến thức nền tảng về Amazon VPC, cách thiết lập Public/Private Subnet và cơ chế định tuyến Internet qua Internet Gateway và NAT Gateway.
* Hiểu rõ vai trò của Amazon CloudWatch trong việc giám sát tài nguyên và AWS CloudTrail trong việc ghi vết lịch sử cuộc gọi API bảo mật.
* Hệ thống hóa và củng cố toàn bộ khối kiến thức cốt lõi đã học (IAM, EC2, S3, RDS, DynamoDB, VPC, CloudWatch) để chuẩn bị cho giai đoạn tiếp theo.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu dịch vụ Amazon Virtual Private Cloud (Amazon VPC). <br> - Hiểu cách AWS tổ chức hệ thống mạng và phân chia tài nguyên trong VPC.                                                                      | 18/05/2026   | 18/05/2026      |<https://docs.aws.amazon.com/vpc/>
| 3   | - Tìm hiểu Internet Gateway.<br> - Tìm hiểu NAT Gateway. <br>- Hiểu cách Public Subnet và Private Subnet kết nối Internet thông qua Internet Gateway và NAT Gateway                                           | 19/05/2026   | 19/05/2026      | <https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html> |
| 4   | - Tìm hiểu dịch vụ Amazon CloudWatch <br> - Hiểu cách CloudWatch giám sát tài nguyên AWS và gửi cảnh báo khi vượt ngưỡng cấu hình. | 20/05/2026   | 20/05/2026      | <https://docs.aws.amazon.com/cloudwatch/> |
| 5   | - Tìm hiểu dịch vụ AWS CloudTrail<br>-Hiểu cách CloudTrail ghi nhận các thao tác và API được thực hiện trên tài khoản AWS để phục vụ kiểm tra và bảo mật.                  | 21/05/2026   | 21/05/2026      | <https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-concepts.html> |
| 6   | - **Ôn tập toàn bộ kiến thức đã học:** <br>&emsp; + IAM. <br>&emsp; + Amazon EC2 <br>&emsp; + Amazon S3. <br>&emsp; + Amazon RDS. <br>&emsp; + Amazon DynamoDB. <br>&emsp; + Amazon VPC. <br>&emsp; + Amazon CloudWatch                                                                                       | 22/05/2026   | 22/05/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 5:

* Nắm vững kiến thức nền tảng về Amazon Virtual Private Cloud (Amazon VPC), hiểu cách AWS thiết lập một mạng đám mây ảo cô lập và cách phân chia dải địa chỉ IP (CIDR block) để quản lý tài nguyên hệ thống.
* Phân biệt rõ ràng vai trò của Public Subnet và Private Subnet; làm chủ cơ chế định tuyến dữ liệu ra Internet thông qua việc thiết lập Internet Gateway (cho Public Subnet) và NAT Gateway (giúp Private Subnet đi Internet an toàn một chiều).
* Hiểu sâu về bộ đôi dịch vụ giám sát và kiểm toán của AWS:
  * Amazon CloudWatch: Thu thập chỉ số (Metrics), theo dõi hiệu năng tài nguyên và thiết lập các ngưỡng cảnh báo tự động (Alarms).
  * AWS CloudTrail: Ghi lại lịch sử toàn bộ các hoạt động, sự kiện và lời gọi API trong tài khoản nhằm phục vụ công tác bảo mật, tuân thủ pháp lý.
* Củng cố và hệ thống hóa toàn bộ mạng lưới kiến thức cốt lõi đã học trong suốt hành trình, bao gồm các nhóm dịch vụ chủ chốt: Quản lý định danh (IAM), Điện toán (EC2), Lưu trữ (S3), Cơ sở dữ liệu (RDS, DynamoDB), Mạng (VPC) và Giám sát (CloudWatch, CloudTrail).
