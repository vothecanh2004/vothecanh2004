---
title: "Worklog Tuần 10"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---


### Mục tiêu tuần 10:

* Final testing cho toàn bộ APIs (HTTP và WebSocket).
* Tạo Postman Collection hoàn chỉnh với tất cả endpoints và WebSocket requests.
* Load testing API Gateway để đảm bảo hiệu năng.
* Fix bugs liên quan đến API và WebSocket.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Final test tất cả HTTP API endpoints (CRUD Quiz, CRUD Question, Room APIs) <br> - Test các trường hợp lỗi (400, 401, 403, 404, 500) <br>- Test rate limiting nếu có                                                                               | 22/06/2026   | 22/06/2026      | https://learning.postman.com/ |
| 3   | - Final test WebSocket API với tất cả actions (START_GAME, NEXT_QUESTION, SUBMIT_ANSWER, END_GAME) <br>- Test WebSocket events (GAME_STARTED, NEXT_QUESTION, ANSWER_RECEIVED, SCORE_UPDATE, GAME_ENDED)                                 | 23/06/2026   | 23/06/2026      |  |
| 4   | - Tạo Postman Collection hoàn chỉnh: <br>&emsp; + Folder cho Authentication <br>&emsp; + Folder cho Quiz APIs <br>&emsp; + Folder cho Room APIs <br>&emsp; + Folder cho WebSocket Testing <br>- Thêm examples cho request/response |24/06/2026  | 24/06/2026      | https://learning.postman.com/ |
| 5   | - Load testing API Gateway với nhiều request đồng thời <br>- Kiểm tra CloudWatch Metrics để xem latency và error rate         | 25/06/2026   | 25/06/2026      | https://docs.aws.amazon.com/|
| 6   | - **Thực hành:** <br>&emsp; + Fix tất cả bugs phát hiện trong quá trình testing <br>&emsp; + Optimize API performance nếu cần <br>&emsp; + Xuất Postman Collection và chia sẻ với nhóm <br>&emsp; + Viết hướng dẫn sử dụng Postman Collection                                                                                        | 26/06/2026   | 26/06/2026      


### Kết quả đạt được tuần 10:

* Final test thành công toàn bộ HTTP API và WebSocket API, bao gồm cả các trường hợp thành công và lỗi.
* Tạo hoàn chỉnh Postman Collection với cấu trúc khoa học, bao gồm Authentication, Quiz APIs, Room APIs và WebSocket Testing, kèm examples.
* Thực hiện load testing và đảm bảo API Gateway xử lý được nhiều request đồng thời, kiểm tra Metrics trên CloudWatch.
* Fix tất cả bugs liên quan đến API và WebSocket phát hiện trong quá trình testing.
* Xuất Postman Collection và viết hướng dẫn sử dụng, chia sẻ với nhóm để dễ dàng testing và phát triển.