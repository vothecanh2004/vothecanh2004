---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---


# TÌM HIỂU AMAZON ELASTIC CONTAINER REGISTRY (AMAZON ECR): QUẢN LÝ VÀ TRIỂN KHAI CONTAINER HIỆU QUẢ

## Giới thiệu

Khi Docker ngày càng được sử dụng rộng rãi trong phát triển ứng dụng, việc lưu trữ và quản lý Docker Image trở thành một thách thức đối với nhiều doanh nghiệp. Tự xây dựng một Docker Registry riêng đòi hỏi phải quản lý hạ tầng, đảm bảo khả năng mở rộng và thiết lập cơ chế bảo mật phù hợp. Amazon Elastic Container Registry (Amazon ECR) được AWS phát triển nhằm giải quyết bài toán này bằng cách cung cấp một dịch vụ lưu trữ Container Image được quản lý hoàn toàn (Fully Managed), giúp đơn giản hóa việc lưu trữ, quản lý và triển khai ứng dụng container trên AWS.

## Những thách thức
Trước khi sử dụng Amazon ECR, các doanh nghiệp thường gặp các vấn đề như:

* Phải tự triển khai và vận hành Docker Registry.
* Khó mở rộng khi số lượng Container Image tăng nhanh.
* Quản lý quyền truy cập phức tạp.
* Khó đồng bộ Image giữa nhiều môi trường hoặc nhiều AWS Region.
* Cần đảm bảo tính sẵn sàng và bảo mật của Image

## Giải pháp
Amazon ECR cung cấp một Container Registry được AWS quản lý hoàn toàn, cho phép:

* Lưu trữ Docker Container Image an toàn.
* Tích hợp trực tiếp với Docker CLI.
* Quản lý quyền truy cập thông qua IAM.
* Tích hợp với Amazon ECS để triển khai container nhanh chóng.
* Tự động mở rộng mà không cần quản lý máy chủ Registry.

## Các tính năng nổi bật

#### 1. Quản lý Container Repository
Amazon ECR cho phép tạo các Repository để lưu trữ Docker Image. Người dùng chỉ cần tạo Repository, sau đó sử dụng Docker CLI để Build, Tag và Push Image lên ECR.
![Ảnh 1](/images/blog3.1.png)
#### 2. Push và quản lý Docker Image
Sau khi tạo Repository, AWS cung cấp sẵn các lệnh Docker để:

* Đăng nhập vào ECR.
* Tag Docker Image.
* Push Image lên Repository.

Toàn bộ Image sau đó được quản lý tập trung trên giao diện ECR Console.
![Ảnh 2](/images/blog3.2.png)
![Ảnh 3](/images/blog3.3.png)

#### 3. Quản lý quyền truy cập (IAM Permissions)
Amazon ECR sử dụng AWS Identity and Access Management (IAM) để kiểm soát quyền truy cập.

Người quản trị có thể:

* Cho phép hoặc từ chối truy cập.
* Phân quyền theo User, Role hoặc AWS Account.
* Kiểm soát quyền Pull, Push hoặc quản trị Repository.
![Ảnh 4](/images/blog3.4.png)
![Ảnh 5](/images/blog3.5.png)

#### 4. Tích hợp với Amazon ECS
Amazon ECR tích hợp trực tiếp với Amazon ECS.

Quy trình triển khai gồm:

* Push Docker Image lên ECR.
* Tạo Task Definition.
* Chỉ định Image từ ECR.
* ECS tự động Pull Image và chạy Container trên Cluster.
![Ảnh 6](/images/blog3.6.png)
![Ảnh 7](/images/blog3.7.png)

## Lợi ích
Amazon ECR mang lại nhiều lợi ích cho quá trình phát triển và triển khai ứng dụng:

* Không cần tự quản lý Docker Registry.
* Khả năng mở rộng và tính sẵn sàng cao.
* Mã hóa Image khi lưu trữ và truyền tải qua HTTPS.
* Phân quyền linh hoạt bằng IAM.
* Tích hợp với Docker CLI và Amazon ECS.
* Giúp đơn giản hóa quy trình CI/CD và triển khai Container.

## Tổng kết
Amazon Elastic Container Registry là dịch vụ lưu trữ Docker Image được quản lý hoàn toàn bởi AWS, giúp doanh nghiệp đơn giản hóa việc quản lý Container Image, tăng cường bảo mật và tối ưu quy trình triển khai ứng dụng. Với khả năng tích hợp chặt chẽ cùng Docker CLI, IAM và Amazon ECS, ECR là thành phần quan trọng trong việc xây dựng các hệ thống ứng dụng hiện đại dựa trên Container.

Link bài viết: <https://aws.amazon.com/vi/blogs/aws/ec2-container-registry-now-generally-available/?utm_source>
