# 🔌 MCP - Model Context Protocol

## Tổng quan
**MCP (Model Context Protocol)** là một protocol mở do **Anthropic** phát triển, chuẩn hóa cách các ứng dụng AI kết nối với nguồn dữ liệu và công cụ bên ngoài.

## MCP là gì?
- Giống như "USB-C cho AI" - một cổng kết nối chuẩn
- Cho phép AI models truy cập tools, data, và services
- Thay thế các custom integrations riêng lẻ bằng một protocol chung

## Kiến trúc MCP

### Các thành phần
```
┌─────────────┐     ┌──────────────┐     ┌──────────────┐
│  MCP Host   │────▶│  MCP Client  │────▶│  MCP Server  │
│ (AI App)    │     │ (Protocol)   │     │ (Tools/Data) │
└─────────────┘     └──────────────┘     └──────────────┘
```

- **Host** - Ứng dụng AI (Claude Desktop, IDE, chatbot...)
- **Client** - Quản lý kết nối với MCP servers
- **Server** - Cung cấp tools, resources, prompts

### MCP Server cung cấp gì?
1. **Tools** - Các hàm/API mà AI có thể gọi
2. **Resources** - Dữ liệu mà AI có thể đọc (files, DB, APIs)
3. **Prompts** - Các prompt templates có sẵn

## Ví dụ MCP Servers
- **Filesystem** - Đọc/ghi files
- **GitHub** - Tương tác với repos, issues, PRs
- **Slack** - Gửi/đọc messages
- **Database** - Query databases
- **Browser** - Điều khiển trình duyệt
- **Google Drive** - Truy cập tài liệu

## So sánh MCP vs Function Calling
| Đặc điểm | MCP | Function Calling |
|-----------|-----|-----------------|
| Chuẩn hóa | Protocol chung | Mỗi provider khác nhau |
| Discovery | Server tự mô tả tools | Phải define trước |
| Mở rộng | Thêm server mới dễ dàng | Phải code integration |
| Community | Ecosystem servers | Tự phát triển |

## 📎 Tài liệu
- [MCP Official](https://modelcontextprotocol.io)
- [MCP Specification](https://spec.modelcontextprotocol.io)
- [MCP Servers Collection](https://github.com/modelcontextprotocol/servers)
- [MCP TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)

---
*Thêm ghi chú vào thư mục `notes/` bên dưới.*
