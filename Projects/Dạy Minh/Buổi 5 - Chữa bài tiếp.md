# Matrix traverse 1

---

## Nhận xét

- Ở mỗi đường chéo, hiệu (số hàng - số cột) là bằng nhau
- Ở đường chéo tiếp theo, hiệu (số hàng - số cột) sẽ bằng hiệu đó của đường chéo trước đó + 1
- ⇒ 2 tính chất này không liên quan đến bài này

## Ý tưởng

- Lặp để đi qua các điểm xuất phát của mỗi đường chéo
    - Ma trận `n * n` có `2 * n - 1` đường chéo
    - Xuất phát từ ô `[1, n]`, giảm dần số cột đến khi không thể giảm được nữa thì tăng số hàng
- Từ mỗi điểm xuất phát, tăng cả số hàng và số cột lên cho đến khi hết mảng
    - Hết mảng tức là hàng hoặc cột `> n`

## Triển khai

```C++
\#include <iostream>

int main() {
    int n;
    std::cin >> n;
    
    int a[n][n];
    for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
        std::cin >> a[i][j];
        
    int startRow = 0, startCol = n - 1;
    
    for (int x = 0; x < 2 * n - 1; x++) {
        int i = startRow, j = startCol;
        
        while (i < n && j < n) {
            std::cout << a[i][j] << ' ';
            i++;
            j++;
        }
        
        if (startCol == 0) startRow++;
        else startCol--;
    }
}
```

# Matrix traverse 2

---

## Ý tưởng

- Tương tự như bài trên, chỉ khác ở 2 điểm:
    - Điểm xuất phát bắt đầu từ `[1, 1]`, tăng dần số hàng, đến khi hết cỡ thì tăng số cột
    - Từ mỗi điểm xuất phát, giảm số hàng và tăng số cột đến khi hết mảng

## Triển khai

```C++
\#include <iostream>

int main() {
    int n;
    std::cin >> n;
    
    int a[n][n];
    for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
        std::cin >> a[i][j];
        
    int startRow = 0, startCol = 0;
    
    for (int x = 0; x < 2 * n - 1; x++) {
        int i = startRow, j = startCol;
        
        while (i >= 0 && j < n) {
            std::cout << a[i][j] << ' ';
            i--;
            j++;
        }
        
        if (startRow == n - 1) startCol++;
        else startRow++;
    }
}
```

# Matrix traverse ultimate

---

## Ý tưởng

- Duy trì 4 biến:
    - `startRow`: giới hạn trên của phép duyệt
    - `endRow`: giới hạn dưới của phép duyệt
    - `startCol`: giới hạn trái của phép duyệt
    - `endCol`: giới hạn phải của phép duyệt
- Ở mỗi vòng:
    - In ra hàng trên cùng theo giới hạn, sau đó tăng `startRow` lên (vì vừa duyệt hàng này rồi)
    - In ra cột bên phải theo giới hạn, sau đó giảm `endCol`
    - In ra hàng dưới cùng theo giới hạn, theo hướng ngược lại, sau đó giảm `endRow`
    - In ra cột bên trái theo giới hạn, theo hướng ngược lại, sau đó tăng `startCol`
- Có tổng cộng `(n + 1) / 2` vòng

```C++
\#include <iostream>

int main() {
    int n;
    std::cin >> n;
    
    int a[n][n];
    for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
        std::cin >> a[i][j];
        
    int startRow = 0, startCol = 0, endRow = n - 1, endCol = n - 1;
    
    // Moi buoc lap se in ra 1 vong
    for (int x = 0; x < (n + 1) / 2; x++) {
        
        // In hang tren cung
        for (int j = startCol; j <= endCol; j++) {
            std::cout << a[startRow][j] << ' ';
        }
        startRow++;
        
        // In ra cot ben phai
        for (int i = startRow; i <= endRow; i++) {
            std::cout << a[i][endCol] << ' ';
        }
        endCol--;
        
        // In ra hang duoi cung
        for (int j = endCol; j >= startCol; j--) {
            std::cout << a[endRow][j] << ' ';
        }
        endRow--;
        
        // In ra cot ben trai
        for (int i = endRow; i >= startRow; i--) {
            std::cout << a[i][startCol] << ' ';
        }
        startCol++;
    }
}
```

# **Vectorrrrrrrrr**

---

## Giới thiệu về vector

Vector là một cấu trúc dữ liệu trong STL (standard template library). Vector tương đối giống mảng. Nó có khả năng chèn thêm phần tử ở cuối, tức là kích thước không cố định như mảng.

### Khai báo

- `std::vector<int> ten_vector` - khai báo một vector có kiểu là `int` (có thể thay bằng kiểu khác - tương tự như mảng) và kích thước = 0 (vector rỗng)
    - Nếu khai báo như này sau đó gọi `ten_vector[0]` là sẽ lỗi bộ nhớ vì vector chưa có phần tử nào
- `std::vector<int> ten_vector(so_phan_tu)` - khai báo một vector kiểu `int` có kích thước = `so_phan_tu`
    - VD: khai báo vector kiểu `int` có `n` phần tử: `std::vector<int> a(n)`

### Truy cập phần tử của vector

- Tương tự như mảng, chỉ số bắt đầu từ 0.

### Một số hàm trong cấu trúc vector

- `size()` - trả về số phần tử hiện tại của vector
    - Kiểu trả về là `size_t` chứ không phải `int` nên đôi khi sẽ gây warning
    - Ép kiểu thành `(int)vector.size()`
- `push_back(n)` - thêm 1 phần tử vào cuối vector, tăng kích thước vector lên 1 đơn vị
- `begin()` - trả về con trỏ trỏ tới phần tử đầu tiên của vector
- `end()` - trả về con trỏ trỏ tới sau phần tử cuối của vector
    - `std::cout << *a.begin()` sẽ in ra phần tử đầu tiên của vector
    - `std::cout << *a.end()` không in ra phần tử cuối của vector
- `pop_back()` - loại bỏ phần tử ở cuối, giảm kích thước đi 1 đơn vị
- `resize(n)` - thay đổi kích thước vector thành n
    - Nếu kích thước hiện tại đang lớn hơn n - sẽ cắt bớt đoạn cuối đi cho đủ n
    - Nếu kích thước hiện tại nhỏ hơn n - thêm các phần tử rỗng vào cuối
- `clear()` - xoá vector - vector thì vẫn còn, nhưng kích thước = 0

### Cách sử dụng các hàm chức năng bên trên

`ten_vector.ham_can_dung()`

VD:

```C++
std::vector<int> a(n); // khai báo vector a có n phần tử
a.push_back(1); // thêm số 1 vào cuối a
int m = a.size(); // gán m bằng kích thước của a, m = n + 1
a.resize(5); // Cắt, giữ lại 5 phần tử đầu tiên của a
a.clear(); // xoá hết a, a.size() = 0
```

### Tại sao không dùng vector thay mảng hết

- Chỉ nên dùng vector cho những trường hợp cần linh hoạt về kích thước
- Dùng vector sẽ chậm hơn mảng trong trường hợp push_back và pop_back nhiều
- Bản chất của push_back:
    - Vector sẽ lưu 1 mảng có kích thước như ban đầu đã khai báo
    - Khi push_back mà vượt quá kích thước hiện tại, vector sẽ khai báo 1 mảng mới có kích thước gấp đôi rồi chuyển hết mảng cũ sang mảng mới

### Duyệt vector

3 cách duyệt vector a để tính tổng

- `for (int i = 0; i < (int)a.size(); i++) sum += a[i]`
- `for (auto it = a.begin(); it != a.end(); it++) sum += *it`
    - it là con trỏ
    - từ khoá `auto` có thể dùng khi phép khai báo có giá trị ở vế phải luôn ⇒ lấy kiểu của vế phải để đặt cho biến
- `for (int tmp: a) sum += tmp`
    - Ý nghĩa: với mọi biến trong a, lần lượt, tạo ra biến tmp có giá trị bằng biến đó và đi vào từng vòng lặp
    - Tương đương: `for (int i = 0; i < a.size(); i++) {int tmp = a[i]; sum += tmp}`

## Xử lý bài tập

```C++
\#include <iostream>
\#include <vector>

int main() {
    int n, m;
    std::cin >> n >> m;
    
    std::vector<int> a(n);
    for (int i = 0; i < n; i++)
        std::cin >> a[i];
        
    while (m--) {
        char c;
        std::cin >> c;
        
        if (c == 'A') {
            int tmp;
            std::cin >> tmp;
            
            a.push_back(tmp);
        }
        else if (c == 'P') {
            for (int i = 0; i < (int)a.size(); i++) 
                std::cout << a[i] << ' ';
            std::cout << '\n';
        }
        else if (c == 'R') {
            for (int i = (int)a.size() - 1; ~i; i--) 
                std::cout << a[i] << ' ';
            std::cout << '\n';
        }
        else {
            // c == 'S'
            int sum = 0;
            for (int tmp: a) sum += tmp;
            std::cout << sum << '\n';
        }
    }
}
```

# Bài tập

---

Làm lại những bài anh đã chữa