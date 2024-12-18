# Lý thuyết

---

Sàng Eratosthenes là một thuật toán để tìm các số nguyên tố nhỏ hơn `n` với độ phức tạp tương đối nhỏ ~ O(n)

Cách cài đặt:

```C++
const int maxn = 1e7 + 1;
bool prime[maxn];

void sieve () {
	// Mặc định tất cả đều là snt
	for (int i = 0; i < maxn; i++) prime[i] = 1;
	
	prime[0] = prime[1] = 0;
	for (int i = 2; i * i < maxn; i++) {
		if (prime[i]) {
			for (int j = i * i; j < maxn; j += i)
				prime[j] = 0; 
		}
	}
}

...
sieve();
// Có mảng prime[i] = 1 nếu i là số nguyên tố và ngược lại
```

⇒ Ứng dụng của sàng là tìm danh sách các số nguyên tố nhỏ hơn n

# Lesser prime

---

Cho số n, đếm số số nguyên tố nhỏ hơn n

⇒ Sàng, sau đó đếm số số 1 trong mảng prime

```C++
\#include <iostream>

const int maxn = 1e7;
bool prime[maxn];

void sieve (int n) {
    for (int i = 0; i < n; i++) prime[i] = 1;
    
    prime[0] = prime[1] = 0;
    for (int i = 2; i * i < n; i++) {
        if (prime[i]) {
            for (int j = i * i; j < n; j += i)
                prime[j] = 0; 
        }
    }
}

int main() {
    int n;
    std::cin >> n;
    
    sieve(n);
    
    int count = 0;
    for (int i = 2; i < n; i++) count += prime[i];
    std::cout << count;
}
```

# Goldbach

---

Giả thuyết Goldbach: với mỗi số tự nhiên chẵn lớn hơn 2 ta có thể phân tích số đó thành tổng của 2 số nguyên tố.

Cho q số tự nhiên lớn hơn 2, hãy phân tích mỗi số thành tổng của 2 số nguyên tố.

## Hướng dẫn giải

B1: lập một mảng prime bằng thuật toán sàng

B2: lập một vector chứa các số nguyên tố

B3: với mỗi số `x` trong truy vấn ⇒ duyệt qua lần lượt các số trong vector trên. Giả sử đang duyệt đến số `p` nào đó. Nếu `prime[x - p] == 1` thì in ra `p` và `x - p` rồi thoát khỏi vòng lặp

```C++
\#include <iostream>
\#include <vector>

const int maxn = 1e7 + 1;
bool prime[maxn];

void sieve () {
    for (int i = 2; i < maxn; i++)
        prime[i] = 1;
    for (int i = 2; i * i < maxn; i++) {
        if (prime[i]) {
            for (int j = i * i; j < maxn; j += i) {
                prime[j] = 0;
            }
        }
    }
}



int main() {
    sieve();
    std::vector<int> primes;
    for (int i = 2; i < maxn; i++) {
        if (prime[i]) primes.push_back(i);
    }
    // Sau vong lap, ta co vector primes chua tat ca 
    // so nguyen to nho hon 10^7
    
    int q;
    std::cin >> q;
    
    while (q--) {
        int n;
        std::cin >> n;
        
        for (int p: primes) {
            if (prime[n - p]) {
                std::cout << p << ' ' << n - p << '\n';
                break;
            }
        }
    }
}
```

# Princess

---

## Hướng dẫn giải

B1: sàng để ra mảng prime ⇒ bài toán quy về đếm số số 1 trong khoảng từ prime[L] đến prime[R]

Nhận xét: mảng prime chỉ có mỗi số 1 và số 0 ⇒ số số 1 trong 1 đoạn nào đó chính là tổng của đoạn con đó

B2: lập mảng tổng tiền tố cho prime.

B3: trả lời truy vấn bằng cách lôi mảng tổng tiền tố ra