# 🤝 AI Agents & Multi-Agent Systems

## Tổng quan
Kiến thức về AI Agents - các hệ thống AI tự chủ có khả năng lập kế hoạch, sử dụng công cụ, và phối hợp với nhau.

## Thư mục con

| Thư mục | Mô tả |
|---------|-------|
| `a2a/` | **Agent-to-Agent Protocol** - Protocol giao tiếp giữa các agent (Google) |
| `agent-teams/` | **Multi-Agent Teams** - Hệ thống nhiều agent phối hợp |
| `mcp/` | **Model Context Protocol** - Protocol cung cấp context cho AI (Anthropic) |
| `frameworks/` | **Agent Frameworks** - Các framework xây dựng agent |

## Khái niệm cốt lõi

### Agent là gì?
Một AI Agent là hệ thống sử dụng LLM làm "bộ não" để:
1. **Perceive** - Nhận thông tin từ môi trường
2. **Plan** - Lập kế hoạch hành động
3. **Act** - Thực thi hành động (gọi tools, APIs)
4. **Reflect** - Đánh giá kết quả và điều chỉnh

### Các thành phần chính
- **LLM Core** - Mô hình ngôn ngữ làm bộ xử lý trung tâm
- **Memory** - Bộ nhớ ngắn hạn (conversation) và dài hạn (vector DB)
- **Tools** - Các công cụ agent có thể sử dụng (API, code, search...)
- **Planning** - Khả năng phân tích và lập kế hoạch (ReAct, CoT, ToT)

### Protocols quan trọng
- **A2A (Agent-to-Agent)** - Cho phép agent giao tiếp với nhau
- **MCP (Model Context Protocol)** - Chuẩn hóa cách cung cấp context/tools cho AI

## 📎 Tài liệu tham khảo
- [Google A2A](https://github.com/google/A2A)
- [Anthropic MCP](https://modelcontextprotocol.io)
- [LLM Agents Survey](https://arxiv.org/abs/2308.11432)

---
*Xem từng thư mục con để tìm hiểu chi tiết.*
