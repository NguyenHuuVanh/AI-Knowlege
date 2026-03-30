# Bản dịch tiếng Việt – Giới thiệu về RAG

Dưới đây là bản dịch tiếng Việt được biên soạn lại thành **nội dung hoàn chỉnh, mạch lạc và tự nhiên**:

---

Các **LLMs** là những công cụ rất đáng kinh ngạc. Chúng có thể trả lời câu hỏi, tóm tắt và viết lại văn bản, đưa ra phản hồi cho tài liệu, tạo mã lập trình và còn làm được nhiều việc khác nữa. Chỉ vài năm trước, những dạng tác vụ như vậy dường như vẫn nằm ngoài khả năng của máy tính. Vì thế, khi tương tác với một mô hình như vậy, bạn có thể có cảm giác như đang làm việc với một con người thực thụ.

**RAG** là một phương pháp giúp cải thiện hơn nữa hiệu năng của các mô hình ngôn ngữ lớn bằng cách cho chúng quyền truy cập vào những thông tin mà chúng không biết từ quá trình huấn luyện. Để làm rõ ý tưởng này, hãy cùng xem qua một vài ví dụ.

Giả sử tôi hỏi bạn: **“Tại sao khách sạn thường đắt vào cuối tuần?”**  
Có lẽ bạn có thể trả lời ngay câu hỏi đó. Nhiều người đi du lịch vào cuối tuần hơn, vì vậy mức độ cạnh tranh để đặt phòng cũng cao hơn.

Bây giờ, giả sử tôi hỏi tiếp: **“Tại sao khách sạn ở Vancouver lại đặc biệt đắt vào cuối tuần này?”**  
Lúc này, bạn sẽ cần thêm thông tin để có thể trả lời. Nếu bạn tìm kiếm trên mạng, bạn có thể phát hiện ra rằng siêu sao quốc tế **Taylor Swift** đang có mặt tại thành phố này vào cuối tuần để tổ chức hai đêm diễn liên tiếp. Với thông tin bổ sung đó, bạn lại có thể đưa ra câu trả lời hợp lý.

Cuối cùng, giả sử tôi hỏi: **“Tại sao Vancouver lại không có nhiều khách sạn gần khu trung tâm hơn?”**  
Để trả lời câu hỏi này, có lẽ bạn sẽ phải nghiên cứu khá sâu về lịch sử phát triển của Vancouver, về quy hoạch đô thị nói chung và nhiều yếu tố liên quan khác. Nói cách khác, bạn sẽ cần quyền truy cập vào một lượng lớn thông tin chuyên biệt.

Bạn có thể hình dung cách mình trả lời những câu hỏi trên gồm **hai giai đoạn**.  
Trước hết, bạn thu thập những thông tin cần thiết. Sau đó, bạn suy luận dựa trên những thông tin đó để xây dựng câu trả lời.

Như ở câu hỏi đầu tiên, đôi khi bạn **không cần thu thập thêm thông tin nào cả**. Dựa trên hiểu biết sẵn có về thế giới, bạn đã có thể trả lời ngay lập tức. Nhưng trong những trường hợp khác, bạn có thể cần thu thập một ít, hoặc thậm chí rất nhiều thông tin.

Trong RAG, quá trình thu thập thông tin hữu ích này được gọi là **retrieval**.  
Còn quá trình suy luận trên thông tin đó và tạo ra phản hồi được gọi là **generation**.

Các mô hình ngôn ngữ lớn được hưởng lợi từ giai đoạn retrieval cũng vì những lý do gần giống như con người. Bạn sẽ tìm hiểu nhiều hơn về LLM trong suốt khóa học này. Nhưng ở thời điểm hiện tại, bạn có thể hình dung chúng giống như một người có vốn hiểu biết phổ thông rất rộng, được tích lũy từ việc “đọc” một lượng khổng lồ nội dung trên Internet.

Khi bạn đưa ra một prompt cho LLM, nó sẽ dựa vào vốn tri thức đó để tạo ra phản hồi. Với nhiều loại prompt, cách làm này hoạt động rất tốt. Tuy nhiên, trong những trường hợp khác, LLM lại không biết đủ thông tin để trả lời chính xác. Prompt có thể liên quan đến một sự kiện rất mới, hoặc một loại thông tin chuyên môn mà mô hình chưa từng thấy trước đây.

Cũng giống như con người, sẽ là không hợp lý nếu mong đợi LLM trở thành chuyên gia trong mọi lĩnh vực. Và chúng sẽ đưa ra phản hồi tốt hơn rất nhiều khi được tiếp cận với thông tin tốt hơn. Việc nhận ra rằng LLM cũng cần được hỗ trợ bởi một giai đoạn retrieval chính là ý tưởng cốt lõi của RAG.

Dĩ nhiên, LLM không phải là những con người chỉ ngồi đọc Wikipedia cả ngày. Thay vào đó, chúng là **các mô hình toán học** được huấn luyện trên những tập dữ liệu khổng lồ thu thập từ khắp nơi trên Internet công khai. Trong quá trình huấn luyện, mô hình học được bất kỳ thông tin nào có trong dữ liệu huấn luyện đó.

Khi bạn gửi một prompt tới LLM, về bản chất bạn đang hy vọng rằng thông tin liên quan đến câu hỏi của mình đã từng xuất hiện trong dữ liệu huấn luyện, lý tưởng nhất là xuất hiện nhiều lần. Đáng tiếc là có rất nhiều loại thông tin sẽ **không** nằm trong dữ liệu đó. Các công ty có cơ sở dữ liệu riêng tư. Một số thông tin bị ẩn hoặc rất khó tiếp cận. Và với việc tin tức mới được xuất bản từng phút từng giây, sẽ luôn tồn tại những thông tin ngoài kia mà LLM chưa từng được huấn luyện trên đó.

Vậy câu hỏi đặt ra là: **làm sao để đảm bảo LLM biết được những thông tin hữu ích này?**

Câu trả lời ngắn gọn là: **hãy đưa chúng vào prompt**.

Ý tưởng cốt lõi của một hệ thống RAG là bạn có thể **chỉnh sửa prompt trước khi gửi nó đến mô hình ngôn ngữ lớn**. Ngoài câu hỏi gốc của người dùng, bạn có thể bổ sung thêm những thông tin giúp LLM trả lời tốt hơn.

Ví dụ, nếu bạn hỏi một hệ thống RAG: **“Tại sao khách sạn ở Vancouver lại đặc biệt đắt vào cuối tuần này?”** thì trước tiên hệ thống sẽ thực hiện một bước **retrieval** để thu thập những thông tin liên quan. Sau đó, mô hình ngôn ngữ sẽ nhận được một **augmented prompt** — tức là một prompt đã được bổ sung, bao gồm cả câu hỏi gốc lẫn các thông tin vừa được truy xuất.

Lúc này, LLM đã có những thông tin cần thiết để trả lời một cách chính xác hơn.

Dĩ nhiên, những thông tin đó phải được truy xuất từ đâu đó. Thành phần của hệ thống RAG chịu trách nhiệm cho quá trình này được gọi là **retriever**.

Retriever quản lý một **knowledge base** gồm những thông tin đáng tin cậy, có liên quan và có thể là dữ liệu riêng tư. Khi hệ thống RAG nhận được một prompt, retriever sẽ tìm và lấy ra những thông tin phù hợp nhất từ knowledge base để chia sẻ với LLM. Sau đó, mô hình sẽ sử dụng những thông tin được truy xuất đó khi tạo phản hồi cho prompt.

Cái tên **Retrieval Augmented Generation** — dù đúng là hơi dài — giờ đây có lẽ đã trở nên dễ hiểu hơn. Về cơ bản, bạn chỉ đang **cải thiện hoặc tăng cường cách mà LLM tạo văn bản**, bằng cách trước tiên truy xuất những thông tin phù hợp từ một knowledge base.

Đó là mô tả ở mức tổng quan về cách RAG hoạt động. Tuy nhiên, đôi khi sẽ hữu ích hơn nếu được nhìn thấy các ứng dụng thực tế. Vì vậy, hãy cùng đến với video tiếp theo, nơi giảng viên sẽ trình bày một số cách mà RAG đang được sử dụng ngay từ hôm nay.
