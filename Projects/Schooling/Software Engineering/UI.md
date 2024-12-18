Gồm có menu, 5 page:

- Overview: line chart
- Analysis
- Savings
- Accounts
- Settings

Ngày 1: smooth area chart

- Nghiên cứu được cách dùng library là oke

Ngày 2: tối ưu navigation bar

- Navigation bar phải có logo, các option, nút about bên dưới cùng
- Phải có responsive, khi window quá nhỏ thì tự thu thành các icon
- Trên header có nút mũi tên để toggle việc chuyển giữa icon và chữ
- Nhớ thêm class để sau thêm theme

Ngày 3: ring chart, progress bar, sửa lại accounts trong csdl

- Nghiên cứu cách dùng library
- Sửa lại account có thêm cột goal
- Nghĩ cách làm progress bar, truyền vào 3 tham số là current, max và expected
    - Current là giá trị hiện tại
    - Max là giá trị của cả tháng
    - Expected là giá trị kì vọng cho đến ngày hiện tại. Được biểu diễn bằng 1 đường thẳng đứng
    - Khi hover thì có tool tip thể hiện rõ các giá trị

Ngày 4: hoàn thiện overview

- Thay đổi cỡ chữ theo window resize
- Thêm các chart
- Đổi lại layout nếu được
- Thêm các class

Ngày 5-6: hoàn thiện analysis

- 1 nút bên trên cùng kiểu pill để chuyển giữa income và expense
- Đối với category
    - 1 bên là ring chart, 1 bên là danh sách các categories. Danh sách bao gồm icon, tên, tổng tương ứng và progress bar thể hiện budget. Nếu không có budget cho category đó thì hiện no budget set. Ưu tiên hiện các category có budget trước. Khi window quá nhỏ thì ring chart ở trên, danh sách ở dưới.
    - Khi bấm vào 1 category, hiện ra 1 window mới. Bên trên là line chart của cả năm, thống kê category đó theo tháng, có highlight điểm tương ứng với tháng, bên dưới là danh sách các category theo tháng. Khi chuyển tháng, highlight và danh sách sẽ thay đổi

Ngày 7: hoàn thiện accounts

- Mỗi account (không goal) thành một pane. Trong pane có tên account, area chart số dư trong tháng (có nút để chỉnh xem theo năm), 1 nút để edit tên và 1 nút hiện chi tiết. Khi bấm vào chi tiết thì hiện cửa sổ phân tích giống category, nhưng có thêm total money in, total money out và total difference.
- Có nút thêm account ở dưới cùng

Ngày 8: hoàn thiện savings

- Nếu không có khoản saving nào thì hiện No savings
- Luôn có nút để thêm 1 khoản saving.
- Đặt các khoản savings thành các ring chart, ở giữa là tên khoản. Ưu tiên hiện các khoản chưa xong trước
- Hiện 3 panes trên cùng: tổng các khoản, số khoản đã xong, số khoản chưa xong

Ngày 9 - 10: hoàn thiện settings page

- Dark mode, light mode
- Cho phép thêm sửa xoá category, accounts
- Chọn đơn vị tiền tệ
- Autocomplete (on/off)
- Chuyển dữ liệu
    - Chuyển dữ liệu settings (bao gồm category và account)
    - Chuyển dữ liệu transactions
    - Chuyển cả 2
    - Nghiên cứu cả kết nối bluetooth để đồng bộ hoá với app trên điện thoại

Ngày 11: thêm animation

ngày 12: thêm dark mode

  

  

  

# Lịch trình mới

xong dark mode

- [x] Dark mode
- [x] Kiểm tra trước khi thêm hoặc sửa transaction
- [ ] Bấm vào 1 category hoặc account thì hiện modal
- [x] Thêm nút sửa saving, kiểm tra hợp lệ. Kiểm tra hợp lệ cho sửa tên account và category
- [x] Thêm các form để thêm account và category, kiểm tra hợp lệ
- [x] Thêm các form xác nhận xoá account và category
- [x] Tính năng filter List Transaction
- [x] export transaction ra csv (cho phép chọn filter)
- [x] export database đến một nơi nào đó (cho phép chọn) và import database từ một nơi nào đó (cho phép chọn)
- [ ] thêm year view