---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---



### Mục tiêu tuần 9:

* Test full WebSocket flow (start game, next question, submit answer, end game).
* Cấu hình API Gateway.
* Viết tài liệu API chi tiết.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Lấy WebSocket Connection URL từ API Gateway <br>                                 | 15/06/2026   | 15/06/2026      | https://docs.aws.amazon.com/ |
| 3   | - Test luồng WebSocket đầy đủ: tạo phòng, tham gia phòng <br>- Test START_GAME action <br>- Test NEXT_QUESTION action <br>- Test SUBMIT_ANSWER action <br>- Test END_GAME action                                  | 16/06/2026   | 16/06/2026      | https://docs.aws.amazon.com/ |
| 4   | - Cấu hình logging cho API Gateway stages <br>- Kiểm tra và optimize API Gateway performance | 17/06/2026   | 17/06/2026      | https://docs.aws.amazon.com/ |
| 5   | - Viết tài liệu API cho HTTP API  <br>- Viết tài liệu API cho WebSocket                 | 18/06/2026   | 18/06/2026      |  |
  | 6   | - **Thực hành:** <br>&emsp; + Test tích hợp full flow giữa HTTP API và WebSocket API  <br>&emsp; + Kiểm tra rằng WebSocket broadcast tin nhắn đến tất cả clients <br>&emsp;                                                                     | 19/06/2026   | 19/06/2026      

### Kết quả đạt được tuần 9:

* Cập nhật  WEBSOCKET_ENDPOINT environment variable .
* Test  full WebSocket flow: tạo phòng, tham gia phòng, START_GAME, NEXT_QUESTION, SUBMIT_ANSWER và END_GAME.
* Đảm bảo tích hợp hoàn chỉnh giữa HTTP API, WebSocket API, Lambda.