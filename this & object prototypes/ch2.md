# Bạn không hiểu JS: `this` và Nguyên mẫu Đối tượng
# Chương 2: Giờ thì `this` đã có ý nghĩa!

Trong Chương 1, chúng ta đã loại bỏ những ngộ nhận khác nhau về `this` và thay vào đó học được rằng `this` là một ràng buộc được tạo ra cho mỗi lần gọi hàm, hoàn toàn dựa vào **nơi gọi** của nó (cách mà hàm được gọi).

## Nơi gọi 

Để hiểu được ràng buộc `this`, chúng ta phải hiểu về nơi gọi: vị trí trong mã nguồn nơi một hàm được gọi (**chứ không phải nơi nó được khai báo**). Chúng ta phải xem xét kỹ nơi gọi để trả lời câu hỏi: cái `this` *này* đang tham chiếu đến cái gì?

Việc tìm ra nơi gọi nhìn chung là: "đi xác định vị trí mà từ đó một hàm được gọi", nhưng không phải lúc nào cũng dễ dàng như vậy vì một số mô hình viết mã có thể che mờ đi nơi gọi *thực sự*.

Điều quan trọng là phải suy nghĩ về **ngăn xếp gọi hàm** (ngăn xếp các hàm đã được gọi để đưa chúng ta đến thời điểm thực thi hiện tại). Nơi gọi mà chúng ta quan tâm nằm *trong* lời gọi *ngay trước* hàm đang được thực thi.

Hãy cùng minh họa về ngăn xếp gọi hàm và nơi gọi:

```js
function baz() {
    // ngăn xếp gọi hàm là: `baz`
    // vì vậy, nơi gọi của chúng ta nằm trong phạm vi toàn cục

    console.log( "baz" );
    bar(); // <-- nơi gọi cho `bar`
}

function bar() {
    // ngăn xếp gọi hàm là: `baz` -> `bar`
    // vì vậy, nơi gọi của chúng ta nằm trong `baz`

    console.log( "bar" );
    foo(); // <-- nơi gọi cho `foo`
}

function foo() {
    // ngăn xếp gọi hàm là: `baz` -> `bar` -> `foo`
    // vì vậy, nơi gọi của chúng ta nằm trong `bar`

    console.log( "foo" );
}

baz(); // <-- nơi gọi cho `baz`
```

Cần hết sức cẩn trọng khi phân tích mã nguồn để tìm ra nơi gọi thực sự (từ ngăn xếp gọi hàm), bởi vì đó là yếu tố duy nhất có ý nghĩa đối với ràng buộc `this`.

**Lưu ý:** Bạn có thể hình dung một ngăn xếp lời gọi trong đầu bằng cách nhìn vào chuỗi các lời gọi hàm theo thứ tự như chúng ta đã làm với các chú thích trong đoạn mã trên. Nhưng cách này rất vất vả và dễ sai sót. Một cách khác để xem ngăn xếp lời gọi là sử dụng công cụ gỡ lỗi (debugger) trong chương trình duyệt của bạn. Hầu hết các chương trình duyệt dành cho máy tính hiện đại đều có sẵn công cụ dành cho nhà phát triển, bao gồm một chương trình gỡ lỗi JS. Trong đoạn mã trên, bạn có thể đặt một điểm ngắt trong công cụ tại dòng đầu tiên của hàm `foo()`, hoặc đơn giản là chèn câu lệnh `debugger;` vào dòng đầu tiên đó. Khi bạn chạy trang, chương trình gỡ lỗi sẽ tạm dừng tại vị trí này và hiển thị cho bạn một danh sách các hàm đã được gọi để đến được dòng đó, đó chính là ngăn xếp gọi hàm của bạn. Vì vậy, nếu bạn đang cố gắng chẩn đoán ràng buộc `this`, hãy sử dụng công cụ dành cho nhà phát triển để lấy ngăn xếp lời gọi, sau đó tìm đến mục thứ hai từ trên xuống, nó sẽ cho bạn thấy nơi gọi thực sự.

## Chỉ Tuân Theo Các Quy Tắc

Bây giờ, chúng ta sẽ chuyển sự chú ý sang việc *cách thức* mà nơi gọi xác định `this` sẽ trỏ đến đâu trong quá trình thực thi một hàm.

Bạn phải xem xét nơi gọi và xác định quy tắc nào trong 4 quy tắc sau được áp dụng. Đầu tiên, chúng ta sẽ giải thích riêng từng quy tắc, sau đó sẽ minh họa thứ tự ưu tiên của chúng nếu có nhiều quy tắc *có thể* áp dụng cho nơi gọi đó.

### Ràng buộc Mặc định

Quy tắc đầu tiên chúng ta xem xét đến từ trường hợp gọi hàm phổ biến nhất: lời gọi hàm độc lập. Hãy coi quy tắc `this` *này* là quy tắc mặc định sau cùng khi không có quy tắc nào khác được áp dụng.

Xét đoạn mã sau:

```js
function foo() {
	console.log( this.a );
}

var a = 2;

foo(); // 2
```

Điều đầu tiên cần lưu ý nếu bạn chưa biết, các biến được khai báo trong phạm vi toàn cục như `var a = 2` đồng nghĩa với các thuộc tính của đối tượng toàn cục có cùng tên. Chúng không phải là bản sao của nhau, chúng *chính là* một. Hãy nghĩ về nó như hai mặt của cùng một đồng xu.

Thứ hai, chúng ta thấy rằng khi `foo()` được gọi, `this.a` phân giải ra biến toàn cục `a` của chúng ta. Tại sao? Bởi vì trong trường hợp này, *ràng buộc mặc định* cho `this` được áp dụng cho lời gọi hàm và do đó trỏ `this` vào đối tượng toàn cục.

Làm thế nào chúng ta biết quy tắc *ràng buộc mặc định* được áp dụng ở đây? Chúng ta xem xét nơi gọi để thấy `foo()` được gọi như thế nào. Trong đoạn mã của chúng ta, `foo()` được gọi bằng một tham chiếu hàm đơn thuần, không đi kèm gì cả. Không có quy tắc nào khác mà chúng ta sẽ trình bày được áp dụng ở đây, vì vậy *ràng buộc mặc định* sẽ được áp dụng.

Nếu `chế độ nghiêm ngặt` (`strict mode`) có hiệu lực, đối tượng toàn cục không đủ điều kiện cho *ràng buộc mặc định*, vì vậy `this` thay vào đó sẽ được đặt thành `undefined`.

```js
function foo() {
	"use strict";

	console.log( this.a );
}

var a = 2;

foo(); // TypeError: `this` is `undefined`
```

Một chi tiết tinh vi nhưng quan trọng là: mặc dù các quy tắc ràng buộc `this` nói chung hoàn toàn dựa vào nơi gọi, đối tượng toàn cục **chỉ** đủ điều kiện cho *ràng buộc mặc định* nếu **nội dung** của `foo()` **không** chạy trong `chế độ nghiêm ngặt`; trạng thái `chế độ nghiêm ngặt` của nơi gọi `foo()` không liên quan gì.

```js
function foo() {
	console.log( this.a );
}

var a = 2;

(function(){
	"use strict";

	foo(); // 2
})();
```

**Lưu ý:** Việc cố tình trộn lẫn `chế độ nghiêm ngặt` và không `nghiêm ngặt` với nhau trong mã của bạn thường không được khuyến khích. Toàn bộ chương trình của bạn có lẽ nên là **Nghiêm ngặt** hoặc **Không nghiêm ngặt**. Tuy nhiên, đôi khi bạn sử dụng một thư viện của bên thứ ba có độ **Nghiêm ngặt** khác với mã của riêng bạn, vì vậy cần phải cẩn thận với những chi tiết tương thích tinh vi này.

### Ràng buộc Ngầm

Một quy tắc khác cần xem xét là: nơi gọi có đối tượng ngữ cảnh, đôi khi còn được gọi là đối tượng sở hữu hoặc đối tượng chứa, hay không, dù *những* thuật ngữ này có thể hơi gây hiểu lầm.

Xét ví dụ:

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2,
	foo: foo
};

obj.foo(); // 2
```

Trước tiên, hãy chú ý đến cách `foo()` được khai báo và sau đó được thêm vào như một thuộc tính tham chiếu trên `obj`. Bất kể ban đầu `foo()` được khai báo *trên* `obj` hay được thêm vào như một tham chiếu sau đó (như đoạn mã cho thấy), trong cả hai trường hợp, **hàm** này không thực sự được "sở hữu" hay "chứa" bởi đối tượng `obj`.

Tuy nhiên, nơi gọi *sử dụng* ngữ cảnh `obj` để **tham chiếu** đến hàm, vì vậy bạn *có thể* nói rằng đối tượng `obj` "sở hữu" hoặc "chứa" **tham chiếu hàm** tại thời điểm hàm được gọi.

Dù bạn chọn gọi khuôn mẫu này là gì, tại thời điểm `foo()` được gọi, nó nằm sau một tham chiếu đối tượng đến `obj`. Khi có một đối tượng ngữ cảnh cho một tham chiếu hàm, quy tắc *ràng buộc ngầm* nói rằng chính đối tượng *đó* sẽ được sử dụng cho ràng buộc `this` của lời gọi hàm.

Bởi vì `obj` là `this` cho lời gọi `foo()`, `this.a` đồng nghĩa với `obj.a`.

Chỉ có cấp trên cùng/cuối cùng của một chuỗi tham chiếu thuộc tính đối tượng mới ảnh hưởng tới nơi gọi. Ví dụ:

```js
function foo() {
	console.log( this.a );
}

var obj2 = {
	a: 42,
	foo: foo
};

var obj1 = {
	a: 2,
	obj2: obj2
};

obj1.obj2.foo(); // 42
```

#### Mất ràng buộc ngầm

Một trong những phiền toái phổ biến nhất mà ràng buộc `this` tạo ra là khi một hàm được *ràng buộc ngầm* bị mất ràng buộc đó, điều này thường có nghĩa là nó sẽ quay về với *ràng buộc mặc định*, của đối tượng toàn cục hoặc `undefined`, tùy thuộc vào `chế độ nghiêm ngặt`.

Xét ví dụ:

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2,
	foo: foo
};

var bar = obj.foo; // tham chiếu/bí danh của hàm!

var a = "oops, global"; // `a` cũng là thuộc tính trên đối tượng toàn cục

bar(); // "oops, global"
```

Mặc dù `bar` có vẻ là một tham chiếu đến `obj.foo`, nhưng trên thực tế, nó thực sự chỉ là một tham chiếu khác đến chính `foo`. Hơn nữa, nơi gọi mới là điều quan trọng, và nơi gọi là `bar()`, một lời gọi đơn thuần, không đi kèm gì cả và do đó quy tắc *ràng buộc mặc định* được áp dụng.

Cách thức tinh vi hơn, phổ biến hơn và bất ngờ hơn mà điều này xảy ra là khi chúng ta xem xét việc truyền vào một hàm gọi lại:

```js
function foo() {
	console.log( this.a );
}

function doFoo(fn) {
	// `fn` chỉ là một tham chiếu khác đến `foo`

	fn(); // <-- nơi gọi!
}

var obj = {
	a: 2,
	foo: foo
};

var a = "oops, global"; // `a` cũng là thuộc tính trên đối tượng toàn cục

doFoo( obj.foo ); // "oops, global"
```

Việc truyền tham số chỉ là một phép gán ngầm, và vì chúng ta đang truyền một hàm, đó là một phép gán tham chiếu ngầm nên kết quả cuối cùng giống như đoạn mã trước.

Nếu hàm mà bạn đang truyền hàm gọi lại vào không phải của riêng bạn mà được tích hợp sẵn trong ngôn ngữ thì sao? Không có gì khác biệt, kết quả tương tự.

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2,
	foo: foo
};

var a = "oops, global"; // `a` cũng là thuộc tính trên đối tượng toàn cục

setTimeout( obj.foo, 100 ); // "oops, global"
```

Hãy nghĩ về cách giả lập thô sơ về mặt lý thuyết của `setTimeout()` được cung cấp như một hàm tích hợp sẵn từ môi trường JavaScript:

```js
function setTimeout(fn,delay) {
	// chờ (bằng cách nào đó) trong `delay` mili giây
	fn(); // <-- nơi gọi!
}
```

Việc các hàm gọi lại của chúng ta *mất* ràng buộc `this` là khá phổ biến, như chúng ta vừa thấy. Nhưng một cách khác mà `this` có thể gây ngạc nhiên cho chúng ta là khi hàm mà chúng ta đã truyền hàm gọi lại vào cố tình thay đổi `this` cho lời gọi. Các hàm xử lý sự kiện trong các thư viện JavaScript phổ biến khá ưa thích việc ép hàm gọi lại của bạn phải có `this` trỏ đến, ví dụ, phần tử DOM đã kích hoạt sự kiện. Mặc dù điều này đôi khi có thể hữu ích, những lúc khác nó có thể cực kỳ khó chịu. Thật không may, những công cụ này hiếm khi cho phép bạn lựa chọn.

Dù `this` bị thay đổi theo một cách không mong muốn, bạn cũng không thực sự kiểm soát được cách tham chiếu hàm gọi lại của mình sẽ được thực thi, vì vậy bạn không (chưa) có cách nào để điều khiển nơi gọi nhằm đưa ra ràng buộc mà bạn mong muốn. Chúng ta sẽ sớm thấy một cách để "sửa" vấn đề đó bằng cách *cố định* `this`.

### Ràng buộc Tường minh

Với *ràng buộc ngầm* như chúng ta vừa thấy, chúng ta phải thay đổi đối tượng đang xét để chứa một tham chiếu đến hàm trên chính nó, và sử dụng tham chiếu hàm thuộc tính này để gián tiếp (ngầm) ràng buộc `this` với đối tượng.

Nhưng nếu bạn muốn ép một lời gọi hàm phải sử dụng một đối tượng cụ thể cho ràng buộc `this` mà không cần đặt một tham chiếu hàm thuộc tính trên đối tượng đó thì sao?

"Tất cả" các hàm trong ngôn ngữ đều có một số tiện ích có sẵn (thông qua `[[Prototype]]` của chúng - sẽ nói thêm về điều này sau) có thể hữu ích cho nhiệm vụ này. Cụ thể, các hàm có phương thức `call(..)` và `apply(..)`. Về mặt kỹ thuật, các môi trường máy chủ JavaScript đôi khi cung cấp các hàm đủ đặc biệt (một cách nói giảm nói tránh!) đến mức chúng không có chức năng như vậy. Nhưng số đó rất ít. Phần lớn các hàm được cung cấp, và chắc chắn là tất cả các hàm bạn sẽ tạo, đều có quyền truy cập vào `call(..)` và `apply(..)`.

Các tiện ích này hoạt động như thế nào? Cả hai đều nhận tham số đầu tiên làm một đối tượng để sử dụng cho `this`, và sau đó gọi hàm với `this` đã được chỉ định. Vì bạn đang trực tiếp nêu rõ bạn muốn `this` là gì, chúng ta gọi nó là *ràng buộc tường minh*.

Xét ví dụ:

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2
};

foo.call( obj ); // 2
```

Việc gọi `foo` với *ràng buộc tường minh* bằng `foo.call(..)` cho phép chúng ta ép `this` của nó phải là `obj`.

Nếu bạn truyền một giá trị nguyên thủy đơn giản (thuộc kiểu `string`, `boolean`, hoặc `number`) làm ràng buộc `this`, giá trị nguyên thủy đó sẽ được bao bọc trong dạng đối tượng của nó (`new String(..)`, `new Boolean(..)`, hoặc `new Number(..)` tương ứng). Điều này thường được gọi là "đóng hộp" (boxing).

**Lưu ý:** Đối với ràng buộc `this`, `call(..)` và `apply(..)` là giống hệt nhau. Chúng *có* hoạt động khác nhau với các tham số bổ sung, nhưng đó không phải là điều chúng ta quan tâm hiện tại.

Thật không may, chỉ riêng *ràng buộc tường minh* vẫn không đưa ra giải pháp nào cho vấn đề đã đề cập trước đó, về việc một hàm "mất" ràng buộc `this` dự định của nó, hoặc bị một bộ khung ghi đè lên, v.v.

#### Ràng buộc cứng

Nhưng một biến thể của khuôn mẫu *ràng buộc tường minh* thực sự giải quyết được vấn đề. Xét ví dụ:

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2
};

var bar = function() {
	foo.call( obj );
};

bar(); // 2
setTimeout( bar, 100 ); // 2

// `bar` ràng buộc cứng `this` của `foo` vào `obj`
// để nó không thể bị ghi đè
bar.call( window ); // 2
```

Hãy xem xét cách biến thể này hoạt động. Chúng ta tạo ra một hàm `bar()` mà bên trong nó, gọi `foo.call(obj)` một cách thủ công, do đó ép `foo` được gọi với ràng buộc `this` là `obj`. Bất kể sau này bạn gọi hàm `bar` như thế nào, nó sẽ luôn luôn gọi `foo` với `obj` một cách thủ công. Ràng buộc này vừa rõ ràng vừa mạnh mẽ, vì vậy chúng ta gọi nó là *ràng buộc cứng*.

Cách điển hình nhất để bao bọc một hàm với *ràng buộc cứng* là tạo ra một đường thông qua cho bất kỳ đối số nào được truyền vào và bất kỳ giá trị trả về nào nhận được:

```js
function foo(something) {
	console.log( this.a, something );
	return this.a + something;
}

var obj = {
	a: 2
};

var bar = function() {
	return foo.apply( obj, arguments );
};

var b = bar( 3 ); // 2 3
console.log( b ); // 5
```

Một cách khác để thể hiện khuôn mẫu này là tạo một hàm trợ giúp tái sử dụng được:

```js
function foo(something) {
	console.log( this.a, something );
	return this.a + something;
}

// hàm trợ giúp `ràng buộc` đơn giản
function bind(fn, obj) {
	return function() {
		return fn.apply( obj, arguments );
	};
}

var obj = {
	a: 2
};

var bar = bind( foo, obj );

var b = bar( 3 ); // 2 3
console.log( b ); // 5
```

Vì *ràng buộc cứng* là một khuôn mẫu rất phổ biến, nó được cung cấp một tiện ích tích hợp sẵn kể từ ES5: `Function.prototype.bind`, và được sử dụng như sau:

```js
function foo(something) {
	console.log( this.a, something );
	return this.a + something;
}

var obj = {
	a: 2
};

var bar = foo.bind( obj );

var b = bar( 3 ); // 2 3
console.log( b ); // 5
```

`bind(..)` trả về một hàm mới được mã hóa cứng để gọi hàm gốc với ngữ cảnh `this` được thiết lập như bạn đã chỉ định.

**Lưu ý:** Kể từ ES6, hàm được ràng buộc cứng do `bind(..)` tạo ra có thuộc tính `.name` bắt nguồn từ *hàm mục tiêu* ban đầu. Ví dụ: `bar = foo.bind(..)` sẽ có giá trị `bar.name` là `"bound foo"`, đây là tên lời gọi hàm sẽ hiển thị trong dấu vết ngăn xếp (stack trace).

#### Ngữ cảnh trong lời gọi API

Nhiều hàm của các thư viện, và thực sự là nhiều hàm tích hợp mới trong ngôn ngữ JavaScript và môi trường máy chủ, cung cấp một tham số tùy chọn, thường được gọi là "ngữ cảnh", được thiết kế như một giải pháp thay thế để bạn không phải sử dụng `bind(..)` nhằm đảm bảo hàm gọi lại của bạn sử dụng một `this` cụ thể.

Ví dụ:

```js
function foo(el) {
	console.log( el, this.id );
}

var obj = {
	id: "awesome"
};

// sử dụng `obj` làm `this` cho các lời gọi `foo(..)`
[1, 2, 3].forEach( foo, obj ); // 1 awesome  2 awesome  3 awesome
```

Bên trong, các hàm khác nhau này gần như chắc chắn sử dụng *ràng buộc tường minh* thông qua `call(..)` hoặc `apply(..)` để giúp bạn tiết kiệm công sức.

### Ràng buộc `new`

Quy tắc thứ tư và cuối cùng cho ràng buộc `this` đòi hỏi chúng ta phải suy nghĩ lại về một quan niệm sai lầm rất phổ biến về hàm và đối tượng trong JavaScript.

Trong các ngôn ngữ hướng lớp truyền thống, "hàm khởi tạo" là các phương thức đặc biệt được gắn vào các lớp, khi lớp được khởi tạo bằng toán tử `new`, hàm khởi tạo của lớp đó sẽ được gọi. Điều này thường có dạng:

```js
something = new MyClass(..);
```

JavaScript có toán tử `new`, và khuôn mẫu mã để sử dụng nó về cơ bản giống hệt với những gì chúng ta thấy trong các ngôn ngữ hướng lớp đó; hầu hết các nhà phát triển cho rằng cơ chế của JavaScript cũng đang làm điều tương tự. Tuy nhiên, thực sự *không có mối liên hệ nào* với chức năng hướng lớp được ngụ ý bởi việc sử dụng `new` trong JS.

Trước tiên, hãy định nghĩa lại "hàm khởi tạo" trong JavaScript là gì. Trong JS, hàm khởi tạo **chỉ là các hàm** tình cờ được gọi với toán tử `new` đứng trước chúng. Chúng không được gắn vào các lớp, cũng không phải là đang khởi tạo một lớp. Chúng thậm chí không phải là các loại hàm đặc biệt. Về bản chất, chúng chỉ là các hàm thông thường bị chiếm quyền bởi việc sử dụng `new` trong lời gọi của chúng.

Ví dụ, hàm `Number(..)` hoạt động như một hàm khởi tạo, trích dẫn từ đặc tả ES5.1:

> 15.7.2 Hàm khởi tạo Number
>
> Khi Number được gọi như một phần của một biểu thức new, nó là một hàm khởi tạo: nó khởi tạo đối tượng mới được tạo ra.

Vì vậy, gần như bất kỳ hàm nào, bao gồm cả các hàm đối tượng tích hợp sẵn như `Number(..)` (xem Chương 3) đều có thể được gọi với `new` đứng trước nó, và điều đó làm cho lời gọi hàm đó trở thành một *lời gọi hàm khởi tạo*. Đây là một sự khác biệt quan trọng nhưng tinh vi: thực sự không có cái gọi là "hàm khởi tạo" mà là các lời gọi khởi tạo *của* các hàm.

Khi một hàm được gọi với `new` đứng trước nó, hay còn gọi là lời gọi hàm khởi tạo, những điều sau đây sẽ được thực hiện tự động:

1. một đối tượng hoàn toàn mới được tạo ra (hay còn gọi là được khởi tạo) từ hư không
2. *đối tượng mới được khởi tạo được liên kết `[[Prototype]]`*
3. đối tượng mới được khởi tạo được đặt làm ràng buộc `this` cho lời gọi hàm đó
4. trừ khi hàm trả về một **đối tượng** thay thế của riêng nó, lời gọi hàm được gọi bằng `new` sẽ *tự động* trả về đối tượng mới được khởi tạo.

Các bước 1, 3 và 4 áp dụng cho cuộc thảo luận hiện tại của chúng ta. Chúng ta sẽ bỏ qua bước 2 bây giờ và quay lại nó trong Chương 5.

Xét đoạn mã này:

```js
function foo(a) {
	this.a = a;
}

var bar = new foo( 2 );
console.log( bar.a ); // 2
```

Bằng cách gọi `foo(..)` với `new` đứng trước, chúng ta đã khởi tạo một đối tượng mới và đặt đối tượng mới đó làm `this` cho lời gọi của `foo(..)`. **Vậy `new` là cách cuối cùng mà `this` của một lời gọi hàm có thể được ràng buộc.** Chúng ta sẽ gọi đây là *ràng buộc new*.

## Thứ Tự Của Mọi Quy Tắc

Vậy là chúng ta đã khám phá ra 4 quy tắc để ràng buộc `this` trong các lời gọi hàm. *Tất cả* những gì bạn cần làm là tìm nơi gọi và xem quy tắc nào được áp dụng. Nhưng nếu nơi gọi có nhiều quy tắc đủ điều kiện thì sao? Phải có một thứ tự ưu tiên cho các quy tắc này, và vì vậy tiếp theo chúng ta sẽ chứng minh thứ tự áp dụng các quy tắc.

Rõ ràng là *ràng buộc mặc định* là quy tắc có độ ưu tiên thấp nhất trong 4 cái. Vì vậy, chúng ta sẽ tạm gác nó sang một bên.

Cái nào có độ ưu tiên cao hơn, *ràng buộc ngầm* hay *ràng buộc tường minh*? Hãy kiểm tra:

```js
function foo() {
	console.log( this.a );
}

var obj1 = {
	a: 2,
	foo: foo
};

var obj2 = {
	a: 3,
	foo: foo
};

obj1.foo(); // 2
obj2.foo(); // 3

obj1.foo.call( obj2 ); // 3
obj2.foo.call( obj1 ); // 2
```

Vậy là *ràng buộc tường minh* có độ ưu tiên cao hơn *ràng buộc ngầm*, đồng nghĩa với việc **đầu tiên** bạn nên hỏi là liệu *ràng buộc tường minh* có áp dụng hay không trước khi kiểm tra *ràng buộc ngầm*.

Bây giờ, chúng ta chỉ cần tìm ra vị trí của *ràng buộc new* trong thứ tự ưu tiên.

```js
function foo(something) {
	this.a = something;
}

var obj1 = {
	foo: foo
};

var obj2 = {};

obj1.foo( 2 );
console.log( obj1.a ); // 2

obj1.foo.call( obj2, 3 );
console.log( obj2.a ); // 3

var bar = new obj1.foo( 4 );
console.log( obj1.a ); // 2
console.log( bar.a ); // 4
```

Được rồi, *ràng buộc new* có độ ưu tiên cao hơn *ràng buộc ngầm*. Nhưng bạn nghĩ *ràng buộc new* có độ ưu tiên cao hơn hay thấp hơn *ràng buộc tường minh*?

**Lưu ý:** `new` và `call`/`apply` không thể được sử dụng cùng nhau, vì vậy `new foo.call(obj1)` không được phép dùng để kiểm tra *ràng buộc new* trực tiếp với *ràng buộc tường minh*. Nhưng chúng ta vẫn có thể sử dụng *ràng buộc cứng* để kiểm tra độ ưu tiên của hai quy tắc.

Trước khi chúng ta khám phá điều đó trong một đoạn mã, hãy nghĩ lại về cách *ràng buộc cứng* hoạt động về mặt vật lý, đó là `Function.prototype.bind(..)` tạo ra một hàm bao bọc mới được mã hóa cứng để bỏ qua ràng buộc `this` của chính nó (bất kể nó là gì), và sử dụng một ràng buộc thủ công mà chúng ta cung cấp.

Theo lý luận đó, giả định rằng *ràng buộc cứng* (là một dạng của *ràng buộc tường minh*) có độ ưu tiên cao hơn *ràng buộc new*, và do đó không thể bị ghi đè bằng `new` dường như rõ ràng.

Hãy kiểm tra:

```js
function foo(something) {
	this.a = something;
}

var obj1 = {};

var bar = foo.bind( obj1 );
bar( 2 );
console.log( obj1.a ); // 2

var baz = new bar( 3 );
console.log( obj1.a ); // 2
console.log( baz.a ); // 3
```

Ồ! `bar` được ràng buộc cứng với `obj1`, nhưng `new bar(3)` đã **không** thay đổi `obj1.a` thành `3` như chúng ta mong đợi. Thay vào đó, lời gọi *ràng buộc cứng* (với `obj1`) đến `bar(..)` ***có thể*** bị ghi đè bằng `new`. Vì `new` đã được áp dụng, chúng ta nhận lại đối tượng mới được tạo ra mà chúng ta đặt tên là `baz`, và chúng ta thấy trên thực tế rằng `baz.a` có giá trị `3`.

Điều này sẽ gây ngạc nhiên nếu bạn quay lại hàm trợ giúp `bind` "giả" của chúng ta:

```JS
function bind(fn, obj) {
	return function() {
		fn.apply( obj, arguments );
	};
}
```

Nếu bạn suy luận về cách mã của hàm trợ giúp hoạt động, bạn sẽ thấy rằng không có cách nào để một lời gọi toán tử `new` có thể ghi đè lên ràng buộc cứng với `obj` như chúng ta vừa quan sát.

Nhưng `Function.prototype.bind(..)` tích hợp sẵn kể từ ES5 thì tinh vi hơn, thực tế là tinh vi hơn khá nhiều. Đây là bản tương thích ngược (đã được định dạng lại một chút) được cung cấp bởi trang MDN cho `bind(..)`:

```js
if (!Function.prototype.bind) {
	Function.prototype.bind = function(oThis) {
		if (typeof this !== "function") {
			// hàm gần nhất với
			// hàm IsCallable bên trong ECMAScript 5
			throw new TypeError( "Function.prototype.bind - what " +
				"is trying to be bound is not callable"
			);
		}

		var aArgs = Array.prototype.slice.call( arguments, 1 ),
			fToBind = this,
			fNOP = function(){},
			fBound = function(){
				return fToBind.apply(
					(
						this instanceof fNOP &&
						oThis ? this : oThis
					),
					aArgs.concat( Array.prototype.slice.call( arguments ) )
				);
			}
		;

		fNOP.prototype = this.prototype;
		fBound.prototype = new fNOP();

		return fBound;
	};
}
```

**Lưu ý:** Hàm tương thích ngược `bind(..)` được hiển thị ở trên khác với `bind(..)` tích hợp trong ES5 đối với các hàm được ràng buộc cứng sẽ được sử dụng với `new` (xem bên dưới để biết tại sao điều đó hữu ích). Bởi vì hàm tương thích ngược không thể tạo ra một hàm không có `.prototype` như tiện ích tích hợp làm, có một số sự gián tiếp tinh vi để đạt được hành vi tương tự. Hãy cẩn thận nếu bạn dự định sử dụng `new` với một hàm được ràng buộc cứng và bạn dựa vào hàm tương thích ngược này.

Phần cho phép `new` ghi đè là:

```js
this instanceof fNOP &&
oThis ? this : oThis

// ... và:

fNOP.prototype = this.prototype;
fBound.prototype = new fNOP();
```

Chúng ta sẽ không thực sự đi sâu vào giải thích cách mánh khóe này hoạt động (nó phức tạp và nằm ngoài phạm vi của chúng ta ở đây), nhưng về cơ bản, tiện ích này xác định xem hàm được ràng buộc cứng có được gọi bằng `new` hay không (dẫn đến một đối tượng mới được khởi tạo là `this` của nó), và nếu có, nó sử dụng `this` mới được tạo *đó* thay vì *ràng buộc cứng* cho `this` đã được chỉ định trước đó.

Tại sao việc `new` có thể ghi đè *ràng buộc cứng* lại hữu ích?

Lý do chính cho hành vi này là để tạo ra một hàm (có thể được sử dụng với `new` để khởi tạo đối tượng) mà về cơ bản bỏ qua ràng buộc *cứng* của `this` nhưng lại đặt trước một số hoặc tất cả các đối số của hàm. Một trong những khả năng của `bind(..)` là bất kỳ đối số nào được truyền sau đối số ràng buộc `this` đầu tiên đều được mặc định là các đối số tiêu chuẩn cho hàm bên dưới (về mặt kỹ thuật được gọi là "ứng dụng cục bộ" (partial application), là một tập hợp con của "kĩ thuật phân rã tham số").

Ví dụ:

```js
function foo(p1,p2) {
	this.val = p1 + p2;
}

// sử dụng `null` ở đây vì chúng ta không quan tâm đến
// ràng buộc cứng `this` trong kịch bản này, và
// nó sẽ bị ghi đè bởi lời gọi `new`!
var bar = foo.bind( null, "p1" );

var baz = new bar( "p2" );

baz.val; // p1p2
```

### Xác định `this`

Bây giờ, chúng ta có thể tóm tắt các quy tắc để xác định `this` từ nơi gọi của một lời gọi hàm, theo thứ tự ưu tiên của chúng. Hãy đặt những câu hỏi này theo thứ tự và dừng lại khi quy tắc đầu tiên được áp dụng.

1. Hàm có được gọi với `new` (**ràng buộc new**) không? Nếu có, `this` là đối tượng mới được khởi tạo.

    `var bar = new foo()`

2. Hàm có được gọi với `call` hoặc `apply` (**ràng buộc tường minh**), ngay cả khi ẩn bên trong một *ràng buộc cứng* `bind` không? Nếu có, `this` là đối tượng được chỉ định một cách rõ ràng.

    `var bar = foo.call( obj2 )`

3. Hàm có được gọi với một ngữ cảnh (**ràng buộc ngầm**), hay còn gọi là một đối tượng sở hữu hoặc chứa, không? Nếu có, `this` là *đối tượng ngữ cảnh đó*.

    `var bar = obj1.foo()`

4. Nếu không, mặc định `this` (**ràng buộc mặc định**). Nếu trong `chế độ nghiêm ngặt`, chọn `undefined`, nếu không thì chọn đối tượng `global`.

    `var bar = foo()`

Chỉ vậy thôi. Đó là *tất cả những gì cần thiết* để hiểu các quy tắc ràng buộc `this` cho các lời gọi hàm thông thường. Chà... gần như vậy.

## Các Ngoại Lệ Của Ràng Buộc

Như thường lệ, có một số *ngoại lệ* cho các "quy tắc".

Hành vi ràng buộc `this` trong một số tình huống có thể gây ngạc nhiên, khi bạn dự định một ràng buộc khác nhưng cuối cùng lại nhận được hành vi ràng buộc từ quy tắc *ràng buộc mặc định* (xem phần trước).

### `this` Bị Bỏ Qua

Nếu bạn truyền `null` hoặc `undefined` làm tham số ràng buộc `this` cho `call`, `apply`, hoặc `bind`, những giá trị đó thực sự bị bỏ qua, và thay vào đó, quy tắc *ràng buộc mặc định* sẽ được áp dụng cho lời gọi.

```js
function foo() {
	console.log( this.a );
}

var a = 2;

foo.call( null ); // 2
```

Tại sao bạn lại cố tình truyền một cái gì đó như `null` cho một ràng buộc `this`?

Việc sử dụng `apply(..)` để trải các giá trị của một mảng ra làm tham số cho một lời gọi hàm là khá phổ biến. Tương tự, `bind(..)` có thể phân rã các tham số (giá trị được đặt trước), điều này có thể rất hữu ích.

```js
function foo(a,b) {
	console.log( "a:" + a + ", b:" + b );
}

// trải mảng ra làm tham số
foo.apply( null, [2, 3] ); // a:2, b:3

// phân rã tham số với `bind(..)`
var bar = foo.bind( null, 2 );
bar( 3 ); // a:2, b:3
```

Cả hai tiện ích này đều yêu cầu một ràng buộc `this` cho tham số đầu tiên. Nếu các hàm đang xét không quan tâm đến `this`, bạn cần một giá trị giữ chỗ, và `null` có vẻ là một lựa chọn hợp lý như được hiển thị trong đoạn mã này.

**Lưu ý:** Chúng ta không đề cập đến nó trong cuốn sách này, nhưng ES6 có toán tử phân rã `...` cho phép bạn "trải" một mảng ra làm tham số mà không cần `apply(..)`, chẳng hạn như `foo(...[1,2])`, tương đương với `foo(1,2)` - về mặt cú pháp tránh được ràng buộc `this` nếu không cần thiết. Thật không may, không có cú pháp thay thế nào trong ES6 cho kĩ thuật phân rã tham số, vì vậy tham số `this` của lời gọi `bind(..)` vẫn cần được chú ý.

Tuy nhiên, có một "nguy cơ" tiềm ẩn nhỏ trong việc luôn sử dụng `null` khi bạn không quan tâm đến ràng buộc `this`. Nếu bạn từng sử dụng điều đó với một lời gọi hàm (ví dụ, một hàm thư viện của bên thứ ba mà bạn không kiểm soát), và hàm đó *có* thực hiện một tham chiếu `this`, quy tắc *ràng buộc mặc định* đồng nghĩa với việc nó có thể vô tình tham chiếu (hoặc tệ hơn, thay đổi!) đối tượng `global` (`window` trong trình duyệt).

Rõ ràng, một cạm bẫy như vậy có thể dẫn đến một loạt các lỗi *rất khó* để chẩn đoán/truy vết.

#### `this` An toàn hơn

Có lẽ một cách làm "an toàn" hơn là truyền một đối tượng được thiết lập đặc biệt cho `this` sao cho đối tượng đó không thể tạo ra các hiệu ứng phụ có vấn đề trong chương trình của bạn. Mượn thuật ngữ từ mạng máy tính (và quân sự), chúng ta có thể tạo ra một đối tượng "DMZ" (vùng phi quân sự) - không có gì đặc biệt hơn một đối tượng hoàn toàn trống, không được ủy quyền (xem Chương 5 và 6).

Nếu chúng ta luôn truyền một đối tượng DMZ để bỏ qua các ràng buộc `this` mà chúng ta nghĩ rằng không cần quan tâm, chúng ta chắc chắn rằng bất kỳ việc sử dụng `this` ẩn/bất ngờ nào cũng sẽ bị giới hạn trong đối tượng trống, điều này cách ly đối tượng `global` của chương trình chúng ta khỏi các hiệu ứng phụ.

Vì đối tượng này hoàn toàn trống, cá nhân tôi thích đặt tên biến cho nó là `ø` (ký hiệu toán học viết thường cho tập hợp rỗng). Trên nhiều bàn phím (như bố cục US trên Mac), ký hiệu này có thể dễ dàng được gõ bằng `⌥`+`o` (option+`o`). Một số hệ thống cũng cho phép bạn thiết lập phím nóng cho các ký hiệu cụ thể. Nếu bạn không thích ký hiệu `ø`, hoặc bàn phím của bạn không dễ gõ được nó, tất nhiên bạn có thể gọi nó là bất cứ thứ gì bạn muốn.

Dù bạn gọi nó là gì, cách dễ nhất để thiết lập nó **hoàn toàn trống** là `Object.create(null)` (xem Chương 5). `Object.create(null)` tương tự như `{ }`, nhưng không có sự ủy quyền cho `Object.prototype`, vì vậy nó "trống hơn" so với chỉ `{ }`.

```js
function foo(a,b) {
	console.log( "a:" + a + ", b:" + b );
}

// đối tượng trống DMZ của chúng ta
var ø = Object.create( null );

// trải mảng ra làm tham số
foo.apply( ø, [2, 3] ); // a:2, b:3

// phân rã tham số với `bind(..)`
var bar = foo.bind( ø, 2 );
bar( 3 ); // a:2, b:3
```

Không chỉ "an toàn" hơn về mặt chức năng, còn có một lợi ích về phong cách với `ø`, ở chỗ nó truyền tải về mặt ngữ nghĩa "Tôi muốn `this` phải trống" một cách rõ ràng hơn một chút so với `null`. Nhưng một lần nữa, bạn có thể đặt bất cứ tên nào cho đối tượng DMZ.

### Tham Chiếu Gián Tiếp

Một điều khác cần lưu ý là bạn có thể (cố ý hoặc không!) tạo ra "tham chiếu gián tiếp" đến các hàm, và trong những trường hợp đó, khi tham chiếu hàm đó được gọi, quy tắc *ràng buộc mặc định* cũng được áp dụng.

Một trong những cách phổ biến nhất mà *tham chiếu gián tiếp* xảy ra là từ một phép gán:

```js
function foo() {
	console.log( this.a );
}

var a = 2;
var o = { a: 3, foo: foo };
var p = { a: 4 };

o.foo(); // 3
(p.foo = o.foo)(); // 2
```

*Giá trị kết quả* của biểu thức gán `p.foo = o.foo` chỉ là một tham chiếu đến đối tượng hàm bên dưới. Do đó, nơi gọi thực sự chỉ là `foo()`, không phải là `p.foo()` hay `o.foo()` như bạn có thể mong đợi. Theo các quy tắc ở trên, quy tắc *ràng buộc mặc định* được áp dụng.

Nhắc lại: bất kể bạn có được một lời gọi hàm sử dụng quy tắc *ràng buộc mặc định* như thế nào, trạng thái `chế độ nghiêm ngặt` của **nội dung** của hàm được gọi đang thực hiện tham chiếu `this` - chứ không phải nơi gọi hàm - sẽ quyết định giá trị *ràng buộc mặc định*: hoặc là đối tượng `global` nếu ở chế độ không `nghiêm ngặt` hoặc `undefined` nếu ở `chế độ nghiêm ngặt`.

### Ràng Buộc Mềm 

Trước đó chúng ta đã thấy rằng *ràng buộc cứng* là một chiến lược để ngăn một lời gọi hàm quay trở lại quy tắc *ràng buộc mặc định* một cách vô ý, bằng cách ép nó phải được ràng buộc với một `this` cụ thể (trừ khi bạn sử dụng `new` để ghi đè nó!). Vấn đề là *ràng buộc cứng* làm giảm đáng kể tính linh hoạt của một hàm, ngăn cản việc ghi đè `this` thủ công bằng *ràng buộc ngầm* hoặc thậm chí các nỗ lực *ràng buộc tường minh* sau đó.

Sẽ thật tuyệt nếu có một cách để cung cấp một mặc định khác cho *ràng buộc mặc định* (không phải `global` hay `undefined`), trong khi vẫn để hàm có thể được ràng buộc `this` thủ công thông qua các kỹ thuật *ràng buộc ngầm* hoặc *ràng buộc tường minh*.

Chúng ta có thể xây dựng một tiện ích gọi là *ràng buộc mềm* để mô phỏng hành vi mong muốn của mình.

```js
if (!Function.prototype.softBind) {
	Function.prototype.softBind = function(obj) {
		var fn = this,
			curried = [].slice.call( arguments, 1 ),
			bound = function bound() {
				return fn.apply(
					(!this ||
						(typeof window !== "undefined" &&
							this === window) ||
						(typeof global !== "undefined" &&
							this === global)
					) ? obj : this,
					curried.concat.apply( curried, arguments )
				);
			};
		bound.prototype = Object.create( fn.prototype );
		return bound;
	};
}
```

Tiện ích `softBind(..)` được cung cấp ở đây hoạt động tương tự như tiện ích `bind(..)` tích hợp sẵn của ES5, ngoại trừ hành vi *ràng buộc mềm* của chúng ta. Nó bao bọc hàm được chỉ định trong logic kiểm tra `this` tại thời điểm gọi và nếu nó là `global` hoặc `undefined`, nó sử dụng một *mặc định* thay thế đã được chỉ định trước (`obj`). Nếu không, `this` sẽ không bị đụng đến. Nó cũng cung cấp tùy chọn phân rã tham số (xem phần thảo luận về `bind(..)` trước đó).

Hãy cùng minh họa cách sử dụng nó:

```js
function foo() {
   console.log("name: " + this.name);
}

var obj = { name: "obj" },
    obj2 = { name: "obj2" },
    obj3 = { name: "obj3" };

var fooOBJ = foo.softBind( obj );

fooOBJ(); // name: obj

obj2.foo = foo.softBind(obj);
obj2.foo(); // name: obj2   <---- nhìn này!!!

fooOBJ.call( obj3 ); // name: obj3   <---- nhìn này!

setTimeout( obj2.foo, 10 ); // name: obj   <---- quay về ràng buộc mềm
```

Phiên bản ràng buộc mềm của hàm `foo()` có thể ràng buộc `this` thủ công với `obj2` hoặc `obj3` như trên, nhưng nó sẽ quay về `obj` nếu *ràng buộc mặc định* được áp dụng.

## `this` Cú pháp Từ vựng

Các hàm thông thường tuân theo 4 quy tắc chúng ta vừa đề cập. Nhưng ES6 giới thiệu một loại hàm đặc biệt không sử dụng các quy tắc này: hàm mũi tên.

Hàm mũi tên được biểu thị không phải bằng từ khóa `function`, mà bằng toán tử `=>`, hay còn gọi là "mũi tên béo". Thay vì sử dụng bốn quy tắc `this` tiêu chuẩn, hàm mũi tên kế thừa ràng buộc `this` từ phạm vi bao quanh nó (hàm hoặc toàn cục).

Hãy cùng minh họa phạm vi từ vựng của hàm mũi tên:

```js
function foo() {
	// trả về một hàm mũi tên
	return (a) => {
		// `this` ở đây được kế thừa theo cú pháp từ vựng từ `foo()`
		console.log( this.a );
	};
}

var obj1 = {
	a: 2
};

var obj2 = {
	a: 3
};

var bar = foo.call( obj1 );
bar.call( obj2 ); // 2, không phải 3!
```

Hàm mũi tên được tạo trong `foo()` nắm bắt theo cú pháp từ vựng bất cứ `this` nào của `foo()` tại thời điểm nó được gọi. Vì `foo()` đã được ràng buộc `this` với `obj1`, `bar` (một tham chiếu đến hàm mũi tên được trả về) cũng sẽ được ràng buộc `this` với `obj1`. Ràng buộc từ vựng của một hàm mũi tên không thể bị ghi đè (ngay cả với `new`!).

Trường hợp sử dụng phổ biến nhất có lẽ là trong việc sử dụng các hàm gọi lại, chẳng hạn như hàm xử lý sự kiện hoặc bộ đếm thời gian:

```js
function foo() {
	setTimeout(() => {
		// `this` ở đây được kế thừa theo cú pháp từ vựng từ `foo()`
		console.log( this.a );
	},100);
}

var obj = {
	a: 2
};

foo.call( obj ); // 2
```

Mặc dù hàm mũi tên cung cấp một giải pháp thay thế cho việc sử dụng `bind(..)` trên một hàm để đảm bảo `this` của nó, điều này có vẻ hấp dẫn, điều quan trọng cần lưu ý là về cơ bản chúng đang vô hiệu hóa cơ chế `this` truyền thống để ủng hộ phạm vi từ vựng được hiểu rộng rãi hơn. Trước ES6, chúng ta đã có một khuôn mẫu khá phổ biến để làm như vậy, về cơ bản gần như không thể phân biệt được với tinh thần của hàm mũi tên ES6:

```js
function foo() {
	var self = this; // nắm bắt `this` theo cú pháp từ vựng
	setTimeout( function(){
		console.log( self.a );
	}, 100 );
}

var obj = {
	a: 2
};

foo.call( obj ); // 2
```

Mặc dù cả `self = this` và hàm mũi tên đều có vẻ là những "giải pháp" tốt để tránh sử dụng `bind(..)`, về cơ bản chúng đang trốn chạy khỏi `this` thay vì tìm cách thấu hiểu và nắm bắt nó.

Nếu bạn thấy mình viết mã theo phong cách `this`, nhưng hầu hết hoặc tất cả thời gian, bạn lại vô hiệu hóa cơ chế `this` bằng các "mánh" từ vựng như `self = this` hoặc hàm mũi tên, có lẽ bạn nên:

1. Chỉ sử dụng phạm vi từ vựng và quên đi cái cớ giả tạo của mã theo phong cách `this`.

2. Nắm bắt hoàn toàn các cơ chế của phong cách `this`, bao gồm cả việc sử dụng `bind(..)` khi cần thiết, và cố gắng tránh các "mánh" `self = this` và "this từ vựng" của hàm mũi tên.

Một chương trình có thể sử dụng hiệu quả cả hai phong cách viết mã (từ vựng và `this`), nhưng bên trong cùng một hàm, và thực sự cho cùng một loại tra cứu, việc trộn lẫn hai cơ chế thường đòi hỏi viết mã khó bảo trì hơn, và có lẽ là đang làm việc quá sức để tỏ ra thông minh.

## Tổng Kết

Việc xác định ràng buộc `this` cho một hàm đang thực thi đòi hỏi phải tìm ra nơi gọi trực tiếp của hàm đó. Sau khi xem xét, bốn quy tắc có thể được áp dụng cho nơi gọi, theo thứ tự ưu tiên *này*:

1. Được gọi với `new`? Sử dụng đối tượng mới được khởi tạo.

2. Được gọi với `call` hoặc `apply` (hoặc `bind`)? Sử dụng đối tượng được chỉ định.

3. Được gọi với một đối tượng ngữ cảnh sở hữu lời gọi? Sử dụng đối tượng ngữ cảnh đó.

4. Mặc định: `undefined` trong `chế độ nghiêm ngặt`, nếu không thì là đối tượng toàn cục.

Hãy cẩn thận với việc vô tình/không chủ ý gọi quy tắc *ràng buộc mặc định*. Trong những trường hợp bạn muốn "an toàn" bỏ qua một ràng buộc `this`, một đối tượng "DMZ" như `ø = Object.create(null)` là một giá trị giữ chỗ tốt để bảo vệ đối tượng `global` khỏi các hiệu ứng phụ không mong muốn.

Thay vì bốn quy tắc ràng buộc tiêu chuẩn, hàm mũi tên ES6 sử dụng phạm vi từ vựng cho ràng buộc `this`, có nghĩa là chúng kế thừa ràng buộc `this` (bất kể nó là gì) từ lời gọi hàm bao quanh nó. Về cơ bản chúng là một sự thay thế cú pháp cho `self = this` trong mã trước ES6.
