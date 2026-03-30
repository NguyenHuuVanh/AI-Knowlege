# Bản dịch tiếng Việt – Ứng dụng của RAG

Dưới đây là bản dịch tiếng Việt được biên soạn lại thành **nội dung hoàn chỉnh, mạch lạc và tự nhiên**:

---

Như bạn vừa thấy, ý tưởng cốt lõi của **RAG** là kết hợp một **LLM có sẵn** với một **kho tri thức** chứa những thông tin mà mô hình có thể chưa từng được tiếp cận trong quá trình huấn luyện.

Hiện nay, rất nhiều hệ thống sử dụng LLM đã áp dụng mô hình này, vì vậy hãy cùng xem qua một vài ví dụ tiêu biểu.

Một ứng dụng quan trọng của RAG là **sinh mã lập trình (code generation)**. Đây là một ứng dụng rất đáng chú ý, bởi vì mặc dù các mô hình ngôn ngữ đã được huấn luyện trên một lượng lớn mã nguồn — có thể hiểu là gần như toàn bộ các kho Git công khai — nhưng để sinh ra **đoạn code chính xác cho một dự án cụ thể**, mô hình vẫn cần những thông tin chuyên biệt hơn.

LLM cần biết các **class, function, định nghĩa** đang tồn tại trong dự án, cũng như **phong cách viết mã tổng thể** mà dự án đó đang sử dụng. Nếu bạn xây dựng một hệ thống RAG sử dụng chính **codebase của mình làm knowledge base**, thì toàn bộ những thông tin này sẽ được cung cấp cho LLM.

Bằng cách truy xuất những class, định nghĩa và tệp mã có liên quan từ repository, LLM sẽ có khả năng tốt hơn rất nhiều trong việc **viết code** hoặc **trả lời các câu hỏi liên quan đến dự án của bạn**.

Một ứng dụng lớn khác của RAG là **tùy biến chatbot cho từng công ty cụ thể**.

Mỗi công ty đều có sản phẩm, chính sách và nguyên tắc giao tiếp riêng. Khi xem các tài liệu doanh nghiệp này như một **knowledge base**, bạn có thể triển khai LLM theo nhiều cách rất hữu ích.

Ví dụ, bạn có thể xây dựng một **chatbot chăm sóc khách hàng** nắm rõ thông tin về sản phẩm của công ty, tình trạng tồn kho hiện tại hoặc các bước khắc phục sự cố thường gặp. Bạn cũng có thể triển khai một **chatbot nội bộ** có khả năng trả lời chính xác các câu hỏi về chính sách công ty hoặc hướng nhân viên đến những tài liệu hữu ích.

Trong cả hai trường hợp, knowledge base sẽ giúp **neo phản hồi của LLM vào những thông tin cụ thể của doanh nghiệp**, từ đó giảm thiểu các tình huống mà mô hình tạo ra những câu trả lời quá chung chung hoặc dễ gây hiểu lầm.

RAG cũng có những ứng dụng rất quan trọng trong các lĩnh vực như **y tế** và **pháp lý**.

Trong các trường hợp này, knowledge base có thể bao gồm các tài liệu pháp lý của một vụ việc cụ thể, văn bản từ những tạp chí y khoa mới được công bố, và nhiều loại tài liệu chuyên ngành khác.

Ở những lĩnh vực như vậy — nơi **độ chính xác là yêu cầu bắt buộc**, đồng thời lượng thông tin chuyên biệt và có thể mang tính riêng tư là vô cùng lớn — thì cách tiếp cận dựa trên RAG có thể là **con đường duy nhất** để triển khai một sản phẩm dựa trên LLM vừa đủ chính xác, vừa có thể sử dụng dữ liệu riêng tư.

Một ứng dụng lớn khác là **tìm kiếm web có hỗ trợ bởi AI**.

Trước đây, các công cụ tìm kiếm hoạt động giống như một **retriever**: khi nhận được truy vấn tìm kiếm, chúng sẽ trả về những trang web có liên quan. Ngày nay, các công cụ tìm kiếm còn cung cấp thêm **bản tóm tắt bằng AI** cho các kết quả đó, nhằm nhanh chóng trình bày những thông tin hữu ích nhất theo cách dễ đọc, dễ lướt.

Những phần tóm tắt web bằng AI này về cơ bản chính là một **hệ thống RAG**, trong đó **knowledge base là toàn bộ Internet**.

Mặc dù các hệ thống RAG quy mô lớn có thể rất mạnh, điều tương tự cũng đúng với những hệ thống **cá nhân hóa cao**.

Các trợ lý cá nhân trong tin nhắn, email, trình soạn thảo văn bản, lịch làm việc và nhiều công cụ khác giờ đây đã có thể hỗ trợ bạn gửi tin nhắn, sắp xếp lịch trình, soạn thảo tài liệu và hoàn thành cả những dự án lớn lẫn nhỏ.

Trong từng trường hợp như vậy, **LLM càng có nhiều ngữ cảnh về công việc mà bạn đang thực hiện thì nó càng hỗ trợ tốt hơn**.

Ở những tình huống này, knowledge base có thể tương đối nhỏ. Chẳng hạn, đó có thể là **tin nhắn văn bản, danh bạ liên hệ, email hoặc một thư mục tài liệu cá nhân**.

Tuy quy mô nhỏ, nhưng những loại tài liệu này lại chứa đựng rất nhiều ngữ cảnh quan trọng. Vì vậy, một hệ thống RAG có quyền truy cập vào nguồn thông tin cá nhân ở quy mô nhỏ như vậy có thể hoàn thành tác vụ theo cách **phù hợp hơn rất nhiều với đúng nhu cầu thực tế của bạn**.

Như bạn có thể thấy, mô hình RAG có thể được ứng dụng trong rất nhiều bối cảnh khác nhau.

Rất nhiều công ty, tổ chức, và thậm chí chính bản thân bạn, đều đang sở hữu những tập hợp thông tin có thể được dùng để nâng cao chất lượng văn bản do LLM tạo ra.

Bất cứ khi nào bạn có quyền truy cập vào những thông tin mà nhiều khả năng **không nằm trong dữ liệu huấn luyện của mô hình ngôn ngữ lớn**, thì đều tồn tại tiềm năng để xây dựng một ứng dụng RAG hữu ích.

Thậm chí, RAG còn có thể mở ra khả năng sử dụng LLM trong những bối cảnh mà trước đây gần như không thể áp dụng được.

Khi tiếp tục theo học khóa học này, hy vọng rằng bạn sẽ bắt đầu nhìn thấy những cơ hội trong công việc và cuộc sống của mình, nơi mà cách tiếp cận RAG có thể mang lại giá trị rõ rệt.

Còn bây giờ, hãy cùng sang video tiếp theo để tìm hiểu sâu hơn về **kiến trúc của một hệ thống RAG**.
