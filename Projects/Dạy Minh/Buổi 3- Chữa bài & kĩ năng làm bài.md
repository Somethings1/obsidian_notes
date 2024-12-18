# Các bước làm bài

---

1. Đọc đề
    - Có loại đề nêu rõ bước làm, nhưng mà rất trâu bò, nếu follow theo các bước đấy thì chắc chắn sẽ bị TLE (time limit exceeded - quá thời gian)
    - Cũng có loại không nêu ra cách làm mà chỉ nêu ra yêu cầu
2. Liệt kê ra các bước **tuần tự** để làm được theo yêu cầu
    - Những bước này ban đầu sẽ lớn (mỗi bước sẽ bao gồm nhiều việc khác nhau)
    - Break down từng bước, tức là chia nhỏ các bước này ra sao cho nó trở nên đơn giản nhất có thể
    - Code theo từng bước
3. Code trâu bò
    - Code kiểu gì cũng lỗi
        - Lỗi cú pháp thì đơn giản, compiler bảo gì thì làm theo thôi
        - Lỗi logic, chạy được, nhưng mà sai kết quả ⇒ kiểm tra từng bước như phần 2 đã nêu ra
        - Lỗi runtime, chạy được, đúng kết quả, nhưng mà nộp lên thì chết
4. Tối ưu
    - Tối ưu ở mức độ dòng code
    - Tối ưu ở mức đoạn code (theo bước ở b2)
    - Tối ưu cả bài (thay đổi toàn bộ cấu trúc của bài - viết lại bước 2)
5. Kiểm thử
    - Viết thêm 1 chương trình để sinh ra test
    - Viết thêm 1 chương trình để gọi ra chương trình sinh test, chạy code ở bước 3 và code sau khi tối ưu và so sánh kết quả của 2 chương trình

# Bài 1: Matrix manip 1

---

## Các bước thực hiện

1. Đọc ma trận vào
2. Đảo ma trận
3. In ma trận ra

|   |   |   |   |   |
|---|---|---|---|---|
||0|1|2|3|
|0|1|2|_**3**_|4|
|1|5|_**6**_|_**7**_|8|
|2|_**0**_|_**0**_|_**0**_|_**0**_|

```C++
int a[n][m];
for (int i = 0; i < n; i++) {
	for (int j = 0; j < m; j++) {
    std::cin >> a[i][j];
  }
}
```

### Nhắc lại vòng for

Ta xem xét cái vòng for bên trong:

```C++
for (int j = 0; j < m; j++) {
  std::cout << "Hello world" << j << '\n';
}
```

Vòng for này có nghĩa là

1. Tạo 1 biến j = 0
2. Kiểm tra j < m, nếu đúng thì sang bước 3, sai thì bước 5
3. Thực hiện lệnh `std::cout << "Hello world" << j << '\n';`
4. j++, quay lại bước 2
5. Thoát vòng lặp

Vậy 2 vòng for lồng nhau sẽ chạy thế nào?

```C++
for (int i = 0; i < n; i++) {
// Đoạn bên trong
	for (int j = 0; j < m; j++) {
    std::cin >> a[i][j];
  }
// Hết đoạn bên trong
}
```

Nhìn từ ngoài vào trong

1. Tạo 1 biến i = 0
2. Kiểm tra i < n, nếu đúng thì bước 3, nếu sai thì bước 5
3. Thực hiện đoạn bên trong
4. i++, quay lại bước 2
5. Thoát vòng lặp

Đến vòng lặp bên trong, làm tương tự như bên trên

### Đảo ngược mảng 1 chiều

Mảng có 11 phần tử, chỉ số từ 0 đến 10.

|   |   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|---|
|0|1|2|3|4|5|6|7|8|9|10|
|6|5|3|5|1|3|2|7|6|5|4|

1. Đổi chỗ phần tử ở vị trí 0 với phần tử ở vị trí 10. 10 = 11 - 0 -1
2. Đổi chỗ phần tử ở vị trí 1 với phần tử ở vị trí 9 = 11 - 1 - 1
3. 2 ↔ 8 = 11 - 2 - 1
4. 3 ↔ 7 = 11 - 3 - 1
5. 4 ↔ 6 = 11 - 4 - 1

Với mảng có 11 phẩn tử: với i từ 0 đến 11 / 2, đổi chỗ phần tử thứ i với phần tử thứ 11 - i - 1

Với mảng có n phần tử: với i từ 0 đến n / 2, đổi chỗ phần tử thứ i với phần tử thứ n - i - 1

### Đổi chỗ

Đổi chỗ a với b:

- c = a
- a = b
- b = c

Cách vip pro không cần dùng biến c:

- a = a + b
- b = a - b
- a = a - b

Cách ultimate: hàm std::swap(a, b)