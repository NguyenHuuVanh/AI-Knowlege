# Retriever Hoạt Động Như Thế Nào

## Mục đích của Retriever

Tại thời điểm này, mục đích của retriever có lẽ đã khá rõ ràng. Nó cần cung cấp **thông tin hữu ích** cho LLM — những thông tin có thể chưa có sẵn khi mô hình được huấn luyện.

Bây giờ hãy cùng xem thành phần này thực sự hoạt động như thế nào.

---

## Phép so sánh với thư viện

Hãy bắt đầu với một phép so sánh.

Hãy tưởng tượng bạn đang đến thăm một **thư viện** để trả lời câu hỏi: *"Làm thế nào để tôi có thể làm pizza kiểu New York tại nhà?"*

Tôi biết, tôi biết — bạn có thể chỉ cần tra cứu trên mạng. Và thành thật mà nói, tôi cũng vậy. Nhưng hãy kiên nhẫn với phép ẩn dụ này vì tôi nghĩ hình ảnh minh họa sẽ rất hữu ích.

Thư viện có một **bộ sưu tập sách lớn** về nhiều chủ đề khác nhau. Để giúp bạn duyệt qua bộ sưu tập, sách được sắp xếp theo **các khu vực và kệ** dựa trên đặc điểm của chủ đề, thể loại, tác giả, v.v.

Nếu bạn chia sẻ câu hỏi của mình với **thủ thư**, họ có thể giúp bạn tìm các khu vực trong thư viện hoặc thậm chí chính xác những cuốn sách liên quan nhất đến câu hỏi của bạn.

---

## Các thành phần tương tự của Retriever

Retriever có nhiều thành phần tương tự:

| Thư viện | Retriever |
|----------|-----------|
| Bộ sưu tập sách | **Knowledge base** (cơ sở tri thức) chứa các tài liệu |
| Hệ thống tổ chức sách theo kệ/khu vực | **Index** (chỉ mục) giữ tài liệu có tổ chức và dễ tìm kiếm |
| Thủ thư hiểu câu hỏi của bạn | Retriever **xử lý prompt** để hiểu ý nghĩa bên dưới |
| Tìm sách liên quan | **Tìm kiếm index** và trả về tài liệu phù hợp nhất |

---

## Quy trình truy xuất (Retrieval Process)

### Bước 1: Xử lý prompt
Trong thư viện, bạn có thể hỏi thủ thư trực tiếp. Thủ thư có khả năng **hiểu ý nghĩa** câu hỏi của bạn và biết phải tìm ở khu vực nào — ví dụ khu vực về nấu ăn, ẩm thực Ý, hoặc có thể là New York.

Khả năng **diễn giải ý nghĩa** câu hỏi cho phép họ xác định đúng kệ sách cần tìm kiếm và cuối cùng tìm ra những cuốn sách liên quan.

Bên trong hệ thống RAG, retriever làm điều tương tự. Nó đầu tiên cần **xử lý prompt** để hiểu ý nghĩa cơ bản của nó.

### Bước 2: Tìm kiếm index
Sau đó, nó sử dụng sự hiểu biết đó để **tìm kiếm trong chỉ mục** tài liệu.

### Bước 3: Trả về tài liệu liên quan
Retriever sau đó **trả về các tài liệu** từ knowledge base mà nó xác định là liên quan nhất đến prompt.

---

## Xếp hạng theo độ liên quan (Relevance Ranking)

Khi hoàn thành tìm kiếm, retriever **xếp hạng các tài liệu** trong knowledge base theo mức độ liên quan đến prompt.

- Mỗi tài liệu nhận được một **điểm số** (numerical score) để định lượng mức độ liên quan
- Thông thường, điểm số này đo lường **độ tương đồng** (similarity) giữa văn bản của prompt và văn bản của tài liệu
- Các tài liệu có **điểm cao nhất** là những tài liệu được trả về

> Có nhiều phương pháp khác nhau để tính điểm tương đồng này mà bạn sẽ tìm hiểu thêm trong khóa học.

---

## Thách thức: Cân bằng giữa đầy đủ và chính xác

Một retriever được thiết kế tốt không chỉ cần **trả về tài liệu liên quan**, mà còn phải **loại bỏ tài liệu không liên quan**.

### ❌ Trả về quá nhiều
Nếu bạn hỏi về cách làm pizza kiểu New York tại nhà và retriever trả về **tất cả tài liệu** trong knowledge base — về mặt kỹ thuật, bạn sẽ có mọi tài liệu liên quan, nhưng chúng sẽ bị chìm trong **núi thông tin không liên quan**.

Điều này cũng dẫn đến:
- Prompt **tốn kém chi phí**
- Hoặc thậm chí **sử dụng hết** context window của LLM

### ❌ Trả về quá ít
Mặt khác, nếu bạn chỉ truy xuất tài liệu được xếp hạng **cao nhất**, bạn có thể **bỏ lỡ** thông tin có giá trị từ các tài liệu xếp hạng thứ 2, thứ 3, hoặc thứ 4.

### ✅ Trường hợp lý tưởng
Trong thế giới hoàn hảo, retriever **xếp hạng chính xác** các tài liệu và chọn đúng số lượng để trả về. Tuy nhiên, trên thực tế, retriever đôi khi sẽ:
- Xếp hạng một số tài liệu liên quan **quá thấp**
- Xếp hạng một số tài liệu không liên quan **quá cao**

→ Khiến việc quyết định trả về bao nhiêu tài liệu trở nên **khó khăn**.

### 🔧 Tối ưu hóa
Để tối ưu hiệu suất của retriever, bạn cần:
- **Theo dõi** nó theo thời gian
- **Thử nghiệm** với các cài đặt khác nhau

---

## So sánh với các công nghệ quen thuộc

Nhiều phần mềm quen thuộc thực hiện các tác vụ rất tương tự với retriever:

| Công nghệ | Chức năng tương tự |
|-----------|-------------------|
| **Công cụ tìm kiếm web** | Truy xuất các trang web liên quan đến truy vấn tìm kiếm |
| **Cơ sở dữ liệu quan hệ** | Truy xuất các hàng và bảng khớp với truy vấn SQL |

Lĩnh vực **Information Retrieval** (truy xuất thông tin) rộng hơn đã trưởng thành từ trước khi các mô hình ngôn ngữ lớn được phát triển, nhưng các ý tưởng từ lĩnh vực này là **nền tảng** cho cách retriever và hệ thống RAG được thiết kế.

---

## Vector Database — Nền tảng của Retriever

Về lý thuyết, có rất nhiều cách để triển khai retriever trong hệ thống RAG. Vì hầu hết các công ty đều đã có dữ liệu trong **cơ sở dữ liệu quan hệ truyền thống**, sẽ rất tiện nếu giữ dữ liệu ở đó và tìm cách truy xuất từ cơ sở dữ liệu đó để hỗ trợ hệ thống RAG.

Tuy nhiên, mặc dù không hoàn toàn bắt buộc, ở quy mô lớn, hầu hết retriever sẽ được xây dựng trên nền tảng **Vector Database** — một loại cơ sở dữ liệu chuyên biệt được tối ưu hóa để **nhanh chóng tìm kiếm** các tài liệu trong knowledge base khớp nhất với prompt.

> **Vector Database**: Cơ sở dữ liệu chuyên biệt, được tối ưu hóa cho việc tìm kiếm nhanh các tài liệu tương đồng nhất với truy vấn đầu vào.

---

## Tổng kết

Trong khóa học này, bạn sẽ học về cả:
1. **Nguyên lý chung của Information Retrieval** — hỗ trợ nhiều công nghệ tìm kiếm khác nhau
2. **Vector Database** — thường được sử dụng làm retriever bên trong hệ thống RAG ở quy mô production

---

## Sơ đồ tổng quan

```
┌─────────────────────────────────────────────────┐
│                  RAG System                      │
│                                                  │
│  ┌──────────┐    ┌────────────┐    ┌──────────┐ │
│  │  Prompt   │───▶│  Retriever │───▶│   LLM    │ │
│  └──────────┘    └─────┬──────┘    └──────────┘ │
│                        │                         │
│                  ┌─────▼──────┐                  │
│                  │   Index    │                  │
│                  └─────┬──────┘                  │
│                        │                         │
│                  ┌─────▼──────┐                  │
│                  │ Knowledge  │                  │
│                  │   Base     │                  │
│                  │ (Vector DB)│                  │
│                  └────────────┘                  │
└─────────────────────────────────────────────────┘
```
