---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# AWS SYSTEMS MANAGER: "TRỢ LÝ VẠN NĂNG" GIÚP BẠN LÀM CHỦ HẠ TẦNG CLOUD VÀ HYBRID
AWS Systems Manager (SSM) là dịch vụ quản lý tập trung của AWS, cho phép quản trị và vận hành hạ tầng trên AWS, môi trường On-Premises và Multi-Cloud thông qua một giao diện thống nhất. Dịch vụ giúp đơn giản hóa việc quản lý tài nguyên, tự động hóa các tác vụ vận hành, rút ngắn thời gian phát hiện và xử lý sự cố, đồng thời nâng cao tính bảo mật và khả năng quản lý ở quy mô lớn
![Ảnh 1](/images/blog1.1.png)
Những điểm nổi bật của AWS Systems Manager
## 1. Quản lý tài nguyên tập trung (Resource Groups)
Systems Manager cho phép nhóm các tài nguyên AWS bằng Tags, chẳng hạn như EC2, S3, RDS, VPC và nhiều dịch vụ khác. Nhờ đó, người quản trị có thể theo dõi và thực hiện các tác vụ trên toàn bộ tài nguyên thuộc cùng một ứng dụng hoặc môi trường chỉ từ một giao diện duy nhất.
![Ảnh 2](/images/blog1.2.png)
## 2. Giám sát và tổng hợp thông tin vận hành (Insights)

Systems Manager tổng hợp dữ liệu từ nhiều dịch vụ như Amazon CloudWatch, AWS CloudTrail, AWS Config và AWS Trusted Advisor để hiển thị trạng thái vận hành, mức độ tuân thủ và tình trạng của hệ thống trên một Dashboard tập trung. Điều này giúp việc theo dõi và xử lý sự cố trở nên nhanh chóng hơn.
![Ảnh 3](/images/blog1.3.png)
![Ảnh 4](/images/blog1.4.png)
## 3. Tự động hóa tác vụ (Automation)

Với Automation, người dùng có thể xây dựng các quy trình tự động (Runbook) để thực hiện các công việc như khởi động hoặc dừng EC2, cập nhật cấu hình, sao lưu dữ liệu và nhiều tác vụ quản trị khác. Các quy trình này có thể được chạy theo lịch hoặc kích hoạt tự động khi có sự kiện xảy ra.
![Ảnh 5](/images/blog1.5.png)
## 4. Thực thi lệnh từ xa (Run Command)

Run Command cho phép chạy lệnh trên nhiều máy chủ cùng lúc mà không cần đăng nhập SSH hoặc Remote Desktop. Quản trị viên có thể quản lý hàng trăm hoặc hàng nghìn máy chủ từ xa, đồng thời kiểm soát quyền truy cập thông qua IAM để tăng cường bảo mật.

## 5. Quản lý bản vá và cấu hình

Systems Manager cung cấp các công cụ như:

* **Patch Manager**: Tự động kiểm tra và cài đặt các bản vá bảo mật cho máy chủ.
* **Maintenance Windows**: Lên lịch bảo trì hệ thống vào thời gian phù hợp.
* **State Manager**: Duy trì cấu hình mong muốn trên các máy chủ và đảm bảo tính nhất quán trong toàn bộ hạ tầng.

## Tổng Kết
AWS Systems Manager là một nền tảng quản lý hạ tầng toàn diện, giúp doanh nghiệp quản lý tập trung các máy chủ và tài nguyên trên nhiều môi trường khác nhau. Với các tính năng như Resource Groups, Automation, Run Command, Patch Manager, State Manager và Dashboard giám sát, Systems Manager giúp giảm công việc thủ công, tăng cường bảo mật, chuẩn hóa cấu hình và nâng cao hiệu quả vận hành hệ thống ở quy mô lớn

Link tài liệu: <https://aws.amazon.com/vi/blogs/aws/aws-systems-manager/>   