# Bạn Không hiểu JS
# Lời mở đầu

Chắc hẳn bạn cũng nhận ra, nhưng "JS" trong tiêu đề của loạt sách này không phải là từ viết tắt để nguyền rủa Javascript, dù việc bực bội vì những điều kỳ quặc của ngôn ngữ này thì ai cũng từng trải qua!

Từ những ngày đầu của web, Javascript đã là một công nghệ nền tảng giúp tạo ra các trải nghiệm tương tác xung quanh nội dung mà ta tiêu thụ. Dù ban đầu chỉ là những vệt chuột lấp lánh hay các hộp thoại bật lên khó chịu, gần hai thập kỷ sau, công nghệ và khả năng của Javascript đã phát triển vượt bậc, và hiếm ai còn nghi ngờ vai trò quan trọng của nó trong nền tảng phần mềm phổ biến nhất thế giới: web.

Tuy nhiên, với tư cách là một ngôn ngữ, Javascript luôn là mục tiêu bị chỉ trích rất nhiều, một phần do nguồn gốc của nó, nhưng phần lớn hơn đến từ triết lý thiết kế. Ngay cả cái tên cũng từng khiến Brendan Eich gọi nó là "đứa em ngốc nghếch" bên cạnh người anh trưởng thành hơn là "Java". Nhưng cái tên đó chỉ là hệ quả của chính trị và tiếp thị. Hai ngôn ngữ này khác nhau hoàn toàn ở nhiều điểm quan trọng. "Javascript" và "Java" khác nhau như "Đường kính" khác với "Đường kính trắng".

Javascript vay mượn ý tưởng và cú pháp từ nhiều ngôn ngữ khác nhau, từ phong cách thủ tục kiểu C đến những ảnh hưởng sâu sắc nhưng ít thấy hơn từ Scheme/Lisp, nên rất dễ tiếp cận với nhiều đối tượng lập trình viên, kể cả những người gần như chưa có kinh nghiệm lập trình. Câu lệnh "Chào thế giới" trong Javascript rất đơn giản, khiến ngôn ngữ này trở nên thân thiện và dễ làm quen ngay từ lần đầu tiếp xúc.

Tuy Javascript là một trong những ngôn ngữ dễ bắt đầu nhất, nhưng những tính chất "quái lạ" của nó lại khiến việc nắm vững ngôn ngữ trở nên hiếm gặp hơn nhiều so với các ngôn ngữ khác. Nếu như để viết một chương trình C hoặc C++ hoàn chỉnh cần kiến thức chuyên sâu, thì nhiều sản phẩm Javascript quy mô lớn thường chỉ mới gãi nhẹ lên bề mặt những gì ngôn ngữ này có thể làm được.

Những khái niệm tinh vi và sâu sắc ăn sâu vào ngôn ngữ lại thường xuất hiện dưới những hình thức *có vẻ* đơn giản, như việc truyền hàm dưới dạng gọi lại, điều này dễ khiến lập trình viên Javascript chỉ dùng một cách nghiễm nhiên mà không quan tâm đến cơ chế vận hành thực sự phía sau.

Javascript vừa là một ngôn ngữ đơn giản, dễ sử dụng và phổ biến, vừa là một hệ thống ngôn ngữ phức tạp và tinh vi mà nếu không nghiên cứu cẩn thận thì ngay cả lập trình viên dày dạn cũng khó hiểu được *tường tận*.

Đó chính là nghịch lý của Javascript, là gót chân Achilles của ngôn ngữ này, và là thách thức mà loạt sách này hướng đến giải quyết. Bởi vì Javascript *có thể* sử dụng mà không cần hiểu rõ, nên việc thấu hiểu nó lại thường bị bỏ qua.

---

## Sứ mệnh

Nếu mỗi khi bạn gặp điều kỳ quặc hay khó chịu trong Javascript mà phản ứng đầu tiên là gạch bỏ nó, như cách nhiều người vẫn làm, thì chẳng mấy chốc bạn sẽ chỉ còn lại một vỏ bọc rỗng của những điều phong phú mà Javascript mang lại.

Tập hợp tính năng hạn chế này đã từng được gọi là "Những phần tốt đẹp", nhưng tôi mong bạn, người đọc thân mến, hãy nhìn nhận chúng là "Những phần dễ dùng", "Những phần an toàn", hay thậm chí là "Những phần chưa đầy đủ".

Loạt sách *Bạn không hiểu Javascript* này đưa ra một thách thức ngược lại: hãy học và hiểu sâu *tất cả* về Javascript, nhất là "những phần khó nhằn".

Chúng tôi đi thẳng vào thói quen phổ biến của lập trình viên JS là chỉ học "vừa đủ dùng", mà không bao giờ buộc bản thân tìm hiểu rõ ràng cách thức và lý do vì sao ngôn ngữ hoạt động như vậy. Chúng tôi cũng bác bỏ lời khuyên thường thấy là nên "rút lui" khi mọi thứ trở nên khó hiểu.

Tôi không hài lòng, và bạn cũng đừng nên, với việc dừng lại khi mọi thứ "chạy được" mà chẳng hiểu lý do thực sự. Tôi mời bạn bước lên con đường gập ghềnh, ít người đi đó, để đón nhận toàn bộ những gì Javascript là và có thể làm được. Khi có được kiến thức ấy, không có kỹ thuật nào, không bộ khung nào, không từ viết tắt mới nổi nào có thể nằm ngoài tầm hiểu biết của bạn.

Mỗi cuốn sách trong loạt này tập trung vào những phần cốt lõi của Javascript, những phần thường bị hiểu sai hoặc chưa được hiểu kỹ, và đi sâu, rất sâu vào chúng. Khi đọc xong, bạn sẽ có được sự tự tin vững chắc, không chỉ về mặt lý thuyết mà cả những điều thực tiễn "cần phải biết".

Phần Javascript mà bạn đang biết *ngay lúc này* có lẽ chỉ là *một phần* do người khác truyền lại, những người đã từng bị thiêu đốt bởi việc hiểu chưa tới nơi tới chốn. Javascript ấy chỉ là cái bóng của ngôn ngữ thực sự. Bạn không *thực sự* hiểu Javascript, *chưa hề*, nhưng nếu đi sâu vào loạt sách này, bạn *sẽ hiểu*. Hãy tiếp tục đọc nhé. Javascript đang đợi bạn.

---

## Tóm tắt

Javascript thật tuyệt vời. Học một phần thì dễ, nhưng học cho đầy đủ (hay thậm chí là *đủ dùng*) thì khó hơn nhiều. Khi lập trình viên gặp điều khó hiểu, họ thường đổ lỗi cho ngôn ngữ hơn là do chưa hiểu rõ. Loạt sách này muốn thay đổi điều đó, truyền cảm hứng để bạn thật sự trân trọng và nắm bắt ngôn ngữ mà bạn hoàn toàn có thể, và *nên*, hiểu *sâu sắc*.

**Lưu ý:** Nhiều ví dụ trong sách giả định rằng bạn đang sử dụng các môi trường Javascript hiện đại (và hướng tới tương lai), như ES6. Một số đoạn mã có thể không hoạt động đúng nếu chạy trong các chương trình thông dịch cũ (trước ES6).

