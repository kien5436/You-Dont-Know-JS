# Bạn không hiểu JS: Phạm vi & Hàm khép kín
# Chương 3: Phạm vi Hàm và Phạm vi Khối

Như chúng ta đã khám phá ở chương 2, phạm vi bao gồm một chuỗi các "bong bóng" đóng vai trò như một vật chứa, nơi các định danh (biến, hàm) được khai báo. Các bong bóng này lồng vào nhau một cách gọn gàng, và sự lồng ghép này được xác định tại thời điểm viết mã.

Nhưng chính xác thì điều gì tạo ra một bong bóng mới? Chỉ có hàm thôi sao? Liệu các cấu trúc khác trong JavaScript có thể tạo ra bong bóng phạm vi không?

## Phạm vi từ Hàm

Câu trả lời phổ biến nhất cho những câu hỏi đó là JavaScript có phạm vi dựa trên hàm. Tức là mỗi hàm bạn khai báo sẽ tạo ra một bong bóng cho chính nó nhưng không có cấu trúc nào khác tạo ra bong bóng phạm vi riêng. Như chúng ta sẽ thấy trong ít phút nữa, điều này không hoàn toàn đúng.

Nhưng trước tiên, hãy cùng khám phá phạm vi hàm và những hệ quả của nó.

Xét đoạn mã sau:

```js
function foo(a) {
	var b = 2;

	// một vài dòng mã

	function bar() {
		// ...
	}

	// nhiều mã nữa

	var c = 3;
}
```

Trong đoạn mã này, bong bóng phạm vi của `foo(..)` bao gồm các định danh `a`, `b`, `c` và `bar`. **Bất kể** một khai báo xuất hiện ở *đâu* trong phạm vi, biến hoặc hàm đó đều thuộc về bong bóng phạm vi chứa nó. Chúng ta sẽ khám phá chính xác *điều đó* hoạt động như thế nào trong chương tiếp theo.

`bar(..)` có bong bóng phạm vi riêng của nó. Phạm vi toàn cục cũng vậy, nó chỉ có một định danh duy nhất được gắn vào: `foo`.

Bởi vì `a`, `b`, `c`, và `bar` đều thuộc về bong bóng phạm vi của `foo(..)`, chúng không thể truy cập được từ bên ngoài `foo(..)`. Tức là đoạn mã sau đây đều sẽ dẫn đến lỗi `ReferenceError`, vì các định danh không tồn tại trong phạm vi toàn cục:

```js
bar(); // lỗi

console.log( a, b, c ); // cả 3 đều lỗi
```

Tuy nhiên, tất cả các định danh này (`a`, `b`, `c`, `foo`, và `bar`) đều có thể truy cập được *bên trong* `foo(..)`, và thực sự cũng có thể xuất hiện bên trong `bar(..)` (giả sử không có khai báo định danh che khuất nào bên trong `bar(..)`).

Phạm vi hàm khuyến khích ý tưởng rằng tất cả các biến đều thuộc về hàm, và có thể được sử dụng và tái sử dụng trong toàn bộ hàm (và thực ra có thể truy cập được với các phạm vi lồng nhau). Phương pháp thiết kế này có thể khá hữu ích, và chắc chắn có thể tận dụng tối đa bản chất "động" của các biến JavaScript để nhận các giá trị thuộc các kiểu khác nhau khi cần.

Mặt khác, nếu bạn không có những biện pháp phòng ngừa cẩn thận, các biến tồn tại trong toàn bộ phạm vi có thể dẫn đến một số cạm bẫy không mong muốn.

## Ẩn mình trong Phạm vi

Cách nghĩ truyền thống về hàm là bạn khai báo một hàm rồi thêm mã vào bên trong. Nhưng tư duy ngược lại cũng mạnh mẽ và hữu ích không kém: lấy bất kỳ một đoạn mã nào bạn đã viết và bao bọc nó bằng một khai báo hàm giúp "ẩn" đoạn mã đó đi.

Kết quả thực tế là tạo ra một bong bóng phạm vi xung quanh đoạn mã đang xét, có nghĩa là bất kỳ khai báo nào (biến hoặc hàm) trong đoạn mã đó giờ đây sẽ bị ràng buộc với phạm vi của hàm bao bọc mới thay vì phạm vi chứa nó trước đây. Nói cách khác, bạn có thể "ẩn" các biến và hàm bằng cách bao bọc chúng trong phạm vi của một hàm.

Tại sao việc "ẩn" các biến và hàm lại là một kỹ thuật hữu ích?

Có nhiều lý do thúc đẩy việc ẩn giấu dựa trên phạm vi này. Chúng thường nảy sinh từ nguyên tắc thiết kế phần mềm "Nguyên tắc Đặc quyền Tối thiểu" [^note-leastprivilege], đôi khi còn được gọi là "Thẩm quyền Tối thiểu" hoặc "Tiết lộ Tối thiểu". Nguyên tắc này nêu rằng: trong thiết kế phần mềm, chẳng hạn như API cho một khối chức năng/đối tượng, bạn chỉ nên để lộ tối thiểu những gì cần thiết và "ẩn" đi mọi thứ khác.

Nguyên tắc này mở rộng đến việc lựa chọn phạm vi nào để chứa các biến và hàm. Nếu tất cả các biến và hàm đều nằm trong phạm vi toàn cục, tất nhiên chúng sẽ có thể truy cập được bởi bất kỳ phạm vi lồng nhau nào. Nhưng điều này sẽ vi phạm nguyên tắc "Tối thiểu..." ở chỗ bạn (có khả năng) đang phơi bày nhiều biến hoặc hàm mà lẽ ra bạn nên giữ riêng tư, vì việc sử dụng mã đúng cách sẽ không khuyến khích truy cập vào những biến/hàm đó.

Ví dụ:

```js
function doSomething(a) {
	b = a + doSomethingElse( a * 2 );

	console.log( b * 3 );
}

function doSomethingElse(a) {
	return a - 1;
}

var b;

doSomething( 2 ); // 15
```

Trong đoạn mã này, biến `b` và hàm `doSomethingElse(..)` có khả năng là các chi tiết "riêng tư" về cách `doSomething(..)` thực hiện công việc của nó. Việc cho phạm vi bao quanh "truy cập" vào `b` và `doSomethingElse(..)` không chỉ không cần thiết mà còn có thể "nguy hiểm", ở chỗ chúng có thể được sử dụng theo những cách không mong muốn, dù cố ý hay không, và điều này có thể vi phạm các giả định về điều kiện tiên quyết của `doSomething(..)`.

Một thiết kế "đúng đắn" hơn sẽ ẩn những chi tiết riêng tư này bên trong phạm vi của `doSomething(..)`, chẳng hạn như:

```js
function doSomething(a) {
	function doSomethingElse(a) {
		return a - 1;
	}

	var b;

	b = a + doSomethingElse( a * 2 );

	console.log( b * 3 );
}

doSomething( 2 ); // 15
```

Bây giờ, `b` và `doSomethingElse(..)` không thể bị ảnh hưởng từ bên ngoài, thay vào đó chỉ được kiểm soát bởi `doSomething(..)`. Chức năng và kết quả cuối cùng không bị ảnh hưởng nhưng thiết kế này giữ cho các chi tiết nội bộ được riêng tư, là dấu hiệu thường thấy của một phần mềm tốt.

### Tránh Xung đột

Một lợi ích khác của việc "ẩn" các biến và hàm bên trong một phạm vi là để tránh xung đột không chủ ý giữa hai định danh khác nhau có cùng tên nhưng mục đích sử dụng khác nhau. Xung đột thường dẫn đến việc ghi đè các giá trị một cách bất ngờ.

Ví dụ:

```js
function foo() {
	function bar(a) {
		i = 3; // thay đổi `i` trong vòng lặp for của phạm vi bao quanh
		console.log( a + i );
	}

	for (var i=0; i<10; i++) {
		bar( i * 2 ); // chà, vòng lặp vô hạn ở phía trước!
	}
}

foo();
```

Phép gán `i = 3` bên trong `bar(..)` ghi đè một cách không mong muốn lên biến `i` đã được khai báo trong `foo(..)` tại vòng lặp for. Trong trường hợp này, nó sẽ dẫn đến một vòng lặp vô hạn, bởi vì `i` được đặt thành một giá trị cố định là `3` và giá trị đó sẽ mãi mãi `< 10`.

Phép gán bên trong `bar(..)` cần khai báo một biến cục bộ để sử dụng, bất kể tên định danh được chọn là gì. `var i = 3;` sẽ khắc phục vấn đề (và sẽ tạo ra khai báo "biến bị che khuất" đã đề cập trước đó cho `i`). Một lựa chọn *bổ sung*, không phải thay thế, là chọn một tên định danh hoàn toàn khác, chẳng hạn như `var j = 3;`. Nhưng thiết kế phần mềm của bạn có thể tự nhiên đòi hỏi cùng một tên định danh, vì vậy việc sử dụng phạm vi để "ẩn" khai báo bên trong của bạn là lựa chọn tốt nhất/duy nhất trong trường hợp đó.

#### "Không gian tên" Toàn cục

Một ví dụ đặc biệt mạnh mẽ về xung đột biến (có khả năng xảy ra) xảy ra trong phạm vi toàn cục. Nhiều thư viện được tải vào chương trình của bạn có thể khá dễ dàng xung đột với nhau nếu chúng không ẩn các hàm và biến nội bộ/riêng tư của mình một cách hợp lệ.

Các thư viện như vậy thường sẽ tạo một khai báo biến duy nhất, thường là một đối tượng, với một tên đủ duy nhất, trong phạm vi toàn cục. Đối tượng này sau đó được sử dụng như một "không gian tên" cho thư viện đó, nơi tất cả các chức năng được phơi bày cụ thể đều được tạo ra như các thuộc tính của đối tượng đó (không gian tên), thay vì là các định danh được định phạm vi từ vựng ở cấp cao nhất.

Ví dụ:

```js
var MyReallyCoolLibrary = {
	awesome: "stuff",
	doSomething: function() {
		// ...
	},
	doAnotherThing: function() {
		// ...
	}
};
```

#### Quản lý Mô-đun

Một lựa chọn khác để tránh xung đột là phương pháp "mô-đun" hiện đại hơn, sử dụng bất kỳ trình quản lý phụ thuộc nào. Khi sử dụng các công cụ này, không có thư viện nào từng thêm bất kỳ định danh nào vào phạm vi toàn cục, mà thay vào đó yêu cầu các định danh của chúng phải được nhập tường minh vào một phạm vi cụ thể khác thông qua việc sử dụng các cơ chế khác nhau của trình quản lý phụ thuộc.

Cần lưu ý rằng các công cụ này không sở hữu chức năng "ma thuật" nào được miễn trừ khỏi các quy tắc phạm vi từ vựng. Chúng chỉ đơn giản là sử dụng các quy tắc về phạm vi như đã giải thích ở đây để đảm bảo rằng không có định danh nào được đưa vào bất kỳ phạm vi chia sẻ nào, và thay vào đó được giữ trong các phạm vi riêng tư, không dễ bị xung đột, điều này ngăn chặn bất kỳ sự xung đột phạm vi tình cờ nào.

Như vậy, bạn có thể lập trình một cách phòng thủ và đạt được kết quả tương tự như các trình quản lý phụ thuộc mà không thực sự cần sử dụng chúng, nếu bạn chọn vậy. Xem Chương 5 để biết thêm thông tin về mẫu hình mô-đun.

## Hàm như là Phạm vi

Chúng ta đã thấy rằng có thể lấy bất kỳ đoạn mã nào và bao bọc nó bằng một hàm, và điều đó thực sự "ẩn" bất kỳ khai báo biến hoặc hàm nào bên trong khỏi phạm vi bên ngoài vào trong phạm vi nội bộ của hàm đó.

Ví dụ:

```js
var a = 2;

function foo() { // <-- chèn vào đây

	var a = 3;
	console.log( a ); // 3

} // <-- và đây
foo(); // <-- và đây

console.log( a ); // 2
```

Mặc dù kỹ thuật này "hoạt động", nó không nhất thiết phải là lý tưởng. Nó gây ra một vài vấn đề. Thứ nhất là chúng ta phải khai báo một hàm có tên `foo()`, có nghĩa là chính tên định danh `foo` "làm ô nhiễm" phạm vi bao quanh (toàn cục, trong trường hợp này). Chúng ta cũng phải gọi hàm một cách tường minh bằng tên (`foo()`) để đoạn mã được bao bọc thực sự được thực thi.

Sẽ lý tưởng hơn nếu hàm không cần tên (hoặc, đúng hơn là, tên không làm ô nhiễm phạm vi bao quanh), và nếu hàm có thể được thực thi một cách tự động.

May mắn thay, JavaScript cung cấp một giải pháp cho cả hai vấn đề.

```js
var a = 2;

(function foo(){ // <-- chèn vào đây

	var a = 3;
	console.log( a ); // 3

})(); // <-- và đây

console.log( a ); // 2
```

Hãy cùng phân tích những gì đang diễn ra ở đây.

Đầu tiên, hãy chú ý rằng câu lệnh hàm bao bọc bắt đầu bằng `(function...` thay vì chỉ `function...`. Mặc dù điều này có vẻ như một chi tiết nhỏ, nhưng thực ra nó là một thay đổi lớn. Thay vì coi hàm như một khai báo tiêu chuẩn, hàm được coi như một biểu thức hàm.

**Lưu ý:** Cách dễ nhất để phân biệt khai báo và biểu thức là vị trí của từ "function" trong câu lệnh (không chỉ là một dòng, mà là một câu lệnh riêng biệt). Nếu "function" là thứ đầu tiên trong câu lệnh, thì đó là một khai báo hàm. Ngược lại, đó là một biểu thức hàm.

Sự khác biệt chính chúng ta có thể quan sát ở đây giữa một khai báo hàm và một biểu thức hàm liên quan đến nơi tên của nó được ràng buộc như một định danh.

Hãy so sánh hai đoạn mã trước đó. Trong đoạn mã đầu tiên, tên `foo` được ràng buộc trong phạm vi bao quanh, và chúng ta gọi nó trực tiếp bằng `foo()`. Trong đoạn mã thứ hai, tên `foo` không được ràng buộc trong phạm vi bao quanh, mà thay vào đó chỉ được ràng buộc bên trong chính hàm của nó.

Nói cách khác, `(function foo(){ .. })` như một biểu thức có nghĩa là định danh `foo` chỉ được tìm thấy *duy nhất* trong phạm vi nơi dấu `..` chỉ định, không phải trong phạm vi bên ngoài. Việc ẩn tên `foo` vào bên trong chính nó có nghĩa là nó không làm ô nhiễm phạm vi bao quanh một cách không cần thiết.

### Vô danh và Có tên

Bạn có lẽ quen thuộc nhất với các biểu thức hàm dưới dạng tham số callback, chẳng hạn như:

```js
setTimeout( function(){
	console.log("Tôi đã đợi 1 giây!");
}, 1000 );
```

Đây được gọi là một "biểu thức hàm vô danh", bởi vì `function()...` không có định danh tên trên nó. Biểu thức hàm có thể vô danh, nhưng khai báo hàm không thể bỏ qua tên -- đó sẽ là ngữ pháp JavaScript không hợp lệ.

Biểu thức hàm vô danh nhanh và dễ gõ, và nhiều thư viện và công cụ có xu hướng khuyến khích phong cách mã hóa này. Tuy nhiên, chúng có một số nhược điểm cần xem xét:

1. Các hàm vô danh không có tên hữu ích để hiển thị trong dấu vết ngăn xếp (stack traces), điều này có thể làm cho việc gỡ lỗi khó khăn hơn.

2. Không có tên, nếu hàm cần tự tham chiếu đến chính nó, cho đệ quy, v.v., thì tham chiếu `arguments.callee` **đã lỗi thời** không may lại là bắt buộc. Một ví dụ khác về việc cần tự tham chiếu là khi một hàm xử lý sự kiện muốn tự hủy liên kết sau khi nó được kích hoạt.

3. Các hàm vô danh bỏ qua một cái tên thường hữu ích trong việc cung cấp mã dễ đọc/dễ hiểu hơn. Một cái tên mô tả giúp tự ghi lại tài liệu cho đoạn mã đang xét.

**Các biểu thức hàm nội tuyến** rất mạnh mẽ và hữu ích -- câu hỏi về vô danh và có tên không làm giảm đi điều đó. Việc cung cấp một cái tên cho biểu thức hàm của bạn giải quyết khá hiệu quả tất cả những nhược điểm này, nhưng không có nhược điểm hữu hình nào. Thực tiễn tốt nhất là luôn đặt tên cho các biểu thức hàm của bạn:

```js
setTimeout( function timeoutHandler(){ // <-- Nhìn này, tôi có tên!
	console.log( "Tôi đã đợi 1 giây!" );
}, 1000 );
```

### Gọi Biểu thức Hàm Ngay lập tức

```js
var a = 2;

(function foo(){

	var a = 3;
	console.log( a ); // 3

})();

console.log( a ); // 2
```

Bây giờ chúng ta đã có một hàm dưới dạng biểu thức nhờ vào việc bao bọc nó trong một cặp `( )`, chúng ta có thể thực thi hàm đó bằng cách thêm một cặp `()` khác ở cuối, như `(function foo(){ .. })()`. Cặp `( )` bao bọc đầu tiên biến hàm thành một biểu thức, và cặp `()` thứ hai thực thi hàm.

Mẫu hình này phổ biến đến mức, vài năm trước cộng đồng đã đồng ý một thuật ngữ cho nó: **IIFE**, viết tắt của **I**mmediately **I**nvoked **F**unction **E**xpression (Biểu thức Hàm được Gọi Ngay lập tức).

Tất nhiên, IIFE không nhất thiết cần tên -- dạng phổ biến nhất của IIFE là sử dụng một biểu thức hàm vô danh. Mặc dù chắc chắn ít phổ biến hơn, việc đặt tên cho một IIFE có tất cả các lợi ích đã đề cập ở trên so với các biểu thức hàm vô danh, vì vậy đó là một thực tiễn tốt để áp dụng.

```js
var a = 2;

(function IIFE(){

	var a = 3;
	console.log( a ); // 3

})();

console.log( a ); // 2
```

Có một biến thể nhỏ của dạng IIFE truyền thống, mà một số người ưa thích: `(function(){ .. }())`. Hãy nhìn kỹ để thấy sự khác biệt. Trong dạng đầu tiên, biểu thức hàm được bao bọc trong `( )`, và sau đó cặp `()` gọi hàm ở ngay bên ngoài sau nó. Trong dạng thứ hai, cặp `()` gọi hàm được di chuyển vào bên trong cặp `( )` bao bọc bên ngoài.

Hai dạng này giống hệt nhau về chức năng. **Việc bạn ưa thích dạng nào hoàn toàn là một lựa chọn về phong cách.**

Một biến thể khác của IIFE khá phổ biến là tận dụng thực tế rằng chúng thực sự chỉ là các lệnh gọi hàm, và truyền vào các đối số.

Ví dụ:

```js
var a = 2;

(function IIFE( global ){

	var a = 3;
	console.log( a ); // 3
	console.log( global.a ); // 2

})( window );

console.log( a ); // 2
```

Chúng ta truyền vào tham chiếu đối tượng `window`, nhưng chúng ta đặt tên tham số là `global`, để có một sự phân định rõ ràng về phong cách cho các tham chiếu toàn cục và không toàn cục. Tất nhiên, bạn có thể truyền vào bất cứ thứ gì từ một phạm vi bao quanh mà bạn muốn, và bạn có thể đặt tên cho các tham số bất cứ điều gì phù hợp với bạn. Điều này chủ yếu chỉ là lựa chọn về phong cách.

Một ứng dụng khác của mẫu hình này giải quyết mối lo ngại (nhỏ và đặc thù) rằng giá trị của định danh `undefined` mặc định có thể bị ghi đè không chính xác, gây ra kết quả không mong muốn. Bằng cách đặt tên một tham số là `undefined`, nhưng không truyền giá trị nào cho đối số đó, chúng ta có thể đảm bảo rằng định danh `undefined` thực sự là giá trị undefined trong một khối mã:

```js
undefined = true; // đặt một quả mìn cho mã khác! hãy tránh!

(function IIFE( undefined ){

	var a;
	if (a === undefined) {
		console.log( "Undefined ở đây an toàn!" );
	}

})();
```

Vẫn còn một biến thể khác của IIFE đảo ngược thứ tự mọi thứ, trong đó hàm cần thực thi được đưa ra thứ hai, *sau khi* gọi và các tham số để truyền cho nó. Mẫu hình này được sử dụng trong dự án UMD (Universal Module Definition). Một số người thấy nó dễ hiểu hơn một chút, mặc dù nó hơi dài dòng hơn.

```js
var a = 2;

(function IIFE( def ){
	def( window );
})(function def( global ){

	var a = 3;
	console.log( a ); // 3
	console.log( global.a ); // 2

});
```

Biểu thức hàm `def` được định nghĩa ở nửa sau của đoạn mã, và sau đó được truyền như một tham số (cũng được gọi là `def`) cho hàm `IIFE` được định nghĩa ở nửa đầu của đoạn mã. Cuối cùng, tham số `def` (hàm) được gọi, truyền `window` vào như là tham số `global`.

## Khối như là Phạm vi

Trong khi hàm là đơn vị phạm vi phổ biến nhất, và chắc chắn là cách tiếp cận thiết kế phổ biến nhất trong phần lớn JS đang lưu hành, các đơn vị phạm vi khác cũng có thể tồn tại, và việc sử dụng các đơn vị phạm vi khác này có thể dẫn đến mã tốt hơn, dễ bảo trì hơn.

Nhiều ngôn ngữ khác ngoài JavaScript hỗ trợ Phạm vi Khối, và do đó các nhà phát triển từ những ngôn ngữ đó đã quen với tư duy này, trong khi những người chủ yếu chỉ làm việc với JavaScript có thể thấy khái niệm này hơi xa lạ.

Nhưng ngay cả khi bạn chưa bao giờ viết một dòng mã nào theo kiểu phạm vi khối, bạn có lẽ vẫn quen thuộc với thành ngữ cực kỳ phổ biến này trong JavaScript:

```js
for (var i=0; i<10; i++) {
	console.log( i );
}
```

Chúng ta khai báo biến `i` trực tiếp bên trong phần đầu của vòng lặp for, rất có thể vì *ý định* của chúng ta là chỉ sử dụng `i` trong bối cảnh của vòng lặp for đó, và về cơ bản bỏ qua thực tế rằng biến này thực sự tự định phạm vi của nó cho phạm vi bao quanh (hàm hoặc toàn cục).

Đó là tất cả những gì về phạm vi khối. Khai báo các biến càng gần càng tốt, càng cục bộ càng tốt, với nơi chúng sẽ được sử dụng. Một ví dụ khác:

```js
var foo = true;

if (foo) {
	var bar = foo * 2;
	bar = something( bar );
	console.log( bar );
}
```

Chúng ta đang sử dụng một biến `bar` chỉ trong bối cảnh của câu lệnh if, vì vậy có vẻ hợp lý khi chúng ta khai báo nó bên trong khối if. Tuy nhiên, nơi chúng ta khai báo biến không liên quan khi sử dụng `var`, bởi vì chúng sẽ luôn thuộc về phạm vi bao quanh. Đoạn mã này về cơ bản là phạm vi khối "giả", vì lý do phong cách, và dựa vào việc tự thực thi để không vô tình sử dụng `bar` ở một nơi khác trong phạm vi đó.

Phạm vi khối là một công cụ để mở rộng "Nguyên tắc Phơi bày Tối thiểu" [^note-leastprivilege] trước đó từ việc ẩn thông tin trong các hàm sang việc ẩn thông tin trong các khối mã của chúng ta.

Hãy xem lại ví dụ về vòng lặp for:

```js
for (var i=0; i<10; i++) {
	console.log( i );
}
```

Tại sao phải làm ô nhiễm toàn bộ phạm vi của một hàm với biến `i` mà chỉ sẽ được (hoặc ít nhất *nên* được) sử dụng cho vòng lặp for?

Nhưng quan trọng hơn, các nhà phát triển có thể muốn *kiểm tra* bản thân để tránh vô tình (tái) sử dụng các biến ngoài mục đích dự định của chúng, chẳng hạn như được cấp một lỗi về một biến không xác định nếu bạn cố gắng sử dụng nó sai chỗ. Phạm vi khối (nếu có thể) cho biến `i` sẽ làm cho `i` chỉ có sẵn cho vòng lặp for, gây ra lỗi nếu `i` được truy cập ở nơi khác trong hàm. Điều này giúp đảm bảo các biến không được tái sử dụng theo những cách khó hiểu hoặc khó bảo trì.

Nhưng, thực tế đáng buồn là, bề ngoài, JavaScript không có cơ sở cho phạm vi khối.

Tức là, cho đến khi bạn đào sâu hơn một chút.

### `with`

Chúng ta đã học về `with` trong Chương 2. Mặc dù nó là một cấu trúc không được tán thành, nó *là* một ví dụ về (một dạng của) phạm vi khối, ở chỗ phạm vi được tạo ra từ đối tượng chỉ tồn tại trong vòng đời của câu lệnh `with` đó, và không tồn tại trong phạm vi bao quanh.

### `try/catch`

Một sự thật *rất* ít người biết là JavaScript trong ES3 đã chỉ định khai báo biến trong mệnh đề `catch` của một `try/catch` là có phạm vi khối đối với khối `catch`.

Ví dụ:

```js
try {
	undefined(); // hoạt động bất hợp pháp để ép một ngoại lệ!
}
catch (err) {
	console.log( err ); // hoạt động!
}

console.log( err ); // ReferenceError: `err` not found
```

Như bạn có thể thấy, `err` chỉ tồn tại trong mệnh đề `catch`, và ném ra một lỗi khi bạn cố gắng tham chiếu đến nó ở nơi khác.

**Lưu ý:** Mặc dù hành vi này đã được chỉ định và đúng với hầu hết tất cả các môi trường JS tiêu chuẩn (ngoại trừ có lẽ là IE cũ), nhiều công cụ kiểm tra mã (linter) dường như vẫn phàn nàn nếu bạn có hai hoặc nhiều mệnh đề `catch` trong cùng một phạm vi mà mỗi mệnh đề đều khai báo biến lỗi của mình với cùng một tên định danh. Đây thực sự không phải là một định nghĩa lại, vì các biến được định phạm vi khối một cách an toàn, nhưng các công cụ kiểm tra mã dường như vẫn, một cách khó chịu, phàn nàn về sự thật này.

Để tránh những cảnh báo không cần thiết này, một số nhà phát triển sẽ đặt tên cho các biến `catch` của họ là `err1`, `err2`, v.v. Các nhà phát triển khác sẽ chỉ đơn giản là tắt kiểm tra trùng lặp tên biến của công cụ kiểm tra mã.

Bản chất phạm vi khối của `catch` có thể có vẻ như một sự thật học thuật vô dụng, nhưng hãy xem Phụ lục B để biết thêm thông tin về mức độ hữu ích của nó.

### `let`

Cho đến nay, chúng ta đã thấy rằng JavaScript chỉ có một số hành vi đặc thù kỳ lạ phơi bày chức năng phạm vi khối. Nếu đó là tất cả những gì chúng ta có, và *đúng là như vậy* trong nhiều, nhiều năm, thì phạm vi khối sẽ không hữu ích lắm đối với nhà phát triển JavaScript.

May mắn thay, ES6 đã thay đổi điều đó, và giới thiệu một từ khóa mới `let` song hành cùng `var` như một cách khác để khai báo các biến.

Từ khóa `let` gắn khai báo biến vào phạm vi của bất kỳ khối nào (thường là một cặp `{ .. }`) chứa nó. Nói cách khác, `let` ngầm chiếm quyền kiểm soát phạm vi của bất kỳ khối nào cho khai báo biến của nó.

```js
var foo = true;

if (foo) {
	let bar = foo * 2;
	bar = something( bar );
	console.log( bar );
}

console.log( bar ); // ReferenceError
```

Sử dụng `let` để gắn một biến vào một khối hiện có có phần ngầm định. Nó có thể làm bạn bối rối nếu bạn không chú ý kỹ đến những khối nào có biến được định phạm vi cho chúng, và có thói quen di chuyển các khối xung quanh, bao bọc chúng trong các khối khác, v.v., khi bạn phát triển và tiến hóa mã.

Việc tạo ra các khối tường minh cho phạm vi khối có thể giải quyết một số mối lo ngại này, làm cho việc các biến được gắn vào đâu trở nên rõ ràng hơn. Thông thường, mã tường minh được ưa thích hơn mã ngầm định hoặc tinh vi. Phong cách phạm vi khối tường minh này dễ dàng đạt được, và phù hợp tự nhiên hơn với cách phạm vi khối hoạt động trong các ngôn ngữ khác:

```js
var foo = true;

if (foo) {
	{ // <-- khối tường minh
		let bar = foo * 2;
		bar = something( bar );
		console.log( bar );
	}
}

console.log( bar ); // ReferenceError
```

Chúng ta có thể tạo một khối tùy ý để `let` liên kết bằng cách chỉ cần bao gồm một cặp `{ .. }` ở bất kỳ đâu một câu lệnh là ngữ pháp hợp lệ. Trong trường hợp này, chúng ta đã tạo một khối tường minh *bên trong* câu lệnh if, điều này có thể dễ dàng hơn như một khối toàn bộ để di chuyển xung quanh sau này trong quá trình tái cấu trúc, mà không ảnh hưởng đến vị trí và ngữ nghĩa của câu lệnh if bao quanh.

**Lưu ý:** Để biết một cách khác để thể hiện phạm vi khối tường minh, hãy xem Phụ lục B.

Trong Chương 4, chúng ta sẽ đề cập đến hoisting, nói về việc các khai báo được coi là tồn tại trong toàn bộ phạm vi mà chúng xuất hiện.

Tuy nhiên, các khai báo được thực hiện với `let` sẽ *không* được hoist lên toàn bộ phạm vi của khối mà chúng xuất hiện. Các khai báo như vậy sẽ không "tồn tại" một cách có thể quan sát được trong khối cho đến câu lệnh khai báo.

```js
{
   console.log( bar ); // ReferenceError!
   let bar = 2;
}
```

#### Thu gom rác

Một lý do khác mà phạm vi khối hữu ích liên quan đến các hàm khép kín (closures) và việc thu gom rác để giải phóng bộ nhớ. Chúng ta sẽ minh họa ngắn gọn ở đây, nhưng cơ chế hàm khép kín được giải thích chi tiết trong Chương 5.

Xét:

```js
function process(data) {
	// làm điều gì đó thú vị
}

var someReallyBigData = { .. };

process( someReallyBigData );

var btn = document.getElementById( "my_button" );

btn.addEventListener( "click", function click(evt){
	console.log("nút đã được nhấn");
}, /*capturingPhase=*/false );
```

Hàm callback xử lý sự kiện `click` không *cần* biến `someReallyBigData` chút nào. Điều đó có nghĩa là, về mặt lý thuyết, sau khi `process(..)` chạy, cấu trúc dữ liệu nặng nề về bộ nhớ có thể được thu gom rác. Tuy nhiên, rất có khả năng (mặc dù phụ thuộc vào việc triển khai) rằng bộ máy JS vẫn sẽ phải giữ lại cấu trúc này, vì hàm `click` có một hàm khép kín trên toàn bộ phạm vi.

Phạm vi khối có thể giải quyết mối lo ngại này, làm cho nó rõ ràng hơn với bộ máy rằng nó không cần phải giữ lại `someReallyBigData`:

```js
function process(data) {
	// làm điều gì đó thú vị
}

// bất cứ thứ gì được khai báo bên trong khối này đều có thể biến mất sau đó!
{
	let someReallyBigData = { .. };

	process( someReallyBigData );
}

var btn = document.getElementById( "my_button" );

btn.addEventListener( "click", function click(evt){
	console.log("nút đã được nhấn");
}, /*capturingPhase=*/false );
```

Việc khai báo các khối tường minh để các biến liên kết cục bộ là một công cụ mạnh mẽ mà bạn có thể thêm vào hộp công cụ mã hóa của mình.

#### Vòng lặp `let`

Một trường hợp đặc biệt mà `let` tỏa sáng là trong trường hợp vòng lặp for như chúng ta đã thảo luận trước đó.

```js
for (let i=0; i<10; i++) {
	console.log( i );
}

console.log( i ); // ReferenceError
```

`let` trong phần đầu của vòng lặp for không chỉ ràng buộc `i` với thân vòng lặp for, mà thực tế, nó còn **tái ràng buộc nó** với mỗi *vòng lặp*, đảm bảo tái gán cho nó giá trị từ cuối vòng lặp trước đó.

Đây là một cách khác để minh họa hành vi ràng buộc theo từng vòng lặp xảy ra:

```js
{
	let j;
	for (j=0; j<10; j++) {
		let i = j; // được tái ràng buộc cho mỗi vòng lặp!
		console.log( i );
	}
}
```

Lý do tại sao việc ràng buộc theo từng vòng lặp này thú vị sẽ trở nên rõ ràng trong Chương 5 khi chúng ta thảo luận về các hàm khép kín.

Bởi vì các khai báo `let` gắn với các khối tùy ý thay vì phạm vi của hàm bao quanh (hoặc toàn cục), có thể có những cạm bẫy khi mã hiện có có sự phụ thuộc ngầm vào các khai báo `var` có phạm vi hàm, và việc thay thế `var` bằng `let` có thể đòi hỏi sự cẩn thận bổ sung khi tái cấu trúc mã.

Xét:

```js
var foo = true, baz = 10;

if (foo) {
	var bar = 3;

	if (baz > bar) {
		console.log( baz );
	}

	// ...
}
```

Mã này khá dễ dàng được tái cấu trúc thành:

```js
var foo = true, baz = 10;

if (foo) {
	var bar = 3;

	// ...
}

if (baz > bar) {
	console.log( baz );
}
```

Nhưng, hãy cẩn thận với những thay đổi như vậy khi sử dụng các biến có phạm vi khối:

```js
var foo = true, baz = 10;

if (foo) {
	let bar = 3;

	if (baz > bar) { // <-- đừng quên `bar` khi di chuyển!
		console.log( baz );
	}
}
```

Xem Phụ lục B để biết một phong cách phạm vi khối thay thế (tường minh hơn) có thể cung cấp mã dễ bảo trì/tái cấu trúc hơn và mạnh mẽ hơn đối với những kịch bản này.

### `const`

Ngoài `let`, ES6 còn giới thiệu `const`, cũng tạo ra một biến có phạm vi khối, nhưng giá trị của nó là cố định (hằng số). Bất kỳ nỗ lực nào để thay đổi giá trị đó sau này đều dẫn đến một lỗi.

```js
var foo = true;

if (foo) {
	var a = 2;
	const b = 3; // có phạm vi khối đối với `if` chứa nó

	a = 3; // hoàn toàn ổn!
	b = 4; // lỗi!
}

console.log( a ); // 3
console.log( b ); // ReferenceError!
```

## Ôn lại (Tóm tắt)

Hàm là đơn vị phạm vi phổ biến nhất trong JavaScript. Các biến và hàm được khai báo bên trong một hàm khác về cơ bản là "ẩn" khỏi bất kỳ "phạm vi" bao quanh nào, đó là một nguyên tắc thiết kế có chủ ý của phần mềm tốt.

Nhưng hàm không phải là đơn vị phạm vi duy nhất. Phạm vi khối đề cập đến ý tưởng rằng các biến và hàm có thể thuộc về một khối mã tùy ý (thường là bất kỳ cặp `{ .. }` nào), thay vì chỉ thuộc về hàm bao quanh.

Bắt đầu từ ES3, cấu trúc `try/catch` có phạm vi khối trong mệnh đề `catch`.

Trong ES6, từ khóa `let` (một người anh em của từ khóa `var`) được giới thiệu để cho phép khai báo các biến trong bất kỳ khối mã tùy ý nào. `if (..) { let a = 2; }` sẽ khai báo một biến `a` mà về cơ bản chiếm quyền kiểm soát phạm vi của khối `{ .. }` của `if` và tự gắn mình vào đó.

Mặc dù một số người dường như tin như vậy, phạm vi khối không nên được coi là một sự thay thế hoàn toàn cho phạm vi hàm `var`. Cả hai chức năng cùng tồn tại, và các nhà phát triển có thể và nên sử dụng cả kỹ thuật phạm vi hàm và phạm vi khối ở những nơi thích hợp để tạo ra mã tốt hơn, dễ đọc/dễ bảo trì hơn.

[^note-leastprivilege]: [Nguyên tắc Đặc quyền Tối thiểu](http://en.wikipedia.org/wiki/Principle_of_least_privilege)
