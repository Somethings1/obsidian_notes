# Chữa bài buổi trước

---

## Salary

### Ý tưởng

- Dùng một `std::unordered_map<std::string, int>` để lưu dữ liệu đầu vào
- Với mỗi truy vấn thì chỉ cần gọi phần tử tương ứng trong map ra
- ⇒ Mỗi dòng input có độ phức tạp ~ O(1), mỗi truy vấn cũng có đpt ~ O(1)
- ⇒ O(max{n, q})

### Code

```C++
\#include <iostream>
\#include <unordered_map>

int main() {
    int n;
    std::cin >> n;
    
    std::unordered_map<std::string, int> map;
    while(n--) {
        std::string name;
        int salary;
        
        std::cin >> name >> salary;
        map[name] = salary;
    }
    
    int q;
    std::cin >> q;
    
    while(q--) {
        std::string name;
        std::cin >> name;
        
        std::cout << map[name] << '\n';
    } 

    return 0;
}
```

## Treasure

### Tóm tắt

Cho mảng n số nguyên và một số k. Đếm xem có bao nhiêu số xuất hiện đúng k lần trong mảng

### Ý tưởng

- Dùng một `std::unordered_map<int, int>`, với ý nghĩa là map[i] = số lần mà số i xuất hiện trong mảng
- Có thể coi map này giống như một mảng tần suất
- VD: `1 1 2 2 3 4 4 4` sẽ có mảng tần suất tương ứng là `2 2 1 3`
- Sử dụng tính chất của map: nếu chưa có phần tử với `key` nào đó trong map, mà mình lại gọi `map[key]` thì nó sẽ tự tạo ra một giá trị mặc định (với số nguyên thì giá trị mặc định = 0)

### Code

```C++
\#include <iostream>
\#include <unordered_map>

int main() {
    int n, k;
    std::cin >> n;
    
    std::unordered_map<int, int> map;
    while(n--) {
        int tmp;
        std::cin >> tmp;
        
        map[tmp]++;
    }
    std::cin >> k;
    
    int count = 0;
    
    for (auto tmp: map) {
        count += tmp.second == k;
    }
    
    std::cout << count;
}
```

## Good array

### Tóm tắt

Cho 1 mảng có n số nguyên. Mảng được gọi là tốt nếu tần suất của mọi con số bằng đúng giá trị của nó. Tìm số phần tử nhỏ nhất phải loại bỏ khỏi mảng để mảng trở nên tốt.

### Ý tưởng

- Liên quan đến tần suất ⇒ nghĩ đến sử dụng `map`, cụ thể là `unordered_map`
- Với từng giá trị
    - Nếu tần suất của nó nhỏ hơn giá trị ⇒ cần loại bỏ hết vì không có cách nào để làm cho tần suất tăng lên được
    - Nếu tần suất bằng giá trị ⇒ giữ nguyên
    - Nếu tần suất lớn hơn ⇒ loại bỏ vừa đủ để sao cho tần suất = giá trị

> [!important] Từ khoá `auto` để khai báo biến có cùng kiểu với vế phải
> 
> ```C++
> std::map<std::string, int> map;
> 
> auto it = map.begin() 
> // std::map<std::string, int>::iterator it = map.begin();
> ```

# Một vài thuật toán cơ bản

## 1. Thuật toán tổng tiền tố 1 chiều (prefix sum)

- Tổng tiền tố của mảng `a` có `n` phần tử là một mảng `s` có `n + 1` phần tử. Để cho tiện tính toán thì các bài sử dụng tổng tiền tố sẽ coi như mảng bắt đầu từ 1
- `s[i]` = tổng `a[1] + a[2] + a[3] + ... + a[i]` = `s[i - 1] + a[i]`, `s[0] = 0`
- Dùng để tính tổng đoạn trong `O(1)` đối với mảng cố định
- Công thức để tính tổng `a[l] + a[l + 1] + ... + a[r - 1] + a[r]` = `s[r] - s[l - 1]`

  

`a[l] + a[l + 1] + ... + a[r - 1] + a[r]`

|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|||||+|+|+|+|+|||||||

`s[r]`

|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|+|+|+|+|+|+|+|+|+|||||||

`s[l - 1]`

|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|+|+|+|+||||||||||||

## 2. Thuật toán tổng tiền tố 2 chiều

- Tổng tiền tố 2 chiều để phục vụ việc tính tổng ma trận con của một ma trận 2 chiều
- Công thức xây dựng tổng:
    - `s[i][j] = a[i][j] + s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1]`
    - `s[i][j]` là tổng của ma trận con từ góc trên bên trái đến hàng i cột j
- Công thức tính tổng từ hàng a cột b đến hàng c cột d
    - `s[c][d] - s[a - 1][d] - s[c][b - 1] + s[a - 1][b - 1]`

## 3. Chặt nhị phân

- Là thuật toán để tìm kiếm một giá trị trong một dãy đã được sắp xếp
- Là thuật toán để tìm kiếm nghiệm của một hàm số có tính đơn điệu
    - Hàm số có tính đơn điệu khi với x1 ≤ x2 thì f(x1) ≤ f(x2) hoặc x1 ≤ x2 thì f(x1) ≥ f(x2)
- Mô tả thuật toán:
    - Xét phần tử ở giữa dãy
        - Nếu phần tử đó lớn hơn phần tử cần tìm, không cần quan tâm đến nửa bên phải nữa, coi như dãy chỉ có nửa trái
        - Nếu phần tử đó nhỏ hơn phần tử cần tìm, không cần quan tâm đến nửa bên trái nữa, coi như dãy chỉ có nửa phải
    - Thực hiện chia dãy ra làm đôi như vậy cho đến khi tìm được phần tử cần tìm hoặc không thể chia nhỏ ra được nữa (không tồn tại phần tử đó trong dãy)
- Độ phức tạp: `O(logn)` - với mảng có 1 triệu phần tử

### Hàm std::upper_bound và std::lower_bound

- Nằm trong thư viện `algorithm`
- Thực hiện tìm kiếm nhị phân trong mảng với giá trị đã cho
- `std::upper_bound`: trả về con trỏ trỏ đến phần tử có giá trị nhỏ nhất mà lớn hơn giá trị cần tìm
- `std::lower_bound`: trả về con trỏ trỏ đến phần tử có giá trị nhỏ nhất mà lớn hơn hoặc bằng giá trị cần tìm (nếu tìm thấy nhiều phần tử thoả mãn thì sẽ `a[l] + a[l + 1] + ... + a[r - 1] + a[r]`lấy phần tử đầu tiên)
- Cách sử dụng:
    - `std::upper_bound(first, last, value)`
    - `first` là con trỏ trỏ đến phần tử đầu tiên của dãy cần tìm
    - `last` là con trỏ trỏ đến phần tử sau phần tử cuối cùng của dãy cần tìm
    - `value` là giá trị cần tìm
- VD:
    - Tìm giá trị phần tử nhỏ nhất lớn hơn d trong vector a
        - `*std::upper_bound(a.begin(), a.end(), d)`
    - Tìm vị trí của phẩn tử nhỏ nhất lớn hơn d trong vector a
        - `std::distance(a.begin(), std::upper_bound(a.begin(), a.end(), d))`
            - Hàm `distance(first, last)` nhận vào 2 con trỏ và trả về khoảng cách giữa chúng
    - Tìm vị trí phần tử nhỏ nhất lớn hơn d trong mảng a có n phần tử
        - `std::upper_bound(a, a + n, d) - a`

# Bài tập

---

Làm cho đến bài Matrix Sum