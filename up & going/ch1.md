# You Don't Know JS: Up & Going
# Chapter 1: Into Programming

Chào mừng anh đến với loạt sách *Bạn không hiểu JS* (*BKHJS*).

Quyển *Khởi động và tiến lên* là phần giới thiệu về một số khái niệm cơ bản trong lập trình - tất nhiên có xu hướng thiên về JavaScript (thường viết tắt là JS) - và cách tiếp cận cũng như hiểu các đầu sách còn lại trong loạt này. Đặc biệt nếu anh mới bắt đầu học lập trình và/hoặc JavaScript, quyển sách này sẽ lướt qua những gì anh cần để bắt đầu *lên đường*.

Sách bắt đầu bằng việc giải thích các nguyên lý lập trình cơ bản ở mức rất tổng quan. Nó chủ yếu dành cho những ai bắt đầu *BKHJS* mà chưa có (hoặc gần như chưa có) kinh nghiệm lập trình, và muốn dùng loạt sách này như một con đường để hiểu lập trình thông qua lăng kính JavaScript.

Chương 1 nên được tiếp cận như một cái nhìn khái quát về những thứ anh muốn học và làm để *bắt đầu lập trình*. Cũng có nhiều tài nguyên tuyệt vời khác để nhập môn lập trình giúp anh đào sâu hơn vào các chủ đề này, và tôi khuyến khích anh học thêm từ chúng bên cạnh chương này.

Khi đã thấy thoải mái với các kiến thức lập trình cơ bản, chương 2 sẽ giúp anh làm quen với phong cách lập trình của JavaScript. Chương này giới thiệu về JavaScript là gì, nhưng một lần nữa, nó không phải là hướng dẫn toàn diện - đó là nhiệm vụ của những quyển *BKHJS* còn lại!

Nếu anh đã khá thoải mái với JavaScript, hãy đọc thử chương 3 để có cái nhìn thoáng qua về những gì *BKHJS* sẽ mang lại, rồi bắt đầu ngay!

## Mã

Chúng ta hãy bắt đầu từ những điều cơ bản.

Một chương trình, thường được gọi là *mã nguồn* hoặc đơn giản là *mã*, là một tập hợp các chỉ dẫn đặc biệt để nói cho máy tính biết cần thực hiện những tác vụ nào. Thông thường, mã được lưu trong một tập tin văn bản, mặc dù với JavaScript, anh cũng có thể gõ trực tiếp vào bảng điều khiển dành cho nhà phát triển trong chương trình duyệt, thứ mà chúng ta sẽ bàn ngay sau đây.

Các quy tắc về định dạng hợp lệ và cách kết hợp các chỉ dẫn được gọi là *ngôn ngữ máy tính*, đôi khi còn được gọi là *cú pháp*, tương tự như tiếng Anh có các quy tắc về cách viết đúng chính tả và cách tạo câu hợp lệ bằng từ vựng và dấu câu.

### Câu lệnh

Trong ngôn ngữ máy tính, một nhóm các từ, con số và toán tử thực hiện một tác vụ cụ thể được gọi là *câu lệnh*. Trong JavaScript, một câu lệnh có thể trông như sau:

```js
a = b * 2;
```

Các kí tự `a` vá `b` được gọi là *biến* (xem phần "Biến"), có thể xem như các hộp đơn giản mà bạn dùng để đựng bất kì cái gì. Trong các chương trình, các biến giữ những giá trị (như số `42`) để chương trình sử dụng. Hãy nghĩ chúng như các kí hiệu giữ chỗ cho các giá trị.

Ngược lại, số `2` chỉ đơn thuần là giá trị, được gọi là *giá trị nguyên thủy*, vì nó không được lưu trữ trong biến.

Các kí tự `=` và `*` là các *toán tử* (xem phần "Toán tử") - chúng thực hiện các hành động với các giá trị và biến như gán và tính toán.

Hầu hết các lệnh trong JavaScript kết thúc với dấu chấm phẩy (`;`).

Câu lệnh `a = b * 2;`  nói với máy tính đại khái là "lấy giá trị được lưu trong biến `b`, nhân với giá trị `2`, sau đó lưu kết quả vào một biến khác mà ta sẽ gọi là `a`".

Chương trình chỉ là tập hợp của nhiều câu lệnh như vậy, cùng nhau mô tả tất cả các bước cần thực hiện để hoàn thành mục đích của chương trình.

### Biểu thức

Câu lệnh được cấu thành từ một hoặc nhiều *biểu thức*. Biểu thức là bất kỳ sự tham chiếu nào đến biến hoặc giá trị, hoặc tập hợp các biến và giá trị được kết hợp với toán tử.

Ví dụ:

```js
a = b * 2;
```

Câu lệnh này có bốn biểu thức:

* `2` là một *biểu thức giá trị nguyên thủy*
* `b` là một *biểu thức biến*, nghĩa là lấy giá trị hiện tại của biến đó
* `b * 2` là một *biểu thức số học*, nghĩa là thực hiện phép nhân
* `a = b * 2` là một *biểu thức gán*, nghĩa là gán kết quả của biểu thức `b * 2` cho biến `a` (sẽ nói rõ hơn về phép gán sau)

Một biểu thức tổng quát đứng riêng cũng được gọi là một *câu lệnh biểu thức*, ví dụ như sau:

```js
b * 2;
```

Kiểu câu lệnh biểu thức này không quá phổ biến hay hữu ích, vì thường thì nó không tạo ra tác động gì đến việc thực thi chương trình - nó chỉ lấy giá trị của `b` và nhân với `2`, nhưng sau đó không làm gì với kết quả đó cả.

Một câu lệnh biểu thức phổ biến hơn là *câu lệnh gọi hàm* (xem phần “Hàm”), vì toàn bộ câu lệnh chính là một biểu thức gọi hàm:

```js
alert( a );
```

### Thực thi một chương trình

Làm cách nào mà những tập hợp các câu lệnh lập trình hướng dẫn được cho máy tính phải làm gì? Chương trình cần được *thực thi*, hay còn gọi là *chạy chương trình*.

Những câu lệnh như `a = b * 2` rất hữu ích với lập trình viên trong quá trình đọc và viết mã, nhưng không thực sự ở dạng mà máy tính có thể hiểu trực tiếp. Vì vậy, cần có một công cụ đặc biệt trên máy tính (gọi là *chương trình thông dịch* hoặc *chương trình biên dịch*) để dịch mã anh viết thành các lệnh mà máy tính hiểu được.

Với một số ngôn ngữ lập trình, việc dịch lệnh thường được thực hiện từ trên xuống dưới, hết dòng này đến dòng khác, mỗi khi chương trình được chạy, cách này thường được gọi là *thông dịch* mã.

Với các ngôn ngữ được dịch sẵn từ trước, gọi là *biên dịch* mã, vì vậy khi chương trình *chạy* sau đó, thứ thực sự chạy là những lệnh đã được biên dịch sẵn, sẵn sàng để thực thi.

Thông thường người ta cho rằng JavaScript là một ngôn ngữ *thông dịch*, vì mã nguồn JavaScript của anh được xử lý mỗi lần nó chạy. Nhưng thực ra điều đó không hoàn toàn chính xác. Bộ máy JavaScript thực sự *biên dịch* chương trình ngay tại thời điểm chạy, rồi lập tức thực thi mã đã biên dịch đó.

**Lưu ý:** Để biết thêm về quá trình biên dịch trong JavaScript, hãy xem hai chương đầu tiên trong cuốn *Phạm vi và hàm đóng* của loạt sách này.

## Tự Thử Nghiệm

Chương này sẽ giới thiệu từng khái niệm lập trình với những đoạn mã đơn giản, tất cả đều được viết bằng JavaScript (tất nhiên rồi!).

Điều này không thể nhấn mạnh đủ: trong lúc anh đọc chương này - và có thể anh sẽ cần dành thời gian đọc lại nhiều lần - anh nên thực hành từng khái niệm bằng cách tự gõ lại mã. Cách dễ nhất để làm điều đó là mở bảng điều khiển trong công cụ dành cho nhà phát triển của chương trình duyệt gần nhất (Firefox, Chrome, IE, v.v.).

**Mẹo:** Thông thường, anh có thể mở bảng điều khiển bằng phím tắt hoặc thông qua bảng chọn. Để biết thêm chi tiết về cách mở và sử dụng bảng điều khiển trong chương trình duyệt yêu thích của mình, hãy xem bài viết "Làm chủ Bảng điều khiển Công cụ cho nhà phát triển" ([http://blog.teamtreehouse.com/mastering-developer-tools-console](http://blog.teamtreehouse.com/mastering-developer-tools-console)). Để gõ nhiều dòng mã cùng lúc trong bảng điều khiển, hãy dùng `<shift> + <enter>` để xuống dòng mới. Khi nhấn `<enter>`, bảng điều khiển sẽ thực thi toàn bộ những gì anh vừa gõ.

Bây giờ hãy làm quen với quá trình chạy mã trong bảng điều khiển. Trước tiên, tôi đề xuất anh mở một thẻ trống trong chương trình duyệt. Tôi thường làm điều đó bằng cách gõ `about:blank` vào thanh địa chỉ. Sau đó, đảm bảo rằng bảng điều khiển của anh đã được mở như đã nói ở trên.

Bây giờ, hãy gõ đoạn mã sau và xem cách nó chạy:

```js
a = 21;

b = a * 2;

console.log( b );
```

Gõ đoạn mã trên vào bảng điều khiển của Chrome sẽ cho đầu ra như thế này:

<img src="fig1.png" width="500">

Hãy thử đi. Cách học lập trình tốt nhất là bắt đầu viết mã!

### Đầu ra

Trong đoạn mã trước đó, chúng ta đã sử dụng `console.log(..)`. Hãy cùng xem nhanh dòng mã đó có ý nghĩa gì.

Anh có thể đã đoán được, nhưng đó chính là cách chúng ta in văn bản (hay còn gọi là *xuất* cho người dùng) trong bảng điều khiển dành cho nhà phát triển. Có hai đặc điểm của câu lệnh đó cần được giải thích.

Thứ nhất, phần `log(b)` được gọi là một lời gọi hàm (xem thêm phần “Hàm”). Điều đang diễn ra là chúng ta đưa biến `b` vào hàm đó, yêu cầu nó lấy giá trị của `b` và in ra bảng điều khiển.

Thứ hai, phần `console.` là một tham chiếu đến đối tượng nơi chứa hàm `log(..)`. Chúng ta sẽ tìm hiểu kỹ hơn về đối tượng và thuộc tính của chúng trong chương 2.

Một cách khác để tạo đầu ra mà anh có thể nhìn thấy là dùng câu lệnh `alert(..)`. Ví dụ:

```js
alert( b );
```

Nếu chạy đoạn mã đó, anh sẽ thấy thay vì in kết quả ra bảng điều khiển, chương trình duyệt sẽ hiển thị một hộp thoại với nút “OK” và nội dung là giá trị của biến `b`. Tuy nhiên, việc dùng `console.log(..)` sẽ giúp anh học lập trình và chạy các chương trình dễ dàng hơn so với `alert(..)` vì anh có thể xuất nhiều giá trị cùng lúc mà không làm gián đoạn giao diện chương trình duyệt.

Trong cuốn sách này, chúng ta sẽ sử dụng `console.log(..)` để hiển thị đầu ra.

### Đầu vào

Trong khi nói về việc xuất kết quả, chắc hẳn anh cũng đang thắc mắc về *đầu vào* (ví dụ như nhận thông tin từ người dùng).

Cách phổ biến nhất là trang HTML hiển thị các thành phần biểu mẫu (chẳng hạn như ô nhập văn bản) để người dùng nhập vào, sau đó dùng JavaScript để đọc các giá trị đó vào biến trong chương trình.

Tuy nhiên, có một cách đơn giản hơn để nhận dữ liệu đầu vào trong các ví dụ học tập và trình diễn như cách mà bạn sẽ làm trong cuốn sách này: sử dụng hàm `prompt(..)`:

```js
age = prompt( "Please tell me your age:" );

console.log( age );
```

Như anh có thể đoán, thông điệp mà anh truyền vào `prompt(..)` - trong ví dụ này là `"Please tell me your age:"` - sẽ được hiển thị trong hộp thoại bật lên.

Nó sẽ trông giống như sau:

<img src="fig2.png" width="500">

Sau khi anh nhập nội dung và nhấn "OK", giá trị anh gõ vào sẽ được lưu vào biến `age`, rồi chúng ta *xuất* giá trị đó bằng `console.log(..)`:

<img src="fig3.png" width="500">

Để đơn giản hoá khi học các khái niệm lập trình cơ bản, các ví dụ trong cuốn sách này sẽ không yêu cầu nhập đầu vào. Nhưng bây giờ anh đã biết cách dùng `prompt(..)`, nên nếu muốn thử thách bản thân, anh có thể dùng thêm phần nhập liệu khi khám phá các ví dụ.

## Toán tử

Toán tử là cách chúng ta thực hiện các thao tác trên biến và giá trị. Anh đã thấy hai toán tử JavaScript rồi: `=` và `*`.

Toán tử `*` thực hiện phép nhân. Quá đơn giản, phải không?

Toán tử `=` là toán tử *gán* - ta tính giá trị ở phía *bên phải* (giá trị nguồn) của dấu `=`, rồi gán nó vào biến được chỉ định ở phía *bên trái* (biến đích).

**Cảnh báo:** Cách viết này có thể hơi ngược với suy nghĩ ban đầu. Thay vì `a = 42`, một số người có thể thích viết theo cách giá trị nguồn bên trái và biến đích bên phải, như `42 -> a` (cách này *không hợp lệ* trong JavaScript!). Tuy nhiên, cú pháp theo kiểu `a = 42` và những biến thể tương tự, lại rất phổ biến trong các ngôn ngữ lập trình hiện đại. Nếu anh thấy cách này không tự nhiên, hãy luyện tập một thời gian để quen dần với thứ tự đó.

Xem ví dụ sau:

```js
a = 2;
b = a + 1;
```

Ở đây, chúng ta gán giá trị `2` cho biến `a`. Sau đó, lấy giá trị của biến `a` (vẫn là `2`), cộng thêm `1` thành `3`, rồi gán giá trị đó vào biến `b`.

Mặc dù không phải là một toán tử về mặt kỹ thuật, nhưng anh sẽ cần dùng từ khóa `var` trong mọi chương trình, vì đây là cách chính để *khai báo* (hay *tạo mới*) các *biến* (xem phần “Biến”).

Anh nên luôn luôn khai báo tên biến trước khi sử dụng nó. Tuy nhiên, anh chỉ cần khai báo biến một lần trong mỗi *phạm vi* (xem phần “Phạm vi”); sau đó có thể sử dụng lại bao nhiêu lần cũng được. Ví dụ:

```js
var a = 20;

a = a + 1;
a = a * 2;

console.log( a );	// 42
```

Dưới đây là một số toán tử phổ biến nhất trong JavaScript:

* **Gán giá trị:** `=` như trong `a = 2`.
* **Toán học:** `+` (cộng), `-` (trừ), `*` (nhân), và `/` (chia), ví dụ `a * 3`.
* **Gán kết hợp:** `+=`, `-=`, `*=`, `/=` là các toán tử kết hợp phép toán và phép gán, ví dụ `a += 2` (tương đương với `a = a + 2`).
* **Tăng/Giảm:** `++` (tăng), `--` (giảm), như trong `a++` (tương đương `a = a + 1`).
* **Truy cập thuộc tính đối tượng:** `.` như trong `console.log()`.

  Đối tượng là các giá trị có thể chứa các giá trị khác tại những vị trí được đặt tên gọi là thuộc tính. `obj.a` nghĩa là một đối tượng tên `obj` có thuộc tính tên là `a`. Thuộc tính cũng có thể được truy cập bằng cú pháp `obj["a"]`. Xem chương 2.
* **So sánh bằng:** `==` (so sánh bằng lỏng lẻo), `===` (so sánh bằng nghiêm ngặt), `!=` (khác lỏng lẻo), `!==` (khác nghiêm ngặt), ví dụ `a == b`.

  Xem phần “Giá trị & Kiểu dữ liệu” và chương 2.
* **So sánh:** `<` (nhỏ hơn), `>` (lớn hơn), `<=` (nhỏ hơn hoặc bằng), `>=` (lớn hơn hoặc bằng), như trong `a <= b`.

  Xem phần “Giá trị & Kiểu dữ liệu” và chương 2.
* **Logic:** `&&` (và), `||` (hoặc), như trong `a || b` nghĩa là chọn `a` *hoặc* `b`.

  Các toán tử này dùng để diễn tả điều kiện phức hợp (xem phần “Điều kiện”), như khi `a` *hoặc* `b` đúng.

**Ghi chú:** Để tìm hiểu chi tiết hơn và xem các toán tử chưa được đề cập ở đây, hãy tham khảo bài viết "Biểu thức và toán tử" trên Mozilla Developer Network (MDN): [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions\_and\_Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators).

---

## Giá trị & Kiểu dữ liệu

Giả sử anh hỏi nhân viên bán điện thoại về giá một chiếc điện thoại, và họ nói “chín mươi chín chín mươi chín” (tức là $99.99), thì họ đang đưa cho anh một con số thật sự đại diện cho số tiền cần trả (chưa tính thuế). Nếu anh muốn mua hai cái, anh dễ dàng nhân đôi con số đó để ra $199.98.

Nhưng nếu nhân viên đó chỉ một cái điện thoại khác và nói là “miễn phí” (có thể còn làm dấu ngoặc tay), thì họ không đưa ra con số, mà là một cách biểu diễn khác cho cái giá bạn mong đợi ($0.00) - đó là từ "miễn phí".

Khi anh hỏi liệu điện thoại có kèm theo sạc không, thì câu trả lời chỉ có thể là “có” hoặc “không”.

Tương tự như vậy, khi biểu diễn các giá trị trong một chương trình, ta chọn những cách biểu diễn khác nhau tùy vào mục đích sử dụng.

Những cách biểu diễn giá trị khác nhau này được gọi là *kiểu dữ liệu* trong lập trình. JavaScript có các kiểu dữ liệu tích hợp sẵn cho những giá trị cơ bản như sau:

* Khi cần thực hiện phép toán, anh dùng kiểu `number`.
* Khi cần in một giá trị ra màn hình, anh dùng kiểu `string` (chuỗi ký tự, từ, câu).
* Khi cần đưa ra quyết định trong chương trình, anh dùng kiểu `boolean` (`true` hoặc `false`).

Giá trị được viết trực tiếp trong mã nguồn gọi là *giá trị gốc* (literal). Giá trị `string` gốc được đặt trong dấu nháy kép `"..."` hoặc nháy đơn `'...'` - khác biệt chỉ là theo sở thích. Giá trị `number` và `boolean` gốc được viết trực tiếp (ví dụ `42`, `true`, v.v.).

Xem ví dụ sau:

```js
"I am a string";
'I am also a string';

42;

true;
false;
```

Ngoài các kiểu giá trị như `string` / `number` / `boolean`, các ngôn ngữ lập trình thường cung cấp *mảng* (arrays), *đối tượng* (objects), *hàm* (functions), và nhiều kiểu khác. Chúng ta sẽ tìm hiểu thêm về giá trị và kiểu dữ liệu trong chương này và chương tiếp theo.

### Chuyển đổi giữa các kiểu

Nếu bạn có một `number` nhưng cần in nó ra màn hình, bạn cần chuyển giá trị đó sang kiểu `string`, và trong JavaScript, quá trình chuyển đổi này được gọi là "ép kiểu". Tương tự, nếu ai đó nhập một chuỗi ký tự số vào một biểu mẫu trên trang thương mại điện tử, đó là một giá trị `string`, nhưng nếu bạn cần dùng giá trị đó để thực hiện tính toán, bạn phải *ép kiểu* nó sang `number`.

JavaScript cung cấp một số cách khác nhau để ép kiểu giữa các loại dữ liệu. Ví dụ:

```js
var a = "42";
var b = Number(a);

console.log(a); // "42"
console.log(b); // 42
```

Việc dùng `Number(..)` (một hàm dựng sẵn) như trên là ép kiểu *tường minh* từ kiểu khác sang kiểu `number`. Điều này khá dễ hiểu.

Tuy nhiên, một chủ đề gây tranh cãi là chuyện gì xảy ra khi bạn so sánh hai giá trị có kiểu khác nhau, điều này đòi hỏi ép kiểu *ngầm định*.

Khi so sánh chuỗi `"99.99"` với số `99.99`, hầu hết mọi người đều đồng ý rằng chúng tương đương. Nhưng thực ra chúng không hoàn toàn giống nhau, đúng không? Đó là cùng một giá trị nhưng ở hai dạng biểu diễn khác nhau, hai *kiểu dữ liệu* khác nhau. Có thể nói rằng chúng "bằng nhau một cách lỏng lẻo", phải không?

Để hỗ trợ trong những tình huống như vậy, JavaScript đôi khi sẽ *tự động ép* hai giá trị về cùng kiểu phù hợp.

Vì vậy, nếu bạn dùng toán tử `==` (so sánh bằng lỏng lẻo) để so sánh `"99.99" == 99.99`, JavaScript sẽ chuyển `"99.99"` sang số `99.99`. So sánh lúc đó trở thành `99.99 == 99.99`, và tất nhiên kết quả sẽ là `true`.

Dù được thiết kế để giúp lập trình viên, nhưng ép kiểu ngầm định có thể gây nhầm lẫn nếu bạn không dành thời gian học rõ các quy tắc điều khiển hành vi của nó. Phần lớn lập trình viên JavaScript chưa từng làm điều đó, vì vậy họ cảm thấy việc ép kiểu ngầm định khó hiểu, dễ gây lỗi không lường trước và nên tránh. Thậm chí nó còn đôi khi bị xem là một lỗi thiết kế của ngôn ngữ.

Tuy nhiên, ép kiểu ngầm định là một cơ chế *hoàn toàn có thể học được*, và hơn nữa *nên học* nếu bạn muốn lập trình JavaScript một cách nghiêm túc. Một khi bạn đã nắm được các quy tắc, nó không còn khó hiểu nữa, mà còn giúp mã của bạn gọn gàng và hiệu quả hơn! Việc bỏ công học sẽ rất xứng đáng.

**Ghi chú:** Để tìm hiểu thêm về ép kiểu, hãy xem chương 2 của cuốn sách này và chương 4 của cuốn *Kiểu và ngữ pháp* trong cùng bộ sách.

## mã Comments

The phone store employee might jot down some notes on the features of a newly released phone or on the new plans her company offers. These notes are only for the employee -- they're not for customers to read. Nevertheless, these notes help the employee do her job better by documenting the hows and whys of what she should tell customers.

One of the most important lessons you can learn about writing mã is that it's not just for the computer. mã is every bit as much, if not more, for the developer as it is for the compiler.

Your computer only cares about machine mã, a series of binary 0s and 1s, that comes from *compilation*. There's a nearly infinite number of programs you could write that yield the same series of 0s and 1s. The choices you make about how to write your program matter -- not only to you, but to your other team members and even to your future self.

You should strive not just to write programs that work correctly, but programs that make sense when examined. You can go a long way in that effort by choosing good names for your variables (see "Variables") and functions (see "Functions").

But another important part is mã comments. These are bits of text in your program that are inserted purely to explain things to a human. The interpreter/compiler will always ignore these comments.

There are lots of opinions on what makes well-commented mã; we can't really define absolute universal rules. But some observations and guidelines are quite useful:

* mã without comments is suboptimal.
* Too many comments (one per line, for example) is probably a sign of poorly written mã.
* Comments should explain *why*, not *what*. They can optionally explain *how* if that's particularly confusing.

In JavaScript, there are two types of comments possible: a single-line comment and a multiline comment.

Consider:

```js
// This is a single-line comment

/* But this is
       a multiline
             comment.
                      */
```

The `//` single-line comment is appropriate if you're going to put a comment right above a single statement, or even at the end of a line. Everything on the line after the `//` is treated as the comment (and thus ignored by the compiler), all the way to the end of the line. There's no restriction to what can appear inside a single-line comment.

Consider:

```js
var a = 42;		// 42 is the meaning of life
```

The `/* .. */` multiline comment is appropriate if you have several lines worth of explanation to make in your comment.

Here's a common usage of multiline comments:

```js
/* The following value is used because
   it has been shown that it answers
   every question in the universe. */
var a = 42;
```

It can also appear anywhere on a line, even in the middle of a line, because the `*/` ends it. For example:

```js
var a = /* arbitrary value */ 42;

console.log( a );	// 42
```

The only thing that cannot appear inside a multiline comment is a `*/`, because that would be interpreted to end the comment.

You will definitely want to begin your learning of programming by starting off with the habit of commenting mã. Throughout the rest of this chapter, you'll see I use comments to explain things, so do the same in your own practice. Trust me, everyone who reads your mã will thank you!

## Variables

Most useful programs need to track a value as it changes over the course of the program, undergoing different operations as called for by your program's intended tasks.

The easiest way to go about that in your program is to assign a value to a symbolic container, called a *variable* -- so called because the value in this container can *vary* over time as needed.

In some programming languages, you declare a variable (container) to hold a specific type of value, such as `number` or `string`. *Static typing*, otherwise known as *type enforcement*, is typically cited as a benefit for program correctness by preventing unintended value conversions.

Other languages emphasize types for values instead of variables. *Weak typing*, otherwise known as *dynamic typing*, allows a variable to hold any type of value at any time. It's typically cited as a benefit for program flexibility by allowing a single variable to represent a value no matter what type form that value may take at any given moment in the program's logic flow.

JavaScript uses the latter approach, *dynamic typing*, meaning variables can hold values of any *type* without any *type* enforcement.

As mentioned earlier, we declare a variable using the `var` statement -- notice there's no other *type* information in the declaration. Consider this simple program:

```js
var amount = 99.99;

amount = amount * 2;

console.log( amount );		// 199.98

// convert `amount` to a string, and
// add "$" on the beginning
amount = "$" + String( amount );

console.log( amount );		// "$199.98"
```

The `amount` variable starts out holding the number `99.99`, and then holds the `number` result of `amount * 2`, which is `199.98`.

The first `console.log(..)` command has to *implicitly* coerce that `number` value to a `string` to print it out.

Then the statement `amount = "$" + String(amount)` *explicitly* coerces the `199.98` value to a `string` and adds a `"$"` character to the beginning. At this point, `amount` now holds the `string` value `"$199.98"`, so the second `console.log(..)` statement doesn't need to do any coercion to print it out.

JavaScript developers will note the flexibility of using the `amount` variable for each of the `99.99`, `199.98`, and the `"$199.98"` values. Static-typing enthusiasts would prefer a separate variable like `amountStr` to hold the final `"$199.98"` representation of the value, because it's a different type.

Either way, you'll note that `amount` holds a running value that changes over the course of the program, illustrating the primary purpose of variables: managing program *state*.

In other words, *state* is tracking the changes to values as your program runs.

Another common usage of variables is for centralizing value setting. This is more typically called *constants*, when you declare a variable with a value and intend for that value to *not change* throughout the program.

You declare these *constants*, often at the top of a program, so that it's convenient for you to have one place to go to alter a value if you need to. By convention, JavaScript variables as constants are usually capitalized, with underscores `_` between multiple words.

Here's a silly example:

```js
var TAX_RATE = 0.08;	// 8% sales tax

var amount = 99.99;

amount = amount * 2;

amount = amount + (amount * TAX_RATE);

console.log( amount );				// 215.9784
console.log( amount.toFixed( 2 ) );	// "215.98"
```

**Note:** Similar to how `console.log(..)` is a function `log(..)` accessed as an object property on the `console` value, `toFixed(..)` here is a function that can be accessed on `number` values. JavaScript `number`s aren't automatically formatted for dollars -- the engine doesn't know what your intent is and there's no type for currency. `toFixed(..)` lets us specify how many decimal places we'd like the `number` rounded to, and it produces the `string` as necessary.

The `TAX_RATE` variable is only *constant* by convention -- there's nothing special in this program that prevents it from being changed. But if the city raises the sales tax rate to 9%, we can still easily update our program by setting the `TAX_RATE` assigned value to `0.09` in one place, instead of finding many occurrences of the value `0.08` strewn throughout the program and updating all of them.

The newest version of JavaScript at the time of this writing (commonly called "ES6") includes a new way to declare *constants*, by using `const` instead of `var`:

```js
// as of ES6:
const TAX_RATE = 0.08;

var amount = 99.99;

// ..
```

Constants are useful just like variables with unchanged values, except that constants also prevent accidentally changing value somewhere else after the initial setting. If you tried to assign any different value to `TAX_RATE` after that first declaration, your program would reject the change (and in strict mode, fail with an error -- see "Strict Mode" in Chapter 2).

By the way, that kind of "protection" against mistakes is similar to the static-typing type enforcement, so you can see why static types in other languages can be attractive!

**Note:** For more information about how different values in variables can be used in your programs, see the *Types & Grammar* title of this series.

## Blocks

The phone store employee must go through a series of steps to complete the checkout as you buy your new phone.

Similarly, in mã we often need to group a series of statements together, which we often call a *block*. In JavaScript, a block is defined by wrapping one or more statements inside a curly-brace pair `{ .. }`. Consider:

```js
var amount = 99.99;

// a general block
{
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```

This kind of standalone `{ .. }` general block is valid, but isn't as commonly seen in JS programs. Typically, blocks are attached to some other control statement, such as an `if` statement (see "Conditionals") or a loop (see "Loops"). For example:

```js
var amount = 99.99;

// is amount big enough?
if (amount > 10) {			// <-- block attached to `if`
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```

We'll explain `if` statements in the next section, but as you can see, the `{ .. }` block with its two statements is attached to `if (amount > 10)`; the statements inside the block will only be processed if the conditional passes.

**Note:** Unlike most other statements like `console.log(amount);`, a block statement does not need a semicolon (`;`) to conclude it.

## Conditionals

"Do you want to add on the extra screen protectors to your purchase, for $9.99?" The helpful phone store employee has asked you to make a decision. And you may need to first consult the current *state* of your wallet or bank account to answer that question. But obviously, this is just a simple "yes or no" question.

There are quite a few ways we can express *conditionals* (aka decisions) in our programs.

The most common one is the `if` statement. Essentially, you're saying, "*If* this condition is true, do the following...". For example:

```js
var bank_balance = 302.13;
var amount = 99.99;

if (amount < bank_balance) {
	console.log( "I want to buy this phone!" );
}
```

The `if` statement requires an expression in between the parentheses `( )` that can be treated as either `true` or `false`. In this program, we provided the expression `amount < bank_balance`, which indeed will either evaluate to `true` or `false` depending on the amount in the `bank_balance` variable.

You can even provide an alternative if the condition isn't true, called an `else` clause. Consider:

```js
const ACCESSORY_PRICE = 9.99;

var bank_balance = 302.13;
var amount = 99.99;

amount = amount * 2;

// can we afford the extra purchase?
if ( amount < bank_balance ) {
	console.log( "I'll take the accessory!" );
	amount = amount + ACCESSORY_PRICE;
}
// otherwise:
else {
	console.log( "No, thanks." );
}
```

Here, if `amount < bank_balance` is `true`, we'll print out `"I'll take the accessory!"` and add the `9.99` to our `amount` variable. Otherwise, the `else` clause says we'll just politely respond with `"No, thanks."` and leave `amount` unchanged.

As we discussed in "Values & Types" earlier, values that aren't already of an expected type are often coerced to that type. The `if` statement expects a `boolean`, but if you pass it something that's not already `boolean`, coercion will occur.

JavaScript defines a list of specific values that are considered "falsy" because when coerced to a `boolean`, they become `false` -- these include values like `0` and `""`. Any other value not on the "falsy" list is automatically "truthy" -- when coerced to a `boolean` they become `true`. Truthy values include things like `99.99` and `"free"`. See "Truthy & Falsy" in Chapter 2 for more information.

*Conditionals* exist in other forms besides the `if`. For example, the `switch` statement can be used as a shorthand for a series of `if..else` statements (see Chapter 2). Loops (see "Loops") use a *conditional* to determine if the loop should keep going or stop.

**Note:** For deeper information about the coercions that can occur implicitly in the test expressions of *conditionals*, see Chapter 4 of the *Types & Grammar* title of this series.

## Loops

During busy times, there's a waiting list for customers who need to speak to the phone store employee. While there's still people on that list, she just needs to keep serving the next customer.

Repeating a set of actions until a certain condition fails -- in other words, repeating only while the condition holds -- is the job of programming loops; loops can take different forms, but they all satisfy this basic behavior.

A loop includes the test condition as well as a block (typically as `{ .. }`). Each time the loop block executes, that's called an *iteration*.

For example, the `while` loop and the `do..while` loop forms illustrate the concept of repeating a block of statements until a condition no longer evaluates to `true`:

```js
while (numOfCustomers > 0) {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
}

// versus:

do {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
} while (numOfCustomers > 0);
```

The only practical difference between these loops is whether the conditional is tested before the first iteration (`while`) or after the first iteration (`do..while`).

In either form, if the conditional tests as `false`, the next iteration will not run. That means if the condition is initially `false`, a `while` loop will never run, but a `do..while` loop will run just the first time.

Sometimes you are looping for the intended purpose of counting a certain set of numbers, like from `0` to `9` (ten numbers). You can do that by setting a loop iteration variable like `i` at value `0` and incrementing it by `1` each iteration.

**Warning:** For a variety of historical reasons, programming languages almost always count things in a zero-based fashion, meaning starting with `0` instead of `1`. If you're not familiar with that mode of thinking, it can be quite confusing at first. Take some time to practice counting starting with `0` to become more comfortable with it!

The conditional is tested on each iteration, much as if there is an implied `if` statement inside the loop.

We can use JavaScript's `break` statement to stop a loop. Also, we can observe that it's awfully easy to create a loop that would otherwise run forever without a `break`ing mechanism.

Let's illustrate:

```js
var i = 0;

// a `while..true` loop would run forever, right?
while (true) {
	// stop the loop?
	if ((i <= 9) === false) {
		break;
	}

	console.log( i );
	i = i + 1;
}
// 0 1 2 3 4 5 6 7 8 9
```

**Warning:** This is not necessarily a practical form you'd want to use for your loops. It's presented here for illustration purposes only.

While a `while` (or `do..while`) can accomplish the task manually, there's another syntactic form called a `for` loop for just that purpose:

```js
for (var i = 0; i <= 9; i = i + 1) {
	console.log( i );
}
// 0 1 2 3 4 5 6 7 8 9
```

As you can see, in both cases the conditional `i <= 9` is `true` for the first 10 iterations (`i` of values `0` through `9`) of either loop form, but becomes `false` once `i` is value `10`.

The `for` loop has three clauses: the initialization clause (`var i=0`), the conditional test clause (`i <= 9`), and the update clause (`i = i + 1`). So if you're going to do counting with your loop iterations, `for` is a more compact and often easier form to understand and write.

There are other specialized loop forms that are intended to iterate over specific values, such as the properties of an object (see Chapter 2) where the implied conditional test is just whether all the properties have been processed. The "loop until a condition fails" concept holds no matter what the form of the loop.

## Functions

The phone store employee probably doesn't carry around a calculator to figure out the taxes and final purchase amount. That's a task she needs to define once and reuse over and over again. Odds are, the company has a checkout register (computer, tablet, etc.) with those "functions" built in.

Similarly, your program will almost certainly want to break up the mã's tasks into reusable pieces, instead of repeatedly repeating yourself repetitiously (pun intended!). The way to do this is to define a `function`.

A function is generally a named section of mã that can be "called" by name, and the mã inside it will be run each time. Consider:

```js
function printAmount() {
	console.log( amount.toFixed( 2 ) );
}

var amount = 99.99;

printAmount(); // "99.99"

amount = amount * 2;

printAmount(); // "199.98"
```

Functions can optionally take arguments (aka parameters) -- values you pass in. And they can also optionally return a value back.

```js
function printAmount(amt) {
	console.log( amt.toFixed( 2 ) );
}

function formatAmount() {
	return "$" + amount.toFixed( 2 );
}

var amount = 99.99;

printAmount( amount * 2 );		// "199.98"

amount = formatAmount();
console.log( amount );			// "$99.99"
```

The function `printAmount(..)` takes a parameter that we call `amt`. The function `formatAmount()` returns a value. Of course, you can also combine those two techniques in the same function.

Functions are often used for mã that you plan to call multiple times, but they can also be useful just to organize related bits of mã into named collections, even if you only plan to call them once.

Consider:

```js
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// calculate the new amount with the tax
	amt = amt + (amt * TAX_RATE);

	// return the new amount
	return amt;
}

var amount = 99.99;

amount = calculateFinalPurchaseAmount( amount );

console.log( amount.toFixed( 2 ) );		// "107.99"
```

Although `calculateFinalPurchaseAmount(..)` is only called once, organizing its behavior into a separate named function makes the mã that uses its logic (the `amount = calculateFinal...` statement) cleaner. If the function had more statements in it, the benefits would be even more pronounced.

### Scope

If you ask the phone store employee for a phone model that her store doesn't carry, she will not be able to sell you the phone you want. She only has access to the phones in her store's inventory. You'll have to try another store to see if you can find the phone you're looking for.

Programming has a term for this concept: *scope* (technically called *lexical scope*). In JavaScript, each function gets its own scope. Scope is basically a collection of variables as well as the rules for how those variables are accessed by name. Only mã inside that function can access that function's *scoped* variables.

A variable name has to be unique within the same scope -- there can't be two different `a` variables sitting right next to each other. But the same variable name `a` could appear in different scopes.

```js
function one() {
	// this `a` only belongs to the `one()` function
	var a = 1;
	console.log( a );
}

function two() {
	// this `a` only belongs to the `two()` function
	var a = 2;
	console.log( a );
}

one();		// 1
two();		// 2
```

Also, a scope can be nested inside another scope, just like if a clown at a birthday party blows up one balloon inside another balloon. If one scope is nested inside another, mã inside the innermost scope can access variables from either scope.

Consider:

```js
function outer() {
	var a = 1;

	function inner() {
		var b = 2;

		// we can access both `a` and `b` here
		console.log( a + b );	// 3
	}

	inner();

	// we can only access `a` here
	console.log( a );			// 1
}

outer();
```

Lexical scope rules say that mã in one scope can access variables of either that scope or any scope outside of it.

So, mã inside the `inner()` function has access to both variables `a` and `b`, but mã in `outer()` has access only to `a` -- it cannot access `b` because that variable is only inside `inner()`.

Recall this mã snippet from earlier:

```js
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// calculate the new amount with the tax
	amt = amt + (amt * TAX_RATE);

	// return the new amount
	return amt;
}
```

The `TAX_RATE` constant (variable) is accessible from inside the `calculateFinalPurchaseAmount(..)` function, even though we didn't pass it in, because of lexical scope.

**Note:** For more information about lexical scope, see the first three chapters of the *Scope & Closures* title of this series.

## Practice

There is absolutely no substitute for practice in learning programming. No amount of articulate writing on my part is alone going to make you a programmer.

With that in mind, let's try practicing some of the concepts we learned here in this chapter. I'll give the "requirements," and you try it first. Then consult the mã listing below to see how I approached it.

* Write a program to calculate the total price of your phone purchase. You will keep purchasing phones (hint: loop!) until you run out of money in your bank account. You'll also buy accessories for each phone as long as your purchase amount is below your mental spending threshold.
* After you've calculated your purchase amount, add in the tax, then print out the calculated purchase amount, properly formatted.
* Finally, check the amount against your bank account balance to see if you can afford it or not.
* You should set up some constants for the "tax rate," "phone price," "accessory price," and "spending threshold," as well as a variable for your "bank account balance.""
* You should define functions for calculating the tax and for formatting the price with a "$" and rounding to two decimal places.
* **Bonus Challenge:** Try to incorporate input into this program, perhaps with the `prompt(..)` covered in "Input" earlier. You may prompt the user for their bank account balance, for example. Have fun and be creative!

OK, go ahead. Try it. Don't peek at my mã listing until you've given it a shot yourself!

**Note:** Because this is a JavaScript book, I'm obviously going to solve the practice exercise in JavaScript. But you can do it in another language for now if you feel more comfortable.

Here's my JavaScript solution for this exercise:

```js
const SPENDING_THRESHOLD = 200;
const TAX_RATE = 0.08;
const PHONE_PRICE = 99.99;
const ACCESSORY_PRICE = 9.99;

var bank_balance = 303.91;
var amount = 0;

function calculateTax(amount) {
	return amount * TAX_RATE;
}

function formatAmount(amount) {
	return "$" + amount.toFixed( 2 );
}

// keep buying phones while you still have money
while (amount < bank_balance) {
	// buy a new phone!
	amount = amount + PHONE_PRICE;

	// can we afford the accessory?
	if (amount < SPENDING_THRESHOLD) {
		amount = amount + ACCESSORY_PRICE;
	}
}

// don't forget to pay the government, too
amount = amount + calculateTax( amount );

console.log(
	"Your purchase: " + formatAmount( amount )
);
// Your purchase: $334.76

// can you actually afford this purchase?
if (amount > bank_balance) {
	console.log(
		"You can't afford this purchase. :("
	);
}
// You can't afford this purchase. :(
```

**Note:** The simplest way to run this JavaScript program is to type it into the developer console of your nearest browser.

How did you do? It wouldn't hurt to try it again now that you've seen my mã. And play around with changing some of the constants to see how the program runs with different values.

## Review

Learning programming doesn't have to be a complex and overwhelming process. There are just a few basic concepts you need to wrap your head around.

These act like building blocks. To build a tall tower, you start first by putting block on top of block on top of block. The same goes with programming. Here are some of the essential programming building blocks:

* You need *operators* to perform actions on values.
* You need values and *types* to perform different kinds of actions like math on `number`s or output with `string`s.
* You need *variables* to store data (aka *state*) during your program's execution.
* You need *conditionals* like `if` statements to make decisions.
* You need *loops* to repeat tasks until a condition stops being true.
* You need *functions* to organize your mã into logical and reusable chunks.

mã comments are one effective way to write more readable mã, which makes your program easier to understand, maintain, and fix later if there are problems.

Finally, don't neglect the power of practice. The best way to learn how to write mã is to write mã.

I'm excited you're well on your way to learning how to mã, now! Keep it up. Don't forget to check out other beginner programming resources (books, blogs, online training, etc.). This chapter and this book are a great start, but they're just a brief introduction.

The next chapter will review many of the concepts from this chapter, but from a more JavaScript-specific perspective, which will highlight most of the major topics that are addressed in deeper detail throughout the rest of the series.
