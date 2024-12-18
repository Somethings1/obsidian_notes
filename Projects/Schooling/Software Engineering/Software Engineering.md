[[Họp 27-9]]

[[Thiết kế hệ thống app]]

[[UI]]

# Introduction

---

## Các khái niệm đầu tiên

- Client: người sở hữu phần mềm mà đội phát triển sẽ xây dựng
- Customer: người mua phần mềm hoặc lựa chọn phần mềm
- User: người dùng phần mềm

## Các rủi ro thường gặp

- Function: chương trình không chạy như mong đợi
- Cost: vượt quá chi phí
- Time: làm chậm

## Các bước trong quy trình phát triển phần mềm

- Đánh giá tính khả thi

> [!important]
> 
> - Phân tích yêu cầu
> - Thiết kế giao diện người dùng
> - Thiết kế hệ thống
> - Phát triển chương trình

- Kiểm thử và bàn giao
- Vận hành và bảo trì

  

# Chu trình phát triển phần mềm

---

## Các bước lặp lại

- Phân tích yêu cầu
- Thiết kế giao diện
- Thiết kế hệ thống
- Xây dựng chương trình
- Kiểm thử và bàn giao

## 4 loại chu trình

1. Waterfall: mỗi bước cần hoàn thành để có thể bắt đầu bước tiếp theo
2. Lặp tinh chỉnh: đi thật nhanh qua các bước khác nhau để hình thành các bản nháp khác nhau của hệ thống sau đó tinh chỉnh dần
3. Xoáy ốc: là mô hình lặp tinh chỉnh, nhưng mỗi thành phần mới hoặc được cập nhật sẽ được thêm vào hệ thống như một phần đã hoàn thiện
4. Phát triển nhanh: sự kết hợp của lặp và tăng dần

## Heavyweight and lightweight methodology

### Heavyweight

- Chu trình thực hiện chậm
- Bàn giao vào giai đoạn cuối
- Thoả thuận với khách hàng
- Luân tuân thủ theo kế hoạch

  

### Lightweight

- Chu trình thực hiện nhanh
- Bàn giao tăng dần
- Cộng tác với khách hàng
- Thích nghi với thay đổi, do đó kế hoạch có thể thay đổi

## Mô hình thác nước (waterfall)

![[image.png]]

> [!important] Thuộc kiểu mô hình **cồng kềnh**, đòi hỏi phải **tài liêu hoá** đầy đủ ở từng bước

### Ưu điểm

- Phân chia tác vụ rõ ràng
- Tính trực quan cao
- Thực hiện đảm bảo chất lượng ở từng bước
- Tính toán chi phí ở từng bước

### Nhược điểm

Trong thực tế, ở từng bước của chu trình thường sẽ xuất hiện những phát hiện hoặc hiểu biết mới về bước trước đó, nên các bước trước lại cần xem xét lại

  

**⇒ Không khả thi**

## Modified waterfall model

![[image 1.png]]

> [!important] Hoạt động tốt với các yêu cầu phần mềm được hiểu một cách đầy đủ và giải pháp thiết kế là mình bạch rõ ràng (straightforward)

## Mô hình lặp tinh chỉnh

> [!important] Tạo một hệ thống nguyên mẫu ngay từ đầu quá trình phát triển. Xem xét nguyên mẫu với khách hàng và thử nghiệm nó với người dùng để cải thiện sự hiểu biết về các yêu cầu và làm rõ các thiết kế. Sau đó tinh chỉnh nguyên mẫu qua một loạt các thao tác lặp lại

Mô hình này thường được coi là trung gian giữa lightweight và heavyweight

## Mô hình xoáy chân ốc

> [!important] Mô hình lặp lại và gia tăng với sự nhấn mạnh hơn vào phân tích rủi ro
> 
> Gồm 4 pha chính
> 
> - Planning
> - Design
> - Construct
> - Evaluation
> 
> 1 lần lặp = 1 lần xoáy chân ốc

![[image 2.png]]

### Góc phần tư thứ nhất

Các yêu cầu được thu thập và phân tích

### Góc phần tư thứ 2

Tất cả các giải pháp được đề xuất cho các vấn đề sẽ được đánh giá. Xác định rủi ro cho giải pháp đã chọn, sau đó giải quyết bằng chiến lược tốt nhất có thể.

⇒ Nguyên mẫu được xây dựng để có giải pháp tốt nhất có thể

### Góc phần tư thứ 3

Các tính năng đã xác định được phát triển và xác minh thông qua phiên bản thử nghiệm

⇒ Phiên bản tiếp theo của hệ thống có thể sẵn sàng

### Góc phần tư thứ 4

Khách hàng đánh giá phiên bản đã phát triển cho đến nay

⇒ Lại bắt đầu kế hoạch cho giai đoạn tiếp theo

## Agile Development

Agile là một phương pháp phát triển phần mềm linh hoạt, tập trung vào việc đáp ứng nhanh chóng với sự thay đổi và cải thiện liên tục.

### Các nguyên tắc cơ bản của Agile

- Ưu tiên sự hài lòng của khách hàng thông qua việc giao sản phẩm sớm và liên tục
- Chấp nhận thay đổi yêu cầu, ngay cả ở giai đoạn cuối của quá trình phát triển
- Giao sản phẩm hoạt động thường xuyên, từ vài tuần đến vài tháng
- Hợp tác chặt chẽ giữa nhóm phát triển và khách hàng
- Xây dựng dự án xung quanh những cá nhân có động lực
- Giao tiếp trực tiếp là phương thức hiệu quả nhất

## Scrum process

### 1. **Roles (Vai trò trong Scrum)**:

- **Product Owner**: Người chịu trách nhiệm về **Product Backlog** và đảm bảo rằng đội phát triển đang làm việc theo đúng hướng để mang lại giá trị tối đa. Product Owner đại diện cho khách hàng và các bên liên quan.
- **Scrum Master**: Là người hướng dẫn đội phát triển tuân theo các nguyên tắc và thực hành Scrum, giúp loại bỏ các rào cản và đảm bảo quy trình làm việc hiệu quả.
- **Development Team**: Nhóm phát triển tự tổ chức và đa chức năng, bao gồm các thành viên có kỹ năng cần thiết để hoàn thành sản phẩm. Nhóm chịu trách nhiệm chuyển giao sản phẩm hoàn thiện trong mỗi **Sprint**.

### 2. **Product Backlog**:

- **Product Backlog** là danh sách các tính năng, cải tiến và công việc cần hoàn thành để phát triển sản phẩm. Đây là một danh sách liên tục thay đổi, do Product Owner quản lý và sắp xếp theo mức độ ưu tiên dựa trên giá trị kinh doanh.

### 3. **Sprint Backlog**:

- **Sprint Backlog** là tập hợp các mục từ **Product Backlog** mà nhóm phát triển cam kết hoàn thành trong một Sprint (chu kỳ phát triển thường kéo dài 1-4 tuần). Nó bao gồm các nhiệm vụ cần thực hiện để hoàn thành các mục tiêu của Sprint.

### 4. **Epic - User Story - Task**:

- **Epic**: Là một yêu cầu lớn, phức tạp, thường phải chia nhỏ thành nhiều **User Stories** để dễ quản lý và thực hiện.
- **User Story**: Là một yêu cầu hoặc tính năng cụ thể, thường được viết từ góc nhìn của người dùng cuối. Cấu trúc phổ biến của User Story: "Là một [loại người dùng], tôi muốn [tính năng] để [lợi ích]."
- **Task**: Là những công việc cụ thể mà đội phát triển phải hoàn thành để thực hiện một User Story. Các Task thường nhỏ hơn và chi tiết hơn User Stories.

### 5. **Meetings in Scrum**:

- **Sprint Planning**: Cuộc họp diễn ra vào đầu mỗi Sprint để lập kế hoạch những công việc cần thực hiện. Nhóm phát triển và Product Owner quyết định mục tiêu của Sprint và chọn các mục từ Product Backlog để đưa vào Sprint Backlog.
- **Daily Scrum (Stand-up)**: Cuộc họp ngắn hàng ngày (thường khoảng 15 phút) để đội phát triển cập nhật tình hình công việc, chia sẻ tiến độ và thảo luận về các vấn đề đang gặp phải. Mục tiêu là đảm bảo mọi người đều hiểu rõ những gì đã làm, đang làm và những rào cản cần giải quyết.
- **Sprint Review**: Diễn ra vào cuối Sprint để đội phát triển trình bày sản phẩm đã hoàn thiện và nhận phản hồi từ Product Owner và các bên liên quan. Đây cũng là cơ hội để điều chỉnh Product Backlog.
- **Sprint Retrospective**: Là cuộc họp cuối Sprint, tập trung vào việc đánh giá quy trình làm việc của nhóm và tìm cách cải tiến. Nhóm sẽ thảo luận về những gì đã làm tốt, những gì cần cải thiện, và những hành động cần thực hiện để tối ưu hoá quy trình trong Sprint tiếp theo.

# Phân tích tính khả thi (Feasibility)

---