# Bạn không hiểu JS: Khởi động và tiến lên
# Chương 2: Đến với JavaScript

Ở chương trước, tôi đã giới thiệu các khối xây dựng cơ bản trong lập trình như biến, vòng lặp, rẽ nhánh và hàm. Dĩ nhiên tất cả ví dụ đều dùng ngôn ngữ JavaScript. Nhưng trong chương này, ta sẽ tập trung vào những điều cụ thể mà bạn cần biết để bắt đầu với tư cách là một lập trình viên JavaScript.

Chúng ta sẽ giới thiệu khá nhiều khái niệm trong chương này nhưng phần lớn sẽ được khám phá trong các cuốn *BKHJS* tiếp theo. Bạn có thể xem chương này như một bản tổng quan các chủ đề sẽ được đào sâu trong phần còn lại của bộ sách.

Đặc biệt nếu bạn là người mới bắt đầu với JavaScript, hãy chuẩn bị dành nhiều thời gian để xem lại các khái niệm và ví dụ nhiều lần. Một nền móng vững chắc được xây từ từng viên gạch một, vì vậy đừng kì vọng bản thân có thể hiểu hết mọi thứ chỉ sau một lần đọc.

Hành trình học JavaScript một cách sâu sắc của bạn bắt đầu từ đây.

**Lưu ý:** Như tôi đã nói ở chương 1, bạn nên thử chạy tất cả các đoạn mã khi đọc qua chương này. Một số đoạn mã sử dụng những tính năng mới được giới thiệu trong phiên bản JavaScript mới nhất tại thời điểm viết sách (thường được gọi là "ES6" - phiên bản thứ 6 của ECMAScript, tên chính thức của tiêu chuẩn JS). Nếu bạn đang dùng trình duyệt cũ (trước ES6), một vài đoạn mã có thể không chạy được. Hãy đảm bảo sử dụng phiên bản mới nhất của các trình duyệt hiện đại như Chrome, Firefox hoặc IE.

## Giá trị và kiểu dữ liệu

Như đã khẳng định ở chương 1, JavaScript có kiểu dữ liệu cho giá trị chứ không phải kiểu cho biến. Các kiểu dữ liệu tích hợp sẵn là:

* `string`
* `number`
* `boolean`
* `null` và `undefined`
* `object`
* `symbol` (mới trong ES6)

JavaScript cung cấp toán tử `typeof` để kiểm tra một giá trị và cho biết kiểu của nó:

```js
var a;
typeof a;				// "undefined"

a = "hello world";
typeof a;				// "string"

a = 42;
typeof a;				// "number"

a = true;
typeof a;				// "boolean"

a = null;
typeof a;				// "object" -- kì lạ thật, lỗi

a = undefined;
typeof a;				// "undefined"

a = { b: "c" };
typeof a;				// "object"
```

Giá trị trả về từ toán tử `typeof` luôn là một trong sáu chuỗi ký tự (bảy kể từ ES6 - với kiểu `"symbol"`). Tức là, `typeof "abc"` sẽ trả về `"string"`, chứ không phải `string`.

Hãy chú ý trong ví dụ, biến `a` lần lượt giữ các giá trị có kiểu khác nhau. Và dù thoạt nhìn có vẻ như `typeof a` đang hỏi "kiểu của `a`" là gì, thực chất nó đang hỏi "kiểu của giá trị hiện tại bên trong `a`" là gì. Trong JavaScript, chỉ có giá trị là có kiểu, biến chỉ đơn giản là hộp chứa cho những giá trị đó mà thôi.

`typeof null` là một trường hợp đặc biệt vì kết quả trả ra một cách sai lầm là `"object"`, dù bạn có thể mong đợi `"null"`.

**Cảnh báo**: Đây là một lỗi lâu đời trong JavaScript nhưng sẽ không bao giờ được sửa. Quá nhiều mã trên web phụ thuộc vào lỗi này, việc sửa nó sẽ gây ra nhiều lỗi hơn.

Ngoài ra hãy lưu ý về cách viết `a = undefined`. Chúng ta đang gán giá trị `undefined` một cách rõ ràng cho biến `a`, hành vi này không khác gì việc khai báo một biến chưa gán giá trị, giống như `var a;` ở đầu đoạn mã. Một biến có thể mang giá trị `undefined` theo nhiều cách, chẳng hạn như hàm không trả về gì hoặc khi dùng toán tử `void`.

### Đối tượng

Kiểu `object` dùng để chỉ những giá trị phức hợp nơi bạn có thể khai báo các thuộc tính (vị trí được đặt tên) giữ giá trị thuộc bất kỳ kiểu nào. Có lẽ đây là kiểu dữ liệu giá trị hữu ích nhất trong JavaScript.

```js
var obj = {
	a: "hello world",
	b: 42,
	c: true
};

obj.a;		// "hello world"
obj.b;		// 42
obj.c;		// true

obj["a"];	// "hello world"
obj["b"];	// 42
obj["c"];	// true
```

Sẽ hữu ích nếu bạn hình dung giá trị `obj` này một cách trực quan:

<img src="fig4.png">

Các thuộc tính có thể được truy cập bằng *kí hiệu chấm* (ví dụ `obj.a`) hoặc *kí hiệu ngoặc* (ví dụ `obj["a"]`). Kí hiệu chấm ngắn gọn hơn và nhìn chung dễ đọc hơn, vì vậy được ưu tiên sử dụng khi có thể.

Kí hiệu ngoặc hữu ích khi bạn có tên thuộc tính chứa ký tự đặc biệt, như `obj["hello world!"]` - những thuộc tính như vậy thường được gọi là *khóa* khi truy cập qua kí hiệu ngoặc. Cú pháp `[ ]` yêu cầu một biến (sẽ được giải thích tiếp theo) hoặc một `string` nguyên bản (phải được bao trong cặp dấu `" .. "` hoặc `' .. '`).

Tất nhiên, kí hiệu ngoặc có thể cũng rất hữu dụng nếu bạn muốn truy cập một thuộc tính/khóa mà tên của nó được lưu trong một biến khác, ví dụ:

```js
var obj = {
    a: "hello world",
    b: 42
};

var b = "a";

obj[b];       // "hello world"
obj["b"];     // 42
```

**Ghi chú:** Để tìm hiểu thêm về `object` trong JavaScript, hãy xem cuốn *this và nguyên mẫu đối tượng* trong loạt sách này, đặc biệt là chương 3.

Có một vài kiểu giá trị khác mà bạn thường xuyên làm việc cùng trong chương trình JavaScript: *mảng* và *hàm*. Tuy nhiên, thay vì là kiểu dựng sẵn thì chúng nên được xem như các kiểu con - phiên bản chuyên biệt của kiểu `object`.

#### Mảng

Một mảng là một `object` chứa các giá trị (thuộc bất kỳ kiểu nào), không phải ở các thuộc tính/khóa có tên, mà ở các vị trí được đánh chỉ số bằng số. Ví dụ:

```js
var arr = [
	"hello world",
	42,
	true
];

arr[0];			// "hello world"
arr[1];			// 42
arr[2];			// true
arr.length;		// 3

typeof arr;		// "object"
```

**Lưu ý:** Những ngôn ngữ bắt đầu đếm từ số 0, như JavaScript, sử dụng `0` làm chỉ số của phần tử đầu tiên trong mảng.

Bạn có thể hình dung `arr` một cách trực quan như sau:

<img src="fig5.png">

Vì mảng là một đối tượng đặc biệt (như `typeof` cho thấy), chúng cũng có thể có các thuộc tính, bao gồm thuộc tính `length` được cập nhật tự động.

Về mặt lý thuyết, bạn có thể dùng một mảng như một đối tượng thông thường với các thuộc tính có tên, hoặc dùng một `object` nhưng chỉ gán cho nó các thuộc tính dạng số (`"0"`, `"1"`, v.v.) giống như một mảng. Tuy nhiên, điều này nhìn chung bị xem là cách sử dụng sai của kiểu tương ứng.

Cách tiếp cận tốt và tự nhiên nhất là: dùng mảng cho các giá trị theo thứ tự chỉ số và dùng `object` cho các thuộc tính có tên.

#### Hàm

Kiểu con `object` khác mà bạn sẽ dùng xuyên suốt trong chương trình JS là hàm:

```js
function foo() {
	return 42;
}

foo.bar = "hello world";

typeof foo;			// "function"
typeof foo();		// "number"
typeof foo.bar;		// "string"
```

Một lần nữa, hàm là một kiểu con của `object` - `typeof` trả về "function", điều này ngụ ý rằng một `function` là một kiểu chính - và do đó có thể có các thuộc tính, nhưng bạn thường chỉ sử dụng thuộc tính của đối tượng hàm (như `foo.bar`) trong một số trường hợp.

**Ghi chú**: Để biết thêm thông tin về các giá trị và kiểu của chúng trong JS, hãy xem hai chương đầu tiên của cuốn *Kiểu dữ liệu và ngữ pháp* trong loạt sách này.

### Các phương thức của kiểu dựng sẵn

Các kiểu và kiểu con dựng sẵn mà chúng ta vừa thảo luận có các hành vi khá mạnh mẽ và hữu ích được thể hiện dưới dạng thuộc tính và phương thức.

Ví dụ:

```js
var a = "hello world";
var b = 3.14159;

a.length;				// 11
a.toUpperCase();		// "HELLO WORLD"
b.toFixed(4);			// "3.1416"
```

Việc có thể gọi `a.toUpperCase()` không chỉ đơn giản là do phương thức đó tồn tại trên giá trị.

Nói một cách ngắn gọn, có một dạng đối tượng `String` (chữ `S` viết hoa) bọc ngoài, thường được gọi là "nguyên bản", tương ứng với kiểu nguyên thủy `string`; chính đối tượng bao bọc này định nghĩa phương thức `toUpperCase()` trên nguyên mẫu của nó.

Khi bạn sử dụng một giá trị nguyên thủy như `"hello world"` như một `object` bằng cách truy cập thuộc tính hoặc phương thức (ví dụ, `a.toUpperCase()` trong đoạn mã trước), JS tự động "đóng hộp" giá trị đó thành đối tượng bao tương ứng (ẩn dưới bề mặt).

Một giá trị `string` có thể được bao bởi một đối tượng `String`, một `number` có thể được bao bởi một đối tượng `Number`, và một `boolean` có thể được bao bởi một đối tượng `Boolean`. Trong hầu hết các trường hợp, bạn không cần quan tâm hay sử dụng trực tiếp các dạng đối tượng bao ngoài này - hãy ưu tiên dùng các giá trị nguyên thủy trong hầu hết mọi tình huống và JavaScript sẽ tự lo phần còn lại cho bạn.

**Ghi chú:** Để tìm hiểu thêm về các đối tượng nguyên bản trong JavaScript và quá trình "đóng hộp", hãy xem chương 3 trong cuốn *Kiểu dữ liệu và ngữ pháp* của loạt sách này. Để hiểu rõ hơn về nguyên mẫu của một đối tượng, hãy xem chương 5 trong cuốn *this và nguyên mẫu đối tượng* của loạt sách này.

### So sánh giá trị

Có hai kiểu so sánh giá trị chính mà bạn sẽ cần thực hiện trong các chương trình JS của mình: *bằng nhau* và *khác nhau*. Kết quả của bất kỳ phép so sánh nào cũng luôn là một giá trị `boolean` nghiêm ngặt (`true` hoặc `false`), bất kể kiểu của các giá trị được so sánh là gì.

#### Ép kiểu

Chúng ta đã đề cập sơ lược về ép kiểu ở chương 1, nhưng hãy quay lại chủ đề này ở đây.

Ép kiểu trong JavaScript có hai hình thức: *tường minh* và *ngầm định*. Ép kiểu tường minh là khi bạn có thể dễ dàng thấy rõ trong mã rằng có sự chuyển đổi kiểu dữ liệu xảy ra, trong khi ép kiểu ngầm định là khi việc chuyển đổi kiểu xảy ra như một hệ quả không rõ ràng của một thao tác khác.

Có lẽ bạn đã nghe những quan điểm như "ép kiểu là điều tệ hại" vì rõ ràng trong một số trường hợp, ép kiểu có thể tạo ra những kết quả bất ngờ. Có lẽ không gì gây bực bội hơn cho lập trình viên bằng việc ngôn ngữ gây bất ngờ cho họ.

Ép kiểu không phải là điều xấu, và nó cũng không nhất thiết phải gây bất ngờ. Thực tế, phần lớn các trường hợp sử dụng ép kiểu có thể được xây dựng một cách hợp lý và dễ hiểu, và thậm chí có thể giúp *cải thiện* tính dễ đọc của mã nguồn. Nhưng chúng ta sẽ không đi sâu hơn vào cuộc tranh luận này - chương 4 của cuốn *Kiểu dữ liệu và ngữ pháp* của loạt sách này sẽ trình bày đầy đủ mọi góc nhìn.

Dưới đây là một ví dụ về ép kiểu *tường minh*:

```js
var a = "42";

var b = Number( a );

a;				// "42"
b;				// 42 -- con số!
```

Và đây là ví dụ về ép kiểu *ngầm định*:

```js
var a = "42";

var b = a * 1;	// "42" ngầm ép về 42

a;				// "42"
b;				// 42 -- con số!
```

#### Đúng và sai

Trong chương 1, chúng ta đã nói sơ qua đến tính chất "đúng" và "sai" của giá trị: khi một giá trị không phải `boolean` bị ép kiểu sang `boolean`, nó sẽ trở thành `true` hay `false`?

Danh sách cụ thể các giá trị "sai" trong JavaScript như sau:

* `""` (chuỗi rỗng)
* `0`, `-0`, `NaN` (not a number - không phải `number`)
* `null`, `undefined`
* `false`

Bất kỳ giá trị nào không nằm trong danh sách "sai" trên đều là "đúng". Dưới đây là một số ví dụ về những giá trị như vậy:

* `"hello"`
* `42`
* `true`
* `[ ]`, `[ 1, "2", 3 ]` (mảng)
* `{ }`, `{ a: 42 }` (đối tượng)
* `function foo() { .. }` (hàm)

Điều quan trọng là phải nhớ rằng một giá trị không phải `boolean` chỉ tuân theo quy tắc ép kiểu "đúng"/"sai" nếu nó thực sự bị ép kiểu sang `boolean`. Không khó để bạn tự làm mình bối rối với những tình huống tưởng như đang ép kiểu sang `boolean` nhưng thực ra lại không phải.

#### So sánh bằng

Có bốn toán tử so sánh bằng: `==`, `===`, `!=` và `!==`. Các dạng có `!` dĩ nhiên là phiên bản "khác" tương ứng với các toán tử còn lại; *so sánh không bằng* không nên bị nhầm với *so sánh khác biệt*.

Sự khác biệt giữa `==` và `===` thường được mô tả là `==` kiểm tra giá trị còn `===` kiểm tra cả giá trị và kiểu. Tuy nhiên, mô tả này không hoàn toàn chính xác. Cách diễn giải đúng hơn là: `==` kiểm tra giá trị cho phép ép kiểu, còn `===` kiểm tra giá trị mà không cho phép ép kiểu; vì lý do đó, `===` thường được gọi là so sánh "bằng nghiêm ngặt".

Hãy xem xét phép ép kiểu ngầm định được phép bởi phép so sánh bằng lỏng lẻo (`==`) nhưng không được phép với so sánh bằng nghiêm ngặt (`===`):

```js
var a = "42";
var b = 42;

a == b;			// true
a === b;		// false
```

Trong phép so sánh `a == b`, JS nhận thấy kiểu của hai giá trị không khớp, nên nó sẽ thực hiện một loạt các bước theo thứ tự để ép kiểu một hoặc cả hai giá trị sang kiểu khác cho đến khi kiểu khớp nhau, lúc đó phép so sánh giá trị đơn giản mới được thực hiện.

Nếu bạn nghĩ về điều này, có hai cách mà `a == b` có thể cho kết quả `true` thông qua ép kiểu. Hoặc là phép so sánh trở thành `42 == 42`, hoặc là `"42" == "42"`. Nhưng cái nào mới đúng?

Câu trả lời là: `"42"` được chuyển thành `42`, để phép so sánh trở thành `42 == 42`. Trong ví dụ đơn giản như vậy, có vẻ không quan trọng cách ép kiểu diễn ra theo hướng nào vì kết quả cuối cùng giống nhau. Nhưng có những trường hợp phức tạp hơn, nơi mà không chỉ kết quả so sánh quan trọng, mà còn cả *cách* đạt được kết quả đó.

Phép so sánh `a === b` trả về `false`, vì không có phép ép kiểu nào được cho phép, nên so sánh giá trị đơn giản tất nhiên sẽ thất bại. Nhiều lập trình viên cảm thấy `===` dễ dự đoán hơn, nên họ khuyên nên luôn dùng nó và tránh xa `==`. Tôi cho rằng quan điểm này quá hạn hẹp. Tôi tin rằng `==` là một công cụ mạnh mẽ giúp ích cho chương trình của bạn, *miễn là bạn chịu khó học cách nó hoạt động*.

Chúng ta sẽ không đi sâu vào tất cả chi tiết phức tạp về cách ép kiểu trong phép so sánh `==`. Phần lớn trong số đó khá hợp lý, nhưng cũng có một số trường hợp góc cần chú ý. Bạn có thể đọc phần 11.9.3 trong đặc tả ES5 ([http://www.ecma-international.org/ecma-262/5.1/](http://www.ecma-international.org/ecma-262/5.1/)) để xem các quy tắc chính xác, và bạn sẽ ngạc nhiên vì cơ chế này thực ra khá đơn giản so với mọi lời bàn tán tiêu cực xung quanh nó.

Để tóm gọn rất nhiều chi tiết thành vài điều cần nhớ đơn giản, giúp bạn biết nên dùng `==` hay `===` trong từng tình huống, dưới đây là một số quy tắc của tôi:

* Nếu một trong hai giá trị trong phép so sánh có thể là `true` hoặc `false`, tránh dùng `==` và hãy dùng `===`.
* Nếu một trong hai giá trị trong phép so sánh có thể là một trong các giá trị sau (`0`, `""`, hoặc `[]` - mảng rỗng), tránh dùng `==` và hãy dùng `===`.
* Trong *mọi* trường hợp khác, bạn có thể dùng `==` một cách an toàn. Không chỉ an toàn, mà trong nhiều trường hợp nó còn giúp đơn giản hóa mã của bạn, làm tăng tính dễ đọc.

Những quy tắc này chủ yếu yêu cầu bạn phải suy nghĩ cẩn thận về mã của mình và về kiểu giá trị nào có thể được gán cho các biến dùng để so sánh. Nếu bạn có thể chắc chắn về các giá trị, và `==` an toàn, thì hãy dùng nó! Nếu bạn không chắc, hãy dùng `===`. Đơn giản vậy thôi.

Toán tử `!=` (so sánh không bằng) đi đôi với `==`, và toán tử `!==` đi đôi với `===`. Tất cả các quy tắc và quan sát mà chúng ta vừa thảo luận cũng áp dụng tương tự cho các phép so sánh không bằng này.

Bạn nên đặc biệt lưu ý các quy tắc so sánh `==` và `===` nếu bạn đang so sánh hai giá trị không phải nguyên thủy, như `object` (bao gồm `function` và `array`). Vì những giá trị này thực ra được giữ bằng tham chiếu, nên cả phép so sánh `==` lẫn `===` sẽ chỉ kiểm tra xem hai tham chiếu có khớp nhau hay không chứ không quan tâm đến giá trị bên trong.

Ví dụ, mặc định thì `array` sẽ bị ép kiểu sang `string` bằng cách nối tất cả các phần tử với dấu phẩy (`,`) ở giữa. Bạn có thể nghĩ rằng hai `array` có cùng nội dung sẽ bằng nhau theo `==`, nhưng thực tế thì không:

```js
var a = [1,2,3];
var b = [1,2,3];
var c = "1,2,3";

a == c;        // true
b == c;        // true
a == b;        // false
```

**Chú ý:** Để biết thêm thông tin về các quy tắc so sánh bằng `==`, xem đặc tả ES5 (phần 11.9.3) và tham khảo chương 4 của cuốn *Kiểu dữ liệu và ngữ pháp* trong loạt sách này; xem chương 2 để hiểu thêm về giá trị và tham chiếu.

#### So sánh không bằng

Toán tử `<`, `>`, `<=`, và `>=` được dùng để so sánh không bằng, trong đặc tả gọi là “so sánh quan hệ”. Thông thường, chúng được dùng với các giá trị có thể so sánh theo thứ tự như `number`. Dễ thấy rằng `3 < 4`.

Tuy nhiên, các giá trị `string` trong JavaScript cũng có thể được so sánh không bằng, theo quy tắc bảng chữ cái thông thường (`"bar" < "foo"`).

Còn việc ép kiểu thì sao? Các quy tắc tương tự như phép so sánh `==` (dù không hoàn toàn giống nhau!) cũng được áp dụng cho các toán tử không bằng. Đáng chú ý là không có toán tử “không bằng nghiêm ngặt” nào không cho ép kiểu như `===` làm với so sánh bằng nghiêm ngặt.

Xem ví dụ:

```js
var a = 41;
var b = "42";
var c = "43";

a < b;        // true
b < c;        // true
```

Chuyện gì đang xảy ra? Phần 11.8.5 của đặc tả ES5 có ghi rằng nếu cả hai giá trị trong phép so sánh `<` đều là `string`, như với `b < c`, thì phép so sánh sẽ được thực hiện theo thứ tự từ điển (thứ tự sắp xếp của chữ cái trong từ điển). Nhưng nếu một hoặc cả hai không phải `string`, như với `a < b`, thì cả hai giá trị sẽ được ép kiểu sang `number` và phép so sánh số học thông thường được thực hiện.

Một lỗi thường gặp bạn có thể vướng phải khi so sánh các kiểu giá trị khác nhau - hãy nhớ rằng không có dạng “không bằng nghiêm ngặt” để dùng - là khi một trong các giá trị không thể chuyển thành số hợp lệ, ví dụ:

```js
var a = 42;
var b = "foo";

a < b;        // false
a > b;        // false
a == b;       // false
```

Khoan đã, sao cả ba phép so sánh đều `false`? Vì `b` bị ép kiểu thành giá trị số không hợp lệ `NaN` trong hai phép so sánh `<` và `>`, và đặc tả nói rằng `NaN` không lớn hơn cũng không nhỏ hơn bất kỳ giá trị nào.

Phép so sánh `==` thất bại vì lý do khác. `a == b` có thể bị hiểu là `42 == NaN` hoặc `"42" == "foo"` - như đã giải thích trước đó, trường hợp đầu là đúng.

**Chú ý:** Để biết thêm thông tin về các quy tắc so sánh không bằng, xem phần 11.8.5 trong đặc tả ES5 và cả chương 4 của cuốn *Kiểu dữ liệu và ngữ pháp* trong loạt sách này.

## Biến

Trong JavaScript, tên biến (kể cả tên hàm) phải là các *định danh* hợp lệ. Các quy tắc đầy đủ và nghiêm ngặt cho định danh hợp lệ có phần phức tạp nếu bạn xét đến các ký tự không truyền thống như Unicode. Nhưng nếu chỉ xét các ký tự chữ và số trong bảng mã ASCII thông thường thì quy tắc khá đơn giản.

Một định danh phải bắt đầu bằng `a`-`z`, `A`-`Z`, `$`, hoặc `_`. Sau đó có thể chứa thêm các ký tự này và các chữ số từ `0` đến `9`.

Thông thường, quy tắc tương tự cũng áp dụng cho tên thuộc tính như với tên biến. Tuy nhiên, có một số từ không thể dùng làm tên biến nhưng lại hợp lệ làm tên thuộc tính. Những từ này được gọi là “từ dành riêng” bao gồm các từ khóa của JS (`for`, `in`, `if`, v.v.) cũng như `null`, `true`, và `false`.

**Chú ý:** Để biết thêm về các từ dành riêng, xem Phụ lục A của cuốn *Kiểu dữ liệu và ngữ pháp* trong loạt sách này.

### Phạm vi của hàm

Bạn dùng từ khóa `var` để khai báo một biến sẽ thuộc về phạm vi của hàm hiện tại, hoặc phạm vi toàn cục nếu nằm ở cấp cao nhất bên ngoài mọi hàm.

#### Hoisting

Wherever a `var` appears inside a scope, that declaration is taken to belong to the entire scope and accessible everywhere throughout.

Metaphorically, this behavior is called *hoisting*, when a `var` declaration is conceptually "moved" to the top of its enclosing scope. Technically, this process is more accurately explained by how code is compiled, but we can skip over those details for now.

Consider:

```js
var a = 2;

foo();					// works because `foo()`
						// declaration is "hoisted"

function foo() {
	a = 3;

	console.log( a );	// 3

	var a;				// declaration is "hoisted"
						// to the top of `foo()`
}

console.log( a );	// 2
```

**Warning:** It's not common or a good idea to rely on variable *hoisting* to use a variable earlier in its scope than its `var` declaration appears; it can be quite confusing. It's much more common and accepted to use *hoisted* function declarations, as we do with the `foo()` call appearing before its formal declaration.

#### Nested Scopes

When you declare a variable, it is available anywhere in that scope, as well as any lower/inner scopes. For example:

```js
function foo() {
	var a = 1;

	function bar() {
		var b = 2;

		function baz() {
			var c = 3;

			console.log( a, b, c );	// 1 2 3
		}

		baz();
		console.log( a, b );		// 1 2
	}

	bar();
	console.log( a );				// 1
}

foo();
```

Notice that `c` is not available inside of `bar()`, because it's declared only inside the inner `baz()` scope, and that `b` is not available to `foo()` for the same reason.

If you try to access a variable's value in a scope where it's not available, you'll get a `ReferenceError` thrown. If you try to set a variable that hasn't been declared, you'll either end up creating a variable in the top-level global scope (bad!) or getting an error, depending on "strict mode" (see "Strict Mode"). Let's take a look:

```js
function foo() {
	a = 1;	// `a` not formally declared
}

foo();
a;			// 1 -- oops, auto global variable :(
```

This is a very bad practice. Don't do it! Always formally declare your variables.

In addition to creating declarations for variables at the function level, ES6 *lets* you declare variables to belong to individual blocks (pairs of `{ .. }`), using the `let` keyword. Besides some nuanced details, the scoping rules will behave roughly the same as we just saw with functions:

```js
function foo() {
	var a = 1;

	if (a >= 1) {
		let b = 2;

		while (b < 5) {
			let c = b * 2;
			b++;

			console.log( a + c );
		}
	}
}

foo();
// 5 7 9
```

Because of using `let` instead of `var`, `b` will belong only to the `if` statement and thus not to the whole `foo()` function's scope. Similarly, `c` belongs only to the `while` loop. Block scoping is very useful for managing your variable scopes in a more fine-grained fashion, which can make your code much easier to maintain over time.

**Note:** For more information about scope, see the *Scope & Closures* title of this series. See the *ES6 & Beyond* title of this series for more information about `let` block scoping.

## Conditionals

In addition to the `if` statement we introduced briefly in Chapter 1, JavaScript provides a few other conditionals mechanisms that we should take a look at.

Sometimes you may find yourself writing a series of `if..else..if` statements like this:

```js
if (a == 2) {
	// do something
}
else if (a == 10) {
	// do another thing
}
else if (a == 42) {
	// do yet another thing
}
else {
	// fallback to here
}
```

This structure works, but it's a little verbose because you need to specify the `a` test for each case. Here's another option, the `switch` statement:

```js
switch (a) {
	case 2:
		// do something
		break;
	case 10:
		// do another thing
		break;
	case 42:
		// do yet another thing
		break;
	default:
		// fallback to here
}
```

The `break` is important if you want only the statement(s) in one `case` to run. If you omit `break` from a `case`, and that `case` matches or runs, execution will continue with the next `case`'s statements regardless of that `case` matching. This so called "fall through" is sometimes useful/desired:

```js
switch (a) {
	case 2:
	case 10:
		// some cool stuff
		break;
	case 42:
		// other stuff
		break;
	default:
		// fallback
}
```

Here, if `a` is either `2` or `10`, it will execute the "some cool stuff" code statements.

Another form of conditional in JavaScript is the "conditional operator," often called the "ternary operator." It's like a more concise form of a single `if..else` statement, such as:

```js
var a = 42;

var b = (a > 41) ? "hello" : "world";

// similar to:

// if (a > 41) {
//    b = "hello";
// }
// else {
//    b = "world";
// }
```

If the test expression (`a > 41` here) evaluates as `true`, the first clause (`"hello"`) results, otherwise the second clause (`"world"`) results, and whatever the result is then gets assigned to `b`.

The conditional operator doesn't have to be used in an assignment, but that's definitely the most common usage.

**Note:** For more information about testing conditions and other patterns for `switch` and `? :`, see the *Types & Grammar* title of this series.

## Strict Mode

ES5 added a "strict mode" to the language, which tightens the rules for certain behaviors. Generally, these restrictions are seen as keeping the code to a safer and more appropriate set of guidelines. Also, adhering to strict mode makes your code generally more optimizable by the engine. Strict mode is a big win for code, and you should use it for all your programs.

You can opt in to strict mode for an individual function, or an entire file, depending on where you put the strict mode pragma:

```js
function foo() {
	"use strict";

	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is not strict mode
```

Compare that to:

```js
"use strict";

function foo() {
	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is strict mode
```

One key difference (improvement!) with strict mode is disallowing the implicit auto-global variable declaration from omitting the `var`:

```js
function foo() {
	"use strict";	// turn on strict mode
	a = 1;			// `var` missing, ReferenceError
}

foo();
```

If you turn on strict mode in your code, and you get errors, or code starts behaving buggy, your temptation might be to avoid strict mode. But that instinct would be a bad idea to indulge. If strict mode causes issues in your program, almost certainly it's a sign that you have things in your program you should fix.

Not only will strict mode keep your code to a safer path, and not only will it make your code more optimizable, but it also represents the future direction of the language. It'd be easier on you to get used to strict mode now than to keep putting it off -- it'll only get harder to convert later!

**Note:** For more information about strict mode, see the Chapter 5 of the *Types & Grammar* title of this series.

## Functions As Values

So far, we've discussed functions as the primary mechanism of *scope* in JavaScript. You recall typical `function` declaration syntax as follows:

```js
function foo() {
	// ..
}
```

Though it may not seem obvious from that syntax, `foo` is basically just a variable in the outer enclosing scope that's given a reference to the `function` being declared. That is, the `function` itself is a value, just like `42` or `[1,2,3]` would be.

This may sound like a strange concept at first, so take a moment to ponder it. Not only can you pass a value (argument) *to* a function, but *a function itself can be a value* that's assigned to variables, or passed to or returned from other functions.

As such, a function value should be thought of as an expression, much like any other value or expression.

Consider:

```js
var foo = function() {
	// ..
};

var x = function bar(){
	// ..
};
```

The first function expression assigned to the `foo` variable is called *anonymous* because it has no `name`.

The second function expression is *named* (`bar`), even as a reference to it is also assigned to the `x` variable. *Named function expressions* are generally more preferable, though *anonymous function expressions* are still extremely common.

For more information, see the *Scope & Closures* title of this series.

### Immediately Invoked Function Expressions (IIFEs)

In the previous snippet, neither of the function expressions are executed -- we could if we had included `foo()` or `x()`, for instance.

There's another way to execute a function expression, which is typically referred to as an *immediately invoked function expression* (IIFE):

```js
(function IIFE(){
	console.log( "Hello!" );
})();
// "Hello!"
```

The outer `( .. )` that surrounds the `(function IIFE(){ .. })` function expression is just a nuance of JS grammar needed to prevent it from being treated as a normal function declaration.

The final `()` on the end of the expression -- the `})();` line -- is what actually executes the function expression referenced immediately before it.

That may seem strange, but it's not as foreign as first glance. Consider the similarities between `foo` and `IIFE` here:

```js
function foo() { .. }

// `foo` function reference expression,
// then `()` executes it
foo();

// `IIFE` function expression,
// then `()` executes it
(function IIFE(){ .. })();
```

As you can see, listing the `(function IIFE(){ .. })` before its executing `()` is essentially the same as including `foo` before its executing `()`; in both cases, the function reference is executed with `()` immediately after it.

Because an IIFE is just a function, and functions create variable *scope*, using an IIFE in this fashion is often used to declare variables that won't affect the surrounding code outside the IIFE:

```js
var a = 42;

(function IIFE(){
	var a = 10;
	console.log( a );	// 10
})();

console.log( a );		// 42
```

IIFEs can also have return values:

```js
var x = (function IIFE(){
	return 42;
})();

x;	// 42
```

The `42` value gets `return`ed from the `IIFE`-named function being executed, and is then assigned to `x`.

### Closure

*Closure* is one of the most important, and often least understood, concepts in JavaScript. I won't cover it in deep detail here, and instead refer you to the *Scope & Closures* title of this series. But I want to say a few things about it so you understand the general concept. It will be one of the most important techniques in your JS skillset.

You can think of closure as a way to "remember" and continue to access a function's scope (its variables) even once the function has finished running.

Consider:

```js
function makeAdder(x) {
	// parameter `x` is an inner variable

	// inner function `add()` uses `x`, so
	// it has a "closure" over it
	function add(y) {
		return y + x;
	};

	return add;
}
```

The reference to the inner `add(..)` function that gets returned with each call to the outer `makeAdder(..)` is able to remember whatever `x` value was passed in to `makeAdder(..)`. Now, let's use `makeAdder(..)`:

```js
// `plusOne` gets a reference to the inner `add(..)`
// function with closure over the `x` parameter of
// the outer `makeAdder(..)`
var plusOne = makeAdder( 1 );

// `plusTen` gets a reference to the inner `add(..)`
// function with closure over the `x` parameter of
// the outer `makeAdder(..)`
var plusTen = makeAdder( 10 );

plusOne( 3 );		// 4  <-- 1 + 3
plusOne( 41 );		// 42 <-- 1 + 41

plusTen( 13 );		// 23 <-- 10 + 13
```

More on how this code works:

1. When we call `makeAdder(1)`, we get back a reference to its inner `add(..)` that remembers `x` as `1`. We call this function reference `plusOne(..)`.
2. When we call `makeAdder(10)`, we get back another reference to its inner `add(..)` that remembers `x` as `10`. We call this function reference `plusTen(..)`.
3. When we call `plusOne(3)`, it adds `3` (its inner `y`) to the `1` (remembered by `x`), and we get `4` as the result.
4. When we call `plusTen(13)`, it adds `13` (its inner `y`) to the `10` (remembered by `x`), and we get `23` as the result.

Don't worry if this seems strange and confusing at first -- it can be! It'll take lots of practice to understand it fully.

But trust me, once you do, it's one of the most powerful and useful techniques in all of programming. It's definitely worth the effort to let your brain simmer on closures for a bit. In the next section, we'll get a little more practice with closure.

#### Modules

The most common usage of closure in JavaScript is the module pattern. Modules let you define private implementation details (variables, functions) that are hidden from the outside world, as well as a public API that *is* accessible from the outside.

Consider:

```js
function User(){
	var username, password;

	function doLogin(user,pw) {
		username = user;
		password = pw;

		// do the rest of the login work
	}

	var publicAPI = {
		login: doLogin
	};

	return publicAPI;
}

// create a `User` module instance
var fred = User();

fred.login( "fred", "12Battery34!" );
```

The `User()` function serves as an outer scope that holds the variables `username` and `password`, as well as the inner `doLogin()` function; these are all private inner details of this `User` module that cannot be accessed from the outside world.

**Warning:** We are not calling `new User()` here, on purpose, despite the fact that probably seems more common to most readers. `User()` is just a function, not a class to be instantiated, so it's just called normally. Using `new` would be inappropriate and actually waste resources.

Executing `User()` creates an *instance* of the `User` module -- a whole new scope is created, and thus a whole new copy of each of these inner variables/functions. We assign this instance to `fred`. If we run `User()` again, we'd get a new instance entirely separate from `fred`.

The inner `doLogin()` function has a closure over `username` and `password`, meaning it will retain its access to them even after the `User()` function finishes running.

`publicAPI` is an object with one property/method on it, `login`, which is a reference to the inner `doLogin()` function. When we return `publicAPI` from `User()`, it becomes the instance we call `fred`.

At this point, the outer `User()` function has finished executing. Normally, you'd think the inner variables like `username` and `password` have gone away. But here they have not, because there's a closure in the `login()` function keeping them alive.

That's why we can call `fred.login(..)` -- the same as calling the inner `doLogin(..)` -- and it can still access `username` and `password` inner variables.

There's a good chance that with just this brief glimpse at closure and the module pattern, some of it is still a bit confusing. That's OK! It takes some work to wrap your brain around it.

From here, go read the *Scope & Closures* title of this series for a much more in-depth exploration.

## `this` Identifier

Another very commonly misunderstood concept in JavaScript is the `this` identifier. Again, there's a couple of chapters on it in the *this & Object Prototypes* title of this series, so here we'll just briefly introduce the concept.

While it may often seem that `this` is related to "object-oriented patterns," in JS `this` is a different mechanism.

If a function has a `this` reference inside it, that `this` reference usually points to an `object`. But which `object` it points to depends on how the function was called.

It's important to realize that `this` *does not* refer to the function itself, as is the most common misconception.

Here's a quick illustration:

```js
function foo() {
	console.log( this.bar );
}

var bar = "global";

var obj1 = {
	bar: "obj1",
	foo: foo
};

var obj2 = {
	bar: "obj2"
};

// --------

foo();				// "global"
obj1.foo();			// "obj1"
foo.call( obj2 );		// "obj2"
new foo();			// undefined
```

There are four rules for how `this` gets set, and they're shown in those last four lines of that snippet.

1. `foo()` ends up setting `this` to the global object in non-strict mode -- in strict mode, `this` would be `undefined` and you'd get an error in accessing the `bar` property -- so `"global"` is the value found for `this.bar`.
2. `obj1.foo()` sets `this` to the `obj1` object.
3. `foo.call(obj2)` sets `this` to the `obj2` object.
4. `new foo()` sets `this` to a brand new empty object.

Bottom line: to understand what `this` points to, you have to examine how the function in question was called. It will be one of those four ways just shown, and that will then answer what `this` is.

**Note:** For more information about `this`, see Chapters 1 and 2 of the *this & Object Prototypes* title of this series.

## Prototypes

The prototype mechanism in JavaScript is quite complicated. We will only glance at it here. You will want to spend plenty of time reviewing Chapters 4-6 of the *this & Object Prototypes* title of this series for all the details.

When you reference a property on an object, if that property doesn't exist, JavaScript will automatically use that object's internal prototype reference to find another object to look for the property on. You could think of this almost as a fallback if the property is missing.

The internal prototype reference linkage from one object to its fallback happens at the time the object is created. The simplest way to illustrate it is with a built-in utility called `Object.create(..)`.

Consider:

```js
var foo = {
	a: 42
};

// create `bar` and link it to `foo`
var bar = Object.create( foo );

bar.b = "hello world";

bar.b;		// "hello world"
bar.a;		// 42 <-- delegated to `foo`
```

It may help to visualize the `foo` and `bar` objects and their relationship:

<img src="fig6.png">

The `a` property doesn't actually exist on the `bar` object, but because `bar` is prototype-linked to `foo`, JavaScript automatically falls back to looking for `a` on the `foo` object, where it's found.

This linkage may seem like a strange feature of the language. The most common way this feature is used -- and I would argue, abused -- is to try to emulate/fake a "class" mechanism with "inheritance."

But a more natural way of applying prototypes is a pattern called "behavior delegation," where you intentionally design your linked objects to be able to *delegate* from one to the other for parts of the needed behavior.

**Note:** For more information about prototypes and behavior delegation, see Chapters 4-6 of the *this & Object Prototypes* title of this series.

## Old & New

Some of the JS features we've already covered, and certainly many of the features covered in the rest of this series, are newer additions and will not necessarily be available in older browsers. In fact, some of the newest features in the specification aren't even implemented in any stable browsers yet.

So, what do you do with the new stuff? Do you just have to wait around for years or decades for all the old browsers to fade into obscurity?

That's how many people think about the situation, but it's really not a healthy approach to JS.

There are two main techniques you can use to "bring" the newer JavaScript stuff to the older browsers: polyfilling and transpiling.

### Polyfilling

The word "polyfill" is an invented term (by Remy Sharp) (https://remysharp.com/2010/10/08/what-is-a-polyfill) used to refer to taking the definition of a newer feature and producing a piece of code that's equivalent to the behavior, but is able to run in older JS environments.

For example, ES6 defines a utility called `Number.isNaN(..)` to provide an accurate non-buggy check for `NaN` values, deprecating the original `isNaN(..)` utility. But it's easy to polyfill that utility so that you can start using it in your code regardless of whether the end user is in an ES6 browser or not.

Consider:

```js
if (!Number.isNaN) {
	Number.isNaN = function isNaN(x) {
		return x !== x;
	};
}
```

The `if` statement guards against applying the polyfill definition in ES6 browsers where it will already exist. If it's not already present, we define `Number.isNaN(..)`.

**Note:** The check we do here takes advantage of a quirk with `NaN` values, which is that they're the only value in the whole language that is not equal to itself. So the `NaN` value is the only one that would make `x !== x` be `true`.

Not all new features are fully polyfillable. Sometimes most of the behavior can be polyfilled, but there are still small deviations. You should be really, really careful in implementing a polyfill yourself, to make sure you are adhering to the specification as strictly as possible.

Or better yet, use an already vetted set of polyfills that you can trust, such as those provided by ES5-Shim (https://github.com/es-shims/es5-shim) and ES6-Shim (https://github.com/es-shims/es6-shim).

### Transpiling

There's no way to polyfill new syntax that has been added to the language. The new syntax would throw an error in the old JS engine as unrecognized/invalid.

So the better option is to use a tool that converts your newer code into older code equivalents. This process is commonly called "transpiling," a term for transforming + compiling.

Essentially, your source code is authored in the new syntax form, but what you deploy to the browser is the transpiled code in old syntax form. You typically insert the transpiler into your build process, similar to your code linter or your minifier.

You might wonder why you'd go to the trouble to write new syntax only to have it transpiled away to older code -- why not just write the older code directly?

There are several important reasons you should care about transpiling:

* The new syntax added to the language is designed to make your code more readable and maintainable. The older equivalents are often much more convoluted. You should prefer writing newer and cleaner syntax, not only for yourself but for all other members of the development team.
* If you transpile only for older browsers, but serve the new syntax to the newest browsers, you get to take advantage of browser performance optimizations with the new syntax. This also lets browser makers have more real-world code to test their implementations and optimizations on.
* Using the new syntax earlier allows it to be tested more robustly in the real world, which provides earlier feedback to the JavaScript committee (TC39). If issues are found early enough, they can be changed/fixed before those language design mistakes become permanent.

Here's a quick example of transpiling. ES6 adds a feature called "default parameter values." It looks like this:

```js
function foo(a = 2) {
	console.log( a );
}

foo();		// 2
foo( 42 );	// 42
```

Simple, right? Helpful, too! But it's new syntax that's invalid in pre-ES6 engines. So what will a transpiler do with that code to make it run in older environments?

```js
function foo() {
	var a = arguments[0] !== (void 0) ? arguments[0] : 2;
	console.log( a );
}
```

As you can see, it checks to see if the `arguments[0]` value is `void 0` (aka `undefined`), and if so provides the `2` default value; otherwise, it assigns whatever was passed.

In addition to being able to now use the nicer syntax even in older browsers, looking at the transpiled code actually explains the intended behavior more clearly.

You may not have realized just from looking at the ES6 version that `undefined` is the only value that can't get explicitly passed in for a default-value parameter, but the transpiled code makes that much more clear.

The last important detail to emphasize about transpilers is that they should now be thought of as a standard part of the JS development ecosystem and process. JS is going to continue to evolve, much more quickly than before, so every few months new syntax and new features will be added.

If you use a transpiler by default, you'll always be able to make that switch to newer syntax whenever you find it useful, rather than always waiting for years for today's browsers to phase out.

There are quite a few great transpilers for you to choose from. Here are some good options at the time of this writing:

* Babel (https://babeljs.io) (formerly 6to5): Transpiles ES6+ into ES5
* Traceur (https://github.com/google/traceur-compiler): Transpiles ES6, ES7, and beyond into ES5

## Non-JavaScript

So far, the only things we've covered are in the JS language itself. The reality is that most JS is written to run in and interact with environments like browsers. A good chunk of the stuff that you write in your code is, strictly speaking, not directly controlled by JavaScript. That probably sounds a little strange.

The most common non-JavaScript JavaScript you'll encounter is the DOM API. For example:

```js
var el = document.getElementById( "foo" );
```

The `document` variable exists as a global variable when your code is running in a browser. It's not provided by the JS engine, nor is it particularly controlled by the JavaScript specification. It takes the form of something that looks an awful lot like a normal JS `object`, but it's not really exactly that. It's a special `object,` often called a "host object."

Moreover, the `getElementById(..)` method on `document` looks like a normal JS function, but it's just a thinly exposed interface to a built-in method provided by the DOM from your browser. In some (newer-generation) browsers, this layer may also be in JS, but traditionally the DOM and its behavior is implemented in something more like C/C++.

Another example is with input/output (I/O).

Everyone's favorite `alert(..)` pops up a message box in the user's browser window. `alert(..)` is provided to your JS program by the browser, not by the JS engine itself. The call you make sends the message to the browser internals and it handles drawing and displaying the message box.

The same goes with `console.log(..)`; your browser provides such mechanisms and hooks them up to the developer tools.

This book, and this whole series, focuses on JavaScript the language. That's why you don't see any substantial coverage of these non-JavaScript JavaScript mechanisms. Nevertheless, you need to be aware of them, as they'll be in every JS program you write!

## Review

The first step to learning JavaScript's flavor of programming is to get a basic understanding of its core mechanisms like values, types, function closures, `this`, and prototypes.

Of course, each of these topics deserves much greater coverage than you've seen here, but that's why they have chapters and books dedicated to them throughout the rest of this series. After you feel pretty comfortable with the concepts and code samples in this chapter, the rest of the series awaits you to really dig in and get to know the language deeply.

The final chapter of this book will briefly summarize each of the other titles in the series and the other concepts they cover besides what we've already explored.
