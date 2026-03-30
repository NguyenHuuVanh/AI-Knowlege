# 🛠️ OpenClaw Setup — Hướng dẫn Thực hành từng bước

> Tài liệu này chứa **tất cả các câu lệnh** bạn cần chạy theo thứ tự.
> Mở **WSL (Ubuntu bash)** và chạy từng lệnh một.

---

## 🔍 BƯỚC 0: Kiểm tra hệ thống

```bash
# 1. Mở WSL (chạy từ PowerShell)
wsl

# 2. Kiểm tra OpenClaw hoạt động
openclaw health

# 3. Xem danh sách plugins
openclaw plugins list

# 4. Kiểm tra channels hiện tại
openclaw channels status --probe

# 5. Kiểm tra Docker đã cài chưa (cần cho Postiz)
docker --version
docker compose version
```

> [!IMPORTANT]
> Nếu `openclaw health` báo lỗi → hãy chạy `openclaw gateway run` trước.
> Nếu `docker --version` báo lỗi → xem Bước B-0 bên dưới.

---

# 📧 PHẦN A: Kết nối Gmail với OpenClaw

> [!IMPORTANT]
> **Khuyến nghị bảo mật:** Tạo 1 tài khoản Gmail riêng cho bot, ví dụ `tenban-openclaw@gmail.com`. Nếu server bị tấn công, Gmail cá nhân của bạn sẽ không bị ảnh hưởng.

Có **3 cách** kết nối Gmail, từ dễ đến khó:

| Cách | Độ khó | Tính năng | Khuyến nghị |
|------|--------|-----------|-------------|
| **Cách 1: IMAP/SMTP** | ⭐ Dễ | Cơ bản (đọc + gửi) | Mới bắt đầu |
| **Cách 2: gogcli** | ⭐⭐ Trung bình | Đầy đủ (Gmail + Calendar + Drive) | Dùng thường xuyên |
| **Cách 3: Composio MCP** | ⭐ Dễ | Đầy đủ, không cần Google Cloud | 👈 **Nhanh nhất** |

---

## 🥇 Cách 1: IMAP/SMTP — Dễ nhất

### A1-1: Bật IMAP trong Gmail

1. Mở Gmail → **Settings (⚙️)** → **See all settings** → tab **Forwarding and POP/IMAP**
2. Bật **Enable IMAP** → **Save Changes**

### A1-2: Tạo App Password

1. Truy cập: `https://myaccount.google.com/apppasswords`
2. Bật **2-Step Verification** nếu chưa bật
3. Tạo App Password cho **"Mail"** → Copy mã **16 ký tự**

### A1-3: Cấu hình OpenClaw

```bash
# Bật plugin Gmail
openclaw plugins enable gmail

# Login với IMAP
openclaw channels login --channel gmail
```

Khi được hỏi, nhập các thông tin:

| Field | Giá trị |
|-------|---------|
| IMAP Host | `imap.gmail.com` |
| IMAP Port | `993` |
| Encryption | `SSL/TLS` |
| Username | `your-email@gmail.com` |
| Password | `xxxx xxxx xxxx xxxx` (App Password) |
| SMTP Host | `smtp.gmail.com` |
| SMTP Port | `465` |

```bash
# Restart và kiểm tra
openclaw gateway restart
openclaw channels status --probe
# → gmail: configured ✅

# Test
openclaw chat "Tóm tắt 5 email mới nhất trong inbox"
```

---

## 🥈 Cách 2: gogcli — Mạnh hơn (Gmail + Calendar + Drive)

### A2-1: Cài gogcli skill

```bash
# Cài từ ClawHub (gõ lệnh chat vào OpenClaw)
openclaw chat "/skills install gogcli"

# Hoặc dùng clawhub CLI
clawhub install gogcli
```

### A2-2: Xác thực OAuth

```bash
# Đăng nhập Google qua gogcli
gogcli auth login --scopes gmail.readonly,gmail.send,gmail.compose

# → Trình duyệt mở ra → đăng nhập Google → cấp quyền
# → Quay lại terminal, token được lưu tự động
```

> [!WARNING]
> Chỉ dùng với tài khoản Gmail **đã có lịch sử sử dụng lâu dài** (vài tháng trở lên). Tài khoản mới tạo dễ bị Google khóa vì nhận diện là bot.

### A2-3: Test

```bash
openclaw chat "How many unread emails do I have?"
openclaw chat "Tóm tắt 5 email mới nhất"
openclaw chat "Kiểm tra lịch hôm nay trên Google Calendar"
```

---

## 🥉 Cách 3: Composio MCP — Nhanh nhất, không cần Google Cloud ⭐ KHUYẾN NGHỊ

Composio quản lý toàn bộ OAuth thay bạn — không cần tạo Google Cloud Project.

### A3-1: Đăng ký Composio (trên trình duyệt)

1. Truy cập: **https://dashboard.composio.dev**
2. Đăng ký tài khoản (miễn phí)
3. Sau khi đăng nhập, vào **Integrations** → tìm **Gmail** → Click **Connect**
4. Đăng nhập Gmail và cấp quyền → Composio lưu token cho bạn

### A3-2: Lấy Consumer Key

1. Trong Composio Dashboard, vào **Settings** → **API Keys**
2. Click **"Generate API Key"**
3. **Copy Consumer Key** (dạng `ck_xxxxxxxxxx`)

### A3-3: Cấu hình OpenClaw

```bash
# Mở file config OpenClaw
nano ~/.openclaw/openclaw.json
```

Tìm hoặc thêm section `plugins` với nội dung sau:

```json
{
  "plugins": {
    "entries": {
      "composio": {
        "enabled": true,
        "config": {
          "consumerKey": "ck_your_key_here"
        }
      }
    }
  }
}
```

> [!TIP]
> Thay `ck_your_key_here` bằng Consumer Key thực tế từ bước A3-2.

```bash
# Lưu file (Ctrl+O → Enter → Ctrl+X)

# Restart gateway
openclaw gateway restart

# Kiểm tra
openclaw plugins list
# → Tìm thấy composio: enabled ✅
```

### A3-4: Test Gmail qua Composio

```bash
# Test đọc email
openclaw chat "Tóm tắt 5 email mới nhất trong inbox của tôi"

# Test gửi email
openclaw chat "Gửi email test đến your-other-email@gmail.com với nội dung 'Hello from OpenClaw + Composio!'"

# Test tìm kiếm
openclaw chat "Tìm email từ tuần trước có chứa từ khóa 'invoice'"
```

> [!NOTE]
> Composio tự động refresh token → không cần cấp quyền lại. Hỗ trợ thêm: Google Calendar, Drive, Sheets, Docs, Contacts.

---

# 📱 PHẦN B: Postiz (Self-hosted, miễn phí)

---

## B-0: Cài Docker (nếu chưa có)

```bash
# === CHẠY TRONG WSL ===

# 1. Kiểm tra Docker đã cài chưa
docker --version

# Nếu chưa có → chạy các lệnh sau:

# 2. Update package list
sudo apt update

# 3. Cài Docker
sudo apt install -y docker.io docker-compose-plugin

# 4. Thêm user vào docker group (để chạy không cần sudo)
sudo usermod -aG docker $USER

# 5. QUAN TRỌNG: Logout & login lại WSL để áp dụng
exit
```

```powershell
# Sau khi exit WSL, mở lại WSL từ PowerShell:
wsl
```

```bash
# 6. Kiểm tra Docker hoạt động
docker --version
docker compose version
docker run hello-world
```

> [!IMPORTANT]
> Nếu `docker run hello-world` báo lỗi permission, hãy chạy:
> ```bash
> sudo service docker start
> ```

---

## B-1: Tạo JWT Secret (chuỗi ngẫu nhiên)

```bash
# Tạo chuỗi ngẫu nhiên 64 ký tự cho JWT_SECRET
openssl rand -hex 32
# → GHI LẠI chuỗi này, sẽ dùng ở bước B-2
```

---

## B-2: Tạo file Docker Compose cho Postiz

```bash
# 1. Tạo thư mục
mkdir -p ~/postiz && cd ~/postiz

# 2. Tạo file .env (THAY các giá trị bên dưới)
cat > .env << 'EOF'
# === CẦN THAY ĐỔI ===
JWT_SECRET=PASTE_CHUOI_64_KY_TU_O_BUOC_B1_VAO_DAY

# === GIỮ NGUYÊN ===
DATABASE_URL=postgresql://postiz:postiz_pass@postiz-db:5432/postiz
REDIS_URL=redis://postiz-redis:6379
FRONTEND_URL=http://localhost:5000
NEXT_PUBLIC_BACKEND_URL=http://localhost:5000/api
BACKEND_INTERNAL_URL=http://localhost:3000
EOF

# 3. Tạo file docker-compose.yml
cat > docker-compose.yml << 'COMPOSE'
version: '3.8'

services:
  postiz:
    image: ghcr.io/gitroomhq/postiz-app:latest
    container_name: postiz
    restart: unless-stopped
    ports:
      - "5000:5000"
      - "3000:3000"
    env_file:
      - .env
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_started
    networks:
      - postiz-network

  postgres:
    image: postgres:16-alpine
    container_name: postiz-db
    restart: unless-stopped
    environment:
      POSTGRES_USER: postiz
      POSTGRES_PASSWORD: postiz_pass
      POSTGRES_DB: postiz
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postiz"]
      interval: 5s
      timeout: 5s
      retries: 5
    networks:
      - postiz-network

  redis:
    image: redis:7-alpine
    container_name: postiz-redis
    restart: unless-stopped
    networks:
      - postiz-network

volumes:
  postgres_data:

networks:
  postiz-network:
    driver: bridge
COMPOSE

# 4. Kiểm tra file đã tạo đúng
echo "=== .env ===" && cat .env
echo ""
echo "=== docker-compose.yml ===" && cat docker-compose.yml
```

---

## B-3: Khởi chạy Postiz

```bash
# 1. Di chuyển vào thư mục Postiz
cd ~/postiz

# 2. Pull images (lần đầu sẽ tải khoảng 1-2GB)
docker compose pull

# 3. Khởi chạy tất cả services
docker compose up -d

# 4. Xem logs để kiểm tra
docker compose logs -f postiz
# → Đợi thấy dòng "ready" hoặc "listening on port 5000"
# → Nhấn Ctrl+C để thoát xem log

# 5. Kiểm tra tất cả containers đang chạy
docker compose ps
# → Cần thấy cả 3: postiz, postiz-db, postiz-redis đều "Up"
```

---

## B-4: Truy cập Postiz Dashboard

1. Mở trình duyệt Windows, truy cập:
   ```
   http://localhost:5000
   ```

2. **Tạo tài khoản admin** (lần đầu tiên)

3. Sau khi đăng nhập, vào **Settings** → **Channels** → Thêm social media:

   | Nền tảng | Cần tạo App tại | Hướng dẫn |
   |----------|-----------------|-----------|
   | **Facebook** | https://developers.facebook.com | Tạo App → lấy App ID & Secret |
   | **Threads** | Kết nối qua Instagram (Meta API) | Cùng Meta Developer App |
   | **X (Twitter)** | https://developer.x.com | Tạo App → lấy API Key & Secret |
   | **LinkedIn** | https://www.linkedin.com/developers | Tạo App → OAuth credentials |
   | **Instagram** | https://developers.facebook.com | Cần tài khoản Business |

4. Sau khi kết nối xong, vào **Settings** → **API**:
   - Click **"Generate API Key"**
   - **Copy API Key** → ghi lại (dùng ở bước B-5)

---

## B-5: Kết nối Postiz với OpenClaw

```bash
# 1. Cài Postiz Skill từ ClawHub
clawhub install nevo-david/postiz

# Nếu clawhub chưa cài:
# npm install -g clawhub
# Hoặc dùng: npx clawhub install nevo-david/postiz

# 2. Cấu hình API Key (THAY your-api-key bằng key thực từ bước B-4)
export POSTIZ_API_KEY="your-postiz-api-key-here"

# 3. Lưu vĩnh viễn vào .bashrc để không mất khi tắt terminal
echo 'export POSTIZ_API_KEY="your-postiz-api-key-here"' >> ~/.bashrc
source ~/.bashrc

# 4. Restart OpenClaw gateway
openclaw gateway restart

# 5. Kiểm tra
openclaw plugins list
```

---

## B-6: Test đăng bài

```bash
# Test đăng bài lên Facebook
openclaw chat "Đăng bài lên Facebook với nội dung: 'Xin chào từ OpenClaw + Postiz! 🚀'"

# Test đăng bài lên Threads
openclaw chat "Đăng bài lên Threads: 'Hello World! OpenClaw đã kết nối thành công 🎉'"

# Test đăng nhiều nền tảng
openclaw chat "Đăng bài này lên Facebook và Threads: 'OpenClaw tự động hóa mọi thứ!' Tự điều chỉnh caption cho từng nền tảng."

# Xem lịch đăng bài
openclaw chat "Xem lịch đăng bài tuần này"

# Lên lịch đăng bài
openclaw chat "Lên lịch đăng bài lên Facebook vào ngày mai lúc 10h sáng: 'Tips hay cho ngày mới 💡'"
```

---

# ⚡ PHẦN C: Thiết lập Automation nâng cao

---

## C-1: Kết nối Telegram (để nhận thông báo)

```bash
# === TRƯỚC TIÊN: Tạo Bot trên Telegram ===
# 1. Mở Telegram, tìm @BotFather
# 2. Gõ /newbot
# 3. Đặt tên bot và username
# 4. BotFather sẽ gửi cho bạn Bot Token (dạng 123456:ABC-DEF...)
# 5. GHI LẠI Bot Token này

# === SAU ĐÓ: Kết nối với OpenClaw ===

# 1. Bật plugin Telegram
openclaw plugins enable telegram

# 2. Login (dán Bot Token khi được hỏi)
openclaw channels login --channel telegram

# 3. Restart gateway
openclaw gateway restart

# 4. Kiểm tra
openclaw channels status --probe
# → Tìm dòng: telegram: configured ✅

# 5. Test: Gửi tin nhắn cho bot trên Telegram, bot sẽ phản hồi
```

> [!IMPORTANT]
> Sau khi tạo bot, bạn cần **gửi 1 tin nhắn bất kỳ** cho bot trên Telegram để "kích hoạt" kết nối.

---

## C-2: Cấu hình SOUL.md (Bộ não automation)

```bash
# 1. Mở file SOUL.md để chỉnh sửa
nano ~/.openclaw/workspace/SOUL.md
```

**Copy và paste TOÀN BỘ nội dung sau vào cuối file SOUL.md:**

```markdown
---

## 📧 Quy tắc Tự động Email

### Tóm tắt Email hàng ngày
- Mỗi sáng lúc 8:00, tự động kiểm tra Gmail và tóm tắt tất cả email quan trọng trong 24 giờ qua
- Email quan trọng bao gồm: email từ sếp, email có chữ "urgent", "deadline", "gấp", "khẩn"
- Gửi bản tóm tắt qua Telegram cho tôi
- Format tóm tắt: [Người gửi] - [Tiêu đề] - [Tóm tắt 1 dòng] - [Mức độ ưu tiên: 🔴🟡🟢]

### Email tự động trả lời
- Nếu có ai gửi email hỏi về lịch meeting, trả lời: "Cảm ơn bạn, mình sẽ xác nhận lại lịch trong vòng 24 giờ."
- Với email quảng cáo/spam: tự động đánh dấu đã đọc và chuyển vào thư mục Promotions
- Với email từ danh sách VIP (sếp, đối tác quan trọng): thông báo NGAY LẬP TỨC qua Telegram

### Danh sách VIP Email
- (Thêm email VIP của bạn vào đây, ví dụ:)
- boss@company.com
- partner@client.com

---

## 📱 Quy tắc Social Media

### Lịch đăng bài
- Thứ 2 đến thứ 6, lúc 10:00 sáng: đăng 1 bài về chủ đề công nghệ/AI
- Thứ 7: đăng 1 bài tổng hợp tuần (top stories)
- Chủ nhật: nghỉ, không đăng

### Phong cách viết bài
- Facebook: chuyên nghiệp nhưng thân thiện, có emoji, 100-200 từ
- Threads: ngắn gọn, trend, tối đa 3-4 dòng
- LinkedIn: formal, industry insights, 150-300 từ

### Hashtag
- Luôn thêm hashtag phù hợp với nội dung
- Tối đa 5 hashtag cho Facebook, 10 cho Threads

---

## 🔗 Workflow kết hợp Gmail + Social Media

### Email → Social Media
- Khi nhận được email newsletter hay (từ các nguồn uy tín), tự động:
  1. Tóm tắt nội dung chính
  2. Tạo bản nháp bài đăng social media
  3. Gửi bản nháp cho tôi duyệt qua Telegram trước khi đăng

### Social Media → Email
- Khi có ai mention/comment quan trọng trên social media:
  1. Gửi email tóm tắt cho tôi
  2. Thông báo qua Telegram

### Báo cáo tuần
- Mỗi thứ 6 lúc 5:00 chiều, gửi báo cáo tuần qua Telegram:
  - Tổng email đã xử lý
  - Số bài đã đăng social media
  - Engagement tổng quan (nếu có)
```

**Sau khi paste xong:**
- Nhấn `Ctrl+O` → `Enter` (để lưu)
- Nhấn `Ctrl+X` (để thoát nano)

---

## C-3: Thiết lập Cron Jobs (chạy tự động hằng ngày)

```bash
# 1. Mở crontab editor
crontab -e
# → Nếu hỏi chọn editor, chọn số 1 (nano)
```

**Thêm các dòng sau vào CUỐI file:**

```cron
# ===== OPENCLAW AUTOMATION =====

# Tóm tắt email quan trọng - chạy lúc 8:00 sáng mỗi ngày
0 8 * * * /usr/local/bin/openclaw chat "Kiểm tra Gmail, tóm tắt email quan trọng trong 24 giờ qua và gửi cho tôi qua Telegram" >> /var/log/openclaw-email-summary.log 2>&1

# Auto-post social media - Thứ 2 đến thứ 6 lúc 10:00 sáng
0 10 * * 1-5 /usr/local/bin/openclaw chat "Viết 1 bài post ngắn gọn, hấp dẫn về chủ đề công nghệ/AI và đăng lên Facebook + Threads. Tự điều chỉnh phong cách cho từng nền tảng." >> /var/log/openclaw-social.log 2>&1

# Tổng hợp tuần - Thứ 7 lúc 10:00 sáng
0 10 * * 6 /usr/local/bin/openclaw chat "Viết bài tổng hợp top stories về công nghệ tuần này và đăng lên Facebook + Threads" >> /var/log/openclaw-social.log 2>&1

# Báo cáo tuần - Thứ 6 lúc 5:00 chiều
0 17 * * 5 /usr/local/bin/openclaw chat "Gửi báo cáo tuần qua Telegram: tổng email xử lý, bài đăng social media, và engagement" >> /var/log/openclaw-report.log 2>&1
```

**Sau khi thêm xong:**
- Nhấn `Ctrl+O` → `Enter` (lưu)
- Nhấn `Ctrl+X` (thoát)

```bash
# 2. Kiểm tra crontab đã lưu thành công
crontab -l

# 3. Tạo thư mục log (nếu chưa có)
sudo mkdir -p /var/log
sudo touch /var/log/openclaw-email-summary.log /var/log/openclaw-social.log /var/log/openclaw-report.log
sudo chmod 666 /var/log/openclaw-email-summary.log /var/log/openclaw-social.log /var/log/openclaw-report.log

# 4. Đảm bảo cron service đang chạy trong WSL
sudo service cron start

# 5. (Tùy chọn) Tự động start cron khi WSL khởi động
echo "sudo service cron start" | sudo tee -a /etc/profile.d/start-cron.sh
sudo chmod +x /etc/profile.d/start-cron.sh
```

---

## C-4: Test toàn bộ hệ thống

```bash
# === TEST GMAIL ===
openclaw chat "Tóm tắt 3 email mới nhất trong inbox"

# === TEST SOCIAL MEDIA ===
openclaw chat "Xem danh sách tài khoản social media đã kết nối"
openclaw chat "Tạo bản nháp bài đăng Facebook về chủ đề AI"

# === TEST TELEGRAM ===
openclaw chat "Gửi tin nhắn test qua Telegram: 'Hệ thống OpenClaw hoạt động bình thường! ✅'"

# === TEST WORKFLOW KẾT HỢP ===
openclaw chat "Kiểm tra email mới, nếu có email hay thì tóm tắt và tạo bản nháp bài đăng social media"

# === XEM LOG ===
tail -f /var/log/openclaw-email-summary.log
tail -f /var/log/openclaw-social.log
```

---

# ✅ Checklist hoàn thành

Chạy xong tất cả? Kiểm tra lại:

```bash
# Kiểm tra tổng thể
openclaw health
openclaw channels status --probe
openclaw plugins list
docker compose -f ~/postiz/docker-compose.yml ps
crontab -l
```

| Mục | Lệnh kiểm tra | Kết quả mong đợi |
|-----|----------------|-------------------|
| OpenClaw | `openclaw health` | ✅ Healthy |
| Gmail OAuth | `openclaw channels status --probe` | gmail: configured |
| Telegram | `openclaw channels status --probe` | telegram: configured |
| Postiz Docker | `docker compose ps` (trong ~/postiz) | 3 containers Up |
| Postiz Dashboard | Mở http://localhost:5000 | Trang web hiện ra |
| Cron Jobs | `crontab -l` | 4 jobs hiện ra |

---

# 🔧 Xử lý sự cố nhanh

```bash
# Postiz lỗi → restart
cd ~/postiz && docker compose restart

# Xem log Postiz
cd ~/postiz && docker compose logs -f postiz

# Gateway lỗi → restart
openclaw gateway restart

# Xem log OpenClaw
openclaw logs --follow

# Cron không chạy → kiểm tra service
sudo service cron status
sudo service cron start

# Docker không chạy → start service
sudo service docker start
```

---

*Hướng dẫn tạo bởi Antigravity AI — 23/03/2026* 🦞
