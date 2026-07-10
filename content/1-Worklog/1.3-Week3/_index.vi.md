---
title: "Worklog Tuần 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---





### Mục tiêu tuần 3:

* Hiểu khái niệm dịch vụ Amazon EC2 và cách phân loại các nhóm EC2 Instance Types (General Purpose, Compute Optimized, Memory Optimized, v.v.).
* Nắm vững nguyên lý hoạt động của Security Group (tường lửa ảo), Amazon Machine Image (AMI) và Amazon EBS (ổ đĩa lưu trữ block).
* Thực hành thành thạo khởi tạo, quản lý và theo dõi các trạng thái vòng đời của EC2 Instance trên AWS Console.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu dịch vụ Amazon EC2. <br> - Tìm hiểu EC2 Instance. <br> - Phân biệt các loại Instance Type                                                                                          | 04/05/2026   | 04/05/2026      | <https://aws.amazon.com/vi/documentation-overview/ec2/>
| 3   | - Tìm hiểu quy trình khởi tạo EC2 Instance. <br> - Thực hành các bước Launch EC2 Instance. <br>                                            | 05/05/2026   | 05/05/2026      | <https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-launch-parameters.html> |
| 4   | - Tìm hiểu Security Group. <br> - Hiểu Security Group hoạt động như tường lửa của EC2 Instance. | 06/05/2026   | 06/05/2026      | <https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html> |
| 5   | - Tìm hiểu Amazon Machine Image (AMI). <br> - Tìm hiểu Amazon Elastic Block Store                  | 07/05/2026   | 07/05/2026      | <https://aws.amazon.com/vi/documentation-overview/ec2/> |
| 6   | - **Thực hành:** <br>&emsp; Thực hành EC2   và quan sát sự thay đổi trạng thái của EC2 trong AWS Management Console.                                                             | 08/05/2026   | 08/05/2026      | <https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-launch-parameters.html> |


### Kết quả đạt được tuần 3:

* Nắm vững khái niệm về dịch vụ Amazon EC2 và phân biệt rõ ràng giữa dịch vụ quản lý tổng quát (EC2) với một máy chủ ảo cụ thể (EC2 Instance).
* Phân loại và hiểu rõ các nhóm EC2 Instance Types (General Purpose, Compute Optimized, Memory Optimized...) để lựa chọn cấu hình phần cứng tối ưu theo nhu cầu sử dụng.
* Hiểu sâu về cơ chế hoạt động của Security Group như một tường lửa ảo (Stateful Firewall) ở cấp độ Instance, biết cách cấu hình Inbound/Outbound rules để kiểm soát lưu lượng mạng an toàn.
* Nắm vững vai trò của Amazon Machine Image (AMI) trong việc cung cấp hệ điều hành/môi trường ban đầu, và Amazon Elastic Block Store (EBS) đóng vai trò là ổ đĩa lưu trữ block-storage bền vững đi kèm với EC2.
* Hoàn thành bài thực hành thực tế: Khởi chạy thành công một EC2 Instance, theo dõi sát sao và hiểu rõ ý nghĩa của các trạng thái vòng đời Instance trực tiếp trên AWS Console.

