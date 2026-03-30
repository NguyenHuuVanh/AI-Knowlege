# Sổ tay kiến thức về OpenClaw

Tài liệu này tổng hợp toàn bộ các kiến thức, lệnh thao tác và kinh nghiệm khi cài đặt, cấu hình và sử dụng **OpenClaw** (Framework tự động hóa Agentic AI).

## Mục lục (Menu)
1. [Tổng quan về OpenClaw](#1-tổng-quan-về-openclaw)
2. [Đường dẫn và File cấu hình quan trọng](#2-đường-dẫn-và-file-cấu-hình-quan-trọng)
3. [Các lệnh (Commands) thường dùng nhất](#3-các-lệnh-commands-thường-dùng-nhất)
   - [3.1. Quản lý Gateway (Trái tim của hệ thống)](#31-quản-lý-gateway-trái-tim-của-hệ-thống)
   - [3.2. Quản lý Plugins & Channels (Zalo)](#32-quản-lý-plugins--channels-zalo)
   - [3.3. Quản lý AI Models & Trí tuệ cho Bot](#33-quản-lý-ai-models--trí-tuệ-cho-bot)
   - [3.4. Quản lý Danh bạ & Gửi tin nhắn test](#34-quản-lý-danh-bạ--gửi-tin-nhắn-test)
   - [3.5. Debug lỗi (Bắt bệnh)](#35-debug-lỗi-bắt-bệnh)
4. [Các lỗi phổ biến đã gặp & Cách xử lý](#4-các-lỗi-phổ-biến-đã-gặp--cách-xử-lý)
5. [Giới hạn người chat & Tắt/Bật Bot tự động phản hồi](#5-giới-hạn-người-chat--tắtbật-bot-tự-động-phản-hồi)
   - [5.1. Tắt hoàn toàn Bot (Không tự động trả lời ai cả)](#51-tắt-hoàn-toàn-bot-không-tự-động-trả-lời-ai-cả)
   - [5.2. Chỉ cho phép Bot trả lời một (hoặc vài) người cụ thể](#52-chỉ-cho-phép-bot-trả-lời-một-hoặc-vài-người-cụ-thể)
   - [5.3. Chỉ cho phép trả lời trong một "Cộng đồng" / "Nhóm" (Group)](#53-chỉ-cho-phép-trả-lời-trong-một-cộng-đồng--nhóm-group)
6. [Mở rộng: Kết nối Bot vào các nền tảng khác (Telegram, Discord, Gmail, Slack...)](#6-mở-rộng-kết-nối-bot-vào-các-nền-tảng-khác-telegram-discord-gmail-slack)

---

## 1. Tổng quan về OpenClaw
OpenClaw là một framework giúp kết nối các AI Models (như GPT-4, OpenAI Codex, Gemini, Github Copilot) với các kênh nhắn tin (Zalo, Telegram, WhatsApp) thông qua một "Gateway" (Cổng giao tiếp).

Các thành phần chính:
- **CLI (`openclaw`)**: Dòng lệnh chính để thao tác cấu hình, gọi bot.
- **Gateway**: Service chạy nền (thường dùng port 18789), đóng vai trò "trái tim" kết nối giữa kênh nhắn tin (Zalo) và AI Agent. Mọi luồng tin nhắn đều đi qua Gateway.
- **Channels**: Kênh giao tiếp với bên ngoài (ví dụ plugin `zalouser` để dùng Zalo personal).
- **Models / Auth**: API / AI Model dùng để trả lời tin nhắn (nằm trong thư mục `.openclaw/agents/main/agent/...`).

---

## 2. Đường dẫn và File cấu hình quan trọng
OpenClaw lưu trữ toàn bộ data ở thư mục `~/.openclaw` (trên Linux/WSL) hoặc `C:\Users\<Tên_User>\.openclaw` (trên Windows).

* **File Cấu hình chính**: `~/.openclaw/openclaw.json` (Chứa thông tin bật tắt gateway, plugin, channel policy, model mặc định).
* **Workspace Bot**: `~/.openclaw/workspace` (Nơi bot AI có thể tạo, chỉnh sửa file).
* **Cấu hình Model cho Agent**: `~/.openclaw/agents/main/agent/agent.json` (hoặc cấu hình nằm thẳng trong `openclaw.json` ở key `agents.defaults`).
* **Lưu phiên đăng nhập (Sessions)**: `~/.openclaw/agents/main/sessions/`

⚠️ **Lưu ý cực kỳ quan trọng về WSL và Windows**:
OpenClaw chạy qua Node.js. Nếu bạn chạy CLI `openclaw` ở Windows CMD/PowerShell, nó sẽ dùng config của Windows (ổ C:). Nếu bạn chạy bên trong WSL (Ubuntu bash), nó sẽ dùng config của WSL (`/home/...`). **Nên đồng nhất chạy toàn bộ lệnh gateway và login bên trong WSL** để tránh lỗi không match token hay không thấy QR code.

---

## 3. Các lệnh (Commands) thường dùng nhất

### 3.1. Quản lý Gateway (Trái tim của hệ thống)
Gateway bắt buộc phải chạy thì bot mới nhận được tin nhắn và phản hồi.
* Khởi động gateway: `openclaw gateway run`
* Khởi động đổi cổng/ép buộc khi bị kẹt: `openclaw gateway run --force --allow-unconfigured`
* Khởi động lại (restart) gateway: `openclaw gateway restart`
* Kiểm tra gateway API: `openclaw health`

### 3.2. Quản lý Plugins & Channels (Zalo)
* Xem danh sách plugins (đang bật/tắt): `openclaw plugins list`
* Bật plugin Zalo cá nhân: `openclaw plugins enable zalouser`
* **Đăng nhập Zalo**: `openclaw channels login --channel zalouser` (Sẽ tạo ra mã QR dạng file `.png` ở `/tmp/...` để mở lên và lấy app Zalo quét).
* Kiểm tra trạng thái kênh (xem đã login thành công chưa): `openclaw channels status --probe`

### 3.3. Quản lý AI Models & Trí tuệ cho Bot
* Xem danh sách Model: `openclaw models list`
* Xem trạng thái/Quota của model: `openclaw models status`
* Bật GitHub Copilot (Không tốn key API riêng): 
  1. `openclaw plugins enable copilot-proxy`
  2. `openclaw models auth login-github-copilot` (Lấy mã nhập vào trình duyệt web).
* Set model mặc định cho bot: `openclaw config set agents.defaults.model 'github-copilot/gpt-4o'`

### 3.4. Quản lý Danh bạ & Gửi tin nhắn test
* Xem thông tin cá nhân (Zalo của bạn): `openclaw directory self --channel zalouser`
* Tìm ID bạn bè trên Zalo: `openclaw directory peers list --channel zalouser --query "Tên"` (Có thể không dấu/Việt key do CLI bị lỗi font).
* Gửi tin nhắn Bot: `openclaw message send --channel zalouser --target <ID_Người_nhận> --message "Tin nhắn"`

### 3.5. Debug lỗi (Bắt bệnh)
* `openclaw doctor` : Quét toàn bộ hệ thống để tìm API Key thiếu, Plugin lỗi.
* `openclaw logs --follow` : Theo dõi console log thời gian thực khi có ai đó nhắn tin cho Bot.

---

## 4. Các lỗi phổ biến đã gặp & Cách xử lý

1. **Lỗi `unauthorized: gateway token missing/mismatch`**
   * **Nguyên nhân**: CLI gọi gateway sai môi trường (Ví dụ CLI trên Windows gọi Gateway đang chạy trên WSL).
   * **Fix**: Giết Gateway cũ và Start lại bằng lệnh có `--force` từ phía WSL. Đồng nhất dùng WSL.

2. **Lỗi `no configured channels detected` hoặc `Unsupported channel: zalouser`**
   * **Nguyên nhân**: Plugin `zalouser` (Zalo) bị tắt mặc định hoặc chưa cài các thư viện phụ thuộc (`zca-js`).
   * **Fix**: Vào thư mục chứa plugin `npm install` các packages, sau đó chạy `openclaw plugins enable zalouser`.

3. Lỗi `Agent failed before reply: No API key found for provider "openai-codex"`
   * **Nguyên nhân**: Dù bạn đã trỏ Bot dùng thư viện Copilot (miễn phí), file config `openclaw.json` (mục `agents.defaults.models`) vẫn còn lưu tên model `openai-codex/...` và nó cố gắng fallback sang mô hình cũ.
   * **Fix**: Viết một script python nhỏ hoặc vào file `~/.openclaw/openclaw.json` XÓA thủ công block `openai-codex` ở bên dưới node `models`, chỉ giữ lại `github-copilot`. Xong xóa sạch config rác đó thì nhớ restart gateway.

4. Bot đã reply nhưng không đọc được chữ Tiếng Việt ở console WSL
   * Là do Encoding UTF-8 của PowerShell/WSL không match. Có thể parse trực tiếp đầu ra json bằng Python `openclaw ... --json | python3 -c "import json..."` để đọc.

---

## 5. Giới hạn người chat & Tắt/Bật Bot tự động phản hồi

Mặc định, chính sách tin nhắn (DM Policy) của OpenClaw đang mở (`open`), tức là **ai nhắn đến Zalo của bạn thì AI Bot cũng tự động trả lời**. Để thay đổi điều này, bạn sử dụng các cấu hình (config) sau:

### 5.1. Tắt hoàn toàn Bot (Không tự động trả lời ai cả)
Nếu bạn muốn tạm thời tắt bot Zalo mà không cần tắt Gateway:
* Lệnh tắt: `openclaw config set channels.zalouser.enabled false`
* Bật lại khi cần: `openclaw config set channels.zalouser.enabled true`
*(Nhớ khởi động lại Gateway để áp dụng: `openclaw gateway restart`)*

### 5.2. Chỉ cho phép Bot trả lời một (hoặc vài) người cụ thể
Sứ dụng cơ chế **Allowlist (Danh sách trắng)**. Bạn cần biết ID Zalo của người đó (dùng lệnh `openclaw directory peers list` để lấy ID).

* Bước 1: Đổi DM Policy sang chế độ allowlist
  `openclaw config set channels.zalouser.dmPolicy "allowlist"`

* Bước 2: Khai báo danh sách ID được phép chat
  Thay vì dấu `*` (tất cả mọi người), bạn truyền vào ID.
  `openclaw config set channels.zalouser.allowFrom '["ID_NGUOI_1", "ID_NGUOI_2"]'`
  
  *Ví dụ cho phép Đỗ Tài (ID: 5144620913902487419) và một bạn khác:*
  `openclaw config set channels.zalouser.allowFrom '["5144620913902487419", "123456789"]'`

* Bỏ giới hạn (Cho phép tất cả trở lại):
  `openclaw config set channels.zalouser.dmPolicy "open"`
  `openclaw config set channels.zalouser.allowFrom '["*"]'`

### 5.3. Chỉ cho phép trả lời trong một "Cộng đồng" / "Nhóm" (Group)
Nếu Zalo có hỗ trợ lấy Group ID (thông qua lệnh `openclaw directory groups list --channel zalouser`), bạn cũng có thể đưa ID của nhóm đó vào danh sách `allowFrom`. Bot sẽ chỉ hoạt động trong nội bộ các ID được định nghĩa.

---

## 6. Mở rộng: Kết nối Bot vào các nền tảng khác (Telegram, Discord, Gmail, Slack...)

OpenClaw hỗ trợ kết nối đồng thời với rất nhiều nền tảng ngoài Zalo. Mỗi nền tảng được quản lý thông qua một **plugin** (hay còn gọi là channel provider). Hệ thống sẽ gom chung tin nhắn từ mọi nền tảng về một chỗ cho AI xử lý.

Để tích hợp, bạn chỉ cần làm theo một quy trình chuẩn 3 bước như sau:

### Quy trình 3 bước chuẩn:
**Bước 1: Bật plugin cho nền tảng muốn dùng**
* Xem danh sách các kênh được hỗ trợ (có sẵn trong máy): `openclaw plugins list`
* Bật nền tảng lên:
  * Ví dụ Telegram: `openclaw plugins enable telegram`
  * Ví dụ Discord: `openclaw plugins enable discord`
  * Ví dụ Gmail: `openclaw plugins enable gmail`
* Khởi động lại Gateway để áp dụng plugin: `openclaw gateway restart`

**Bước 2: Cung cấp thông tin đăng nhập (Token / QR)**
Mỗi nền tảng có cách xác thực khác nhau (API Token, Quét QR, hoặc OAuth). Chạy lệnh đăng nhập theo kênh tương ứng:

* **Với Telegram** (Dạo Bot): 
  - Tạo bot trên ứng dụng Telegram qua @BotFather để lấy API Token.
  - Chạy lệnh: `openclaw channels login --channel telegram` (Hệ thống sẽ bảo bạn dán Token vào).
* **Với Discord** (Bot Discord):
  - Tạo bot trên Discord Developer Portal để lấy Token.
  - Chạy lệnh: `openclaw channels login --channel discord`
* **Với Gmail** (Đọc, gửi email bằng trí tuệ nhân tạo):
  - Chạy lệnh: `openclaw channels login --channel gmail` (Làm theo hướng dẫn xác thực Google OAuth trên màn hình).

**Bước 3: Kiểm tra trạng thái và thiết lập DM Policy**
* Chạy: `openclaw channels status --probe` để xem kênh mới đã hiện trạng thái `configured` (thành công) hay chưa.
* Nếu muốn giới hạn không cho người ngoài chat với bot Telegram/Discord của bạn, hãy áp dụng lại các lệnh `dmPolicy` giống y hệt phần 5.2 (chỉ thay chữ `zalouser` bằng `telegram`, `discord`, v.v.).

---

## 7. Dạy Skill và Tuỳ chỉnh Tính cách cho Bot (Agentic Mode)

OpenClaw được thiết kế theo hướng Agentic AI, nghĩa là thay vì cấu hình bằng code phức tạp, bạn "dạy" Bot thông qua các file văn bản (Markdown) để định hình tính cách và trí tuệ.

Toàn bộ "linh hồn" và kỹ năng của Bot được lưu ở thư mục Workspace: `~/.openclaw/workspace/`. Trong này có 2 file quan trọng để bạn tùy chỉnh:

### 7.1. File \`SOUL.md\` (Linh hồn & Kỹ năng của Bot)
Quy định cách Bot hành xử, bộ quy tắc giao tiếp và phong cách nói chuyện.
* **Cách mở/sửa**: chạy lệnh `wsl nano ~/.openclaw/workspace/SOUL.md` (hoặc mở bằng VS Code nếu đang ở thư mục này).
* **Bạn có thể viết thêm vào file này (bằng Tiếng Việt) các quy tắc như:**
  - *"Chỉ xưng hô là 'Em' và gọi người khác là 'Anh/Chị'."*
  - *"Luôn trả lời thật ngắn gọn, tự nhiên, không dùng format dấu sao, dấu gạch ngang."*
  - *"Bạn là chuyên gia tư vấn tình cảm, hãy trả lời thật sâu sắc và mặn mòi."*
  - *"Nếu ai hỏi về sản phẩm A, bao gồm thông tin chi tiết B, thì báo giá là C."*

### 7.2. File \`IDENTITY.md\` (Định danh riêng)
Dùng để thiết lập tên gọi tiếng việt cho bot (khi nó xưng hô).
* **Cách mở/sửa**: chạy lệnh `wsl nano ~/.openclaw/workspace/IDENTITY.md`.
* **Sửa các dòng nội dung sau:**
  - `- **Name:**` (Ví dụ: Ngọc Trinh)
  - `- **Vibe:**` (Ví dụ: Thân thiện, Lịch sự, Hơi kiêu ngạo...)
  - `- **Emoji:**` (Ví dụ: 💖, 🍓)

### 💡 Mẹo nhỏ để Bot trở nên SIÊU THÔNG MINH
1. **Chia nhỏ kiến thức**: Bạn có thể thả các file tài liệu định dạng `.txt` hoặc `.md` chứa thông tin sản phẩm, báo giá, câu hỏi thường gặp (FAQ)... thẳng vào thư mục `~/.openclaw/workspace/`. Bot sẽ **Tự đọc** các file này và dựa vào đó trả lời tin nhắn Zalo cực chuẩn xác mà không cần copy paste mỗi lần chat.
2. **Restart lại đầu não**: Bất cứ khi nào bạn sửa đổi các file `SOUL.md` hay tài liệu lớn và muốn Bot "thấm nhận" ngay, hãy chạy lệnh:
   * `openclaw gateway restart`

---
*Generated by Antigravity AI* 🦞
