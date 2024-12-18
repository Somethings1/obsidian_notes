# Good Array

---

```C++
\#include <iostream>
\#include <unordered_map>

int main() {
    int n;
    std::cin >> n;
    
    std::unordered_map<int, int> freq;
    while (n--) {
        int tmp;
        std::cin >> tmp;
        freq[tmp]++;
    }
    
    int count = 0;
    for (auto num: freq) {
        // num.first la gia tri
        // num.second la so lan xuat hien tuong ung
        if (num.second < num.first) count += num.second;
        else count += num.second - num.first;
    }
    std::cout << count;

    return 0;
}
```

# Max k-sum

## Tóm tắt

Cho n, k và mảng n phần tử. Tìm mảng con có tổng lớn nhất mà độ dài không quá k

## Ý tưởng

Vì n, k ≤ 1000 nên độ phức tạp `O(n*k)` vẫn chấp nhận được

Với mỗi điểm xuất phát, cho tổng hiện tại bằng 0, dịch điểm kết thúc từ phía sau điểm xuất phát đến khi nào kết thúc - xuất phát + 1 ≤ k.

Với mỗi điểm kết thúc, cập nhật tổng hiện tại

## Code

```C++
\#include <iostream>

int main() {
    int n, k;
    std::cin >> n >> k;
    
    int a[n];
    for (int i = 0; i < n; i++) std::cin >> a[i];
    
    long long maxSum = -1e12;
    for (int start; start <= n - k; start++) {
        long long currentSum = 0;
        for (int end = start; end < start + k && end < n; end++) {
            currentSum += a[end];
            maxSum = std::max(maxSum, currentSum);
        }
    }
    
    std::cout << maxSum;
}
```

# Binary search

---

## Ứng dụng

Tìm nghiệm của hàm đơn điệu

- Hàm đơn điệu là hàm x1 ≤ x2 thì f(x1) ≤ f(x2) hoặc x1 ≤ x2 thì f(x1) ≥ f(x2)
- VD tìm một số x trong mảng sắp xếp tăng dần thì cũng là tìm nghiệm của hàm đơn điệu
    - Coi tên mảng là một hàm số. Tham số của hàm là chỉ số của mảng
    - Tăng chỉ số lên thì giá trị tăng lên
- VD bài Cows 1
    - Có n cái chuồng và k con bò. Tìm cách đặt mỗi con bò vào 1 chuồng sao cho khoảng cách gần nhất giữa 2 con bò là xa nhau nhất có thể. Tìm khoảng cách đó
    - Đặt hàm f(x) = Có thể đặt các con bò sao cho khoảng cách gần nhất giữa 2 con không nhỏ hơn x hay không
        - Nếu có thể đặt các con bò sao cho khoảng cách gần nhất giữa 2 con không nhỏ hơn x thì hoàn toàn có thể đặt các con bò sao cho khoảng cách giữa 2 con không nhỏ hơn x - 1
        - Nếu f(x) = 1 thì f(x - 1) = 1
        - Bài toán quy về tìm x0 sao cho f(x0) = 1 và f(x0 + 1) = 0
        - Hàm f(x) sẽ bằng 1 với mọi x ≤ x0, x0 là đáp án của bài toán.

## Dấu hiệu để áp dụng tìm kiếm nhị phân

- Có hàm đơn điệu và bài toán yêu cầu tìm một giá trị x nào đó trên hàm đó
- Có một số n vào khoảng 1e5 và đáp án của bài toán có thể nằm trong khoảng rất lớn (vài tỷ, vài triệu tỷ) ⇒ ĐPT = `O(nlog(không gian nghiệm))`
    - Bài Cows 1: ĐPT `O(nlog(vị trí max))` = 3e6
    - Tìm kiếm nhị phân trên mangr: ĐPT `O(nlogn)` = 1e6

## Phương pháp chung

Giả sử f(x) là hàm giảm (Cows 1)

- Duy trì 2 biến `left` và `right` là giá trị nhỏ nhất và giá trị lớn nhất của khoảng tìm kiếm
- `while(left <= right)`:
    - Tính `mid = (low + high) / 2` - vị trí giữa của khoảng tìm kiếm
    - Kiểm tra f(mid), nếu f(mid) = 1 thì cho `kết quả = mid`, `left = mid + 1`
        - `kết quả = mid` : có khả năng mid là giá trị mà f(mid) = 1 và f(mid + 1) = 0 ⇒ tìm trong đoạn sau sẽ không ra giá trị nào cho f = 1 nữa, nên phải gán luôn
        - `left = mid + 1`: giá trị giữa hiện tại đang thoả mãn, nên cố gắng tìm kiếm một giá trị lớn hơn bằng cách dịch khoảng tìm kiếm ra nửa sau
    - Nếu f(mid) = 0 thì cho `right = mid - 1`: giá trị giữa hiện tại đang không thoả mãn ⇒ nửa sau cũng đều là 0 hết ⇒ tìm trong nửa trước của khoảng

Mọi bài toán tìm kiếm nhị phân đều có chung cách này. Vấn đề chỉ quay về là xác định và viết được hàm f sao cho độ phức tạp để tính toán hàm f phải khả thi.

# Chữa Cows 1

```C++
\#include <iostream>
\#include <algorithm>

const int maxn = 1e5 + 1;
int n, k;
int a[maxn];

// Tra ve 1 neu co the dat k con bo sao cho khoang cach gan nhat giua 2 con bo la >= x
bool f (int x) {
    // Dat con bo dau tien o vi tri dau tien
    int lastPosition = a[0]; // Vi tri cuoi cung ma da dat 1 con bo vao
    int count = 1; // so con bo da duoc dat vao chuong
    
    for (int i = 1; i < n; i++) {
        if (a[i] - lastPosition >= x) {
            // Vi tri hien tai da cach con bo cuoi cung mot khoang >= x
            lastPosition = a[i];
            count++;
        }
        if (count == k) {
            // Neu da dat duoc het k con bo thi tuc la dat duoc
            return 1;
        }
    }
    return 0;
}

int main() {
    std::cin >> n >> k;
    for (int i = 0; i < n; i++) std::cin >> a[i];
    
    std::sort(a, a + n);
    
    int left = 1, right = a[n - 1] - a[0], res;
    while (left <= right) {
        int mid = (left + right) / 2;
        
        if (f(mid) == 1) {
            res = mid;
            left = mid + 1;
        }
        else {
            right = mid - 1;
        }
    }
    
    std::cout << res;
} 
```

# Profitting

```C++
\#include <iostream>
\#include <algorithm>

const int maxn = 1e6 + 1;
int n, q;
int a[maxn];

int find (int k) {
    int left = 0, right = n - 1, res = -1;
    
    while (left <= right) {
        int mid = (left + right) / 2;
        
        if (a[mid] > k) {
            right = mid - 1;
            res = mid;
        }
        else {
            left = mid + 1;
        }
    }
    
    return res;
}

int main() {
    std::cin >> n >> q;
    for (int i = 0; i < n; i++)
        std::cin >> a[i];
        
    for (; q--; ) {
        int query;
        std::cin >> query;
        
        int pos = find(query);
        if (pos < 0)
            std::cout << -1 << '\n';
        else 
            std::cout << a[pos] << '\n';
    }
}
```