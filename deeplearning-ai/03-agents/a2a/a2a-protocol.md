# 🔗 A2A - Agent-to-Agent Protocol

## Tổng quan
**A2A (Agent-to-Agent)** là một protocol mở do **Google** phát triển, cho phép các AI agent giao tiếp và phối hợp với nhau một cách chuẩn hóa, bất kể framework hay nhà cung cấp nào.

## Tại sao cần A2A?
- Các agent hiện tại hoạt động trong "silos" riêng biệt
- Không có chuẩn chung để agent từ các hệ thống khác nhau giao tiếp
- A2A giải quyết vấn đề **interoperability** giữa các agent

## Kiến trúc A2A

### Các khái niệm chính
- **Agent Card** - Mô tả khả năng của agent (như "resume" của agent)
- **Task** - Đơn vị công việc giữa client và remote agent
- **Message** - Giao tiếp giữa các agent trong một task
- **Artifact** - Kết quả/output của task
- **Streaming** - Hỗ trợ real-time updates qua SSE

### Luồng hoạt động
```
Client Agent                    Remote Agent
    |                               |
    |--- Discover (Agent Card) ---->|
    |<-- Agent Card Response -------|
    |                               |
    |--- Send Task --------------->|
    |<-- Task Status/Updates -------|
    |<-- Artifacts (results) -------|
    |                               |
```

### So sánh A2A vs MCP
| Đặc điểm | A2A | MCP |
|-----------|-----|-----|
| Mục đích | Agent ↔ Agent | Agent ↔ Tools/Data |
| Phạm vi | Giao tiếp giữa agents | Cung cấp context cho agent |
| Nhà phát triển | Google | Anthropic |
| Quan hệ | Bổ sung cho MCP | Bổ sung cho A2A |

## 📎 Tài liệu
- [A2A GitHub](https://github.com/google/A2A)
- [A2A Specification](https://google.github.io/A2A/)
- [Google Blog - A2A](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/)

---
*Thêm ghi chú chi tiết vào thư mục `notes/` bên dưới.*
