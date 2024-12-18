# Ba lừn

---

## Tóm tắt bài toán

- Cho một dãy ngoặc gồm (), {}, []. Bỏ qua các chữ cái. Một dãy ngoặc đúng là một dãy ngoặc làm cho biểu thức có nghĩa. `()` là một dãy ngoặc đúng, còn `)(` thì không phải
- Dãy rỗng đương nhiên là dãy ngoặc đúng
- Nếu dãy A đúng, thì `(A)`, `[A]` và `{A}` cũng sẽ đúng
- Nếu A và B đúng, thì `AB` cũng đúng. VD `()` và `[]` đúng, thì `()[]` sẽ đúng

## Thuật toán sử dụng stack

### Nếu chỉ có một loại ngoặc

- Trường hợp bài toán chỉ có 1 loại ngoặc `()`, `[]` hoặc `{}` thì chỉ cần dùng 1 biến đếm.
- Ban đầu biến đếm = 0.
- Ta sẽ đi lần lượt từng ký tự, nếu là `(` thì tăng biến đếm lên `1`, nếu là đóng ngoặc thì kiểm tra.
    - Nếu biến đếm `=0` thì dãy ngoặc sai,
    - nếu biến đếm `!=0` thì giảm biến đếm đi 1.
- Sau khi duyệt qua dãy thì kiểm tra biến đếm lần nữa
    - Nếu biến đếm `!=0` thì dãy ngoặc là sai (số dấu mở ngoặc nhiều hơn số dấu đóng ngoặc)
    - Nếu biến đếm `=0` thì dãy ngoặc là đúng

```C++
\#include <iostream>
\#include <string>

int main () {
	std::string inp;
	std::cin >> inp;
	
	int d = 0;
	for (char c: inp) {
		if (c == '(') d++;
		else {
			if (d == 0) {
				std::cout << 0;
				reutrn 0;
			}
			else {
				d--;
			}
		}
	}
	
	if (d != 0) std::cout << 0;
	else std::cout << 1;
	return 0;
}
```

### Ý tưởng cho bài toán nhiều loại ngoặc

- Kể cả 3 biến đếm cũng không giải quyết được
    - VD: `([{)]}` là dãy ngoặc sai. Nếu dùng 3 biến đếm thì sau khi duyệt qua 3 ký tự đầu, 3 biến đếm đều lên `1`, và cuối cùng kết quả sẽ là đúng
    - ⇒ Không dùng biến đếm được nữa
- Nếu đã mở ngoặc gì thì phải đóng ngoặc đấy luôn, và khi đóng ngoặc xong, ta không cần quan tâm đến cụm đó nữa
    - `([{}])`
        - Ta thấy có chuỗi `{}` ở giữa là đúng, nên ta không quan tâm đến cụm đó nữa
- Ta sẽ quan tâm đến dấu mở ngoặc đằng trước nó, nếu dấu mở ngoặc đó có dấu đóng ngoặc ở ngay sau thì nó sẽ đúng, còn không thì sai luôn
- Sử dụng stack để lưu lần lượt các dấu mở ngoặc.
    - Khi gặp dấu đóng ngoặc, xem trên đỉnh stack có phải dấu mở ngoặc tương ứng hay không.
        - Nếu không phải thì dãy ngoặc sai.
        - Nếu đúng, bỏ nó ra khỏi stack
    - Đến cuối cùng, kiểm tra xem trong stack còn gì không.
        - Nếu còn, thì dãy ngoặc sai
        - Nếu không, dãy ngoặc đúng.

### Code

```C++
\#include <iostream>
\#include <string>
\#include <stack>

bool isPair (char a, char b) {
    if (a == '(' && b == ')') return true;
    if (a == '[' && b == ']') return true;
    if (a == '{' && b == '}') return true;
    
    return false;
}

int main() {
    std::string inp;
    std::cin >> inp;
    
    std::stack<char> s;
    bool isValid = 1;
    
    for (char c: inp) {
        if (c == '(' || c == '[' || c == '{') {
            s.push(c);
        }    
        else if (c == ')' || c == ']' || c == '}') {
            if (s.size() == 0) {
                isValid = 0;
                break;
            }
            if (isPair(s.top(), c)) {
                s.pop();
            }
            else {
                isValid = 0;
                break;
            }
        }
    }
    
    if (s.size() != 0 || !isValid) std::cout << 0;
    else std::cout << 1;
}
```

Code dùng mảng (mảng hoạt động như stack)

```C++
\#include <iostream>
\#include <string>
\#include <stack>

bool isPair (char a, char b) {
    if (a == '(' && b == ')') return true;
    if (a == '[' && b == ']') return true;
    if (a == '{' && b == '}') return true;
    
    return false;
}

int main() {
    std::string inp;
    std::cin >> inp;
    
    char s[inp.size()];
    bool isValid = 1;
    int top = -1;
    
    for (char c: inp) {
        if (c == '(' || c == '[' || c == '{') {
            top++;
            s[top] = c;
        }    
        else if (c == ')' || c == ']' || c == '}') {
            if (top < 0) {
                isValid = 0;
                break;
            }
            if (isPair(s[top], c)) {
                top--;
            }
            else {
                isValid = 0;
                break;
            }
        }
    }
    
    if (top < 0 || !isValid) std::cout << 0;
    else std::cout << 1;
}
```

# Vjp pro sỏt

---

## Tóm tắt bài toán

- n học sinh
- Mỗi học sinh có 4 thông tin
    - MSHS: int
    - Tên: string
    - Điểm giữa kỳ: double
    - Điểm cuối kỳ: double
- Sau đó là có nhiều chỉ thị
    - “MSHS”: sắp xếp ds tăng dần theo MSHS
    - “T”: sắp xếp ds tăng dần theo tên
    - “DGK”: sắp xếp ds giảm dần theo điểm giữa kỳ
    - “DCK”: sắp xếp ds giảm dần theo điểm cuối kỳ
    - “P”: in ra ds, kết thúc bằng dấu ‘-’
    - “\#”: kết thúc nhập lệnh

## Ý tưởng

- Tất cả các thông tin đều quan trọng (bởi vì mình phải in ra toàn bộ thông tin sau đó) ⇒ phải lưu tất cả thông tin
- Có thể dùng mảng, 1 mảng chứa tên, 1 mảng chứa mã số, 1 mảng chứa điểm gk, 1 mảng chứa điểm ck
- ⇒ phải tự viết hàm sort, mỗi lần sort thì phải chú ý đổi chỗ các phần tử ở cả 4 mảng
- ⇒ Dễ sai

⇒ Lưu thông tin học sinh bằng cấu trúc struct

> [!important]
> 
> ### Cấu trúc struct
> 
> Những kiểu dữ liệu thường dùng (`int`, `double`...) được gọi là kiểu dữ liệu nguyên thuỷ, chỉ chứa 1 thông tin duy nhất. Khi muốn mô tả một đối tượng phức tạp ở ngoài đời thật, ví dụ như một con người, phải có một kiểu dữ liệu gồm nhiều dữ liệu khác bên trong. Đó chính là struct.
> 
> ```C++
> struct Student {
> 	int MSHS;
> 	std::string name;
> 	double dgk;
> 	double dck;
> };
> ```
> 
> ⇒ mô tả Student gồm có các thông tin về MSHS, tên, DGK và DCK
> 
> Bây giờ, ta có thể dùng `Student` như một biến (`int`, `double`)
> 
> ```C++
> Student a; // Khai báo 1 biến a thuộc kiểu student
> // => trong a có tất cả các biến MSHS, name, dgk, dck
> a.MSHS = 1;
> a.name = "Minh"
> a.dgk = 9;
> a.dck = 8;
> 
> Student students[n]; // khai báo mảng students có kiểu là Student
> // Mỗi phần tử trong mảng đều có các chỉ số MSHS, tên, DGK & DCK
> ```

- Sau khi lưu thông tin, sẽ có phần xử lý truy vấn, với các hàm tuỳ chỉnh để sort

  

```C++
\#include <iostream>
\#include <algorithm>

struct Student {
    int MSHS;
    std::string name;
    double dgk, dck;
};

// muon sau khi sort ma a dung truoc b thi return true
bool comp1 (Student a, Student b) {
    return a.MSHS < b.MSHS;
}

bool comp2 (Student a, Student b) {
    // 2 chuoi so sanh voi nhau thi se so tung ki tu 
    // nen a.name < b.name se dung khi a co thu tu tu dien nho hon b 
    return a.name < b.name;
}

bool comp3 (Student a, Student b) {
    return a.dgk > b.dgk;
}

bool comp4 (Student a, Student b) {
    return a.dck > b.dck;
}

int main () {
    int n;
    std::cin >> n;
    
    Student students[n];
    for (int i = 0; i < n; i++) {
        // Doc thong tin cua hoc sinh thu i
        std::cin >> students[i].MSHS >> students[i].name;
        std::cin >> students[i].dgk >> students[i].dck;
    }
    
    while (true) {
        std::string query;
        std::cin >> query;
        
        if (query == "#") break;
        
        if (query == "MSHS") {
            std::sort(students, students + n, comp1);
        }
        else if (query == "T") {
            std::sort(students, students + n, comp2);
        }
        else if (query == "DGK") {
            std::sort(students, students + n, comp3);
        }
        else if (query == "DCK") {
            std::sort(students, students + n, comp4);
        }
        else {
            // query == "P"
            for (int i = 0; i < n; i++) {
                std::cout << students[i].MSHS << ' ' << students[i].name << ' ';
                std::cout << students[i].dgk << ' ' << students[i].dck << '\n';
            }
            std::cout << "-\n";
        }
    }
}
```

# Bài tập

---

Làm 3 bài cuối (Salary, Treasure, Good Array). Nhớ báo lại cho anh tình hình vào tối t3 để chuẩn bị buổi sau