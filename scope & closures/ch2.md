# Bạn không hiểu JS: Phạm vi và Hàm khép kín
# Chương 2: Phạm vi Từ vựng

Ở chương 1, chúng ta đã định nghĩa "phạm vi" là một tập hợp các quy tắc chi phối cách *Bộ máy* có thể tra cứu một biến bằng tên định danh của nó và tìm ra nó, hoặc trong *Phạm vi* hiện tại, hoặc trong bất kỳ *Phạm vi Lồng nhau* nào chứa nó.

Có hai mô hình nổi trội về cách hoạt động của phạm vi. Mô hình đầu tiên phổ biến hơn cả, được đại đa số các ngôn ngữ lập trình sử dụng. Nó được gọi là **Phạm vi Từ vựng (Lexical Scope)**, chúng ta sẽ xem xét nó một cách sâu sắc. Mô hình còn lại, vẫn được một số ngôn ngữ sử dụng (như ngôn ngữ kịch bản Bash, một số chế độ trong Perl, v.v.) được gọi là **Phạm vi Động (Dynamic Scope)**.

Phạm vi Động được trình bày trong Phụ lục A. Tôi đề cập đến nó ở đây chỉ để tạo ra sự tương phản với Phạm vi Từ vựng, vốn là mô hình phạm vi mà JavaScript sử dụng.

## Thời điểm Phân tích Từ vựng (Lex-time)

Như chúng ta đã thảo luận trong chương 1, giai đoạn truyền thống đầu tiên của một chương trình biên dịch ngôn ngữ tiêu chuẩn được gọi là phân tích từ vựng (hay còn gọi là phân đoạn). Nếu bạn còn nhớ, quá trình phân tích từ vựng sẽ kiểm tra một chuỗi các ký tự mã nguồn và gán ngữ nghĩa cho các đoạn như là kết quả của một quá trình phân tích có trạng thái.

Chính khái niệm này cung cấp nền tảng để hiểu phạm vi từ vựng là gì và tên gọi của nó xuất phát từ đâu.

Có thể định nghĩa một cách hơi lòng vòng rằng, phạm vi từ vựng là phạm vi được xác định tại thời điểm phân tích từ vựng. Nói cách khác, phạm vi từ vựng dựa trên nơi các biến và các khối phạm vi được bạn viết ra, tại thời điểm viết mã, và do đó (hầu như) đã được định hình vào lúc chương trình phân tích từ vựng xử lý mã của bạn.

**Lưu ý:** Lát nữa chúng ta sẽ thấy có một số cách để lách luật phạm vi từ vựng, qua đó sửa đổi nó sau khi chương trình phân tích từ vựng đã thông qua, nhưng những cách này không được tán thành. Cách làm tốt nhất trong thực tế là hãy đối xử với phạm vi từ vựng như đúng bản chất của nó, chỉ mang tính từ vựng, và do đó hoàn toàn phụ thuộc vào thời điểm viết mã.

Hãy xem xét khối mã này:

```js
function foo(a) {

	var b = a * 2;

	function bar(c) {
		console.log( a, b, c );
	}

	bar(b * 3);
}

foo( 2 ); // 2 4 12
```

Có ba phạm vi lồng nhau vốn có trong mã ví dụ này. Sẽ hữu ích nếu bạn hình dung các phạm vi này như những bong bóng lồng vào nhau.

<img src="fig2.png" width="500">

**Bong bóng 1** bao trùm phạm vi toàn cục, và chỉ có một định danh trong đó: `foo`.

**Bong bóng 2** bao trùm phạm vi của `foo`, bao gồm ba định danh: `a`, `bar` và `b`.

**Bong bóng 3** bao trùm phạm vi của `bar`, và nó chỉ bao gồm một định danh: `c`.

Các bong bóng phạm vi được xác định bởi nơi các khối phạm vi được viết, cái nào được lồng bên trong cái nào, v.v. Trong chương tiếp theo, chúng ta sẽ thảo luận về các đơn vị phạm vi khác nhau, nhưng bây giờ, hãy cứ cho rằng mỗi hàm tạo ra một bong bóng phạm vi mới.

Bong bóng của `bar` hoàn toàn nằm trong bong bóng của `foo`, bởi vì (và chỉ bởi vì) đó là nơi chúng ta đã chọn để định nghĩa hàm `bar`.

Lưu ý rằng các bong bóng này được lồng nhau một cách nghiêm ngặt. Chúng ta không nói về các biểu đồ Venn nơi các bong bóng có thể cắt nhau ở viền. Nói cách khác, không có bong bóng của một hàm nào có thể đồng thời tồn tại (một phần) bên trong hai bong bóng phạm vi bên ngoài khác, cũng như không có hàm nào có thể nằm một phần bên trong hai hàm cha.

### Tra cứu

Cấu trúc và vị trí tương đối của các bong bóng phạm vi này giải thích đầy đủ cho *Bộ máy* tất cả những nơi nó cần để tìm thấy một định danh.

Trong đoạn mã trên, *Bộ máy* thực thi câu lệnh `console.log(..)` và bắt đầu tìm kiếm ba biến được tham chiếu là `a`, `b`, và `c`. Nó bắt đầu với bong bóng phạm vi trong cùng, phạm vi của hàm `bar(..)`. Nó sẽ không tìm thấy `a` ở đó, vì vậy nó đi lên một cấp, ra bong bóng phạm vi gần nhất tiếp theo, phạm vi của `foo(..)`. Nó tìm thấy `a` ở đó, và vì vậy nó sử dụng `a` đó. Điều tương tự cũng xảy ra với `b`. Nhưng với `c`, nó tìm thấy ngay bên trong `bar(..)`.

Nếu có một biến `c` tồn tại cả bên trong `bar(..)` và bên trong `foo(..)`, câu lệnh `console.log(..)` sẽ tìm thấy và sử dụng biến trong `bar(..)`, không bao giờ chạm tới biến trong `foo(..)`.

**Việc tra cứu phạm vi sẽ dừng lại ngay khi tìm thấy kết quả khớp đầu tiên**. Cùng một tên định danh có thể được chỉ định ở nhiều lớp phạm vi lồng nhau, điều này được gọi là "che khuất" (định danh bên trong "che khuất" định danh bên ngoài). Bất kể có sự che khuất hay không, việc tra cứu phạm vi luôn bắt đầu ở phạm vi trong cùng đang được thực thi tại thời điểm đó, và tiến dần ra ngoài/lên trên cho đến khi gặp kết quả khớp đầu tiên, và dừng lại.

**Lưu ý:** Các biến toàn cục cũng tự động là thuộc tính của đối tượng toàn cục (`window` trong chương trình duyệt, v.v.), vì vậy *có thể* tham chiếu đến một biến toàn cục không phải trực tiếp bằng tên từ vựng của nó, mà thay vào đó là gián tiếp thông qua một tham chiếu thuộc tính của đối tượng toàn cục.

```js
window.a
```

Kỹ thuật này cho phép truy cập vào một biến toàn cục mà nếu không có sẽ không thể truy cập được do bị che khuất. Tuy nhiên, các biến bị che khuất không phải toàn cục thì không thể truy cập được.

Bất kể một hàm được gọi từ *đâu*, hoặc thậm chí được gọi *như thế nào*, phạm vi từ vựng của nó **chỉ** được xác định bởi nơi hàm đó được khai báo.

Quá trình tra cứu phạm vi từ vựng *chỉ* áp dụng cho các định danh hàng đầu, chẳng hạn như `a`, `b`, và `c`. Nếu bạn có một tham chiếu đến `foo.bar.baz` trong một đoạn mã, việc tra cứu phạm vi từ vựng sẽ áp dụng để tìm định danh `foo`, nhưng một khi nó đã xác định được biến đó, các quy tắc truy cập thuộc tính đối tượng sẽ tiếp quản để phân giải các thuộc tính `bar` và `baz`.

## Lách luật Phạm vi Từ vựng

Nếu phạm vi từ vựng chỉ được xác định bởi nơi một hàm được khai báo, một quyết định hoàn toàn thuộc về thời điểm viết mã, thì làm thế nào có thể có cách để "sửa đổi" (hay, lách luật) phạm vi từ vựng tại thời điểm chạy?

JavaScript có hai cơ chế như vậy. Cả hai đều bị cộng đồng rộng lớn xem là những thực tiễn tồi tệ để sử dụng trong mã của bạn. Nhưng những lập luận điển hình chống lại chúng thường bỏ lỡ điểm quan trọng nhất: **lách luật phạm vi từ vựng dẫn đến hiệu năng kém hơn.**

Tuy nhiên, trước khi tôi giải thích vấn đề hiệu năng, chúng ta hãy xem hai cơ chế này hoạt động như thế nào.

### `eval`

Hàm `eval(..)` trong JavaScript nhận một chuỗi làm đối số, và coi nội dung của chuỗi đó như thể nó thực sự là mã đã được viết tại điểm đó trong chương trình. Nói cách khác, bạn có thể tạo mã một cách có lập trình bên trong mã bạn đã viết, và chạy đoạn mã được tạo ra như thể nó đã ở đó từ lúc viết mã.

Nhìn nhận `eval(..)` (một cách chơi chữ) dưới lăng kính đó, hẳn sẽ rõ ràng cách `eval(..)` cho phép bạn sửa đổi môi trường phạm vi từ vựng bằng cách lách luật và giả vờ rằng mã tại thời điểm viết (tức là, từ vựng) đã luôn ở đó.

Trên các dòng mã tiếp theo sau khi một `eval(..)` đã thực thi, *Bộ máy* sẽ không "biết" hoặc "quan tâm" rằng đoạn mã trước đó đã được thông dịch động và do đó đã sửa đổi môi trường phạm vi từ vựng. *Bộ máy* sẽ chỉ đơn giản thực hiện các tra cứu phạm vi từ vựng của mình như mọi khi.

Hãy xem xét đoạn mã sau:

```js
function foo(str, a) {
	eval( str ); // lách luật!
	console.log( a, b );
}

var b = 2;

foo( "var b = 3;", 1 ); // 1 3
```

Chuỗi `"var b = 3;"` được coi, tại thời điểm gọi `eval(..)`, như là mã đã tồn tại ở đó. Bởi vì đoạn mã đó tình cờ khai báo một biến mới `b`, nó sửa đổi phạm vi từ vựng hiện có của `foo(..)`. Thực tế, như đã đề cập ở trên, đoạn mã này thực sự tạo ra biến `b` bên trong `foo(..)` mà che khuất biến `b` đã được khai báo ở phạm vi bên ngoài (toàn cục).

Khi lệnh gọi `console.log(..)` xảy ra, nó tìm thấy cả `a` và `b` trong phạm vi của `foo(..)`, và không bao giờ tìm thấy `b` bên ngoài. Do đó, chúng ta in ra "1 3" thay vì "1 2" như trường hợp thông thường.

**Lưu ý:** Trong ví dụ này, để cho đơn giản, chuỗi "mã" chúng ta truyền vào là một chuỗi ký tự cố định. Nhưng nó có thể dễ dàng được tạo ra một cách có lập trình bằng cách nối các ký tự lại với nhau dựa trên logic của chương trình. `eval(..)` thường được sử dụng để thực thi mã được tạo động, vì việc đánh giá động một đoạn mã gần như tĩnh từ một chuỗi ký tự sẽ không mang lại lợi ích thực sự nào so với việc viết mã trực tiếp.

Theo mặc định, nếu một chuỗi mã mà `eval(..)` thực thi chứa một hoặc nhiều khai báo (biến hoặc hàm), hành động này sẽ sửa đổi phạm vi từ vựng hiện có nơi `eval(..)` cư trú. Về mặt kỹ thuật, `eval(..)` có thể được gọi "gián tiếp", thông qua các thủ thuật khác nhau (nằm ngoài phạm vi thảo luận của chúng ta ở đây), khiến nó thay vào đó thực thi trong bối cảnh của phạm vi toàn cục, do đó sửa đổi nó. Nhưng trong cả hai trường hợp, `eval(..)` có thể sửa đổi một phạm vi từ vựng vốn được xác định tại thời điểm viết mã.

**Lưu ý:** `eval(..)` khi được sử dụng trong một chương trình ở chế độ nghiêm ngặt (strict mode) sẽ hoạt động trong phạm vi từ vựng riêng của nó, có nghĩa là các khai báo được thực hiện bên trong `eval()` không thực sự sửa đổi phạm vi bao quanh.

```js
function foo(str) {
   "use strict";
   eval( str );
   console.log( a ); // ReferenceError: a is not defined
}

foo( "var a = 2" );
```

Có những cơ sở khác trong JavaScript tạo ra hiệu ứng rất giống với `eval(..)`. `setTimeout(..)` và `setInterval(..)` *có thể* nhận một chuỗi cho đối số đầu tiên của chúng, nội dung của chuỗi đó được `eval` như là mã của một hàm được tạo động. Đây là hành vi cũ, kế thừa và từ lâu đã không còn được dùng nữa. Đừng làm vậy!

Hàm khởi tạo `new Function(..)` tương tự cũng nhận một chuỗi mã trong đối số **cuối cùng** của nó để biến thành một hàm được tạo động (các đối số đầu tiên, nếu có, là các tham số được đặt tên cho hàm mới). Cú pháp hàm khởi tạo này an toàn hơn một chút so với `eval(..)`, nhưng bạn vẫn nên tránh nó trong mã của mình.

Các trường hợp sử dụng để tạo mã động bên trong chương trình của bạn là cực kỳ hiếm, vì sự suy giảm hiệu năng gần như không bao giờ đáng để đánh đổi lấy khả năng đó.

### `with`

Tính năng khác bị xem là bất hảo (và bây giờ đã bị loại bỏ!) trong JavaScript mà lách luật phạm vi từ vựng là từ khóa `with`. Có nhiều cách hợp lệ để giải thích `with`, nhưng ở đây tôi sẽ chọn giải thích nó từ góc độ cách nó tương tác và ảnh hưởng đến phạm vi từ vựng.

`with` thường được giải thích như một cách viết tắt để thực hiện nhiều tham chiếu thuộc tính đối với một đối tượng *mà không* lặp lại tham chiếu đối tượng mỗi lần.

Ví dụ:

```js
var obj = {
	a: 1,
	b: 2,
	c: 3
};

// "tẻ nhạt" hơn khi phải lặp lại "obj"
obj.a = 2;
obj.b = 3;
obj.c = 4;

// cách viết tắt "dễ dàng" hơn
with (obj) {
	a = 3;
	b = 4;
	c = 5;
}
```

Tuy nhiên, có nhiều điều đang diễn ra ở đây hơn là chỉ một cách viết tắt tiện lợi để truy cập thuộc tính đối tượng. Hãy xem xét:

```js
function foo(obj) {
	with (obj) {
		a = 2;
	}
}

var o1 = {
	a: 3
};

var o2 = {
	b: 3
};

foo( o1 );
console.log( o1.a ); // 2

foo( o2 );
console.log( o2.a ); // undefined
console.log( a ); // 2 -- Chà, làm rò rỉ biến toàn cục!
```

Trong ví dụ mã này, hai đối tượng `o1` và `o2` được tạo ra. Một cái có thuộc tính `a`, và cái kia thì không. Hàm `foo(..)` nhận một tham chiếu đối tượng `obj` làm đối số, và gọi `with (obj) { .. }` trên tham chiếu đó. Bên trong khối `with`, chúng ta thực hiện một tham chiếu có vẻ như là một tham chiếu từ vựng thông thường đến một biến `a`, thực chất là một tham chiếu LHS (xem Chương 1), để gán cho nó giá trị là `2`.

Khi chúng ta truyền vào `o1`, phép gán `a = 2` tìm thấy thuộc tính `o1.a` và gán cho nó giá trị `2`, như được phản ánh trong câu lệnh `console.log(o1.a)` sau đó. Tuy nhiên, khi chúng ta truyền vào `o2`, vì nó không có thuộc tính `a`, không có thuộc tính nào như vậy được tạo ra, và `o2.a` vẫn là `undefined`.

Nhưng sau đó chúng ta lưu ý một tác dụng phụ kỳ lạ, đó là một biến toàn cục `a` đã được tạo ra bởi phép gán `a = 2`. Làm thế nào điều này có thể xảy ra?

Câu lệnh `with` nhận một đối tượng, một đối tượng có không hoặc nhiều thuộc tính, và **coi đối tượng đó như thể *nó* là một phạm vi từ vựng hoàn toàn riêng biệt**, và do đó các thuộc tính của đối tượng được coi như là các định danh được xác định theo quy tắc từ vựng trong "phạm vi" đó.

**Lưu ý:** Mặc dù một khối `with` coi một đối tượng như một phạm vi từ vựng, một khai báo `var` thông thường bên trong khối `with` đó sẽ không thuộc phạm vi của khối `with`, mà thay vào đó thuộc phạm vi của hàm chứa nó.

Trong khi hàm `eval(..)` có thể sửa đổi phạm vi từ vựng hiện có nếu nó nhận một chuỗi mã với một hoặc nhiều khai báo trong đó, câu lệnh `with` thực sự tạo ra một **phạm vi từ vựng hoàn toàn mới** từ hư không, từ đối tượng mà bạn truyền cho nó.

Hiểu theo cách này, "phạm vi" được khai báo bởi câu lệnh `with` khi chúng ta truyền `o1` chính là `o1`, và "phạm vi" đó có một "định danh" tương ứng với thuộc tính `o1.a`. Nhưng khi chúng ta sử dụng `o2` làm "phạm vi", nó không có "định danh" `a` nào như vậy, và vì vậy các quy tắc thông thường của việc tra cứu định danh LHS (xem Chương 1) đã diễn ra.

Cả "phạm vi" của `o2`, lẫn phạm vi của `foo(..)`, và ngay cả phạm vi toàn cục, đều không có định danh `a` nào để tìm thấy, vì vậy khi `a = 2` được thực thi, nó dẫn đến việc tạo ra biến toàn cục tự động (vì chúng ta đang ở chế độ không nghiêm ngặt).

Thật là một ý tưởng kỳ lạ và khó hình dung khi thấy `with` biến, tại thời điểm chạy, một đối tượng và các thuộc tính của nó thành một "phạm vi" *với* các "định danh". Nhưng đó là lời giải thích rõ ràng nhất mà tôi có thể đưa ra cho kết quả chúng ta thấy.

**Lưu ý:** Ngoài việc là một ý tưởng tồi để sử dụng, cả `eval(..)` và `with` đều bị ảnh hưởng (hạn chế) bởi Chế độ Nghiêm ngặt. `with` bị cấm hoàn toàn, trong khi các hình thức khác nhau của `eval(..)` gián tiếp hoặc không an toàn bị cấm trong khi vẫn giữ lại chức năng cốt lõi.

### Hiệu năng

Cả `eval(..)` và `with` đều lách luật phạm vi từ vựng vốn được xác định tại thời điểm viết mã bằng cách sửa đổi hoặc tạo ra phạm vi từ vựng mới tại thời điểm chạy.

Vậy, có gì to tát đâu, bạn hỏi? Nếu chúng cung cấp chức năng phức tạp hơn và sự linh hoạt trong mã hóa, chẳng phải chúng là những tính năng *tốt* sao? **Không.**

*Bộ máy* JavaScript thực hiện hàng loạt tối ưu hóa hiệu năng trong giai đoạn biên dịch. Mấu chốt của một vài tối ưu hóa này nằm ở khả năng phân tích tĩnh mã nguồn ngay khi nó phân tích từ vựng, và xác định trước vị trí của tất cả các khai báo biến và hàm, để tốn ít nỗ lực hơn trong việc phân giải các định danh trong quá trình thực thi.

Nhưng nếu *Bộ máy* tìm thấy `eval(..)` hay `with` trong mã, về cơ bản nó phải *giả định* rằng mọi nhận định của nó về vị trí của các định danh đều có thể không còn hợp lệ nữa, bởi vì nó không thể biết tại thời điểm phân tích từ vựng chính xác đoạn mã bạn có thể truyền cho `eval(..)` để sửa đổi phạm vi từ vựng, hoặc nội dung của đối tượng bạn có thể truyền cho `with` để tạo ra một phạm vi từ vựng mới để tham khảo.

Nói cách khác, theo hướng bi quan nhất, hầu hết các tối ưu hóa mà nó *lẽ ra sẽ* thực hiện đều trở nên vô nghĩa nếu có sự hiện diện của `eval(..)` hoặc `with`, vì vậy nó đơn giản là không thực hiện các tối ưu hóa đó *chút nào*.

Mã của bạn gần như chắc chắn sẽ có xu hướng chạy chậm hơn chỉ vì bạn đưa `eval(..)` hoặc `with` vào bất kỳ đâu trong mã. Bất kể *Bộ máy* có thể thông minh đến đâu trong việc cố gắng hạn chế các tác dụng phụ của những giả định bi quan này, **không thể phủ nhận một thực tế rằng nếu không có các tối ưu hóa, mã sẽ chạy chậm hơn.**

## Ôn lại (Tóm tắt)

Phạm vi từ vựng có nghĩa là phạm vi được xác định bởi các quyết định tại thời điểm viết mã về nơi các hàm được khai báo. Giai đoạn phân tích từ vựng của quá trình biên dịch về cơ bản có thể biết tất cả các định danh được khai báo ở đâu và như thế nào, và do đó dự đoán cách chúng sẽ được tra cứu trong quá trình thực thi.

Hai cơ chế trong JavaScript có thể "lách luật" phạm vi từ vựng: `eval(..)` và `with`. Cơ chế đầu tiên có thể sửa đổi phạm vi từ vựng hiện có (tại thời điểm chạy) bằng cách đánh giá một chuỗi "mã" có một hoặc nhiều khai báo trong đó. Cơ chế thứ hai về cơ bản tạo ra một phạm vi từ vựng hoàn toàn mới (cũng tại thời điểm chạy) bằng cách coi một tham chiếu đối tượng *như* một "phạm vi" và các thuộc tính của đối tượng đó như là các định danh thuộc phạm vi.

Nhược điểm của các cơ chế này là nó làm vô hiệu hóa khả năng của *Bộ máy* trong việc thực hiện các tối ưu hóa tại thời điểm biên dịch liên quan đến việc tra cứu phạm vi, bởi vì *Bộ máy* phải giả định một cách bi quan rằng các tối ưu hóa như vậy sẽ không hợp lệ. Mã *sẽ* chạy chậm hơn do sử dụng một trong hai tính năng này. **Đừng dùng chúng.**
