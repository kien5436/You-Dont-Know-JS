# Bạn không hiểu JS: Phạm vi & Hàm khép kín
# Chương 4: Kéo lên (Hoisting)

Đến giờ, hẳn bạn đã tương đối quen thuộc với khái niệm phạm vi và cách các biến được gắn vào các cấp khác nhau của phạm vi tùy thuộc vào vị trí và cách thức chúng được khai báo. Cả phạm vi hàm và phạm vi khối đều tuân theo cùng một quy tắc về phương diện này: bất kỳ biến nào được khai báo bên trong một phạm vi đều thuộc về phạm vi đó.

Tuy nhiên, có một chi tiết tinh vi trong cách thức mà phạm vi gắn kết với các khai báo xuất hiện ở nhiều vị trí khác nhau trong một phạm vi, chi tiết đó chính là điều chúng ta sẽ khảo sát trong chương này.

## Gà có trước hay trứng có trước?

Ta thường có xu hướng nghĩ rằng toàn bộ mã nguồn bạn thấy trong một chương trình JavaScript đều được thông dịch tuần tự từng dòng, từ trên xuống dưới, khi chương trình thực thi. Dù về cơ bản điều này là đúng, có một phần trong giả định đó có thể dẫn đến những suy luận thiếu chính xác về chương trình của bạn.

Hãy xem xét đoạn mã này:

```js
a = 2;

var a;

console.log( a );
```

Bạn kỳ vọng kết quả nào sẽ được in ra trong câu lệnh `console.log(..)`?

Nhiều lập trình viên sẽ cho rằng kết quả là `undefined` vì câu lệnh `var a` đứng sau `a = 2`, thật tự nhiên khi cho rằng biến này được định nghĩa lại và do đó được gán giá trị mặc định là `undefined`. Tuy nhiên, kết quả in ra sẽ là `2`.

Hãy xem xét một đoạn mã khác:

```js
console.log( a );

var a = 2;
```

Bạn có thể cho rằng, vì đoạn mã trước đã thể hiện một hành vi có vẻ không hoàn toàn tuần tự từ trên xuống, nên có lẽ trong đoạn mã này, số `2` cũng sẽ được in ra. Những người khác lại có thể nghĩ rằng vì biến `a` được sử dụng trước khi nó được khai báo, điều này hẳn sẽ gây ra lỗi `ReferenceError`.

Thật không may, cả hai phỏng đoán đều không chính xác. Kết quả in ra là `undefined`.

**Vậy, chuyện gì đang diễn ra?** Dường như chúng ta đang đối mặt với câu hỏi kinh điển: con gà (phép gán) có trước, hay quả trứng (khai báo) có trước?

## Chương trình biên dịch lại ra tay

Để trả lời câu hỏi này, chúng ta cần phải xem lại chương 1, phần thảo luận về chương trình biên dịch. Hãy nhớ lại rằng *Bộ máy* JavaScript thực chất sẽ biên dịch mã nguồn của bạn trước khi thông dịch nó. Một phần của giai đoạn biên dịch là tìm và liên kết tất cả các khai báo với phạm vi phù hợp của chúng. Chương 2 đã cho chúng ta thấy đây chính là cốt lõi của Phạm vi Từ vựng.

Vì vậy, cách hình dung tốt nhất là tất cả các khai báo, cả biến và hàm, đều được xử lý đầu tiên, trước khi bất kỳ phần nào trong mã của bạn được thực thi.

Khi thấy `var a = 2;`, bạn có thể nghĩ đó là một câu lệnh đơn. Nhưng JavaScript thực chất xem nó như hai câu lệnh riêng biệt: `var a;` và `a = 2;`. Câu lệnh đầu tiên, phần khai báo, được xử lý trong giai đoạn biên dịch. Câu lệnh thứ hai, phần gán giá trị, thì được để lại **nguyên tại chỗ** và chờ đến giai đoạn thực thi.

Do đó, đoạn mã đầu tiên của chúng ta nên được hình dung là được xử lý như sau:

```js
var a;
```
```js
a = 2;

console.log( a );
```

...trong đó phần đầu tiên là của giai đoạn biên dịch và phần thứ hai là của giai đoạn thực thi.

Tương tự, đoạn mã thứ hai của chúng ta thực chất được xử lý như sau:

```js
var a;
```
```js
console.log( a );

a = 2;
```

Vậy nên, có một cách để suy nghĩ về quá trình này, một cách ẩn dụ, là các khai báo biến và hàm được "di chuyển" từ vị trí chúng xuất hiện trong luồng mã lên trên đầu của mã. Chính vì thế mà có cái tên "Kéo lên" (Hoisting).

Nói cách khác, **quả trứng (khai báo) có trước con gà (phép gán)**.

**Lưu ý:** Chỉ riêng các khai báo mới được kéo lên, trong khi bất kỳ phép gán hay logic có thể thực thi nào khác đều được để lại *nguyên tại chỗ*. Nếu việc kéo lên sắp xếp lại cả logic thực thi của mã nguồn, điều đó có thể gây ra sự hỗn loạn khôn lường.

```js
foo();

function foo() {
	console.log( a ); // undefined

	var a = 2;
}
```

Phần khai báo của hàm `foo` (trong trường hợp này *bao gồm* cả giá trị mặc định của nó là một hàm thực sự) được kéo lên, nhờ vậy lời gọi ở dòng đầu tiên có thể thực thi được.

Cũng cần lưu ý rằng việc kéo lên này diễn ra trong **từng phạm vi một**. Vì vậy, trong khi các đoạn mã trước đây của chúng ta được đơn giản hóa vì chỉ bao gồm phạm vi toàn cục, thì chính hàm `foo(..)` mà chúng ta đang khảo sát đây lại cho thấy `var a` được kéo lên đầu của `foo(..)` (chứ hiển nhiên không phải là lên đầu toàn bộ chương trình). Do đó, chương trình có thể được diễn giải một cách chính xác hơn như thế này:

```js
function foo() {
	var a;

	console.log( a ); // undefined

	a = 2;
}

foo();
```

Khai báo hàm được kéo lên, như chúng ta vừa thấy. Nhưng biểu thức hàm thì không.

```js
foo(); // không phải ReferenceError, mà là TypeError!

var foo = function bar() {
	// ...
};
```

Định danh của biến `foo` được kéo lên và gắn vào phạm vi bao ngoài (toàn cục) của chương trình này, vì vậy `foo()` không thất bại với lỗi `ReferenceError`. Nhưng `foo` chưa có giá trị (như nó đáng lẽ sẽ có nếu đây là một khai báo hàm thực sự thay vì một biểu thức hàm). Do đó, `foo()` đang cố gắng gọi đến giá trị `undefined`, đây là một thao tác bất hợp lệ và gây ra lỗi `TypeError`.

Cũng hãy nhớ lại rằng mặc dù đây là một biểu thức hàm có định danh, tên định danh đó không tồn tại trong phạm vi bao ngoài:

```js
foo(); // TypeError
bar(); // ReferenceError

var foo = function bar() {
	// ...
};
```

Đoạn mã này được diễn giải (cùng với việc kéo lên) một cách chính xác hơn là:

```js
var foo;

foo(); // TypeError
bar(); // ReferenceError

foo = function() {
	var bar = ...chính nó...
	// ...
}
```

## Hàm trước tiên

Cả khai báo hàm và khai báo biến đều được kéo lên. Nhưng có một chi tiết tinh vi (thứ *có thể* xuất hiện trong mã nguồn có nhiều khai báo "trùng lặp") là hàm được kéo lên trước, sau đó mới đến biến.

Xem xét:

```js
foo(); // 1

var foo;

function foo() {
	console.log( 1 );
}

foo = function() {
	console.log( 2 );
};
```

Kết quả in ra là `1` thay vì `2`! Đoạn mã này được *Bộ máy* diễn giải thành:

```js
function foo() {
	console.log( 1 );
}

foo(); // 1

foo = function() {
	console.log( 2 );
};
```

Lưu ý rằng `var foo` là phần khai báo bị trùng lặp (và do đó bị bỏ qua), mặc dù nó đứng trước khai báo `function foo()...`, bởi vì các khai báo hàm được kéo lên trước các biến thông thường.

Trong khi các khai báo `var` trùng lặp về cơ bản sẽ bị bỏ qua, thì các khai báo hàm sau đó *sẽ* ghi đè lên các khai báo trước đó.

```js
foo(); // 3

function foo() {
	console.log( 1 );
}

var foo = function() {
	console.log( 2 );
};

function foo() {
	console.log( 3 );
}
```

Dù tất cả những điều này nghe có vẻ chẳng khác gì những kiến thức lý thuyết suông thú vị, nó lại nhấn mạnh một thực tế rằng việc có nhiều định nghĩa trùng lặp trong cùng một phạm vi là một ý tưởng cực kỳ tồi tệ và thường sẽ dẫn đến những kết quả khó lường.

Các khai báo hàm xuất hiện bên trong các khối lệnh thông thường thì thường được kéo lên phạm vi bao ngoài, thay vì mang tính điều kiện như đoạn mã này ngụ ý:

```js
foo(); // "b"

var a = true;
if (a) {
   function foo() { console.log( "a" ); }
}
else {
   function foo() { console.log( "b" ); }
}
```

Tuy nhiên, cần phải lưu ý rằng hành vi này không đáng tin cậy và có thể thay đổi trong các phiên bản JavaScript tương lai, vì vậy tốt nhất là nên tránh khai báo hàm bên trong các khối lệnh.

## Nhìn lại (TL;DR)

Chúng ta có thể dễ dàng xem `var a = 2;` như là một câu lệnh duy nhất, nhưng *Bộ máy* JavaScript không nhìn nhận như vậy. Nó xem `var a` và `a = 2` là hai câu lệnh riêng biệt, câu lệnh đầu tiên là một tác vụ của giai đoạn biên dịch, và câu lệnh thứ hai là một tác vụ của giai đoạn thực thi.

Điều này dẫn đến việc tất cả các khai báo trong một phạm vi, bất kể chúng xuất hiện ở đâu, đều được xử lý *trước tiên* trước khi chính đoạn mã đó được thực thi. Bạn có thể hình dung quá trình này như thể các khai báo (biến và hàm) được "di chuyển" lên đầu phạm vi tương ứng của chúng, mà chúng ta gọi là "kéo lên".

Bản thân các khai báo được kéo lên, nhưng các phép gán, ngay cả phép gán các biểu thức hàm, thì *không* được kéo lên.

Hãy cẩn trọng với các khai báo trùng lặp, đặc biệt là khi trộn lẫn giữa khai báo var thông thường và khai báo hàm -- rắc rối khôn lường đang chờ bạn phía trước
