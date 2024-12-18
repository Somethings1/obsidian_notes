# Nhắc lại về vòng lặp for

---

## Cú pháp

```Java
for (<câu lệnh khởi tạo>; <điều kiện lặp>; <lệnh cuối>) {

…

}
```

1. Thực hiện `<câu lệnh khởi tạo>`
2. Kiểm tra `<điều kiện lặp>`
    1. Nếu đúng
        1. Thực hiện lệnh bên trong cặp dấu ngoặc `{}`
        2. Thực hiện `<lệnh cuối>`
        3. Quay trở lại đầu bước 2
    2. Nếu sai thì out luôn

## Vòng for lồng nhau

```C++
for (int i = 0; i < 4; i++) {
// begin
	for (int j = 0; j < 4; j++) {
		std::cout << i << ' ' j << '\n';
	}
// end
}
```

Output

```C++
0 0
0 1
0 2
0 3
1 0
1 1
1 2
1 3
2 0
2 1
2 2
2 3
3 0
3 1
3 2
3 3
```

- Xét vòng lặp bên ngoài, trong khi thực hiện lệnh trong cặp dấu `{}` thì giá trị của `i` không thay đổi
- Lệnh trong cặp dấu `{}` là vòng lặp `for` với biến `j` chạy từ 0 đến 3

⇒ Với `i` giữ nguyên, `j` đi từ 0 đến 3

⇒ Với mỗi giá trị của `i`, `j` sẽ đi từ 0 đến 3

Mà `i` cũng đi từ 0 đến 3, nên output sẽ như trên.

### Nhận xét

Output của chương trình trên là các chỉ số của một mảng 2 chiều có 4 dòng và 4 cột.

|   |   |   |   |   |
|---|---|---|---|---|
||0|1|2|3|
|0|a[0][0]|a[0][1]|a[0][2]|a[0][3]|
|1|a[1][0]|a[1][1]|a[1][2]|a[1][3]|
|2|a[2][0]|a[2][1]|a[2][2]|a[2][3]|
|3|a[3][0]|a[3][1]|a[3][2]|a[3][3]|

Output kia chính là thứ tự duyệt mảng theo từ trái qua phải, từ trên xuống dưới.

⇒ code nhập mảng 2 chiều có n dòng và m cột

```C++
int a[n][m];
for (int i = 0; i < n; i++) {
	for (int j = 0; j < n; j++) {
		std::cin >> a[i][j];
	}
}
```

  

# Matrix manip 1

---

## Ý tưởng

- Nhập mảng
- In mảng ra - in ngược lại

## Xử lý

```C++
\#include <bits/stdc++.h>

int main () {
    int n, m;
    std::cin >> n >> m;
    
    // Input mảng
    int a[n][m];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            std::cin >> a[i][j];
        }
    }

    // In mảng ra
    for (int i = 0; i < n; i++) {
        for (int j = m - 1; j >= 0; j--) {
            std::cout << a[i][j] << ' ';
        }
        std::cout << '\n';
    }
}
```

# Matrix manip 2

---

## Ý tưởng

- Nhập mảng
- In ra với thứ tự hàng ngược lại

## Xử lý

```C++
\#include <bits/stdc++.h>

int main () {
    int n, m;
    std::cin >> n >> m;
    
    int a[n][m];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            std::cin >> a[i][j];
        }
    }
    
    for (int i = n - 1; i >= 0; i--) {
        for (int j = 0; j < m; j++) {
            std::cout << a[i][j] << ' ';
        }
        std::cout << '\n';
    }
}
```

# Thư viện algorithm

---

Sử dụng bằng cách thêm `#include<algorithm>` (nếu không dùng `bits/stdc++.h`)

- `bits/stdc++.h` là một header chứa 1 đống thư viện
- 1 thư viện là một phần code đã được viết sẵn theo một chủ đề nào đó
- `#include` là để khai báo thư viện, tức là thêm toàn bộ phần code viết sẵn đấy vào trong code của mình.
- ⇒ thêm 1 đống thư viện vào sẽ không có lợi về bộ nhớ (tốn bộ nhớ)

  

### [Link web tra cứu](https://cplusplus.com/reference/algorithm/)

### Một vài hàm cơ bản nhất

- min(a, b): trả về số nhỏ hơn trong 2 số
- max(a, b): trả về số lớn hơn trong 2 số
- swap(a, b): đổi chỗ 2 biến a, b

### Hàm sort()

- Cú pháp:
    - `sort(Iterator a, Iterator b)`
    - `sort(Iterator a, Iterator b, Compare compare)`
- `Iterator` là 1 con trỏ
- Hàm sort sẽ sắp xếp một cấu trúc nào đó từ con trỏ `a` đến trước con trỏ `b`. Sắp xếp tăng dần ở cú pháp đầu tiên, và sắp xếp theo `compare` ở cú pháp thứ 2

```C++
int n;
std::cin >> n;

int a[n];
// Đối với một mảng, thì tên mảng sẽ coi như là một con trỏ, 
// trỏ vào phần tử đầu tiên của mảng
// Con trỏ trỏ vào phần tử thứ i sẽ là tên mảng + i
// Con trỏ trỏ vào phần tử thứ i sẽ là a + i
// Muốn sắp xếp mảng a tăng dần => truyền a và a + n vào hàm sort
sort(a, a + n);

// Sắp xếp nửa đầu của mảng (nửa cuối giữ nguyên)
sort(a, a + n / 2);

// sắp xếp nửa cuối (nửa đầu giữ nguyên)
sort(a + n / 2, a + n);
```

Tóm lại, muốn sắp xếp mảng `a` từ vị trí `i` đến vị trí `j` theo thứ tự tăng dần thì gọi

`sort(a + i, a + j + 1)`

> [!important] Nếu muốn sắp xếp giảm dần thì sao?
> 
> - Cách 1: đảo dấu của tất cả các phần tử: `a[i] = -a[i]` xong sort như thường
> - Cách 2: gọi `sort(a, a + n, greater<int>())`
> - Cách 3:
> 
> ```C++
> \#include <bits/stdc++.h>
> 
> bool compare (int a, int b) {
>     return a > b;
> }
> 
> int main () {
>     int a[] = {4, 3, 1, 5, 3, 6, 7, 4};
>     std::sort(a, a + 8, compare);
>     for (int i = 0; i < 8; i++) std::cout << a[i] << ' ';
> }
> ```

Hàm `compare`:

- Giúp chúng ta sắp xếp theo thứ tự mà mình muốn
- Kiểu là `bool`
- Nhận vào 2 tham số có kiểu trùng với kiểu của mảng mà mình muốn sắp xếp
- Trả về `true` nếu muốn a đứng trước b
    - Ví dụ trên là sắp xếp giảm dần, a > b = true thì a đứng trước b

### Hàm reverse()

- Cú pháp: `reverse(Iterator a, Iterator b)`
    - Trong đó a, b, là 2 con trỏ
- Hàm sẽ đảo ngược đoạn [a, b)
- Ví dụ muốn đảo ngược mảng a có n phần tử: `reverse(a, a + n)`

# Bài tập

- Cố gắng làm 3 bài cuối và 2 bài đầu mà anh đã chữa
- Thử làm lại 2 bài đầu bằng cách thật sự đảo ngược (chứ không phải là chỉ đơn giản là in ra ngược nữa)
- Đọc trước các bài tập còn lại trong hackerrank