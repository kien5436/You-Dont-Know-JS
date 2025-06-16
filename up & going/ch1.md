# Bạn không hiểu JS: Khởi động và tiến lên
# Chương 1: Bước vào con đường lập trình

Chào mừng bạn đến với loạt sách *Bạn không hiểu JS* (*BKHJS*).

Quyển *Khởi động và tiến lên* là phần giới thiệu về một số khái niệm cơ bản trong lập trình - tất nhiên có xu hướng thiên về JavaScript (thường viết tắt là JS) - và cách tiếp cận cũng như hiểu các đầu sách còn lại trong loạt này. Đặc biệt nếu bạn mới bắt đầu học lập trình và/hoặc JavaScript, quyển sách này sẽ lướt qua những gì bạn cần để bắt đầu *lên đường*.

Sách bắt đầu bằng việc giải thích các nguyên lý lập trình cơ bản ở mức rất tổng quan. Nó chủ yếu dành cho những ai bắt đầu *BKHJS* mà chưa có (hoặc gần như chưa có) kinh nghiệm lập trình, và muốn dùng loạt sách này như một con đường để hiểu lập trình thông qua lăng kính JavaScript.

Chương 1 nên được tiếp cận như một cái nhìn khái quát về những thứ bạn muốn học và làm để *bắt đầu lập trình*. Cũng có nhiều tài nguyên tuyệt vời khác để nhập môn lập trình giúp bạn đào sâu hơn vào các chủ đề này, và tôi khuyến khích bạn học thêm từ chúng bên cạnh chương này.

Khi đã thấy thoải mái với các kiến thức lập trình cơ bản, chương 2 sẽ giúp bạn làm quen với phong cách lập trình của JavaScript. Chương này giới thiệu về JavaScript là gì, nhưng một lần nữa, nó không phải là hướng dẫn toàn diện - đó là nhiệm vụ của những quyển *BKHJS* còn lại!

Nếu bạn đã khá thoải mái với JavaScript, hãy đọc thử chương 3 để có cái nhìn thoáng qua về những gì *BKHJS* sẽ mang lại, rồi bắt đầu ngay!

## Mã

Chúng ta hãy bắt đầu từ những điều cơ bản.

Một chương trình, thường được gọi là *mã nguồn* hoặc đơn giản là *mã*, là một tập hợp các chỉ dẫn đặc biệt để nói cho máy tính biết cần thực hiện những tác vụ nào. Thông thường, mã được lưu trong một tập tin văn bản, mặc dù với JavaScript, bạn cũng có thể gõ trực tiếp vào bảng điều khiển dành cho nhà phát triển trong chương trình duyệt, thứ mà chúng ta sẽ bàn ngay sau đây.

Các quy tắc về định dạng hợp lệ và cách kết hợp các chỉ dẫn được gọi là *ngôn ngữ máy tính*, đôi khi còn được gọi là *cú pháp*, tương tự như tiếng bạn có các quy tắc về cách viết đúng chính tả và cách tạo câu hợp lệ bằng từ vựng và dấu câu.

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

Một câu lệnh biểu thức phổ biến hơn là *câu lệnh gọi hàm* (xem phần "Hàm"), vì toàn bộ câu lệnh chính là một biểu thức gọi hàm:

```js
alert( a );
```

### Thực thi một chương trình

Làm cách nào mà những tập hợp các câu lệnh lập trình hướng dẫn được cho máy tính phải làm gì? Chương trình cần được *thực thi*, hay còn gọi là *chạy chương trình*.

Những câu lệnh như `a = b * 2` rất hữu ích với lập trình viên trong quá trình đọc và viết mã, nhưng không thực sự ở dạng mà máy tính có thể hiểu trực tiếp. Vì vậy, cần có một công cụ đặc biệt trên máy tính (gọi là *chương trình thông dịch* hoặc *chương trình biên dịch*) để dịch mã bạn viết thành các lệnh mà máy tính hiểu được.

Với một số ngôn ngữ lập trình, việc dịch lệnh thường được thực hiện từ trên xuống dưới, hết dòng này đến dòng khác, mỗi khi chương trình được chạy, cách này thường được gọi là *thông dịch* mã.

Với các ngôn ngữ được dịch sẵn từ trước, gọi là *biên dịch* mã, vì vậy khi chương trình *chạy* sau đó, thứ thực sự chạy là những lệnh đã được biên dịch sẵn, sẵn sàng để thực thi.

Thông thường người ta cho rằng JavaScript là một ngôn ngữ *thông dịch*, vì mã nguồn JavaScript của bạn được xử lý mỗi lần nó chạy. Nhưng thực ra điều đó không hoàn toàn chính xác. Bộ máy JavaScript thực sự *biên dịch* chương trình ngay tại thời điểm chạy, rồi lập tức thực thi mã đã biên dịch đó.

**Lưu ý:** Để biết thêm về quá trình biên dịch trong JavaScript, hãy xem hai chương đầu tiên trong cuốn *Phạm vi và hàm đóng* của loạt sách này.

## Tự Thử Nghiệm

Chương này sẽ giới thiệu từng khái niệm lập trình với những đoạn mã đơn giản, tất cả đều được viết bằng JavaScript (tất nhiên rồi!).

Điều này không thể nhấn mạnh đủ: trong lúc bạn đọc chương này - và có thể bạn sẽ cần dành thời gian đọc lại nhiều lần - bạn nên thực hành từng khái niệm bằng cách tự gõ lại mã. Cách dễ nhất để làm điều đó là mở bảng điều khiển trong công cụ dành cho nhà phát triển của chương trình duyệt gần nhất (Firefox, Chrome, IE, v.v.).

**Mẹo:** Thông thường, bạn có thể mở bảng điều khiển bằng phím tắt hoặc thông qua bảng chọn. Để biết thêm chi tiết về cách mở và sử dụng bảng điều khiển trong chương trình duyệt yêu thích của mình, hãy xem bài viết "Làm chủ Bảng điều khiển Công cụ cho nhà phát triển" ([http://blog.teamtreehouse.com/mastering-developer-tools-console](http://blog.teamtreehouse.com/mastering-developer-tools-console)). Để gõ nhiều dòng mã cùng lúc trong bảng điều khiển, hãy dùng `<shift> + <enter>` để xuống dòng mới. Khi nhấn `<enter>`, bảng điều khiển sẽ thực thi toàn bộ những gì bạn vừa gõ.

Bây giờ hãy làm quen với quá trình chạy mã trong bảng điều khiển. Trước tiên, tôi đề xuất bạn mở một thẻ trống trong chương trình duyệt. Tôi thường làm điều đó bằng cách gõ `about:blank` vào thanh địa chỉ. Sau đó, đảm bảo rằng bảng điều khiển của bạn đã được mở như đã nói ở trên.

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

bạn có thể đã đoán được, nhưng đó chính là cách chúng ta in văn bản (hay còn gọi là *xuất* cho người dùng) trong bảng điều khiển dành cho nhà phát triển. Có hai đặc điểm của câu lệnh đó cần được giải thích.

Thứ nhất, phần `log(b)` được gọi là một lời gọi hàm (xem thêm phần "Hàm"). Điều đang diễn ra là chúng ta đưa biến `b` vào hàm đó, yêu cầu nó lấy giá trị của `b` và in ra bảng điều khiển.

Thứ hai, phần `console.` là một tham chiếu đến đối tượng nơi chứa hàm `log(..)`. Chúng ta sẽ tìm hiểu kỹ hơn về đối tượng và thuộc tính của chúng trong chương 2.

Một cách khác để tạo đầu ra mà bạn có thể nhìn thấy là dùng câu lệnh `alert(..)`. Ví dụ:

```js
alert( b );
```

Nếu chạy đoạn mã đó, bạn sẽ thấy thay vì in kết quả ra bảng điều khiển, chương trình duyệt sẽ hiển thị một hộp thoại với nút "OK" và nội dung là giá trị của biến `b`. Tuy nhiên, việc dùng `console.log(..)` sẽ giúp bạn học lập trình và chạy các chương trình dễ dàng hơn so với `alert(..)` vì bạn có thể xuất nhiều giá trị cùng lúc mà không làm gián đoạn giao diện chương trình duyệt.

Trong cuốn sách này, chúng ta sẽ sử dụng `console.log(..)` để hiển thị đầu ra.

### Đầu vào

Trong khi nói về việc xuất kết quả, chắc hẳn bạn cũng đang thắc mắc về *đầu vào* (ví dụ như nhận thông tin từ người dùng).

Cách phổ biến nhất là trang HTML hiển thị các thành phần biểu mẫu (chẳng hạn như ô nhập văn bản) để người dùng nhập vào, sau đó dùng JavaScript để đọc các giá trị đó vào biến trong chương trình.

Tuy nhiên, có một cách đơn giản hơn để nhận dữ liệu đầu vào trong các ví dụ học tập và trình diễn như cách mà bạn sẽ làm trong cuốn sách này: sử dụng hàm `prompt(..)`:

```js
age = prompt( "Hãy nhập tuổi của bạn:" );

console.log( age );
```

Như bạn có thể đoán, thông điệp mà bạn truyền vào `prompt(..)` - trong ví dụ này là `"Hãy nhập tuổi của bạn:"` - sẽ được hiển thị trong hộp thoại bật lên.

Nó sẽ trông giống như sau:

<img src="fig2.png" width="500">

Sau khi bạn nhập nội dung và nhấn "OK", giá trị bạn gõ vào sẽ được lưu vào biến `age`, rồi chúng ta *xuất* giá trị đó bằng `console.log(..)`:

<img src="fig3.png" width="500">

Để đơn giản hoá khi học các khái niệm lập trình cơ bản, các ví dụ trong cuốn sách này sẽ không yêu cầu nhập đầu vào. Nhưng bây giờ bạn đã biết cách dùng `prompt(..)`, nên nếu muốn thử thách bản thân, bạn có thể dùng thêm phần nhập liệu khi khám phá các ví dụ.

## Toán tử

Toán tử là cách chúng ta thực hiện các thao tác trên biến và giá trị. bạn đã thấy hai toán tử JavaScript rồi: `=` và `*`.

Toán tử `*` thực hiện phép nhân. Quá đơn giản, phải không?

Toán tử `=` là toán tử *gán* - ta tính giá trị ở phía *bên phải* (giá trị nguồn) của dấu `=`, rồi gán nó vào biến được chỉ định ở phía *bên trái* (biến đích).

**Cảnh báo:** Cách viết này có thể hơi ngược với suy nghĩ ban đầu. Thay vì `a = 42`, một số người có thể thích viết theo cách giá trị nguồn bên trái và biến đích bên phải, như `42 -> a` (cách này *không hợp lệ* trong JavaScript!). Tuy nhiên, cú pháp theo kiểu `a = 42` và những biến thể tương tự, lại rất phổ biến trong các ngôn ngữ lập trình hiện đại. Nếu bạn thấy cách này không tự nhiên, hãy luyện tập một thời gian để quen dần với thứ tự đó.

Xem ví dụ sau:

```js
a = 2;
b = a + 1;
```

Ở đây, chúng ta gán giá trị `2` cho biến `a`. Sau đó, lấy giá trị của biến `a` (vẫn là `2`), cộng thêm `1` thành `3`, rồi gán giá trị đó vào biến `b`.

Mặc dù không phải là một toán tử về mặt kỹ thuật, nhưng bạn sẽ cần dùng từ khóa `var` trong mọi chương trình, vì đây là cách chính để *khai báo* (hay *tạo mới*) các *biến* (xem phần "Biến").

bạn nên luôn luôn khai báo tên biến trước khi sử dụng nó. Tuy nhiên, bạn chỉ cần khai báo biến một lần trong mỗi *phạm vi* (xem phần "Phạm vi"); sau đó có thể sử dụng lại bao nhiêu lần cũng được. Ví dụ:

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

  Xem phần "Giá trị & Kiểu dữ liệu" và chương 2.
* **So sánh:** `<` (nhỏ hơn), `>` (lớn hơn), `<=` (nhỏ hơn hoặc bằng), `>=` (lớn hơn hoặc bằng), như trong `a <= b`.

  Xem phần "Giá trị & Kiểu dữ liệu" và chương 2.
* **Logic:** `&&` (và), `||` (hoặc), như trong `a || b` nghĩa là chọn `a` *hoặc* `b`.

  Các toán tử này dùng để diễn tả điều kiện phức hợp (xem phần "Điều kiện"), như khi `a` *hoặc* `b` đúng.

**Ghi chú:** Để tìm hiểu chi tiết hơn và xem các toán tử chưa được đề cập ở đây, hãy tham khảo bài viết "Biểu thức và toán tử" trên Mozilla Developer Network (MDN): [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions\_and\_Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators).

---

## Giá trị & Kiểu dữ liệu

Giả sử bạn hỏi nhân viên bán điện thoại về giá một chiếc điện thoại, và họ nói "chín mươi chín, chín mươi chín" (tức là $99.99), thì họ đang đưa cho bạn một con số thật sự đại diện cho số tiền cần trả (chưa tính thuế). Nếu bạn muốn mua hai cái, bạn dễ dàng nhân đôi con số đó để ra $199.98.

Nhưng nếu nhân viên đó chỉ một cái điện thoại khác và nói là "miễn phí" (có thể còn làm dấu ngoặc tay), thì họ không đưa ra con số, mà là một cách biểu diễn khác cho cái giá bạn mong đợi ($0.00) - đó là từ "miễn phí".

Khi bạn hỏi liệu điện thoại có kèm theo sạc không, thì câu trả lời chỉ có thể là "có" hoặc "không".

Tương tự như vậy, khi biểu diễn các giá trị trong một chương trình, ta chọn những cách biểu diễn khác nhau tùy vào mục đích sử dụng.

Những cách biểu diễn giá trị khác nhau này được gọi là *kiểu dữ liệu* trong lập trình. JavaScript có các kiểu dữ liệu tích hợp sẵn cho những giá trị cơ bản như sau:

* Khi cần thực hiện phép toán, bạn dùng kiểu `number`.
* Khi cần in một giá trị ra màn hình, bạn dùng kiểu `string` (chuỗi ký tự, từ, câu).
* Khi cần đưa ra quyết định trong chương trình, bạn dùng kiểu `boolean` (`true` hoặc `false`).

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

## Chú thích mã

Nhân viên cửa hàng điện thoại có thể ghi chú vài điều về tính năng của một mẫu điện thoại mới phát hành hoặc các kế hoạch mới mà công ty đang đề xuất. Những ghi chú này chỉ dành cho nhân viên - không phải để khách hàng xem. Tuy vậy, chúng giúp nhân viên làm việc tốt hơn bằng cách ghi lại cách thức và lý do của những gì họ nên nói với khách hàng.

Một trong những bài học quan trọng nhất bạn có thể học về việc viết mã là nó không chỉ dành cho máy tính. Mã quan trọng với lập trình viên không kém gì với chương trình biên dịch.

Máy tính của bạn chỉ quan tâm đến mã máy, một chuỗi các số nhị phân 0 và 1, được tạo ra từ quá trình *biên dịch*. Có gần như vô hạn cách viết chương trình khác nhau có thể tạo ra cùng một chuỗi 0 và 1 đó. Vì thế, cách bạn chọn viết mã rất quan trọng - không chỉ cho bạn, mà còn cho những thành viên khác trong nhóm và cho chính bạn trong tương lai.

Bạn không chỉ cần viết chương trình chạy đúng, mà còn cần viết sao cho dễ hiểu khi đọc lại. Cách đặt tên biến (xem phần "Biến") và hàm (xem phần "Hàm") là một phần quan trọng trong việc đó.

Nhưng một phần khác cũng rất quan trọng là chú thích mã. Đây là những dòng văn bản được chèn vào trong chương trình chỉ để giải thích cho con người. Chương trình thông dịch hoặc biên dịch sẽ luôn bỏ qua những chú thích này.

Có rất nhiều quan điểm khác nhau về cách viết chú thích mã tốt nhưng không có quy tắc tuyệt đối nào. Tuy nhiên, có một vài quan sát và hướng dẫn hữu ích như sau:

* Mã không có chú thích thì chưa tối ưu.
* Quá nhiều chú thích (ví dụ, mỗi dòng một chú thích) có thể là dấu hiệu của mã viết kém.
* Chú thích nên giải thích *tại sao*, không phải *cái gì*. Nếu cần thiết, cũng có thể giải thích *cách làm* nếu điều đó gây khó hiểu.

Trong JavaScript, có hai loại chú thích: chú thích một dòng và chú thích nhiều dòng.

Ví dụ:

```js
// Đây là chú thích một dòng

/* Còn đây là
   chú thích
   nhiều dòng. */
```

* Chú thích một dòng (`//`) thích hợp khi bạn viết chú thích ngay trên một câu lệnh, hoặc ở cuối dòng đó. Mọi thứ sau `//` đến hết dòng đều là chú thích (và vì thế sẽ bị chương trình biên dịch bỏ qua). Không có giới hạn gì về những thứ có thể xuất hiện bên trong chú thích một dòng.

```js
var a = 42;	// 42 là ý nghĩa của cuộc sống
```

* Chú thích nhiều dòng (`/* ... */`) phù hợp khi bạn cần nhiều dòng để giải thích.

Dưới đây là một cách dùng phổ biến của chú thích nhiều dòng:

```js
/* Giá trị sau được dùng vì
   người ta cho rằng nó là
   câu trả lời cho mọi câu hỏi trong vũ trụ. */
var a = 42;
```

Nó có thể xuất hiện ở bất kì vị trí nào trên một dòng, kể cả ở giữa dòng vì `*/` kết thúc nó. Ví dụ:

```js
var a = /* giá trị tuỳ ý */ 42;

console.log(a);  // 42
```

Thứ duy nhất không được chèn vào bên trong một chú thích nhiều dòng là `*/` vì nó sẽ kết thúc chú thích.

Bạn nên bắt đầu học lập trình với thói quen viết chú thích mã thường xuyên. Trong phần còn lại của chương này, tôi sẽ dùng chú thích để giải thích mã, bạn cũng nên làm như vậy trong thực tế. Tin tôi đi, những người đọc mã của bạn sau này sẽ biết ơn bạn!

## Biến

Hầu hết các chương trình hữu ích đều cần theo dõi một giá trị khi nó thay đổi trong suốt quá trình chạy chương trình, trải qua các phép xử lý khác nhau tùy theo nhiệm vụ mà chương trình cần thực hiện.

Cách đơn giản nhất để làm điều đó là gán giá trị vào một biểu tượng chứa có tên gọi là *biến* - gọi như vậy vì giá trị trong vùng chứa này có thể *biến đổi* theo thời gian khi cần thiết.

Trong một số ngôn ngữ lập trình, bạn khai báo một biến (vùng chứa) để giữ một loại giá trị cụ thể như `number` hoặc `string`. *Kiểu tĩnh* hay còn gọi là *kiểu ràng buộc*, thường được xem là có lợi cho độ đúng đắn của chương trình vì ngăn chặn việc chuyển đổi kiểu không chủ ý.

Một số ngôn ngữ khác lại tập trung vào kiểu của giá trị thay vì kiểu của biến. *Kiểu yếu* hay còn gọi là *kiểu động*, cho phép biến giữ bất kỳ loại giá trị nào tại bất kỳ thời điểm nào. Cách này thường được đánh giá cao vì sự linh hoạt khi cho phép một biến đại diện cho nhiều loại giá trị khác nhau tùy theo luồng logic của chương trình.

JavaScript chọn cách tiếp cận thứ hai, *kiểu động*, tức là các biến có thể giữ bất kỳ *loại* giá trị nào mà không bị *ràng buộc kiểu*.

Như đã nói trước đó, ta khai báo biến bằng lệnh `var` - lưu ý là không có thông tin về *kiểu* nào trong khai báo. Xem ví dụ đơn giản sau:

```js
var amount = 99.99;

amount = amount * 2;

console.log(amount);      // 199.98

// chuyển `amount` thành chuỗi và thêm dấu "$" vào đầu
amount = "$" + String(amount);

console.log(amount);      // "$199.98"
```

Biến `amount` ban đầu giữ giá trị số `99.99`, sau đó giữ kết quả phép nhân `amount * 2` là `199.98`.

Lệnh `console.log(..)` đầu tiên sẽ *ngầm* ép giá trị `number` đó thành `string` để in ra.

Sau đó, dòng `amount = "$" + String(amount)` *rõ ràng* ép `199.98` thành `string` và nối thêm dấu `$` vào đầu. Lúc này, `amount` giữ giá trị chuỗi `"$199.98"`, nên lệnh `console.log(..)` thứ hai không cần ép kiểu gì cả.

Các lập trình viên JavaScript sẽ thấy sự linh hoạt khi dùng biến `amount` cho cả `99.99`, `199.98` và `"$199.98"`. Những người chuộng kiểu tĩnh sẽ thích tạo một biến riêng như `amountStr` để giữ giá trị chuỗi `"$199.98"` vì đây là kiểu dữ liệu khác.

Dù chọn cách nào, bạn cũng sẽ thấy rằng `amount` đang giữ giá trị thay đổi theo thời gian, điều đó minh họa mục đích chính của biến: quản lý *trạng thái* chương trình.

Nói cách khác, *trạng thái* là việc theo dõi sự thay đổi của các giá trị trong quá trình chương trình chạy.

Một cách dùng phổ biến khác của biến là để tập trung việc thiết lập giá trị. Khi bạn khai báo một biến mà bạn *không muốn thay đổi* giá trị trong suốt chương trình, người ta gọi đó là *hằng số*.

Bạn khai báo các *hằng số* này, thường ở đầu chương trình, để tiện điều chỉnh khi cần thay vì phải sửa nhiều lần trong toàn bộ chương trình. Theo quy ước, biến đóng vai trò là hằng số trong JavaScript thường được viết in hoa, có dấu gạch dưới `_` giữa các từ.

Dưới đây là một ví dụ đơn giản:

```js
var TAX_RATE = 0.08; // thuế 8%

var amount = 99.99;

amount = amount * 2;

amount = amount + (amount * TAX_RATE);

console.log(amount);               // 215.9784
console.log(amount.toFixed(2));    // "215.98"
```

**Ghi chú:** Giống như `console.log(..)`, hàm `log(..)` được truy cập như thuộc tính của đối tượng `console` thì `toFixed(..)` là một hàm có thể gọi trên giá trị kiểu `number`. JavaScript không tự động định dạng số cho đô-la - bộ máy không biết ý định của bạn là gì và cũng không có kiểu dữ liệu riêng cho tiền tệ. Hàm `toFixed(..)` giúp chúng ta chỉ định số chữ số thập phân muốn làm tròn và trả về một `string`.

Biến `TAX_RATE` chỉ là *hằng số* theo quy ước - không có gì ngăn bạn thay đổi giá trị của nó trong chương trình cả. Nhưng nếu thành phố tăng thuế lên 9%, bạn có thể dễ dàng cập nhật chương trình bằng cách sửa một dòng `TAX_RATE = 0.09` thay vì phải sửa tất cả các chỗ có số `0.08`.

Phiên bản mới nhất của JavaScript tính đến thời điểm viết cuốn sách này (thường gọi là "ES6") đã thêm cách khai báo *hằng số* mới là dùng từ khóa `const` thay cho `var`:

```js
// kể từ ES6:
const TAX_RATE = 0.08;

var amount = 99.99;

// ...
```

Hằng số vẫn giống như biến nhưng thêm đặc điểm là chống lại việc thay đổi giá trị một cách vô tình sau khi đã khởi tạo. Nếu bạn cố gán giá trị mới cho `TAX_RATE` sau khi đã khai báo, chương trình sẽ từ chối thay đổi đó (và chế độ nghiêm ngặt sẽ báo lỗi - Xem phần "Chế độ nghiêm ngặt" trong chương 2).

Điều này cũng giống như cách mà kiểu dữ liệu tĩnh giúp ngăn lỗi, vì vậy bạn có thể thấy vì sao một số lập trình viên lại chuộng kiểu dữ liệu tĩnh hơn.

**Ghi chú:** Để tìm hiểu thêm về cách dùng biến với các kiểu giá trị khác nhau trong chương trình, hãy xem sách *Kiểu và ngữ pháp* thuộc cùng bộ sách này.

## Khối lệnh

Nhân viên cửa hàng điện thoại phải thực hiện một loạt các bước để hoàn tất việc thanh toán khi bạn mua một chiếc điện thoại mới.

Tương tự, trong lập trình, ta thường cần nhóm nhiều câu lệnh lại với nhau và gọi đó là một *khối lệnh*. Trong JavaScript, khối lệnh được xác định bằng cách đặt một hoặc nhiều câu lệnh trong cặp ngoặc nhọn `{ .. }`. Ví dụ:

```js
var amount = 99.99;

// một khối lệnh chung
{
	amount = amount * 2;
	console.log(amount);	// 199.98
}
```

Khối `{ .. }` độc lập như trên là hợp lệ, nhưng không quá phổ biến trong các chương trình JS. Thông thường, khối lệnh được gắn liền với một câu lệnh điều khiển, chẳng hạn như `if` (xem phần "Câu lệnh điều kiện") hoặc vòng lặp (xem phần "Vòng lặp"). Ví dụ:

```js
var amount = 99.99;

// amount có đủ lớn không?
if (amount > 10) {          // <-- khối gắn với `if`
	amount = amount * 2;
	console.log(amount);    // 199.98
}
```

Tôi sẽ giải thích lệnh `if` ở phần tiếp theo, nhưng bạn có thể thấy, khối `{ .. }` chứa hai câu lệnh mà chỉ được thực hiện nếu điều kiện `amount > 10` là đúng.

**Ghi chú:** Không giống hầu hết câu lệnh khác như `console.log(amount);`, một khối lệnh không cần dấu chấm phẩy (`;`) ở cuối.

## Câu lệnh điều kiện

"Bạn có muốn thêm miếng dán màn hình với giá 9.99 đô không?", nhân viên cửa hàng điện thoại đang chờ bạn đưa ra quyết định. Và có lẽ bạn cần tham khảo *tình trạng* cái ví hoặc tài khoản ngân hàng trước khi trả lời. Nhưng đó rất rõ ràng là một câu hỏi dạng "có hoặc không".

Trong lập trình, ta cũng có nhiều cách để thể hiện các *điều kiện* (hay quyết định).

Cách phổ biến nhất là dùng câu lệnh `if`. Cơ bản là: "*Nếu* điều kiện đúng thì làm điều sau đây...". Ví dụ:

```js
var bank_balance = 302.13;
var amount = 99.99;

if (amount < bank_balance) {
  console.log("Tôi muốn mua chiếc điện thoại này!");
}
```

Câu lệnh `if` cần một biểu thức trong dấu ngoặc tròn `( )`, biểu thức này có thể đánh giá thành `true` hoặc `false`. Trong chương trình trên, ta dùng biểu thức `amount < bank_balance` để trả về `true` hoặc `false` tùy theo số dư trong biến `bank_balance`.

Bạn cũng có thể thêm một lựa chọn khác nếu điều kiện không đúng, được gọi là mệnh đề `else`. Ví dụ:

```js
const ACCESSORY_PRICE = 9.99;

var bank_balance = 302.13;
var amount = 99.99;

amount = amount * 2;

// ta có đủ tiền cho món phụ kiện không?
if (amount < bank_balance) {
	console.log("Tôi sẽ mua món phụ kiện!");
	amount = amount + ACCESSORY_PRICE;
}
// nếu không:
else {
	console.log("Không, cảm ơn.");
}
```

Ở đây, nếu điều kiện `amount < bank_balance` đúng, chương trình sẽ in ra `"Tôi sẽ mua món phụ kiện!"` và cộng thêm `9.99` vào biến `amount`. Nếu không, mệnh đề `else` sẽ thực hiện `"Không, cảm ơn."` và không thay đổi gì nữa.

Như đã nói trong phần "Giá trị & Kiểu dữ liệu", các giá trị chưa thuộc kiểu mong muốn sẽ được ép về đúng dạng. Câu lệnh `if` cần một biểu thức `boolean` nhưng nếu bạn truyền vào một giá trị không phải `boolean`, ép kiểu sẽ xảy ra.

JavaScript định nghĩa một danh sách các giá trị được xem là "sai" bởi vì khi ép chúng sang `boolean` sẽ trở thành `false`, bao gồm: `0` và `""`. Mọi giá trị khác đều là "đúng" - sẽ trở thành `true` khi ép kiểu sang `boolean`. Ví dụ `99.99` hoặc `"free"` là các giá trị đúng. Xem phần "Đúng và sai" trong chương 2 để biết thêm thông tin chi tiết.

Câu lệnh điều kiện còn có các dạng khác ngoài `if`. Chẳng hạn `switch` có thể dùng thay cho nhiều câu lệnh `if..else` liên tiếp (xem chương 2). Vòng lặp (xem "Vòng lặp") cũng sử dụng điều kiện để xác định xem có tiếp tục lặp không.

**Ghi chú:** Để hiểu kỹ hơn về các trường hợp ép kiểu ngầm trong biểu thức điều kiện, xem chương 4 của cuốn *Kiểu và ngữ pháp* trong bộ sách này.

## Vòng lặp

Trong những lúc đông khách, sẽ có một danh sách những khách hàng chờ nói chuyện với nhân viên cửa hàng điện thoại. Miễn là còn người trong danh sách, cô ấy chỉ cần tiếp tục phục vụ khách tiếp theo.

Lặp lại một loạt hành động cho đến khi điều kiện không còn đúng - nói cách khác, chỉ lặp lại khi điều kiện vẫn khả thi - là nhiệm vụ của vòng lặp trong lập trình; vòng lặp có thể có nhiều dạng, nhưng tất cả đều tuân theo hành vi cơ bản này.

Một vòng lặp bao gồm điều kiện kiểm tra cùng với một khối lệnh (thường là `{ .. }`). Mỗi lần khối lệnh trong vòng lặp được thực thi, ta gọi đó là một *lượt lặp*.

Ví dụ, vòng lặp `while` và `do..while` minh họa ý tưởng lặp lại một khối lệnh cho đến khi điều kiện không còn được đánh giá là `true`:

```js
while (numOfCustomers > 0) {
	console.log("Tôi có thể giúp gì cho bạn?");

	// phục vụ khách hàng...

	numOfCustomers = numOfCustomers - 1;
}

// so sánh với:

do {
	console.log("Tôi có thể giúp gì cho bạn?");

	// phục vụ khách hàng...

	numOfCustomers = numOfCustomers - 1;
} while (numOfCustomers > 0);
```

Sự khác biệt thực tế duy nhất giữa hai vòng lặp này là liệu điều kiện được kiểm tra trước lượt lặp đầu tiên (`while`) hay sau lượt lặp đầu tiên (`do..while`).

Dù ở dạng nào, nếu điều kiện kiểm tra là `false`, lượt lặp tiếp theo sẽ không chạy. Nghĩa là nếu điều kiện ban đầu đã `false`, vòng lặp `while` sẽ không chạy lần nào, còn `do..while` sẽ chạy đúng một lần đầu tiên.

Đôi khi bạn lặp để đếm một tập hợp số nhất định, chẳng hạn từ `0` đến `9` (mười số). Bạn có thể làm điều đó bằng cách đặt một biến đếm như `i` ở giá trị `0` và tăng nó lên `1` mỗi lượt lặp.

**Cảnh báo:** Vì nhiều lý do mang tính lịch sử, các ngôn ngữ lập trình hầu như luôn đếm theo kiểu bắt đầu từ số 0 thay vì 1. Nếu bạn chưa quen với cách nghĩ này, lúc đầu có thể sẽ khá rối. Hãy dành thời gian luyện tập đếm bắt đầu từ 0 để cảm thấy quen hơn!

Điều kiện được kiểm tra mỗi lượt lặp, như thể có một câu lệnh `if` ngầm bên trong vòng lặp.

Ta có thể dùng câu lệnh `break` của JavaScript để dừng vòng lặp. Ngoài ra, có thể thấy rằng rất dễ tạo ra một vòng lặp chạy mãi mãi nếu không có cơ chế `break`.

Ví dụ minh họa:

```js
var i = 0;

// vòng lặp `while(true)` sẽ chạy mãi mãi đúng không?
while (true) {
	// dừng vòng lặp?
	if ((i <= 9) === false) {
		break;
	}

	console.log(i);
	i = i + 1;
}
// 0 1 2 3 4 5 6 7 8 9
```

**Cảnh báo:** Đây không hẳn là cách thực tế bạn nên dùng để viết vòng lặp. Nó chỉ được trình bày nhằm minh họa.

Dù vòng lặp `while` (hoặc `do..while`) có thể làm được việc này một cách thủ công, có một dạng cú pháp khác gọi là vòng lặp `for` sinh ra để phục vụ việc đếm:

```js
for (var i = 0; i <= 9; i = i + 1) {
	console.log(i);
}
// 0 1 2 3 4 5 6 7 8 9
```

Bạn có thể thấy, trong cả hai trường hợp, điều kiện `i <= 9` là `true` trong 10 lượt lặp đầu tiên (khi `i` tăng từ `0` đến `9`), và trở thành `false` khi `i` bằng `10`.

Vòng lặp `for` gồm ba phần: phần khởi tạo (`var i = 0`), phần kiểm tra điều kiện (`i <= 9`), và phần cập nhật (`i = i + 1`). Vì vậy, nếu bạn cần đếm trong lượt lặp, `for` là dạng ngắn gọn hơn và thường dễ viết cũng như dễ hiểu hơn.

Còn có những dạng vòng lặp đặc biệt khác dùng để lặp qua các giá trị cụ thể, như các thuộc tính của một đối tượng (xem chương 2), trong đó điều kiện ngầm là kiểm tra xem đã duyệt hết tất cả thuộc tính hay chưa. Dù ở dạng nào, nguyên tắc "lặp cho đến khi điều kiện không còn đúng" vẫn luôn được giữ nguyên.

## Hàm

Nhân viên cửa hàng điện thoại có lẽ không mang máy tính theo người để tính thuế và tổng số tiền mua hàng. Đó là một công việc cô ấy cần xác định một lần rồi tái sử dụng nhiều lần. Khả năng cao là cửa hàng đã trang bị máy tính tiền (máy tính bảng, máy tính, v.v.) với các "chức năng" đó được tích hợp sẵn.

Tương tự, chương trình của bạn gần như chắc chắn sẽ muốn chia nhỏ các công việc thành các phần có thể tái sử dụng thay vì cứ lặp đi lặp lại một cách rập khuôn. Cách để làm điều đó là định nghĩa một `function` (hàm).

Một hàm thường là một đoạn mã có tên có thể được "gọi" bằng tên đó, và đoạn mã bên trong sẽ được chạy mỗi khi gọi. Xem ví dụ:

```js
function printAmount() {
	console.log(amount.toFixed(2));
}

var amount = 99.99;

printAmount(); // "99.99"

amount = amount * 2;

printAmount(); // "199.98"
```

Hàm có thể nhận vào các đối số (còn gọi là tham số) - các giá trị bạn truyền vào. Và chúng cũng có thể trả về một giá trị.

```js
function printAmount(amt) {
	console.log(amt.toFixed(2));
}

function formatAmount() {
	return "$" + amount.toFixed(2);
}

var amount = 99.99;

printAmount(amount * 2);       // "199.98"

amount = formatAmount();
console.log(amount);           // "$99.99"
```

Hàm `printAmount(..)` nhận một tham số được gọi là `amt`. Hàm `formatAmount()` trả về một giá trị. Tất nhiên, bạn cũng có thể kết hợp cả hai kỹ thuật trong cùng một hàm.

Hàm thường được dùng cho đoạn mã bạn định gọi nhiều lần, nhưng chúng cũng hữu ích để tổ chức các phần mã có liên quan vào một khối được đặt tên, ngay cả khi bạn chỉ định gọi một lần.

Xem ví dụ:

```js
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// tính tổng với thuế
	amt = amt + (amt * TAX_RATE);

	// trả về số tiền mới
	return amt;
}

var amount = 99.99;

amount = calculateFinalPurchaseAmount(amount);

console.log(amount.toFixed(2));     // "107.99"
```

Dù `calculateFinalPurchaseAmount(..)` chỉ được gọi một lần, việc tổ chức logic của nó thành một hàm riêng có tên giúp đoạn mã sử dụng nó (`amount = calculateFinal...`) trở nên sáng sủa hơn. Nếu hàm có nhiều câu lệnh hơn nữa, lợi ích sẽ càng rõ rệt.

### Phạm vi

Nếu bạn hỏi nhân viên cửa hàng điện thoại về một mẫu điện thoại mà cửa hàng không có, cô ấy sẽ không thể bán cho bạn. Cô ấy chỉ có quyền truy cập vào những chiếc điện thoại trong kho của cửa hàng mình. Bạn sẽ phải thử tìm ở cửa hàng khác nếu muốn mua mẫu điện thoại đó.

Trong lập trình có một thuật ngữ cho khái niệm này: *phạm vi* (về mặt kỹ thuật gọi là *phạm vi từ vựng*). Trong JavaScript, mỗi hàm có phạm vi riêng của nó. Phạm vi về cơ bản là một tập hợp các biến cùng với các quy tắc về cách các biến đó được truy cập bằng tên. Chỉ những đoạn mã bên trong hàm đó mới có thể truy cập các biến trong phạm vi của hàm đó.

Tên biến phải là duy nhất trong cùng một phạm vi - không thể có hai biến `a` khác nhau nằm cạnh nhau. Nhưng cùng một tên biến `a` có thể xuất hiện trong các phạm vi khác nhau.

```js
function one() {
	// biến `a` này chỉ thuộc về hàm `one()`
	var a = 1;
	console.log(a);
}

function two() {
	// biến `a` này chỉ thuộc về hàm `two()`
	var a = 2;
	console.log(a);
}

one();		// 1
two();		// 2
```

Ngoài ra, một phạm vi có thể được lồng trong một phạm vi khác, giống như một chú hề trong bữa tiệc sinh nhật thổi một quả bong bóng bên trong một quả bóng khác. Nếu một phạm vi được lồng bên trong một phạm vi khác, đoạn mã bên trong phạm vi hẹp hơn có thể truy cập các biến từ cả hai phạm vi.

Xem ví dụ:

```js
function outer() {
	var a = 1;

	function inner() {
		var b = 2;

		// có thể truy cập cả `a` và `b` tại đây
		console.log(a + b);	// 3
	}

	inner();

	// chỉ có thể truy cập `a` tại đây
	console.log(a);		// 1
}

outer();
```

Các quy tắc phạm vi từ vựng nói rằng đoạn mã trong một phạm vi có thể truy cập các biến thuộc phạm vi đó hoặc bất kỳ phạm vi bên ngoài nào của nó.

Vì vậy, đoạn mã trong hàm `inner()` có quyền truy cập cả hai biến `a` và `b`, nhưng đoạn mã trong `outer()` chỉ truy cập được `a` - nó không thể truy cập `b` vì biến đó chỉ nằm trong `inner()`.

Hãy nhớ lại đoạn mã sau:

```js
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// tính tổng với thuế
	amt = amt + (amt * TAX_RATE);

	// trả về số tiền mới
	return amt;
}
```

Hằng số (biến) `TAX_RATE` có thể được truy cập từ bên trong hàm `calculateFinalPurchaseAmount(..)` mặc dù ta không truyền vào nhờ phạm vi từ vựng.

**Ghi chú:** Để tìm hiểu thêm về phạm vi từ vựng, xem ba chương đầu tiên của sách *Phạm vi và hàm khép kín* trong bộ sách này.

## Luyện tập

Không có gì thay thế được việc luyện tập trong quá trình học lập trình. Dù tôi có viết hay đến đâu cũng không thể giúp bạn trở thành lập trình viên được.

Với tinh thần đó, hãy thử luyện tập một vài khái niệm chúng ta đã học trong chương này. Tôi sẽ đưa ra "yêu cầu", bạn thử làm trước. Sau đó hãy tham khảo đoạn mã bên dưới để xem tôi giải quyết ra sao.

* Viết một chương trình để tính tổng giá tiền khi mua điện thoại. Bạn sẽ tiếp tục mua điện thoại (gợi ý: dùng vòng lặp!) cho đến khi hết tiền trong tài khoản ngân hàng. Bạn cũng sẽ mua thêm phụ kiện cho mỗi điện thoại miễn là tổng tiền vẫn dưới ngưỡng chi tiêu của bạn.
* Sau khi tính xong tổng tiền, hãy cộng thêm thuế, sau đó in ra tổng tiền đã tính theo đúng định dạng.
* Cuối cùng, so sánh số tiền đó với số dư tài khoản ngân hàng để xem bạn có đủ tiền không.
* Bạn nên khai báo một số hằng số cho "thuế suất", "giá điện thoại", "giá phụ kiện", và "ngưỡng chi tiêu", cũng như một biến cho "số dư tài khoản ngân hàng".
* Bạn nên định nghĩa các hàm để tính thuế và định dạng số tiền với dấu `$` và làm tròn tới hai chữ số thập phân.
* **Thử thách bổ sung:** Hãy thử kết hợp đầu vào trong chương trình này, ví dụ như dùng `prompt(..)` đã đề cập trong phần "Đầu vào" trước đó. Chẳng hạn, bạn có thể yêu cầu người dùng nhập số dư tài khoản của họ. Hãy sáng tạo và vui vẻ nhé!

Được rồi, giờ bạn hãy thử làm đi. Đừng nhìn đoạn mã mẫu của tôi cho đến khi bạn tự làm xong nhé!

**Ghi chú:** Vì đây là sách dạy JavaScript, tôi sẽ giải bài tập bằng JavaScript. Nhưng bạn cũng có thể thử bằng ngôn ngữ khác nếu bạn thấy quen thuộc hơn.

Dưới đây là cách tôi giải bài tập này bằng JavaScript:

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
	return "$" + amount.toFixed(2);
}

// tiếp tục mua điện thoại nếu còn tiền
while (amount < bank_balance) {
	// mua một chiếc điện thoại mới!
	amount = amount + PHONE_PRICE;

	// có đủ tiền mua phụ kiện không?
	if (amount < SPENDING_THRESHOLD) {
		amount = amount + ACCESSORY_PRICE;
	}
}

// đừng quên nộp thuế nhé
amount = amount + calculateTax(amount);

console.log("Đơn hàng: " + formatAmount(amount));
// Đơn hàng: $334.76

// bạn có đủ tiền không?
if (amount > bank_balance) {
	console.log("Bạn không thể thanh toán đơn hàng này. :(");
}
// Bạn không thể thanh toán đơn hàng này. :(
```

**Ghi chú:** Cách đơn giản nhất để chạy chương trình JavaScript này là nhập nó vào bảng điều khiển dành cho lập trình viên của chương trình duyệt gần nhất.

Bạn làm thế nào rồi? Sẽ không hại gì nếu bạn thử lại một lần nữa sau khi đã xem qua đoạn mã của tôi. Và hãy thử thay đổi một số hằng số xem chương trình chạy thế nào với các giá trị khác nhau.

## Nhìn lại

Học lập trình không nhất thiết phải là một quá trình phức tạp và quá tải. Bạn chỉ cần nắm vững một vài khái niệm cơ bản.

Những khái niệm này giống như những khối xây dựng. Để xây được một tòa tháp cao, bạn phải bắt đầu bằng cách đặt từng khối lên nhau. Lập trình cũng vậy. Dưới đây là một số khối nền thiết yếu trong lập trình:

* Bạn cần các *toán tử* để thực hiện các thao tác trên giá trị.
* Bạn cần giá trị và *kiểu dữ liệu* để thực hiện các thao tác khác nhau, như toán học với `number` hoặc xuất ra với `string`.
* Bạn cần *biến* để lưu trữ dữ liệu (hay còn gọi là *trạng thái*) trong quá trình chương trình chạy.
* Bạn cần các *điều kiện* như câu lệnh `if` để đưa ra quyết định.
* Bạn cần *vòng lặp* để lặp lại các tác vụ cho đến khi một điều kiện không còn đúng.
* Bạn cần *hàm* để tổ chức mã của mình thành các phần logic có thể tái sử dụng.

Chú thích trong mã là một cách hiệu quả để viết mã dễ đọc hơn, giúp chương trình dễ hiểu, dễ bảo trì và dễ sửa lỗi khi cần.

Cuối cùng, đừng bỏ qua sức mạnh của thực hành. Cách tốt nhất để học viết mã là hãy viết thật nhiều mã.

Tôi rất vui vì bạn đã đi được một chặng đường đáng kể trong hành trình học lập trình rồi! Hãy tiếp tục cố gắng. Đừng quên tìm hiểu thêm các tài nguyên lập trình cho người mới bắt đầu (sách, blog, khóa học trực tuyến, v.v.). Chương này và cuốn sách này là một khởi đầu tuyệt vời, nhưng mới chỉ là phần giới thiệu sơ lược.

Chương tiếp theo sẽ đánh giá nhiều khái niệm ở chương này, nhưng dưới góc nhìn cụ thể hơn của JavaScript, từ đó làm nổi bật những chủ đề quan trọng sẽ được đào sâu hơn trong phần còn lại của loạt sách.
