---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---




### Mục tiêu tuần 2:

* Hiểu vai trò và tầm quan trọng của AWS IAM trong quản lý danh tính và phân quyền bảo mật tài nguyên.
* Nắm vững các khái niệm cốt lõi: IAM User, Group, Policy, Role và phân biệt rõ ràng giữa chúng.
* Nhận thức được rủi ro bảo mật của Root Account và các best practices bảo vệ tài khoản.
* Thực hành thành thạo tạo User/Group, gán Policy và kiểm tra quyền truy cập trên AWS Console.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu dịch vụ AWS Identity and Access Management (IAM). <br> - Hiểu vai trò của IAM trong việc quản lý danh tính và phân quyền trên AWS.                                                                                             | 27/04/2026   | 27/04/2026    |<https://docs.aws.amazon.com/>|
| 3   | - Tìm hiểu IAM Policy <br>- Thực hành một số IAM Policy mẫu. <br>                                            | 28/04/2026   | 28/04/2026      | https://docs.aws.amazon.com/ |
| 4   | - Tìm hiểu IAM Role. <br> - Hiểu sự khác nhau giữa IAM User và IAM Role <br>  | 29/04/2026   | 29/04/2026      | https://docs.aws.amazon.com/ |
| 5   | - Tìm hiểu Root Account và các rủi ro bảo mật khi sử dụng Root User.   <br>                  | 30/04/2026   | 30/04/2026      | https://docs.aws.amazon.com/ |
| 6   | - **Thực hành:** <br>&emsp; + Tạo IAM User <br>&emsp; + Tạo IAM Group. <br>&emsp; + Gán IAM Policy cho User/Group <br>&emsp; + Kiểm tra quyền truy cập của IAM User sau khi được phân quyền.                                                                                       | 01/05/2026   | 01/05/2026      |  |



### Kết quả đạt được tuần 2:

* Nắm vững kiến thức cốt lõi về dịch vụ AWS Identity and Access Management (IAM) và vai trò quan trọng của nó trong việc quản lý danh tính, phân quyền tài nguyên bảo mật trên AWS.
* Hiểu sâu về cấu trúc và cách hoạt động của IAM Policy và đã thử nghiệm thành công với một số mẫu policy thông dụng.
* Phân biệt rõ ràng thực thể IAM User và IAM Role; hiểu rõ các trường hợp sử dụng cụ thể khi nào cần cấp tài khoản cố định (User) và khi nào cần ủy quyền tạm thời (Role).
* Nhận thức được các rủi ro bảo mật nghiêm trọng khi sử dụng Root Account trong vận hành hàng ngày và nắm được các Best Practices của AWS để bảo vệ tài khoản.
* Hoàn thành bài thực hành thực tế trên AWS Console bao gồm:
  * Khởi tạo thành công IAM User và cấu hình nhóm quyền IAM Group.
  * Thực hiện gán các IAM Policy phù hợp cho User/Group theo nguyên tắc phân quyền tối thiểu .
  * Kiểm tra, xác thực và đánh giá chính xác quyền truy cập của IAM User sau khi được phân quyền để đảm bảo hệ thống hoạt động đúng thiết kế bảo mật.


