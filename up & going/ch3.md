# Bạn không hiểu JS: Khởi động và tiến lên
# Chương 3: Đến với BKHJS

Bộ sách này nói về điều gì? Nói một cách đơn giản, đó là việc nghiêm túc với nhiệm vụ học *mọi thứ về JavaScript* chứ không chỉ một tập con của ngôn ngữ mà người ta gọi là "những phần tốt đẹp", cũng không chỉ là lượng kiến thức tối thiểu bạn cần để cho xong việc.

Lập trình viên chuyên nghiệp ở các ngôn ngữ khác đều xác định sẽ nỗ lực học hỏi hầu hết hoặc toàn bộ (các) ngôn ngữ mà họ chủ yếu dùng để viết mã, nhưng lập trình viên JS dường như nổi bật giữa đám đông theo cái cách là thường không học hỏi bao nhiêu về ngôn ngữ này. Đây không phải là điều hay ho và cũng không phải là điều chúng ta nên mặc nhiên cho là bình thường.

Bộ sách *Bạn không hiểu JS* (*BKHJS*) hoàn toàn tương phản với những cách tiếp cận thông thường để học JS, và không giống với hầu hết bất kỳ cuốn sách JS nào khác mà bạn sẽ đọc. Nó thách thức bạn vượt ra khỏi vùng thoải mái của mình và đặt ra những câu hỏi "tại sao" sâu sắc hơn cho mỗi một hành vi mà bạn gặp phải. Bạn có sẵn sàng cho thử thách đó không?

Tôi sẽ dùng chương cuối cùng này để tóm lược ngắn gọn những gì sẽ có trong các cuốn sách còn lại của bộ sách, và cách hiệu quả nhất để xây dựng nền tảng học JS từ *BKHJS*.

## Phạm vi và Hàm khép kín

Có lẽ một trong những điều căn bản nhất bạn cần nhanh chóng nắm bắt được là cách phạm vi của biến thực sự hoạt động trong JavaScript. Chỉ có những *niềm tin* mơ hồ, mang tính truyền miệng về phạm vi là không đủ.

Cuốn *Phạm vi và Hàm khép kín* bắt đầu bằng việc lật tẩy một quan niệm sai lầm phổ biến rằng JS là một "ngôn ngữ thông dịch" và do đó không được biên dịch. Sai bét.

Bộ máy JS biên dịch mã của bạn ngay trước khi (và đôi khi là trong cả lúc!) thực thi. Vì vậy, chúng ta sẽ vận dụng hiểu biết sâu hơn về cách tiếp cận của chương trình biên dịch để hiểu cách nó tìm và xử lý các khai báo biến và hàm. Trong quá trình đó, ta sẽ thấy thuật ngữ hình tượng điển hình cho việc quản lý phạm vi biến trong JS, "Kéo lên" ("Hoisting").

Sự am tường cốt lõi về "phạm vi từ vựng" chính là nền tảng để chúng ta khám phá về hàm khép kín trong chương cuối của cuốn sách. Hàm khép kín có lẽ là khái niệm quan trọng bậc nhất trong toàn bộ JS, nhưng nếu bạn chưa nắm vững cách hoạt động của phạm vi, hàm khép kín rất có thể sẽ vẫn nằm ngoài tầm tay của bạn.

Một ứng dụng quan trọng của hàm khép kín là mô hình khối chức năng, như chúng ta đã giới thiệu ngắn gọn trong cuốn sách này ở chương 2. Khối chức năng có lẽ là mô hình tổ chức mã phổ biến nhất trong toàn bộ JavaScript; hiểu biết sâu sắc về nó nên là một trong những ưu tiên hàng đầu của bạn.

## `this` và Nguyên mẫu Đối tượng

Có lẽ một trong những lầm tưởng tai hại và dai dẳng nhất về JavaScript là từ khóa `this` chỉ đến cái hàm mà nó xuất hiện bên trong. Một sai lầm tệ hại.

Từ khóa `this` được ràng buộc một cách linh động dựa vào cách mà hàm đó được thực thi, và hóa ra có bốn quy tắc đơn giản để thấu hiểu và xác định toàn bộ ràng buộc của `this`.

Liên quan mật thiết đến từ khóa `this` là cơ chế nguyên mẫu đối tượng (object prototype), một chuỗi tra cứu thuộc tính tương tự như cách các biến trong phạm vi từ vựng được tìm thấy. Nhưng ẩn chứa bên trong các nguyên mẫu là một ngộ nhận tai hại khác về JS: ý tưởng mô phỏng (làm giả) các lớp và sự kế thừa (còn gọi là "kế thừa qua nguyên mẫu").

Thật không may, ước muốn đem tư duy thiết kế theo lớp và kế thừa vào JavaScript gần như là điều tồi tệ nhất bạn có thể làm, bởi vì trong khi cú pháp có thể đánh lừa bạn tin rằng có một thứ gì đó giống như lớp đang hiện diện, thực tế cơ chế nguyên mẫu lại có hành vi, về cơ bản, hoàn toàn trái ngược.

Vấn đề nằm ở chỗ liệu sẽ tốt hơn nếu phớt lờ sự bất tương đồng và giả vờ rằng những gì bạn đang triển khai là "kế thừa", hay sẽ phù hợp hơn nếu học hỏi và trân trọng cách hệ thống nguyên mẫu đối tượng thực sự vận hành. Cách thứ hai được đặt một cái tên thích hợp hơn là "ủy quyền hành vi".

Đây không chỉ là sở thích về cú pháp. Ủy quyền là một mô hình thiết kế hoàn toàn khác biệt và mạnh mẽ hơn, một mô hình thay thế cho nhu cầu thiết kế bằng lớp và kế thừa. Nhưng những khẳng định này chắc chắn sẽ đi ngược lại gần như mọi bài blog, sách và bài nói chuyện tại hội thảo về chủ đề này trong suốt chiều dài lịch sử của JavaScript.

Luận điểm tôi đưa ra về ủy quyền so với kế thừa không phải đến từ sự ghét bỏ ngôn ngữ này và cú pháp của nó, mà từ khao khát được thấy tiềm năng thực sự của ngôn ngữ được khai thác đúng cách và xóa sạch những mơ hồ cùng bức xúc không hồi kết.

Nhưng luận điểm tôi đưa ra về nguyên mẫu và ủy quyền còn phức tạp hơn nhiều so với những gì tôi sẽ trình bày ở đây. Nếu bạn đã sẵn sàng để xem xét lại mọi thứ bạn nghĩ rằng bạn biết về "lớp" và "kế thừa" trong JavaScript, tôi mời bạn hãy "chọn viên thuốc đỏ" (*Ma Trận* 1999) và tìm đọc chương 4-6 của cuốn *`this` và Nguyên mẫu Đối tượng* trong bộ sách này.

## Kiểu dữ liệu và Ngữ pháp

Cuốn thứ ba trong bộ sách này chủ yếu tập trung vào việc giải quyết một chủ đề gây tranh cãi nảy lửa khác: ép kiểu. Có lẽ không có chủ đề nào gây ra nhiều sự bực dọc cho lập trình viên JS hơn khi bàn về những rối rắm quanh việc ép kiểu ngầm.

Cho đến nay, lối nghĩ thông thường cho rằng ép kiểu ngầm là một "phần tồi tệ" của ngôn ngữ và phải tránh bằng mọi giá. Trên thực tế, một số người đã đi xa đến mức gọi nó là một "khiếm khuyết" trong thiết kế ngôn ngữ. Thật vậy, có những công cụ mà toàn bộ công việc của nó chỉ là quét mã của bạn và phàn nàn nếu bạn đang làm bất cứ điều gì dù chỉ hơi giống với ép kiểu.

Nhưng liệu ép kiểu có thực sự rối rắm, tệ hại, và nguy hiểm đến mức mã của bạn cầm chắc thất bại ngay từ đầu nếu dùng nó?

Tôi sẽ nói không. Sau khi đã xây dựng được sự hiểu biết về cách các kiểu và giá trị thực sự hoạt động trong chương 1-3, chương 4 sẽ đi vào cuộc tranh luận này và giải thích cặn kẽ cách ép kiểu hoạt động đến từng ngóc ngách. Chúng ta sẽ thấy rõ những phần nào của ép kiểu thực sự gây ngạc nhiên và những phần nào thực sự hoàn toàn có lý nếu dành thời gian để học.

Nhưng tôi không chỉ đơn thuần cho rằng ép kiểu là hợp lý và có thể học được, tôi còn khẳng định rằng ép kiểu là một công cụ cực kỳ hữu ích và hoàn toàn bị đánh giá thấp mà *bạn nên sử dụng trong mã của mình*. Tôi muốn nói rằng khi ép kiểu được sử dụng đúng cách, nó không chỉ hoạt động mà còn khiến mã của bạn tốt hơn. Tất cả những kẻ phản đối và hoài nghi chắc chắn sẽ chế nhạo một lập trường như vậy, nhưng tôi tin rằng đó là một trong những chìa khóa chính để nâng trình JS của bạn.

Bạn muốn tiếp tục nghe theo đám đông hay sẵn sàng gạt bỏ mọi giả định sang một bên và nhìn nhận việc ép kiểu với một con mắt hoàn toàn mới? Cuốn *Kiểu dữ liệu và Ngữ pháp* trong bộ sách này sẽ ép buộc cả tư duy của bạn.

## Bất đồng bộ và Hiệu năng

Ba cuốn sách đầu tiên của bộ sách tập trung vào các cơ chế cốt lõi của ngôn ngữ, nhưng cuốn thứ tư mở rộng ra một chút để bao quát các mẫu hình được xây dựng trên các cơ chế đó nhằm quản lý lập trình bất đồng bộ. Tính bất đồng bộ không chỉ tối quan trọng với hiệu năng của các ứng dụng mà ngày càng trở thành yếu tố then chốt quyết định khả năng viết và bảo trì mã nguồn.

Cuốn sách bắt đầu bằng việc làm sáng tỏ rất nhiều thuật ngữ và khái niệm mơ hồ xoay quanh những thứ như "bất đồng bộ" (async), "song song" (parallel), và "đồng thời" (concurrent), và giải thích sâu về cách chúng áp dụng và không áp dụng cho JS như thế nào.

Sau đó, chúng ta chuyển sang xem xét hàm gọi lại là phương pháp chính để hiện thực hóa tính bất đồng bộ. Nhưng chính tại đây, chúng ta nhanh chóng thấy rằng chỉ riêng hàm gọi lại thì không đáp ứng đủ cho các yêu cầu hiện đại của lập trình bất đồng bộ. Chúng ta xác định hai khiếm khuyết lớn của việc lập trình chỉ dùng hàm gọi lại: mất đi sự tin cậy do *Điều khiển nghịch đảo* (Inversion of Control - IoC) và thiếu đi khả năng suy luận tuần tự.

Để giải quyết hai khiếm khuyết lớn này, ES6 giới thiệu hai cơ chế (và thực ra là các mô hình) mới: lời hứa (promise) và hàm sinh (generator).

Lời hứa là một lớp vỏ bọc độc lập với thời gian quanh một "giá trị tương lai", cho phép bạn suy luận và kết hợp chúng bất kể giá trị đã sẵn sàng hay chưa. Hơn nữa, chúng giải quyết hiệu quả các vấn đề về sự tin cậy trong IoC bằng cách điều hướng các hàm gọi lại thông qua một cơ chế hứa hẹn đáng tin cậy và có khả năng kết hợp.

Hàm sinh giới thiệu một chế độ thực thi mới cho các hàm JS, theo đó hàm có thể được tạm dừng tại các điểm `yield` và được tiếp tục một cách bất đồng bộ sau đó. Khả năng tạm dừng và tiếp tục này cho phép đoạn mã trông có vẻ đồng bộ, tuần tự trong hàm sinh được xử lý bất đồng bộ dưới nền. Bằng cách đó, chúng ta giải quyết được những rối rắm do các bước nhảy phi tuyến tính, không cục bộ của hàm gọi lại và do đó khiến mã bất đồng bộ của chúng ta trông giống như đồng bộ, từ đó dễ suy luận hơn.

Nhưng chính sự kết hợp của lời hứa và hàm sinh đã "mang lại" mẫu hình lập trình bất đồng bộ hiệu quả nhất của chúng ta cho đến nay trong JavaScript. Trên thực tế, phần lớn những sự tinh vi trong tương lai của tính bất đồng bộ sắp tới trong ES7 và các phiên bản sau này chắc chắn sẽ được xây dựng trên nền tảng này. Để nghiêm túc về việc lập trình hiệu quả trong một thế giới bất đồng bộ, bạn sẽ cần phải thực sự thoải mái với việc kết hợp lời hứa và hàm sinh.

Nếu lời hứa và hàm sinh là về việc thể hiện các mô hình cho phép chương trình của chúng ta chạy đồng thời hơn và do đó hoàn thành nhiều xử lý hơn trong một khoảng thời gian ngắn hơn, thì JS còn có nhiều khía cạnh khác về tối ưu hóa hiệu năng đáng để khám phá.

Chương 5 đi sâu vào các chủ đề như tính song song của chương trình với Web Worker và tính song song của dữ liệu với SIMD, cũng như các kỹ thuật tối ưu hóa cấp thấp như ASM.js. Chương 6 xem xét tối ưu hóa hiệu năng từ góc độ các kỹ thuật đo lường hiệu năng đúng đắn, bao gồm cả những loại hiệu năng nào cần lo lắng và những loại nào có thể bỏ qua.

Viết JavaScript hiệu quả có nghĩa là viết mã có thể phá vỡ rào cản ràng buộc của việc chạy động trong một loạt các trình duyệt và môi trường khác nhau. Nó đòi hỏi rất nhiều kế hoạch và nỗ lực chi tiết và phức tạp từ phía chúng ta để đưa một chương trình từ chỗ "chạy được" đến chỗ "chạy tốt".

Cuốn *Bất đồng bộ và Hiệu năng* được thiết kế để cung cấp cho bạn tất cả các công cụ và kỹ năng cần thiết để viết mã JavaScript hợp lý và hiệu quả.

## ES6 và hơn thế nữa

Bất kể bạn cảm thấy mình đã làm chủ JavaScript đến mức nào, sự thật là JavaScript sẽ không bao giờ ngừng phát triển, và hơn nữa, tốc độ phát triển đang tăng lên nhanh chóng. Thực tế này gần như là một phép ẩn dụ cho tinh thần của bộ sách này, đó là hãy chấp nhận rằng chúng ta sẽ không bao giờ *hiểu* trọn vẹn mọi phần của JS, bởi vì ngay khi bạn làm chủ tất cả, sẽ có những thứ mới xuất hiện mà bạn sẽ cần phải học.

Cuốn sách này dành riêng cho cả tầm nhìn ngắn hạn và trung hạn về hướng đi của ngôn ngữ, không chỉ những thứ *đã biết* như ES6 mà còn cả những thứ *có khả năng* sẽ vượt xa hơn thế.

Trong khi tất cả các cuốn sách của bộ này đều đón nhận trạng thái của JavaScript tại thời điểm viết cuốn sách này, tức là giữa giai đoạn áp dụng ES6, trọng tâm chính trong bộ sách vẫn là ES5. Bây giờ, chúng ta muốn chuyển sự chú ý sang ES6, ES7, và ...

Vì ES6 đã gần như hoàn thiện tại thời điểm viết cuốn sách này, *ES6 và hơn thế nữa* bắt đầu bằng cách phân chia những thứ hữu hình từ toàn cảnh ES6 thành nhiều loại chính, bao gồm cú pháp mới, cấu trúc dữ liệu mới (bộ sưu tập), và các khả năng xử lý cùng API mới. Chúng ta đề cập đến từng tính năng mới này của ES6 ở các mức độ chi tiết khác nhau, bao gồm cả việc xem lại các chi tiết được đề cập trong các cuốn khác của bộ sách.

Một số điều thú vị của ES6 mong chờ bạn khám phá: phân rã cấu trúc, giá trị tham số mặc định, biểu tượng, phương thức tinh gọn, thuộc tính được tính toán, hàm mũi tên, phạm vi khối lệnh, lời hứa, hàm sinh, bộ lặp, khối chức năng, lớp ủy quyền, WeakMap, và còn nhiều, nhiều nữa! Quả thật, nội lực của ES6 mạnh mẽ vô cùng!

Phần đầu của cuốn sách là lộ trình cho tất cả những gì bạn cần học để sẵn sàng cho một JavaScript mới và cải tiến mà bạn sẽ viết và khám phá trong vài năm tới.

Phần sau của cuốn sách chuyển sự chú ý sang việc lướt qua những điều mà chúng ta có thể mong đợi sẽ thấy trong tương lai gần của JavaScript. Nhận thức quan trọng nhất ở đây là sau ES6, JS có khả năng sẽ phát triển theo từng tính năng hơn là theo từng phiên bản, điều đó có nghĩa là chúng ta có thể mong đợi thấy những thứ trong tương lai gần này đến sớm hơn nhiều so với bạn tưởng tượng.

Tương lai của JavaScript thật xán lạn. Đã đến lúc chúng ta bắt đầu học nó rồi, phải không!?

## Nhìn lại

Bộ sách *BKHJS* tập trung vào một mục tiêu rằng tất cả các lập trình viên JS đều có thể và nên học tất cả các phần của ngôn ngữ tuyệt vời này. Không một ý kiến cá nhân, không một giả định của bộ khung, và không một hạn chót của dự án nào có thể là cái cớ để bạn không bao giờ học và am hiểu sâu sắc JavaScript.

Chúng ta sẽ xem xét từng mảng kiến thức trọng yếu của ngôn ngữ và dành cho mỗi mảng một cuốn sách ngắn gọn nhưng cô đọng, đi sâu khai phá những khía cạnh mà có lẽ bạn tưởng mình đã nắm vững nhưng thực ra lại chưa hề.

"Bạn không hiểu JS" không phải là một lời chỉ trích hay một sự sỉ nhục. Đó là một sự thật mà tất cả chúng ta, bao gồm cả tôi, phải chấp nhận. Học JavaScript không phải là đích đến mà là một quá trình. Chúng ta chưa hiểu JavaScript, chưa đâu. Nhưng chúng ta sẽ hiểu!
