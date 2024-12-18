# Binary exponentiation

---

Xuất phát từ phép nhân của ae Ấn Độ. Ý tưởng của phép nhân Ấn Độ là a * b = a + a + a… + a. Nếu b chẵn thì a * b = (a * 2) * (b / 2). Nếu b lẻ thì a * b = (a * 2) * (b / 2) + a ⇒ thay vì phải thực hiện b - 1 phép cộng thì nếu ta cứ liên tục tách b ra thì cuối cùng ta chỉ cần thực hiện log2(b) phép cộng mà thôi.

Binary exponentiation tương tự như thế, nhưng áp dụng cho phép luỹ thừa.

Để tính `a ^ b` thì phải có một vòng lặp

```C++
// Hàm tính a ^ b
int pow (int a, int b) {
	int res = 1;
	while (b--) res *= a;
	return res;
}
// ĐPT: O(b)
```

  

> [!important] Trường hợp luỹ thừa có số mũ quá lớn thì kết quả cũng sẽ quá lớn ⇒ không tính được mà chỉ yêu cầu tính kết quả khi chia lấy dư cho một số nào đó thôi (1e9+7, 99244353)
> 
> Phép chia lấy dư (mod) có tính chất phân phối với cả phép nhân và phép cộng
> 
> `(a + b) % p = (a % p + b % p) % p`
> 
> `(a * b) % p = (a % p * b % p) % p`

  

```C++
// Hàm tính a ^ b % p
int pow (int a, int b, int p) {
	int res = 1;
	while (b--) {
		res = 1ll * res * a % p
	}
	return res;
}
```

Nhận xét: hàm này chạy được, nhưng khi b lớn thì lại không hiệu quả.

> [!important] a ^ b = a * a * a…* a (b lần)
> 
> Nếu b chẵn:
> 
> a ^ b = (a ^ 2) ^ (b / 2)
> 
> Nếu b lẻ:
> 
> a ^ b = (a ^ 2) ^ (b / 2) * a

  

```C++
// Tính a ^ b
int pow (int a, int b) {
	if (b == 1) return a;
	if (b == 0) return 1;
	
	if (b % 2 == 0) return pow(a * a, b / 2);
	else return pow(a * a, b / 2) * a;
}
// ĐPT: O(log2(b))
```

Khử đệ quy

```C++
int pow (int a, int b, int p) {
		int res = 1;
		while (b != 0) {
			if (b % 2 == 1) res = 1ll * res * a % p;
			b = b / 2;
			a = 1ll * a * a % p;
		}
}
```

Viết gọn lại, dùng phép toán bit để tối ưu hoá

```C++
int pow (int a, int b, int p) {
		int res = 1;
		for (; b; b >>= 1, a = 1ll * a * a % p) 
			if (b & 1) res = 1ll * res * a % p;
}
// b >>= 1 tương đương b = b / 2
// b & 1 tương đương b % 2 == 1
```

# Sàng Eratosthenes

---

Thường gọi tắt là sàng

Sàng để liệt kê các số nguyên tố nhỏ hơn n, với n tương đối lớn. ĐPT của sàng là `O(nlog(log(n)))`

Tư tưởng của phép sàng là:

Viết các số từ 2 đến 100 lên giấy. Lấy số đầu tiên (số 2), gạch bỏ tất cả các số là bội của 2, trừ chính nó. Lấy số tiếp theo mà chưa bị gạch, lại gạch tất cả bội của nó, trừ chính nó. Làm tương tự như vậy đến khi không còn số nào nữa. Những số còn sót lại (chưa bị gạch) là các số nguyên tố.

## Sàng cơ bản

```C++
// Hàm nhận vào số n, trả về vector có n phần tử, mỗi phần tử là một biến bool
// Phần tử thứ i là 1 nếu i là số nguyên tố
vector<bool> prime (int n) {
	vector<bool> res(n, 1);
	
	res[0] = res[1] = 0;
	for (int i = 2; i <= n; i++) {
		if (res[i] == 1) {
			for (int j = i + i; j <= n; j += i) {
				res[j] = 0;
			}
		}
	}
	
	return res;
}
```

  

## Cải tiến thời gian

```C++
vector<bool> prime (int n) {
	vector<bool> res(n, 1);
	
	res[0] = res[1] = 0;
	for (int i = 2; i * i <= n; i++) {
		if (res[i] == 1) {
			for (int j = i * i; j <= n; j += i) {
				res[j] = 0;
			}
		}
	}
	
	return res;
}
```

## Cải tiến thêm ước mỗi số

Trong một số bài toán, sẽ có yêu cầu phân tích một số tự nhiên thành các thừa số nguyên tố.

Nếu cần phân tích nhiều hoặc số lớn thì sử dụng sàng sẽ rất hiệu quả.

```C++
// Hàm trả về một vector có n phần tử, mỗi phần tử là một số nguyên sao cho
// giá trị của phần tử thứ i là ước nguyên tố nhỏ nhất của i
vector<int> prime (int n) {
	vector<int> res(n, 1);
	
	for (int i = 2; i * i <= n; i++) {
		// Ước nguyên tố nhỏ nhất của i mà bằng i <=> i là số nguyên tố
		if (res[i] == i) {
			for (int j = i * i; j <= n; j += i) {
				res[j] = i;
			}
		}
	}
	
	return res;
}

// Hàm nhận vào một số tự nhiên và phân tích nó thành các thừa số nguyên tố
// Trả về một vector là các số trong phép nhân dưới
// 8473476 = 2×2×3×11×23×2791
// [2, 2, 3, 11, 23, 2791]
vector<int> pt (int n, vector<int> &pr) {
	vector<int> res;
	while (n != 1) {
		res.push_back(pr[n]);
		n /= pr[n];
	}
	return res;
}

// Đâu đó trong hàm main
vector<int> pr = prime(n);
vector<int> primaryFactorOfN = pt(n, pr);
```

# Sliding window

---

Hay còn gọi là “cửa sổ trượt”. “Cửa sổ” ở đây được hiểu là một đoạn con (liên tiếp). Phương pháp cửa sổ trượt có tư tưởng là quan tâm đến một đoạn con nhất định, sau đó dịch chuyển sự quan tâm đó đi dần dần (trượt).

## Fixed size

VD: tìm đoạn con có độ dài K với tổng lớn nhất

Tạo ra một cửa sổ có độ dài là K (quan tâm đến tổng của K số đầu tiên trong mảng). Dịch cửa sổ sang 1 ô <=> bỏ số tận cùng bên trái của cửa sổ và thêm số ngay sau số ở ngoài bên phải của cửa sổ. Cứ lặp lại cho đến hết.

```C++
\#include <iostream>

int main () {
	int n, k;
	std::cin >> n >> k;
	
	int a[n];
	for (int i = 0; i < n; i++) 
		std::cin >> a[i];
	
	int max_sum = 0;
	for (int i = 0; i < k; i++) max_sum += a[i];
	
	int cur_sum = max_sum;
	for (int i = k; i < n; i++) {
		cur_sum += a[i] - a[i - k];
		if (max_sum < cur_sum) max_sum = cur_sum;
	}
	std::cout << max_sum;
}
```

## Variable size

VD: tìm chuỗi con ngắn nhất chứa tất cả các chữ cái xuất hiện trong s

- Có một biến A để lưu những vấn đề mình quan tâm đến trong khung cửa sổ
- Duy trì 2 con trỏ, 1 con thể hiện giới hạn trái của cửa sổ, 1 con thể hiện giới hạn phải
- Chừng nào mà biến A còn chưa thoả mãn yêu cầu của mình thì mình sẽ còn tăng giới hạn phải lên
- Chừng nào mà biến A còn vượt quá yêu cầu của mình thì mình sẽ còn tăng giới hạn trái lên