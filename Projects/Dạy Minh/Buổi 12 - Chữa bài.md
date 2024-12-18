# I. `upper_bound` and `lower_bound`

- 2 hàm này trong `algorithm`
- `upper_bound(it1, it2, x)` :
    - Dùng để tìm số nhỏ nhất mà lớn hơn x trong khoảng giữa con trỏ it1 đến trước it2 với điều kiện là các số giữa it1 và it2 đã được sắp xếp không giảm
    - Dùng với mảng: tên mảng cũng là một con trỏ trỏ đến phần tử đầu tiên của mảng
        - Tìm từ đầu đến cuối mảng `a` có `n` phần tử
            
            `upper_bound(a, a + n, x)`
            
        - Tìm từ phần tử thứ 2 đến phần tử thứ 5 của mảng
            
            `upper_bound(a + 1, a + 5, x)`
            
    - Dùng với vector: dùng `vector.begin()` và `vector.end()`
        - Tìm từ đầu đến cuối vector `a`
            
            `upper_bound(a.begin(), a.end(), x)`
            
    - Trả về con trỏ trỏ vào phần tử cần tìm (tức là phần tử nhỏ nhất lớn hơn x)
        - VD: in ra giá trị của phần tử nhỏ nhất lớn hơn x trong mảng `a`
            
            `cout << *upper_bound(a, a + n, x) << ‘\n’;`
            
        - VD: in ra chỉ số của phần tử nhỏ nhất lớn hơn x trong mảng `a`
            
            `cout << upper_bound(a, a + n, x) - a << '\n';`
            
        - VD: in ra giá trị của phần tử nhỏ nhất lớn hơn x trong vector `a`
            
            `cout << *upper_bound(a.begin(), a.end(), x) << '\n';`
            
        - VD: in ra chỉ số của phần tử nhỏ nhất lớn hơn x trong vector `a`
            
            `cout << distance(a.begin(), upper_bound(a.begin(), a.end(), x)) << ‘\n’;`
            
- `lower_bound(it1, it2, x)`:
    - Dùng để tìm số nhỏ nhất lớn hơn hoặc bằng x trong khoảng giữa it1 đến trước it2 với điều kiện là các số giữa it1 và it2 được sắp xếp không giảm
    - Dùng giống hệt `upper_bound`
- VD mảng a[7] = {1, 2, 3, 3, 4, 5, 6}
    - `*upper_bound(a, a + 7, 3)` = 4
        
        `upper_bound(a, a + 7, 3) - a` = 4
        
    - `*lower_bound(a, a + 7, 3)` = 3
        
        `lower_bound(a, a + 7, 3) - a` = 2
        
          
        

# II. First and last

## Cách 1

---

Dùng `lower_bound` để tìm vị trí đầu tiên xuất hiện và `upper_bound` để tìm vị trí sau vị trí cuối cùng xuất hiện

## Cách 2

---

Vì mảng đã sắp xếp tăng dần và `m` được cho trước (nếu nhập mảng xong mới nhập m thì cách này không hợp lý lắm) nên ta sẽ xử lý ngay trong lúc nhập mảng.

Vì mảng sắp xếp tăng dần nên các số bằng `m` sẽ nằm trong 1 cụm liền nhau.

Dùng 2 biến kiểu `bool` tên là `flag1` và `flag2`

- `flag1` để đánh dấu là hiện tại có nằm trong cụm các số giống với `m` hay không
- `flag2` để đánh dấu xem `flag1` đã từng được thay đổi hay chưa (đã từng nhập số bằng với `m`)
- Cuối cùng kiểm tra, nếu `flag2 = 0` thì in ra `-1 -1`

```C++
\#include <cmath>
\#include <cstdio>
\#include <vector>
\#include <iostream>
\#include <algorithm>


int main() {
    int n, k;
    std::cin >> n >> k;
    
    bool flag1 = 0;
    bool flag2 = 0;
    int a[n];
    for (int i = 0; i < n; i++) {
        std::cin >> a[i];
        if (a[i] == k && !flag1) {
            std::cout << i + 1 << ' ';
            flag1 = 1;
            flag2 = 1;
        }
        if (a[i] != k && flag1) {
            std::cout << i << ' ';
            flag1 = 0;
        }
    }
    
    if (!flag2) std::cout << "-1 -1" << '\n';
    return 0;
}
```

  

Tự chặt nhị phân

```C++
\#include <iostream>

int main() {
    int n, x;
    std::cin >> n >> x;
    
    int a[n];
    for (int i = 0; i < n; i++)
        std::cin >> a[i];
        
    // Tìm vị trí đầu tiên của x trong a
    // Tìm số nhỏ nhất lớn hơn hoặc bằng x
    // f(m): a[m] >= x
    int left = 0, right = n - 1, res = 0;
    while (left < right) {
        int m = (left + right) / 2;
        
        if (a[m] >= x) {
            right = m - 1;
            res = m;
        }
        else {
            left = m + 1;
        }
    }
    if (a[res] != x) std::cout << -1 << ' ';
    else std::cout << res + 1 << ' ';
    
    // Tìm vị trí cuối cùng của x trong a
    // Tìm số lớn nhất nhỏ hơn hoặc bằng x
    // f(m): a[m] <= x
    left = 0, right = n - 1, res = 0;
    while (left < right) {
        int m = (left + right) / 2;
        
        if (a[m] <= x) {
            left = m + 1;
            res = m;
        }
        else {
            right = m - 1;
        }
    }
    
    if (a[res] != x) std::cout << -1;
    else std::cout << res + 1;
}
```