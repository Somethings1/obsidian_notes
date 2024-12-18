# Software design principle
---

---

## Features of good design

1. Code reuse
2. Extensibility

## Design principles

1. Encapsulate what varies: phát hiện những phần khác nhau trong code và cô lập chúng khỏi những gì được lặp lại
2. Program to an Interface, not an Implementation: khi class A phụ thuộc vào class B, xem xem liệu A cần những method nào từ B, và tách riêng ra thành một interface, sau đó để A phụ thuộc vào interface chứ không phải phụ thuộc vào B.
3. Composition over inheritance: thay vì mối quan hệ “is a” thì chuyển thành mối quan hệ “has a ”

## SOLID principles

- Single Responsibility: mỗi class chỉ chịu 1 trách nhiệm duy nhất, và trách nhiệm đó cũng chỉ được chịu trách nhiệm bởi 1 class duy nhất
- Open/closed: các class nên “mở” cho các tiện ích mở rộng (extensions) nhưng nên “đóng” với mọi sự chỉnh sửa (sau khi đã final)
- Liskov Substitution: khi mở rộng một class, đảm bảo rằng có thể truyền object của class con thay cho object của class cha mà không làm hỏng code của client - tóm lại là hành vi của class con phải tương hợp với class cha
    - Kiểu của tham số trong phương thức của class con phải rộng (trừu tượng) bằng hoặc hơn tham số tương ứng trong class cha
    - Kiểu trả về của method trong class con phải bằng hoặc hẹp hơn kiểu trả về của method trong class cha
    - Một method trong class con không nên throw exceptions mà method trong class cha không throws
    - Class con không nên tạo những điều kiện đầu vào chặt hơn (VD method cha nhận 1 số nguyên, method con đi kiểm tra số nguyên và throw exception nếu số đó < 0, khi đó là điều kiện đầu vào chặt hơn)
    - Class con không nên tạo những điều kiện đầu ra yếu hơn (VD method cha đọc file xong đóng vào luôn, method con đ thèm đóng, thì client sẽ k biết để mà đóng file vào)
    - Khi mở rộng class thì chỉ nên thêm, không nên bớt
    - Class con không nên thay đổi những cái private của class cha (đ hiểu sao)
- Interface segregation: client không nên bị ép triển khai những method mà nó k dùng, tức là interface thì làm chặt hết mức có thể để không bị thừa
- Dependency inversion: class bậc cao không nên phụ thuộc vào class bậc thấp (tức là nên code cái trừu tượng hơn trước rồi code cái cụ thể sau)

# Creational design patterns

---

> [!important] **Creational patterns** cung cấp cơ chế giúp tăng sự linh hoạt và khả năng tái sử dụng code khi tạo các objects

## Factory method

Cung cấp một interface để tạo các objects trong một lớp cha, nhưng cho phép các lớp con thay đổi kiểu của object được tạo.

### ⚠️ Problem

Tưởng tượng app giống Grab. Thời gian đầu app chỉ có tính năng đặt xe máy. Tất cả tính năng khác, từ book xe chở người, book ship hàng, book gửi quà đều xây dựng dựa trên cái class XeMay. Giờ nếu muốn thêm oto vào thì phải sửa hết code

```C++
class Motorbike {
	...
}
class Logistics {
	public void planDelivery () {
		Motorbike bike = new Motorbike();
		...
	}
}
```

### ✅ Solution

Thay đổi tất cả các construction call (qua từ khoá new) bằng việc gọi 1 method _factory._ Bây giờ, ta có thể override cái factory method này ở các subclass và thay đổi cái thứ được tạo bên trong method thành cái khác cho phù hợp. Điều này đòi hỏi tạo thêm kha khá class khác. Được cái là mình gần như không cần chỉnh sửa class ban đầu (chỉ sửa mỗi chỗ khởi tạo)

```Java
interface Transport {
	void deliver();
	void sendGift();
}

class Motorbike implements Transport {
	...
}

class Car implements Transport {
	...
}

abstract class Logistics {
	Transport createTransport ();

	void planDelivery () {
		Transport t = createTransport();
		...
	}
}

class RoadLogistics extends Logistics {
	Transport createTransport () {
		return new Motorbike();
	}
}
```

### ⚡ Applicability

- Khi không biết trước kiểu và phụ thuộc của những đối tượng mà code sẽ làm việc cùng (giống như trên, mình không biết sau này logistic sẽ phụ thuộc vào mỗi xe máy hay cả oto)
- Khi muốn cung cấp một cách để mở rộng các thành phần của một thư viện hay framework
- Khi muốn tiết kiệm tài nguyên hệ thống bằng cách dùng lại các đối tượng thay vì tạo mới

### 🖼️ UML

![[/image 7.png|image 7.png]]

## Abstract factory

### ⚠️ Problem

Tưởng tượng bạn đang làm một ứng dụng mô phỏng cửa hàng nội thất. Code của bạn cần mô phỏng các đối tượng có trong cửa hàng. Cửa hàng có nhiều dòng sản phẩm: nghệ thuật, hiện đại, cổ điển… Mỗi dòng sản phẩm lại có các sản phẩm khác nhau: bàn, ghế, bàn trà, đèn…

Bây giờ, nếu mỗi đối tượng, bạn tạo ra 1 class, thì có khoảng 1 tỷ cái class khác nhau, ví dụ như class Bàn Hiện Đại, Ghế Cổ Điển, Bàn Trà Nghệ Thuật…

### ✅ Solution

Giải pháp ở đây là, tạo 1 interface Ghế, sau đó tất cả Ghế Cổ Điển, Ghế Nghệ Thuật, Ghế Hiện Đại sẽ implements Ghế. Tương tự với Bàn, Bàn Trà, Đèn…

Sau đó, tạo Abstract Factory - một interface chứa các method để tạo ra các sản phẩm (ví dụ createGhe, createBanTra, createBan). Các method này trả về abstract type (interface) của cái đống bên trên.

Tiếp theo, với mỗi dòng sản phẩm, tạo 1 factory class implements cái Abstract factory bên trên. Các factory method ở mỗi class sẽ trả về kiểu cụ thể.

Bây giờ, client code làm việc với 2 cái factory qua abstract object. Client chỉ cần biết nó là cái ghế, không cần quan tâm cái ghế thuộc kiểu gì, miễn là nó có thể ngồi lên chẳng hạn

### ⚡ Applicability

- Khi cần làm việc với nhiều dòng sản phẩm thuộc nhiều kiểu khác nhau, nhưng không muốn phải phụ thuộc vào các class cụ thể của mỗi sản phẩm.
- Khi dùng cái Factory Method nhưng nó nhiều quá

### 🖼️ UML

![[/image 1 4.png|image 1 4.png]]

## Builder

### ⚠️ Problem

Tưởng tượng một object phức tạp với rất nhiều thành phần mà cần khởi tạo.

Ví dụ như một cái nhà, ngoài những thứ cơ bản như bốn bức tường ngoài, một cửa chính, vài cửa sổ, thì còn nhiều tuỳ chọn khác ví dụ như cây quanh nhà, bể bơi, tượng trang trí…

Có 2 cách thường thấy:

1. Tạo cho mỗi 1 cách tuỳ chọn 1 subclass ⇒ quá nhiều lựa chọn, quá nhiều subclass
2. 1 class duy nhất, với 1 đống parameter cần khởi tạo ⇒ phần lớn parameter là thừa thãi, ví dụ như có bể bơi hay không, bể bơi dài bao nhiêu, rộng bao nhiêu, sâu bao nhiêu, lát đá gì… Nếu không có bể bơi, thì đống parameters đằng sau là thừa thãi

### ✅ Solution

Giải pháp ở đây là, tách các phần khởi tạo ra theo từng bước thành các class riêng, gọi là builders. Sau đó, tạo thêm 1 class để gọi các method của bọn builders, gọi là director. Tức là, client code đưa yêu cầu cho director, director gọi đến bọn builders, còn bọn builders mới là bọn thực sự xây dựng mọi thứ.

### ⚡ Applicability

- Khi phần constructor khủng quá
- Khi muốn có khả năng tạo nhiều kiểu khác nhau của cùng 1 sản phẩm (nhà băng, nhà đá…)

### 🖼️ UML

![[/image 2 4.png|image 2 4.png]]

## Prototype

### ⚠️ Problem

Tưởng tượng bạn có một object. Giờ, bạn muốn copy nó. Làm thế nào?

Thường thì, ta sẽ tạo 1 object mới thuộc cùng 1 class, sau đó copy tất cả các thuộc tính. Tuy nhiên, việc copy này thường sẽ phức tạp và có thể không khả thi (do có các invisible field)

### ✅ Solution

Giải pháp ở đây là giao việc “copy” cho chính cái object đó. Tạo 1 interface cho tất cả các object cần tạo ra các bản copy, cho nó một method clone.

Một object cho phép clone được gọi là một prototype.

### ⚡ Applicability

- Khi cần copy
- Khi muốn giảm số lượng các class con mà chúng chỉ khác nhau mỗi cách khởi .

### 🖼️ UML

![[/image 1 3.png|image 1 3.png]]

## Singleton

### ⚠️ Problem

Singleton giải quyết 2 vấn đề cùng 1 lúc:

1. Đảm bảo một class chỉ có 1 object được tạo ra
2. Tạo 1 điểm truy cập toàn cục cho object đó

Ví dụ, với 1 file hay 1 database, không nên tạo ra 2 object giống nhau làm gì cả, mất gấp đôi RAM

### ✅ Solution

- Làm cho hàm khởi tạo mặc định là private
- Tạo 1 method khởi tạo static. Method này sẽ check, nếu đã từng có object được tạo ra rồi thì trả về nó, nếu không thì tạo ra.

### ⚡ Applicability

- Khi có 1 class chỉ nên có 1 instance duy nhất
- Khi cần kiểm soát chặt chẽ các biến toàn cục.

### 🖼️ UML

![[/image 6.png|image 6.png]]

# Structural design patterns

---

> [!important] **Structural patterns** cung cấp phương thức kết hợp các object và class thành các cấu trúc lớn hơn, đồng thời giữ các cấu trúc đó linh hoạt và hiệu quả

## Adapter

### ⚠️ Problem

Tưởng tượng bạn đang tạo ra một ứng dụng chứng khoán. Ứng dụng này tải data từ vài nguồn trên mạng ở định dạng XML, sau đó sẽ hiển thị các dữ liệu này trên biểu đồ.

Bây giờ, bạn muốn mở rộng thêm vài chức năng phân tích. Có sẵn 1 thư viện to đùng cho việc phân tích. Nhưng thư viện này chỉ làm việc với định dạng JSON.

### ✅ Solution

Hãy tạo một adapter. Đây là một object đặc biệt mà có thể convert cái interface của một object sao cho object khác có thể hiểu được

### ⚡ Applicability

- Khi muốn sử dụng các class có sẵn, nhưng interface của nó không hợp với phần còn lại của code

### 🖼️ UML

![[/image 3 4.png|image 3 4.png]]

## Bridge

### ⚠️ Problem

Tưởng tượng có 2 class Hình Vuông và Hình Tròn. Mỗi hình lại có 2 màu. Bây giờ, vì hình khối và màu sắc đều quan trọng như nhau, nên nếu muốn tạo ra các class con thì phải tạo ra 4 class.

Nếu thêm 1 hình, sẽ phải thêm 2 class. Sau đó thêm 1 màu thì thêm 3 class. Quá cồng kềnh

### ✅ Solution

Tất cả điều này xảy ra vì chúng ta đang cố mở rộng chương trình theo 2 hướng khác nhau: hình dáng và màu sắc.

The Bridge Pattern giải quyết vấn đề này bằng cách composition over inheritance. Tức là, tách 1 hướng ra thành một lớp khác hẳn, sao cho class ban đầu sẽ có 1 object chứa class đó, chứ không phải là chứa tất cả thuộc tính của nó.

Tức là, có 1 class Hình Khối và 1 class Màu Sắc. Trong Hình Khối sẽ có 1 object Màu Sắc. Sự liên kết này sẽ là cầu nối giữa 2 class.

  

Tư tưởng lớn của Bridge pattern là Abstraction & Implementation

- Abstraction: lớp kiểm soát cao hơn, trừu tượng hơn. Lớp này không nên làm cái gì trực tiếp, mà giao cho lớp Implementation làm. Trong thực tế, lớp này thường là GUI. Ví dụ có 1 class cho admin panel, 1 class cho normal user…
- Implementation: Lớp thực sự làm việc. Trong thực tế, lớp này thường là API của hệ thống. Ví dụ 1 class để render cái button, 1 class render cái window…

### ⚡ Applicability

- Khi muốn chia và tổ chức một class đơn lẻ mà có nhiều chức năng khác nhau (ví dụ như class làm việc với nhiều database)
- Khi muốn mở rộng class theo nhiều chiều không phụ thuộc nhau.
- Khi muốn có khả năng cập nhật cách triển khai trong thời gian thực

### 🖼️ UML

![[/image 4 3.png|image 4 3.png]]

## Composite

### ⚠️ Problem

Tưởng tượng có 2 loại objects: Hộp và Sản Phẩm. Một cái hộp có thể chứa vài sản phẩm và vài cái hộp nhỏ hơn. Như vậy mình có 1 cái cây. Vậy ví dụ mình muốn tính giá trị của một cái hộp thì tính như nào?

### ✅ Solution

Giải pháp là, làm việc với cả Hộp và Sản Phẩm qua 1 interface duy nhất có bao gồm phương thức tính giá trị. Phương thức này được triển khai như sau:

- Ở Sản Phẩm, phương thức này trả về giá trị của chính nó
- Ở Hộp, phương thức này lặp qua tất cả những thứ ở trong nó, gọi đến phương thức tính giá trị của mỗi thằng, và tính tổng đống đấy.

Như vậy, ta không cần quan tâm trong hộp chứa loại gì, một cái hộp khác hay là sản phẩm

### ⚡ Applicability

- Khi cần triển khai một cấu trúc cây gồm các objects
- Khi muốn client code coi object đơn giản và phức tạp như nhau.

### 🖼️ UML

![[/image 5 2.png|image 5 2.png]]

## Decorator

### ⚠️ Problem

Tưởng tượng bạn đang làm việc với một thư viện thông báo cho phép các phần mềm khác thông báo cho người dùng của họ về những sự kiện quan trọng. Thư viện cho phép tạo 1 thông báo và gửi đến một danh sách email.

```Java
class Notifier {
	public void send(message) {
		sendEmail(message);
	}
}
```

  

Sau một thời gian sử dụng, bạn để ý rằng người dùng muốn nhiều hơn là chỉ gửi thông báo qua email. Họ muốn gửi cả tin nhắn SMS, Tweeter và qua Facebook nữa.

Ban đầu, tất cả gói gọn trong class Notifier. Khi muốn mở rộng, bạn triển khai SMSNotifier, TweeterNotifier và FacebookNotifier, tất cả kế thừa Notifier, quá đẹp.

```Java
class SMSNotifier extends Notifier {
	public void send(message) {
		super.send(message);
		sendSMS(message);
	}
}

class TweeterNotifier extends Notifier {
	public void send(message) {
		super.send(message);
		sendTweeterNotification(message);
	}
}

class FacebookNotifier extends Notifier {
	public void send(message) {
		super.send(message);
		sendFacebookNotification(message);
	}
}
```

Cũng ổn, client nếu muốn thông báo qua các kênh khác thì chỉ cần tạo thêm notifier, tức là thêm code thôi. Phần code này không mâu thuẫn gì với code lúc trước, tức là đều có method send cả.

Bỗng một ngày đẹp zời, một người muốn gửi thông báo qua cả SMS và Facebook, ví dụ cho một sự kiện quan trọng hơn chẳng hạn. Rồi sau đó một người khác nghĩ đến trường hợp, ví dụ như cháy nhà, bạn sẽ muốn được nhận thông báo trên tất cả các kênh.

Thế là bạn tạo ra SMSFacebookNotifier, SMSGoogleNotifier, GoogleFacebookNotifier, SMSGoogleFacebookNotifier…

Quá khốn nạn.

### ✅ Solution

Giải pháp ở đây là sử dụng Decorator pattern, hay có một cái tên khác là Wrapper, tức là “gói” các thứ lại với nhau. Hãy để chức năng thông báo qua email vào lớp cơ sở Notifier, và chuyển tất cả các chức năng khác thành decorators. Client phải gói 1 object thông báo cơ bản thành một danh sách decorators phù hợp. Cuối cùng sẽ cho ra đời 1 object như là stack.

Decorator cuối cùng trong stack là object mà client sẽ làm việc cùng. Bởi vì tất cả decorators đều có chung 1 interface giống như lớp cơ sở, các phần còn lại của client sẽ không cần quan tâm nó đang làm việc với một object thông báo thuần tuý hay một object đã decorated.

```Java
class Notifier {
	public void send(message) {
		sendEmail(mesage);
	}
}

class BaseDecorator extends Notifier {
	private Notifier wrapee;
	public BaseDecorator (Notifier notifier) {}
	public void send(message) {
		wrapee.send(message);
	}
}

class SMSDecorator extends BaseDecorator {
	public void send(message) {
		super.send(message);
		sendSMS(message);
	}
}

class FacebookDecorator extends BaseDecorator {
	public void send(message) {
		super.send(message);
		sendFacebookNotification(message);
	}
}
...
```

Và trong client, code sẽ như sau:

```Java
stack = new Notifier();
if (facebookEnabled) 
	stack = new FacebookDecorator(stack);
if (SMSEnabled) 
	stack = new SMSDecorator(stack);

app.setNotifier(stack)

...

notifier.send("Alert");
```

Như vậy, mọi lời gọi notifier.send() ở client sẽ giữ nguyên, chỉ thay đổi mỗi phần khởi tạo trước khi setNotifier. Mọi thứ đều được đóng gói lại theo lớp

### ⚡ Applicability

- Khi muốn thêm các chức năng bổ sung vào các objects mà không làm hỏng các phần code đang sử dụng các objects đó
- Khi cảm thây rất kỳ cục, hoặc thậm chí không thể mở rộng chức năng cho object bằng cách kế thừa thông thường.

### 🖼️ UML

![[/image 6 2.png|image 6 2.png]]

## Facade

### ⚠️ Problem

Tưởng tượng bạn đang làm việc với một đống objects đến từ một thư viện hoặc một framework cực kỳ phức tạp. Thường thì bạn sẽ khởi tạo đống objects đó, kiểm soát đống tham số, gọi các phương thức theo đúng thứ tự…

Cứ như thế, logic của code của bạn sẽ trở nên cực kỳ phức tạp và gắn chặt với việc triển khai chi tiết cái thư biện ngoài kia, tức là cực kì khó hiểu và khó bảo trì.

### ✅ Solution

Giải pháp là, hãy tạo 1 class để cung cấp một giao diện đơn giản hơn cho những gì bạn đang cần ở thư viện ngoài kia. Một facade có thể cho bạn ít sự tuỳ biến, ít sự linh hoạt hơn so với việc làm một cách trực tiếp. Tuy nhiên, nó sẽ chỉ chứa những logic mà bạn thật sự quan tâm đến.

Ví dụ bạn đang làm một cái app để ngắm ảnh mèo. Bạn cần một thư viện để nén mấy tấm ảnh cho gọn. Thư viện đó bao gồm các hàm để đọc hình ảnh, nén, lưu, render hình ảnh… Tất cả những gì bạn quan tâm đến là việc nén ảnh. Vậy thì hãy tạo 1 class chuyên để làm việc với thư viện đó, chỉ bao gồm 1 method duy nhất là nén ảnh (trong method sẽ làm việc với thư viện kia). Vậy thì trong code chính của bạn sẽ chỉ có mỗi class bạn vừa tạo ra (1 facade) thay vì lời gọi đến một đống method từ thư viện kia.

### ⚡ Applicability

- Khi cần một giao diện tuy hạn chế nhưng đơn giản và trực quan để làm việc với một hệ thống phức tạp
- Khi muốn cấu trúc lại một hệ thống thành các lớp

### 🖼️ UML

![[/image 7 2.png|image 7 2.png]]

## Flyweight

### ⚠️ Problem

Tưởng tượng bạn đang tạo 1 con gêm. Người chơi sẽ di chuyển quanh map và bắn nhau. Mỗi viên đạn sẽ được mô phỏng chính xác bằng 1 object, tạo cho con gêm của bạn một trải nghiệm chân thực.

Quá tuyệt vời, mỗi tội không chơi được. Mỗi viên đạn của bạn trông như sau

```Java
class Particle {
	public Coordinate coor; // 8B
	public Vector vector; // 16B
	public Speed speed; // 4B
	public Color color; // 4B
	public Bitmap sprite; // 20KB	
}
```

Mỗi viên đạn 21KB ⇒ 1 triệu viên đạn là 21GB ⇒ cháy RAM

### ✅ Solution

Nhìn lại Particle class, phần color và sprite có vẻ sêm sêm ở mọi viên đạn, nhưng lại tốn quá nhiều diện tích. Hơn nữa, những phần này lại có tính cố định, tức là một thực thể khi sinh ra cho đến khi mất đi thì 2 cái này không đổi, còn những cái khác đều thay đổi.

Flyweight bảo là, đừng lưu những cái không thay đổi ở bên trong thực thể. Thay vào đó, chỉ cần truyền những cái đó vào những phương thức cần nó. Chỉ lưu những cái gì thay đổi liên tục thôi. VD như trong cái trên, mình sẽ lưu, ví dụ như

```Java
class Particle {
	private Color color;
	private Bitmap sprite;
	
	public void move (Coordinate coor, Vector vector, Speed speed) {}
	public void draw (Coordinate coor, Canvas canvas) {}
}

class MovingParticle {
	private Particle particle;
	private Coordinate coor;
	private Vector vector;
	private Speed speed;
	
	public void move () {}
	public void draw (Canvas canvas) {}
}

class Game {
	public MovingParticle mps[];
	private Particle particles[];
	
	public void addParticle (Coordinate coor, Vector vector, 
	Speed speed, Color color, Bitmap sprite) {}
}

class Unit {
	public void fireAt (Unit target) {
		Game.addParticle(...)
	}
}
```

Đấy, thế là tính ra con gêm còn có 32MB RAM

### ⚡ Applicability

- Khi nhìn thấy 1 đống lặp lại và RAM thì không đủ chỗ chứa

### 🖼️ UML

![[image 8.png]]

## Proxy

### ⚠️ Problem

Tưởng tượng bạn có một object to đùng, chiếm rất nhiều tài nguyên, mà bạn lại cần dùng nó nhiều lần, nhưng không thường xuyên. VD như một database. Nếu cứ tạo ra rồi để đấy thì sẽ rất tốn tài nguyên, mà mỗi lần cần dùng lại phải tạo ra rồi khởi tạo các kiểu thì sẽ lặp lại code ở nhiều nơi.

Nếu mà ngon, thì có thể nhét luôn đống khởi tạo vào class gốc. Nhưng mà như database thì làm gì có chuyện thay đổi được code gốc. Cho nên proxy ra đời.

### ✅ Solution

Proxy pattern bảo là tạo ra một proxy class có cùng interface với object kia, sau đó cập nhật app của bạn để nó truyền cái proxy object đó cho client thay vì object ban đầu. Khi nhận 1 request, proxy sẽ tạo 1 object kia thật.

Thế thì có tác dụng vẹo gì? Có, nếu bạn muốn làm một cái gì đó trước hoặc sau khi nhận request.

### ⚡ Applicability

- “Khởi tạo lười” (lazy initialization). Tức là khi có một object to đùng mà chiến nhiều tài nguyên, kể cả khi không dùng đến và bạn chỉ dùng nó vài lần.
- Kiểm soát truy cập, phân quyền truy cập
- Xử lý local cho một dịch vụ remote. Proxy truyền request của client qua mạng, xử lý tất cả những cái loằng ngoằng trong quá trình làm việc với mạng.
- Tạo log cho request
- Tạo cache cho request
- Tiết kiệm bộ nhớ

### 🖼️ UML

![[image 9.png]]

# Behavioral design patterns

---

> [!important] **Behavioral patterns** cung cấp cơ chế giao tiếp và phân công trách nhiệm hiệu quả giữa các object

## Chain of Responsibility

### ⚠️ Problem

Tưởng tượng bạn đang làm 1 web bán hàng. Bạn muốn giới hạn chỉ người đăng nhập mới được mua hàng. Thêm nữa, chỉ người có quyền admin mới được truy cập vào mọi đơn hàng.  
Quá trình kiểm tra quyền rất tốn thời gian: mã hoá thông tin, check IP trùng lặp, check cookie, check trong database…  

Đoạn code để check trông như một mớ hỗn độn. Thi thoảng, bạn còn phải dùng 1 vài cái check để bảo vệ các thành phần khác trong hệ thống. Vài cái thôi, không phải tất cả. Nhưng vấn đề là nó dính vào nhau rồi. Hệ thống trở nên rất khó kiểm soát.

### ✅ Solution

Chain of responsibility bảo là, phải tách nó ra, mỗi cái check làm 1 class khác nhau, gọi là handler, chỉ gồm 1 method duy nhất là check. Mỗi request cùng với data của nó sẽ được truyền cho method đó dứoi dạng tham số.

Sau đó, nối đống handler đó vào thành 1 chuỗi. Mỗi handler lại chứa một tham chiếu đến handler tiếp theo trong chuỗi. Ngoài việc xử lý request, handler còn gửi request đó cho handler tiếp theo trong chuỗi thực hiện.

Cái hay nhất là, một handler hoàn toàn có thể quyết định không gửi request đó cho handler tiếp theo nữa.

### ⚡ Applicability

- Khi cần xử lý nhiều loại request theo vài cách khác nhau, nhưng kiểu và chuỗi request là chưa được biết trước

### 🖼️ UML

![[image 10.png]]

## Command

### ⚠️ Problem

Tưởng tượng bạn đang code 1 con app soạn thảo văn bản. Bây giờ bạn phải làm 1 cái toolbar với 1 đống nút. Bạn tạo 1 class Button, dùng chung cho đống ở toolbar và các button chức năng khác. Tuy đống button này trông giống nhau, nhưng mỗi cái lại có chức năng khác nhau. Vậy phải làm sao?

Cách đơn giản nhất là tạo 1 class con cho mỗi button. Tuy nhiên, như thế sẽ đẻ ra quá nhiều class, nếu thay đổi Base thì có thể ăn cứt luôn. Hơn nữa, sẽ có vài chức năng, ví dụ như copy văn bản chẳng hạn, có nhiều cách để làm điều đó, ví dụ như Ctrl+C, hay bấm nút copy, hay chuột phải chọn copy. Bây giờ, đống code cho việc copy sẽ bị lặp lại ở handler của bàn phím, context menu và cái button kia.

### ✅ Solution

Thường thì ae làm app sẽ chia ra GUI và backend. Command pattern bảo là GUI đừng gửi request trực tiếp cho backend. Thay vào đó, nên đem tất cả mọi thông tin liên quan đến request đó, cho vào 1 _command_ class với một method duy nhất là thực hiện request. GUI chỉ trigger cái command class, còn command class sẽ trigger phần backend. Tức là thay vì như này

![[image 11.png]]

Thì mình sẽ làm như này

![[image 12.png]]

### ⚡ Applicability

- Khi muốn tham số hoá thực thể với các phép xử lý
- Khi muốn để các phép xử lý xếp hàng, định thứ tự xử lý, hoặc xử lý chúng từ xa
- Khi muốn cài đặt các phép xử lý có thể đảo ngược

### 🖼️ UML

![[image 13.png]]

## Iterator

### ⚠️ Problem

Các collection có rất nhiều cách để biểu diễn dữ liệu bên trong nó, có thể là dạng mảng, dạng bảng, dạng cây… Nhưng dù thế nào, nó cũng phải cung cấp một vài cách để code chỗ khác có thể sử dụng nó. Phải có một cách để đi qua tất cả các phần tử mà không lặp lại.

Điều này dễ đối với các collection dạng list, nhưng sẽ phức tạp với collection dạng cây chẳng hạn. Có khi cần BFS, có khi cần DFS, thậm chí là random. Nếu thêm tất cả các thuật toán duyệt vào thì sẽ làm mờ đi tính năng chính của collection là lưu trữ dữ liệu, hơn nữa có thể người dùng chỉ cần 1 cách duyệt thì những cái khác sẽ lãng phí.

### ✅ Solution

Tách thuật toán duyệt sang một class khác, gọi là iterator. Iterator sẽ đóng gói tất cả những chi tiết linh tinh của việc duyệt, ví dụ như vị trí hiện tại hay là còn bao nhiêu phần tử khác. Như vậy, một vài iterator có thể duyệt cùng một collection cùng lúc.

Tất cả iterator nên có chung interface, để client có thể dễ sử dụng.

### ⚡ Applicability

- Khi collection có một cấu trúc phức tạp, nhưng bạn muốn ẩn sự phức tạp đó đi
- Để hạn chế sự lặp lại của code duyệt

### 🖼️ UML

![[image 14.png]]

## Mediator

### ⚠️ Problem

Tưởng tượng bạn có một đoạn code để tạo và chỉnh sửa một profile trên app. Các form liên kết với nhau rất nhiều, ví dụ nếu người dùng chọn là “tôi có một con chó” thì sẽ có mấy câu khác hiện ra như màu lông, tên… Hay một cái nút submit mà check rất nhiều giá trị trước khi lưu data.

Nếu cài tất cả những logic này vào các thành phần thì các thành phần sẽ không thể được sử dụng cho các mục đích khác trong app này.

### ✅ Solution

Có lẽ, bạn không nên để các thành phần giao tiếp trực tiếp với nhau, mà nên để chúng giao tiếp thông qua một object mediator. Trong ví dụ trên, tạo 1 phần tử Dialog để làm trung gian. Ví dụ như nút submit. Trước đây, mỗi khi người dùng bấm submit, nó phải kiểm tra tất cả nội dung của các phần tử khác trong form. Bây giờ, việc duy nhất của nó là thông báo cho Dialog là nó vừa được bấm. Khi nhận thông báo này, Dialog sẽ thực hiện việc kiểm tra nội dung của các phần tử khác.

### ⚡ Applicability

- Khi rất khó để thay đổi 1 class vì nó đã dính chặt với các class khác
- Khi nhận thấy đang có một đống subclass của 1 component duy nhất chỉ để dùng lại một vài hành vi cơ bản trong nhiều ngữ cảnh khác nhau.

### 🖼️ UML

![[image 15.png]]

## Memento

### ⚠️ Problem

Tưởng tượng bạn đang tạo ra một app edit văn bản. App của bạn có chức năng undo. Do đó, trước khi thực hiện bất kỳ thay đổi nào, app sẽ lưu lại MỌI thông tin đang có của mọi phần tử rồi lưu nó lại. Nhưng bạn thực hiện việc lưu thông tin như thế nào? Trong khi có những thành phần private?

Mà cứ giả sử như các thành phần đều public, thì mỗi khi bạn thay đổi một thành phần nào đó, bạn cũng phải thay đổi thành phần thực hiện việc lưu thông tin.

Hơn nữa, khi có một class để thực hiện việc lưu, thì class đó sẽ có 1 đống thuộc tính và gần như không có method. Các thuộc tính sẽ phải public để các thành phần khác có thể truy cập được khi cần thiết. Tức là mọi thông tin đều sẽ lồ lộ ra, và mọi class khác đều phụ thuộc vào class này.

### ✅ Solution

Vậy đừng tạo class riêng nữa. Để việc lưu thông tin cho các class thành phần đi. Các class khác có thể truy cập 1 vài thông tin về sự thay đổi, ví dụ như thời gian và người thực hiện, nhưng không thể truy cập các thông tin khác. Bảo mật tuyệt đối.

Class lưu thông tin được gọi là memento, class cần truy cập thông tin (ví dụ như History, để lưu lại mọi thông tin không quan trọng trong những lần thay đổi) được gọi là caretaker.

### ⚡ Applicability

- Khi muốn tạo ra các bản sao lưu để có thể undo
- Khi mà sự truy cập xung đột với tính đóng gói

### 🖼️ UML

![[image 16.png]]

![[image 17.png]]

![[image 18.png]]

## Observer

### ⚠️ Problem

Tưởng tượng bạn có 2 class: Customer và Store. Customer đăng ký nhận thông tin từ Store, nhưng không phải tất cả. Vậy thì cơ chế nào để Store phát tin một cách hiệu quả?

### ✅ Solution

Store ở đây gọi là publisher, và Customer gọi là subscriber. Hãy cung cấp một cơ chế cho publisher để các subscribers có thể theo dõi hoặc không theo dõi một luồng các sự kiện. Trong một publisher sẽ có một danh sách subscriber có hứng thú và nó sẽ gọi đến một method của subscriber để thông báo về thông tin mới. Do đó nên subscribers phải có chung interface.

Nếu muốn mở rộng ra, ví dụ có nhiều publisher, thì có thể để tất cả các publisher có cùng interface.

### ⚡ Applicability

- Khi mà thay đổi của một object có thể yêu cầu sự thay đổi ở 1 object khác, và các object này là chưa được biết trước hoặc thường xuyên thay đổi
- Khi một vài object cần quan sát các objects khác, nhưng chỉ trong một thời gian hoặc trong các tình huống đặc biệt

### 🖼️ UML

![[image 19.png]]

## State

### ⚠️ Problem

Ý tưởng là, ở mỗi thời điểm, chương trình chỉ có thể ở 1 trong số hữu hạn các trạng thái. Các quy luật chuyển trạng thái, gọi là transitions, cũng là hữu hạn và có thể biết trước.

Tưởng tượng bạn có một class `Document`, class này chỉ có thể ở một trong ba trạng thái: `Draft`, `Moderation` và `Published`. Phương thức `publish` ở mỗi trạng thái sẽ có đôi chút khác nhau:

- Ở trạng thái `Draft`, nó chuyển Document sang Maderation
- Ở trạng thái `Moderation`, nó chuyển Document sang trạng thái public với những người dùng là admin
- Ở trạng thái `Published`, nó không làm gì cả.

Ta có thể cho các trạng thái chỉ đơn giản là một biến string, và dùng switch để phân loại hoạt động trong `publish`, tuy nhiên mọi thứ sẽ phức tạp hơn nhiều nếu số trạng thái, hoặc số hoạt động ứng với mỗi trạng thái tăng lên.

### ✅ Solution

State pattern gợi ý rằng bạn nên tạo class mới cho tất cả các trạng thái có thể của một object và lôi tất cả những hành vi liên quan đến trạng thái vào các class này.

Thay vì tự thực hiện các hành vi, thì object ban đầu, gọi là _context_, sẽ lưu 1 tham chiếu tới 1 trong số các object trạng thái mà mô tả trạng thái của nó hiện tại, và chuyển việc cho object đó làm.

### ⚡ Applicability

- Khi có một object mà hoạt động của nó phụ thuộc vào trạng thái hiện tại, số trạng thái thì lại nhiều, và code thực hiện với mỗi trạng thái thì thay đổi liên tục
- Khi có một class có 1 đống câu lệnh điều kiện để xử lý cho các giá trị khác nhau của các thuộc tính của class
- Khi có nhiều code trùng lặp suốt các trạng thái

### 🖼️ UML

![[image 20.png]]

## Strategy

### ⚠️ Problem

Một ngày nọ, bạn muốn làm một app chỉ đường cho ae đi du lịch. App xoay quanh một cái map. Một chức năng ae rất thích là chỉ đường. Phiên bản đầu tiên của app chỉ có tính năng chỉ đường trên đường ô tô. Sau đấy ae đi bộ ý kiến, bạn thêm chức năng đường đi bộ, rồi đi bằng phương tiện công cộng, rồi đi bằng xe đạp…

Con app trở nên quá kinh khủng.

### ✅ Solution

Strategy pattern bảo bạn nên chia tách 1 class mà làm 1 việc cụ thể theo rất nhiều cách khác nhau thành các class riêng biệt gọi là _strategies_.

Class ban đầu, gọi là _context_, nên có một thuộc tính để lưu 1 trong số các strategies. Context chuyển việc xuống cho strategy mà nó đang lưu thay vì tự làm hết. Context cũng không chịu trách nhiệm cho việc chọn thuật toán đúng cho công việc hiện tại, mà client sẽ chuyển strategy mong muốn cho context. Trong thực tế, context thậm chí không biết gì nhiều về strategy, mà chỉ làm việc với chung qua 1 interface đơn giản.

### ⚡ Applicability

- Khi muốn có vài cách triển khai 1 thuật toán và khả năng chuyển giữa các cách triển khai khác nhau trong thời gian chạy
- Khi có nhiều class giống nhau, chỉ khác mỗi cách nó thực hiện một vài hành vi
- Khi muốn cô lập logic lớn của class với cách triển khai chi tiết kĩ thuật mà có thể không quan trọng trong logic đó.
- Khi class có 1 đống lệnh điều kiện mà chuyển qua chuyển lại giữa các cách khác nhau của một thuật toán giống nhau.

### 🖼️ UML

![[image 21.png]]

## Template method

### ⚠️ Problem

Tưởng tượng một app thu thập dữ liệu trên mạng. Có nhiều loại tài liệu: PDF, DOC, CSV, và con app này cố gắng lôi các thông tin hữu ích ra từ những tài liệu này.

Phiên bản đầu tiên có thể làm việc với file DOC. Phiên bản tiếp theo có thêm CSV, và sau nữa là PDF. Vào một lúc nào đó. Bạn nhận ra 3 class để xử lý 3 loại tài liệu này có nhiều phần giống nhau phết.

### ✅ Solution

Template method bảo là chia thuật toán ra thành một dãy các bước, chuyển các bước đó thành phương thức, cho dãy các lời gọi các bước này vào một _template method_. Các bước có thể là `abstract`, hoặc có một vài khai triển mặc định. Để sử dụng thuật toán, client phải tự có 1 class kế thừa, khai triển tất cả các bước trừu tượng, và ghi đè một vài bước nếu cần.

Trong ví dụ bên trên, mình có thể triển khai như sau

```Java
abstract class DataMiner {
	public void mine(path) {
		file = openFile(path);
		rawData = extractData(file);
		data = parseData(rawData);
		analysis = analyseData(data);
		sendReport(analysis);
		closeFile(file);
	} // template method
	abstract File openFile(path);
	abstract RawData extractData(file);
	abstract Data parseData(rawData);
	Analysis analyseData(data) {} // step with default implementation
	void sendReport(analysis) {} // another stop with dèault implementation
	abstract void closeFile(file);
}

class PDFDataMiner {
	File openFile (path) {...}
	...
}
```

### ⚡ Applicability

- Khi muốn cho client chỉ được phép mở rộng một vài bước trong 1 thuật toán, chứ không cho phép thay đổi toàn bộ hoặc cấu trúc của thuật toán
- Khi có vài class chứa các thuật toán gần giống nhau, khác mỗi tí.

### 🖼️ UML

![[image 22.png]]

## Visitor

### ⚠️ Problem

Tượng tượng bạn có một app làm việc với thông tin địa lý dưới dạng một đồ thị siêu to. Bạn có nhiệm vụ chuyển đồ thị thành file XML. Công việc khá đơn giản, chỉ cần viết 1 method để chuyển cho mỗi class đại diện cho node trong đồ thị, sau đó dùng đệ quy để đi qua các node khác.

Mỗi tội là bạn không được động vào các class đó, vì nhỡ có làm sao thì cty ôn lằn. Hơn nữa, code chuyển sang XML ở các node nghe không hợp lý lắm, vì logic của các node là mô tả thông tin địa lý chứ không phải XML. Một lý do khác, nhỡ sau này bạn phải chuyển thông tin sang một định dạng file khác, thì lại phải động vào đống class đấy, lại có nguy cơ làm hỏng.

### ✅ Solution

Visitor pattern bảo là, hãy đặt cái hành vi mới vào một class khác hẳn, gọi là _visitor_, thay vì cố nhét vào class ban đầu.

### ⚡ Applicability

### 🖼️ UML