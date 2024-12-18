# 1. Largest sum

![[/image 3.png|image 3.png]]

> [!important] Đề thi thường có nhiều subtask, mỗi subtask là một giới hạn của những số cho trong đề bài
> 
> Thường là mỗi subtask tương ứng với một thuật toán khác nhau, từ dễ đến khó
> 
> Subtask đầu tiên sẽ tương ứng với cách trâu bò nhất có thể nghĩ ra

## Subtask 1: n ≤ 1e4

---

B1: Lặp m từ n - 2 về 1. Tạo 1 biến `max_sum = 0, max_m = 0`

B2: Kiểm tra tổng gcd(n, m) + m, nếu tổng này lớn hơn max_sum thì `max_sum = gcd(m, n) + m, max_m = m`

B3: Sau vòng lặp thì in ra max_m

> [!important] Trong thư viện numeric có hàm gcd(m, n)
> 
> Trong thư viện algorithm có hàm __gcd(m, n)
> 
> Đều để tính gcd

## Subtask 2: n ≤ 1e14

---

### Nhận xét 1

gcd(m, n) + m với m < n thì không vượt quá n, hay

`gcd(m, n) + m ≤ n (m < n)`

Chứng minh:

Gọi g là gcd(m, n)

ta có thể viết

n = g * p, m = g * q (p > q vì n > m)

g + m = g + g * q = g * (1 + q)

Mà q < p nên 1 + q ≤ p

⇒ g + m = g * (1 + q) ≤ g * p = n

⇒ ĐPCM

### Nhận xét 2

`Nếu n chia hết cho k thì n - k cũng chia hết cho k`

k là một ước chung của n và n - k (với k đủ nhỏ)

### Nhận xét 3

Từ 2 nhận xét trên

⇒ `gcd(n - k, n) + n - k = k + n - k = n với mọi k mà n chia hết cho k`

  

Từ 3 nhận xét trên ⇒ việc giải bài toán ban đầu quy về `tìm số k nhỏ nhất sao cho n chia hết cho k` và kết quả bài toán là `n - k`

⇒ Tìm k trong khoảng từ 2 đến sqrt(n), vì nếu từ 2 đến sqrt(n) mà không có số k nào mà n chia hết cho k thì n là số nguyên tố

Nếu n là số nguyên tố ⇒ ước chung lớn nhất của n với mọi số nhỏ hơn nó đều bằng 1 ⇒ in ra số lớn nhất có thể và chấp nhận gcd(m, n) + m < n

cụ thể là gcd(m, n) + m = n - 1, với m = n - 2 là trường hợp tốt nhất có thể

# 2. Numpad

![[/image 1 2.png|image 1 2.png]]

Bài này không nói rõ là xâu có thể rỗng hay không. Nếu xâu rỗng mà n = 1e7 thì thuật toán sẽ phức tạp hơn 1 chút. Còn nếu không thì chỉ cần duyệt bình thường.

## Toàn bộ subtask

---

B1: lặp các số từ 1 đến n

B2: với mỗi số, kiểm tra nó có chứa chữ số bị hỏng hay không

Tách từng chữ số của một số ra

> [!important] Lưu mảng bool cho các số dùng được:
> 
> available[i]: chữ số i có dùng được hay không
> 
> Ban đầu available[i] = 1 với mọi i
> 
> Khi nhập chữ số bị hỏng k thì cho available[k] = 0

> [!important] Để tách từng chữ số trong một số n
> 
> while (n) {
> 
> // n % 10 là chữ số cuối cùng của n
> 
> // Kiểm tra chữ số cuối cùng này xem có available không
> 
> n /= 10; // loại bỏ chữ số cuối cùng khỏi n
> 
> }

⇒ Hàm check số k có hợp lệ hay không

```C++
bool available[10]; // giả sử đã có mảng available như trên 
bool valid (int k) {
	while (k) {
		if (!available[k % 10]) return false;
		k /= 10;
	}
	return 1;
}
```

# 3. The game

![[/image 2 2.png|image 2 2.png]]

![[/image 3 2.png|image 3 2.png]]

Cho mảng n số nguyên dương và 1 số m, tìm số k nhỏ nhất sao cho mọi đoạn con có độ dài là k có tổng không nhỏ hơn m.

## Subtask1: n ≤ 5000

---

Thử mọi k có thể (1 ≤ k ≤ n) rồi với mỗi k thì kiểm tra xem điều kiện chính là mọi đoạn con có độ dài k đều có tổng ≥ m có thoả mãn hay không

ĐPT: lặp k từ 1 đến n ⇒ O(n)

Với mỗi k, đi 1 lượt qua mảng để kiểm tra điều kiện chính có thoả mãn không ⇒ O(n)

⇒ ĐPT lời giải O(n^2)

## Subtask 2: n ≤ 1e6

Chặt nhị phân số k trong khoảng 1 đến n ⇒ ĐPT chỉ còn O(logn) ⇒ ĐPT lời giải là O(nlogn)

> [!important] Chặt nhị phân:
> 
> - Xác định hàm f(x) đơn điệu trong bài toán
>     - Ở bài này, hàm f(x) = có phải mọi đoạn con có độ dài là x đều có tổng không nhỏ hơn m hay không
>     - Bởi vì mảng này dương, nên nếu f(x) = true, thì f(x + 1) = true
>     - Hàm f(x) có dạng …0, 0, 0, 1, 1, 1…
> - Xác định dạng của hàm f(x) là tăng hay giảm (trong bài này là hàm tăng)
> - Áp dụng form để tìm số 1 đầu tiên
> 
> ```C++
> int left = 1, right = n, res = -1;
> while (left <= right) {
> 	int mid = (left + right) / 2;
> 	if (f(mid)) {
> 		res = mid; // tuỳ từng bài mà cho nó ở đây hay ở phần else 
> 		right = mid - 1; // tuỳ từng bài mà cái này có thể 
> 		// đổi chỗ với left = mid + 1
> 	}
> 	else {
> 		left = mid + 1;
> 	}
> }
> ```
> 
> VD hàm f có dạng …0, 0, 0, 1, 1, 1… và muốn tìm số 0 cuối cùng (bài toán thay đổi thành tìm số k lớn nhất sao cho có ít nhất một đoạn con có tổng nhỏ hơn m)
> 
> ```C++
> int left = 1, right = n, res = -1;
> while (left <= right) {
> 	int mid = (left + right) / 2;
> 	if (f(mid)) {
> 		right = mid - 1; 
> 	}
> 	else {
> 		res = mid;
> 		left = mid + 1;
> 	}
> }
> ```

# 4. Increasing number

![[image 4.png]]

## Hướng giải 1

---

Nghĩ đến một mảng chứa các số từ 0 đến 9 `{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}`

Mỗi 1 tập con của mảng này cho ta một số tăng

`{0, 4, 5}` ⇒ 45

`{1, 4, 5, 6}` ⇒ 1456

Mảng có n phần tử thì có 2^n tập con (mỗi phần tử có thể được chọn hoặc không được chọn vào trong tập con ⇒ có 2 phương án cho mỗi phần tử ⇒ số tập con = 2 ^ n)

⇒ Số lượng các số tăng chỉ là 2 ^ 10 (hoặc 2 ^ 9, tầm đấy, nói chung là tương đối nhỏ)

⇒ Cách giải

B1: Sử dụng bitmask để tạo ra tất cả các tập con của mảng này

B2: từ đó tạo ra một mảng chứa tất cả các số tăng. Mảng này có khoảng 2 ^ 10 phần tử.

B3: Ta sẽ sắp xếp mảng này tăng dần

B4: Với mỗi truy vấn `a` ⇒ dùng `upper_bound` để tìm số nhỏ nhất lớn hơn `a`

1. nếu số này là số đầu tiên trong mảng thì in ra 0
2. còn ngược lại thì in ra số trước nó (vì `upper_bound` tìm ra số nhỏ nhất lớn hơn `a` nên số trước nó sẽ là số lớn nhất mà ≤ `a`)

  

Ở mỗi truy vấn thì ĐPT là O(log(n)) với n là số phần tử của mảng chứa các số tăng

⇒ ĐPT tổng của bài là O(Tlog(n))

## Hướng giải 2

---

Tập trung vào vấn đề implementation.

Tư tưởng: để tìm số tăng lớn nhất nhỏ hơn hoặc bằng n thì thực hiện các bước sau

B1: tách số n thành mảng chứa các chữ số của nó. Gọi mảng này là a, coi a có m phần tử

B2: for (int i = 0; i < m - 1; i++)

- if (a[i] < a[i + 1]) continue;
- while (a[i] ≥ a[i + 1]) a[i]—, i—;
- a[m - 1] = 9;
- for (int j = m - 2; j > i; j—) a[j] = a[j + 1] - 1;

B3: Sau vòng lặp thì lập lại số từ mảng a

ĐPT: O(m*T) với m là số chữ số trong số đang xét ⇒ tương đương với cách trên về độ phức tạp O(), nhưng độ phức tạp khi gõ code và kiểm tra lỗi sẽ lớn hơn.

# Tổng kết

Buổi hôm nay, ngoài việc chữa bài, chúng ta đã nghiên cứu về

1. Cách phân bố điểm trong bài thi điển hình
2. Cách tách các chữ số của một số
3. Ôn lại về chặt nhị phân: hàm đơn điệu, cách áp dụng công thức chặt nhị phân

Em cố gắng tự code các bài này. Mở lại lý thuyết các buổi trước anh đã dạy, kết hợp tìm kiếm trên mạng, cộng với chăm chỉ thì chắc chắn sẽ code được 3 bài đầu, bài cuối thì hên xui. Chúc em may mắn