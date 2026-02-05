# Bạn không hiểu JS: Phạm vi & Hàm khép kín
# Phụ lục A: Phạm vi Động

Ở Chương 2, chúng ta đã tìm hiểu về "Phạm vi Động" như một cơ chế đối lập với mô hình "Phạm vi Từ vựng" - vốn là cách mà phạm vi vận hành trong JavaScript (và thực tế là hầu hết các ngôn ngữ khác).

Chúng ta sẽ khảo sát sơ lược về phạm vi động nhằm khắc sâu sự tương phản này. Nhưng quan trọng hơn, phạm vi động thực chất lại có mối liên hệ mật thiết với một cơ chế khác (`this`) trong JavaScript, mà chúng ta đã đề cập trong cuốn "*this & Nguyên mẫu Đối tượng*" thuộc bộ sách này.

Như ta đã biết ở Chương 2, phạm vi từ vựng là tập hợp các quy tắc về cách *Bộ máy* có thể tra cứu một biến và nơi nó sẽ tìm thấy biến đó. Đặc tính cốt lõi của phạm vi từ vựng là nó được định nghĩa tại thời điểm viết mã, khi mã nguồn được tạo ra (giả sử bạn không "gian lận" với `eval()` hay `with`).

Phạm vi động, đúng như tên gọi, dường như gợi lên ý tưởng về một mô hình mà ở đó phạm vi có thể được xác định một cách linh động tại thời điểm thực thi thay vì được xác định tĩnh tại thời điểm viết mã. Và thực tế đúng là như vậy. Hãy cùng minh họa qua mã nguồn:

```js
function foo() {
	console.log( a ); // 2
}

function bar() {
	var a = 3;
	foo();
}

var a = 2;

bar();
```

Phạm vi từ vựng quy định rằng tham chiếu RHS đến `a` trong hàm `foo()` sẽ được phân giải thành biến toàn cục `a`, dẫn đến kết quả là giá trị `2` được xuất ra.

Phạm vi động, ngược lại, không quan tâm đến cách thức và vị trí mà các hàm và phạm vi được khai báo, thay vào đó là **nơi chúng được gọi**. Nói cách khác, chuỗi phạm vi được dựa trên ngăn-xếp-gọi-hàm (call-stack), chứ không phải cấu trúc lồng nhau của các phạm vi trong mã nguồn.

Vì vậy, nếu JavaScript có phạm vi động, khi `foo()` được thực thi, **về mặt lý thuyết** đoạn mã dưới đây sẽ cho ra kết quả là `3`.

```js
function foo() {
	console.log( a ); // 3  (chứ không phải 2!)
}

function bar() {
	var a = 3;
	foo();
}

var a = 2;

bar();
```

Tại sao lại như vậy? Bởi vì khi `foo()` không thể phân giải được tham chiếu đến biến `a`, thay vì leo lên chuỗi phạm vi (từ vựng) lồng nhau, nó sẽ đi ngược lên ngăn-xếp-gọi-hàm để tìm xem `foo()` được *gọi từ đâu*. Vì `foo()` được gọi từ `bar()`, nó sẽ kiểm tra các biến trong phạm vi của `bar()` và tìm thấy một biến `a` có giá trị `3` ở đó.

Kỳ lạ phải không? Có lẽ lúc này bạn đang nghĩ vậy.

Nhưng có lẽ đó là vì bạn đã quen làm việc (hoặc ít nhất là tư duy sâu) với mã nguồn có phạm vi theo kiểu từ vựng. Do đó phạm vi động có vẻ xa lạ. Nếu bạn từng viết mã bằng một ngôn ngữ có phạm vi động, nó sẽ có vẻ rất tự nhiên, và khi đó phạm vi từ vựng mới là thứ kỳ quặc.

Cần phải khẳng định rõ: **trên thực tế** JavaScript **không có phạm vi động**. Nó có phạm vi từ vựng. Chỉ đơn giản vậy thôi. Nhưng cơ chế `this` lại có phần nào đó giống với phạm vi động.

Điểm đối lập mấu chốt: **phạm vi từ vựng thuộc về thời điểm viết mã, trong khi phạm vi động (và cả `this`!) lại thuộc về thời điểm thực thi**. Phạm vi từ vựng quan tâm *nơi một hàm được khai báo*, nhưng phạm vi động lại quan tâm *nơi một hàm được gọi*.

Cuối cùng: `this` quan tâm đến *cách thức một hàm được gọi*, điều này cho thấy cơ chế `this` có mối liên hệ chặt chẽ đến nhường nào với ý tưởng về phạm vi động. Để tìm hiểu sâu hơn về `this`, hãy đọc cuốn "*this & Nguyên mẫu Đối tượng*".
