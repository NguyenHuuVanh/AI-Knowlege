# Bản dịch tiếng Việt – Thành phần LLM trong hệ thống RAG

Dưới đây là bản dịch tiếng Việt được biên soạn lại thành **nội dung hoàn chỉnh, mạch lạc và tự nhiên** (không giữ mốc thời gian trong video):

---

Sau khi đã xem kiến trúc tổng quan của một hệ thống RAG, bây giờ hãy nhìn kỹ từng thành phần — bắt đầu từ **mô hình ngôn ngữ lớn (Large Language Model – LLM)**.

Hiểu LLM vận hành như thế nào, điểm mạnh và giới hạn của nó sẽ giúp bạn thấy vì sao các thành phần còn lại trong RAG được thiết kế để **bù đắp** và **cải thiện** hiệu năng của LLM.

## 1) LLM thực chất làm gì?

LLM đôi khi được gọi đùa là “**tự động hoàn thành câu** (fancy autocomplete)”, và thật ra mô tả này khá đúng.

Ở mức đơn giản nhất, LLM chỉ làm một việc: **dự đoán token tiếp theo** nên xuất hiện trong một đoạn văn bản.

Ví dụ, nếu bạn thấy cụm từ chưa hoàn chỉnh:

“Thật là một ngày đẹp trời, mặt trời đang …”

Bạn và mình có thể đoán từ nào hợp lý để hoàn thành câu. Một đáp án tự nhiên có thể là “**tỏa sáng**”, nhưng “**đang lên**” hoặc “**đang rực rỡ**” cũng có thể đúng tùy ngữ cảnh.

- Cụm từ ban đầu được gọi là **prompt**.
- Mỗi phiên bản hoàn chỉnh mà mô hình tạo ra được gọi là **completion**.

Mô hình cũng có thể tạo ra một completion kiểu như “mặt trời đang **phát nổ**”. Câu này vẫn đúng ngữ pháp, nhưng bạn sẽ thấy nó **không hợp lý** vì xác suất xảy ra thấp trong bối cảnh “ngày đẹp trời”.

## 2) Token là gì?

Về mặt kỹ thuật, LLM không tạo ra “từ” (word) mà tạo ra **token** — những mảnh nhỏ của từ.

- Một số từ đơn giản (ví dụ như tên riêng hoặc các từ phổ biến) có thể tương ứng với **một token**.
- Nhiều từ dài hoặc từ ghép thường được tách thành **nhiều token**.
- Dấu câu như dấu chấm, phẩy, dấu hỏi… cũng có thể là token riêng.

Hầu hết LLM có “từ vựng token” (token vocabulary) cỡ **10 nghìn đến hơn 100 nghìn token**.

Việc dùng token (mảnh từ) giúp mô hình có thể “lắp ghép” để biểu diễn rất nhiều từ khác nhau mà không cần gán token cho từng từ riêng lẻ.

## 3) LLM sinh văn bản như thế nào?

Khi LLM tạo một completion, nó chỉ đang **nối thêm token vào cuối prompt**, từng token một.

Ví dụ, với câu “Thật là một ngày đẹp trời, mặt trời đang …”, mô hình có thể lần lượt thêm:

“tỏa” → “sáng” → “trên” → “bầu” → “trời” …

và đến một thời điểm nào đó, mô hình sẽ dừng và trả completion cho người dùng.

Trước mỗi token mới, LLM thường làm quy trình như sau:

1. **Xử lý toàn bộ ngữ cảnh hiện tại** (prompt + các token đã sinh), từ đó “hiểu sâu” mối quan hệ giữa các từ/token và ý nghĩa tổng thể.
2. Với **mọi token trong từ vựng** (thường là hàng chục đến hàng trăm nghìn token), mô hình ước lượng **xác suất** token đó là token tiếp theo.
3. Kết quả là một **phân phối xác suất** trên toàn bộ từ vựng.
4. Mô hình **lấy mẫu (sample) một token** theo phân phối xác suất đó.

Trong ví dụ trên, token tương ứng với “tỏa sáng” có thể có xác suất cao nhất, nhưng các lựa chọn ít hợp lý hơn như “phát nổ” vẫn có **một xác suất rất nhỏ**.

Điều này giải thích vì sao chạy cùng một prompt nhiều lần có thể cho ra **các completion khác nhau**.

## 4) Tính tự hồi quy (autoregressive)

LLM là mô hình **tự hồi quy** (autoregressive), nghĩa là:

- Token sinh ra ở bước trước sẽ trở thành một phần của ngữ cảnh
- và sẽ **ảnh hưởng** đến token mà mô hình chọn ở các bước sau.

Vì thế, khi mô hình đã chọn một hướng diễn đạt ban đầu, nó thường sẽ “đi tiếp” theo hướng đó.

## 5) LLM học “ngôn ngữ” từ đâu?

LLM được huấn luyện trên các bộ dữ liệu văn bản rất lớn.

Bản thân mô hình là một mạng nơ-ron khổng lồ với **hàng tỷ tham số** (các trọng số số học). Trước khi huấn luyện, nó sẽ tạo ra văn bản vô nghĩa.

Trong quá trình huấn luyện:

- Mô hình được đưa vào các đoạn văn bản **chưa hoàn chỉnh**
- cố gắng đoán token tiếp theo
- rồi cập nhật tham số dựa trên độ đúng/sai của dự đoán.

Nhờ vậy, mô hình học được cả:

- **thông tin thực tế** (đến mức độ dữ liệu có chứa)
- lẫn **phong cách ngôn ngữ** và các “mẫu” diễn đạt.

Nhiều LLM hiện nay được huấn luyện trên **hàng nghìn tỷ token**, phần lớn đến từ Internet. Vì vậy, chúng có thể viết về rất nhiều chủ đề và theo nhiều phong cách khác nhau.

## 6) Vì sao LLM bị “ảo giác” (hallucination)?

LLM chỉ tối ưu cho việc sinh ra chuỗi token **có vẻ hợp lý** dựa trên những mẫu nó học được.

Nếu bạn hỏi về:

- dữ liệu nội bộ của công ty
- tin tức mới nhất hôm nay
- thông tin riêng tư mà mô hình chưa từng thấy

thì khả năng cao mô hình **không có** dữ liệu đó trong quá trình huấn luyện. Khi đó, nó có thể tạo ra câu trả lời nghe “rất thuyết phục” nhưng thực tế lại **sai**.

Dù gọi là “hallucination”, điều quan trọng là nhớ rằng mô hình không “mất kiểm soát” theo nghĩa tâm lý; nó đang làm đúng nhiệm vụ: **tạo văn bản có xác suất cao**, không phải “tạo văn bản đúng sự thật”.

Với LLM, “đúng” thường gần với “**có khả năng đúng** theo phân phối học từ dữ liệu”. Nếu dữ liệu huấn luyện chất lượng cao, trực giác về “đúng/sai” của con người và “hợp lý” theo xác suất của mô hình có thể tương đối khớp nhau. Nhưng chúng không phải lúc nào cũng trùng.

## 7) RAG giải quyết vấn đề này như thế nào?

RAG tận dụng một khả năng rất mạnh của LLM: **hiểu và sử dụng ngữ cảnh trong prompt**.

Nếu hệ thống RAG bổ sung thông tin liên quan vào prompt, LLM có thể **đọc, hiểu và dùng** thông tin đó để trả lời — ngay cả khi thông tin ấy **không nằm trong dữ liệu huấn luyện**.

Người ta thường nói các thông tin được thêm vào prompt sẽ giúp **ground** (neo/đặt nền) câu trả lời, tức là làm cho câu trả lời bám vào dữ liệu thực tế hơn.

## 8) Vì sao không “nhét” thật nhiều thông tin vào prompt?

Trực giác là: “càng nhiều thông tin liên quan càng tốt”, nhưng thực tế có hai giới hạn lớn:

1. **Prompt dài hơn → tốn chi phí tính toán hơn**

Trước khi sinh mỗi token mới, mô hình cần thực hiện các phép tính phức tạp dựa trên toàn bộ chuỗi token hiện có. Chuỗi càng dài, chi phí càng tăng.

2. **Giới hạn context window**

Mỗi LLM có một **cửa sổ ngữ cảnh** (context window) — độ dài tối đa của chuỗi token mà nó có thể xử lý trong một lần.

- Các mô hình cũ có thể chỉ vài nghìn token.
- Một số mô hình mới có thể xử lý tới rất lớn (thậm chí hàng triệu token, tùy mô hình).

Khi retriever thêm quá nhiều thông tin vào augmented prompt, bạn sẽ vừa làm tăng chi phí, vừa có nguy cơ **chạm trần context window**.

## 9) Ghi chú về nhà cung cấp mô hình

Có rất nhiều nhà cung cấp LLM. Trong khóa học này, bạn sẽ dùng **TogetherAI**, nơi lưu trữ nhiều mô hình mã nguồn mở phổ biến.

Dùng mô hình mã nguồn mở giúp bạn dễ “mở nắp” quan sát bên trong và thực hành nhiều khái niệm quan trọng.

---

Tóm lại, ý chính cần nhớ là: **LLM có thể tích hợp thông tin nằm trong prompt vào câu trả lời**, kể cả khi thông tin đó không có trong dữ liệu huấn luyện.

Vì vậy, bước tiếp theo là chuyển sang **retriever** — thành phần chịu trách nhiệm tìm và đưa đúng thông tin vào prompt để tận dụng điểm mạnh này của LLM.
