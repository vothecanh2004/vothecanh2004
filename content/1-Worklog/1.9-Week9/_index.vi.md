---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---



### Mục tiêu tuần 9:

* Cập nhật WEBSOCKET_ENDPOINT environment variable cho các Lambda functions.
* Test full WebSocket flow (start game, next question, submit answer, end game).
* Cấu hình API Gateway stages và optimize.
* Viết tài liệu API chi tiết.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Lấy WebSocket Connection URL từ API Gateway <br> - Cập nhật WEBSOCKET_ENDPOINT cho Lambda ws-message <br>- Cập nhật WEBSOCKET_ENDPOINT cho Lambda score-calculator                                                                      | 15/06/2026   | 15/06/2026      | https://docs.aws.amazon.com/ |
| 3   | - Test luồng WebSocket đầy đủ: tạo phòng, tham gia phòng <br>- Test START_GAME action <br>- Test NEXT_QUESTION action <br>- Test SUBMIT_ANSWER action <br>- Test END_GAME action                                  | 16/06/2026   | 16/06/2026      | https://docs.aws.amazon.com/ |
| 4   | - Cấu hình logging cho API Gateway stages <br>- Cấu hình throttling nếu cần <br>- Kiểm tra và optimize API Gateway performance | 17/06/2026   | 17/06/2026      | https://docs.aws.amazon.com/ |
| 5   | - Viết tài liệu API cho HTTP API (endpoints, request/response, auth) <br>- Viết tài liệu API cho WebSocket (actions, events, message format)                | 18/06/2026   | 18/06/2026      |  |
  | 6   | - **Thực hành:** <br>&emsp; + Test tích hợp full flow giữa HTTP API và WebSocket API <br>&emsp; + Kiểm tra rằng score-calculator nhận được event từ EventBridge <br>&emsp; + Kiểm tra rằng WebSocket broadcast tin nhắn đến tất cả clients <br>&emsp; + Hoàn thiện tài liệu API                                                                                       | 19/06/2026   | 19/06/2026      

### Kết quả đạt được tuần 9:

* Cập nhật thành công WEBSOCKET_ENDPOINT environment variable cho các Lambda functions cần thiết (ws-message và score-calculator).
* Test thành công full WebSocket flow: tạo phòng, tham gia phòng, START_GAME, NEXT_QUESTION, SUBMIT_ANSWER và END_GAME.
* Cấu hình hoàn chỉnh logging và optimize performance cho API Gateway stages.
* Viết xong tài liệu API chi tiết cho cả HTTP API và WebSocket API, bao gồm endpoints, request/response format, authentication và các actions/events.
* Đảm bảo tích hợp hoàn chỉnh giữa HTTP API, WebSocket API, Lambda và EventBridge, toàn bộ luồng game hoạt động đúng.