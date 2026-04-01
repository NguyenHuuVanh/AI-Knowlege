# Frontend Development Prompts / Prompts Phát Triển Frontend

## English Prompts

### 1. Component Creation

```
Create a React component named [ComponentName] that:
- Uses TypeScript with proper type definitions
- Implements [specific functionality]
- Follows best practices for accessibility (ARIA labels, semantic HTML)
- Includes proper error handling
- Uses modern React patterns (hooks, functional components)
```

### 2. State Management

```
Implement state management for [feature] using [Redux/Zustand/Context API]:
- Define clear action types and reducers
- Include proper TypeScript types
- Add selectors for accessing state
- Implement async actions with proper error handling
```

### 3. API Integration

```
Create an API service for [endpoint]:
- Use axios/fetch with proper error handling
- Implement request/response interceptors
- Add TypeScript interfaces for request/response types
- Include loading and error states
- Add retry logic for failed requests
```

### 4. Form Handling

```
Build a form for [purpose] with:
- Form validation using [Formik/React Hook Form/Zod]
- Proper error messages
- Accessibility features
- Submit handling with loading states
- Field-level and form-level validation
```

### 5. Styling

```
Style [component] using [CSS Modules/Styled Components/Tailwind]:
- Responsive design (mobile-first approach)
- Follow design system guidelines
- Ensure proper contrast ratios for accessibility
- Add hover/focus states
- Support dark mode if applicable
```

### 6. Performance Optimization

```
Optimize [component/feature] for performance:
- Implement code splitting and lazy loading
- Use React.memo/useMemo/useCallback appropriately
- Optimize re-renders
- Add proper loading states
- Implement virtualization for large lists if needed
```

### 7. Testing

```
Write tests for [component/function]:
- Unit tests using Jest and React Testing Library
- Test user interactions and edge cases
- Mock API calls and external dependencies
- Achieve good code coverage
- Follow testing best practices
```

### 8. Code Review

```
Review this code and suggest improvements for:
- Code quality and readability
- Performance optimizations
- Security vulnerabilities
- Accessibility issues
- Best practices adherence
- TypeScript type safety
```

---

## Prompts Tiếng Việt

### 1. Tạo Component

```
Tạo một React component tên [TênComponent] với các yêu cầu:
- Sử dụng TypeScript với định nghĩa type đầy đủ
- Thực hiện [chức năng cụ thể]
- Tuân thủ best practices về accessibility (ARIA labels, semantic HTML)
- Xử lý lỗi đầy đủ
- Sử dụng các pattern React hiện đại (hooks, functional components)
```

### 2. Quản Lý State

```
Triển khai quản lý state cho [tính năng] sử dụng [Redux/Zustand/Context API]:
- Định nghĩa rõ ràng action types và reducers
- Bao gồm TypeScript types đầy đủ
- Thêm selectors để truy cập state
- Triển khai async actions với xử lý lỗi phù hợp
```

### 3. Tích Hợp API

```
Tạo API service cho [endpoint]:
- Sử dụng axios/fetch với xử lý lỗi đầy đủ
- Triển khai request/response interceptors
- Thêm TypeScript interfaces cho request/response types
- Bao gồm loading và error states
- Thêm retry logic cho các request thất bại
```

### 4. Xử Lý Form

```
Xây dựng form cho [mục đích] với:
- Validation sử dụng [Formik/React Hook Form/Zod]
- Thông báo lỗi rõ ràng
- Tính năng accessibility
- Xử lý submit với loading states
- Validation ở cấp field và form
```

### 5. Styling

```
Style cho [component] sử dụng [CSS Modules/Styled Components/Tailwind]:
- Thiết kế responsive (mobile-first approach)
- Tuân theo design system guidelines
- Đảm bảo contrast ratios phù hợp cho accessibility
- Thêm hover/focus states
- Hỗ trợ dark mode nếu có
```

### 6. Tối Ưu Hiệu Năng

```
Tối ưu [component/tính năng] về hiệu năng:
- Triển khai code splitting và lazy loading
- Sử dụng React.memo/useMemo/useCallback hợp lý
- Tối ưu re-renders
- Thêm loading states phù hợp
- Triển khai virtualization cho danh sách lớn nếu cần
```

### 7. Testing

```
Viết tests cho [component/function]:
- Unit tests sử dụng Jest và React Testing Library
- Test các tương tác người dùng và edge cases
- Mock API calls và external dependencies
- Đạt code coverage tốt
- Tuân theo testing best practices
```

### 8. Code Review

```
Review code này và đề xuất cải thiện về:
- Chất lượng code và khả năng đọc hiểu
- Tối ưu hiệu năng
- Lỗ hổng bảo mật
- Vấn đề accessibility
- Tuân thủ best practices
- Type safety với TypeScript
```

---

## Advanced Prompts / Prompts Nâng Cao

### English: Architecture Design

```
Design the architecture for [feature/application]:
- Define folder structure and file organization
- Identify reusable components and utilities
- Plan state management strategy
- Define API layer structure
- Consider scalability and maintainability
- Document key architectural decisions
```

### Tiếng Việt: Thiết Kế Kiến Trúc

```
Thiết kế kiến trúc cho [tính năng/ứng dụng]:
- Định nghĩa cấu trúc thư mục và tổ chức file
- Xác định các components và utilities có thể tái sử dụng
- Lập kế hoạch chiến lược quản lý state
- Định nghĩa cấu trúc API layer
- Xem xét khả năng mở rộng và bảo trì
- Tài liệu hóa các quyết định kiến trúc quan trọng
```

### English: Refactoring

```
Refactor this code to:
- Improve readability and maintainability
- Extract reusable logic into custom hooks/utilities
- Reduce code duplication
- Improve type safety
- Follow SOLID principles
- Maintain existing functionality
```

### Tiếng Việt: Tái Cấu Trúc Code

```
Tái cấu trúc code này để:
- Cải thiện khả năng đọc hiểu và bảo trì
- Tách logic có thể tái sử dụng thành custom hooks/utilities
- Giảm code trùng lặp
- Cải thiện type safety
- Tuân theo nguyên tắc SOLID`
- Duy trì chức năng hiện có
```

---

## Quick Tips / Mẹo Nhanh

### English

- Always specify the framework/library version you're using
- Mention any specific design patterns you want to follow
- Include context about the project (e.g., "This is for an e-commerce app")
- Be specific about requirements and constraints
- Ask for explanations along with the code

### Tiếng Việt

- Luôn chỉ rõ phiên bản framework/library bạn đang sử dụng
- Đề cập các design patterns cụ thể bạn muốn tuân theo
- Bao gồm context về dự án (ví dụ: "Đây là cho ứng dụng thương mại điện tử")
- Cụ thể về yêu cầu và ràng buộc
- Yêu cầu giải thích kèm theo code

---

## Debugging & Fixing Prompts / Prompts Sửa Lỗi

### English: Fix Variable Errors

```
Fix the variable error in this code:
[paste code]

Issues to check:
- Undefined variables
- Variable scope issues
- Naming conflicts
- Type mismatches
- Unused variables
- Hoisting problems
```

### Tiếng Việt: Sửa Lỗi Biến

```
Sửa lỗi biến trong đoạn code này:
[paste code]

Các vấn đề cần kiểm tra:
- Biến chưa được định nghĩa
- Vấn đề về scope của biến
- Xung đột tên biến
- Không khớp kiểu dữ liệu
- Biến không sử dụng
- Vấn đề hoisting
```

### English: Fix Component Errors

```
Debug this React component that's showing [error message]:
[paste component code]

Check for:
- Missing imports
- Incorrect prop types
- Hook usage errors (rules of hooks)
- State update issues
- Event handler problems
- Lifecycle issues
- Key prop warnings
```

### Tiếng Việt: Sửa Lỗi Component

```
Debug React component này đang báo lỗi [thông báo lỗi]:
[paste component code]

Kiểm tra:
- Thiếu imports
- Prop types không đúng
- Lỗi sử dụng hooks (rules of hooks)
- Vấn đề cập nhật state
- Lỗi event handlers
- Vấn đề lifecycle
- Cảnh báo key prop
```

### English: Fix Import/Export Errors

```
Fix import/export errors in [file path]:
[paste code]

Common issues:
- Default vs named imports
- Circular dependencies
- Module not found
- Path resolution issues
- Missing file extensions
```

### Tiếng Việt: Sửa Lỗi Import/Export

```
Sửa lỗi import/export trong [đường dẫn file]:
[paste code]

Các lỗi thường gặp:
- Default vs named imports
- Circular dependencies
- Module không tìm thấy
- Vấn đề path resolution
- Thiếu file extensions
```

### English: Fix TypeScript Errors

```
Resolve TypeScript errors in this file:
[paste code with errors]

Error types to fix:
- Type 'X' is not assignable to type 'Y'
- Property 'X' does not exist on type 'Y'
- Object is possibly 'null' or 'undefined'
- Argument of type 'X' is not assignable to parameter
- Cannot find name 'X'
- Missing return type
```

### Tiếng Việt: Sửa Lỗi TypeScript

```
Giải quyết lỗi TypeScript trong file này:
[paste code with errors]

Các loại lỗi cần sửa:
- Type 'X' is not assignable to type 'Y'
- Property 'X' does not exist on type 'Y'
- Object is possibly 'null' or 'undefined'
- Argument of type 'X' is not assignable to parameter
- Cannot find name 'X'
- Thiếu return type
```

### English: Fix Async/Promise Errors

```
Fix async/await or Promise errors in:
[paste code]

Check for:
- Missing await keyword
- Unhandled promise rejections
- Async function not returning Promise
- Race conditions
- Error handling in async code
- Promise chain issues
```

### Tiếng Việt: Sửa Lỗi Async/Promise

```
Sửa lỗi async/await hoặc Promise trong:
[paste code]

Kiểm tra:
- Thiếu từ khóa await
- Promise rejection chưa xử lý
- Async function không return Promise
- Race conditions
- Xử lý lỗi trong async code
- Vấn đề Promise chain
```

### English: Fix CSS/Styling Issues

```
Fix styling issues in [component name]:
[paste component and styles]

Problems to address:
- Layout breaking on mobile
- Overlapping elements
- Z-index conflicts
- Flexbox/Grid issues
- CSS specificity problems
- Missing vendor prefixes
```

### Tiếng Việt: Sửa Lỗi CSS/Styling

```
Sửa vấn đề styling trong [tên component]:
[paste component and styles]

Các vấn đề cần giải quyết:
- Layout bị vỡ trên mobile
- Các phần tử chồng lên nhau
- Xung đột z-index
- Vấn đề Flexbox/Grid
- Vấn đề CSS specificity
- Thiếu vendor prefixes
```

---

## File & Folder Specific Prompts / Prompts Cụ Thể Theo File/Thư Mục

### English: Analyze Specific File

```
Analyze the file at [file path] and:
- Identify all bugs and potential issues
- Suggest improvements for code quality
- Check for security vulnerabilities
- Verify best practices compliance
- Recommend refactoring opportunities
```

### Tiếng Việt: Phân Tích File Cụ Thể

```
Phân tích file tại [đường dẫn file] và:
- Xác định tất cả bugs và vấn đề tiềm ẩn
- Đề xuất cải thiện chất lượng code
- Kiểm tra lỗ hổng bảo mật
- Xác minh tuân thủ best practices
- Đề xuất cơ hội refactoring
```

### English: Fix All Errors in File

```
Fix all errors in [file path]:
[paste file content or error messages]

Priority order:
1. Syntax errors
2. Type errors
3. Runtime errors
4. Linting warnings
5. Code smell issues
```

### Tiếng Việt: Sửa Tất Cả Lỗi Trong File

```
Sửa tất cả lỗi trong [đường dẫn file]:
[paste file content or error messages]

Thứ tự ưu tiên:
1. Lỗi cú pháp
2. Lỗi kiểu dữ liệu
3. Lỗi runtime
4. Cảnh báo linting
5. Vấn đề code smell
```

### English: Refactor Folder Structure

```
Review and refactor the folder structure at [folder path]:
- Suggest better organization
- Identify misplaced files
- Recommend naming conventions
- Propose component grouping strategy
- Ensure scalability
```

### Tiếng Việt: Tái Cấu Trúc Thư Mục

```
Review và tái cấu trúc thư mục tại [đường dẫn thư mục]:
- Đề xuất tổ chức tốt hơn
- Xác định files đặt sai vị trí
- Đề xuất quy ước đặt tên
- Đề xuất chiến lược nhóm components
- Đảm bảo khả năng mở rộng
```

### English: Fix Component in Specific File

```
Fix the [ComponentName] component in [file path]:
[describe the issue or paste error]

Focus on:
- Component logic errors
- Props validation
- State management issues
- Side effects handling
- Performance problems
- Accessibility issues
```

### Tiếng Việt: Sửa Component Trong File Cụ Thể

```
Sửa component [TênComponent] trong [đường dẫn file]:
[mô tả vấn đề hoặc paste lỗi]

Tập trung vào:
- Lỗi logic component
- Validation props
- Vấn đề quản lý state
- Xử lý side effects
- Vấn đề hiệu năng
- Vấn đề accessibility
```

### English: Debug Specific Function

```
Debug the function [functionName] in [file path]:
[paste function code]

Issues to investigate:
- Logic errors
- Edge cases not handled
- Performance bottlenecks
- Memory leaks
- Incorrect return values
- Side effects
```

### Tiếng Việt: Debug Function Cụ Thể

```
Debug function [tênFunction] trong [đường dẫn file]:
[paste function code]

Các vấn đề cần điều tra:
- Lỗi logic
- Edge cases chưa xử lý
- Điểm nghẽn hiệu năng
- Memory leaks
- Giá trị return không đúng
- Side effects
```

### English: Fix Hook Usage

```
Fix custom hook usage in [file path]:
[paste hook code]

Common hook issues:
- Breaking rules of hooks
- Infinite re-render loops
- Stale closure problems
- Missing dependencies in useEffect
- Incorrect cleanup functions
```

### Tiếng Việt: Sửa Lỗi Sử Dụng Hook

```
Sửa lỗi sử dụng custom hook trong [đường dẫn file]:
[paste hook code]

Các lỗi hook thường gặp:
- Vi phạm rules of hooks
- Vòng lặp re-render vô hạn
- Vấn đề stale closure
- Thiếu dependencies trong useEffect
- Cleanup functions không đúng
```

---

## Context-Aware Debugging / Debug Theo Ngữ Cảnh

### English: Debug with Console Error

```
I'm getting this console error:
[paste exact error message]

In file: [file path]
At line: [line number]

Please:
1. Explain what's causing the error
2. Provide the fix
3. Explain why the fix works
4. Suggest how to prevent similar errors
```

### Tiếng Việt: Debug Với Lỗi Console

```
Tôi đang gặp lỗi console này:
[paste exact error message]

Trong file: [đường dẫn file]
Tại dòng: [số dòng]

Vui lòng:
1. Giải thích nguyên nhân gây lỗi
2. Cung cấp cách sửa
3. Giải thích tại sao cách sửa này hoạt động
4. Đề xuất cách phòng tránh lỗi tương tự
```

### English: Fix Build Errors

```
My build is failing with these errors:
[paste build error output]

Project structure:
[describe or paste relevant folder structure]

Please identify and fix all issues preventing the build.
```

### Tiếng Việt: Sửa Lỗi Build

```
Build của tôi đang thất bại với các lỗi này:
[paste build error output]

Cấu trúc project:
[mô tả hoặc paste cấu trúc thư mục liên quan]

Vui lòng xác định và sửa tất cả vấn đề ngăn build.
```

### English: Fix Linting Errors in Directory

```
Fix all ESLint/Prettier errors in [directory path]:
[paste linting output]

Apply fixes that:
- Follow project's linting rules
- Maintain code functionality
- Improve code quality
- Are consistent across files
```

### Tiếng Việt: Sửa Lỗi Linting Trong Thư Mục

```
Sửa tất cả lỗi ESLint/Prettier trong [đường dẫn thư mục]:
[paste linting output]

Áp dụng các sửa đổi:
- Tuân theo linting rules của project
- Duy trì chức năng code
- Cải thiện chất lượng code
- Nhất quán giữa các files
```
