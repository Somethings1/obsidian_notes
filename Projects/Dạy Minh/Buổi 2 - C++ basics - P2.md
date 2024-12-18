# Mảng nhiều chiều

---

- Mảng 1 chiều: dãy các biến đặt cạnh nhau

|   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|||||h|||||||||||

- Mảng 2 chiều: a[9][9]
    - Bảng:

|   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|
||0|1|2|3|4|5|6|7|8|
|0|a00|a01||||||||
|1|a10|c||||||||
|2|a20|||0||||||
|3||||||||||
|4||0|||0|||||
|5||||||||||
|6|||||||d|||
|7|||||0|||||
|8||||||||||

- Chiều thứ nhất sẽ lớn hơn. Mỗi phần tử ở chiều đầu tiên sẽ chứa nhiều phần tử ở chiều thứ 2
- VD: int a[9][8] tức là mảng có 9 phần tử, mỗi phần tử là 1 mảng có 8 phần tử, mỗi phần tử là một số nguyên. Được lưu trong máy tính là: a00, a01, a02, a03, a04, a05, a06, a07, a10, a11…
- Mảng n chiều:
    - Chiều thứ i sẽ chứa nhiều phần tử ở chiều thứ i + 1. Chiều cuối cùng chứa 1 phần tử, chính là biến
    - VD: int a[n][m][k][d] là mảng có n phần tử, mỗi phần tử là 1 mảng có m phần tử, trong đó mỗi phần tử là một mảng có k phần tử, trong đó mỗi phần tử là một mảng có d phần tử, trong đó mỗi phần tử là một số nguyên
    - Được lưu trong máy tính là a[0][0][0][0], a[0][0][0][1],.., a[0][0][0][d-1], a[0][0][1][0],…, a[0][0][k-1][d-1], a[0][1][0][0]

# Kiểu struct

---

Struct là một kiểu dữ liệu do người dùng tự định nghĩa, bao gồm nhiều biến khác nhau ở trong nó, để biểu diễn các thực thể phức tạp.

  

VD: yêu cầu viết chương trình nhập vào thông tin của một lớp và in ra thông tin đó dưới dạng bảng. Thông tin là một danh sách các học sinh, bao gồm tên, địa chỉ, điểm tổng kết, rank…

```C++
int n; // số học sinh
std::cin >> n;

std::string name[n];
std::string address[n];
double score[n];
int rank[n]
...
```

Nếu không có struct, ta phải lưu thông tin như trên. Bây giờ, người dùng yêu cầu không chỉ in ra thông tin, mà cần có chức năng sắp xếp danh sách theo tên, điểm và rank. Với mỗi thuộc tính, cần có 1 hàm sắp xếp riêng. Trong mỗi hàm sắp xếp, ở thao tác đổi chỗ các phần tử, phải đổi chỗ TOÀN BỘ các thuộc tính (để đồng bộ). Bây giờ, nếu có thêm 1 thuộc tính, chẳng hạn số lần đi muộn trong năm, ta phải sửa tất cả hàm sắp xếp

⇒ rất dễ lẫn lộn, và code trở thành một mớ hỗn độn

⇒ Giải pháp:

```C++
struct Student {
	std::string name;
	std::string address;
	double score;
	int rank;
};

int main () {
	int n;
	std::cin >> n;
	
	Student students[n];
}
```

Lúc này, ở các hàm sắp xếp, ta chỉ cần đổi chỗ 2 đối tượng Student, nên khi thêm thuộc tính cần quan tâm, ta cũng không cần sửa hàm sắp xếp nào cả, mà các hàm sắp xếp cũng ngắn hơn rất nhiều

# Con trỏ

---

Con trỏ là một biến để lưu địa chỉ (trong ram).

- Cú pháp khai báo con trỏ: <kiểu dữ liệu> *<tên biến>;

```C++
int a;
int *p = &a;
std::cout << p << ' ' << *p;
```

Trong ví dụ trên, &a là phép lấy địa chỉ của a trong RAM. *p đầu tiên là để khai báo 1 con trỏ. *p thứ 2 ở phần cout là để lấy giá trị của biến mà con trỏ đang trỏ tới

Kiểu dữ liệu khi khao báo con trỏ để phục vụ cho việc cộng trừ và truy cập giá trị của con trỏ cho chính xác.

- Chúng ta biết rằng mỗi loại dữ liệu lại có kích thước khác nhau, ví dụ int là 4 bytes, long long là 8 bytes.
- Ở trong RAM, bộ nhớ được chia thành các ô 1 byte nằm liên tiếp nhau.
- Như vậy biến kiểu int sẽ gồm 4 ô liên tiếp, biến long long gồm 8 ô liên tiếp
- Nhưng con trỏ chỉ lưu địa chỉ của ô đầu tiên
- ⇒ cần xác định kiểu để biết biến hiện tại kéo dài đến đâu, và thuộc kiểu gì
- VD nếu ta có 1 biến int, nhưng lại dùng 1 con trỏ long long trỏ vào. Thì bản chất, con trỏ đó đang mang “thêm” 1 biến int nữa

|   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|
|1|2|3|4|5|6|7|8|

Ví dụ một biến int được đặt ở địa chỉ 1-4. Nếu dùng con trỏ int* để trỏ thì con trỏ int* sẽ mang giá trị là 1, chương trình sẽ tự biết độ dài là 4. Tuy nhiên, nếu con trỏ là kiểu long long* thì cũng sẽ mang giá trị là 1, nhưng độ dài sẽ là 8. Khi dùng phép * để gọi giá trị của biến mà con trỏ đang trỏ tới, nó sẽ lấy thừa so với cái mà chúng ta muốn.

# Giới thiệu về danh sách liên kết đơn

---

> [!important] Nhắc lại về độ phức tạp: làm tròn của số phép tính để thực hiện 1 thuật toán nào đó. Làm tròn tức là
> 
> 1. bỏ hết hằng số (chỉ quan tâm biến số thôi), và
> 2. chỉ giữ lại bậc cao nhất của mỗi biến
> 3. Nếu sau khi làm tròn mà không còn gì (số phép tính là hằng số) thì độ phức tạp là O(1)
> 
>   
> 
> VD đọc 1 mảng có n phần tử thì cần n thao tác đọc => đpt là O(n)
> 
> VD liệt kê tất cả các cặp phần tử của mảng có n phần tử:
> 
> 1. Tính số phép tính: với phần tử thứ i thì bắt cặp nó với i - 1 phần tử đứng trước nó.
>     - Ptu thứ 1 k bắt cặp được với ptu nào.
>     - Ptu thứ 2 bắt cặp với 1 ptu.
>     - Ptu thứ 3 thì bắt cặp được 2 ptu
>     - …
>     - ptu thứ n bắt cặp với n - 1 ptu
>     - ⇒ số cặp ptu = 1 + 2 + 3 +...+n - 1 = n(n - 1)/2 = n^2/2 - n/2.
> 2. Bỏ hết hằng số -> n^2 - n
> 3. Giữ lại bậc cao nhất: n^2
> 
> => đpt O(n^2)

## Vấn đề với mảng

- Khi xoá 1 phần tử trong mảng thì độ phức tạp là O(n) và bị trống phần tử ở cuối ⇒ dư thừa thông tin
- Chèn 1 phần tử trong mảng cũng có đpt là O(n) và sẽ bị mất phần tử ở cuối ⇒ mất thông tin
- Kích thước của mảng là cố định
- Mảng bắt buộc phải kề nhau trong bộ nhớ, mà bộ nhớ lại rời rạc ⇒ có thể số ô trống là đủ, nhưng không khai báo được mảng, vì các ô trống đó không liên tiếp nhau

## Danh sách liên kết đơn

Ý tưởng: mỗi phần tử sẽ lưu giá trị và con trỏ để trỏ đến phần tử tiếp theo

[![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcB9ynCzRoU5QFMio2TzUkaD-qGuhgdOXe-AjM1FtqLgzI7YKrpZP7wpQzkUN9i0vboriX18MBB5qXiPPKmHFmi_CVcRcDOAHfLH1quikgsCCzf3VRdrj5jizb6LqtATf7MPYdjcrd6cY74m1DuY-hTxrCP?key=VFdA1F5zBYT64WRojJqFGA)](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcB9ynCzRoU5QFMio2TzUkaD-qGuhgdOXe-AjM1FtqLgzI7YKrpZP7wpQzkUN9i0vboriX18MBB5qXiPPKmHFmi_CVcRcDOAHfLH1quikgsCCzf3VRdrj5jizb6LqtATf7MPYdjcrd6cY74m1DuY-hTxrCP?key=VFdA1F5zBYT64WRojJqFGA)

Như vậy, linked list sẽ giải quyết được toàn bộ điểm yếu của mảng

- Khi xoá 1 phần tử trong linked list thì độ phức tạp là O(1): VD muốn xoá C thì chỉnh lại con trỏ của B để nó trỏ tới D
- Khi thêm 1 ptu thì đpt là O(1): VD muốn thêm F vào sau B thì chỉnh con trỏ của B để trỏ đến F và cho con trỏ của F trỏ vào C
- Kích thước của linked list không cố định: chèn hay xoá thì cũng không gây mất hoặc thừa thông tin
- Vì linked list nối với nhau bằng con trỏ nên các phần tử không bắt buộc phải kề nhau trong bộ nhớ

  

Tuy nhiên, linked list lại có các nhược điểm:

- Truy cập phần tử thứ i thì cần đi qua i - 1 phần tử ở trước (trong khi với mảng thì không cần)
- Tốn bộ nhớ gấp đôi mảng

⇒ linked list sẽ phù hợp với những bài toán cần nhiều thao tác chỉnh sửa (thêm, xoá) ở trong danh sách nhưng sẽ không phù hợp với những bài toán cần truy cập thường xuyên vào phần tử theo chỉ số của nó