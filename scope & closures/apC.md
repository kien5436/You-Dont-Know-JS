# Bạn không hiểu JS: Phạm vi & Hàm khép kín
# Phụ lục C: `this` theo phạm vi từ vựng

Mặc dù tiêu đề này không đi sâu vào giải quyết cơ chế `this`, tuy nhiên có một chủ đề trong ES6 liên hệ `this` với phạm vi từ vựng theo một cách thức quan trọng, và chúng ta sẽ nhanh chóng xem xét nó.

ES6 bổ sung một dạng cú pháp đặc biệt để khai báo hàm gọi là "hàm mũi tên". Nó trông như thế này:

```js
var foo = a => {
	console.log( a );
};

foo( 2 ); // 2
```

Cái gọi là "mũi tên béo" thường được nhắc đến như một cách viết tắt cho từ khóa `function` *dài dòng đến phát ngán* (một cách mỉa mai).

Nhưng có một điều quan trọng hơn nhiều đang diễn ra với các hàm mũi tên, một điều không liên quan gì đến việc tiết kiệm vài lần gõ phím khi khai báo.

Nói ngắn gọn, đoạn mã này gặp phải một vấn đề:

```js

var obj = {
	id: "awesome",
	cool: function coolFn() {
		console.log( this.id );
	}
};

var id = "not awesome";

obj.cool(); // awesome

setTimeout( obj.cool, 100 ); // not awesome
```

Vấn đề nằm ở việc hàm `cool()` bị mất đi sự ràng buộc của `this`. Có nhiều cách khác nhau để giải quyết vấn đề đó, nhưng một giải pháp thường được nhắc đi nhắc lại là `var self = this;`.

Nó có thể trông như sau:

```js
var obj = {
	count: 0,
	cool: function coolFn() {
		var self = this;

		if (self.count < 1) {
			setTimeout( function timer(){
				self.count++;
				console.log( "awesome?" );
			}, 100 );
		}
	}
};

obj.cool(); // awesome?
```

Không đi quá sâu vào chi tiết ở đây, nhưng cái "giải pháp" `var self = this` chỉ đơn giản là lảng tránh toàn bộ vấn đề về việc thấu hiểu và sử dụng đúng đắn sự ràng buộc của `this`, thay vào đó lại dựa vào một thứ mà có lẽ chúng ta cảm thấy quen thuộc hơn: phạm vi từ vựng. `self` chỉ trở thành một định danh có thể được phân giải thông qua phạm vi từ vựng và cơ chế bao đóng, và không mảy may quan tâm đến những gì đã xảy ra với sự ràng buộc của `this` trên suốt chặng đường.

Người ta không thích viết những thứ dài dòng, đặc biệt là khi phải lặp đi lặp lại. Do đó, một trong những động lực của ES6 là giúp giảm bớt những tình huống này, và thực ra là *sửa chữa* những vấn đề cố hữu trong các lối viết mã phổ biến, ví như vấn đề này.

Giải pháp của ES6, hàm mũi tên, giới thiệu một hành vi được gọi là "`this` theo phạm vi từ vựng".

```js
var obj = {
	count: 0,
	cool: function coolFn() {
		if (this.count < 1) {
			setTimeout( () => { // hàm mũi tên ra tay?
				this.count++;
				console.log( "awesome?" );
			}, 100 );
		}
	}
};

obj.cool(); // awesome?
```

Giải thích ngắn gọn là khi xét đến sự ràng buộc của `this`, các hàm mũi tên không hành xử giống như hàm thông thường một chút nào. Chúng loại bỏ tất cả các quy tắc thông thường về ràng buộc `this`, thay vào đó nhận lấy giá trị `this` từ phạm vi từ vựng bao bọc ngay bên ngoài nó, bất kể đó là gì.

Vì vậy, trong đoạn mã trên, hàm mũi tên không bị mất ràng buộc `this` theo một cách khó lường nào đó, nó chỉ đơn giản là "kế thừa" sự ràng buộc `this` của hàm `cool()` (điều này là chính xác nếu chúng ta gọi nó như đã trình bày!).

Mặc dù điều này giúp mã ngắn gọn hơn, nhưng theo quan điểm của tôi, hàm mũi tên thực chất chỉ là đang mã hóa một *sai lầm* phổ biến của giới lập trình viên vào cú pháp ngôn ngữ, đó là việc nhầm lẫn và đánh đồng các quy tắc về "ràng buộc `this`" với các quy tắc về "phạm vi từ vựng".

Nói cách khác: tại sao phải nhọc công và dài dòng để sử dụng mô thức lập trình theo phong cách `this`, chỉ để rồi tự triệt tiêu đi thế mạnh của nó bằng cách trộn lẫn với các tham chiếu từ vựng. Dường như sẽ tự nhiên hơn nếu ta chấp nhận một trong hai cách tiếp cận cho bất kỳ đoạn mã nào và không pha trộn chúng trong cùng một đoạn mã.

**Lưu ý:** một điểm trừ khác của hàm mũi tên là chúng vô danh, không được đặt tên. Xem Chương 3 để biết lý do tại sao các hàm vô danh lại kém ưu thế hơn các hàm có tên.

Theo quan điểm của tôi, một cách tiếp cận thích hợp hơn cho "vấn đề" này là sử dụng và chấp nhận cơ chế `this` một cách đúng đắn.

```js
var obj = {
	count: 0,
	cool: function coolFn() {
		if (this.count < 1) {
			setTimeout( function timer(){
				this.count++; // `this` an toàn nhờ có `bind(..)`
				console.log( "more awesome" );
			}.bind( this ), 100 ); // kìa, `bind()`!
		}
	}
};

obj.cool(); // more awesome
```

Dù bạn ưa chuộng hành vi `this` theo phạm vi từ vựng của hàm mũi tên hay bạn thích giải pháp `bind()` đã được kiểm chứng qua thời gian, điều quan trọng cần lưu ý là hàm mũi tên **không** chỉ đơn thuần là gõ "function" ít hơn.

Chúng có một *hành vi khác biệt có chủ đích* mà chúng ta nên học hỏi, thấu hiểu, và tận dụng nếu muốn.

Giờ đây khi chúng ta đã hoàn toàn thấu suốt về phạm vi từ vựng (và cơ chế bao đóng!), việc hiểu được `this` theo phạm vi từ vựng sẽ dễ như trở bàn tay!
