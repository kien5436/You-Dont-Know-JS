# Bạn không hiểu JS: Phạm vi & Hàm khép kín
# Chương 5: Sự bao đóng của Phạm vi

Hy vọng rằng đến thời điểm này, chúng ta đã có một sự am tường vững chắc và lành mạnh về cách phạm vi hoạt động.

Giờ đây, chúng ta sẽ chuyển sự chú ý sang một phần vô cùng quan trọng nhưng lại luôn khó nắm bắt, *gần như một huyền thoại* của ngôn ngữ: **cơ chế bao đóng (closure)**. Nếu bạn đã theo dõi cuộc thảo luận của chúng ta về phạm vi từ vựng cho đến nay, thành quả bạn nhận được là khái niệm này hóa ra lại chẳng có gì ghê gớm, gần như là một điều hiển nhiên. *Có một người đằng sau tấm rèm của vị pháp sư, và chúng ta sắp diện kiến người đó*. Không, tên ông ấy không phải là Crockford!

Tuy nhiên, nếu bạn vẫn còn những câu hỏi dai dẳng về phạm vi từ vựng, đây sẽ là thời điểm tốt để quay lại và xem lại chương 2 trước khi tiếp tục.

## Khai sáng

Đối với những người đã có ít nhiều kinh nghiệm về JavaScript nhưng có lẽ chưa bao giờ hiểu trọn vẹn khái niệm về sự bao đóng, việc *thấu hiểu nó* có thể giống như một cảnh giới niết bàn đặc biệt mà người ta phải phấn đấu và hy sinh để đạt được.

Tôi nhớ lại nhiều năm về trước, khi tôi đã nắm vững JavaScript nhưng lại không biết cơ chế bao đóng là gì. Gợi ý về việc có một *khía cạnh khác* của ngôn ngữ, một khía cạnh hứa hẹn còn nhiều khả năng hơn cả những gì tôi đã sở hữu, đã trêu ngươi và thách thức tôi. Tôi nhớ mình đã đọc qua mã nguồn của các bộ khung đời đầu để cố gắng hiểu cách nó thực sự hoạt động. Tôi nhớ lần đầu tiên một thứ gì đó tựa như "mô hình khối chức năng" bắt đầu nhen nhóm trong tâm trí tôi. Tôi nhớ những khoảnh khắc *a ha!* một cách sống động.

Điều mà tôi không biết lúc đó, điều mà tôi đã mất nhiều năm để hiểu, và điều mà tôi hy vọng sẽ truyền đạt cho bạn ngay bây giờ, chính là bí mật này: **cơ chế bảo đóng tồn tại xung quanh bạn trong JavaScript, bạn chỉ cần nhận ra và tiếp thu nó.** Cơ chế bao đóng không phải là một công cụ đặc biệt bạn phải lựa chọn sử dụng, không phải là thứ bạn phải học cú pháp và các mô hình mới. Không, cơ chế bao đóng thậm chí không phải là một vũ khí mà bạn phải học cách sử dụng và làm chủ như Luke luyện tập Thần Lực.

Cơ chế bao đóng xảy ra như một kết quả của việc viết mã dựa trên phạm vi từ vựng. Chúng cứ thế xảy ra. Bạn thậm chí không thực sự phải cố tình tạo ra chúng để tận dụng lợi thế của chúng. Chúng được tạo ra và sử dụng trên khắp mã của bạn. Điều bạn đang *thiếu* là một bối cảnh tư duy đúng đắn để nhận ra, tiếp thu, và tận dụng cơ chế này theo ý muốn của riêng bạn.

Giây phút giác ngộ sẽ là: **ồ, cơ chế này vốn đã tồn tại khắp nơi trong mã của mình, cuối cùng mình cũng *thấy* được chúng rồi.** Việc thấu hiểu cơ chế bao đóng giống như khi Neo lần đầu nhìn thấu Ma Trận.

## Đi vào cốt lõi

Được rồi, đủ những lời nói quá và những tham chiếu phim ảnh trơ trẽn rồi.

Đây là một định nghĩa thẳng thắn về những gì bạn cần biết để hiểu và nhận ra cơ chế bao đóng:

> Cơ chế bao đóng là khi một hàm có khả năng ghi nhớ và truy cập vào phạm vi từ vựng của nó ngay cả khi hàm đó được thực thi bên ngoài phạm vi từ vựng ấy.

Hãy đi vào một vài đoạn mã để minh họa cho định nghĩa đó.

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

Đoạn mã này trông khá quen thuộc từ các cuộc thảo luận của chúng ta về Phạm vi Lồng nhau. Hàm `bar()` có *quyền truy cập* vào biến `a` trong phạm vi bao ngoài nhờ vào các quy tắc tra cứu phạm vi từ vựng (trong trường hợp này là một tra cứu tham chiếu RHS).

Đây có phải là "cơ chế bao đóng"?

Chà, về mặt kỹ thuật... *có lẽ vậy*. Nhưng theo định nghĩa những-gì-bạn-cần-biết của chúng ta ở trên thì... *không hẳn*. Tôi nghĩ cách giải thích chính xác nhất cho việc `bar()` tham chiếu đến `a` là thông qua các quy tắc tra cứu phạm vi từ vựng, và những quy tắc đó *chỉ* là một **phần** (quan trọng!) của cơ chế bao đóng.

Từ góc độ hoàn toàn học thuật, điều được nói về đoạn mã trên là hàm `bar()` có một *cơ chế bao đóng* trên phạm vi của `foo()` (và thực sự là cả trên phần còn lại của các phạm vi mà nó có quyền truy cập, chẳng hạn như phạm vi toàn cục trong trường hợp của chúng ta). Nói một cách khác, `bar()` khép kín trên phạm vi của `foo()`. Tại sao? Bởi vì `bar()` xuất hiện lồng bên trong `foo()`. Rõ ràng và đơn giản.

Nhưng sự bao đóng được định nghĩa theo cách này không thể *quan sát* trực tiếp, chúng ta cũng không thấy nó được *thực thi* trong đoạn mã đó. Chúng ta thấy rõ phạm vi từ vựng nhưng cơ chế bao đóng vẫn còn là một cái bóng bí ẩn di chuyển đằng sau đoạn mã.

Vậy hãy xem xét một đoạn mã đưa cơ chế bao đóng ra ánh sáng hoàn toàn:

```js
function foo() {
	var a = 2;

	function bar() {
		console.log( a );
	}

	return bar;
}

var baz = foo();

baz(); // 2 -- Ồ, cơ chế bao đóng vừa được thể hiện, bạn ạ.
```

Hàm `bar()` có quyền truy cập phạm vi từ vựng vào phạm vi nội bộ của `foo()`. Nhưng sau đó, chúng ta lấy chính hàm `bar()` và truyền nó đi *như* một giá trị. Trong trường hợp này, chúng ta `return` chính đối tượng hàm mà `bar` đang tham chiếu.

Sau khi thực thi `foo()`, chúng ta gán giá trị mà nó trả về (hàm `bar()` bên trong) cho một biến có tên là `baz`, và sau đó chúng ta thực sự gọi `baz()`, điều này dĩ nhiên là đang gọi hàm `bar()` bên trong của chúng ta dưới một tham chiếu định danh khác.

`bar()` chắc chắn được thực thi. Nhưng trong trường hợp này, nó được thực thi *bên ngoài* phạm vi từ vựng đã được khai báo của nó.

Sau khi `foo()` thực thi xong, thông thường chúng ta sẽ mong đợi rằng toàn bộ phạm vi nội bộ của `foo()` sẽ biến mất, vì chúng ta biết rằng *Bộ máy* sử dụng một *Chương trình thu gom rác* đến và giải phóng bộ nhớ khi nó không còn được sử dụng. Bởi vì nội dung của `foo()` có vẻ như không còn được sử dụng nữa, việc chúng được coi là *biến mất* có vẻ tự nhiên.

Nhưng sự "kỳ diệu" của cơ chế bao đóng không để điều này xảy ra. Phạm vi nội bộ đó trên thực tế *vẫn* "đang được sử dụng", và do đó không biến mất. Ai đang sử dụng nó? **Chính hàm `bar()`**.

Nhờ vào nơi nó được khai báo, `bar()` có một liên kết phạm vi từ vựng trên phạm vi nội bộ của `foo()`, điều này giữ cho phạm vi đó tồn tại để `bar()` có thể tham chiếu vào bất kỳ lúc nào về sau.

**`bar()` vẫn giữ một tham chiếu đến phạm vi đó, sự tham chiếu ấy được gọi là cơ chế bao đóng.**

Vậy là sau vài micro giây, khi biến `baz` được gọi (gọi hàm bên trong mà ban đầu chúng ta đặt tên là `bar`), nó vẫn có *quyền truy cập* vào phạm vi từ vựng tại thời điểm viết mã, vì vậy nó có thể truy cập biến `a` đúng như chúng ta mong đợi.

Hàm đang được gọi ở bên ngoài phạm vi từ vựng tại thời điểm viết mã của nó. **Cơ chế bao đóng** cho phép hàm tiếp tục truy cập phạm vi từ vựng mà nó đã được định nghĩa tại thời điểm viết mã.

Tất nhiên, trong vô vàn cách khác nhau mà các hàm có thể được *truyền đi khắp nơi* dưới dạng giá trị và thực sự được gọi ở các vị trí khác, dù theo cách nào thì chúng đều là những ví dụ về việc quan sát/thực thi cơ chế bao đóng.

```js
function foo() {
	var a = 2;

	function baz() {
		console.log( a ); // 2
	}

	bar( baz );
}

function bar(fn) {
	fn(); // xem này mẹ, con thấy cơ chế bao đóng rồi!
}
```

Chúng ta truyền hàm `baz` bên trong cho `bar` và gọi hàm nội bộ đó (bây giờ được đặt tên là `fn`), và khi làm vậy, sự bao đóng của nó trên phạm vi nội bộ của `foo()` được quan sát thấy bằng cách truy cập `a`.

Việc truyền các hàm này đi cũng có thể thực hiện gián tiếp.

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
	fn(); // xem này mẹ, con thấy cơ chế bao đóng rồi!
}

foo();

bar(); // 2
```

Bất kể chúng ta sử dụng phương tiện nào để *vận chuyển* một hàm nội bộ ra ngoài phạm vi từ vựng của nó, nó sẽ duy trì một tham chiếu phạm vi đến nơi nó được khai báo ban đầu, và bất cứ nơi nào chúng ta thực thi nó, sự bao đóng đó sẽ được thể hiện.

## Giờ tôi đã thấy

Các đoạn mã trước đây có phần học thuật và được xây dựng một cách giả tạo để minh họa việc *sử dụng cơ chế bao đóng*. Nhưng tôi đã cam kết với bạn một thứ gì đó hơn là một món đồ chơi mới thú vị. Tôi đã cam kết rằng cơ chế đó là thứ tồn tại xung quanh trong mã hiện có của bạn. Bây giờ chúng ta hãy *xem* sự thật đó.

```js
function wait(message) {

	setTimeout( function timer(){
		console.log( message );
	}, 1000 );

}

wait( "Xin chào, cơ chế bao đóng!" );
```

Chúng ta lấy một hàm bên trong (tên là `timer`) và truyền nó cho `setTimeout(..)`. Nhưng `timer` có một sự bao đóng phạm vi trên phạm vi của `wait(..)`, thực sự giữ và sử dụng một tham chiếu đến biến `message`.

Một nghìn mili giây sau khi chúng ta đã thực thi `wait(..)`, và phạm vi nội bộ của nó đáng lẽ đã biến mất từ lâu, hàm `timer` bên trong đó vẫn có sự bao đóng trên phạm vi ấy.

Sâu trong lòng của *Bộ máy*, tiện ích tích hợp sẵn `setTimeout(..)` có tham chiếu đến một tham số nào đó, có thể được gọi là `fn` hoặc `func` hoặc một cái gì đó tương tự. *Bộ máy* đi đến để gọi hàm đó, dẫn tới việc gọi hàm `timer` bên trong của chúng ta, và tham chiếu phạm vi từ vựng vẫn còn nguyên vẹn.

**Đó là sự bao đóng.**

Hoặc, nếu bạn là người theo trường phái jQuery (hoặc bất kỳ bộ khung JS nào cũng vẫn đúng):

```js
function setupBot(name,selector) {
	$( selector ).click( function activator(){
		console.log( "Đang kích hoạt: " + name );
	} );
}

setupBot( "Bot Bao Đóng 1", "#bot_1" );
setupBot( "Bot Bao Đóng 2", "#bot_2" );
```

Tôi không chắc bạn viết loại mã nào, nhưng tôi thường xuyên viết mã chịu trách nhiệm điều khiển tất cả đội quân máy bay không người lái toàn cầu gồm các máy bao đóng, vì vậy điều này hoàn toàn thực tế!

Đùa một chút thôi, về cơ bản *bất cứ khi nào* và *bất cứ nơi đâu* bạn coi các hàm (truy cập vào các phạm vi từ vựng tương ứng của chúng) như những giá trị hạng nhất và truyền chúng đi, bạn có khả năng sẽ thấy những hàm đó thực thi cơ chế bao đóng. Dù đó là bộ đếm thời gian, hàm xử lý sự kiện, yêu cầu Ajax, truyền thông tin giữa các cửa sổ, web worker, hay bất kỳ tác vụ bất đồng bộ (hoặc đồng bộ!) nào khác, khi bạn truyền vào một *hàm gọi lại*, hãy sẵn sàng tung hoành với cơ chế bao đóng!

**Ghi chú:** Chương 3 đã giới thiệu mô hình IIFE. Mặc dù người ta thường nói rằng IIFE (đứng riêng) là một ví dụ về sự bao đóng quan sát được, tôi sẽ không hoàn toàn đồng ý, theo định nghĩa của chúng ta ở trên.

```js
var a = 2;

(function IIFE(){
	console.log( a );
})();
```

Đoạn mã này "hoạt động", nhưng nó không hoàn toàn là một sự quan sát về cơ chế bao đóng. Tại sao? Bởi vì hàm (mà chúng ta đặt tên là "IIFE") không được thực thi bên ngoài phạm vi từ vựng của nó. Nó vẫn được gọi ngay tại cùng một phạm vi mà nó được khai báo (phạm vi bao ngoài/toàn cục cũng chứa `a`). `a` được tìm thấy thông qua tra cứu phạm vi từ vựng thông thường, không thực sự thông qua cơ chế bao đóng.

Mặc dù về mặt kỹ thuật, sự bao đóng có thể xảy ra tại thời điểm khai báo, nó *không* thể quan sát được một cách chặt chẽ, và do đó, như người ta nói, *một cái cây đổ trong rừng mà không có ai xung quanh để nghe thấy.*

Mặc dù một IIFE *tự nó* không phải là một ví dụ về cơ chế bao đóng, nó hoàn toàn tạo ra phạm vi, và nó là một trong những công cụ phổ biến nhất mà chúng ta sử dụng để tạo ra phạm vi có thể được bao đóng. Vì vậy, IIFE thực sự có liên quan mật thiết đến cơ chế bao đóng, ngay cả khi chúng không tự thực thi cơ chế đó.

Giờ thì hãy đặt cuốn sách này xuống, bạn đọc thân mến. Tôi có một nhiệm vụ cho bạn. Hãy mở một số mã JavaScript gần đây của bạn. Tìm kiếm các hàm-như-giá-trị và xác định nơi bạn đã sử dụng cơ chế bao đóng mà có thể trước đây bạn thậm chí không biết.

Tôi sẽ đợi.

Giờ thì... bạn thấy rồi chứ!

## Vòng lặp + Cơ chế bao đóng

Ví dụ kinh điển phổ biến nhất được sử dụng để minh họa cơ chế bao đóng liên quan đến vòng lặp for khiêm tốn.

```js
for (var i=1; i<=5; i++) {
	setTimeout( function timer(){
		console.log( i );
	}, i*1000 );
}
```

**Ghi chú:** Các công cụ kiểm tra mã thường phàn nàn khi bạn đặt các hàm bên trong vòng lặp vì những sai lầm do không hiểu về cơ chế bao đóng **rất phổ biến trong giới lập trình viên**. Ở đây, chúng tôi giải thích cách thực hiện đúng, tận dụng toàn bộ sức mạnh của cơ chế này. Nhưng các công cụ kiểm tra mã thường bỏ qua sự tinh tế đó và bất kể thế nào, chúng cứ cho rằng bạn *thực sự* không biết mình đang làm gì.

Tinh thần của đoạn mã này là chúng ta *mong đợi* hành vi sẽ là các số "1", "2", .. "5" sẽ được in ra, lần lượt từng số một mỗi giây.

Trên thực tế, nếu chạy mã này, bạn sẽ nhận được số "6" in ra 5 lần, tại các khoảng thời gian cách một giây.

**Hả?**

Trước hết, hãy giải thích tại sao lại có số `6`. Điều kiện kết thúc của vòng lặp là khi `i` *không* `<=5`. Lần đầu tiên điều đó xảy ra là khi `i` bằng 6. Vì vậy, đầu ra đang phản ánh giá trị cuối cùng của `i` sau khi vòng lặp kết thúc.

Điều này thực ra có vẻ hiển nhiên khi nhìn lại lần thứ hai. Các hàm gọi lại của hàm hết thời gian đều chạy bình thường sau khi vòng lặp hoàn thành. Thực tế, đối với các bộ đếm thời gian, ngay cả khi đó là `setTimeout(.., 0)` trong mỗi lần lặp, tất cả các hàm gọi lại đó vẫn sẽ chạy một cách nghiêm ngặt sau khi vòng lặp hoàn thành, và do đó sẽ in ra `6` mỗi lần.

Nhưng có một câu hỏi sâu sắc hơn đang diễn ra ở đây. Mã của chúng ta đang *thiếu* điều gì để nó thực sự hoạt động như chúng ta đã ngụ ý về mặt ngữ nghĩa?

Điều còn thiếu là chúng ta đang cố gắng *ngụ ý* rằng mỗi lần lặp "chụp lại" một bản sao của `i` của riêng nó, tại thời điểm lặp. Nhưng theo cách hoạt động của phạm vi, tất cả 5 hàm đó, mặc dù được định nghĩa riêng biệt trong mỗi lần lặp, tất cả đều **bao đóng trên cùng một phạm vi toàn cục dùng chung**, mà thực tế, chỉ có một biến `i` trong đó.

Nói theo cách đó, *dĩ nhiên* tất cả các hàm đều chia sẻ tham chiếu đến cùng một `i`. Có điều gì đó về cấu trúc vòng lặp khiến chúng ta bối rối khi nghĩ rằng có một thứ gì đó phức tạp hơn đang hoạt động. Không hề. Không có sự khác biệt nào nếu mỗi lệnh trong số 5 lệnh gọi lại thời gian chờ được khai báo liên tiếp mà không có vòng lặp nào cả.

Được rồi, hãy trở lại câu hỏi cấp bách của chúng ta. Điều gì còn thiếu? Chúng ta cần thêm phạm vi được bao đóng. Cụ thể, chúng ta cần một phạm vi được bao đóng mới cho mỗi vòng lặp.

Chúng ta đã học trong chương 3 rằng IIFE tạo ra phạm vi bằng cách khai báo một hàm và thực thi nó ngay lập tức.

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

Cách đó có hoạt động không? Cứ thử đi. Một lần nữa, tôi sẽ đợi.

Tôi sẽ kết thúc sự hồi hộp cho bạn. **Không.** Nhưng tại sao? Bây giờ chúng ta rõ ràng có thêm phạm vi từ vựng. Mỗi lệnh gọi lại của hàm thời gian chờ thực sự đang bao đóng trên phạm vi riêng của từng vòng lặp được tạo ra bởi mỗi IIFE.

Chỉ có một phạm vi để bao đóng là chưa đủ **nếu phạm vi đó trống rỗng**. Hãy nhìn kỹ. IIFE của chúng ta chỉ là một phạm vi trống không làm gì cả. Nó cần *thứ gì đó* bên trong để có ích cho chúng ta.

Nó cần một biến của riêng nó, với một bản sao của giá trị `i` tại mỗi lần lặp.

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

**Ơ-rê-ca! Nó hoạt động rồi!**

Một biến thể nhỏ mà một số người thích hơn là:

```js
for (var i=1; i<=5; i++) {
	(function(j){
		setTimeout( function timer(){
			console.log( j );
		}, j*1000 );
	})( i );
}
```

Tất nhiên, vì các IIFE này chỉ là các hàm, chúng ta có thể truyền `i` vào và gọi nó là `j` nếu chúng ta thích, thậm chí chúng ta có thể vẫn gọi nó là `i`. Dù bằng cách nào, bây giờ mã đã hoạt động.

Việc sử dụng một IIFE bên trong mỗi vòng lặp đã tạo ra một phạm vi mới cho mỗi lần lặp, điều này đã cho các hàm gọi lại của thời gian chờ cơ hội để thực hiện bao đóng trên một phạm vi mới cho mỗi lần lặp, một phạm vi có một biến với giá trị đúng cho từng lần lặp để chúng ta truy cập.

Vấn đề đã được giải quyết!

### Thăm lại Phạm vi khối

Hãy xem xét kỹ phân tích của chúng ta về giải pháp trước đó. Chúng ta đã sử dụng một IIFE để tạo ra phạm vi mới cho mỗi lần lặp. Nói cách khác, chúng ta thực sự *cần* một **phạm vi khối** cho mỗi lần lặp. Chương 3 đã cho chúng ta thấy khai báo `let` có thể chiếm lấy một khối và khai báo một biến ngay tại đó.

**Về cơ bản, nó biến một khối lệnh thành một phạm vi mà chúng ta có thể thực hiện bao đóng.** Vì vậy, đoạn mã tuyệt vời sau đây "cứ thế hoạt động":

```js
for (var i=1; i<=5; i++) {
	let j = i; // tuyệt vời, phạm vi khối cho sự bao đóng!
	setTimeout( function timer(){
		console.log( j );
	}, j*1000 );
}
```

*Nhưng đó chưa phải là tất cả!* (bằng giọng Bob Barker tốt nhất của tôi). Có một hành vi đặc biệt được định nghĩa cho các khai báo `let` được sử dụng trong phần đầu của vòng lặp for. Hành vi này nói rằng biến sẽ được khai báo không chỉ một lần trong vòng lặp, **mà là mỗi lần lặp**. Và, một cách hữu ích, nó sẽ được khởi tạo ở mỗi lần lặp tiếp theo với giá trị từ cuối của lần lặp trước.

```js
for (let i=1; i<=5; i++) {
	setTimeout( function timer(){
		console.log( i );
	}, i*1000 );
}
```

Thật tuyệt vời phải không? Phạm vi khối và cơ chế bao đóng làm việc tay trong tay, giải quyết mọi vấn đề của thế giới. Tôi không biết bạn thế nào, nhưng điều đó làm tôi trở thành một JavaScripter hạnh phúc.

## Khối chức năng

Có những mô hình mã khác tận dụng sức mạnh của cơ chế bao đóng nhưng bề ngoài không có vẻ liên quan tới các hàm gọi lại. Hãy xem xét mô hình mạnh mẽ nhất trong số đó: *khối chức năng (module)*.

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

Như đoạn mã này hiện tại, không có cơ chế bao đóng nào có thể quan sát được. Chúng ta chỉ đơn giản có một số biến dữ liệu riêng tư `something` và `another`, và một vài hàm nội bộ `doSomething()` và `doAnother()`, cả hai đều có phạm vi từ vựng (và do đó là sự bao đóng!) trên phạm vi nội bộ của `foo()`.

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

Đây là mô hình trong JavaScript mà chúng ta gọi là *khối chức năng*. Cách phổ biến nhất để thực hiện mô hình khối chức năng thường được gọi là "Mô hình Khối chức năng Lộ diện" (Revealing Module), và đó là biến thể chúng ta trình bày ở đây.

Hãy xem xét một số điều về đoạn mã này.

Thứ nhất, `CoolModule()` chỉ là một hàm, nhưng nó *phải được gọi* để một thực thể khối chức năng được tạo ra. Nếu không có việc thực thi hàm bên ngoài, việc tạo ra phạm vi nội bộ và các cơ chế bao đóng sẽ không xảy ra.

Thứ hai, hàm `CoolModule()` trả về một đối tượng, được biểu thị bằng cú pháp đối tượng nguyên thủy `{ key: value, ... }`. Đối tượng chúng ta trả về có các tham chiếu đến các hàm nội bộ, nhưng *không* phải đến các biến dữ liệu nội bộ. Chúng ta giữ cho chúng bí ẩn và bí mật. Thật thích hợp khi nghĩ giá trị trả về đối tượng này về cơ bản là một **API công khai cho khối chức năng của chúng ta**.

Giá trị trả về đối tượng này cuối cùng được gán cho biến `foo` bên ngoài, và sau đó chúng ta có thể truy cập các phương thức thuộc tính đó trên API như `foo.doSomething()`.

**Ghi chú:** Chúng ta không bắt buộc phải trả về một đối tượng thực tế (nguyên thủy) từ khối chức năng của mình. Chúng ta có thể chỉ trả về trực tiếp một hàm nội bộ. jQuery thực sự là một ví dụ tốt cho trường hợp này. Các định danh `jQuery` và `$` là API công khai cho "khối chức năng" jQuery, nhưng bản thân chúng chỉ là một hàm (mà bản thân nó có thể có các thuộc tính, vì tất cả các hàm đều là đối tượng).

Các hàm `doSomething()` và `doAnother()` có sự bao đóng trên phạm vi bên trong của "thực thể" khối chức năng (đạt được bằng cách thực sự gọi `CoolModule()`). Khi chúng ta chuyển các hàm đó ra ngoài phạm vi từ vựng, thông qua các tham chiếu thuộc tính trên đối tượng trả về, chúng ta đã thiết lập một điều kiện mà qua đó cơ chế bao đóng có thể được quan sát và thực thi.

Nói một cách đơn giản hơn, có hai "yêu cầu" để mô hình khối chức năng được thực thi:

1. Phải có một hàm bao bọc bên ngoài, và nó phải được gọi ít nhất một lần (mỗi lần tạo ra một thực thể khối chức năng mới).

2. Hàm bao bọc phải trả về ít nhất một hàm nội bộ, để hàm nội bộ này có sự bao đóng trên phạm vi riêng, và có thể truy cập và/hoặc sửa đổi trạng thái riêng tư đó.

Một đối tượng chỉ có một thuộc tính hàm trên nó không *thực sự* là một khối chức năng. Một đối tượng được trả về từ một lời gọi hàm chỉ có các thuộc tính dữ liệu trên nó và không có hàm nào được bao đóng thì không *thực sự* là một khối chức năng, theo nghĩa có thể quan sát được.

Đoạn mã trên cho thấy một hàm tạo khối chức năng độc lập có tên là `CoolModule()` có thể gọi bao nhiêu lần cũng được, mỗi lần tạo ra một thực thể khối chức năng mới. Một biến thể nhỏ của mô hình này là khi bạn chỉ quan tâm đến việc có một thực thể duy nhất, một dạng "đơn thể (singleton)":

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

Ở đây, chúng ta đã biến hàm khối chức năng của mình thành một IIFE (xem chương 3), và chúng ta gọi nó *ngay lập tức* và trực tiếp gán giá trị trả về của nó cho định danh thực thể khối chức năng duy nhất của chúng ta là `foo`.

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

Một biến thể nhỏ nhưng mạnh mẽ khác của mô hình khối chức năng là đặt tên cho đối tượng bạn đang trả về làm API công khai của mình:

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
})( "foo module" );

foo.identify(); // foo module
foo.change();
foo.identify(); // FOO MODULE
```

Bằng cách giữ lại một tham chiếu nội bộ đến đối tượng API công khai bên trong thực thể khối chức năng của bạn, bạn có thể sửa đổi thực thể khối chức năng đó **từ bên trong**, bao gồm thêm và xóa các phương thức, thuộc tính, *và* thay đổi giá trị của chúng.

### Các Khối chức năng Hiện đại

Các thư viện tải/quản lý phụ thuộc khối chức năng khác nhau về cơ bản gói gọn mô hình định nghĩa khối chức năng này vào một API thân thiện. Thay vì xem xét bất kỳ một thư viện cụ thể nào, hãy để tôi trình bày một minh chứng khái niệm *rất đơn giản* **(chỉ) cho mục đích minh họa**:

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

Phần quan trọng của đoạn mã này là `modules[name] = impl.apply(impl, deps)`. Đoạn này gọi hàm bao bọc định nghĩa cho một khối chức năng (truyền vào bất kỳ phụ thuộc nào) và lưu trữ giá trị trả về, API của khối chức năng vào một danh sách nội bộ các khối chức năng được theo dõi bằng tên.

Và đây là cách tôi có thể sử dụng nó để định nghĩa một số khối chức năng:

```js
MyModules.define( "bar", [], function(){
	function hello(who) {
		return "Để tôi giới thiệu: " + who;
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
); // Để tôi giới thiệu: hippo

foo.awesome(); // ĐỂ TÔI GIỚI THIỆU: HIPPO
```

Cả hai khối chức năng "foo" và "bar" đều được định nghĩa với một hàm trả về một API công khai. "foo" thậm chí còn nhận thực thể của "bar" như một tham số phụ thuộc và có thể sử dụng nó tương ứng.

Hãy dành chút thời gian xem xét các đoạn mã này để hiểu đầy đủ sức mạnh của cơ chế bao đóng được đưa vào sử dụng cho các mục đích tốt đẹp của chúng ta. Điểm mấu chốt là không thực sự có bất kỳ "phép màu" đặc biệt nào đối với các chương trình quản lý khối chức năng. Chúng thỏa mãn cả hai đặc điểm của mô hình khối chức năng mà tôi đã liệt kê ở trên: gọi một hàm bao bọc định nghĩa và giữ giá trị trả về của nó làm API cho khối chức năng đó.

Nói cách khác, khối chức năng vẫn chỉ là khối chức năng, ngay cả khi bạn đặt một công cụ bao bọc thân thiện lên trên chúng.

### Các Khối chức năng Tương lai

ES6 thêm cú pháp ưu tiên lớp hỗ trợ cho khái niệm khối chức năng. Khi được tải qua hệ thống khối chức năng, ES6 coi một tập tin như một khối chức năng riêng biệt. Mỗi khối chức năng có thể vừa nhập các khối chức năng khác hoặc các API thành viên cụ thể, vừa xuất các API thành viên công khai của riêng mình.

**Ghi chú:** Các khối chức năng dựa trên hàm không phải là một mô hình được nhận dạng tĩnh (thứ mà chương trình biên dịch biết đến), vì vậy các ngữ nghĩa API của chúng không được xem xét cho đến khi chạy. Tức là, bạn thực sự có thể sửa đổi API của một khối chức năng trong thời gian chạy (xem thảo luận về `publicAPI` trước đó).

Ngược lại, API của Khối chức năng ES6 là tĩnh (các API không thay đổi trong thời gian chạy). Vì chương trình biên dịch biết *điều đó*, nó có thể (và có!) kiểm tra trong quá trình (tải tập tin và) biên dịch rằng một tham chiếu đến một thành viên của API của một khối chức năng được nhập *thực sự tồn tại*. Nếu tham chiếu API không tồn tại, chương trình biên dịch sẽ ném ra một lỗi "sớm" tại thời điểm biên dịch thay vì chờ đợi sự phân giải động truyền thống trong thời gian chạy (và các lỗi, nếu có).

Các khối chức năng ES6 **không** có định dạng "nội tuyến", chúng phải được định nghĩa trong các tập tin riêng biệt (mỗi tập tin một khối chức năng). Các chương trình duyệt/bộ máy có một "lớp tải khối chức năng" mặc định (có thể bị ghi đè, nhưng điều đó vượt xa cuộc thảo luận của chúng ta ở đây) tải đồng bộ một tập tin khối chức năng khi nó được nhập.

Xem xét:

**bar.js**
```js
function hello(who) {
	return "Để tôi giới thiệu: " + who;
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
); // Để tôi giới thiệu: rhino

foo.awesome(); // ĐỂ TÔI GIỚI THIỆU: HIPPO
```

**Ghi chú:** Các tập tin riêng biệt **"foo.js"** và **"bar.js"** cần được tạo với nội dung tương ứng như trong hai đoạn mã đầu tiên. Sau đó, chương trình của bạn sẽ tải/nhập các khối chức năng đó để sử dụng chúng như trong đoạn mã thứ ba.

`import` nhập một hoặc nhiều thành viên từ API của một khối chức năng vào phạm vi hiện tại, mỗi thành viên vào một biến được liên kết (`hello` trong trường hợp của chúng ta). `module` nhập toàn bộ API của một khối chức năng vào một biến được liên kết (`foo`, `bar` trong trường hợp của chúng ta). `export` xuất một định danh (biến, hàm) ra API công khai cho khối chức năng hiện tại. Các thao tác này có thể được sử dụng nhiều lần trong định nghĩa của một khối chức năng khi cần thiết.

Nội dung bên trong *tập tin khối chức năng* coi như được bao bọc trong một phạm vi, giống như với các khối chức năng khép kín - hàm đã thấy trước đó.

## Tóm lại

Đối với người chưa giác ngộ, cơ chế bao đóng dường như là một thế giới huyền bí được tách biệt bên trong JavaScript mà chỉ có những tâm hồn dũng cảm nhất mới có thể đạt tới. Nhưng nó thực sự chỉ là một tiêu chuẩn và gần như hiển nhiên về cách chúng ta viết mã trong một môi trường có phạm vi từ vựng, nơi các hàm là giá trị và có thể được truyền đi theo ý muốn.

**Cơ chế bao đóng là khi một hàm có thể ghi nhớ và truy cập phạm vi từ vựng của nó ngay cả khi nó được gọi bên ngoài phạm vi từ vựng đó.**

Cơ chế này có thể làm chúng ta vấp ngã, chẳng hạn như với các vòng lặp, nếu chúng ta không cẩn thận nhận ra chúng và cách chúng hoạt động. Nhưng chúng cũng là một công cụ vô cùng mạnh mẽ, cho phép các mô hình như *khối chức năng* trong các dạng khác nhau của chúng.

Các khối chức năng yêu cầu hai đặc điểm chính: 1) một hàm bao bọc bên ngoài được gọi để tạo ra phạm vi bao quanh 2) giá trị trả về của hàm bao bọc phải bao gồm tham chiếu đến ít nhất một hàm nội bộ mà sau đó có sự bao đóng trên phạm vi riêng bên trong của hàm bao bọc.

Giờ đây, chúng ta có thể nhìn thấy cơ chế này hiện diện khắp nơi trong mã nguồn của mình, và chúng ta đã có khả năng nhận biết cũng như tận dụng chúng vì lợi ích của chính mình.
