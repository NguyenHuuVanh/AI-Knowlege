# Bản dịch tiếng Việt – Kiến trúc của hệ thống RAG

Dưới đây là bản dịch tiếng Việt được biên soạn lại thành **nội dung hoàn chỉnh, mạch lạc và tự nhiên**:

---

Bạn đã thấy những thành phần quan trọng nhất trong một hệ thống **RAG**, bao gồm chính **LLM**, một **knowledge base** chứa thông tin liên quan, và một **retriever** có khả năng tìm kiếm trong knowledge base đó.

Bây giờ, hãy cùng xem **kiến trúc tổng thể của một hệ thống RAG** và cách những thành phần này phối hợp với nhau.

Trước tiên, hãy bắt đầu bằng cách nhìn lại cách bạn thường sử dụng một mô hình ngôn ngữ lớn. Bạn gõ một prompt và gửi nó đến LLM. Sau đó, LLM xử lý prompt và tạo ra phản hồi.

Về mặt trải nghiệm người dùng, một hệ thống RAG thực ra **trông hoàn toàn giống như vậy**. Bạn vẫn gửi một prompt và vẫn nhận lại một câu trả lời.

Tuy nhiên, bên trong hệ thống sẽ có thêm một vài bước nữa.

Khi hệ thống RAG nhận được prompt của bạn, trước tiên nó sẽ chuyển prompt đó đến **retriever**. Retriever có quyền truy cập vào **knowledge base**, mà trên thực tế có thể hiểu đơn giản là một **cơ sở dữ liệu chứa các tài liệu hữu ích**.

Retriever sẽ truy vấn cơ sở dữ liệu này và trả về những tài liệu mà nó xác định là **liên quan nhất** đến prompt.

Tiếp theo, hệ thống sẽ tạo ra một **augmented prompt** — tức là một prompt đã được bổ sung — bằng cách đưa thông tin từ các tài liệu liên quan vào prompt ban đầu.

Ví dụ, một augmented prompt có thể có dạng như sau:

**“Hãy trả lời câu hỏi sau: Tại sao khách sạn ở Vancouver lại đắt như vậy vào cuối tuần này? Dưới đây là năm bài viết liên quan có thể giúp bạn trả lời.”**

Sau đó, hệ thống sẽ chèn phần nội dung trích từ các bài viết đó vào prompt.

Đến thời điểm này, hệ thống lại hoạt động giống như bất kỳ quy trình sử dụng LLM nào khác. Augmented prompt được gửi đến LLM, và LLM sẽ tạo ra phản hồi.

Lúc này, LLM có thể trả lời dựa trên **cả kiến thức đã học trong quá trình huấn luyện**, lẫn **ngữ cảnh bổ sung được cung cấp từ các tài liệu truy xuất được**.

Về phía người dùng, trải nghiệm gần như không thay đổi, có chăng chỉ là thêm một chút độ trễ. Bạn gõ prompt và nhận lại phản hồi.

Tuy nhiên, nhờ “đường vòng” qua retriever đó, khả năng cao là câu trả lời sẽ **chính xác hơn, cập nhật hơn và phù hợp ngữ cảnh hơn**.

Như bạn vừa thấy, khác biệt lớn nhất giữa việc sử dụng LLM trực tiếp và việc sử dụng một hệ thống RAG chính là **sự xuất hiện của retriever**.

Dù đây chỉ là một phần bổ sung khá đơn giản, nó lại mang đến rất nhiều lợi ích.

Trước hết, và cũng là quan trọng nhất, RAG giúp cung cấp cho LLM những thông tin mà nếu không có nó, mô hình sẽ không thể biết được. Đó có thể là **chính sách công ty, một mẩu thông tin cá nhân, hoặc tiêu đề tin tức sáng nay**. Trong nhiều trường hợp, RAG gần như là cách duy nhất để đưa những loại thông tin đó vào phạm vi hiểu biết của LLM.

Liên quan đến điểm này, RAG còn giúp **giảm khả năng hallucination** hoặc tạo ra những phản hồi gây hiểu lầm. Những vấn đề như vậy thường xảy ra khi LLM tạo phản hồi về những chủ đề không có trong dữ liệu huấn luyện, hoặc chỉ xuất hiện rất hiếm trong đó.

Khi bạn đưa trực tiếp thông tin liên quan vào prompt, phản hồi của mô hình ngôn ngữ sẽ được **neo chặt hơn vào dữ liệu thực tế**, từ đó giảm khả năng tạo ra các câu trả lời chung chung hoặc sai lệch.

RAG cũng giúp việc **cập nhật LLM với những thông tin thay đổi nhanh** trở nên dễ dàng hơn rất nhiều. Việc huấn luyện lại một mô hình ngôn ngữ thường rất tốn kém và mất thời gian, nên các LLM thường gặp khó khăn trong việc theo kịp những thông tin rất mới.

Trong một hệ thống RAG, bạn chỉ cần **cập nhật dữ liệu trong knowledge base**, giống như cách bạn cập nhật dữ liệu trong bất kỳ cơ sở dữ liệu nào khác. Ngay khi những thay đổi đó được **đánh chỉ mục (indexed)**, LLM đã có thể tạo phản hồi dựa trên thông tin mới.

Một lợi ích khác là RAG cải thiện khả năng **trích dẫn nguồn** của LLM. Hệ thống RAG có thể đưa thông tin trích dẫn vào augmented prompt, và LLM sau đó có thể đưa các thông tin nguồn đó vào phản hồi cuối cùng.

Điều này không chỉ giúp câu trả lời có cơ sở hơn, mà còn cho phép người đọc **kiểm tra lại, đào sâu thêm và xác thực nội dung được sinh ra**.

Cuối cùng, RAG cho phép LLM tập trung vào đúng thế mạnh của nó là **tạo sinh văn bản**.

Retriever sẽ lọc qua một thế giới thông tin rất rộng lớn, tìm ra những gì quan trọng và liên quan nhất, rồi trình bày chúng một cách ngắn gọn. LLM vẫn cần viết ra một phản hồi tốt, nhưng nó không còn phải gánh luôn cả nhiệm vụ **tự tìm kiếm sự thật** hay **lọc thông tin**.

Nói cách khác, mỗi thành phần trong hệ thống sẽ được giao đúng phần việc phù hợp nhất với thế mạnh của mình.

Đến cuối khóa học này, bạn sẽ được đi sâu vào cách xây dựng hệ thống RAG từ đầu. Nhưng trước hết, hãy xem một ví dụ mã nguồn rất đơn giản minh họa cách RAG hoạt động, trong đó hầu hết các chi tiết phức tạp đã được lược bỏ đi.

Giả sử tôi có một hàm **retrieve** và một hàm **generate**.

Hàm **retrieve** là một lớp bọc (wrapper) quanh retriever. Nó nhận vào một truy vấn văn bản và trả về những tài liệu liên quan từ knowledge base.

Hàm **generate** là một lớp bọc quanh mô hình ngôn ngữ lớn. Nó nhận vào một prompt dạng văn bản và trả về phản hồi do LLM tạo ra.

Bây giờ, tôi sẽ viết một prompt và lưu nó vào biến `prompt`. Nội dung của prompt là:

**“Tại sao giá khách sạn ở Vancouver lại cực kỳ đắt vào cuối tuần này?”**

Trước hết, hãy gửi prompt này trực tiếp đến LLM và xem nó trả về câu trả lời gì.

Sau đó, hãy xem retriever của chúng ta có thêm những thông tin gì liên quan đến prompt này.

Tiếp theo, chúng ta sẽ xây dựng một **augmented prompt** chứa cả **câu hỏi gốc của người dùng** lẫn **thông tin đã được truy xuất**.

Prompt mở rộng này có thể có dạng:

**“Hãy trả lời prompt sau. Sử dụng những thông tin được truy xuất dưới đây để hỗ trợ câu trả lời.”**

Sau đó, chúng ta chèn vào prompt gốc, rồi thêm các tài liệu đã được truy xuất.

Cuối cùng, hãy gửi augmented prompt này đến LLM.

Lúc này, LLM có thể đưa ra một câu trả lời chính xác hơn cho câu hỏi, bởi nó đã tích hợp được những thông tin vừa được lấy từ knowledge base.

Và đó thực sự chính là toàn bộ ý tưởng của RAG:

**Bổ sung thêm ngữ cảnh vào prompt để giúp LLM phản hồi chính xác hơn.**

Tất nhiên, đây mới chỉ là một cái nhìn rất tổng quan về kiến trúc RAG. Vì vậy, hãy cùng đến với video tiếp theo, nơi chúng ta sẽ bắt đầu phân tích từng thành phần một cách chi tiết hơn.
