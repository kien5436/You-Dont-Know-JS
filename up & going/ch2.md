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

**Lưu ý:** Để tìm hiểu thêm về `object` trong JavaScript, hãy xem cuốn *this và nguyên mẫu đối tượng* trong loạt sách này, đặc biệt là chương 3.

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

**Lưu ý**: Để biết thêm thông tin về các giá trị và kiểu của chúng trong JS, hãy xem hai chương đầu tiên của cuốn *Kiểu dữ liệu và ngữ pháp* trong loạt sách này.

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

**Lưu ý:** Để tìm hiểu thêm về các đối tượng nguyên bản trong JavaScript và quá trình "đóng hộp", hãy xem chương 3 trong cuốn *Kiểu dữ liệu và ngữ pháp* của loạt sách này. Để hiểu rõ hơn về nguyên mẫu của một đối tượng, hãy xem chương 5 trong cuốn *this và nguyên mẫu đối tượng* của loạt sách này.

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

Toán tử `<`, `>`, `<=`, và `>=` được dùng để so sánh không bằng, trong đặc tả gọi là "so sánh quan hệ". Thông thường, chúng được dùng với các giá trị có thể so sánh theo thứ tự như `number`. Dễ thấy rằng `3 < 4`.

Tuy nhiên, các giá trị `string` trong JavaScript cũng có thể được so sánh không bằng, theo quy tắc bảng chữ cái thông thường (`"bar" < "foo"`).

Còn việc ép kiểu thì sao? Các quy tắc tương tự như phép so sánh `==` (dù không hoàn toàn giống nhau!) cũng được áp dụng cho các toán tử không bằng. Đáng chú ý là không có toán tử "không bằng nghiêm ngặt" nào không cho ép kiểu như `===` làm với so sánh bằng nghiêm ngặt.

Xem ví dụ:

```js
var a = 41;
var b = "42";
var c = "43";

a < b;        // true
b < c;        // true
```

Chuyện gì đang xảy ra? Phần 11.8.5 của đặc tả ES5 có ghi rằng nếu cả hai giá trị trong phép so sánh `<` đều là `string`, như với `b < c`, thì phép so sánh sẽ được thực hiện theo thứ tự từ điển (thứ tự sắp xếp của chữ cái trong từ điển). Nhưng nếu một hoặc cả hai không phải `string`, như với `a < b`, thì cả hai giá trị sẽ được ép kiểu sang `number` và phép so sánh số học thông thường được thực hiện.

Một lỗi thường gặp bạn có thể vướng phải khi so sánh các kiểu giá trị khác nhau - hãy nhớ rằng không có dạng "không bằng nghiêm ngặt" để dùng - là khi một trong các giá trị không thể chuyển thành số hợp lệ, ví dụ:

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

Thông thường, quy tắc tương tự cũng áp dụng cho tên thuộc tính như với tên biến. Tuy nhiên, có một số từ không thể dùng làm tên biến nhưng lại hợp lệ làm tên thuộc tính. Những từ này được gọi là "từ dành riêng" bao gồm các từ khóa của JS (`for`, `in`, `if`, v.v.) cũng như `null`, `true`, và `false`.

**Chú ý:** Để biết thêm về các từ dành riêng, xem Phụ lục A của cuốn *Kiểu dữ liệu và ngữ pháp* trong loạt sách này.

### Phạm vi của hàm

Bạn dùng từ khóa `var` để khai báo một biến sẽ thuộc về phạm vi của hàm hiện tại, hoặc phạm vi toàn cục nếu nằm ở cấp cao nhất bên ngoài mọi hàm.

### Kéo lên

Bất cứ khi nào `var` xuất hiện bên trong một phạm vi, khai báo đó sẽ được xem là thuộc về phạm vi đó và có thể truy cập ở bất kỳ đâu trong nó.

Một cách ẩn dụ, hành vi này được gọi là *kéo lên* khi một khai báo `var` coi như được "di chuyển" lên đầu của phạm vi bao quanh nó. Về mặt kỹ thuật, quá trình này được giải thích chính xác hơn bởi cách mã được biên dịch, nhưng ta sẽ bỏ qua chi tiết đó lúc này.

Xem ví dụ:

```js
var a = 2;
foo();                    // chạy được vì `foo()` đã được "kéo lên"

function foo() {
    a = 3;
    console.log(a);       // 3
    var a;                // khai báo được "kéo lên" đầu hàm `foo()`
}

console.log(a);           // 2
```

**Cảnh báo:** Việc dựa vào *kéo lên* để sử dụng biến trước khi khai báo bằng `var` trong cùng một phạm vi không phải là một ý tưởng hay hoặc phổ biến - nó dễ gây nhầm lẫn. Tuy nhiên, khai báo hàm thì có thể chấp nhận vì hàm được *kéo lên*, như trong ví dụ gọi `foo()` phía trên.

### Phạm vi lồng nhau

Khi bạn khai báo một biến, nó có thể được sử dụng ở bất kỳ đâu trong phạm vi hiện tại cũng như các phạm vi thấp hơn/nhỏ hơn. Ví dụ:

```js
function foo() {
    var a = 1;

    function bar() {
        var b = 2;

        function baz() {
            var c = 3;
            console.log(a, b, c);  // 1 2 3
        }

        baz();
        console.log(a, b);        // 1 2
    }

    bar();
    console.log(a);              // 1
}

foo();
```

Chú ý rằng `c` không có sẵn bên trong `bar()` vì nó chỉ được khai báo trong phạm vi `baz()`. Tương tự, `b` không có sẵn cho `foo()` vì cùng lí do.

Nếu bạn cố gắng truy cập một biến không có trong phạm vi hiện tại, bạn sẽ gặp lỗi `ReferenceError`. Nếu bạn gán giá trị cho một biến chưa khai báo, bạn có thể vô tình tạo ra một biến toàn cục (rất tệ!), hoặc gặp lỗi nếu đang ở chế độ nghiêm ngặt (Xem phần "Chế độ nghiêm ngặt"). Hãy xem xét ví dụ sau:

```js
function foo() {
    a = 1;   // `a` chưa được khai báo chính thức
}

foo();

a;          // 1 - ôi không, biến toàn cục được tạo ra tự động :(
```

Đây là một cách làm rất tệ. Đừng làm vậy! Hãy luôn luôn khai báo rõ ràng các biến của bạn.

Ngoài việc tạo khai báo biến ở cấp độ hàm, ES6 cho phép bạn khai báo biến ở phạm vi khối (các cặp `{ .. }`) bằng từ khóa `let`. Về cơ bản, các quy tắc phạm vi sẽ hoạt động gần giống như những gì chúng ta vừa thấy với các hàm:

```js
function foo() {
    var a = 1;

    if (a >= 1) {
        let b = 2;

        while (b < 5) {
            let c = b * 2;
            b++;
            console.log(a + c);
        }
    }
}

foo(); // 5 7 9
```

Sử dụng `let` thay vì `var`, biến `b` chỉ tồn tại trong khối `if`, không thuộc toàn bộ hàm `foo()`. Tương tự, `c` chỉ tồn tại trong vòng lặp `while`. Phạm vi khối rất hữu ích cho việc quản lý biến một cách chi tiết hơn, giúp mã của bạn dễ bảo trì hơn theo thời gian.

**Lưu ý:** Xem thêm chủ đề *Phạm vi và hàm khép kín* trong loạt sách này để hiểu thêm về phạm vi. Và xem *ES6 và hơn thế nữa* để tìm hiểu thêm về phạm vi khối với `let`.

## Câu lệnh điều kiện

Ngoài câu lệnh `if` được giới thiệu sơ lược ở chương 1, JavaScript cung cấp thêm một vài cơ chế điều kiện khác mà bạn nên xem qua.

Đôi khi bạn có thể viết một chuỗi câu lệnh `if..else..if` như sau:

```js
if (a == 2) {
    // làm gì đó
} else if (a == 10) {
    // làm việc khác
} else if (a == 42) {
    // làm việc khác nữa
} else {
    // các trường hợp khác vào đây
}
```

Cấu trúc này hoạt động tốt nhưng hơi dài vì bạn phải lặp lại việc kiểm tra `a` cho mỗi trường hợp. Đây là một lựa chọn khác: câu lệnh `switch`:

```js
switch (a) {
    case 2:
        // làm gì đó
        break;
    case 10:
        // làm việc khác
        break;
    case 42:
        // làm việc khác nữa
        break;
    default:
        // các trường hợp khác vào đây
}
```

Từ khóa `break` rất quan trọng nếu bạn chỉ muốn các câu lệnh trong một `case` chạy. Nếu bạn bỏ `break` trong một `case`, và `case` đó khớp, việc thực thi sẽ tiếp tục sang các câu lệnh của `case` tiếp theo bất kể nó có khớp hay không. Điều này còn được gọi là "rơi tiếp", đôi khi lại hữu ích/mong muốn:

```js
switch (a) {
    case 2:
    case 10:
        // vài thao tác
        break;
    case 42:
        // thao tác khác
        break;
    default:
        // các trường hợp khác vào đây
}
```

Ở đây, nếu `a` là `2` hoặc `10`, nó sẽ thực thi đoạn mã trong phần "vài thao tác".

Một dạng điều kiện khác trong JavaScript là "toán tử điều kiện", thường gọi là "toán tử ba ngôi". Nó là một dạng ngắn gọn hơn của `if..else` như sau:

```js
var a = 42;
var b = (a > 41) ? "hello" : "world";

// tương đương:
// if (a > 41) {
//     b = "hello";
// } else {
//     b = "world";
// }
```

Nếu biểu thức kiểm tra (`a > 41`) trả về `true`, phần đầu (`"hello"`) sẽ được chọn, ngược lại là phần sau (`"world"`), và kết quả sẽ được gán cho `b`.

Toán tử điều kiện không bắt buộc phải dùng để gán, nhưng đây là cách dùng phổ biến nhất.

**Lưu ý:** Để biết thêm về kiểm tra điều kiện và các khuôn mẫu sử dụng `switch` và `? :`, xem phần *Kiểu dữ liệu và ngữ pháp* trong loạt sách này.

## Chế độ nghiêm ngặt

ES5 đã thêm "chế độ nghiêm ngặt" vào ngôn ngữ, giúp siết chặt các hành vi nhất định. Nói chung, các giới hạn này được xem là giúp mã an toàn và hợp lý hơn. Ngoài ra, tuân theo chế độ nghiêm ngặt cũng giúp mã của bạn được tối ưu hóa hơn bởi công cụ. Chế độ nghiêm ngặt là một cải tiến lớn mà bạn nên dùng cho tất cả chương trình của mình.

Bạn có thể bật chế độ nghiêm ngặt cho một hàm riêng lẻ hoặc cho toàn bộ tập tin, tùy vào nơi đặt chỉ thị:

```js
function foo() {
    "use strict";
    // mã ở đây thuộc chế độ nghiêm ngặt
    function bar() {
        // mã ở đây cũng thuộc chế độ nghiêm ngặt
    }
}
// mã ở ngoài không thuộc chế độ nghiêm ngặt
```

So với:

```js
"use strict";
function foo() {
    // mã ở đây thuộc chế độ nghiêm ngặt
    function bar() {
        // mã ở đây cũng thuộc chế độ nghiêm ngặt
    }
}
// mã ở ngoài cũng thuộc chế độ nghiêm ngặt
```

Một điểm khác biệt chính (cải tiến!) của chế độ nghiêm ngặt là không cho phép tạo biến toàn cục một cách ngầm định nếu thiếu `var`:

```js
function foo() {
    "use strict";
    a = 1;  // thiếu `var`, gây lỗi ReferenceError
}

foo();
```

Nếu bạn bật chế độ nghiêm ngặt và thấy lỗi, hoặc mã hoạt động kỳ lạ, có thể bạn sẽ muốn bỏ chế độ nghiêm ngặt. Nhưng điều đó là sai lầm. Nếu chế độ nghiêm ngặt gây ra lỗi, gần như chắc chắn là chương trình của bạn đang có vấn đề cần sửa.

Chế độ nghiêm ngặt không chỉ giúp mã an toàn hơn, dễ tối ưu hơn, mà còn phản ánh định hướng tương lai của ngôn ngữ. Làm quen với chế độ nghiêm ngặt từ bây giờ sẽ dễ hơn nhiều so với việc trì hoãn và phải chuyển đổi sau này.

**Lưu ý:** Để biết thêm về chế độ nghiêm ngặt, xem chương 5 trong *Kiểu dữ liệu và ngữ pháp*.

## Hàm như giá trị

Cho đến giờ, ta đã nói về hàm như cơ chế chính để tạo *phạm vi* trong JavaScript. Cú pháp khai báo hàm thường như sau:

```js
function foo() {
    // ..
}
```

Mặc dù cú pháp này có vẻ không rõ ràng nhưng thật ra `foo` chỉ là một biến trong phạm vi bao ngoài, được gán tham chiếu đến một giá trị kiểu `function`. Tức là bản thân hàm cũng là một giá trị giống như `42` hoặc `[1, 2, 3]`.

Ban đầu điều này có thể nghe hơi lạ nên bạn hãy dừng lại và suy nghĩ một chút. Không chỉ có thể truyền một giá trị (tham số) *vào* hàm, mà *chính hàm cũng có thể là giá trị* để gán cho biến, truyền vào hoặc trả về từ một hàm khác.

Vì vậy, giá trị hàm nên được xem như một biểu thức, tương tự như mọi giá trị hoặc biểu thức khác.

Xem ví dụ:

```js
var foo = function() {
    // ..
};

var x = function bar() {
    // ..
};
```

Hàm đầu tiên gán cho biến `foo` là *hàm ẩn danh* vì nó không có tên.

Hàm thứ hai là *hàm có tên* (`bar`) dù nó cũng được gán cho biến `x`. *Hàm có tên* thường được ưu tiên hơn, dù *hàm ẩn danh* vẫn rất phổ biến.

Để tìm hiểu thêm, xem phần *Phạm vi và hàm khép kín* trong loạt sách này.

### Biểu thức hàm được gọi ngay lập tức (IIFE)

Trong đoạn mã trước, cả hai biểu thức hàm đều không được thực thi - ta có thể làm thế nếu thêm `foo()` hoặc `x()` chẳng hạn.

Có một cách khác để thực thi một biểu thức hàm thường được gọi là *biểu thức hàm được gọi ngay lập tức* (Immediately Invoked Function Expression - IIFE):

```js
(function IIFE(){
    console.log( "Hello!" );
})();
```

Cặp dấu ngoặc ngoài `( .. )` bao quanh `(function IIFE(){ .. })` chỉ là một chi tiết ngữ pháp của JS để ngăn nó bị xử lý như một khai báo hàm thông thường.

Cặp `()` ở cuối dòng `})();` là phần thực thi biểu thức hàm vừa được tham chiếu ngay trước đó.

Nghe có vẻ lạ, nhưng không quá khác biệt như tưởng tượng. Xét sự tương đồng giữa `foo` và `IIFE`:

```js
function foo() { .. }

// tham chiếu đến hàm `foo`,
// sau đó `()` thực thi nó
foo();

// biểu thức hàm `IIFE`,
// sau đó `()` thực thi nó
(function IIFE(){ .. })();
```

Như bạn thấy, việc đặt `(function IIFE(){ .. })` trước dấu `()` thực thi nó về bản chất giống như đặt `foo` trước dấu `()` - trong cả hai trường hợp, tham chiếu hàm được thực thi ngay lập tức bằng `()`.

Vì IIFE chỉ là một hàm, và hàm tạo ra *phạm vi* biến nên việc dùng IIFE theo cách này thường được áp dụng để khai báo các biến không ảnh hưởng đến mã bên ngoài IIFE:

```js
var a = 42;

(function IIFE(){
    var a = 10;
    console.log( a );
    // 10
})();

console.log( a );        // 42
```

IIFE cũng có thể trả về giá trị:

```js
var x = (function IIFE(){
    return 42;
})();

x;    // 42
```

Giá trị `42` được `return` từ hàm tên `IIFE` khi được thực thi, sau đó được gán cho `x`.

### Hàm khép kín

*Hàm khép kín* là một trong những khái niệm quan trọng nhất và thường bị hiểu sai nhất trong JavaScript. Tôi sẽ không trình bày quá sâu ở đây mà giới thiệu bạn đến phần *Phạm vi và Hàm khép kín* trong loạt sách này. Nhưng tôi muốn nói một vài điều để bạn hiểu được khái niệm chung. Đây sẽ là một trong những kỹ thuật quan trọng nhất trong bộ kỹ năng JS của bạn.

Bạn có thể hiểu hàm khép kín là một cách để "ghi nhớ" và tiếp tục truy cập vào phạm vi của một hàm (các biến của nó) ngay cả khi hàm đó đã thực thi xong.

Xét ví dụ:

```js
function makeAdder(x) {
    // tham số `x` là một biến bên trong

    // hàm bên trong `add()` dùng `x`, nên
    // nó có "sự khép kín" với `x`
    function add(y) {
        return y + x;
    };
    return add;
}
```

Tham chiếu đến hàm `add(..)` bên trong được trả về mỗi lần gọi đến `makeAdder(..)` bên ngoài có thể ghi nhớ giá trị `x` đã được truyền vào `makeAdder(..)`. Bây giờ, ta sử dụng `makeAdder(..)`:

```js
var plusOne = makeAdder( 1 );
var plusTen = makeAdder( 10 );

plusOne( 3 );         // 4  <-- 1 + 3
plusOne( 41 );        // 42 <-- 1 + 41
plusTen( 13 );        // 23 <-- 10 + 13
```

Cách đoạn mã trên hoạt động:

1. Khi gọi `makeAdder(1)`, ta nhận được một tham chiếu đến hàm `add(..)` bên trong mà ghi nhớ `x = 1`. Ta gọi tham chiếu này là `plusOne(..)`.

2. Khi gọi `makeAdder(10)`, ta nhận được một tham chiếu khác đến hàm `add(..)` bên trong mà ghi nhớ `x = 10`. Ta gọi tham chiếu này là `plusTen(..)`.

3. Khi gọi `plusOne(3)`, nó cộng `3` (là `y`) với `1` (được `x` ghi nhớ) và trả về `4`.

4. Khi gọi `plusTen(13)`, nó cộng `13` (là `y`) với `10` (được `x` ghi nhớ) và trả về `23`.

Đừng lo nếu điều này có vẻ lạ và khó hiểu lúc đầu - có thể đúng là như vậy! Bạn sẽ cần luyện tập nhiều để thực sự hiểu rõ.

Nhưng tin tôi đi, một khi hiểu được, đây là một trong những kỹ thuật mạnh mẽ và hữu dụng nhất trong lập trình. Rất đáng để dành thời gian suy ngẫm về hàm khép kín. Trong phần tiếp theo, ta sẽ luyện tập thêm một chút với hàm khép kín.

#### Khối chức năng 

Cách sử dụng hàm khép kín phổ biến nhất trong JavaScript là mô hình khối chức năng. Khối chức năng cho phép bạn định nghĩa các chi tiết triển khai riêng tư (biến, hàm) ẩn khỏi thế giới bên ngoài, cùng với một API công khai *có thể* được truy cập từ bên ngoài.

Xem ví dụ:

```js
function User(){
    var username, password;

    function doLogin(user, pw) {
        username = user;
        password = pw;
        // thực hiện phần còn lại của đăng nhập
    }

    var publicAPI = {
        login: doLogin
    };

    return publicAPI;
}

// tạo một phiên bản khối chức năng `User`
var fred = User();

fred.login( "fred", "12Battery34!" );
```

Hàm `User()` đóng vai trò là phạm vi bên ngoài chứa các biến `username` và `password`, cũng như hàm `doLogin()` bên trong; tất cả đều là các chi tiết bên trong riêng tư của khối `User` này và không thể truy cập từ bên ngoài.

**Cảnh báo:** Ta không gọi `new User()` ở đây là có chủ đích, mặc dù điều này có thể sẽ quen thuộc hơn với đa số lập trình viên. `User()` chỉ là một hàm, không phải một lớp để khởi tạo, nên chỉ cần gọi bình thường. Dùng `new` là không phù hợp và thực tế còn lãng phí tài nguyên.

Gọi `User()` sẽ tạo ra một *phiên bản* của khối `User` - một phạm vi mới được tạo ra, và do đó là một bản sao hoàn toàn mới của mỗi biến/hàm bên trong. Ta gán phiên bản này vào biến `fred`. Nếu gọi `User()` lần nữa, ta sẽ có một phiên bản mới hoàn toàn tách biệt với `fred`.

Hàm `doLogin()` bên trong có một hàm khép kín với `username` và `password`, nghĩa là nó vẫn giữ được quyền truy cập đến chúng ngay cả sau khi hàm `User()` kết thúc thực thi.

`publicAPI` là một đối tượng có một thuộc tính/phương thức là `login`, chính là một tham chiếu đến hàm `doLogin()` bên trong. Khi ta trả về `publicAPI` từ `User()`, nó trở thành phiên bản mà ta gán vào `fred`.

Tại thời điểm này, hàm `User()` bên ngoài đã kết thúc thực thi. Thông thường, ta sẽ nghĩ rằng các biến bên trong như `username` và `password` sẽ biến mất. Nhưng ở đây thì không, vì hàm khép kín trong hàm `login()` vẫn giữ chúng tồn tại.

Đó là lý do vì sao ta có thể gọi `fred.login(..)` - thực chất là gọi `doLogin(..)` bên trong - và nó vẫn có thể truy cập đến các biến `username` và `password` bên trong.

Có khả năng cao là chỉ với cái nhìn thoáng qua về hàm khép kín và mô hình khối chức năng như trên, một số phần vẫn còn khá mơ hồ. Không sao cả! Cần có thời gian để não bộ của bạn tiếp nhận và hiểu rõ vấn đề này.

Từ đây, hãy đọc phần *Phạm vi và Hàm khép kín* trong loạt sách này để khám phá sâu hơn.

## Định danh `this`

Một khái niệm khác cũng thường bị hiểu nhầm trong JavaScript là định danh `this`. Một lần nữa, có vài chương về nó trong phần *this và Nguyên mẫu đối tượng* của loạt sách này, nên ở đây ta chỉ giới thiệu ngắn gọn.

Dù `this` thường có vẻ như liên quan đến "mô hình hướng đối tượng", nhưng trong JS `this` là một cơ chế khác.

Nếu một hàm có tham chiếu `this` bên trong, thì `this` thường trỏ đến một `object`. Nhưng `object` nào thì tùy thuộc vào cách hàm được gọi.

Điều quan trọng là hãy hiểu rằng `this` *không* trỏ đến chính bản thân hàm, đây là một hiểu lầm phổ biến nhất.

Dưới đây là ví dụ minh họa nhanh:

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
foo();               // "global"
obj1.foo();          // "obj1"
foo.call( obj2 );    // "obj2"
new foo();           // undefined
```

Có bốn quy tắc để xác định `this`, và chúng được thể hiện trong bốn dòng cuối cùng của đoạn mã trên:

1. `foo()` coi `this` là đối tượng toàn cục - trong chế độ nghiêm ngặt, `this` sẽ là `undefined` và bạn sẽ gặp lỗi khi truy cập thuộc tính `bar` - nên `"global"` là giá trị tìm được từ `this.bar`.

2. `obj1.foo()` gán `this` cho đối tượng `obj1`.

3. `foo.call(obj2)` gán `this` cho đối tượng `obj2`.

4. `new foo()` gán `this` cho một đối tượng rỗng mới hoàn toàn.

Tóm lại: để hiểu `this` trỏ đến đâu, bạn phải xem xét cách mà hàm đó được gọi. Nó sẽ rơi vào một trong bốn cách vừa liệt kê, và điều đó sẽ quyết định `this` là gì.

**Lưu ý:** Để tìm hiểu thêm về `this`, hãy xem chương 1 và 2 trong phần *this và nguyên mẫu đối tượng* của loạt sách này.

## Nguyên mẫu

Cơ chế nguyên mẫu trong JavaScript khá phức tạp. Ở đây, chúng ta chỉ lướt qua nó. Bạn sẽ muốn dành nhiều thời gian xem lại chương 4-6 của cuốn "this và nguyên mẫu đối tượng" trong loạt sách này để biết mọi chi tiết.

Khi bạn tham chiếu đến một thuộc tính trên một đối tượng, nếu thuộc tính đó không tồn tại, JavaScript sẽ tự động sử dụng tham chiếu nguyên mẫu nội bộ của đối tượng đó để tìm một đối tượng khác và kiểm tra thuộc tính trên đó. Bạn có thể xem điều này như một phương án dự phòng nếu thuộc tính bị thiếu.

Liên kết tham chiếu nguyên mẫu nội bộ từ một đối tượng đến đối tượng dự phòng của nó xảy ra vào thời điểm đối tượng được tạo. Cách đơn giản nhất để minh họa điều này là với một tiện ích dựng sẵn có tên là `Object.create(..)`.

Xem xét ví dụ:

```js
var foo = {
    a: 42
};

// tạo `bar` và liên kết nó với `foo`
var bar = Object.create( foo );

bar.b = "hello world";

bar.b;        // "hello world"
bar.a;        // 42 <-- được ủy nhiệm cho `foo`
```

Có thể sẽ hữu ích nếu bạn hình dung các đối tượng `foo` và `bar` cùng mối quan hệ của chúng như sau:

<img src="fig6.png">

Thuộc tính `a` thực tế không tồn tại trên đối tượng `bar`, nhưng vì `bar` được liên kết nguyên mẫu với `foo`, JavaScript sẽ tự động tìm đến `a` trên đối tượng `foo`, nơi nó được tìm thấy.

Sự liên kết này có thể trông giống như một đặc điểm kỳ lạ của ngôn ngữ. Cách phổ biến nhất mà tính năng này được sử dụng - và tôi cho rằng là lạm dụng - là để cố gắng bắt chước/giả lập một cơ chế "lớp" với "sự kế thừa".

Nhưng một cách áp dụng tự nhiên hơn của nguyên mẫu là một mô hình được gọi là "ủy nhiệm hành vi", nơi bạn cố ý thiết kế các đối tượng được liên kết để có thể *ủy nhiệm* từ đối tượng này sang đối tượng kia cho các phần hành vi cần thiết.

**Lưu ý:** Để biết thêm thông tin về nguyên mẫu và ủy nhiệm hành vi, xem chương 4-6 của tiêu đề "this và nguyên mẫu đối tượng" trong loạt sách này.

## Cũ & Mới

Một số tính năng JS mà chúng ta đã đề cập, và chắc chắn nhiều tính năng sẽ được đề cập trong phần còn lại của loạt sách này, là những bổ sung mới hơn và sẽ không nhất thiết có sẵn trong các trình duyệt cũ. Thực tế là một số tính năng mới nhất trong đặc tả thậm chí còn chưa được triển khai trong bất kỳ trình duyệt ổn định nào.

Vậy bạn làm gì với các tính năng mới? Có phải bạn chỉ ngồi chờ nhiều năm hoặc nhiều thập kỷ cho đến khi tất cả các trình duyệt cũ trở nên lỗi thời?

Đó là cách nhiều người nghĩ về tình huống này, nhưng thực sự đây không phải là cách tiếp cận lành mạnh với JS.

Có hai kỹ thuật chính bạn có thể sử dụng để "mang" những tính năng JavaScript mới đến trình duyệt cũ: vá lỗ hổng và biên dịch chuyển đổi.

### Vá lỗ hổng

Từ "vá lỗ hổng" là một thuật ngữ được sáng tạo ra (bởi Remy Sharp) ([https://remysharp.com/2010/10/08/what-is-a-vá lỗ hổng](https://remysharp.com/2010/10/08/what-is-a-vá lỗ hổng)) để chỉ việc lấy định nghĩa của một tính năng mới và tạo ra một đoạn mã có hành vi tương đương, nhưng có thể chạy trong môi trường JS cũ hơn.

Ví dụ, ES6 định nghĩa một tiện ích có tên `Number.isNaN(..)` để cung cấp một cách kiểm tra chính xác và không lỗi đối với giá trị `NaN`, thay thế tiện ích `isNaN(..)` cũ. Nhưng việc vá lỗ hổng tiện ích đó rất dễ để bạn có thể bắt đầu sử dụng nó trong mã của mình bất kể người dùng đang dùng trình duyệt ES6 hay không.

Xem ví dụ:

```js
if (!Number.isNaN) {
    Number.isNaN = function isNaN(x) {
        return x !== x;
    };
}
```

Câu lệnh `if` giúp tránh việc áp dụng định nghĩa vá lỗ hổng trong các trình duyệt ES6 nơi tiện ích đó đã tồn tại. Nếu chưa tồn tại, ta định nghĩa `Number.isNaN(..)`.

**Lưu ý:** Việc kiểm tra ở đây tận dụng một đặc điểm kỳ lạ của giá trị `NaN`: nó là giá trị duy nhất trong toàn bộ ngôn ngữ không bằng chính nó. Vì vậy `NaN` là giá trị duy nhất khiến `x !== x` trả về `true`.

Không phải tất cả các tính năng mới đều có thể vá lỗ hổng đầy đủ. Đôi khi hầu hết hành vi có thể được vá nhưng vẫn có một vài sai khác nhỏ. Bạn cần thực sự cẩn thận khi tự viết một bản vá để đảm bảo tuân thủ đặc tả càng chặt chẽ càng tốt.

Hoặc tốt hơn hết là sử dụng một bộ vá lỗ hổng đã được kiểm chứng, chẳng hạn như ES5-Shim ([https://github.com/es-shims/es5-shim](https://github.com/es-shims/es5-shim)) và ES6-Shim ([https://github.com/es-shims/es6-shim](https://github.com/es-shims/es6-shim)).

### Chuyển mã

Không có cách nào để vá lỗ hổng cú pháp mới được thêm vào ngôn ngữ. Cú pháp mới sẽ gây lỗi trong các chương trình thông dịch JS cũ vì không thể nhận diện/không hợp lệ.

Vì vậy, lựa chọn tốt hơn là sử dụng một công cụ chuyển đổi mã mới thành mã cũ tương đương. Quá trình này thường được gọi là "chuyển mã" (chuyển mã), thuật ngữ được tạo thành từ chuyển đổi (transform) + biên dịch (compile).

Về cơ bản, mã nguồn của bạn được viết bằng cú pháp mới, nhưng những gì bạn triển khai lên trình duyệt là mã đã được chuyển đổi sang cú pháp cũ. Bạn thường sẽ tích hợp công cụ chuyển mã vào quy trình xây dựng của mình, tương tự như chương trình kiểm tra mã hoặc chương trình nén mã.

Bạn có thể thắc mắc tại sao lại phải viết cú pháp mới rồi lại chuyển đổi về cú pháp cũ - tại sao không viết mã cũ ngay từ đầu?

Có một số lý do quan trọng khiến bạn nên quan tâm đến việc chuyển mã:

* Cú pháp mới được thêm vào ngôn ngữ được thiết kế để làm cho mã của bạn dễ đọc và bảo trì hơn. Cách viết cũ thường rối rắm hơn rất nhiều. Bạn nên ưu tiên viết cú pháp mới và sáng sủa hơn, không chỉ cho bạn mà còn cho tất cả thành viên khác trong nhóm phát triển.

* Nếu bạn chỉ chuyển mã cho trình duyệt cũ, nhưng cung cấp cú pháp mới cho trình duyệt mới nhất, bạn có thể tận dụng các tối ưu hiệu năng trình duyệt với cú pháp mới. Điều này cũng cho phép các nhà phát triển trình duyệt có mã thực tế để kiểm thử và tối ưu.

* Việc sử dụng cú pháp mới sớm hơn cho phép nó được kiểm thử nhiều hơn trong thực tế, giúp Ủy ban JavaScript (TC39) nhận phản hồi sớm hơn. Nếu có vấn đề được phát hiện đủ sớm, chúng có thể được sửa trước khi các sai lầm thiết kế ngôn ngữ trở nên cố định vĩnh viễn.

Dưới đây là một ví dụ nhanh về chuyển mã. ES6 thêm một tính năng gọi là "giá trị tham số mặc định". Nó trông như sau:

```js
function foo(a = 2) {
    console.log( a );
}

foo();        // 2
foo( 42 );    // 42
```

Đơn giản phải không? Và cũng hữu ích nữa! Nhưng đây là cú pháp mới không hợp lệ trong các chương trình thông dịch trước ES6. Vậy công cụ chuyển mã sẽ làm gì với đoạn mã đó để chạy được trong môi trường cũ?

```js
function foo() {
    var a = arguments[0] !== (void 0) ? arguments[0] : 2;
    console.log( a );
}
```

Như bạn thấy, nó kiểm tra xem giá trị `arguments[0]` có phải là `void 0` (tức là `undefined`) hay không, và nếu có thì cung cấp giá trị mặc định là `2`; nếu không, nó gán giá trị đã truyền vào.

Ngoài việc có thể sử dụng cú pháp tốt hơn ngay cả trong trình duyệt cũ, việc xem mã sau khi chuyển đổi thực sự giúp giải thích hành vi mong muốn một cách rõ ràng hơn.

Bạn có thể chưa nhận ra chỉ từ việc nhìn vào cú pháp ES6 rằng `undefined` là giá trị duy nhất không thể được truyền vào một cách rõ ràng cho một tham số có giá trị mặc định, nhưng mã đã được chuyển đổi làm điều đó rõ ràng hơn nhiều.

Chi tiết cuối cùng quan trọng cần nhấn mạnh về các công cụ chuyển mã là: chúng nên được coi là một phần tiêu chuẩn trong hệ sinh thái và quy trình phát triển JavaScript. JS sẽ tiếp tục phát triển nhanh hơn trước rất nhiều, vì vậy cứ vài tháng lại có cú pháp và tính năng mới được thêm vào.

Nếu bạn mặc định sử dụng công cụ chuyển mã, bạn sẽ luôn có thể chuyển sang cú pháp mới bất cứ khi nào thấy hữu ích thay vì luôn phải chờ nhiều năm cho đến khi trình duyệt hôm nay trở nên lỗi thời.

Hiện tại có khá nhiều công cụ chuyển mã tuyệt vời để bạn lựa chọn. Dưới đây là một vài cái tên tốt vào thời điểm viết cuốn sách này:

* Babel ([https://babeljs.io](https://babeljs.io)) (trước đây là 6to5): Chuyển mã ES6+ sang ES5
* Traceur ([https://github.com/google/traceur-compiler](https://github.com/google/traceur-compiler)): Chuyển mã ES6, ES7 và hơn nữa sang ES5

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

The first step to learning JavaScript's flavor of programming is to get a basic understanding of its core mechanisms like values, types, function hàm khép kíns, `this`, and prototypes.

Of course, each of these topics deserves much greater coverage than you've seen here, but that's why they have chapters and books dedicated to them throughout the rest of this series. After you feel pretty comfortable with the concepts and code samples in this chapter, the rest of the series awaits you to really dig in and get to know the language deeply.

The final chapter of this book will briefly summarize each of the other titles in the series and the other concepts they cover besides what we've already explored.
