# 🚀 Hướng dẫn Setup OpenClaw: Gmail + Social Media

> Tài liệu này hướng dẫn chi tiết từng bước cài đặt **2 tính năng chính**:
> 1. **Đọc Gmail, thông báo & tự động trả lời email**
> 2. **Tự động lên lịch & đăng bài lên Facebook, Threads, và các nền tảng khác**

---

## 📋 Mục lục

1. [Điều kiện tiên quyết](#-điều-kiện-tiên-quyết)
2. [PHẦN A: Cài đặt Gmail Plugin](#-phần-a-cài-đặt-gmail-plugin)
   - [Cách 1 (Dễ): IMAP + App Password](#cách-1-dễ-nhất-imap--smtp-dùng-app-password)
   - [Cách 2 (Nâng cao): Gmail API + OAuth](#cách-2-nâng-cao-gmail-rest-api--oauth)
   - [Cách 3 (Pro): Gmail Watch + Pub/Sub (Realtime)](#cách-3-pro-gmail-watch--pubsub-realtime)
3. [PHẦN B: Cài đặt PostFast / Postiz (Đăng bài Social Media)](#-phần-b-cài-đặt-đăng-bài-social-media)
   - [Lựa chọn 1: PostFast (Đơn giản, trả phí)](#lựa-chọn-1-postfast-đơn-giản-có-free-trial)
   - [Lựa chọn 2: Postiz (Self-hosted, miễn phí)](#lựa-chọn-2-postiz-self-hosted-miễn-phí)
4. [PHẦN C: Thiết lập Automation nâng cao](#-phần-c-thiết-lập-automation-nâng-cao)
5. [Xử lý sự cố thường gặp](#-xử-lý-sự-cố-thường-gặp)

---

## ✅ Điều kiện tiên quyết

Trước khi bắt đầu, hãy đảm bảo bạn đã có:

| # | Yêu cầu | Trạng thái |
|---|---------|------------|
| 1 | OpenClaw đã cài đặt và chạy được | ✅ (Đã có) |
| 2 | Gateway đang chạy (`openclaw gateway run`) | ✅ (Kiểm tra lại) |
| 3 | GitHub Copilot đã kết nối (model AI) | ✅ (Đã có) |
| 4 | Chạy OpenClaw trên WSL (khuyến nghị) | ✅ (Đã setup) |

> [!IMPORTANT]
> **Luôn chạy tất cả lệnh OpenClaw từ WSL (Ubuntu bash)** để tránh xung đột config giữa Windows và WSL. Mở WSL bằng cách gõ `wsl` trong PowerShell.

**Kiểm tra nhanh hệ thống:**
```bash
# Vào WSL trước
wsl

# Kiểm tra OpenClaw đã hoạt động chưa
openclaw health

# Kiểm tra plugins đang bật
openclaw plugins list

# Kiểm tra channels
openclaw channels status --probe
```

---

# 📧 PHẦN A: Cài đặt Gmail Plugin

Bạn có **3 cách** để kết nối Gmail. Mình khuyến nghị bắt đầu từ **Cách 1** vì đơn giản nhất.

---

## Cách 1 (Dễ nhất): IMAP + SMTP dùng App Password

Đây là cách đơn giản nhất — OpenClaw sẽ đọc email qua IMAP và gửi email qua SMTP.

### Bước 1.1: Tạo App Password trên Google

1. Vào trình duyệt, truy cập: **https://myaccount.google.com/security**
2. Bật **Xác minh 2 bước (2-Step Verification)** nếu chưa bật
3. Sau khi bật 2FA, truy cập: **https://myaccount.google.com/apppasswords**
4. Chọn **"Select app"** → chọn **"Mail"**
5. Chọn **"Select device"** → chọn **"Other"** → gõ **"OpenClaw"**
6. Nhấn **"Generate"**
7. **Ghi lại mật khẩu 16 ký tự** (dạng `xxxx xxxx xxxx xxxx`) — đây là App Password

> [!WARNING]
> **KHÔNG** dùng mật khẩu Gmail chính! Luôn dùng App Password. Nếu bị lộ, bạn chỉ cần xóa App Password mà không ảnh hưởng tài khoản chính.

### Bước 1.2: Bật IMAP trong Gmail

1. Mở Gmail trên trình duyệt
2. Vào **Settings (⚙️)** → **See all settings** → Tab **"Forwarding and POP/IMAP"**
3. Ở mục **IMAP Access**, chọn **"Enable IMAP"**
4. Nhấn **Save Changes**

### Bước 1.3: Bật plugin Gmail trong OpenClaw

```bash
# Vào WSL
wsl

# Bật plugin Gmail
openclaw plugins enable gmail

# Restart gateway để áp dụng
openclaw gateway restart
```

### Bước 1.4: Đăng nhập Gmail channel

```bash
# Chạy lệnh login - hệ thống sẽ hỏi bạn thông tin
openclaw channels login --channel gmail
```

Khi hệ thống hỏi, nhập các thông tin sau:

| Thông tin | Giá trị |
|-----------|---------|
| IMAP Host | `imap.gmail.com` |
| IMAP Port | `993` |
| Encryption | `SSL/TLS` |
| Username | `your-email@gmail.com` (email đầy đủ) |
| Password | `xxxx xxxx xxxx xxxx` (App Password ở bước 1.1) |
| SMTP Host | `smtp.gmail.com` |
| SMTP Port | `465` (SSL) hoặc `587` (TLS) |

### Bước 1.5: Kiểm tra kết nối

```bash
# Kiểm tra trạng thái
openclaw channels status --probe

# Nếu thấy gmail: configured → Thành công! 🎉
```

### Bước 1.6: Test thử

```bash
# Hỏi OpenClaw về email mới nhất
openclaw chat "Tóm tắt 5 email mới nhất trong inbox của tôi"

# Hoặc gửi email test
openclaw chat "Gửi email test đến my-other-email@gmail.com với nội dung 'Hello from OpenClaw!'"
```

---

## Cách 2 (Nâng cao): Gmail REST API + OAuth

Cách này cho phép truy cập nâng cao hơn — tìm kiếm email theo label, thread, metadata mà không cần lưu mật khẩu.

### Bước 2.1: Tạo Google Cloud Project

1. Truy cập: **https://console.cloud.google.com/**
2. Click **"Select a Project"** ở thanh trên → **"New Project"**
3. Đặt tên: **"OpenClaw Email Bot"** → Click **"Create"**

### Bước 2.2: Bật Gmail API

1. Trong project vừa tạo, vào **"APIs & Services"** → **"Library"**
2. Tìm **"Gmail API"** → Click vào → Click **"Enable"**

### Bước 2.3: Cấu hình OAuth Consent Screen

1. Vào **"APIs & Services"** → **"OAuth consent screen"**
2. Chọn **"External"** → Click **"Create"**
3. Điền thông tin:
   - **App name**: `OpenClaw Bot`
   - **User support email**: email của bạn
   - **Developer contact**: email của bạn
4. Ở bước **"Scopes"**, thêm các scope:
   - `https://www.googleapis.com/auth/gmail.readonly` (đọc email)
   - `https://www.googleapis.com/auth/gmail.send` (gửi email)
   - `https://www.googleapis.com/auth/gmail.modify` (sửa label, đánh dấu đã đọc)
5. Ở bước **"Test users"**, thêm email Gmail của bạn
6. Click **"Save"**

> [!TIP]
> Nếu bạn chỉ muốn đọc email (không gửi), chỉ cần scope `gmail.readonly` là đủ. Càng ít quyền càng an toàn.

### Bước 2.4: Tạo OAuth Credentials

1. Vào **"APIs & Services"** → **"Credentials"**
2. Click **"Create Credentials"** → **"OAuth client ID"**
3. Application type: **"Desktop app"**
4. Name: `OpenClaw Gmail`
5. Click **"Create"**
6. **Download file JSON** (nút "Download JSON") → đổi tên thành `client_secret.json`

### Bước 2.5: Di chuyển credentials vào OpenClaw

```bash
# Vào WSL
wsl

# Tạo thư mục cho Gmail extension
mkdir -p ~/.openclaw/extensions/gmail

# Copy file credentials vào (thay đường dẫn cho đúng)
cp /mnt/c/Users/$(whoami)/Downloads/client_secret*.json ~/.openclaw/extensions/gmail/client_secret.json
```

### Bước 2.6: Chạy xác thực OAuth

```bash
# Bật plugin Gmail (nếu chưa)
openclaw plugins enable gmail

# Login bằng OAuth
openclaw channels login --channel gmail

# Hệ thống sẽ mở trình duyệt hoặc in ra URL
# → Mở URL đó, đăng nhập Google, cấp quyền
# → Sau khi cấp quyền xong, hệ thống sẽ tự lưu token

# Restart gateway
openclaw gateway restart

# Kiểm tra
openclaw channels status --probe
```

> [!NOTE]
> Token OAuth sẽ tự động refresh. Bạn chỉ cần cấp quyền 1 lần duy nhất.

---

## Cách 3 (Pro): Gmail Watch + Pub/Sub (Realtime)

Cách này cho phép nhận email **ngay lập tức** (push notification) thay vì phải polling định kỳ.

### Yêu cầu thêm:
- Đã setup xong **Cách 2** (OAuth)
- Có **server public** hoặc dùng **Tailscale Funnel / ngrok** để tạo webhook URL

### Bước 3.1: Bật Pub/Sub API

1. Trong Google Cloud Console, vào **"APIs & Services"** → **"Library"**
2. Tìm **"Cloud Pub/Sub API"** → **Enable**

### Bước 3.2: Tạo Pub/Sub Topic & Subscription

```bash
# Dùng gcloud CLI (cần cài Google Cloud SDK)
gcloud pubsub topics create gmail-openclaw
gcloud pubsub subscriptions create gmail-openclaw-sub \
  --topic gmail-openclaw \
  --push-endpoint="https://your-webhook-url.com/gmail-webhook"
```

### Bước 3.3: Cấp quyền cho Gmail gửi vào Topic

```bash
# Gmail service account cần publish vào topic
gcloud pubsub topics add-iam-policy-binding gmail-openclaw \
  --member="serviceAccount:gmail-api-push@system.gserviceaccount.com" \
  --role="roles/pubsub.publisher"
```

### Bước 3.4: Setup webhook trong OpenClaw

```bash
# Tạo webhook endpoint (nếu dùng Tailscale)
openclaw gateway run --tailscale

# Hoặc dùng ngrok
ngrok http 18789

# Cấu hình Gmail watch
openclaw config set channels.gmail.watch.enabled true
openclaw config set channels.gmail.watch.topicName "projects/YOUR_PROJECT/topics/gmail-openclaw"

# Restart
openclaw gateway restart
```

### Bước 3.5: Kích hoạt Gmail Watch

```bash
# OpenClaw sẽ đăng ký nhận thông báo từ Gmail
openclaw chat "Activate Gmail watch for real-time notifications"
```

> [!CAUTION]
> Cách 3 yêu cầu server luôn online và có public URL. Nếu bạn chỉ chạy local, hãy dùng **Cách 1 hoặc 2**.

---

# 📱 PHẦN B: Cài đặt Đăng bài Social Media

Bạn có 2 lựa chọn:

| So sánh | PostFast | Postiz |
|---------|----------|--------|
| Giá | Có bản trial 7 ngày miễn phí, sau đó trả phí | Hoàn toàn miễn phí (self-hosted) |
| Cài đặt | Rất dễ, chỉ cần API key | Cần Docker, phức tạp hơn |
| Nền tảng hỗ trợ | TikTok, IG, FB, X, YouTube, LinkedIn, Threads, Bluesky, Pinterest, Telegram | Tương tự |
| Phù hợp với | Người muốn nhanh gọn | Người có VPS / thích tự host |

---

## Lựa chọn 1: PostFast (Đơn giản, có free trial)

### Bước PF-1: Tạo tài khoản PostFast

1. Truy cập: **https://postfa.st**
2. Đăng ký tài khoản (có bản trial 7 ngày miễn phí)
3. Sau khi đăng ký, vào **Dashboard** → kết nối các tài khoản social media:
   - Click **"Add Account"** → chọn Facebook, Threads, Instagram, TikTok, v.v.
   - Đăng nhập vào từng nền tảng để cấp quyền cho PostFast

### Bước PF-2: Lấy API Key từ PostFast

1. Trong Dashboard PostFast, vào **Settings** → **API**
2. Click **"Generate API Key"**
3. **Copy API Key** và lưu lại cẩn thận

### Bước PF-3: Cài PostFast Skill vào OpenClaw

```bash
# Vào WSL
wsl

# Cài skill PostFast từ ClawHub
clawhub install postfast

# Thiết lập API Key
openclaw config set skills.postfast.apiKey "YOUR_POSTFAST_API_KEY_HERE"

# Restart gateway
openclaw gateway restart
```

### Bước PF-4: Test đăng bài

```bash
# Test đăng bài ngay
openclaw chat "Đăng bài lên Facebook với nội dung: 'Xin chào từ OpenClaw! 🚀'"

# Lên lịch đăng bài
openclaw chat "Lên lịch đăng bài lên Threads vào ngày mai lúc 9h sáng: 'Tips hay cho ngày mới 💡'"

# Đăng nhiều nền tảng cùng lúc
openclaw chat "Đăng bài này lên Facebook và Threads: 'OpenClaw thật tuyệt vời!' Tự điều chỉnh caption cho từng nền tảng."
```

> [!TIP]
> PostFast tự động tối ưu nội dung cho từng nền tảng: LinkedIn → chuyên nghiệp, X → ngắn gọn, Threads → thân thiện.

---

## Lựa chọn 2: Postiz (Self-hosted, miễn phí)

### Yêu cầu:
- Server/VPS có Docker & Docker Compose
- Hoặc chạy Docker trên máy local

### Bước PZ-1: Cài Docker (nếu chưa có)

```bash
# Trên WSL Ubuntu
sudo apt update
sudo apt install -y docker.io docker-compose

# Thêm user vào docker group
sudo usermod -aG docker $USER

# Logout rồi login lại WSL
exit
wsl
```

### Bước PZ-2: Tạo file docker-compose cho Postiz

```bash
# Tạo thư mục cho Postiz
mkdir -p ~/postiz && cd ~/postiz

# Tạo file docker-compose.yml
cat > docker-compose.yml << 'EOF'
version: '3.8'
services:
  postiz:
    image: ghcr.io/gitroomhq/postiz-app:latest
    container_name: postiz
    restart: unless-stopped
    ports:
      - "5000:5000"
    environment:
      # ⚠️ THAY ĐỔI CÁC GIÁ TRỊ NÀY
      - DATABASE_URL=postgresql://postiz:postiz_pass@postgres:5432/postiz
      - REDIS_URL=redis://redis:6379
      - JWT_SECRET=thay-bang-chuoi-ngau-nhien-cua-ban-64-ky-tu
      - FRONTEND_URL=http://localhost:5000
      - NEXT_PUBLIC_BACKEND_URL=http://localhost:5000/api
      # Bỏ comment nếu dùng Cloudflare R2 để lưu ảnh
      # - CLOUDFLARE_ACCOUNT_ID=your-account-id
      # - CLOUDFLARE_ACCESS_KEY=your-access-key
      # - CLOUDFLARE_SECRET_ACCESS_KEY=your-secret
      # - CLOUDFLARE_BUCKETNAME=postiz
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:16
    container_name: postiz-db
    restart: unless-stopped
    environment:
      - POSTGRES_USER=postiz
      - POSTGRES_PASSWORD=postiz_pass
      - POSTGRES_DB=postiz
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    container_name: postiz-redis
    restart: unless-stopped

volumes:
  postgres_data:
EOF
```

### Bước PZ-3: Khởi chạy Postiz

```bash
# Chạy Postiz
cd ~/postiz
docker compose up -d

# Kiểm tra log xem chạy OK chưa
docker compose logs -f postiz
# Đợi thấy dòng "ready" hoặc "listening on port 5000" rồi Ctrl+C

# Mở trình duyệt: http://localhost:5000
# Tạo tài khoản admin đầu tiên
```

### Bước PZ-4: Kết nối Social Media trong Postiz Dashboard

1. Mở **http://localhost:5000** trên trình duyệt
2. Đăng ký tài khoản admin
3. Vào **Settings** → **Channels** → Thêm các nền tảng:
   - **Facebook**: Cần tạo Facebook App tại https://developers.facebook.com
   - **Threads**: Kết nối qua Instagram (cùng Meta API)
   - **X (Twitter)**: Cần tạo app tại https://developer.x.com
   - **LinkedIn**: Cần tạo app tại https://www.linkedin.com/developers
4. Copy **API Key** từ Postiz Dashboard (Settings → API)

### Bước PZ-5: Cài Postiz Skill vào OpenClaw

```bash
# Cài skill Postiz từ ClawHub
clawhub install nevo-david/postiz

# Set API Key
export POSTIZ_API_KEY="your-postiz-api-key-here"

# Hoặc lưu vĩnh viễn vào .bashrc
echo 'export POSTIZ_API_KEY="your-postiz-api-key-here"' >> ~/.bashrc
source ~/.bashrc

# Restart OpenClaw gateway
openclaw gateway restart
```

### Bước PZ-6: Test đăng bài

```bash
openclaw chat "Đăng bài lên Facebook: 'Hello World từ Postiz + OpenClaw! 🎉'"
openclaw chat "Xem lịch đăng bài tuần này"
openclaw chat "Lên lịch đăng bài hàng ngày lúc 10h sáng từ thứ 2 đến thứ 6"
```

---

# ⚡ PHẦN C: Thiết lập Automation nâng cao

Sau khi cài xong Gmail và Social Media, bạn có thể thiết lập các kịch bản tự động hóa.

---

## C1. Tóm tắt email hàng ngày qua Telegram

### Bước C1-1: Kết nối Telegram

```bash
# Bật plugin Telegram
openclaw plugins enable telegram

# Login (cần Bot Token từ @BotFather trên Telegram)
openclaw channels login --channel telegram
# → Dán Bot Token khi được hỏi

# Restart
openclaw gateway restart
```

### Bước C1-2: Tạo lệnh tóm tắt hàng ngày

Thêm vào file `~/.openclaw/workspace/SOUL.md`:

```bash
wsl nano ~/.openclaw/workspace/SOUL.md
```

Thêm đoạn sau vào cuối file:

```markdown
## Quy tắc Tự động Email

### Tóm tắt Email hàng ngày
- Mỗi sáng lúc 8:00, tự động kiểm tra Gmail và tóm tắt tất cả email quan trọng trong 24 giờ qua
- Email quan trọng bao gồm: email từ sếp, email có chữ "urgent", "deadline", "gấp"
- Gửi bản tóm tắt qua Telegram cho tôi

### Email tự động trả lời
- Nếu có ai gửi email hỏi về lịch meeting, trả lời: "Cảm ơn bạn, mình sẽ xác nhận lại lịch trong vòng 24 giờ."
- Với email quảng cáo/spam: tự động đánh dấu đã đọc và chuyển vào thư mục Promotions
- Với email từ danh sách VIP (sếp, đối tác), thông báo ngay lập tức qua Telegram
```

### Bước C1-3: Setup Cron Job (tự động chạy hàng ngày)

```bash
# Mở crontab
crontab -e

# Thêm dòng sau (chạy lúc 8h sáng mỗi ngày)
0 8 * * * /usr/local/bin/openclaw chat "Tóm tắt email quan trọng trong 24 giờ qua và gửi cho tôi qua Telegram" >> /var/log/openclaw-email-summary.log 2>&1
```

---

## C2. Auto-post Social Media theo lịch

### Tạo lịch đăng bài tự động:

```bash
# Crontab - đăng bài tự động thứ 2 đến thứ 6 lúc 10h sáng
0 10 * * 1-5 /usr/local/bin/openclaw chat "Viết 1 bài post ngắn gọn, hấp dẫn về chủ đề công nghệ/AI và đăng lên Facebook + Threads" >> /var/log/openclaw-social.log 2>&1
```

Hoặc dùng lệnh trực tiếp:

```bash
# Lên lịch hàng loạt
openclaw chat "Lên lịch đăng 5 bài trong tuần này lên Facebook và Threads, mỗi ngày 1 bài lúc 10h sáng, chủ đề về tips công nghệ"
```

---

## C3. Workflow kết hợp Gmail + Social Media

Thêm vào `SOUL.md`:

```markdown
## Workflow kết hợp
- Khi nhận được email newsletter hay, tự động tóm tắt nội dung và tạo bản nháp bài đăng social media
- Khi có ai mention trên social media và nội dung cần follow-up, gửi email tóm tắt cho tôi
```

---

# 🔧 Xử lý sự cố thường gặp

## Lỗi 1: `Plugin gmail not found`
```bash
# Kiểm tra danh sách plugins
openclaw plugins list

# Nếu không thấy gmail, có thể cần cài thêm
openclaw plugins install gmail
# Hoặc
clawhub install gmail
```

## Lỗi 2: `Authentication failed` khi login Gmail
- **Kiểm tra App Password** — đảm bảo không có khoảng trắng thừa
- **Kiểm tra 2FA đã bật** — App Password chỉ hoạt động khi 2FA đã bật
- **Kiểm tra IMAP đã enable** — vào Gmail Settings kiểm tra lại

## Lỗi 3: `clawhub: command not found`
```bash
# Cài clawhub CLI
npm install -g clawhub

# Hoặc dùng npx
npx clawhub install postfast
```

## Lỗi 4: PostFast/Postiz không đăng được bài
- Kiểm tra API Key còn hợp lệ không
- Kiểm tra tài khoản social media đã được kết nối trong Dashboard
- Một số nền tảng (Facebook, Instagram) yêu cầu tài khoản Business

## Lỗi 5: Gateway crash sau khi thêm plugin mới
```bash
# Xem log chi tiết
openclaw logs --follow

# Chạy doctor để quét lỗi
openclaw doctor

# Nếu cần, force restart
openclaw gateway run --force --allow-unconfigured
```

---

# 🗺️ Tóm tắt nhanh: Lộ trình Setup

```
Bước 1: Kiểm tra OpenClaw hoạt động
       ↓
Bước 2: Chọn cách kết nối Gmail (Cách 1 dễ nhất)
       ↓
Bước 3: Bật plugin → Login → Test đọc email
       ↓
Bước 4: Cài PostFast hoặc Postiz
       ↓
Bước 5: Kết nối social media accounts
       ↓
Bước 6: Test đăng bài
       ↓
Bước 7: Thiết lập automation (cron + SOUL.md)
       ↓
🎉 DONE! OpenClaw tự động quản lý email + social media cho bạn
```

---

# 🔒 Lời khuyên Bảo mật quan trọng

> [!CAUTION]
> 1. **KHÔNG** cấp quyền Gmail cá nhân chính cho bot — tạo một email riêng cho OpenClaw
> 2. **Dùng scope tối thiểu** — chỉ cấp quyền đọc nếu không cần gửi email
> 3. **Backup API keys** ở nơi an toàn (password manager)
> 4. **Kiểm tra log thường xuyên** — `openclaw logs --follow` để phát hiện hành vi bất thường
> 5. **Cẩn thận prompt injection** — email độc hại có thể chứa lệnh lừa AI, nên dùng chế độ "duyệt trước khi gửi" cho email quan trọng

---

*Tài liệu tạo bởi Antigravity AI — Cập nhật: 23/03/2026* 🦞
