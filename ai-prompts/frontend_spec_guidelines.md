# Hướng dẫn viết Spec cho Dev Frontend

> Tài liệu này áp dụng cho **mọi dự án frontend** — React, Next.js, Vue, Angular, hoặc bất kỳ framework nào.
> Giúp bạn viết spec đủ rõ để giảm các lỗi thường gặp khi làm việc với AI coding assistant hoặc khi handoff giữa PM, Designer, QA và Frontend Developer.

---

## Mục lục

1. [Mục tiêu của spec frontend](#mục-tiêu-của-spec-frontend)
2. [Khi nào cần spec](#khi-nào-cần-spec)
3. [12 nguyên tắc viết spec hiệu quả](#12-nguyên-tắc-viết-spec-hiệu-quả)
4. [Cấu trúc chuẩn của một spec frontend](#cấu-trúc-chuẩn-của-một-spec-frontend)
5. [Template spec Markdown](#template-spec-markdown)
6. [Ví dụ spec hoàn chỉnh](#ví-dụ-spec-hoàn-chỉnh)
7. [Checklist trước khi giao cho dev hoặc AI](#checklist-trước-khi-giao-cho-dev-hoặc-ai)
8. [Các lỗi viết spec phổ biến cần tránh](#các-lỗi-viết-spec-phổ-biến-cần-tránh)
9. [Quy trình đề xuất khi làm việc với AI](#quy-trình-đề-xuất-khi-làm-việc-với-ai)

---

## Mục tiêu của spec frontend

Một spec frontend tốt không chỉ mô tả UI "trông như thế nào", mà còn phải trả lời được:

- User vào flow này từ đâu?
- User nhìn thấy gì ở từng trạng thái?
- User tương tác như thế nào?
- Hệ thống phản hồi ra sao khi thành công, thất bại, loading hoặc dữ liệu rỗng?
- Điều gì là bắt buộc, điều gì chỉ là tùy chọn?
- Những phần nào tuyệt đối không được phá vỡ?

### Kết quả mong muốn

Spec tốt sẽ giúp:

- Giảm việc hiểu sai yêu cầu
- Giảm bug do edge case bị bỏ sót
- Giảm hiện tượng "fix chỗ này vỡ chỗ kia"
- Dễ viết test, dễ review, dễ maintain
- Dễ dùng với AI coding assistant

---

## Khi nào cần spec

### Có thể chỉ cần prompt ngắn hoặc ticket đơn giản khi

- Thay text hoặc icon nhỏ
- Chỉnh spacing đơn giản
- Sửa màu theo design token đã rõ
- Viết component nhỏ có test rõ ràng

### Nên viết spec khi

- Làm feature mới
- Sửa bug có nhiều tác động liên quan
- Có nhiều state UI
- Có tương tác phức tạp
- Có responsive behavior khác nhau giữa mobile/tablet/desktop
- Có gọi API, cache, optimistic update hoặc retry
- Có yêu cầu accessibility hoặc performance
- Có rủi ro regression
- Có form với validation phức tạp
- Có animation hoặc transition quan trọng

---

## 12 nguyên tắc viết spec hiệu quả

### 1. Mỗi spec chỉ nên tập trung vào một feature hoặc một bugfix

Không nên viết kiểu:

> Cải thiện màn dashboard và tối ưu trải nghiệm orders.

Nên tách thành các spec nhỏ như:

- Filter orders theo date range
- Order detail drawer
- Fix double-submit ở checkout
- Empty state cho notification center

**Mục tiêu:** giảm scope mơ hồ và tránh phá vỡ chức năng khác.

---

### 2. Dùng từ khóa mức độ bắt buộc rõ ràng

Nên quy ước dùng (theo RFC 2119):

- **MUST**: bắt buộc
- **SHOULD**: nên làm, trừ khi có lý do chính đáng
- **MAY**: tùy chọn
- **MUST NOT**: không được phép

Ví dụ:

- Search input **MUST** debounce 300ms.
- Error banner **SHOULD** giữ nguyên đến khi user sửa input.
- Drag-and-drop upload **MAY** được đưa vào phase 2.
- Submit button **MUST NOT** cho phép bấm nhiều lần trong lúc request đang chạy.

---

### 3. Luôn mở đầu bằng Goal, Non-goals, Constraints, Invariants

#### Goal
Feature này giải quyết vấn đề gì?

#### Non-goals
Những gì chủ động không làm trong phạm vi này.

#### Constraints
Các ràng buộc về:

- Design system (Ant Design, Shadcn, Material UI...)
- API hiện có (REST, GraphQL, tRPC...)
- Routing (app router, pages router, file-based...)
- Browser support (Chrome, Safari, Firefox, IE...)
- Thư viện phải dùng / không được dùng
- Performance budget (bundle size, LCP, CLS...)

#### Invariants
Những thứ tuyệt đối không được thay đổi hoặc phá vỡ.

Ví dụ invariants:

- Shortcut bàn phím hiện tại phải giữ nguyên
- Checkout total luôn phải khớp với server-calculated amount
- Shift-click row selection phải tiếp tục hoạt động
- Existing URL params phải backward-compatible

---

### 4. Viết spec theo user scenario, không viết theo component tree

Spec yếu thường viết:

> Tạo modal có 2 input, 1 button, 1 icon close.

Spec tốt nên viết:

- User vào từ đâu
- User thấy gì đầu tiên
- User thao tác gì
- Hệ thống phản hồi thế nào
- Flow kết thúc ra sao

Ví dụ:

1. User bấm "Add address" từ trang checkout
2. Modal mở lên với focus nằm ở trường Full name
3. Khi user submit form hợp lệ, hệ thống gửi request tạo địa chỉ mới
4. Khi thành công, modal đóng và danh sách địa chỉ được refresh
5. Khi lỗi, modal giữ nguyên dữ liệu người dùng đã nhập

---

### 5. Mọi màn hình hoặc component quan trọng phải có state matrix

Tối thiểu hãy mô tả các state sau nếu có:

- Initial
- Loading (phân biệt: initial load, refetch, background refresh)
- Success
- Empty
- Error (network error, validation error, server error)
- Disabled
- Submitting
- Offline
- Unauthorized / Forbidden
- Crashed (error boundary fallback)

Mỗi state nên mô tả:

- User thấy gì?
- User làm được gì?
- Thành phần nào bị disable?
- Điều gì làm chuyển sang state tiếp theo?

Ví dụ:

| State | User thấy gì | User làm được gì | Transition |
|---|---|---|---|
| Initial | Skeleton placeholder | Không tương tác | Khi API bắt đầu gọi |
| Loading | Skeleton 6 dòng | Không click row | Khi API success/error |
| Success | Data table đầy đủ | Click, sort, filter | Khi đổi filter → refetch |
| Empty | Empty illustration + CTA "Create item" | Click CTA | Sang create flow |
| Error | Error banner + nút Retry | Retry | Gọi lại API |
| Submitting | Button loading + form disabled | Không submit lại | Khi request resolve |
| Crashed | Fallback UI "Đã xảy ra lỗi" + nút Tải lại | Click tải lại | Remount component |

#### Loading strategy

Cần phân biệt rõ các kiểu loading:

- **Initial loading**: Lần đầu vào trang → Skeleton hoặc Spinner
- **Refetch loading**: Đổi filter → Giữ data cũ + loading indicator, hay xóa sạch + skeleton?
- **Pagination loading**: Load thêm → Append data hay replace?
- **Action loading**: Submit form → Button spinner + disable form
- **Background loading**: Auto-refresh → Không hiển thị loading cho user

---

### 6. Luôn có interaction contract

Đừng chỉ mô tả giao diện. Hãy mô tả chính xác hành vi tương tác:

- Click làm gì?
- Tap trên mobile làm gì?
- Enter/Space/Esc làm gì?
- Focus ban đầu ở đâu?
- Focus trả về đâu sau khi đóng modal?
- Có cho submit bằng Enter không?
- Double click hoặc double submit xử lý thế nào?
- Long press trên mobile có behavior riêng không?
- Drag and drop có support không?

Ví dụ:

- Pressing `Enter` trong trường Password **MUST** submit form nếu dữ liệu hợp lệ.
- Pressing `Esc` **MUST** đóng modal nếu không có request đang submit.
- Khi modal đóng, focus **MUST** quay về phần tử đã mở modal.
- Double click row **MUST NOT** gửi 2 request.

---

### 7. Viết rõ responsive contract

Không được chỉ ghi "responsive". Cần ghi:

- Breakpoint nào là nguồn sự thật
- Layout thay đổi ra sao theo breakpoint
- Text dài xử lý bằng truncate hay wrap
- Table trên mobile chuyển thành card hay scroll ngang
- Kích thước ảnh giữ tỷ lệ thế nào
- Touch target size tối thiểu (48x48 dp trên mobile)

Ví dụ:

- `< 768px`: login dialog render full-screen sheet
- `>= 768px`: dialog render centered, max-width 480px
- Data table trên mobile **MUST** chuyển sang card list hoặc horizontal scroll
- Product title **MUST** clamp 2 dòng ở mobile và 1 dòng ở desktop list view
- Button touch target **MUST** tối thiểu 44x44px

---

### 8. Accessibility là bắt buộc, không phải bổ sung sau

Ít nhất spec phải nêu rõ:

- Accessible name của control
- Keyboard operability
- Focus visible
- Focus order (tab order)
- ARIA role nếu là custom widget
- Screen reader announcement nếu có async update
- Contrast tối thiểu theo guideline nội bộ hoặc WCAG 2.1 AA
- `prefers-reduced-motion` cho animation

Ví dụ:

- Dialog **MUST** trap focus trong lúc mở.
- Primary CTA **MUST** có accessible name trùng với visible label.
- Error message **MUST** được liên kết với input tương ứng qua `aria-describedby`.
- Focus ring **MUST** luôn nhìn thấy được khi điều hướng bằng bàn phím.
- Toast notification **MUST** dùng `role="alert"` hoặc `aria-live="polite"`.

---

### 9. Tách data contract khỏi UI contract

Frontend spec cần chỉ rõ:

- Endpoint nào được gọi
- Method gì
- Params nào bắt buộc
- Field nào nullable
- Mapping trạng thái HTTP sang UI như thế nào
- Có retry không
- Có optimistic update không
- Có cache invalidate không

#### Với REST API

Ví dụ:

- `GET /api/orders?from=&to=` dùng để lấy danh sách orders
- `401` hiển thị session expired banner
- `403` hiển thị không có quyền truy cập
- `500` hiển thị generic error + Retry
- Response field `customer.avatarUrl` có thể là `null`, UI phải fallback về initials avatar

#### Với GraphQL

Ví dụ:

- `useOrdersQuery({ variables: { filter } })` — eager query, skip khi chưa chọn filter
- `useCreateOrderMutation()` — mutation, invalidate cache `orders` khi success
- Field `customer.phone` **MAY** là `null`, UI **MUST** hiển thị `"-"`
- Fetch policy: `cache-and-network` cho danh sách, `cache-first` cho chi tiết
- Lazy query cho modal/drawer (chỉ gọi khi mở)

#### Mapping lỗi sang UI

| HTTP / GraphQL Error | UI Behavior |
|---|---|
| `401 / UNAUTHENTICATED` | Redirect đến trang login hoặc hiển thị session expired |
| `403 / FORBIDDEN` | Banner "Bạn không có quyền truy cập" |
| `404 / NOT_FOUND` | Empty state hoặc redirect |
| `422 / BAD_USER_INPUT` | Inline validation error trên từng field |
| `500 / INTERNAL_SERVER_ERROR` | Generic error + nút Retry |
| Network error | Offline banner hoặc toast |

---

### 10. Validation contract cho form

Mỗi form field cần ghi rõ:

- Bắt buộc hay tùy chọn
- Validation rule (min/max length, regex, custom logic)
- Thời điểm validate (on blur, on change, on submit, hoặc kết hợp)
- Error message hiển thị gì và ở đâu (inline dưới field, toast, banner)
- Có server-side validation không (ví dụ: check email trùng)

Ví dụ:

| Field | Required | Rule | Validate khi | Error message |
|-------|----------|------|-------------|---------------|
| Email | MUST | Email format (RFC 5322) | On blur + on submit | "Email không hợp lệ" |
| Họ tên | MUST | Min 2, max 100 ký tự | On submit | "Vui lòng nhập họ tên" |
| SĐT | SHOULD | 10-11 chữ số | On blur | "Số điện thoại không đúng định dạng" |
| Mật khẩu | MUST | Min 8, có chữ hoa + số | On change (strength) + on submit | "Mật khẩu quá yếu" |

Quy tắc chung:

- Form **MUST NOT** bị reset khi API trả về lỗi validation.
- Inline error **MUST** xuất hiện ngay dưới field liên quan.
- Focus **SHOULD** nhảy đến field lỗi đầu tiên khi submit thất bại.
- Server-side validation error **MUST** được map về đúng field tương ứng nếu có thể.

---

### 11. Animation và transition contract

Đừng chỉ ghi "có animation". Cần mô tả:

- Transition nào cần animate (modal open/close, tab switch, toast, hover...)
- Duration bao nhiêu ms
- Easing function nào (ease, ease-out, spring...)
- Có respect `prefers-reduced-motion` không (**MUST**)
- Animation có gây layout shift không (**MUST NOT**)

Ví dụ:

- Modal open: `opacity 0→1 + scale 0.95→1`, 150ms, `ease-out`
- Modal close: `opacity 1→0`, 100ms, `ease-in`
- Tab switch: **MUST NOT** có animation gây layout shift
- Toast enter: slide từ phải, 200ms, `ease-out`
- Skeleton → content: **MUST** giữ nguyên kích thước, không gây CLS

Quy tắc chung:

- Khi user bật `prefers-reduced-motion: reduce`, mọi animation **MUST** bị tắt hoặc chuyển thành instant.
- Animation **MUST NOT** vượt quá 400ms cho bất kỳ transition nào.
- Infinite animation (spinner, pulse) **SHOULD** dùng `will-change` hoặc GPU-accelerated properties.

---

### 12. Mỗi spec nên có acceptance criteria đo được

Đừng viết:

- Trang phải mượt
- UX phải tốt
- Dễ sử dụng

Hãy viết:

- Submit button **MUST** bị disable trong lúc request chạy
- Form **MUST NOT** bị reset khi API trả về lỗi validation
- Search results **SHOULD** hiển thị trong vòng 300ms sau khi debounce kết thúc trong điều kiện mock API nội bộ
- Không được phát sinh layout shift (CLS = 0) khi skeleton chuyển sang content
- LCP **SHOULD** dưới 2.5s trên mạng 4G
- Bundle size tăng thêm **MUST NOT** vượt quá 50KB gzipped cho feature mới

Acceptance criteria càng đo được, dev và QA càng dễ xác minh.

---

## Cấu trúc chuẩn của một spec frontend

Dưới đây là cấu trúc khuyến nghị (16 phần):

### 1. Feature
Tên feature rõ ràng.

### 2. Background
Bối cảnh và vấn đề hiện tại.

### 3. Goal
Kết quả user/business mong muốn.

### 4. Non-goals
Những gì nằm ngoài scope.

### 5. Entry points
Route, CTA, screen hoặc nơi user bắt đầu flow.

### 6. User scenarios
Luồng chính, luồng phụ, luồng lỗi.

### 7. UI states
Initial, loading, success, empty, error, disabled, offline, crashed...

### 8. Interaction contract
Keyboard, click, focus, submit, cancel, navigation, double-action prevention.

### 9. Validation contract
Từng field: required, rule, thời điểm validate, error message.

### 10. Responsive contract
Breakpoints và cách layout thay đổi.

### 11. Accessibility contract
Keyboard, focus, screen reader, contrast, motion.

### 12. Animation contract
Transition, duration, easing, reduced-motion, CLS prevention.

### 13. Data contract
API endpoint/query, params, mapping response/error, cache strategy.

### 14. Acceptance criteria
Các câu MUST/SHOULD có thể kiểm thử, có thể đo được.

### 15. Out of scope / Future work
Những gì để phase sau.

### 16. Risks / Open questions
Điều gì còn chưa chắc chắn.

---
## Hướng dẫn cho AI coding assistant
Khi được yêu cầu viết spec hoặc phân tích feature, AI **MUST**:
1. Hỏi clarify nếu yêu cầu còn mơ hồ
2. Output bằng **tiếng Việt**
3. Tuân theo đúng **cấu trúc 16 phần** đã nêu ở trên
4. Dùng từ khóa **MUST / SHOULD / MAY / MUST NOT** cho mọi yêu cầu
5. Bao gồm **state matrix** dạng bảng cho component quan trọng
6. Bao gồm **data contract** với API/GraphQL cụ thể
7. Liệt kê **acceptance criteria** đo được, có thể kiểm thử
Khi được yêu cầu implement code, AI **MUST**:
1. Đọc spec trước nếu có
2. Kiểm tra component dependencies trước khi sửa
3. Tôn trọng mọi **Invariants** đã khai báo
4. Không phá vỡ accessibility contract
5. Đảm bảo loading state và error state được xử lý đầy đủ
6. Không tạo layout shift khi chuyển state

## Template spec Markdown

Sao chép template dưới đây để dùng ngay.

```md
# [Feature Name]

## 1. Background
[Mô tả ngắn về vấn đề hiện tại]

## 2. Goal
- [Goal 1]
- [Goal 2]

## 3. Non-goals
- [Non-goal 1]
- [Non-goal 2]

## 4. Constraints
- [Design system / thư viện / API / browser support / performance budget]

## 5. Invariants
- [Điều tuyệt đối không được phá vỡ]
- [Điều phải tiếp tục giữ nguyên]

## 6. Entry points
- [Route / screen / CTA bắt đầu flow]

## 7. User scenarios
### 7.1 Primary scenario
1. [Step 1]
2. [Step 2]
3. [Step 3]

### 7.2 Secondary scenario
1. [Step 1]
2. [Step 2]

### 7.3 Failure scenario
1. [Step 1]
2. [Step 2]

## 8. UI states
| State | UI hiển thị | User actions | Transition |
|---|---|---|---|
| Initial | [Mô tả] | [Mô tả] | [Mô tả] |
| Loading | [Mô tả] | [Mô tả] | [Mô tả] |
| Success | [Mô tả] | [Mô tả] | [Mô tả] |
| Empty | [Mô tả] | [Mô tả] | [Mô tả] |
| Error | [Mô tả] | [Mô tả] | [Mô tả] |
| Disabled | [Mô tả] | [Mô tả] | [Mô tả] |
| Crashed | [Mô tả] | [Mô tả] | [Mô tả] |

### Loading strategy
- Initial loading: [Skeleton / Spinner]
- Refetch loading: [Giữ data cũ / Xóa + skeleton]
- Pagination loading: [Append / Replace]
- Action loading: [Button spinner + disable]
- Background loading: [Không hiển thị loading]

## 9. Interaction contract
- [Component/action] MUST [hành vi]
- [Component/action] MUST NOT [hành vi không được phép]
- Pressing `Enter` MUST [hành vi]
- Pressing `Esc` SHOULD [hành vi]
- Focus MUST start at [element]
- On close, focus MUST return to [element]
- Double click/submit MUST NOT [gửi nhiều request]

## 10. Validation contract
| Field | Required | Rule | Validate khi | Error message |
|-------|----------|------|-------------|---------------|
| [Field 1] | MUST | [Rule] | [On blur / submit] | "[Message]" |
| [Field 2] | SHOULD | [Rule] | [On change] | "[Message]" |

- Form MUST NOT reset khi API trả lỗi validation
- Focus SHOULD nhảy đến field lỗi đầu tiên khi submit thất bại

## 11. Responsive contract
- `< 768px`: [layout mobile]
- `>= 768px and < 1024px`: [layout tablet]
- `>= 1024px`: [layout desktop]
- Long text MUST [wrap/truncate/clamp]
- Table on mobile MUST [card list / horizontal scroll]
- Touch target MUST tối thiểu 44x44px

## 12. Accessibility contract
- [Control] MUST be keyboard accessible
- [Dialog] MUST trap focus
- [Input] MUST expose accessible label
- [Error] MUST be associated with input via aria-describedby
- Focus indicator MUST remain visible
- [Toast] MUST use role="alert" or aria-live="polite"
- All animations MUST respect prefers-reduced-motion

## 13. Animation contract
- [Modal open]: opacity 0→1 + scale 0.95→1, [duration]ms, [easing]
- [Modal close]: opacity 1→0, [duration]ms, [easing]
- [Toast enter]: slide from right, [duration]ms, [easing]
- Skeleton → content MUST NOT gây layout shift (CLS = 0)
- Animation MUST NOT vượt quá 400ms
- prefers-reduced-motion: reduce → tắt hoặc instant

## 14. Data contract
### 14.1 API (REST)
- `GET /...`
- `POST /...`

### 14.1 API (GraphQL) — nếu dùng
- `useXxxQuery({ variables: { ... } })` — eager/lazy, fetch policy
- `useXxxMutation()` — cache invalidation strategy

### 14.2 Request
- Required params: [...]
- Optional params: [...]

### 14.3 Response mapping
| Status / Error | UI Behavior |
|---|---|
| `200 / success` | [UI behavior] |
| `401 / UNAUTHENTICATED` | [Redirect login / session expired] |
| `403 / FORBIDDEN` | [Banner không có quyền] |
| `404 / NOT_FOUND` | [Empty state / redirect] |
| `422 / BAD_USER_INPUT` | [Inline validation error] |
| `500 / INTERNAL_SERVER_ERROR` | [Generic error + Retry] |
| Network error | [Offline banner / toast] |

### 14.4 Data fallback rules
- Nếu `avatarUrl = null` thì [fallback]
- Nếu `name = null` thì [fallback]

## 15. Acceptance criteria
- [ ] [Câu MUST có thể test]
- [ ] [Câu MUST có thể test]
- [ ] [Câu SHOULD có thể test]
- [ ] LCP SHOULD dưới 2.5s trên mạng 4G
- [ ] Bundle size tăng thêm MUST NOT vượt quá 50KB gzipped
- [ ] CLS = 0 khi chuyển loading → content

## 16. Out of scope
- [Phase sau / không làm trong spec này]

## 17. Risks / Open questions
- [Risk 1]
- [Question 1]
```

---

## Ví dụ spec hoàn chỉnh

# Login Dialog

## 1. Background
Người dùng hiện phải chuyển sang một trang riêng để đăng nhập, làm gián đoạn flow trên landing page và checkout. Cần thêm login dialog để giảm drop-off trong các flow cần xác thực nhanh.

## 2. Goal
- Cho phép user đăng nhập mà không rời khỏi trang hiện tại
- Hỗ trợ đầy đủ desktop và mobile
- Giảm lỗi submit lặp và xử lý lỗi xác thực rõ ràng

## 3. Non-goals
- Không bao gồm đăng nhập bằng social provider ở phase này
- Không hỗ trợ forgot password trong dialog này
- Không thay đổi API auth hiện có

## 4. Constraints
- Phải dùng design system hiện tại
- Phải dùng endpoint đăng nhập hiện có
- Không thêm thư viện form mới
- Hỗ trợ Chrome, Edge, Safari phiên bản hiện hành
- Bundle size tăng thêm **MUST NOT** vượt quá 20KB gzipped

## 5. Invariants
- Flow checkout hiện tại không được thay đổi ngoài việc chèn login dialog khi cần auth
- User không được mất dữ liệu form checkout đã nhập trước đó
- Session handling tiếp tục tuân theo cơ chế auth hiện tại của hệ thống

## 6. Entry points
- CTA "Log in" trên header
- CTA "Continue to checkout" khi user chưa đăng nhập
- CTA "Save wishlist" nếu yêu cầu đăng nhập

## 7. User scenarios

### 7.1 Primary scenario
1. User bấm CTA mở login dialog
2. Dialog mở trên lớp overlay của trang hiện tại
3. Focus nằm ở ô Email
4. User nhập Email và Password hợp lệ
5. User bấm Submit hoặc nhấn Enter trong ô Password
6. Hệ thống gửi request đăng nhập
7. Khi thành công, dialog đóng và user tiếp tục ở trang ban đầu

### 7.2 Secondary scenario
1. User mở dialog từ mobile
2. Dialog hiển thị dạng full-screen sheet
3. User có thể đóng bằng nút Close hoặc gesture/back nếu ứng dụng hỗ trợ

### 7.3 Failure scenario
1. User nhập sai mật khẩu
2. API trả về `401`
3. Dialog giữ nguyên input Email, xóa hoặc giữ Password tùy policy sản phẩm
4. Thông báo lỗi hiển thị bên dưới ô Password
5. User có thể sửa lại và submit lần nữa

## 8. UI states

| State | UI hiển thị | User actions | Transition |
|---|---|---|---|
| Initial | Email input, Password input, Remember me, Submit, Close | Có thể nhập liệu | Sang validating/submitting |
| Invalid form | Inline validation message | Sửa input | Sang valid khi dữ liệu hợp lệ |
| Submitting | Submit button loading, toàn bộ form disabled | Không submit lại | Sang success/error |
| Success | Dialog đóng | Không còn tương tác trong dialog | Về trang trước đó với trạng thái đã đăng nhập |
| 401 Error | Inline error dưới Password | Sửa Password và submit lại | Sang submitting |
| Network error | Generic error banner + Retry | Retry hoặc Close | Sang submitting hoặc close |

## 9. Interaction contract
- Login dialog **MUST** mở dưới dạng modal trên desktop và full-screen sheet trên mobile.
- Focus ban đầu **MUST** nằm ở ô Email.
- Tab order **MUST** là: Email → Password → Remember me → Submit → Close.
- Pressing `Enter` trong ô Password **MUST** submit form nếu dữ liệu hợp lệ.
- Pressing `Esc` **MUST** đóng dialog trên desktop khi không có request đang submit.
- Trong lúc submit, toàn bộ form **MUST** bị disable.
- Submit button **MUST NOT** cho phép double-submit.
- Khi dialog đóng, focus **MUST** quay về phần tử đã mở dialog.

## 10. Validation contract

| Field | Required | Rule | Validate khi | Error message |
|-------|----------|------|-------------|---------------|
| Email | MUST | Email format (RFC 5322) | On blur + on submit | "Vui lòng nhập email hợp lệ" |
| Password | MUST | Min 1 ký tự | On submit | "Vui lòng nhập mật khẩu" |

- Form **MUST NOT** bị reset khi API trả lỗi `401`.
- Focus **SHOULD** nhảy đến field lỗi đầu tiên khi submit thất bại.

## 11. Responsive contract
- `< 768px`: dialog **MUST** render dạng full-screen sheet.
- `>= 768px`: dialog **MUST** render centered với `max-width: 480px`.
- Nội dung lỗi dài **MUST** wrap thay vì overflow.
- Password field và Email field **MUST** full-width ở mọi breakpoint.
- Touch target cho Submit và Close **MUST** tối thiểu 44x44px trên mobile.

## 12. Accessibility contract
- Dialog **MUST** trap focus trong lúc mở.
- Dialog **MUST** có accessible name là "Log in".
- Email và Password inputs **MUST** có label hiển thị và accessible label tương ứng.
- Validation errors **MUST** được liên kết với input liên quan qua `aria-describedby`.
- Focus indicator **MUST** luôn nhìn thấy khi điều hướng bằng bàn phím.
- Close button **MUST** có accessible name rõ nghĩa (ví dụ: "Close login dialog").
- Khi user bật `prefers-reduced-motion: reduce`, animation **MUST** bị tắt.

## 13. Animation contract
- Modal open: `opacity 0→1 + scale 0.95→1`, 150ms, `ease-out`
- Modal close: `opacity 1→0`, 100ms, `ease-in`
- Mobile sheet: slide up from bottom, 200ms, `ease-out`
- Skeleton → content: **MUST** giữ nguyên kích thước (CLS = 0)
- Khi `prefers-reduced-motion: reduce`, tất cả animation → instant (0ms)

## 14. Data contract

### 14.1 API
- `POST /api/auth/login`

### 14.2 Request body
```json
{
  "email": "string",
  "password": "string",
  "rememberMe": true
}
```

### 14.3 Response mapping

| Status | UI Behavior |
|---|---|
| `200` | Lưu session, đóng dialog, refresh auth state |
| `400` | Hiển thị validation error nếu server trả field errors |
| `401` | Hiển thị "Sai email hoặc mật khẩu" |
| `429` | Hiển thị rate limit message |
| `500` | Generic error banner + cho phép retry |
| Network error | Giữ nguyên form, hiển thị offline banner |

### 14.4 Data fallback rules
- Nếu response không có `displayName`, UI header fallback về email đã đăng nhập
- Nếu request timeout, giữ nguyên form values để user thử lại

## 15. Acceptance criteria
- [ ] Dialog mở từ mọi entry point đã nêu mà không rời khỏi trang hiện tại
- [ ] Focus ban đầu luôn nằm ở Email input
- [ ] Nhấn Enter trong Password submit được form khi form hợp lệ
- [ ] Trong lúc submit, không thể bấm submit nhiều lần
- [ ] Khi API trả `401`, Email vẫn được giữ nguyên
- [ ] Khi đóng dialog, focus quay về đúng phần tử đã mở dialog
- [ ] Trên mobile, dialog hiển thị full-screen
- [ ] Trên desktop, dialog hiển thị centered với max-width 480px
- [ ] CLS = 0 khi dialog mở/đóng
- [ ] Animation tắt khi prefers-reduced-motion: reduce

## 16. Out of scope
- Social login
- Forgot password flow
- Captcha
- Biometric/passkey authentication

## 17. Risks / Open questions
- Password có nên bị xóa sau `401` hay giữ nguyên?
- Có cần hỗ trợ magic link trong phase sau không?
- Cần tracking event analytics nào cho open, submit, success, error?
- Rate limiting: sau bao nhiêu lần thất bại thì lock tạm thời?

---

## Checklist trước khi giao cho dev hoặc AI

### Checklist logic

- [ ] Spec có đúng 1 feature hoặc 1 bugfix chính
- [ ] Goal và Non-goals rõ ràng
- [ ] Có ghi các invariants
- [ ] Có mô tả primary flow và failure flow
- [ ] Có state matrix (bao gồm crashed state)
- [ ] Có loading strategy (initial / refetch / action / background)
- [ ] Có interaction contract
- [ ] Có validation contract (nếu có form)
- [ ] Có responsive contract
- [ ] Có accessibility contract
- [ ] Có animation contract (nếu có transition)
- [ ] Có data contract
- [ ] Có acceptance criteria đo được

### Checklist chất lượng câu chữ

- [ ] Không dùng từ mơ hồ như "đẹp", "mượt", "thân thiện", "dễ dùng" mà không định nghĩa
- [ ] Mỗi câu yêu cầu đều có thể được QA kiểm thử
- [ ] Câu nào bắt buộc dùng MUST
- [ ] Câu nào tùy chọn dùng MAY
- [ ] Không trộn lẫn requirement với implementation detail không cần thiết

### Checklist chống regression

- [ ] Đã ghi rõ thứ gì không được thay đổi
- [ ] Đã mô tả behavior khi lỗi
- [ ] Đã mô tả behavior với dữ liệu null hoặc rỗng
- [ ] Đã mô tả behavior khi loading hoặc submitting
- [ ] Đã mô tả fallback cho mobile
- [ ] Đã ghi rõ animation respect `prefers-reduced-motion`

---

## Các lỗi viết spec phổ biến cần tránh

### 1. Chỉ mô tả UI tĩnh
Sai:

> Có 1 modal đăng nhập với 2 ô input.

Thiếu:

- Enter có submit không?
- Loading ra sao?
- Lỗi API hiển thị đâu?
- Focus đi đâu?

### 2. Viết requirement không đo được
Sai:

> Màn này phải thân thiện và mượt.

Đúng hơn:

> Trong lúc submit, form phải disable để tránh double-submit. LCP SHOULD dưới 2.5s.

### 3. Không ghi rõ out of scope
Khi không ghi rõ, dev hoặc AI dễ tự thêm logic ngoài ý muốn.

### 4. Không mô tả error handling
Đây là nguyên nhân phổ biến làm feature demo thì chạy nhưng production thì lỗi.

### 5. Không mô tả responsive
"Responsive" tự thân không có ý nghĩa nếu không chỉ rõ layout behavior.

### 6. Không mô tả invariants
Rất dễ xảy ra tình trạng hoàn thành feature mới nhưng phá flow cũ.

### 7. Không phân biệt các kiểu loading
Ghi "có loading" mà không phân biệt initial load, refetch, background refresh sẽ dẫn đến UX không nhất quán.

### 8. Bỏ qua animation / transition
Không spec animation dẫn đến: dev tự chọn duration/easing khác nhau, không respect `prefers-reduced-motion`, gây layout shift.

### 9. Không mô tả validation timing
Ghi "validate email" nhưng không nói rõ validate on blur hay on submit sẽ dẫn đến behavior khác nhau giữa các developer.

---

## Quy trình đề xuất khi làm việc với AI

### Bước 1. Viết spec trước
Không bắt đầu bằng prompt mơ hồ kiểu:

> Tạo cho tôi login modal đẹp, responsive, dễ dùng.

Hãy bắt đầu bằng spec tối thiểu theo cấu trúc 16 phần.

### Bước 2. Yêu cầu AI triển khai theo spec
Ví dụ prompt tốt hơn:

> Implement the feature exactly according to the spec below. Do not change any behavior not explicitly mentioned. Preserve all listed invariants. If any requirement is ambiguous, list assumptions separately.

### Bước 3. Yêu cầu AI tạo test từ acceptance criteria
Ví dụ:

> Based on the acceptance criteria below, generate unit tests and integration test cases. Do not add new product behavior.

### Bước 4. Review theo checklist
Không review theo cảm giác. Review theo:

- flow
- states (bao gồm crashed và offline)
- keyboard
- responsive
- validation timing
- animation / prefers-reduced-motion
- API mapping
- regressions

### Bước 5. Nếu bug xuất hiện, sửa spec trước khi sửa code lớn
Nếu root cause là requirement mơ hồ, hãy chỉnh spec rồi mới chỉnh code.

---

## Hướng dẫn cho AI coding assistant

Khi được yêu cầu viết spec hoặc phân tích feature, AI **MUST**:

1. Hỏi clarify nếu yêu cầu còn mơ hồ
2. Output bằng **tiếng Việt**
3. Tuân theo đúng **cấu trúc 16 phần** đã nêu ở trên
4. Dùng từ khóa **MUST / SHOULD / MAY / MUST NOT** cho mọi yêu cầu
5. Bao gồm **state matrix** dạng bảng cho component quan trọng
6. Bao gồm **data contract** với API/GraphQL cụ thể
7. Liệt kê **acceptance criteria** đo được, có thể kiểm thử

Khi được yêu cầu implement code, AI **MUST**:

1. Đọc spec trước nếu có
2. Kiểm tra component dependencies trước khi sửa
3. Tôn trọng mọi **Invariants** đã khai báo
4. Không phá vỡ accessibility contract
5. Đảm bảo loading state và error state được xử lý đầy đủ
6. Không tạo layout shift khi chuyển state

---

## Kết luận

Với frontend, spec tốt là công cụ chống hỗn loạn. Nó giúp bạn giữ được ý đồ sản phẩm, kiểm soát hành vi UI, giảm hiểu sai và hạn chế bug phát sinh khi hệ thống lớn dần.

Nếu chỉ nhớ 5 điều, hãy nhớ:

1. Mỗi spec chỉ nên ôm 1 việc rõ ràng
2. Dùng MUST / SHOULD / MAY để tránh mơ hồ
3. Luôn mô tả states, interaction, và validation
4. Responsive, accessibility, animation và error handling phải viết rõ
5. Mỗi requirement phải test được

---

## Gợi ý đặt tên file cho team

- `spec-login-dialog.md`
- `spec-order-filter-date-range.md`
- `spec-checkout-double-submit-fix.md`
- `spec-notification-empty-state.md`
- `spec-dashboard-loading-strategy.md`
