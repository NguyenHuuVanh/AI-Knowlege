# 🧠 Plan Mode, Agent là gì? Cách Đọc, Phản Biện Plan & Nhận Biết Plan Tệ

> Hướng dẫn thực chiến giúp bạn hiểu cách AI Agent lập kế hoạch, cách đọc & phản biện plan, và nhận biết khi nào plan đang đi sai hướng — trước khi nó phá nát codebase của bạn.

---

## Mục lục

1. [Agent là gì?](#1-agent-là-gì)
2. [Plan Mode là gì?](#2-plan-mode-là-gì)
3. [Tại sao Plan Mode quan trọng?](#3-tại-sao-plan-mode-quan-trọng)
4. [Cách đọc một Plan hiệu quả](#4-cách-đọc-một-plan-hiệu-quả)
5. [Cách phản biện Plan](#5-cách-phản-biện-plan)
6. [Nhận biết Plan tệ — Red Flags](#6-nhận-biết-plan-tệ--red-flags)
7. [Plan tốt trông như thế nào?](#7-plan-tốt-trông-như-thế-nào)
8. [Workflow thực tế: Plan → Review → Execute](#8-workflow-thực-tế-plan--review--execute)
9. [Prompt templates hữu ích](#9-prompt-templates-hữu-ích)

---

## 1. Agent là gì?

### Định nghĩa đơn giản

**AI Agent** là một hệ thống AI có khả năng **tự chủ hành động** để đạt được mục tiêu, thay vì chỉ trả lời câu hỏi một lần rồi dừng.

Nếu một chatbot bình thường giống **Google Translate** (bạn đưa input → nhận output), thì agent giống **một nhân viên** — họ nhận nhiệm vụ, tự chia nhỏ công việc, tự thực hiện từng bước, và tự điều chỉnh khi gặp lỗi.

### Vòng lặp cốt lõi của Agent

```
┌─────────────────────────────────────────┐
│           AGENT LOOP                    │
│                                         │
│   ┌──────────┐                          │
│   │ PERCEIVE │ ← Đọc file, error log,  │
│   │          │   context hiện tại       │
│   └────┬─────┘                          │
│        ▼                                │
│   ┌──────────┐                          │
│   │   PLAN   │ ← Lập kế hoạch:         │
│   │          │   cần sửa gì, ở đâu     │
│   └────┬─────┘                          │
│        ▼                                │
│   ┌──────────┐                          │
│   │   ACT    │ ← Viết code, chạy cmd,  │
│   │          │   gọi API, tạo file      │
│   └────┬─────┘                          │
│        ▼                                │
│   ┌──────────┐                          │
│   │ REFLECT  │ ← Kết quả đúng chưa?    │
│   │          │   Lỗi gì? Thử lại?      │
│   └────┬─────┘                          │
│        │                                │
│        └──────── Lặp lại ──────────┘    │
└─────────────────────────────────────────┘
```

### Sự khác biệt giữa Chatbot và Agent

| Đặc điểm | Chatbot (LLM đơn thuần) | Agent |
|-----------|------------------------|-------|
| **Hành vi** | Trả lời rồi dừng | Tự lặp, tự hành động |
| **Sử dụng tools** | Không | Có (đọc file, chạy lệnh, gọi API...) |
| **Bộ nhớ** | Chỉ trong cuộc hội thoại | Ngắn hạn + dài hạn |
| **Lập kế hoạch** | Không | Có — phân tích → lên plan → thực thi |
| **Tự sửa lỗi** | Không | Có — retry, rollback, điều chỉnh |
| **Ví dụ** | ChatGPT chat mode | Claude Code, Cursor Agent, Devin |

### Phân loại Agent theo mức độ tự chủ

```
Thấp ◀──────────────────────────────────────▶ Cao

Autocomplete    Copilot Chat    Plan Mode    Full Agent
(Tab suggest)   (hỏi-đáp)      (lên kế      (tự chạy
                                 hoạch,       từ đầu
                                 chờ duyệt)   đến cuối)
```

> **Quy tắc vàng**: Mức tự chủ càng cao → bạn càng cần kiểm soát chặt bằng plan review.

---

## 2. Plan Mode là gì?

### Định nghĩa

**Plan Mode** là chế độ mà AI Agent **chỉ lập kế hoạch, không thực thi**. Agent sẽ phân tích yêu cầu, khảo sát codebase, và đưa ra một plan chi tiết các bước cần làm — rồi **dừng lại chờ bạn duyệt**.

### So sánh Plan Mode vs Act Mode

| | Plan Mode | Act Mode |
|--|-----------|----------|
| **Agent làm gì** | Phân tích → Đưa ra plan → Chờ duyệt | Phân tích → Tự thực thi luôn |
| **Bạn kiểm soát** | Cao — duyệt từng bước | Thấp — chạy rồi xem kết quả |
| **Rủi ro** | Thấp — chưa thay đổi gì | Cao — có thể sửa sai file, xoá code |
| **Phù hợp khi** | Task phức tạp, codebase lớn, chưa rõ approach | Task đơn giản, đã rõ cách làm |
| **Tương tự** | Kiến trúc sư vẽ bản thiết kế | Thợ xây xây nhà |

### Ví dụ Plan Mode output

```markdown
## Plan: Thêm Dark Mode cho ứng dụng

### Phân tích hiện tại
- App đang dùng CSS variables cho màu sắc trong `src/styles/theme.css`
- Có 3 components dùng hardcoded colors: Header, Sidebar, Card
- Chưa có theme context hoặc toggle mechanism

### Các bước thực hiện

1. **Tạo theme context** (`src/contexts/ThemeContext.tsx`)
   - Dùng React Context + useReducer
   - Lưu preference vào localStorage
   
2. **Bổ sung CSS variables cho dark theme** (`src/styles/theme.css`)
   - Thêm `[data-theme="dark"]` selector
   - Map lại tất cả color variables

3. **Sửa 3 components dùng hardcoded colors**
   - `Header.tsx` — line 42: `color: #333` → `var(--text-primary)`
   - `Sidebar.tsx` — line 18: `background: white` → `var(--bg-secondary)`
   - `Card.tsx` — line 7, 23: hardcoded borders

4. **Thêm toggle button** vào Header
   - Icon sun/moon
   - Gọi `toggleTheme()` từ context

### Ảnh hưởng
- 5 files thay đổi, 1 file tạo mới
- Không breaking changes
- Không cần thêm dependency
```

---

## 3. Tại sao Plan Mode quan trọng?

### Vấn đề khi bỏ qua Plan Mode

```
❌ Bạn: "Thêm authentication cho app"
   Agent: *Tự cài NextAuth, tạo 12 file, sửa 8 file, thêm 3 dependencies*
   Bạn: "Khoan... app dùng Express thuần mà, sao lại cài NextAuth?!"
   → Mất 30 phút revert + dọn dẹp
```

### Lợi ích của Plan Mode

| Lợi ích | Giải thích |
|---------|-----------|
| 🛡️ **Phòng ngừa sai sót** | Phát hiện hiểu sai yêu cầu TRƯỚC khi code bị thay đổi |
| 🧭 **Alignment** | Đảm bảo agent hiểu đúng ý bạn, dùng đúng approach |
| 🏗️ **Architectural review** | Xem agent có vi phạm kiến trúc hiện tại không |
| 📐 **Scope control** | Kiểm soát phạm vi thay đổi — tránh agent "over-engineer" |
| 🧠 **Learning** | Bạn hiểu codebase sâu hơn qua cách agent phân tích |
| 🔄 **Iteration** | Dễ dàng yêu cầu điều chỉnh plan trước khi thực thi |

> **Mindset quan trọng**: Plan Mode biến bạn từ "người ngồi xem AI code" thành **"Tech Lead review bản thiết kế"**. Đây là kỹ năng sống còn trong thời đại AI.

---

## 4. Cách đọc một Plan hiệu quả

### Checklist đọc Plan (5 câu hỏi then chốt)

Mỗi khi nhận được plan từ agent, hãy tự hỏi 5 câu hỏi sau:

#### ① Agent có hiểu đúng yêu cầu không?

```
🔍 Kiểm tra phần "Analysis" hoặc "Understanding" đầu plan
   → Agent tóm tắt lại task có đúng ý bạn không?
   → Có bỏ sót yêu cầu nào không?
   → Có "tự thêm" yêu cầu nào bạn không nhờ không?
```

**Red flag**: Agent diễn giải yêu cầu khác xa so với ý bạn, hoặc mở rộng scope.

#### ② Approach có phù hợp không?

```
🔍 Kiểm tra giải pháp kỹ thuật
   → Có dùng đúng pattern/tech stack hiện tại không?
   → Có cách nào đơn giản hơn không?
   → Có vi phạm convention của project không?
```

**Red flag**: Agent chọn một approach hoàn toàn mới thay vì follow pattern hiện có.

#### ③ Scope có hợp lý không?

```
🔍 Đếm số file bị ảnh hưởng
   → Quá ít file → Có thể thiếu sót
   → Quá nhiều file → Có thể over-engineer hoặc refactor không cần thiết
   → File nào KHÔNG NÊN bị sửa mà lại nằm trong plan?
```

**Red flag**: Task đơn giản mà plan sửa 15+ files.

#### ④ Thứ tự các bước có logic không?

```
🔍 Đọc trình tự thực hiện
   → Dependencies giữa các bước có đúng không? (Tạo context trước khi dùng)
   → Có bước nào nên làm trước mà bị đặt sau không?
   → Có thiếu bước nào không? (migration, test, cleanup...)
```

**Red flag**: Agent dùng một module ở bước 2 nhưng tạo module đó ở bước 5.

#### ⑤ Có side effects hoặc rủi ro gì không?

```
🔍 Kiểm tra ảnh hưởng lan tỏa
   → Thêm/xoá dependency có ảnh hưởng gì?
   → Có breaking changes cho code khác không?
   → Database migration? API contract thay đổi?
   → Performance impact?
```

**Red flag**: Agent thêm dependency mới cho task không cần thiết.

---

## 5. Cách phản biện Plan

### Kỹ thuật phản biện hiệu quả

#### 🔬 Technique 1: "Zoom vào chi tiết"

Khi plan mô tả chung chung, yêu cầu agent giải thích cụ thể hơn.

```
❌ Plan: "Sẽ refactor component để tối ưu performance"

✅ Phản biện: "Cụ thể sẽ refactor gì? Memo? useMemo? 
   Code splitting? Hiện tại component render lại bao nhiêu lần 
   và target là bao nhiêu?"
```

#### 🔭 Technique 2: "Zoom ra toàn cảnh"

Khi plan quá chi tiết từng dòng code mà thiếu bức tranh tổng thể.

```
❌ Plan chỉ liệt kê: "Sửa line 42, thêm line 55, xoá line 78..."

✅ Phản biện: "Trước khi đi vào chi tiết, giải thích overall approach. 
   Tại sao chọn cách này thay vì [alternative]? 
   Kiến trúc sau khi sửa sẽ trông như thế nào?"
```

#### ⚖️ Technique 3: "Thách thức giả định"

Agent thường đưa ra giả định không nói rõ. Hãy thách thức chúng.

```
❌ Plan: "Tạo custom hook useAuth để quản lý authentication"

✅ Phản biện: "Tại sao tạo custom hook thay vì dùng 
   authContext đã có trong src/contexts/AuthContext? 
   Hai cái này khác gì nhau?"
```

#### 🔀 Technique 4: "Yêu cầu alternatives"

Đừng chấp nhận plan đầu tiên. Hỏi xem có cách khác không.

```
✅ "Cho tôi 2-3 approach khác nhau cho task này, 
    kèm trade-offs của từng approach. 
    Recommend approach nào và tại sao."
```

#### 🎯 Technique 5: "Edge cases & failure modes"

```
✅ "Plan này xử lý thế nào khi:
    - User không có permission?
    - API timeout?
    - Data rỗng hoặc malformed?
    - Concurrent requests?"
```

### Template phản biện nhanh

```markdown
## Review plan của tôi:

1. **Hiểu đúng yêu cầu**: ✅/❌ — [ghi chú]
2. **Approach**: ✅/❌ — [tại sao đồng ý/không đồng ý]
3. **Scope**: ✅/❌ — [quá rộng/quá hẹp/vừa đủ]
4. **Thiếu sót**: [list những gì plan chưa đề cập]
5. **Yêu cầu điều chỉnh**: [cụ thể cần thay đổi gì]
```

---

## 6. Nhận biết Plan tệ — Red Flags 🚩

### 🚩 Red Flag #1: "Boil the Ocean" — Làm quá nhiều thứ

```
❌ PLAN TỆ:
Bạn nhờ: "Fix cái button bị lệch"
Agent plan: 
  1. Refactor toàn bộ component library
  2. Migrate từ CSS Modules sang Tailwind
  3. Thêm design system mới
  4. Rồi mới fix button
```

**Vấn đề**: Scope explode. Agent biến task 5 phút thành project 2 ngày.

**Cách xử lý**: *"Tôi chỉ cần fix button bị lệch. Không refactor, không migrate. Fix đúng cái button đó thôi."*

---

### 🚩 Red Flag #2: "Hallucinated Architecture" — Bịa ra cấu trúc

```
❌ PLAN TỆ:
Agent plan sử dụng:
  - src/services/AuthService.ts     ← File này không tồn tại
  - src/utils/formatDate.ts         ← Cũng không tồn tại
  - src/hooks/usePermission.ts      ← Cũng không
```

**Vấn đề**: Agent **bịa ra** file paths, function names, hoặc APIs không có trong codebase.

**Cách xử lý**: *"Kiểm tra lại — các file sau có thực sự tồn tại trong project không? Hãy list files hiện tại trước khi plan."*

---

### 🚩 Red Flag #3: "Dependency Happy" — Thêm library vô tội vạ

```
❌ PLAN TỆ:
Bạn nhờ: "Thêm date picker"
Agent plan:
  1. npm install moment dayjs date-fns luxon
  2. npm install react-datepicker @mui/x-date-pickers
  3. npm install @radix-ui/react-popover
```

**Vấn đề**: Thêm 7 dependencies cho một tính năng. Mỗi dependency là thêm bundle size, attack surface, và maintenance burden.

**Cách xử lý**: *"Project đang dùng date-fns rồi, không thêm moment/dayjs. Chọn 1 datepicker component thôi, giải thích tại sao chọn nó."*

---

### 🚩 Red Flag #4: "Vague Steps" — Mơ hồ, không cụ thể

```
❌ PLAN TỆ:
Bước 1: Cải thiện performance
Bước 2: Tối ưu hoá code
Bước 3: Fix các bugs
Bước 4: Clean up
```

**Vấn đề**: Mỗi bước đều **không nói CỤ THỂ** sẽ làm gì. Không file name, không function name, không metric.

**Cách xử lý**: *"Plan quá mơ hồ. Mỗi bước cần có: file nào bị ảnh hưởng, thay đổi cụ thể là gì, và expected outcome."*

---

### 🚩 Red Flag #5: "Wrong Pattern" — Dùng sai pattern của project

```
❌ PLAN TỆ (trong project đang dùng Redux Toolkit):
Agent plan:
  1. Tạo React Context cho state management
  2. Dùng useReducer + custom hooks
  3. Share state qua Provider
```

**Vấn đề**: Project đã có hệ thống state management rõ ràng (Redux), nhưng agent tự ý dùng pattern khác. Dẫn đến **2 hệ thống quản lý state chồng chéo nhau**.

**Cách xử lý**: *"Project dùng Redux Toolkit. Follow pattern hiện có: tạo slice trong src/store/slices/, không tạo Context mới."*

---

### 🚩 Red Flag #6: "Confident but Wrong" — Tự tin nhưng sai

```
❌ PLAN TỆ:
"API endpoint /api/users đã hỗ trợ pagination 
 qua query params page và limit"
→ Thực tế API KHÔNG có pagination
```

**Vấn đề**: Agent **phát biểu rất tự tin** về thứ nó **không biết chắc**. Đây là dạng hallucination nguy hiểm nhất vì bạn dễ tin.

**Cách xử lý**: *"Verify giúp tôi — mở file route handler của /api/users và xác nhận pagination có thực sự được implement chưa."*

---

### 🚩 Red Flag #7: "Delete and Rewrite" — Xoá hết viết lại

```
❌ PLAN TỆ:
"Xoá toàn bộ component UserProfile hiện tại 
 và viết lại từ đầu với kiến trúc mới"
```

**Vấn đề**: Agent muốn **xoá sạch code đang hoạt động** và viết lại. Rủi ro cực cao: mất edge cases đã xử lý, mất business logic ẩn, regression bugs.

**Cách xử lý**: *"KHÔNG xoá và viết lại. Sửa incremental — giữ code hiện tại, thay đổi từng phần một. Giải thích cụ thể phần nào cần thay đổi và tại sao."*

---

### Bảng tóm tắt Red Flags

| # | Red Flag | Dấu hiệu nhận biết | Mức nguy hiểm |
|---|----------|-------------------|---------------|
| 1 | Boil the Ocean | Task nhỏ → plan khổng lồ | 🟡 Trung bình |
| 2 | Hallucinated Architecture | Đề cập file/function không tồn tại | 🔴 Cao |
| 3 | Dependency Happy | Thêm nhiều library không cần thiết | 🟡 Trung bình |
| 4 | Vague Steps | Plan mơ hồ, không cụ thể | 🟠 Khá cao |
| 5 | Wrong Pattern | Sai convention/pattern của project | 🔴 Cao |
| 6 | Confident but Wrong | Khẳng định sai về codebase | 🔴 Rất cao |
| 7 | Delete and Rewrite | Xoá code đang chạy, viết lại | 🔴 Rất cao |

---

## 7. Plan tốt trông như thế nào?

### Tiêu chí của một Plan chất lượng

```
✅ PLAN TỐT bao gồm:

┌─ 1. ANALYSIS ──────────────────────────────────┐
│  • Tóm tắt lại yêu cầu bằng lời agent        │
│  • Liệt kê files đã đọc để hiểu context       │
│  • Xác định constraints & limitations          │
└────────────────────────────────────────────────┘
         │
         ▼
┌─ 2. APPROACH ──────────────────────────────────┐
│  • Giải pháp được chọn + LÝ DO chọn           │
│  • Alternatives đã cân nhắc + tại sao loại    │
│  • Trade-offs rõ ràng                          │
└────────────────────────────────────────────────┘
         │
         ▼
┌─ 3. DETAILED STEPS ───────────────────────────┐
│  • Từng bước CỤ THỂ: file nào, dòng nào      │
│  • Thứ tự logic (dependencies đúng)            │
│  • Code snippets minh họa khi cần              │
└────────────────────────────────────────────────┘
         │
         ▼
┌─ 4. IMPACT ASSESSMENT ────────────────────────┐
│  • Files changed / created / deleted           │
│  • Breaking changes (nếu có)                   │
│  • Dependencies added/removed                  │
│  • Test cases cần update                       │
└────────────────────────────────────────────────┘
```

### Ví dụ Plan tốt vs Plan tệ

````
❌ PLAN TỆ:
"Sẽ thêm search feature cho user list"

---

✅ PLAN TỐT:

## Task: Thêm search cho User List page

### Phân tích
- `UserList.tsx` hiện render danh sách user từ `useUsers()` hook
- Data được fetch từ `GET /api/users` — API đã hỗ trợ 
  query param `?search=` (verified trong `routes/users.ts:45`)
- Hiện tại không có search UI

### Approach
**Server-side search** qua API query param (không client-side filter)
- Lý do: Danh sách có thể lên đến 10k users, filter client-side sẽ lag
- Trade-off: Cần debounce input để tránh spam requests

### Các bước

1. **Thêm search param vào `useUsers` hook** (`src/hooks/useUsers.ts`)
   - Thêm parameter `searchQuery?: string`
   - Append `?search=${searchQuery}` vào API URL
   - Update React Query key để invalidate khi search thay đổi

2. **Tạo SearchInput component** (`src/components/SearchInput.tsx`)
   - Input field với debounce 300ms (dùng `useDebouncedValue` đã có)
   - Search icon, clear button
   - Follow design pattern của `FilterDropdown` component hiện có

3. **Integrate vào UserList** (`src/pages/UserList.tsx`)
   - Thêm SearchInput phía trên table
   - Pass search value vào useUsers hook
   - Hiển thị "Không tìm thấy kết quả" khi empty

### Ảnh hưởng
- 3 files thay đổi, 1 file tạo mới
- Không thêm dependency
- Không breaking changes
- Cần thêm 1 test case cho search functionality
````

---

## 8. Workflow thực tế: Plan → Review → Execute

### Quy trình khuyến nghị

```
┌──────────────────────────────────────────────────────────┐
│                    YOUR WORKFLOW                          │
│                                                          │
│  ① Describe task rõ ràng                                │
│     ↓                                                    │
│  ② Yêu cầu Plan Mode (không execute)                   │
│     ↓                                                    │
│  ③ Đọc plan với 5 câu hỏi checklist                    │
│     ↓                                                    │
│  ④ Phản biện / yêu cầu điều chỉnh                      │
│     ↓    ↑                                               │
│     └────┘ (lặp cho đến khi hài lòng)                   │
│     ↓                                                    │
│  ⑤ Approve plan → Agent execute                         │
│     ↓                                                    │
│  ⑥ Review kết quả → Test → Commit                      │
└──────────────────────────────────────────────────────────┘
```

### Khi nào nên dùng Plan Mode?

| Tình huống | Plan Mode? | Lý do |
|-----------|-----------|-------|
| Fix typo trong UI | ❌ Không cần | Quá đơn giản |
| Thêm field vào form | ⚠️ Tuỳ | Nếu form đơn giản thì không, form phức tạp thì có |
| Implement feature mới | ✅ Nên dùng | Cần đảm bảo approach đúng |
| Refactor module | ✅ Bắt buộc | Rủi ro cao, cần review kỹ |
| Fix bug phức tạp | ✅ Nên dùng | Cần hiểu root cause trước khi sửa |
| Thay đổi architecture | ✅ Bắt buộc | Ảnh hưởng toàn bộ project |
| Xoá/thay thế dependency | ✅ Bắt buộc | Ảnh hưởng rộng, regression risk |

---

## 9. Prompt templates hữu ích

### Yêu cầu Plan Mode

```
Tôi muốn [mô tả task]. 

KHÔNG thực thi ngay. Hãy phân tích codebase hiện tại, 
đưa ra plan chi tiết với:
1. Phân tích hiện trạng (files nào liên quan, pattern đang dùng)
2. Approach và lý do chọn approach đó
3. Từng bước cụ thể (file nào, thay đổi gì)
4. Ảnh hưởng và rủi ro

Chờ tôi review xong mới bắt đầu implement.
```

### Yêu cầu so sánh Approach

```
Cho tôi 2-3 approach khác nhau cho task [mô tả].
Với mỗi approach, nêu:
- Ưu điểm
- Nhược điểm  
- Phù hợp khi nào
- Ước tính số files thay đổi

Recommend approach nào và giải thích lý do.
```

### Phản biện Plan

```
Review lại plan:
1. File X có thực sự tồn tại không? Verify giúp tôi.
2. Tại sao không dùng [pattern/approach khác]?
3. Step 3 nên đặt trước Step 2 vì [lý do].
4. Thiếu xử lý edge case: [mô tả].
5. Scope quá rộng — bỏ bước [X] ra, chỉ focus vào [Y].
```

### Kiểm soát scope

```
QUAN TRỌNG: Chỉ thay đổi những gì TRỰC TIẾP liên quan đến task.
- KHÔNG refactor code không liên quan
- KHÔNG thêm dependency mới nếu không thực sự cần  
- KHÔNG thay đổi naming convention, formatting, style hiện tại
- Follow ĐÚNG pattern và convention đang có trong project
```

---

## Tổng kết

```
┌─────────────────────────────────────────────────────────┐
│                    KEY TAKEAWAYS                         │
│                                                          │
│  🤖 Agent = LLM + Tools + Planning + Memory loop        │
│                                                          │
│  📋 Plan Mode = Agent chỉ lên kế hoạch, không thực thi │
│                                                          │
│  📖 Đọc Plan = 5 câu hỏi: Hiểu đúng? Approach đúng?   │
│     Scope hợp lý? Thứ tự logic? Side effects?           │
│                                                          │
│  🔍 Phản biện = Zoom in, Zoom out, Thách thức giả định, │
│     Yêu cầu alternatives, Hỏi edge cases               │
│                                                          │
│  🚩 7 Red Flags = Boil the Ocean, Hallucinated,         │
│     Dependency Happy, Vague, Wrong Pattern,             │
│     Confident but Wrong, Delete & Rewrite               │
│                                                          │
│  💡 Plan Mode là kỹ năng sống còn —                     │
│     nó biến bạn từ "người xem AI code"                  │
│     thành "Tech Lead review thiết kế"                   │
└─────────────────────────────────────────────────────────┘
```

---

## 📎 Tài liệu tham khảo

- [Building Effective Agents — Anthropic](https://www.anthropic.com/research/building-effective-agents)
- [ReAct: Synergizing Reasoning and Acting](https://arxiv.org/abs/2210.03629)
- [The AI-Assisted Developer Workflow](https://github.blog/ai-and-ml/github-copilot/)
- [Cursor AI Documentation](https://docs.cursor.com)

---
*Cập nhật: 2026-04-01*
