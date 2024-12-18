> [!important] Là các thư viện mặc định đi kèm với compiler

# Cấu trúc dữ liệu

---

## 1. Array-like structures

Là những cấu trúc dữ liệu có cách tổ chức giống mảng (tức là cũng là 1 dãy biến liên tiếp nhau). Chỉ khác nhau về cách tương tác với các phần tử

### Vector

Vector chính xác là giống như mảng, nhưng có khả năng thêm và xoá phần tử ở cuối (thay đổi kích thước)

- Khai báo: `vector<kiểu> tên(số phần tử)`
    - VD: vector<int> a(5): vector kiểu int có 5 phần tử
    - Số phần tử có thể để trống - khai báo vector không có phần tử nào: `vector<int> a`
- Truy cập phần tử thứ i: `tên_vector[i]` (giống như mảng)
- Các hàm chức năng đặc biệt:
    - `push_back(a)`: thêm a vào cuối vector
    - `pop_back()`: xoá phần tử cuối cùng của vector

### Queue

Queue là hàng đợi. Nguyên tắc của queue là FIFO (First In First Out - vào trước thì ra trước). Tưởng tượng giống như một hàng người đang đợi mua đồ - ai đến trước sẽ được mua trước.

Vì nó tuân thủ nguyên tắc FIFO nên ta không thể truy cập được phần tử thứ i của queue, mà chỉ có thể truy cập phần tử đầu tiên của nó.

- Khai báo: `queue<kiểu> tên`
    - VD: `queue<int> q` khai báo một hàng đợi số nguyên có tên là `q`
- Các hàm chức năng đặc biệt:
    - `front()`: lấy phần tử đầu tiên của queue
    - `pop()`: xoá phần tử đầu tiên của queue
    - `push(a)`: thêm `a` vào cuối queue
    - `empty()`: trả về true nếu queue không còn phần tử nào

```C++
\#include <iostream>
\#include <queue>

int main() {
    std::queue<int> q;
    
    for (int i = 0; i <= 10; i++)
        q.push(i); // Thêm i vào cuối hàng đợi
        
    // Lặp đến khi vào kích thước còn khác 0 (không rỗng)
    while (!q.empty()) {
        std::cout << q.front() << ' '; // Truy cập phần tử đầu tiên của hàng đợi
        q.pop(); // Xoá phần tử đầu tiên của hàng đợi
    }

    return 0;
}
// Output: 0 1 2 3 4 5 6 7 8 9 10
```

Chủ yếu sử dụng trong thuật toán BFS

### Stack

Stack là ngăn xếp. Nguyên tắc của stack là LIFO (Last In First Out - vào sau ra trước). Tưởng tượng nó giống như một chồng đĩa: cái đĩa cuối cùng được đặt vào chồng đĩa sẽ là cái đĩa đầu tiên được lấy ra.

Tương tự như queue, stack không thể được truy cập phần tử thứ i, mà chỉ có thể truy cập phần tử trên cùng của nó.

- Khai báo: `stack<kiểu> tên`
    - VD: `stack<int> a` - khai báo ngăn xếp số nguyên có tên là a
- Các hàm chức năng của stack
    - `top()`: lấy phần tử trên cùng của stack
    - `pop()`: xoá phần tử trên cùng của stack
    - `push(a)`: thêm `a` lên trên cùng của stack
    - `empty()`: trả về `true` nếu stack không còn phần tử nào

```C++
\#include <iostream>
\#include <stack>

int main() {
    std::stack<int> q;
    
    for (int i = 0; i <= 10; i++)
        q.push(i);
        
    while (q.size() != 0) {
        std::cout << q.top() << ' '; 
        q.pop(); 
    }

    return 0;
}
```

Chủ yếu được dùng trong

- Khử đệ quy: đang có hàm đệ quy, nhưng không muốn đệ quy nữa
- Bài toán kiểm tra dãy ngoặc đúng

### Dequeue

Giống cái queue, nhưng có 2 đầu.

- Khai báo: `dequeue<kiểu> tên`
    - VD: `dequeue<int> d`
- Các hàm chức năng đặc biệt
    - `push_front(a)`: thêm a vào đầu hàng
    - `pop_front()`: xoá phần tử ở đầu hàng
    - `push_back(a)`: thêm a vào cuối hàng
    - `pop_back()`: xoá phần tử cuối hàng
    - `front()`: lấy phần tử đầu hàng
    - `back()`: lấy phần tử cuối hàng

Chủ yếu được dùng trong kỹ thuật sliding window (cửa sổ trượt)

- Tìm đoạn con có tổng cho trước
- Chuỗi con dài nhất có các chữ cái khác nhau

## 2. Tree-like structures

### Set

Set là cấu trúc để lưu các phần tử unique - tức là nếu thêm 2 giá trị giống nhau vào trong set thì nó chỉ nhận 1. Các phần tử được sắp xếp tăng dần.

- Khai báo: `set<kiểu> tên`
    - VD: `set<int> s` - khai báo 1 set kiểu int có tên là s
- Các hàm chức năng
    - `insert(a)` - thêm phần tử `a` vào trong set -
        - ĐPT: `O(logn)` - `n` là số phần tử của set
    - `erase(a)`
        - Nếu `a` là một giá trị thì set sẽ xoá `a`
            - Tương đương `erase(find(a))`
        - Nếu `a` là một con trỏ thì set sẽ xoá phần tử mà `a` đang trỏ đến
        - ĐPT: `O(logn)`
    - `find(a)` - trả về con trỏ trỏ đến vị trí của phần tử có giá trị bằng `a` trong set
        - ĐPT: `O(logn)`
    - `clear()` - xoá tất cả các phần tử trong set

```C++
\#include <iostream>
\#include <set>

int main() {
    std::set<int> s;
    s.insert(1);
    s.insert(1);
    s.insert(2);
    s.insert(3);
    s.insert(4);
    s.insert(5);
    
    std::cout << s.size() << '\n'; // In ra 5 dù được insert 6 lần
    
    s.erase(1);
    for (int i: s) std::cout << i << ' ';
    std::cout << '\n';
    // Output: 2 3 4 5
    
    auto it = s.find(2);
    s.erase(it);
    for (int i: s) std::cout << i << ' '; // O(nlogn)
    std::cout << '\n';
    // Output: 3 4 5
}
```

Sử dụng khi cần làm việc với những tập unique, quan tâm đến thứ tự

### Unordered_set

Tương tự như set, nhưng các phần tử không được sắp xếp tăng dần như trong set.

Dùng tương tự set, chả khác gì.

Nhưng các thao tác có độ phức tạp O(1)

- Xét bài toán: nhập vào n số nguyên, đếm xem có bao nhiêu số khác nhau
    
    - Nếu dùng set:
    
    ```C++
    \#include <iostream>
    \#include <set>
    
    int main() {
        int n;
        std::cin >> n;
        
        std::set<int> s;
        for (int i = 0, tmp; i < n; i++) { // O(n)
            std::cin >> tmp;
            s.insert(tmp); // O(logn)
        }
        // => O(nlogn)
        
        std::cout << s.size();
    }
    ```
    
    - Nếu dùng unordered_set
    
    ```C++
    \#include <iostream>
    \#include <unordered_set>
    
    int main() {
        int n;
        std::cin >> n;
        
        std::unordered_set<int> s;
        for (int i = 0, tmp; i < n; i++) { // O(n)
            std::cin >> tmp;
            s.insert(tmp); // O(1)
        }
        // => O(n)
        
        std::cout << s.size();
    }
    ```
    

### Map

Map là cấu trúc dữ liệu lưu dữ liệu dưới dạng 1 cặp `(key, value)` tương ứng với nhau. VD mảng bình thường cũng có thể coi là lưu dữ liệu thành các cặp `(key, value)`, với `key` là các chỉ số của mảng:

- VD: `int a[] = {1, 2, 3, 4, 5}`
    - `a[0]` = 1
    - `a[1]` = 2
    - …

Mảng không cho phép tự tạo key, mà các key là các số nguyên từ 0 đến n - 1

Map sẽ cho phép tự tạo key. Các key trong map sẽ được sắp xếp tăng dần.

- Khai báo: `map<kiểu_của_key, kiểu_của_value> tên`
    - VD: `map<string, int> m`
        - Khai báo một map có kiểu của key là `string` và kiểu của value là `int` - có tên là `m`
        - `m[”hello”] = 1` ⇒ Cứ khi nào gọi đến `m["hello"]` nó sẽ trả về 1
- Các hàm chức năng
    - Toán tử `[a]` - truy cập đến phần tử có `key = a` trong map,
        - `m[a] = b` nếu trong map chưa có key `a` thì nó sẽ tự thêm vào và gán bằng `b`
        - `std::cout << m[a]` nếu trong map chưa có key `a` thì nó sẽ trả về 0 ⇒ trước khi gọi thì nên kiểm tra `if(count(a))`
    - Thay vì `m.insert(pair<...>(a, b))` thì có thể viết thành `m[a] = b`
    - `find(a)` - truyền vào một key, trả về con trỏ trỏ đến phần tử có key = a
        - Nếu mà không thấy thì sẽ trả về `map.end()`
    - `count(a)` - truyền vào một key, trả về 1 nếu có phần tử có key đó, 0 nếu ngược lại

Các hàm chức năng sẽ có độ phức tạp là `O(logn)` với `n` là số phần tử của map hiện tại

### Unordered_map

Giống map nhưng không sort ⇒ độ phức tạp của các hàm chức năng là O(1)

### Lưu ý khi dùng unordered_set với unordered_map

Độ phức tạp của các hàm chức năng là O(m) với m là độ dài của key

VD có một `unordered_map<std::string, int> m` sau đó có `std::string s` và gọi đến `m[s]` ⇒ ĐPT trên lý thuyết là O(1), nhưng thực tế ĐPT là O(s.size())

VD có một `unordered_set<std::string> s` sau đó có `std::string t` và gọi `s.insert(t)` ⇒ DPT cũng là O(t.size())

## 3. Others

### Pair

Có thể hiểu đơn giản là một cặp giá trị (có thể khác kiểu nhau)

- Khai báo: `pair<kiểu_1, kiểu_2> tên(giá_trị_1, giá_trị_2)`
    - Phần `(giá_trị_1, giá_trị_2)` có thể bỏ
    - VD: `pair<int, int> a(5, 5)` - a là một cặp giá trị gồm 2 số nguyên đều bằng 5
    - `pair<std::string, double> b("hello", 1.1)` - b là một cặp giá trị gồm một chuỗi và một số double
- Các hàm chức năng
    - `first` lấy giá trị đầu tiên
        - VD bên trên `b.first` = “hello”
        - `a.first` = 5
    - `second` lấy giá trị thứ 2
        - `b.second` = 1.1
        - `a.second` = 5

Hữu ích trong trường hợp là có một mảng gồm các cặp dữ liệu đi với nhau thì tạo 1 mảng gồm các pair thay vì tách thành 2 mảng riêng biệt