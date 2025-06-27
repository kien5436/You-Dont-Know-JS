# Bạn không hiểu JS: Phạm vi & Hàm khép kín
# Chương 1: Phạm vi là gì?

Một trong những mô thức nền tảng nhất của gần như mọi ngôn ngữ lập trình là khả năng lưu trữ giá trị vào các biến, sau đó truy xuất hoặc sửa đổi các giá trị đó. Trong thực tế, khả năng lưu trữ và lấy giá trị ra khỏi biến tạo nên *trạng thái* cho một chương trình.

Nếu không có khái niệm này, một chương trình vẫn có thể thực hiện vài tác vụ, nhưng chúng sẽ cực kỳ hạn chế và không mấy thú vị.

Thế nhưng việc đưa biến vào chương trình lại làm nảy sinh những câu hỏi thú vị nhất mà chúng ta sẽ giải quyết ngay bây giờ: những biến đó *tồn tại* ở đâu? Nói cách khác, chúng được lưu trữ ở đâu? Và quan trọng nhất, làm thế nào chương trình của ta tìm thấy chúng khi cần?

Những câu hỏi này cho thấy sự cần thiết của một tập hợp các quy tắc được định nghĩa rõ ràng để lưu trữ biến ở một vị trí nào đó và tìm lại chúng sau này. Ta sẽ gọi tập hợp các quy tắc đó là: *Phạm vi*.

Nhưng các quy tắc *Phạm vi* này được thiết lập ở đâu và như thế nào?

## Lý thuyết chương trình biên dịch

Tùy vào mức độ tương tác của bạn với các ngôn ngữ khác nhau, điều này có thể là hiển nhiên, hoặc cũng có thể gây ngạc nhiên, mặc dù JavaScript được xếp vào loại ngôn ngữ "động" hoặc "thông dịch", nó thực chất là một ngôn ngữ biên dịch. Nó *không* được biên dịch kĩ càng như nhiều ngôn ngữ biên dịch truyền thống, kết quả biên dịch của nó cũng không thể mang đi sử dụng trên các hệ thống phân tán khác nhau.

Tuy nhiên, bộ máy JavaScript vẫn thực hiện nhiều bước tương tự như bất kỳ chương trình biên dịch ngôn ngữ truyền thống nào, dù theo những cách tinh vi hơn chúng ta thường nghĩ.

Trong một quy trình của ngôn ngữ biên dịch truyền thống, một đoạn mã nguồn, tức chương trình của bạn, thường sẽ trải qua ba bước *trước khi* được thực thi, gọi chung là "biên dịch":

1. **Phân đoạn/Phân tích từ vựng (đoạnizing/Lexing):** chia một chuỗi ký tự thành các khối có ý nghĩa (đối với ngôn ngữ), gọi là đoạn. Ví dụ, xét chương trình: `var a = 2;`. Chương trình này có thể sẽ được chia thành các đoạn sau: `var`, `a`, `=`, `2`, và `;`. Khoảng trắng có thể được giữ lại hoặc không, tùy thuộc vào việc nó có ý nghĩa hay không.

    **Lưu ý:** Sự khác biệt giữa phân đoạn và phân tích từ vựng khá tinh vi và mang tính học thuật, nhưng nó xoay quanh việc liệu các đoạn này được xác định theo cách *không trạng thái* hay *có trạng thái*. Nói một cách đơn giản, nếu bộ phân đoạn phải gọi đến các quy tắc phân tích trạng thái để xác định xem `a` nên được coi là một đoạn riêng biệt hay chỉ là một phần của một đoạn khác, thì *đó* chính là **phân tích từ vựng**.

2. **Phân tích cú pháp (Parsing):** lấy một luồng (mảng) các đoạn và biến nó thành một cây gồm các phần tử lồng nhau, đại diện cho cấu trúc ngữ pháp của chương trình. Cây này được gọi là "AST" (<b>A</b>bstract <b>S</b>yntax <b>T</b>ree - Cây Cú pháp Trừu tượng).

    Cây biểu diễn cho `var a = 2;` có thể bắt đầu bằng một nút cấp cao nhất gọi là `VariableDeclaration` (Khai báo biến), với một nút con gọi là `Identifier` (Định danh) (có giá trị là `a`), và một nút con khác gọi là `AssignmentExpression` (Biểu thức gán), bản thân nó lại có một nút con gọi là `NumericLiteral` (Thuần số) (có giá trị là `2`).

3. **Sinh mã (Code-Generation):** quá trình lấy một AST và biến nó thành mã có thể thực thi. Phần này khác nhau rất nhiều tùy thuộc vào ngôn ngữ, nền tảng mà nó nhắm đến, v.v.

    Vì vậy, thay vì sa lầy vào chi tiết, ta sẽ chỉ nói một cách vắn tắt rằng có một cách để lấy AST đã mô tả ở trên cho `var a = 2;` và biến nó thành một tập hợp các lệnh máy để thực sự *tạo* ra một biến tên là `a` (bao gồm cả việc cấp phát bộ nhớ, v.v.), rồi lưu một giá trị vào `a`.

    **Lưu ý:** Chi tiết về cách bộ máy quản lý tài nguyên hệ thống sâu sắc hơn những gì chúng ta sẽ tìm hiểu, vì vậy ta sẽ tạm chấp nhận rằng bộ máy có khả năng tạo và lưu trữ biến khi cần.

Bộ máy JavaScript phức tạp hơn *chỉ* ba bước trên rất nhiều, cũng như hầu hết các chương trình biên dịch ngôn ngữ khác. Ví dụ, trong quá trình phân tích cú pháp và sinh mã, chắc chắn có các bước để tối ưu hóa hiệu suất thực thi, bao gồm cả việc thu gọn các phần tử dư thừa, v.v.

Vì vậy, ở đây tôi chỉ đang phác thảo những nét đại cương. Nhưng tôi nghĩ bạn sẽ sớm hiểu tại sao những chi tiết *này* mà chúng ta *đề cập đến*, dù ở mức độ tổng quan, lại có liên quan.

Một lý do là các bộ máy JavaScript không có được sự xa xỉ (như các chương trình biên dịch ngôn ngữ khác) là có nhiều thời gian để tối ưu hóa, bởi vì việc biên dịch JavaScript không diễn ra trong một giai đoạn xây dựng từ đầu như các ngôn ngữ khác.

Đối với JavaScript, việc biên dịch xảy ra, trong nhiều trường hợp, chỉ vài micro giây (hoặc ít hơn!) trước khi mã được thực thi. Để đảm bảo hiệu suất nhanh nhất, các bộ máy JS sử dụng đủ mọi loại thủ thuật (như JIT, biên dịch trễ và thậm chí biên dịch lại ngay lúc chạy, v.v.) mà đã vượt ra ngoài "phạm vi" thảo luận của chúng ta ở đây.

Hãy cứ nói một cách đơn giản rằng, bất kỳ đoạn mã JavaScript nào cũng phải được biên dịch trước khi (thường là *ngay* trước khi!) nó được thực thi. Vì vậy, chương trình biên dịch JS sẽ nhận lấy chương trình `var a = 2;` và biên dịch nó *trước*, sau đó sẵn sàng để thực thi, thường là ngay lập tức.

## Hiểu về Phạm vi

Cách chúng ta tiếp cận việc học về phạm vi là nghĩ về quá trình này như một cuộc trò chuyện. Nhưng, *ai* đang trò chuyện?

### Dàn nhân vật

Hãy cùng gặp gỡ dàn nhân vật tương tác với nhau để xử lý chương trình `var a = 2;`, từ đó chúng ta có thể hiểu được các cuộc đối thoại mà chúng ta sẽ nghe lỏm ngay sau đây:

1. ***Bộ máy***: chịu trách nhiệm từ đầu đến cuối việc biên dịch và thực thi chương trình JavaScript của chúng ta.

2. ***Chương trình biên dịch***: một trong những người bạn của *Bộ máy*; xử lý tất cả phần việc nặng nhọc của việc phân tích cú pháp và sinh mã (xem phần trước).

3. ***Phạm vi***: một người bạn khác nữa của *Bộ máy*; thu thập và duy trì một danh sách tra cứu tất cả các định danh (biến) đã được khai báo, và thực thi một bộ quy tắc nghiêm ngặt về cách mã đang thực thi có thể truy cập chúng.

Để *hoàn toàn thấu hiểu* cách JavaScript hoạt động, bạn cần bắt đầu *suy nghĩ* như cách *Bộ máy* (và những người bạn) suy nghĩ, hỏi theo cách họ hỏi và trả lời theo cách họ trả lời.

### Đối đáp

Khi bạn thấy chương trình `var a = 2;`, nhiều khả năng bạn nghĩ đó là một câu lệnh. Nhưng đó không phải là cách người bạn mới của chúng ta, *Bộ máy*, nhìn nhận nó. Thực tế, *Bộ máy* thấy hai câu lệnh riêng biệt, một câu lệnh mà *Chương trình biên dịch* sẽ xử lý trong quá trình biên dịch, và một câu lệnh mà *Bộ máy* sẽ xử lý trong quá trình thực thi.

Vậy hãy cùng phân tích cách *Bộ máy* và những người bạn sẽ tiếp cận chương trình `var a = 2;`.

Điều đầu tiên *Chương trình biên dịch* sẽ làm với chương trình này là thực hiện phân tích từ vựng để chia nó thành các đoạn, sau đó sẽ phân tích cú pháp của chúng thành một cây. Nhưng khi *Chương trình biên dịch* đến bước sinh mã, nó sẽ xử lý chương trình này hơi khác so với những gì có thể được giả định.

Một giả định hợp lý là *Chương trình biên dịch* sẽ tạo ra mã có thể được tóm tắt bằng mã giả sau: "Cấp phát bộ nhớ cho một biến, gán nhãn là `a`, sau đó đặt giá trị `2` vào biến đó". Rất tiếc, điều đó không hoàn toàn chính xác.

Thay vào đó *Chương trình biên dịch* sẽ tiến hành như sau:

1. Gặp `var a`, *Chương trình biên dịch* hỏi *Phạm vi* xem một biến `a` đã tồn tại trong tập hợp phạm vi cụ thể đó chưa. Nếu có, *Chương trình biên dịch* bỏ qua khai báo này và tiếp tục. Nếu không, *Chương trình biên dịch* yêu cầu *Phạm vi* khai báo một biến mới tên là `a` cho tập hợp phạm vi đó.

2.  *Chương trình biên dịch* sau đó tạo ra mã cho *Bộ máy* để thực thi sau này, để xử lý phép gán `a = 2`. Mã mà *Bộ máy* chạy sẽ đầu tiên hỏi *Phạm vi* xem có biến nào tên là `a` có thể truy cập được trong tập hợp phạm vi hiện tại không. Nếu có, *Bộ máy* sử dụng biến đó. Nếu không, *Bộ máy* tìm ở *nơi khác* (xem phần *Phạm vi* lồng nhau bên dưới).

Nếu *Bộ máy* cuối cùng tìm thấy một biến, nó sẽ gán giá trị `2` cho biến đó. Nếu không, *Bộ máy* sẽ giơ tay và hét lên một lỗi!

Tóm lại: hai hành động riêng biệt được thực hiện cho một phép gán biến: Thứ nhất, *Chương trình biên dịch* khai báo một biến (nếu chưa được khai báo trước đó trong phạm vi hiện tại), và thứ hai, khi thực thi, *Bộ máy* tra cứu biến trong *Phạm vi* và gán giá trị cho nó, nếu tìm thấy.

### Thuật ngữ Trình biên dịch

Chúng ta cần thêm một chút thuật ngữ trình biên dịch để tiếp tục đi sâu vào việc tìm hiểu.

Khi *Bộ máy* thực thi mã mà *Chương trình biên dịch* đã tạo ra cho bước (2), nó phải tra cứu biến `a` để xem nó đã được khai báo chưa, và việc tra cứu này là tham vấn *Phạm vi*. Nhưng loại tra cứu mà *Bộ máy* thực hiện sẽ ảnh hưởng đến kết quả của việc tra cứu.

Trong trường hợp của chúng ta, người ta nói rằng *Bộ máy* sẽ thực hiện một tra cứu "LHS" cho biến `a`. Loại tra cứu còn lại được gọi là "RHS".

Tôi cá là bạn có thể đoán được "L" và "R" có nghĩa là gì. Các thuật ngữ này là viết tắt của "Left-hand Side" (Vế Trái) và "Right-hand Side" (Vế Phải).

Vế... của cái gì? **Của một phép toán gán.**

Nói cách khác, một tra cứu LHS được thực hiện khi một biến xuất hiện ở vế trái của một phép toán gán, và một tra cứu RHS được thực hiện khi một biến xuất hiện ở vế phải của một phép toán gán.

Thực ra, hãy nói chính xác hơn một chút. Một tra cứu RHS, đối với mục đích của chúng ta, không thể phân biệt được với việc đơn giản là tra cứu giá trị của một biến nào đó, trong khi tra cứu LHS là cố gắng tìm chính vùng chứa của biến đó, để có thể gán giá trị. Theo cách này, RHS không *thực sự* có nghĩa là "vế phải của một phép gán", mà chính xác hơn, nó chỉ có nghĩa là "không phải là vế trái".

Nói một cách ví von, bạn có thể xem "RHS" như là "lấy giá trị nguồn của nó" (retrieve his/her source value), ngụ ý rằng RHS có nghĩa là "đi lấy giá trị của...".

Hãy cùng đào sâu hơn.

Khi tôi viết:

```js
console.log( a );
```

Tham chiếu đến `a` là một tham chiếu RHS, bởi vì không có gì được gán cho `a` ở đây. Thay vào đó, chúng ta đang tra cứu để lấy giá trị của `a`, để giá trị đó có thể được truyền cho `console.log(..)`.

Ngược lại:

```js
a = 2;
```

Tham chiếu đến `a` ở đây là một tham chiếu LHS, bởi vì chúng ta không thực sự quan tâm đến giá trị hiện tại của nó là gì, chúng ta chỉ đơn giản muốn tìm biến đó như một mục tiêu cho phép toán gán `= 2`.

**Lưu ý:** LHS và RHS có nghĩa là "vế trái/phải của một phép gán" không nhất thiết có nghĩa đen là "bên trái/phải của toán tử gán `=`". Có một số cách khác mà phép gán xảy ra, và vì vậy tốt hơn là nên suy nghĩ về nó một cách khái niệm là: "ai là mục tiêu của phép gán (LHS)" và "ai là nguồn của phép gán (RHS)".

Hãy xem xét chương trình này, có cả tham chiếu LHS và RHS:

```js
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
```

Dòng cuối cùng gọi `foo(..)` như một lời gọi hàm đòi hỏi một tham chiếu RHS đến `foo`, có nghĩa là, "đi tra cứu giá trị của `foo`, và đưa nó cho tôi." Hơn nữa, `(..)` có nghĩa là giá trị của `foo` nên được thực thi, vì vậy tốt hơn hết nó thực sự phải là một hàm!

Có một phép gán tinh vi nhưng quan trọng ở đây. **Bạn có phát hiện ra không?**

Bạn có thể đã bỏ lỡ phép gán ngầm `a = 2` trong đoạn mã này. Nó xảy ra khi giá trị `2` được truyền làm đối số cho hàm `foo(..)`, trong trường hợp đó giá trị `2` được **gán** cho tham số `a`. Để (ngầm) gán cho tham số `a`, một tra cứu LHS được thực hiện.

Cũng có một tham chiếu RHS cho giá trị của `a`, và giá trị kết quả đó được truyền cho `console.log(..)`. `console.log(..)` cần một tham chiếu để thực thi. Đó là một tra cứu RHS cho đối tượng `console`, sau đó một quá trình phân giải thuộc tính xảy ra để xem nó có một phương thức tên là `log` hay không.

Cuối cùng, chúng ta có thể hình dung rằng có một sự trao đổi LHS/RHS khi truyền giá trị `2` (thông qua tra cứu RHS của biến `a`) vào `log(..)`. Bên trong việc triển khai gốc của `log(..)`, chúng ta có thể giả định nó có các tham số, tham số đầu tiên (có thể gọi là `arg1`) có một tra cứu tham chiếu LHS, trước khi gán `2` cho nó.

**Lưu ý:** Bạn có thể bị cám dỗ để hình dung việc khai báo hàm `function foo(a) {...` như một khai báo biến và phép gán thông thường, chẳng hạn như `var foo` và `foo = function(a){...`. Khi làm như vậy, sẽ rất dễ nghĩ rằng việc khai báo hàm này liên quan đến một tra cứu LHS.

Tuy nhiên, sự khác biệt tinh vi nhưng quan trọng là *Chương trình biên dịch* xử lý cả việc khai báo và định nghĩa giá trị trong quá trình sinh mã, sao cho khi *Bộ máy* đang thực thi mã, không có quá trình xử lý nào cần thiết để "gán" một giá trị hàm cho `foo`. Vì vậy, không thực sự phù hợp để nghĩ về một khai báo hàm như một phép gán tra cứu LHS theo cách chúng ta đang thảo luận ở đây.

### Cuộc trò chuyện giữa Bộ máy và Phạm vi

```js
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
```

Hãy tưởng tượng cuộc trao đổi ở trên (xử lý đoạn mã này) như một cuộc trò chuyện. Cuộc trò chuyện sẽ diễn ra đại loại như thế này:

> ***Bộ máy***: Này *Phạm vi* ơi, tôi có một tham chiếu RHS cho `foo`. Có nghe qua về nó chưa?

> ***Phạm vi***: Ồ có chứ. *Chương trình biên dịch* vừa mới khai báo nó một giây trước. Nó là một hàm. Của anh đây.

> ***Bộ máy***: Tuyệt, cảm ơn! OK, tôi đang thực thi `foo`.

> ***Bộ máy***: Này, *Phạm vi*, tôi có một tham chiếu LHS cho `a`, có nghe qua về nó chưa?

> ***Phạm vi***: Ồ có chứ. *Chương trình biên dịch* vừa mới khai báo nó như một tham số chính thức cho `foo` gần đây. Của anh đây.

> ***Bộ máy***: *Phạm vi* lúc nào cũng hữu ích thật. Cảm ơn lần nữa. Bây giờ, đến lúc gán `2` cho `a`.

> ***Bộ máy***: Này, *Phạm vi*, xin lỗi lại làm phiền. Tôi cần một tra cứu RHS cho `console`. Có nghe qua về nó chưa?

> ***Phạm vi***: Không vấn đề gì, *Bộ máy*, đây là việc tôi làm cả ngày mà. Vâng, tôi có `console`. Nó là một đối tượng tích hợp sẵn. Đây nhé.

> ***Bộ máy***: Hoàn hảo. Đang tra cứu `log(..)`. OK, tuyệt, nó là một hàm.

> ***Bộ máy***: Yo, *Phạm vi*. Giúp tôi với một tham chiếu RHS đến `a` được không. Tôi nghĩ là tôi nhớ nó, nhưng chỉ muốn kiểm tra lại cho chắc.

> ***Phạm vi***: Anh nói đúng rồi, *Bộ máy*. Vẫn là nó, không thay đổi gì. Đây nhé.

> ***Bộ máy***: Tuyệt. Đang truyền giá trị của `a`, tức là `2`, vào `log(..)`.

> ...

### Câu đố

Kiểm tra sự hiểu biết của bạn cho đến nay. Hãy chắc chắn rằng bạn đóng vai *Bộ máy* và có một "cuộc trò chuyện" với *Phạm vi*:

```js
function foo(a) {
	var b = a;
	return a + b;
}

var c = foo( 2 );
```

1.  Xác định tất cả các tra cứu LHS (có 3!).

2.  Xác định tất cả các tra cứu RHS (có 4!).

**Lưu ý:** Xem đáp án câu đố ở phần tổng kết chương!

## Phạm vi lồng nhau

Chúng ta đã nói rằng *Phạm vi* là một tập hợp các quy tắc để tra cứu các biến theo tên định danh của chúng. Tuy nhiên, thường có nhiều hơn một *Phạm vi* cần xem xét.

Giống như một khối lệnh hoặc hàm được lồng bên trong một khối lệnh hoặc hàm khác, các phạm vi cũng được lồng bên trong các phạm vi khác. Vì vậy, nếu một biến không thể được tìm thấy trong phạm vi ngay lập tức, *Bộ máy* sẽ tham vấn phạm vi chứa nó ở bên ngoài tiếp theo, tiếp tục cho đến khi tìm thấy hoặc cho đến khi đạt đến phạm vi ngoài cùng nhất (hay còn gọi là, toàn cục).

Xét ví dụ:

```js
function foo(a) {
	console.log( a + b );
}

var b = 2;

foo( 2 ); // 4
```

Tham chiếu RHS cho `b` không thể được giải quyết bên trong hàm `foo`, nhưng nó có thể được giải quyết trong *Phạm vi* bao quanh nó (trong trường hợp này là toàn cục).

Vì vậy, trở lại cuộc trò chuyện giữa *Bộ máy* và *Phạm vi*, chúng ta sẽ nghe lỏm được:

> ***Bộ máy***: "Này, *Phạm vi* của `foo`, có nghe qua về `b` chưa? Tôi có một tham chiếu RHS cho nó."

> ***Phạm vi***: "Chưa, chưa bao giờ nghe về nó. Anh tìm chỗ khác thử xem."

> ***Bộ máy***: "Này, *Phạm vi* bên ngoài của `foo`, à anh là *Phạm vi* toàn cục, ok tuyệt. Có nghe qua về `b` chưa? Tôi có một tham chiếu RHS cho nó."

> ***Phạm vi***: "Rồi, chắc chắn có. Đây nhé."

Các quy tắc đơn giản để duyệt qua *Phạm vi* lồng nhau: *Bộ máy* bắt đầu tại *Phạm vi* đang thực thi hiện tại, tìm biến ở đó, nếu không tìm thấy, tiếp tục đi lên một cấp, và cứ thế. Nếu đã đến phạm vi toàn cục ngoài cùng nhất, việc tìm kiếm sẽ dừng lại, cho dù có tìm thấy biến hay không.

### Xây dựng dựa trên phép ẩn dụ

Để hình dung quá trình phân giải *Phạm vi* lồng nhau, tôi muốn bạn nghĩ về tòa nhà cao tầng này.

<img src="fig1.png" width="250">

Tòa nhà đại diện cho bộ quy tắc *Phạm vi* lồng nhau của chương trình chúng ta. Tầng một của tòa nhà đại diện cho *Phạm vi* đang thực thi hiện tại của bạn, bất kể bạn đang ở đâu. Tầng cao nhất của tòa nhà là *Phạm vi* toàn cục.

Bạn giải quyết các tham chiếu LHS và RHS bằng cách tìm kiếm trên tầng hiện tại của mình, và nếu không tìm thấy, bạn đi thang máy lên tầng tiếp theo, tìm ở đó, rồi tầng tiếp theo, và cứ thế. Một khi bạn lên đến tầng cao nhất (Phạm vi toàn cục), bạn hoặc sẽ tìm thấy thứ mình cần, hoặc không. Nhưng dù sao bạn cũng phải dừng lại.

## Lỗi

Tại sao việc chúng ta gọi nó là LHS hay RHS lại quan trọng?

Bởi vì hai loại tra cứu này hành xử khác nhau trong trường hợp biến chưa được khai báo (không được tìm thấy trong bất kỳ *Phạm vi* nào được tham vấn).

Xét ví dụ:

```js
function foo(a) {
	console.log( a + b );
	b = a;
}

foo( 2 );
```

Khi tra cứu RHS cho `b` xảy ra lần đầu tiên, nó sẽ không được tìm thấy. Đây được gọi là một biến "chưa được khai báo", bởi vì nó không được tìm thấy trong phạm vi.

Nếu một tra cứu RHS không bao giờ tìm thấy một biến, ở bất cứ đâu trong các *Phạm vi* lồng nhau, điều này sẽ dẫn đến một `ReferenceError` được ném ra bởi *Bộ máy*. Điều quan trọng cần lưu ý là lỗi này thuộc loại `ReferenceError`.

Ngược lại, nếu *Bộ máy* đang thực hiện một tra cứu LHS và đến được tầng cao nhất (Phạm vi toàn cục) mà không tìm thấy nó, và nếu chương trình không chạy trong "Strict Mode" [^note-strictmode], thì *Phạm vi* toàn cục sẽ tạo một biến mới có tên đó **trong phạm vi toàn cục**, và trả nó lại cho *Bộ máy*.

*"Không, trước đây không có, nhưng tôi đã tốt bụng tạo một cái cho anh rồi."*

"Strict Mode" [^note-strictmode], được thêm vào trong ES5, có một số hành vi khác với chế độ bình thường/thoải mái/lười biếng. Một trong những hành vi đó là nó không cho phép tạo biến toàn cục một cách tự động/ngầm định. Trong trường hợp đó, sẽ không có biến nào trong *Phạm vi* toàn cục để trả về từ một tra cứu LHS, và *Bộ máy* sẽ ném ra một `ReferenceError` tương tự như trường hợp RHS.

Bây giờ, nếu một biến được tìm thấy cho một tra cứu RHS, nhưng bạn cố gắng làm điều gì đó với giá trị của nó mà không thể, chẳng hạn như cố gắng thực thi một giá trị không phải là hàm như một hàm, hoặc tham chiếu một thuộc tính trên một giá trị `null` hoặc `undefined`, thì *Bộ máy* sẽ ném ra một loại lỗi khác, gọi là `TypeError`.

`ReferenceError` liên quan đến thất bại trong việc phân giải *Phạm vi*, trong khi `TypeError` ngụ ý rằng việc phân giải *Phạm vi* đã thành công, nhưng đã có một hành động bất hợp pháp/không thể thực hiện được đối với kết quả.

## Tổng kết (TL;DR)

Phạm vi là tập hợp các quy tắc xác định một biến (định danh) có thể được tra cứu ở đâu và như thế nào. Việc tra cứu này có thể nhằm mục đích gán giá trị cho biến, đó là một tham chiếu LHS (vế trái), hoặc có thể nhằm mục đích truy xuất giá trị của nó, đó là một tham chiếu RHS (vế phải).

Các tham chiếu LHS xuất phát từ các phép toán gán. Các phép gán liên quan đến *Phạm vi* có thể xảy ra với toán tử `=` hoặc bằng cách truyền đối số cho (gán cho) các tham số của hàm.

*Bộ máy* JavaScript đầu tiên biên dịch mã trước khi thực thi, và trong quá trình đó, nó chia các câu lệnh như `var a = 2;` thành hai bước riêng biệt:

1.  Đầu tiên, `var a` để khai báo nó trong *Phạm vi* đó. Điều này được thực hiện ngay từ đầu, trước khi thực thi mã.

2.  Sau đó, `a = 2` để tra cứu biến (tham chiếu LHS) và gán giá trị cho nó nếu tìm thấy.

Cả hai tra cứu tham chiếu LHS và RHS đều bắt đầu tại *Phạm vi* đang thực thi hiện tại, và nếu cần (tức là, chúng không tìm thấy thứ chúng đang tìm ở đó), chúng sẽ đi lên theo *Phạm vi* lồng nhau, từng phạm vi (tầng) một, tìm kiếm định danh, cho đến khi chúng đến được phạm vi toàn cục (tầng trên cùng) và dừng lại, và hoặc tìm thấy nó, hoặc không.

Các tham chiếu RHS không được đáp ứng sẽ dẫn đến việc ném ra `ReferenceError`. Các tham chiếu LHS không được đáp ứng sẽ dẫn đến việc tạo ra một biến toàn cục tự động, ngầm định có tên đó (nếu không ở trong "Strict Mode" [^note-strictmode]), hoặc một `ReferenceError` (nếu ở trong "Strict Mode" [^note-strictmode]).

### Đáp án câu đố

```js
function foo(a) {
	var b = a;
	return a + b;
}

var c = foo( 2 );
```

1.  Xác định tất cả các tra cứu LHS (có 3!).

    **`c = ..`, `a = 2` (gán ngầm cho tham số) và `b = ..`**

2.  Xác định tất cả các tra cứu RHS (có 4!).

    **`foo(2..`, `= a;`, `a + ..` và `.. + b`**

[^note-strictmode]: MDN: [Strict Mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode)
