# Chương trình C++ đầu tiên

---

```C++
\#include <iostream>
using namespace std;

int main () {
	cout << "Hello World!";
}
```

- Lập trình là ra lệnh cho CPU làm 1 cái gì đó
- Cout là lệnh in ra màn hình. Cout << “Hello” là in ra dòng “Hello”
- Bao bên ngoài cout là **hàm main**. Mọi chương trình C++ đều bắt đầu từ hàm main()
    - Máy tính sẽ đi tìm xem hàm main nằm ở đâu và bắt đầu xử lý từ trong màn main
- **Using namespace std coi như là bắt buộc phải có**
- Include là khai báo thư viện
    - Thư viện là một file chứa các chức năng được viết sẵn cho 1 mục đích nào đó. Mình có thể sử dụng thư viện để làm các công việc được người viết thư viện đã viết sẵn.
    - Thư viện iostream chứa cin, cout, cerr, getline… để phục vụ cho việc đọc từ bàn phím và ghi ra màn hình
    - 1 vài thư viện:
        - cmath: chứa các hàm toán học (sin cos, tan)
        - Algorithm: chứa các hàm thuật toán cơ bản (chặt nhị phân, sắp xếp)

```C++
\#include <iostream>
\#include <cmath>
using namespace std;

// Chương trình giải phương trình bậc 2
int main () {
	float a, b, c;
	cin >> a >> b >> c;
	
	float delta = b * b - 4 * a * c;
	
	if (delta < 0) {
		cout << "Vô nghiệm";
	}
	else if (delta == 0) {
		cout << "x1 = x2 = " << -b / (2 * a);
	}
	else { // delta > 0
		// Hàm sqrt(x) nằm trong thư viện cmath
		// Nhận vào 1 số, trả về căn bậc 2 của số ấy
		cout << "x1 = " << (-b + sqrt(delta)) / (2 * a) << '\n';
		cout << "x2 = " << (-b - sqrt(delta)) / (2 * a) << '\n';
	}
	return 0;
}
```

# Vòng lặp

- Vòng lặp for:
    - Cú pháp: for (<s1>; <condition>; <s2>) <s3>
        1. <s1> được thực hiện
        2. Kiểm tra <condition>, nếu đúng thì thực hiện bước 3, nếu sai thì đến bước 5
        3. Thực hiện <s3> và <s2>
        4. Quay lại bước 2
        5. Thoát khỏi vòng lặp
- Vòng lặp while:
    - Cú pháp: while (<condition>) <s1>
        1. Kiểm tra <condition>, nếu đúng thì sang bước 2, sai thì bước 4
        2. <s1> được thực hiện
        3. Nhảy về bước 1
        4. Thoát khỏi vòng lặp

# Mảng

- Mảng là một dãy các biến đặt cạnh nhau trong bộ nhớ, có cùng 1 cái tên. Do đó từng biến phải được xác định bằng chỉ số của nó trong mảng. Chỉ số bắt đầu từ 0 => mảng có n phần tử thì phần tử cuối cùng là n - 1
- VD 1 con phố cũng giống như một mảng: có tên đường, có thể xác định các ngôi nhà bằng số nhà. VD số 10 Hoa Lư
- Khai báo mảng: <kiểu biến> <tên mảng>[số phần tử];

```C++
\#include <iostream>
\#include <string>
using namespace std;

int main () {
	string h = "Hello"; // h có thể coi là mảng kiểu char với độ dài 5
	// Chuỗi là mảng các ký tự
	cout << h[4]; // output: o
}
```

# Hàm

- Hàm là một đoạn code được đặt tên, hay còn gọi là chương trình con.
    - Chương trình bao gồm input, xử lý và output
    - Chương trình con (hàm) cũng bao gồm những yếu tố đó, và nó nằm trong chương trình chính
- Chỉ có duy nhất hàm void là không bao gồm output
- Từ khoá return (trả về, trả ra) để xác định output của hàm

```C++
\#include <iostream>
using namespace std;

// f(x) = x + 1;
int f(int x) {
	return x + 1;
}

int main () {
	cout << f(4); // 5
}
```