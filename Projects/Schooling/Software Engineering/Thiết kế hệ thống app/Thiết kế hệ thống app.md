# I. Design patterns

## Singleton

---

Một class Singleton là một class chỉ có một instance duy nhất được tạo ra trong toàn bộ quá trình app chạy.

### Why?

Các transaction được lưu trong csdl sẽ lưu cả account nguồn, đích và category tương ứng (nếu có). Các thông tin này chỉ lưu dưới dạng id để làm khoá ngoài tới bảng Accounts và Categories. Khi đọc, cũng chỉ đọc thông tin id này, việc đọc sẽ nhanh hơn là cứ mỗi khi đọc 1 transaction thì phải đọc 1-2 account và 1 category tương ứng trong 2 bảng khác. Hơn nữa, việc lưu cả 1 đối tượng Account trong 1 đối tượng Transaction sẽ tốn bộ nhớ hơn là chỉ lưu mỗi 1 id. Vậy khi cần hiển thị thông tin của account tương ứng với transaction, phải tìm ở đâu? Chẳng lẽ lại kết nối database rồi đọc tiếp?

Thay vì thế, tạo 1 class App theo pattern Singleton. Đối tượng này được tạo 1 lần duy nhất trong cả quá trình app chạy. Khi khởi tạo, App sẽ đọc tất cả Account và Category, lưu chúng vào mảng. Các thành phần khác, khi muốn biết thông tin về account nguồn của một transaction nào đó, chỉ cần gọi đối tượng App ra và tra cứu trong mảng đã lưu. Không cần kết nối lại database, và việc đọc thông tin của 1 transaction cũng sẽ nhanh hơn do không cần tham chiếu đến các bảng khác.

### How?

![[/image 6.png|image 6.png]]

## Prototype

---

Là một method cho phép tạo 1 bản sao của 1 object nào đó.

### Why?

Khi muốn thay đổi thông tin của 1 Transaction nhìn thấy trong list, người dùng sẽ bấm vào transaction đó, và một cửa sổ chứa thông tin của transaction đó sẽ được mở ra, cho phép người dùng chỉnh sửa thông tin, sau đó bấm lưu hoặc huỷ. Chỉ khi nào bấm lưu, thông tin của Transaction tương ứng mới được thay đổi. Quá trình này đòi hỏi tạo ra 1 bản sao của Transaction. Việc tạo bản sao này, nếu đặt trong phần UI thì sẽ vô lý, vì UI chỉ nên làm UI. Thế thì phải để nó trong chính class Transaction.

### How?

![[/image 1 3.png|image 1 3.png]]

### Example

```Java
public Transaction prototype() {
        return new Transaction(
            this.id,                       // Copying the ID
            this.dateTime,                 // Copying the date and time
            this.amount,                   // Copying the amount
            this.sourceAccount,            // Copying the source account
            this.category,                 // Copying the category
            this.destinationAccount,       // Copying the destination account (if any)
            this.note,                     // Copying the note
            this.type                      // Copying the type (income, expense, or transfer)
        );
}
```

## Strategy

---

Strategy là pattern cho phép thực hiện 1 công việc theo nhiều cách khác nhau, tuỳ vào người sử dụng code

### Why?

Chức năng filter có thể có một vài lớp filter. VD người dùng có thể chọn filter theo account, theo category, theo note, hoặc 2 trong 3 cái, hoặc cả 3. Có rất nhiều tổ hợp các filter có thể xảy ra.

Việc viết một class Filter theo Strategy method sẽ giúp việc code chức năng filter đơn giản và dễ đọc hơn. Xem ví dụ.

### Example

Interface FilterStrategy:

```Java
public interface FilterStrategy {
    List<Transaction> filter(List<Transaction> transactions);
}
```

Class FilterBySourceAccount:

```Java
public class FilterBySourceAccount implements FilterStrategy {
    private Account account;

    public FilterBySourceAccount(Account account) {
        this.account = account;
    }

    @Override
    public List<Transaction> filter(List<Transaction> transactions) {
        return transactions.stream()
                .filter(transaction -> transaction.getSourceAccount().equals(account))
                .collect(Collectors.toList());
    }
}
```

Class FilterByDestinationAccount:

```Java
public class FilterByDestinationAccount implements FilterStrategy {
    private Account account;

    public FilterByDestinationAccount(Account account) {
        this.account = account;
    }

    @Override
    public List<Transaction> filter(List<Transaction> transactions) {
        return transactions.stream()
                .filter(transaction -> transaction.getDestinationAccount().equals(account))
                .collect(Collectors.toList());
    }
}
```

Class FilterBy Note:

```Java
public class FilterByNote implements FilterStrategy {
    private String note;

    public FilterByNote(String note) {
        this.note = note;
    }

    @Override
    public List<Transaction> filter(List<Transaction> transactions) {
        return transactions.stream()
                .filter(transaction -> transaction.getNote().equals(note))
                .collect(Collectors.toList());
    }
}
```

Class FilterByCategory:

```Java
public class FilterByCategory implements FilterStrategy {
    private Category category;

    public FilterByCategory(Category category) {
        this.category = category;
    }

    @Override
    public List<Transaction> filter(List<Transaction> transactions) {
        return transactions.stream()
                .filter(transaction -> transaction.getCategory().equals(category))
                .collect(Collectors.toList());
    }
}
```

Class FilterContext:

```Java
public class FilterContext {
    private List<FilterStrategy> strategies = new ArrayList<>();

    // Add a filter strategy to the context
    public void addStrategy(FilterStrategy strategy) {
        strategies.add(strategy);
    }

    // Apply all filter strategies
    public List<Transaction> applyFilters(List<Transaction> transactions) {
        List<Transaction> filteredTransactions = new ArrayList<>(transactions);

        for (FilterStrategy strategy : strategies) {
            filteredTransactions = strategy.filter(filteredTransactions);
        }

        return filteredTransactions;
    }

    // Clear all strategies
    public void clearStrategies() {
        strategies.clear();
    }
}
```

Oke. Khi cần filter, chỉ cần làm thế này

```Java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Transaction> transactions = new ArrayList<>();
        // Initialize transactions...

        Account sourceAccount = new Account("Account1");
        Account destinationAccount = new Account("Account2");
        String note = "Payment";
        Category category = new Category("Groceries");

        FilterContext context = new FilterContext();

        // Add multiple filter strategies
        context.addStrategy(new FilterBySourceAccount(sourceAccount));
        context.addStrategy(new FilterByDestinationAccount(destinationAccount));
        context.addStrategy(new FilterByNote(note));
        context.addStrategy(new FilterByCategory(category));

        // Apply all filters
        List<Transaction> filteredTransactions = context.applyFilters(transactions);

        // Clear filters and start over if needed
        context.clearStrategies();
    }
}
```

Như vậy thì lúc cần filter chỉ cần gọi context, add strategy, rồi applyFilters. Quá clear.

# II. Cấu trúc file

Sau khi nghiên cứu xong đống pattern bên trên, thì giờ là cách tổ chức các file trong project

## Package server

---

Chứa các package con liên quan đến các model và làm việc với database

### util

Chứa class DBConnection để tạo 1 kết nối với database

### DAO

Data access objects. Chứa các class phục vụ việc gửi lệnh thêm, sửa, xoá cho database. Các class trong này chỉ được gọi từ các class ở phần Service

### Service

Chứa các class phục vụ việc gửi lệnh từ code ở UI đến DAO. Trước khi gửi, các class này sẽ có bước kiểm tra hoặc chuẩn hoá dữ liệu. VD khi người dùng bấm nút tạo 1 Transaction mới, UI sẽ gọi TransactionService, bảo thêm cái Transaction này đi. TransactionService sẽ kiểm tra các thông tin xem có hợp lệ không (ngày tháng, số tiền,…). Nếu không hợp lệ, TransactionService sẽ throws Exception báo không hợp lệ. Nếu hợp lệ, TransactionService sẽ gọi TransactionDAO, bảo thêm Transaction này vào database đi. TransactionDAO sẽ tạo câu lệnh SQL tương ứng, gọi DBConnection để lấy kết nối, rồi gửi câu lệnh vừa tạo cho kết nối vừa tạo. Quy trình này sẽ rất dễ test lỗi, xác định thành phần gây lỗi và cô lập nó.

### Model

Chứa các class Transaction, Account và Category

Transaction gồm:

- int id: id trong database
- String type: “Income”, “Expense” hoặc “Transfer”
- double amount: số tiền
- int category: id của category tương ứng, nếu `type` là “Transfer” thì id là -1
- int sourceAccount: id của tài khoản nguồn
- int destinationAccount: id của tài khoản đích, nếu `type` là “Income” hoặc “Expense” thì id là -1
- String note

Account gồm:

- int id: id trong database
- String name: tên account (VD MB Bank)
- String group: tên group của account (VD tài khoản)
- double balance: số dư tài khoản
- double goal: mục tiêu tiết kiệm. Các tài khoản thường sẽ có goal = 0. Account có goal ≠ 0 được coi là một khoản tiết kiệm

Category gồm:

- id: id trong database
- String name
- String type: “Income” hoặc “Expense”
- double budget: ngân sách hàng tháng của khoản này (VD mỗi tháng ăn hết 2 củ)

## Package App

---

Package app sẽ liên kết Server với UI, bao gồm class AppSettings, App và Main

### AppSettings

Chứa các cài đặt về app. Các cài đặt được lưu trong file cài đặt. Khi khởi tạo sẽ đọc các cài đặt trong file cài đặt này. Các cài đặt sẽ ảnh hưởng đến UI. Các cài đặt bao gồm:

- Đơn vị tiền tệ
- Chế độ sáng/tối
- Ngôn ngữ
- …

### App

Là một singleton, như đã giải thích ở trên. App sẽ chứa:

- Danh sách account
- Danh sách category có type=”income”
- Danh sách category có type=”expense”
- Một object AppSettings

### Main

chỉ chứa mỗi method main. Method main sẽ rất ngắn gọn. File main hiện tại của t trông như này

```Java
import gui.pages.SidebarNavigationPane;
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.stage.Stage;

public class Main extends Application {

    @Override
    public void start(Stage primaryStage) {
        SidebarNavigationPane page = new SidebarNavigationPane();
        Scene scene = new Scene(page, 1000, 600);

        // Set up the stage
        primaryStage.setTitle("Test");
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## Package GUI

---

Chứa các package con liên quan đến UI

Cái SidebarNavigationPane trên kia là một class `extends MigPane` (ở trong MigLayout, t nhắc đến 1 lần rồi đấy, đại khái là một cái Pane xong nó có khả năng tuỳ chỉnh kích thước các thành phần bên trong vip pro vcl).

Bên trong SidebarNavigationPane chứa 2 thành phần, 1 là cái menu bên trái, và 2 là khu vực bên phải. Khi nhấn vào 1 lựa chọn trong menu, nó sẽ xoá page hiện tại trong khu vực bên phải đi, rồi thêm page tương ứng vào đấy.

Ae nghiên cứu cách viết các class extends các thành phần mặc định trong Javafx và MigPane đi, thì phần UI mới làm chung nhau được, và các thành phần cũng có thể được sử dụng lại. VD như t có một class `gui.components.util.BalanceLabel` như này

```Java
package gui.components.util;

import javafx.geometry.Pos;
import javafx.scene.control.Label;
import javafx.scene.layout.Region;
import javafx.scene.paint.Color;

public class BalanceLabel extends Label {

    public BalanceLabel(double number) {
        this(number, getColorBasedOnValue(number));
    }

    public BalanceLabel(double number, Color color) {
        super(formatNumber(number));
        
    }

    private static Color getColorBasedOnValue(double number) {
        return number >= 0 ? Color.BLUE : Color.RED;
    }

    private static String formatNumber(double number) {
        return String.format("%.2f", Math.abs(number));
    }
    
    public void update (double number, Color color) {
    	setTextFill(color);
        setAlignment(Pos.CENTER_RIGHT); 
        setMinWidth(Region.USE_PREF_SIZE);
        super.setText(formatNumber(number));
    }
    
    public void update (double number) {
    	update(number, getColorBasedOnValue(number));
    }
}
```

Thì ở trong page OverviewPage chẳng hạn, t sẽ dùng nó như này

```Java
RoundedPane pane = new RoundedPane("Total Balance");
BalanceLabel amount = new BalanceLabel(0);
...
amount.update(sum, Color.BLACK);
amount.setFont(new Font("Montserrat Bold", 25));
pane.getChildren().add(amount);
```

hoặc như này

```Java
RoundedPane pane = new RoundedPane("Total Expense");
BalanceLabel amount = new BalanceLabel(0);
double sum = 0;
for (Transaction transaction: app.getTransactionList()) {
	if (transaction.getType().equals("Expense"))
		sum -= transaction.getAmount();
}
amount.update(sum);
amount.setFont(new Font("Montserrat Bold", 25));
pane.getChildren().add(amount);
```

Kết quả nó sẽ như này. Các BalanceLabel là các con số trong ảnh

![[/image 2 3.png|image 2 3.png]]

Lợi ích của việc này là, ví dụ nếu thấy màu xanh kia xấu quá, muốn thay màu xanh khác, thì chỉ cần sửa trong class `BalanceLabel`, chứ không cần đi tìm tất cả các nơi có cái `Label` hiển thị số tiền nữa (có rất nhiều `Label` kiểu này trong app). Cái `RoundedPane` là 3 cục chữ nhật trong ảnh trên đấy. Nó được định nghĩa là `class RoundedPane extends VBox`

Với cách này, package gui sẽ gồm 1 class `SidebarNavigationPane` và các package con:

### pages

Chứa các class `OverviewPage`, `AnalysisPage`, `AccountsPage`, `SavingsPage`, `SettingsPage`. Các page này extends cái gì thì tuỳ, miễn là ghép vào được

### components

Chứa các thành phần dùng đi dùng lại nhiều lần trong app. VD như `BalanceLabel` - Label hiển thị số tiền, `RoundedPane` - một cái VBox có viền cong và border, `Modal` - một cửa sổ mới, hiển thị content đã cho (xem ví dụ)

```Java
package gui.components.util;

import javafx.scene.Node;
import javafx.scene.Scene;
import javafx.scene.layout.VBox;
import javafx.stage.Modality;
import javafx.stage.Stage;

public class Modal {

    private final Stage stage;

    public Modal(Node content, String title) {
        // Create a new Stage
        stage = new Stage();
        stage.setTitle(title);

        // Set the modality to block input events to other windows
        stage.initModality(Modality.APPLICATION_MODAL);

        // Create the content of the modal
        VBox layout = new VBox(10);
        layout.setPadding(new javafx.geometry.Insets(20));
        
        // Add the provided content Node
        layout.getChildren().addAll(content);

        // Set the scene
        Scene scene = new Scene(layout);
        stage.setScene(scene);
    }

    // Method to show the modal
    public void show() {
        stage.showAndWait(); // Show the modal and wait for it to close
    }

    // Method to close the modal
    public void close() {
        stage.close(); // Close the stage
    }
}
```

Sử dụng class Modal trong một TransactionListItem

```Java
setOnMouseClicked(event -> {
  if (event.getClickCount() == 2) {
     EditTransactionForm form = new EditTransactionForm(this.transaction);
     form.setItem(this);
     Modal modal = new Modal(form, "Edit transaction");
     modal.show();
  }
});
```

Đúng rồi, `TransactionListItem` là một class `extends MigPane`, `EditTransactionForm` cũng `extends MigPane` luôn. Đây là hình ảnh một `TransactionListItem` trong app:

![[/image 3 3.png|image 3 3.png]]

Và khi nháy chuột 2 lần vào, thì `Modal` sẽ hiện ra:

![[/image 4 2.png|image 4 2.png]]

Nội dung của `Modal` chính là một `EditTransactionForm`, với thông tin lấy từ chính thông tin của cái `TransactionListItem` hiện tại (`EditTransactionForm form = new EditTransactionForm(this.transaction);`). Người dùng sẽ không thể bấm vào app chính cho đến khi đóng `Modal` qua các nút bên trong nó hoặc nút tắt.

Nhờ việc tách các thành phần ra, nên code ở mỗi phần cực kỳ ngắn gọn. Hơn nữa, các thành phần đều có thể được sử dụng lại. Ví dụ bây giờ code chức năng cứ bấm vào 1 account nào đó thì sẽ xem được toàn bộ Transaction liên quan đến account đó, thì không cần viết lại từ đầu, mà có thể tận dụng lại `TransactionListItem`