# Bạn không hiểu JS: Phạm vi và Hàm khép kín
# Chương 5: Hàm khép kín trong Phạm vi

Chúng ta đi đến chương này với một sự am hiểu vững chắc và lành mạnh về cách hoạt động của phạm vi, tôi hy vọng là vậy.

Bây giờ, chúng ta sẽ chuyển sự chú ý sang một phần cực kỳ quan trọng nhưng lại luôn khó nắm bắt, *gần như huyền thoại* của ngôn ngữ: **hàm khép kín (closure)**. Nếu bạn đã theo dõi cuộc thảo luận của chúng ta về phạm vi từ vựng cho đến giờ, phần thưởng là khái niệm hàm khép kín sẽ trở nên, phần lớn, không có gì bất ngờ, gần như là điều hiển nhiên. *Có một người đằng sau tấm rèm của vị pháp sư, và chúng ta sắp diện kiến ông ta*. Không, tên ông ấy không phải là Crockford!

Tuy nhiên, nếu bạn vẫn còn những câu hỏi dai dẳng về phạm vi từ vựng, bây giờ là thời điểm tốt để quay lại và xem lại chương 2 trước khi tiếp tục.

## Khai sáng

Đối với những người đã có kinh nghiệm về JavaScript nhưng có lẽ chưa bao giờ nắm bắt trọn vẹn khái niệm hàm khép kín, việc *thấu hiểu hàm khép kín* có thể giống như một cõi niết bàn đặc biệt mà người ta phải phấn đấu và hy sinh để đạt được.

Tôi còn nhớ nhiều năm về trước, khi tôi đã nắm vững JavaScript nhưng lại không hề biết hàm khép kín là gì. Lời gợi ý rằng có *một khía cạnh khác* của ngôn ngữ, một khía cạnh hứa hẹn còn nhiều khả năng hơn cả những gì tôi đã sở hữu, đã trêu ngươi và ám ảnh tôi. Tôi nhớ mình đã đọc qua mã nguồn của các bộ khung thời kỳ đầu để cố gắng hiểu cách chúng thực sự hoạt động. Tôi nhớ lần đầu tiên một cái gì đó của "mô hình khối chức năng" bắt đầu manh nha trong tâm trí tôi. Tôi nhớ rất rõ những khoảnh khắc *à há!* ấy.

Điều tôi không biết lúc đó, điều mà tôi đã mất nhiều năm để hiểu, và điều tôi hy vọng sẽ truyền lại cho bạn ngay bây giờ, là bí mật này: **hàm khép kín hiện hữu xung quanh bạn trong JavaScript, bạn chỉ cần nhận ra và tiếp thu nó**. Hàm khép kín không phải là một công cụ tùy chọn đặc biệt mà bạn phải học cú pháp và mô hình mới để sử dụng. Không, hàm khép kín thậm chí không phải là một thứ vũ khí mà bạn phải học cách sử dụng và làm chủ như Luke luyện tập Thần Lực.

Hàm khép kín xảy ra như một kết quả của việc viết mã dựa trên phạm vi từ vựng. Chúng cứ thế xảy ra. Bạn thậm chí không phải thực sự cố ý tạo ra hàm khép kín để tận dụng chúng. Hàm khép kín được tạo ra và sử dụng ở khắp mọi nơi trong mã của bạn. Điều bạn đang *thiếu* là bối cảnh tư duy phù hợp để nhận ra, tiếp thu và vận dụng hàm khép kín theo ý mình.

Khoảnh khắc khai sáng sẽ là: **ồ, hàm khép kín đã và đang xảy ra trên khắp mã của tôi, cuối cùng thì tôi cũng có thể *nhìn thấy* chúng.** Hiểu được hàm khép kín cũng giống như khi Neo nhìn thấy Ma Trận lần đầu tiên vậy.

## Đi vào chi tiết

Thôi, đủ những lời khoa trương và các tham chiếu phim ảnh trơ trẽn rồi.

Đây là một định nghĩa trần trụi về những gì bạn cần biết để hiểu và nhận ra hàm khép kín:

> Hàm khép kín là khi một hàm có khả năng ghi nhớ và truy cập vào phạm vi từ vựng của nó, ngay cả khi hàm đó đang được thực thi bên ngoài phạm vi từ vựng của mình.

Hãy đi vào một số đoạn mã để minh họa cho định nghĩa đó.

```js
function foo() {
	var a = 2;

	function bar() {
		console.log( a ); // 2
	}

	bar();
}

foo();
```

Đoạn mã này trông khá quen thuộc từ các cuộc thảo luận của chúng ta về Phạm vi Lồng nhau. Hàm `bar()` có *quyền truy cập* vào biến `a` trong phạm vi bao ngoài nhờ các quy tắc tra cứu phạm vi từ vựng (trong trường hợp này, đó là một tra cứu tham chiếu RHS).

Đây có phải là "hàm khép kín" không?

Chà, về mặt kỹ thuật thì... *có lẽ*. Nhưng theo định nghĩa những-gì-bạn-cần-biết của chúng ta ở trên thì... *không hẳn*. Tôi nghĩ cách giải thích chính xác nhất về việc `bar()` tham chiếu đến `a` là thông qua các quy tắc tra cứu phạm vi từ vựng, và những quy tắc đó *chỉ* là **một phần** (quan trọng!) của khái niệm hàm khép kín.

Từ góc độ hoàn toàn học thuật, những gì có thể nói về đoạn mã trên là hàm `bar()` có một *sự khép kín* bao trùm phạm vi của `foo()` (và thực tế là cả trên các phạm vi còn lại mà nó có quyền truy cập, chẳng hạn như phạm vi toàn cục trong trường hợp của chúng ta). Nói một cách hơi khác, `bar()` đóng kín trên phạm vi của `foo()`. Tại sao? Bởi vì `bar()` xuất hiện lồng bên trong `foo()`. Rất đơn giản và rõ ràng.

Tuy nhiên, hàm khép kín được định nghĩa theo cách này không thể *quan sát* một cách trực tiếp, và chúng ta cũng không thấy hàm khép kín được *thực thi* trong đoạn mã đó. Chúng ta thấy rõ phạm vi từ vựng nhưng hàm khép kín vẫn còn là một cái bóng bí ẩn phía sau đoạn mã.

Vậy thì hãy xem xét đoạn mã đưa hàm khép kín ra ánh sáng hoàn toàn:

```js
function foo() {
	var a = 2;

	function bar() {
		console.log( a );
	}

	return bar;
}

var baz = foo();

baz(); // 2 -- Chà, hàm khép kín vừa mới được quan sát.
```

Hàm `bar()` có quyền truy cập phạm vi từ vựng vào phạm vi bên trong của `foo()`. Nhưng sau đó, chúng ta lấy chính bản thân `bar()` và truyền nó đi *như* một giá trị. Trong trường hợp này, chúng ta `return` chính đối tượng hàm mà `bar` tham chiếu đến.

Sau khi thực thi `foo()`, chúng ta gán giá trị mà nó trả về (hàm `bar()` bên trong của chúng ta) cho một biến có tên là `baz`, và sau đó chúng ta thực sự gọi `baz()`, điều này tất nhiên là đang gọi hàm `bar()` bên trong của chúng ta dưới một định danh tham chiếu khác.

`bar()` chắc chắn được thực thi. Nhưng trong trường hợp này, nó được thực thi *bên ngoài* phạm vi từ vựng đã được khai báo của nó.

Sau khi `foo()` thực thi xong, thông thường chúng ta sẽ mong đợi rằng toàn bộ phạm vi bên trong của `foo()` sẽ biến mất, bởi vì chúng ta biết rằng *Bộ máy* sử dụng một *Chương trình thu gom rác* đi theo và giải phóng bộ nhớ một khi nó không còn được sử dụng. Vì có vẻ như nội dung của `foo()` không còn được sử dụng nữa nên việc chúng được coi là *đã biến mất* có vẻ tự nhiên.

Nhưng "phép màu" của hàm khép kín không để điều này xảy ra. Phạm vi bên trong đó thực tế *vẫn* "đang được sử dụng", và do đó không biến mất. Ai đang sử dụng nó? **Chính hàm `bar()`**.

Nhờ vào vị trí được khai báo, `bar()` có một hàm khép kín phạm vi từ vựng trên phạm vi bên trong của `foo()`, điều này giữ cho phạm vi đó tồn tại để sau này `bar()` có thể tham chiếu đến bất kỳ lúc nào.

**`bar()` vẫn có một tham chiếu đến phạm vi đó, và tham chiếu đó được gọi là hàm khép kín.**

Vì vậy, vài micro giây sau, khi biến `baz` được gọi (gọi hàm bên trong mà ban đầu chúng ta đặt tên là `bar`), nó vẫn có *quyền truy cập* vào phạm vi từ vựng tại thời điểm viết mã, do đó nó có thể truy cập biến `a` đúng như chúng ta mong đợi.

Hàm đang được gọi ở một nơi hoàn toàn bên ngoài phạm vi từ vựng tại thời điểm viết mã của nó. **Hàm khép kín** cho phép hàm tiếp tục truy cập vào phạm vi từ vựng mà nó đã được định nghĩa tại thời điểm viết mã.

Tất nhiên, bất kỳ cách nào trong số các cách khác nhau mà hàm có thể được *truyền đi khắp nơi* dưới dạng giá trị, và thực sự được gọi ở các vị trí khác, đều là những ví dụ về việc quan sát/thực thi hàm khép kín.

```js
function foo() {
	var a = 2;

	function baz() {
		console.log( a ); // 2
	}

	bar( baz );
}

function bar(fn) {
	fn(); // thế là tôi đã chứng kiến hàm khép kín!
}
```

Chúng ta truyền hàm bên trong `baz` cho `bar`, và gọi hàm bên trong đó (bây giờ được đặt tên là `fn`), và khi chúng ta làm vậy, hàm khép kín của nó trên phạm vi bên trong của `foo()` được quan sát, bằng cách truy cập `a`.

Việc truyền các hàm đi vòng quanh này cũng có thể là gián tiếp.

```js
var fn;

function foo() {
	var a = 2;

	function baz() {
		console.log( a );
	}

	fn = baz; // gán `baz` cho biến toàn cục
}

function bar() {
	fn(); // thế là tôi đã chứng kiến hàm khép kín!
}

foo();

bar(); // 2
```

Bất kể chúng ta sử dụng phương tiện nào để *vận chuyển* một hàm bên trong ra khỏi phạm vi từ vựng của nó, nó sẽ duy trì một tham chiếu phạm vi đến nơi nó được khai báo ban đầu, và bất cứ nơi nào chúng ta thực thi nó, hàm khép kín đó sẽ được thực thi.

## Bây giờ tôi đã thấy

Các đoạn mã trước đây có phần học thuật và được xây dựng một cách giả tạo để minh họa việc *sử dụng hàm khép kín*. Nhưng tôi đã hứa với bạn một điều gì đó hơn là chỉ một món đồ chơi mới thú vị. Tôi đã hứa rằng hàm khép kín là thứ hiện hữu xung quanh bạn trong mã hiện có của bạn. Bây giờ chúng ta hãy *thấy* sự thật đó.

```js
function wait(message) {

	setTimeout( function timer(){
		console.log( message );
	}, 1000 );

}

wait( "Xin chào, hàm khép kín!" );
```

Chúng ta lấy một hàm bên trong (tên là `timer`) và truyền nó cho `setTimeout(..)`. Nhưng `timer` có một hàm khép kín phạm vi trên phạm vi của `wait(..)`, thực sự giữ và sử dụng một tham chiếu đến biến `message`.

Một nghìn mili giây sau khi chúng ta đã thực thi `wait(..)`, và phạm vi bên trong của nó lẽ ra đã biến mất từ lâu, hàm bên trong `timer` đó vẫn có hàm khép kín trên phạm vi đó.

Sâu bên trong bộ máy của *Engine*, tiện ích tích hợp `setTimeout(..)` có tham chiếu đến một tham số nào đó, có thể được gọi là `fn` hoặc `func` hoặc một cái gì đó tương tự. *Engine* đi đến để gọi hàm đó, tức là gọi hàm `timer` bên trong của chúng ta, và tham chiếu phạm vi từ vựng vẫn còn nguyên vẹn.

**Hàm khép kín.**

Hoặc, nếu bạn là người theo trường phái jQuery (hoặc bất kỳ framework JS nào, về vấn đề đó):

```js
function setupBot(name,selector) {
	$( selector ).click( function activator(){
		console.log( "Đang kích hoạt: " + name );
	} );
}

setupBot( "Bot Hàm khép kín 1", "#bot_1" );
setupBot( "Bot Hàm khép kín 2", "#bot_2" );
```

Tôi không chắc bạn viết loại mã nào, nhưng tôi thường xuyên viết mã chịu trách nhiệm điều khiển cả một đội quân drone toàn cầu gồm các bot hàm khép kín, vì vậy điều này hoàn toàn thực tế!

(Đùa chút thôi), về cơ bản *bất cứ khi nào* và *bất cứ nơi đâu* bạn coi các hàm (truy cập vào các phạm vi từ vựng tương ứng của chúng) như những giá trị hạng nhất và truyền chúng đi khắp nơi, bạn có khả năng sẽ thấy những hàm đó thực thi hàm khép kín. Dù đó là bộ đếm thời gian, trình xử lý sự kiện, yêu cầu Ajax, nhắn tin giữa các cửa sổ, web worker, hoặc bất kỳ tác vụ bất đồng bộ (hoặc đồng bộ!) nào khác, khi bạn truyền vào một *hàm gọi lại*, hãy sẵn sàng để sử dụng hàm khép kín!

**Lưu ý:** Chương 3 đã giới thiệu mẫu IIFE. Mặc dù người ta thường nói rằng IIFE (một mình nó) là một ví dụ về hàm khép kín được quan sát, tôi sẽ không hoàn toàn đồng ý, theo định nghĩa của chúng ta ở trên.

```js
var a = 2;

(function IIFE(){
	console.log( a );
})();
```

Đoạn mã này "hoạt động", nhưng nó không hoàn toàn là một sự quan sát về hàm khép kín. Tại sao? Bởi vì hàm (mà chúng ta đặt tên là "IIFE" ở đây) không được thực thi bên ngoài phạm vi từ vựng của nó. Nó vẫn được gọi ngay tại cùng phạm vi mà nó được khai báo (phạm vi bao ngoài/toàn cục cũng chứa `a`). `a` được tìm thấy thông qua tra cứu phạm vi từ vựng thông thường, không thực sự thông qua hàm khép kín.

Mặc dù về mặt kỹ thuật, hàm khép kín có thể xảy ra tại thời điểm khai báo, nó *không* thể quan sát được một cách nghiêm ngặt, và do đó, như người ta nói, *đó là một cái cây đổ trong rừng mà không có ai xung quanh để nghe thấy.*

Mặc dù một IIFE *tự nó* không phải là một ví dụ về hàm khép kín, nó hoàn toàn tạo ra phạm vi, và nó là một trong những công cụ phổ biến nhất mà chúng ta sử dụng để tạo ra phạm vi có thể được khép kín. Vì vậy, IIFE thực sự liên quan mật thiết đến hàm khép kín, ngay cả khi chúng không tự thực thi hàm khép kín.

Hãy đặt cuốn sách này xuống ngay bây giờ, độc giả thân mến. Tôi có một nhiệm vụ cho bạn. Hãy đi mở một số mã JavaScript gần đây của bạn. Tìm kiếm các hàm-dưới-dạng-giá-trị của bạn và xác định nơi bạn đã sử dụng hàm khép kín mà có thể trước đây bạn thậm chí không biết.

Tôi sẽ đợi.

Bây giờ... bạn đã thấy rồi chứ!

## Vòng lặp + Hàm khép kín

Ví dụ kinh điển phổ biến nhất được sử dụng để minh họa hàm khép kín liên quan đến vòng lặp for khiêm tốn.

```js
for (var i=1; i<=5; i++) {
	setTimeout( function timer(){
		console.log( i );
	}, i*1000 );
}
```

**Lưu ý:** Các công cụ kiểm tra mã (Linter) thường phàn nàn khi bạn đặt các hàm bên trong vòng lặp, bởi vì những sai lầm do không hiểu hàm khép kín là **rất phổ biến trong giới lập trình viên**. Ở đây, chúng tôi giải thích cách thực hiện đúng, tận dụng toàn bộ sức mạnh của hàm khép kín. Nhưng sự tinh tế đó thường bị các công cụ kiểm tra mã bỏ qua và chúng sẽ phàn nàn bất kể, cho rằng bạn *thực sự* không biết mình đang làm gì.

Tinh thần của đoạn mã này là chúng ta thường *mong đợi* hành vi sẽ là các số "1", "2", .. "5" sẽ được in ra, lần lượt từng số, mỗi giây một lần.

Trên thực tế, nếu bạn chạy mã này, bạn sẽ nhận được số "6" được in ra 5 lần, tại các khoảng thời gian một giây.

**Hả?**

Đầu tiên, hãy giải thích tại sao lại có số `6`. Điều kiện kết thúc của vòng lặp là khi `i` *không* `<=5`. Lần đầu tiên điều đó xảy ra là khi `i` bằng 6. Vì vậy, đầu ra phản ánh giá trị cuối cùng của `i` sau khi vòng lặp kết thúc.

Điều này thực sự có vẻ hiển nhiên khi nhìn lại lần thứ hai. Các hàm gọi lại của timeout đều chạy rất lâu sau khi vòng lặp hoàn thành. Thực tế, đối với các bộ đếm thời gian, ngay cả khi nó là `setTimeout(.., 0)` trong mỗi lần lặp, tất cả các hàm gọi lại đó vẫn sẽ chạy hoàn toàn sau khi vòng lặp hoàn thành, và do đó mỗi lần đều in ra `6`.

Nhưng có một câu hỏi sâu sắc hơn đang diễn ra ở đây. Điều gì đang *thiếu* trong mã của chúng ta để nó thực sự hoạt động như chúng ta đã ngụ ý về mặt ngữ nghĩa?

Điều còn thiếu là chúng ta đang cố gắng *ngụ ý* rằng mỗi lần lặp của vòng lặp sẽ "nắm bắt" một bản sao của riêng nó của `i`, tại thời điểm của lần lặp đó. Nhưng, theo cách hoạt động của phạm vi, tất cả 5 hàm đó, mặc dù được định nghĩa riêng biệt trong mỗi lần lặp, tất cả đều **khép kín trên cùng một phạm vi toàn cục được chia sẻ**, mà phạm vi này, trên thực tế, chỉ có một biến `i` trong đó.

Nói theo cách đó, *tất nhiên* tất cả các hàm đều chia sẻ một tham chiếu đến cùng một `i`. Có điều gì đó về cấu trúc vòng lặp có xu hướng làm chúng ta bối rối khi nghĩ rằng có một cái gì đó phức tạp hơn đang hoạt động. Không có. Không có sự khác biệt nào so với việc mỗi trong số 5 hàm gọi lại của timeout chỉ được khai báo lần lượt ngay sau nhau, mà không có vòng lặp nào cả.

Được rồi, vậy, trở lại câu hỏi cấp bách của chúng ta. Điều gì đang thiếu? Chúng ta cần thêm ~~chuông bò~~ phạm vi khép kín. Cụ thể, chúng ta cần một phạm vi khép kín mới cho mỗi lần lặp của vòng lặp.

Chúng ta đã học trong Chương 3 rằng IIFE tạo ra phạm vi bằng cách khai báo một hàm và thực thi nó ngay lập tức.

Hãy thử xem:

```js
for (var i=1; i<=5; i++) {
	(function(){
		setTimeout( function timer(){
			console.log( i );
		}, i*1000 );
	})();
}
```

Liệu có hoạt động không? Hãy thử đi. Một lần nữa, tôi sẽ đợi.

Tôi sẽ kết thúc sự hồi hộp cho bạn. **Không.** Nhưng tại sao? Chúng ta bây giờ rõ ràng có nhiều phạm vi từ vựng hơn. Mỗi hàm gọi lại của timeout thực sự đang khép kín trên phạm vi riêng của từng lần lặp được tạo ra tương ứng bởi mỗi IIFE.

Chỉ có một phạm vi để khép kín là không đủ **nếu phạm vi đó trống rỗng**. Hãy nhìn kỹ. IIFE của chúng ta chỉ là một phạm vi trống rỗng không làm gì cả. Nó cần *một cái gì đó* bên trong nó để hữu ích cho chúng ta.

Nó cần một biến của riêng mình, với một bản sao của giá trị `i` ở mỗi lần lặp.

```js
for (var i=1; i<=5; i++) {
	(function(){
		var j = i;
		setTimeout( function timer(){
			console.log( j );
		}, j*1000 );
	})();
}
```

**Tuyệt vời! Nó hoạt động rồi!**

Một biến thể nhỏ mà một số người ưa thích là:

```js
for (var i=1; i<=5; i++) {
	(function(j){
		setTimeout( function timer(){
			console.log( j );
		}, j*1000 );
	})( i );
}
```

Tất nhiên, vì các IIFE này chỉ là các hàm, chúng ta có thể truyền vào `i`, và chúng ta có thể gọi nó là `j` nếu chúng ta thích, hoặc thậm chí chúng ta có thể gọi nó là `i` một lần nữa. Dù bằng cách nào, mã bây giờ đã hoạt động.

Việc sử dụng một IIFE bên trong mỗi lần lặp đã tạo ra một phạm vi mới cho mỗi lần lặp, điều này đã cho các hàm gọi lại của timeout của chúng ta cơ hội để khép kín trên một phạm vi mới cho mỗi lần lặp, một phạm vi có một biến với giá trị đúng của từng lần lặp trong đó để chúng ta truy cập.

Vấn đề đã được giải quyết!

### Nhìn lại Phạm vi Khối

Hãy xem xét kỹ lưỡng phân tích của chúng ta về giải pháp trước đó. Chúng ta đã sử dụng một IIFE để tạo ra phạm vi mới cho mỗi lần lặp. Nói cách khác, chúng ta thực sự *cần* một **phạm vi khối** cho mỗi lần lặp. Chương 3 đã cho chúng ta thấy khai báo `let`, nó chiếm lấy một khối và khai báo một biến ngay tại đó trong khối.

**Nó về cơ bản biến một khối thành một phạm vi mà chúng ta có thể khép kín.** Vì vậy, đoạn mã tuyệt vời sau đây "cứ thế hoạt động":

```js
for (var i=1; i<=5; i++) {
	let j = i; // tuyệt, phạm vi khối cho hàm khép kín!
	setTimeout( function timer(){
		console.log( j );
	}, j*1000 );
}
```

*Nhưng, đó chưa phải là tất cả!* (bằng giọng Bob Barker tốt nhất của tôi). Có một hành vi đặc biệt được định nghĩa cho các khai báo `let` được sử dụng trong phần đầu của vòng lặp for. Hành vi này nói rằng biến sẽ được khai báo không chỉ một lần cho cả vòng lặp, **mà cho từng lượt lặp**. Và, một cách hữu ích, nó sẽ được khởi tạo ở mỗi lần lặp tiếp theo với giá trị từ cuối của lần lặp trước đó.

```js
for (let i=1; i<=5; i++) {
	setTimeout( function timer(){
		console.log( i );
	}, i*1000 );
}
```

Thật tuyệt vời phải không? Phạm vi khối và hàm khép kín làm việc tay trong tay, giải quyết tất cả các vấn đề của thế giới. Tôi không biết bạn thế nào, nhưng điều đó làm tôi trở thành một lập trình viên JavaScripter hạnh phúc.

## Khối chức năng (Modules)

Có những mẫu mã khác tận dụng sức mạnh của hàm khép kín nhưng bề ngoài không có vẻ là về các hàm gọi lại. Hãy xem xét mẫu mạnh mẽ nhất trong số chúng: *khối chức năng*.

```js
function foo() {
	var something = "cool";
	var another = [1, 2, 3];

	function doSomething() {
		console.log( something );
	}

	function doAnother() {
		console.log( another.join( " ! " ) );
	}
}
```

Như đoạn mã này hiện tại, không có hàm khép kín nào có thể quan sát được đang diễn ra. Chúng ta chỉ đơn giản có một số biến dữ liệu riêng tư `something` và `another`, và một vài hàm bên trong `doSomething()` và `doAnother()`, cả hai đều có phạm vi từ vựng (và do đó là hàm khép kín!) trên phạm vi bên trong của `foo()`.

Nhưng bây giờ hãy xem xét:

```js
function CoolModule() {
	var something = "cool";
	var another = [1, 2, 3];

	function doSomething() {
		console.log( something );
	}

	function doAnother() {
		console.log( another.join( " ! " ) );
	}

	return {
		doSomething: doSomething,
		doAnother: doAnother
	};
}

var foo = CoolModule();

foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

Đây là mẫu trong JavaScript mà chúng ta gọi là *khối chức năng*. Cách phổ biến nhất để triển khai mẫu khối chức năng thường được gọi là "Mẫu khối chức năng Lộ diện" (Revealing Module), và đó là biến thể chúng tôi trình bày ở đây.

Hãy xem xét một số điều về đoạn mã này.

Thứ nhất, `CoolModule()` chỉ là một hàm, nhưng nó *phải được gọi* để có một thể hiện của khối chức năng được tạo ra. Nếu không có sự thực thi của hàm bên ngoài, việc tạo ra phạm vi bên trong và các hàm khép kín sẽ không xảy ra.

Thứ hai, hàm `CoolModule()` trả về một đối tượng, được biểu thị bằng cú pháp đối tượng nguyên thủy `{ key: value, ... }`. Đối tượng chúng ta trả về có các tham chiếu đến các hàm bên trong của chúng ta, nhưng *không* phải là các biến dữ liệu bên trong của chúng ta. Chúng ta giữ chúng ẩn và riêng tư. Thật thích hợp khi nghĩ về giá trị trả về đối tượng này về cơ bản là một **API công khai cho khối chức năng của chúng ta**.

Giá trị trả về đối tượng này cuối cùng được gán cho biến bên ngoài `foo`, và sau đó chúng ta có thể truy cập các phương thức thuộc tính đó trên API, như `foo.doSomething()`.

**Lưu ý:** Không bắt buộc chúng ta phải trả về một đối tượng thực tế (nguyên thủy) từ khối chức năng của mình. Chúng ta có thể chỉ trả về trực tiếp một hàm bên trong. jQuery thực sự là một ví dụ điển hình về điều này. Các định danh `jQuery` và `$` là API công khai cho "khối chức năng" jQuery, nhưng chúng, bản thân chúng, chỉ là một hàm (mà bản thân nó có thể có các thuộc tính, vì tất cả các hàm đều là đối tượng).

Các hàm `doSomething()` và `doAnother()` có hàm khép kín trên phạm vi bên trong của "thể hiện" khối chức năng (đạt được bằng cách thực sự gọi `CoolModule()`). Khi chúng ta vận chuyển các hàm đó ra bên ngoài phạm vi từ vựng, thông qua các tham chiếu thuộc tính trên đối tượng chúng ta trả về, chúng ta đã thiết lập một điều kiện mà qua đó hàm khép kín có thể được quan sát và thực thi.

Nói một cách đơn giản hơn, có hai "yêu cầu" để mẫu khối chức năng được thực thi:

1.  Phải có một hàm bao ngoài, và nó phải được gọi ít nhất một lần (mỗi lần tạo ra một thể hiện khối chức năng mới).

2.  Hàm bao ngoài phải trả về ít nhất một hàm bên trong, để hàm bên trong này có hàm khép kín trên phạm vi riêng tư, và có thể truy cập và/hoặc sửa đổi trạng thái riêng tư đó.

Một đối tượng chỉ có một thuộc tính hàm trên nó thì không *thực sự* là một khối chức năng. Một đối tượng được trả về từ một lời gọi hàm chỉ có các thuộc tính dữ liệu trên nó và không có hàm khép kín nào thì không *thực sự* là một khối chức năng, theo nghĩa có thể quan sát được.

Đoạn mã trên cho thấy một hàm tạo khối chức năng độc lập có tên `CoolModule()` có thể được gọi bao nhiêu lần tùy ý, mỗi lần tạo ra một thể hiện khối chức năng mới. Một biến thể nhỏ của mẫu này là khi bạn chỉ quan tâm đến việc có một thể hiện duy nhất, một dạng "singleton":

```js
var foo = (function CoolModule() {
	var something = "cool";
	var another = [1, 2, 3];

	function doSomething() {
		console.log( something );
	}

	function doAnother() {
		console.log( another.join( " ! " ) );
	}

	return {
		doSomething: doSomething,
		doAnother: doAnother
	};
})();

foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

Ở đây, chúng ta đã biến hàm khối chức năng của mình thành một IIFE (xem Chương 3), và chúng ta *ngay lập tức* gọi nó và gán giá trị trả về của nó trực tiếp cho định danh thể hiện khối chức năng duy nhất của chúng ta là `foo`.

Các khối chức năng chỉ là các hàm, vì vậy chúng có thể nhận tham số:

```js
function CoolModule(id) {
	function identify() {
		console.log( id );
	}

	return {
		identify: identify
	};
}

var foo1 = CoolModule( "foo 1" );
var foo2 = CoolModule( "foo 2" );

foo1.identify(); // "foo 1"
foo2.identify(); // "foo 2"
```

Một biến thể nhỏ nhưng mạnh mẽ khác của mẫu khối chức năng là đặt tên cho đối tượng bạn đang trả về làm API công khai của mình:

```js
var foo = (function CoolModule(id) {
	function change() {
		// sửa đổi API công khai
		publicAPI.identify = identify2;
	}

	function identify1() {
		console.log( id );
	}

	function identify2() {
		console.log( id.toUpperCase() );
	}

	var publicAPI = {
		change: change,
		identify: identify1
	};

	return publicAPI;
})( "khối chức năng foo" );

foo.identify(); // khối chức năng foo
foo.change();
foo.identify(); // KHỐI CHỨC NĂNG FOO
```

Bằng cách giữ lại một tham chiếu bên trong đến đối tượng API công khai bên trong thể hiện khối chức năng của bạn, bạn có thể sửa đổi thể hiện khối chức năng đó **từ bên trong**, bao gồm thêm và xóa các phương thức, thuộc tính, *và* thay đổi giá trị của chúng.

### Các khối chức năng hiện đại

Các trình tải/quản lý phụ thuộc khối chức năng khác nhau về cơ bản gói gọn mẫu định nghĩa khối chức năng này vào một API thân thiện. Thay vì xem xét bất kỳ thư viện cụ thể nào, hãy để tôi trình bày một bằng chứng khái niệm *rất đơn giản* **chỉ cho mục đích minh họa**:

```js
var MyModules = (function Manager() {
	var modules = {};

	function define(name, deps, impl) {
		for (var i=0; i<deps.length; i++) {
			deps[i] = modules[deps[i]];
		}
		modules[name] = impl.apply( impl, deps );
	}

	function get(name) {
		return modules[name];
	}

	return {
		define: define,
		get: get
	};
})();
```

Phần quan trọng của đoạn mã này là `modules[name] = impl.apply(impl, deps)`. Điều này đang gọi hàm bao bọc định nghĩa cho một khối chức năng (truyền vào bất kỳ phụ thuộc nào), và lưu trữ giá trị trả về, API của khối chức năng, vào một danh sách nội bộ các khối chức năng được theo dõi theo tên.

Và đây là cách tôi có thể sử dụng nó để định nghĩa một số khối chức năng:

```js
MyModules.define( "bar", [], function(){
	function hello(who) {
		return "Xin giới thiệu: " + who;
	}

	return {
		hello: hello
	};
} );

MyModules.define( "foo", ["bar"], function(bar){
	var hungry = "hippo";

	function awesome() {
		console.log( bar.hello( hungry ).toUpperCase() );
	}

	return {
		awesome: awesome
	};
} );

var bar = MyModules.get( "bar" );
var foo = MyModules.get( "foo" );

console.log(
	bar.hello( "hippo" )
); // Xin giới thiệu: hippo

foo.awesome(); // XIN GIỚI THIỆU: HIPPO
```

Cả hai khối chức năng "foo" và "bar" đều được định nghĩa bằng một hàm trả về một API công khai. "foo" thậm chí còn nhận thể hiện của "bar" như một tham số phụ thuộc, và có thể sử dụng nó tương ứng.

Hãy dành chút thời gian xem xét các đoạn mã này để hiểu đầy đủ sức mạnh của hàm khép kín được đưa vào sử dụng cho các mục đích tốt đẹp của chúng ta. Điểm mấu chốt là không thực sự có bất kỳ "phép màu" cụ thể nào đối với các trình quản lý khối chức năng. Chúng đáp ứng cả hai đặc điểm của mẫu khối chức năng mà tôi đã liệt kê ở trên: gọi một hàm bao bọc định nghĩa, và giữ giá trị trả về của nó làm API cho khối chức năng đó.

Nói cách khác, khối chức năng vẫn chỉ là khối chức năng, ngay cả khi bạn đặt một công cụ bao bọc thân thiện lên trên chúng.

### Các khối chức năng Tương lai

ES6 bổ sung hỗ trợ cú pháp hạng nhất cho khái niệm khối chức năng. Khi được tải qua hệ thống khối chức năng, ES6 coi một tệp như một khối chức năng riêng biệt. Mỗi khối chức năng có thể vừa nhập các khối chức năng khác hoặc các thành viên API cụ thể, vừa xuất các thành viên API công khai của riêng mình.

**Lưu ý:** Các khối chức năng dựa trên hàm không phải là một mẫu được nhận dạng tĩnh (thứ mà trình biên dịch biết đến), vì vậy ngữ nghĩa API của chúng không được xem xét cho đến khi chạy. Tức là, bạn thực sự có thể sửa đổi API của một khối chức năng trong thời gian chạy (xem thảo luận về `publicAPI` trước đó).

Ngược lại, API của khối chức năng ES6 là tĩnh (các API không thay đổi trong thời gian chạy). Vì trình biên dịch biết *điều đó*, nó có thể (và có!) kiểm tra trong quá trình (tải tệp và) biên dịch rằng một tham chiếu đến một thành viên của API của một khối chức năng được nhập vào *thực sự tồn tại*. Nếu tham chiếu API không tồn tại, trình biên dịch sẽ ném ra một lỗi "sớm" tại thời điểm biên dịch, thay vì chờ đợi sự phân giải động truyền thống trong thời gian chạy (và các lỗi, nếu có).

Các khối chức năng ES6 **không** có định dạng "nội tuyến", chúng phải được định nghĩa trong các tệp riêng biệt (một tệp cho mỗi khối chức năng). Các trình duyệt/bộ máy có một "trình tải khối chức năng" mặc định (có thể bị ghi đè, nhưng điều đó vượt xa cuộc thảo luận của chúng ta ở đây) mà nó sẽ tải đồng bộ một tệp khối chức năng khi nó được nhập.

Xem xét:

**bar.js**
```js
function hello(who) {
	return "Xin giới thiệu: " + who;
}

export hello;
```

**foo.js**
```js
// chỉ nhập `hello()` từ khối chức năng "bar"
import hello from "bar";

var hungry = "hippo";

function awesome() {
	console.log(
		hello( hungry ).toUpperCase()
	);
}

export awesome;
```

```js
// nhập toàn bộ khối chức năng "foo" và "bar"
module foo from "foo";
module bar from "bar";

console.log(
	bar.hello( "rhino" )
); // Xin giới thiệu: rhino

foo.awesome(); // XIN GIỚI THIỆU: HIPPO
```

**Lưu ý:** Các tệp riêng biệt **"foo.js"** và **"bar.js"** sẽ cần được tạo ra, với nội dung như được hiển thị trong hai đoạn mã đầu tiên, tương ứng. Sau đó, chương trình của bạn sẽ tải/nhập các khối chức năng đó để sử dụng chúng, như được hiển thị trong đoạn mã thứ ba.

`import` nhập một hoặc nhiều thành viên từ API của một khối chức năng vào phạm vi hiện tại, mỗi thành viên vào một biến được ràng buộc (`hello` trong trường hợp của chúng ta). `module` nhập toàn bộ API của một khối chức năng vào một biến được ràng buộc (`foo`, `bar` trong trường hợp của chúng ta). `export` xuất một định danh (biến, hàm) ra API công khai cho khối chức năng hiện tại. Các toán tử này có thể được sử dụng nhiều lần trong định nghĩa của một khối chức năng nếu cần thiết.

Nội dung bên trong *tệp khối chức năng* được coi như được bao bọc trong một phạm vi khép kín, giống như với các khối chức năng dựa trên hàm đã thấy trước đó.

## Tổng kết (Nói ngắn gọn)

Đối với người chưa khai sáng, hàm khép kín có vẻ như một thế giới huyền bí được tách biệt bên trong JavaScript mà chỉ có vài linh hồn dũng cảm nhất mới có thể vươn tới. Nhưng thực ra nó chỉ là một thực tế tiêu chuẩn và gần như hiển nhiên về cách chúng ta viết mã trong một môi trường có phạm vi từ vựng, nơi các hàm là giá trị và có thể được truyền đi theo ý muốn.

**Hàm khép kín là khi một hàm có thể ghi nhớ và truy cập vào phạm vi từ vựng của nó ngay cả khi nó được gọi bên ngoài phạm vi từ vựng của mình.**

Hàm khép kín có thể làm chúng ta vấp ngã, chẳng hạn như với các vòng lặp, nếu chúng ta không cẩn thận nhận ra chúng và cách chúng hoạt động. Nhưng chúng cũng là một công cụ vô cùng mạnh mẽ, cho phép các mẫu như *khối chức năng* trong các dạng khác nhau của chúng.

Khối chức năng đòi hỏi hai đặc điểm chính: 1) một hàm bao bọc bên ngoài được gọi, để tạo ra phạm vi bao quanh 2) giá trị trả về của hàm bao bọc phải bao gồm tham chiếu đến ít nhất một hàm bên trong mà sau đó có hàm khép kín trên phạm vi riêng tư bên trong của hàm bao bọc.

Bây giờ chúng ta có thể thấy các hàm khép kín ở khắp mọi nơi trong mã hiện có của mình, và chúng ta có khả năng nhận ra và tận dụng chúng vì lợi ích của chính mình
