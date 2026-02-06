# Bạn không hiểu JS: `this` và Nguyên mẫu Đối tượng
# Chương 1: `this` hay là that?

Một trong những cơ chế gây hoang mang bậc nhất trong JavaScript chính là từ khóa `this`. Nó là một từ khóa định danh đặc biệt, được tự động định nghĩa trong phạm vi của mọi hàm, nhưng việc nó trỏ đích xác vào đâu lại là một bài toán làm đau đầu ngay cả những lập trình viên JavaScript lão làng.

> Bất kỳ công nghệ nào đủ *tân tiến* cũng không thể phân biệt được với ma thuật. -- Arthur C. Clarke

Cơ chế `this` của JavaScript thực ra không *tân tiến đến thế*, nhưng các lập trình viên thường diễn giải lại câu trích dẫn trên trong đầu bằng cách thêm vào hai chữ "phức tạp" hay "rối rắm", và chắc chắn rằng khi thiếu đi một sự thấu hiểu tường tận, `this` có thể trở nên kỳ diệu đúng nghĩa trong mớ bòng bong của *chính bạn*.

**Lưu ý:** Từ "this" là một đại từ cực kỳ phổ biến trong văn nói hàng ngày. Vì vậy, có thể rất khó khăn, đặc biệt là khi giao tiếp bằng lời, để xác định xem chúng ta đang dùng "this" như một đại từ hay đang muốn đề cập đến từ khóa định danh thực sự. Để rõ ràng, tôi sẽ luôn dùng `this` để chỉ từ khóa đặc biệt, và "this" hoặc *this* trong các trường hợp khác.

## Vì sao cần `this`?

Nếu cơ chế `this` lại phức tạp đến vậy ngay cả với các lập trình viên JavaScript giàu kinh nghiệm, hẳn sẽ có người tự hỏi nó thực sự hữu dụng đến đâu? Liệu nó có phiền phức hơn giá trị mà nó mang lại không? Trước khi đi sâu vào phần *cách thức*, chúng ta nên xem xét phần *lý do*.

Hãy thử minh họa động cơ và tiện ích của `this`:

```js
function identify() {
	return this.name.toUpperCase();
}

function speak() {
	var greeting = "Hello, I'm " + identify.call( this );
	console.log( greeting );
}

var me = {
	name: "Kyle"
};

var you = {
	name: "Reader"
};

identify.call( me ); // KYLE
identify.call( you ); // READER

speak.call( me ); // Hello, I'm KYLE
speak.call( you ); // Hello, I'm READER
```

Nếu phần *cách thức* của đoạn mã này làm bạn bối rối, đừng lo! Chúng ta sẽ sớm tìm hiểu nó. Chỉ cần tạm gác những câu hỏi đó sang một bên để có thể nhìn vào phần *lý do* một cách rõ ràng hơn.

Đoạn mã này cho phép các hàm `identify()` và `speak()` được tái sử dụng trên nhiều đối tượng *ngữ cảnh* (`me` và `you`), thay vì phải tạo ra một phiên bản hàm riêng biệt cho mỗi đối tượng.

Thay vì dựa vào `this`, bạn có thể đã truyền một đối tượng ngữ cảnh một cách tường minh cho cả `identify()` và `speak()`.

```js
function identify(context) {
	return context.name.toUpperCase();
}

function speak(context) {
	var greeting = "Hello, I'm " + identify( context );
	console.log( greeting );
}

identify( you ); // READER
speak( me ); // Hello, I'm KYLE
```

Tuy nhiên, cơ chế `this` mang lại một phương thức tao nhã hơn để "truyền đi" một tham chiếu đối tượng một cách ngầm định, giúp cho thiết kế API trở nên sáng sủa và việc tái sử dụng cũng dễ dàng hơn.

Mô hình sử dụng của bạn càng phức tạp, bạn sẽ càng thấy rõ rằng việc truyền ngữ cảnh như một tham số tường minh thường lộn xộn hơn là truyền một ngữ cảnh `this`. Khi chúng ta khám phá các đối tượng và nguyên mẫu, bạn sẽ thấy sự hữu ích của việc một tập hợp các hàm có thể tự động tham chiếu đến đối tượng ngữ cảnh phù hợp.

## Những ngộ nhận

Chúng ta sẽ sớm bắt đầu giải thích cách `this` hoạt động *trên thực tế*, nhưng trước hết, cần phải đập tan một vài lầm tưởng về cách nó *không* hoạt động.

Cái tên "this" tạo ra sự nhầm lẫn khi các lập trình viên cố gắng suy nghĩ về nó một cách quá sát nghĩa đen. Có hai ý nghĩa thường được gán cho nó nhưng cả hai đều không chính xác.

### Chính nó

Xu hướng phổ biến đầu tiên là giả định `this` trỏ đến chính hàm đó. Ít nhất thì đó cũng là một suy luận ngữ pháp hợp lý.

Tại sao bạn lại muốn tham chiếu đến một hàm từ bên trong chính nó? Những lý do phổ biến nhất có thể là các tác vụ như đệ quy (gọi một hàm từ bên trong chính nó) hoặc có một hàm xử lý sự kiện có thể tự hủy liên kết khi nó được gọi lần đầu tiên.

Các lập trình viên mới tiếp cận với cơ chế của JS thường nghĩ rằng việc tham chiếu hàm như một đối tượng (tất cả các hàm trong JavaScript đều là đối tượng!) cho phép bạn lưu trữ *trạng thái* (các giá trị trong thuộc tính) giữa các lần gọi hàm. Mặc dù điều này hoàn toàn có thể và có một số công dụng hạn chế, phần còn lại của cuốn sách sẽ trình bày nhiều mô hình khác để lưu trữ trạng thái ở những nơi *tốt hơn* thay vì trên chính đối tượng hàm.

Nhưng với hiện tại, chúng ta sẽ khám phá mô hình đó để minh họa cách `this` không cho phép một hàm có được tham chiếu đến chính nó như chúng ta có thể đã giả định.

Hãy xem xét đoạn mã sau, nơi chúng ta cố gắng theo dõi số lần một hàm (`foo`) được gọi:

```js
function foo(num) {
	console.log( "foo: " + num );

	// theo dõi số lần `foo` được gọi
	this.count++;
}

foo.count = 0;

var i;

for (i=0; i<10; i++) {
	if (i > 5) {
		foo( i );
	}
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// `foo` đã được gọi bao nhiêu lần?
console.log( foo.count ); // 0 -- Cái quái gì vậy?
```

`foo.count` *vẫn* là `0`, mặc dù bốn câu lệnh `console.log` chỉ ra rõ ràng rằng `foo(..)` thực sự đã được gọi bốn lần. Sự khó chịu này bắt nguồn từ việc diễn giải *quá sát nghĩa đen* về ý nghĩa của `this` (trong `this.count++`).

Khi đoạn mã thực thi `foo.count = 0`, nó thực sự đang thêm một thuộc tính `count` vào đối tượng hàm `foo`. Nhưng đối với tham chiếu `this.count` bên trong hàm, trên thực tế `this` *hoàn toàn không* trỏ đến đối tượng hàm đó, và do đó, mặc dù tên thuộc tính giống nhau, các đối tượng gốc lại khác nhau, và sự nhầm lẫn xảy ra.

**Lưu ý:** Một lập trình viên có trách nhiệm *nên* tự hỏi ở thời điểm này: "Nếu tôi đang tăng một thuộc tính `count` nhưng nó không phải là cái tôi mong đợi, vậy tôi *đã* tăng `count` nào?" Trên thực tế, nếu cô ấy đào sâu hơn, cô ấy sẽ phát hiện ra rằng mình đã vô tình tạo ra một biến toàn cục `count` (xem Chương 2 để biết *làm thế nào* điều đó xảy ra!), và nó hiện có giá trị là `NaN`. Tất nhiên, một khi cô ấy xác định được kết quả kỳ lạ này, cô ấy lại có một loạt câu hỏi khác: "Làm thế nào nó lại là biến toàn cục, và tại sao nó lại thành `NaN` thay vì một giá trị đếm phù hợp?" (xem Chương 2).

Thay vì dừng lại ở điểm này và đào sâu vào lý do tại sao tham chiếu `this` dường như không hoạt động như *mong đợi*, và trả lời những câu hỏi khó nhưng quan trọng đó, nhiều lập trình viên chỉ đơn giản là lảng tránh vấn đề và tìm đến một giải pháp khác, chẳng hạn như tạo một đối tượng khác để chứa thuộc tính `count`:

```js
function foo(num) {
	console.log( "foo: " + num );

	// theo dõi số lần `foo` được gọi
	data.count++;
}

var data = {
	count: 0
};

var i;

for (i=0; i<10; i++) {
	if (i > 5) {
		foo( i );
	}
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// `foo` đã được gọi bao nhiêu lần?
console.log( data.count ); // 4
```

Mặc dù đúng là cách tiếp cận này "giải quyết" được vấn đề, nhưng không may là nó lại đơn thuần lờ đi vấn đề thật sự - sự thiếu hiểu biết về ý nghĩa và cách hoạt động của `this` - và thay vào đó lại quay về vùng an toàn của một cơ chế quen thuộc hơn: phạm vi từ vựng.

**Lưu ý:** Phạm vi từ vựng là một cơ chế hoàn toàn tốt và hữu ích; tôi không hề có ý xem nhẹ việc sử dụng nó (xem cuốn *"Phạm vi & Cơ chế bao đóng"* trong loạt sách này). Nhưng việc liên tục *đoán mò* cách sử dụng `this`, và thường là *sai*, không phải là một lý do chính đáng để quay về với phạm vi từ vựng và không bao giờ tìm hiểu *tại sao* `this` lại lẩn tránh bạn.

Để tham chiếu đến một đối tượng hàm từ bên trong chính nó, chỉ riêng `this` thường là không đủ. Bạn thường cần một tham chiếu đến đối tượng hàm thông qua một định danh từ vựng (biến) trỏ đến nó.

Hãy xem xét hai hàm sau:

```js
function foo() {
	foo.count = 4; // `foo` tham chiếu đến chính nó
}

setTimeout( function(){
	// hàm vô danh (không có tên), không thể
	// tham chiếu đến chính nó
}, 10 );
```

Trong hàm đầu tiên, được gọi là "hàm có tên", `foo` là một tham chiếu có thể được sử dụng để trỏ đến hàm từ bên trong chính nó.

Nhưng trong ví dụ thứ hai, hàm callback được truyền cho `setTimeout(..)` không có định danh tên (do đó được gọi là "hàm vô danh"), vì vậy không có cách nào phù hợp để tham chiếu đến chính đối tượng hàm đó.

**Lưu ý:** Tham chiếu `arguments.callee` kiểu cũ, hiện đã lỗi thời và không được khuyến khích, bên trong một hàm *cũng* trỏ đến đối tượng hàm của hàm đang thực thi. Tham chiếu này thường là cách duy nhất để truy cập đối tượng của một hàm vô danh từ bên trong chính nó. Tuy nhiên, cách tiếp cận tốt nhất là hoàn toàn tránh sử dụng các hàm vô danh, ít nhất là đối với những hàm yêu cầu tự tham chiếu, và thay vào đó hãy sử dụng một hàm (biểu thức) có tên. `arguments.callee` đã lỗi thời và không nên được sử dụng.

Vì vậy, một giải pháp khác cho ví dụ của chúng ta sẽ là sử dụng định danh `foo` như một tham chiếu đối tượng hàm ở mọi nơi, và hoàn toàn không sử dụng `this`, cách này *hoạt động*:

```js
function foo(num) {
	console.log( "foo: " + num );

	// theo dõi số lần `foo` được gọi
	foo.count++;
}

foo.count = 0;

var i;

for (i=0; i<10; i++) {
	if (i > 5) {
		foo( i );
	}
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// `foo` đã được gọi bao nhiêu lần?
console.log( foo.count ); // 4
```

Tuy nhiên, cách tiếp cận đó cũng tương tự là né tránh việc hiểu *thực sự* về `this` và hoàn toàn dựa vào phạm vi từ vựng của biến `foo`.

Thêm một cách tiếp cận vấn đề khác là ép `this` thực sự trỏ đến đối tượng hàm `foo`:

```js
function foo(num) {
	console.log( "foo: " + num );

	// theo dõi số lần `foo` được gọi
	// Lưu ý: `this` BÂY GIỜ thực sự LÀ `foo`, dựa trên
	// cách `foo` được gọi (xem bên dưới)
	this.count++;
}

foo.count = 0;

var i;

for (i=0; i<10; i++) {
	if (i > 5) {
		// sử dụng `call(..)`, chúng ta đảm bảo `this`
		// trỏ đến chính đối tượng hàm (`foo`)
		foo.call( foo, i );
	}
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// `foo` đã được gọi bao nhiêu lần?
console.log( foo.count ); // 4
```

**Thay vì né tránh `this`, chúng ta đón nhận nó.** Chúng ta sẽ giải thích chi tiết hơn *cách thức* các kỹ thuật như vậy hoạt động trong một lát nữa, vì vậy đừng lo lắng nếu bạn vẫn còn hơi bối rối!

### Phạm vi của nó

Ngộ nhận phổ biến tiếp theo về ý nghĩa của `this` là bằng cách nào đó nó trỏ đến phạm vi của hàm. Đây là một câu hỏi hóc búa, bởi vì ở một khía cạnh nào đó thì có một phần sự thật, nhưng ở khía cạnh khác, nó lại khá sai lầm.

Để nói cho rõ, `this` không hề, theo bất kỳ cách nào, trỏ đến **phạm vi từ vựng** của một hàm. Đúng là ở bên trong, phạm vi giống như một đối tượng với các thuộc tính cho mỗi định danh có sẵn. Nhưng "đối tượng" phạm vi đó không thể truy cập được bằng mã JavaScript. Nó là một phần nội tại trong việc triển khai của *Bộ máy*.

Hãy xem xét đoạn mã cố gắng (và thất bại!) vượt qua ranh giới và sử dụng `this` để ngầm định tham chiếu đến phạm vi từ vựng của một hàm:

```js
function foo() {
	var a = 2;
	this.bar();
}

function bar() {
	console.log( this.a );
}

foo(); //undefined
```

Có nhiều hơn một lỗi trong đoạn mã này. Mặc dù nó có vẻ được dàn dựng, đoạn mã bạn thấy là sự chắt lọc từ những đoạn mã thực tế đã được trao đổi trên các diễn đàn trợ giúp cộng đồng. Đó là một minh chứng tuyệt vời (nếu không muốn nói là đáng buồn) về việc những giả định về `this` có thể sai lầm đến mức nào.

Thứ nhất, có một nỗ lực tham chiếu đến hàm `bar()` thông qua `this.bar()`. Việc nó hoạt động gần như chắc chắn là một sự *tình cờ*, nhưng chúng ta sẽ giải thích *cách thức* của điều đó ngay sau đây. Cách tự nhiên nhất để gọi `bar()` sẽ là bỏ đi `this.` ở đầu và chỉ cần thực hiện một tham chiếu từ vựng đến định danh đó.

Tuy nhiên, lập trình viên viết đoạn mã như vậy đang cố gắng sử dụng `this` để tạo ra một cầu nối giữa các phạm vi từ vựng của `foo()` và `bar()`, để `bar()` có thể truy cập vào biến `a` trong phạm vi nội tại của `foo()`. **Không có một cầu nối nào như vậy có thể tồn tại.** Bạn không thể sử dụng một tham chiếu `this` để tra cứu một thứ gì đó trong phạm vi từ vựng. Điều đó là không thể.

Mỗi khi bạn thấy mình đang cố gắng trộn lẫn việc tra cứu trong phạm vi từ vựng với `this`, hãy tự nhắc nhở bản thân: *không hề có một cầu nối nào cả*.

## Vậy `this` là gì?

Sau khi gạt bỏ những giả định sai lầm, chúng ta hãy chuyển sự chú ý đến cách cơ chế `this` thực sự hoạt động.

Chúng ta đã nói trước đó rằng `this` không phải là một ràng buộc tại thời điểm khởi tạo mà là một ràng buộc tại thời điểm thực thi. Nó mang tính ngữ cảnh, dựa trên các điều kiện khi hàm được gọi. Ràng buộc `this` không liên quan gì đến nơi một hàm được khai báo mà hoàn toàn phụ thuộc vào cách thức hàm đó được gọi.

Khi một hàm được gọi, một bản ghi kích hoạt, hay còn gọi là bối cảnh thực thi, được tạo ra. Bản ghi này chứa thông tin về nơi hàm được gọi (ngăn xếp gọi hàm), *cách thức* hàm được gọi, những tham số nào đã được truyền vào, v.v. Một trong những thuộc tính của bản ghi này là tham chiếu `this` sẽ được sử dụng trong suốt quá trình thực thi của hàm đó.

Trong chương tiếp theo, chúng ta sẽ học cách tìm ra **điểm gọi** của một hàm để xác định cách việc thực thi của nó sẽ ràng buộc `this`.

## Tóm tắt

Ràng buộc `this` là một nguồn cơn gây nhầm lẫn không ngớt cho các lập trình viên JavaScript không dành thời gian để tìm hiểu cách cơ chế này thực sự hoạt động. Việc đoán mò, thử-và-sai, và sao chép-dán một cách mù quáng từ các câu trả lời trên Stack Overflow không phải là một cách hiệu quả hay đúng đắn để tận dụng cơ chế `this` quan trọng này.

Để học về `this`, trước tiên bạn phải học `this` *không phải* là gì, bất chấp mọi giả định hay ngộ nhận có thể dẫn bạn đi sai đường. `this` không phải là một tham chiếu đến chính hàm đó, cũng không phải là một tham chiếu đến phạm vi *từ vựng* của hàm.

`this` thực chất là một ràng buộc được tạo ra khi một hàm được gọi, và *thứ* mà nó trỏ tới được quyết định hoàn toàn bởi điểm gọi nơi hàm đó được thực thi.
