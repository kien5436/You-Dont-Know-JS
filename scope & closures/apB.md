# Bạn không hiểu JS: Phạm vi & Hàm khép kín
# Phụ lục B: Tương thích ngược cho Phạm vi khối

Trong Chương 3, chúng ta đã khảo sát về Phạm vi khối. Chúng ta đã thấy rằng `with` và mệnh đề `catch` đều là những ví dụ nhỏ cho thấy phạm vi khối đã tồn tại trong JavaScript tối thiểu là từ khi ES3 ra đời.

Nhưng chính sự xuất hiện của `let` trong ES6 sau cùng đã mang đến cho mã nguồn của chúng ta một năng lực phạm vi khối toàn diện và không bị ràng buộc. Có rất nhiều điều thú vị, cả về mặt chức năng lẫn phong cách mã nguồn, mà phạm vi khối sẽ mang lại.

Nhưng sẽ ra sao nếu chúng ta muốn sử dụng phạm vi khối trong các môi trường trước ES6?

Hãy xem xét đoạn mã này:

```js
{
	let a = 2;
	console.log( a ); // 2
}

console.log( a ); // ReferenceError
```

Đoạn mã này sẽ hoạt động tuyệt vời trong các môi trường ES6. Nhưng liệu chúng ta có thể làm vậy trong các môi trường trước ES6 không? `catch` chính là câu trả lời.

```js
try{throw 2}catch(a){
	console.log( a ); // 2
}

console.log( a ); // ReferenceError
```

Ôi! Đoạn mã này trông thật xấu xí và kỳ dị. Chúng ta thấy một khối `try/catch` xuất hiện chỉ để cố tình ném ra một lỗi, nhưng "lỗi" mà nó ném ra chỉ là giá trị `2`, và sau đó biến nhận giá trị đó lại được khai báo trong mệnh đề `catch(a)`. Não: bùm.

Đúng vậy, mệnh đề `catch` có phạm vi khối riêng, điều đó có nghĩa là nó có thể được dùng như một mã tương thích ngược cho phạm vi khối trong các môi trường trước ES6.

Bạn sẽ nói: "Nhưng... chẳng ai muốn viết một đoạn mã xấu xí như thế cả!". Điều đó đúng. Cũng chẳng có ai viết (một vài) đoạn mã do chương trình biên dịch CoffeeScript xuất ra cả. Vấn đề không nằm ở đó.

Vấn đề nằm ở chỗ các công cụ có thể chuyển dịch mã ES6 để hoạt động trong các môi trường trước ES6. Bạn có thể viết mã sử dụng phạm vi khối, hưởng lợi từ chức năng đó và để cho một công cụ ở bước xây dựng đảm nhiệm việc tạo ra đoạn mã thực sự *hoạt động* được khi triển khai.

Đây thực sự là lộ trình chuyển đổi được ưu tiên cho tất cả (e hèm, hầu hết) các tính năng của ES6: sử dụng một chương trình chuyển dịch mã để lấy mã ES6 và tạo ra mã tương thích với ES5 trong giai đoạn chuyển tiếp từ trước ES6 lên ES6.

## Traceur

Google duy trì một dự án có tên là "Traceur" [^note-traceur], với nhiệm vụ chính xác là chuyển dịch các tính năng của ES6 thành mã trước ES6 (chủ yếu là ES5, nhưng không phải tất cả!) để sử dụng rộng rãi. Ủy ban TC39 dựa vào công cụ này (và các công cụ khác) để kiểm tra ngữ nghĩa của các tính năng mà họ đặc tả.

Traceur tạo ra kết quả gì từ đoạn mã của chúng ta? Chắc bạn cũng đoán ra rồi!

```js
{
	try {
		throw undefined;
	} catch (a) {
		a = 2;
		console.log( a );
	}
}

console.log( a );
```

Vậy, với việc sử dụng các công cụ như thế, chúng ta có thể bắt đầu tận dụng lợi thế của phạm vi khối bất kể chúng ta đang nhắm đến ES6 hay không, bởi vì `try/catch` đã tồn tại (và hoạt động theo cách này) từ thời ES3.

## Khối ngầm định và Khối tường minh

Trong Chương 3, chúng ta đã xác định một số cạm bẫy tiềm tàng đối với khả năng bảo trì và tái cấu trúc mã nguồn khi chúng ta đưa phạm vi khối vào sử dụng. Liệu có cách nào khác để tận dụng phạm vi khối nhưng giảm bớt được nhược điểm này không?

Hãy xem xét một dạng khác của `let`, được gọi là "khối let" hoặc "câu lệnh let" (đối lập với "khai báo let" đã nói trước đây).

```js
let (a = 2) {
	console.log( a ); // 2
}

console.log( a ); // ReferenceError
```

Thay vì ngầm chiếm dụng một khối lệnh có sẵn, câu lệnh let tạo ra một khối lệnh tường minh cho việc liên kết phạm vi của nó. Khối lệnh tường minh không chỉ nổi bật hơn, và có lẽ bền vững hơn trong quá trình tái cấu trúc mã, mà nó còn tạo ra mã nguồn có phần sạch sẽ hơn bằng cách, về mặt ngữ pháp, buộc tất cả các khai báo phải nằm ở đầu khối. Điều này giúp ta dễ dàng nhìn vào bất kỳ khối lệnh nào và biết được những gì thuộc về phạm vi của nó.

Về mặt mẫu hình, nó phản ánh cách tiếp cận mà nhiều người sử dụng trong phạm vi hàm khi họ tự tay di chuyển/nâng tất cả các khai báo `var` của mình lên đầu hàm. Câu lệnh let đặt chúng ở đầu khối một cách có chủ đích, và nếu bạn không sử dụng các khai báo `let` rải rác khắp nơi, các khai báo phạm vi khối của bạn sẽ dễ nhận biết và bảo trì hơn phần nào.

Nhưng có một vấn đề. Dạng câu lệnh let không nằm trong ES6. Chương trình biên dịch Traceur chính thức cũng không chấp nhận dạng mã này.

Chúng ta có hai lựa chọn. Chúng ta có thể định dạng bằng cú pháp hợp lệ của ES6 và thêm một chút quy ước mã:

```js
/*let*/ { let a = 2;
	console.log( a );
}

console.log( a ); // ReferenceError
```

Nhưng công cụ sinh ra là để giải quyết vấn đề cho chúng ta. Vì vậy, lựa chọn còn lại là viết các khối câu lệnh let tường minh và để một công cụ chuyển đổi chúng thành mã hợp lệ và hoạt động được.

Thế là tôi xây dựng một công cụ có tên là "let-er" [^note-let_er] để giải quyết chính vấn đề này. *let-er* là một chương trình chuyển dịch mã ở bước xây dựng, nhưng nhiệm vụ duy nhất của nó là tìm các dạng câu lệnh let và chuyển dịch chúng. Nó sẽ không đụng đến phần còn lại của mã nguồn, bao gồm bất kỳ khai báo let nào. Bạn có thể an toàn sử dụng *let-er* như bước chuyển dịch ES6 đầu tiên, và sau đó đưa mã của bạn qua một công cụ khác như Traceur nếu cần.

Hơn nữa, *let-er* có một cờ cấu hình là `--es6`, khi được bật (mặc định là tắt), nó sẽ thay đổi loại mã được tạo ra. Thay vì dùng mẹo tương thích ngược `try/catch` của ES3, *let-er* sẽ lấy đoạn mã của chúng ta và tạo ra phiên bản hoàn toàn tuân thủ ES6 và không dùng mẹo:

```js
{
	let a = 2;
	console.log( a );
}

console.log( a ); // ReferenceError
```

Như vậy, bạn có thể bắt đầu sử dụng *let-er* ngay lập tức và nhắm đến tất cả các môi trường trước ES6, và khi bạn chỉ quan tâm đến ES6, bạn có thể thêm cờ này vào và ngay lập tức chỉ nhắm đến ES6.

Và quan trọng nhất, **bạn có thể sử dụng dạng câu lệnh let tường minh và được ưa chuộng hơn** mặc dù nó không phải là một phần chính thức của bất kỳ phiên bản ES nào (cho đến nay).

## Hiệu năng

Cho phép tôi nói thêm một ghi chú ngắn sau cùng về hiệu năng của `try/catch`, và/hoặc để giải quyết câu hỏi, "tại sao không chỉ dùng một IIFE để tạo ra phạm vi?"

Thứ nhất, hiệu năng của `try/catch` *đúng là* chậm hơn, nhưng không có giả định hợp lý nào cho rằng nó *phải* như vậy, hoặc thậm chí nó *sẽ luôn* như vậy. Vì chương trình chuyển dịch ES6 chính thức được TC39 phê duyệt sử dụng `try/catch`, đội ngũ Traceur đã yêu cầu Chrome cải thiện hiệu năng của `try/catch`, và rõ ràng họ có động lực để làm điều đó.

Thứ hai, IIFE không phải là một sự so sánh tương đương với `try/catch`, bởi vì một hàm bao bọc quanh bất kỳ đoạn mã tùy ý nào cũng sẽ làm thay đổi ý nghĩa của `this`, `return`, `break`, và `continue` bên trong đoạn mã đó. IIFE không phải là một phương án thay thế tổng quát phù hợp. Nó chỉ có thể được sử dụng thủ công trong một số trường hợp nhất định.

Câu hỏi thực sự trở thành: bạn có muốn dùng phạm vi khối hay không. Nếu có, những công cụ này cung cấp cho bạn lựa chọn đó. Nếu không, hãy tiếp tục sử dụng `var` và viết mã theo cách của bạn!

[^note-traceur]: [Google Traceur](http://google.github.io/traceur-compiler/demo/repl.html)

[^note-let_er]: [let-er](https://github.com/getify/let-er)
