# Git Conflict Resolution Guide / Hướng Dẫn Xử Lý Conflict Git

## Understanding Conflicts / Hiểu Về Conflict

### What is a Git Conflict? / Conflict Git là gì?

**English**: A conflict occurs when Git cannot automatically merge changes because two people modified the same lines in a file, or one person deleted a file while another modified it.

**Tiếng Việt**: Conflict xảy ra khi Git không thể tự động merge các thay đổi vì hai người đã sửa cùng một dòng trong file, hoặc một người xóa file trong khi người khác sửa nó.

---

## Common Conflict Scenarios / Các Tình Huống Conflict Thường Gặp

### Scenario 1: Push Rejected / Push Bị Từ Chối

**English**:

```bash
# Error message
! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'repository-url'
```

**Tiếng Việt**: Lỗi này xảy ra khi remote repository có commits mới mà local chưa có.

**Solution / Giải pháp**:

```bash
# Step 1: Fetch latest changes
git fetch origin

# Step 2: Pull and merge
git pull origin main

# Step 3: Resolve conflicts if any (see below)
# Step 4: Push again
git push origin main
```

---

## Step-by-Step Conflict Resolution / Các Bước Xử Lý Conflict

### Method 1: Manual Resolution / Xử Lý Thủ Công

**Step 1: Identify conflicts / Xác định conflicts**

```bash
git status
```

**Step 2: Open conflicted files / Mở các file bị conflict**

Conflict markers look like / Dấu hiệu conflict:

```
<<<<<<< HEAD
Your changes
=======
```

Their changes

> > > > > > > branch-name

````

**English**:
- `<<<<<<< HEAD`: Your current changes
- `=======`: Separator
- `>>>>>>> branch-name`: Incoming changes

**Tiếng Việt**:
- `<<<<<<< HEAD`: Thay đổi của bạn
- `=======`: Dấu phân cách
- `>>>>>>> branch-name`: Thay đổi từ remote

**Step 3: Edit the file / Sửa file**

Choose one of these options / Chọn một trong các cách:

1. **Keep your changes / Giữ thay đổi của bạn**:
```javascript
// Remove conflict markers and keep only your code
function myFunction() {
  // Your implementation
}
````

2. **Accept their changes / Chấp nhận thay đổi của họ**:

```javascript
// Remove conflict markers and keep only their code
function myFunction() {
  // Their implementation
}
```

3. **Combine both / Kết hợp cả hai**:

```javascript
// Keep the best parts from both versions
function myFunction() {
  // Combined implementation
}
```

**Step 4: Mark as resolved / Đánh dấu đã giải quyết**

```bash
git add <file-name>
```

**Step 5: Complete the merge / Hoàn thành merge**

```bash
git commit -m "Resolve merge conflicts"
git push origin main
```

---

### Method 2: Using VS Code / Sử dụng VS Code

**English**: VS Code provides visual tools for conflict resolution.

**Tiếng Việt**: VS Code cung cấp công cụ trực quan để xử lý conflict.

**Steps / Các bước**:

1. Open conflicted file in VS Code
2. You'll see options above each conflict:
   - `Accept Current Change` (your changes)
   - `Accept Incoming Change` (their changes)
   - `Accept Both Changes`
   - `Compare Changes`

3. Click the appropriate option
4. Save the file
5. Stage and commit:

```bash
git add .
git commit -m "Resolve conflicts using VS Code"
git push
```

---

### Method 3: Using Git Tools / Sử dụng Git Tools

**Option A: Accept all theirs / Chấp nhận tất cả của họ**

```bash
git checkout --theirs <file-name>
git add <file-name>
```

**Option B: Accept all yours / Giữ tất cả của bạn**

```bash
git checkout --ours <file-name>
git add <file-name>
```

**Option C: Use merge tool / Dùng merge tool**

```bash
git mergetool
```

---

## Advanced Scenarios / Tình Huống Nâng Cao

### Scenario 2: Merge Conflict During Pull / Conflict Khi Pull

```bash
# You see this error
Auto-merging file.js
CONFLICT (content): Merge conflict in file.js
Automatic merge failed; fix conflicts and then commit the result.
```

**Solution / Giải pháp**:

```bash
# 1. Check which files have conflicts
git status

# 2. Resolve conflicts in each file (see Method 1 above)

# 3. Stage resolved files
git add <resolved-files>

# 4. Complete the merge
git commit -m "Merge branch 'main' and resolve conflicts"

# 5. Push
git push origin main
```

---

### Scenario 3: Rebase Conflict / Conflict Khi Rebase

```bash
git rebase main
# CONFLICT appears
```

**Solution / Giải pháp**:

```bash
# 1. Resolve conflicts in files

# 2. Stage resolved files
git add <resolved-files>

# 3. Continue rebase
git rebase --continue

# Or abort if you want to start over
git rebase --abort
```

---

### Scenario 4: Cherry-pick Conflict / Conflict Khi Cherry-pick

```bash
git cherry-pick <commit-hash>
# CONFLICT appears
```

**Solution / Giải pháp**:

```bash
# 1. Resolve conflicts

# 2. Stage files
git add <resolved-files>

# 3. Continue cherry-pick
git cherry-pick --continue

# Or abort
git cherry-pick --abort
```

---

## Prevention Strategies / Chiến Lược Phòng Tránh

### English Best Practices:

1. **Pull frequently**: Always pull before starting work

```bash
git pull origin main
```

2. **Commit often**: Make small, frequent commits

```bash
git add .
git commit -m "Small incremental change"
```

3. **Communicate**: Coordinate with team on who's working on what

4. **Use feature branches**: Work on separate branches

```bash
git checkout -b feature/my-feature
```

5. **Keep branches up to date**:

```bash
git checkout main
git pull
git checkout feature/my-feature
git merge main
```

### Tiếng Việt - Thực Hành Tốt:

1. **Pull thường xuyên**: Luôn pull trước khi bắt đầu làm việc
2. **Commit thường xuyên**: Tạo các commits nhỏ, thường xuyên
3. **Giao tiếp**: Phối hợp với team về ai đang làm gì
4. **Dùng feature branches**: Làm việc trên các nhánh riêng
5. **Giữ branches cập nhật**: Thường xuyên merge main vào branch của bạn

---

## Useful Commands / Lệnh Hữu Ích

### Check conflict status / Kiểm tra trạng thái conflict

```bash
git status
git diff
```

### View conflicts / Xem conflicts

```bash
git diff --name-only --diff-filter=U
```

### Abort merge / Hủy merge

```bash
git merge --abort
```

### Abort rebase / Hủy rebase

```bash
git rebase --abort
```

### View merge conflicts in a file / Xem conflicts trong file

```bash
git diff <file-name>
```

### List all conflicted files / Liệt kê tất cả files bị conflict

```bash
git diff --name-only --diff-filter=U
```

---

## Conflict Resolution Workflow / Quy Trình Xử Lý Conflict

### English Workflow:

```
1. git pull origin main
   ↓
2. CONFLICT detected
   ↓
3. git status (identify conflicted files)
   ↓
4. Open and edit conflicted files
   ↓
5. Remove conflict markers (<<<, ===, >>>)
   ↓
6. git add <resolved-files>
   ↓
7. git commit -m "Resolve conflicts"
   ↓
8. git push origin main
```

### Tiếng Việt - Quy Trình:

```
1. git pull origin main
   ↓
2. Phát hiện CONFLICT
   ↓
3. git status (xác định files bị conflict)
   ↓
4. Mở và sửa các files bị conflict
   ↓
5. Xóa các dấu conflict (<<<, ===, >>>)
   ↓
6. git add <resolved-files>
   ↓
7. git commit -m "Giải quyết conflicts"
   ↓
8. git push origin main
```

---

## Common Mistakes to Avoid / Lỗi Thường Gặp Cần Tránh

### English:

1. **Don't force push** unless absolutely necessary

```bash
# Dangerous - avoid this
git push -f origin main
```

2. **Don't ignore conflicts** - they won't go away
3. **Don't delete conflict markers** without resolving
4. **Don't commit without testing** after resolving conflicts
5. **Don't panic** - conflicts are normal and fixable

### Tiếng Việt:

1. **Không force push** trừ khi thực sự cần thiết
2. **Không bỏ qua conflicts** - chúng sẽ không tự biến mất
3. **Không xóa dấu conflict** mà không giải quyết
4. **Không commit mà không test** sau khi giải quyết conflicts
5. **Không hoảng sợ** - conflicts là bình thường và có thể sửa được

---

## Emergency Recovery / Khôi Phục Khẩn Cấp

### If you mess up / Nếu bạn làm hỏng

**Undo last commit / Hoàn tác commit cuối**:

```bash
git reset --soft HEAD~1
```

**Discard all local changes / Hủy tất cả thay đổi local**:

```bash
git reset --hard HEAD
```

**Go back to before merge / Quay lại trước khi merge**:

```bash
git reset --hard ORIG_HEAD
```

**Create backup branch / Tạo branch backup**:

```bash
git branch backup-branch
```

---

## Tools & Resources / Công Cụ & Tài Nguyên

### Recommended Tools / Công cụ đề xuất:

- **VS Code**: Built-in merge conflict resolver
- **GitKraken**: Visual Git client
- **Sourcetree**: Free Git GUI
- **Meld**: Visual diff and merge tool
- **P4Merge**: Free merge tool

### Useful Git Aliases / Git aliases hữu ích:

```bash
# Add to ~/.gitconfig
[alias]
    conflicts = diff --name-only --diff-filter=U
    resolve = !git add $(git conflicts)
```

---

## Quick Reference / Tham Khảo Nhanh

| Situation          | Command                        |
| ------------------ | ------------------------------ |
| Check conflicts    | `git status`                   |
| Accept theirs      | `git checkout --theirs <file>` |
| Accept ours        | `git checkout --ours <file>`   |
| Abort merge        | `git merge --abort`            |
| Continue after fix | `git add . && git commit`      |
| View diff          | `git diff <file>`              |

---

## Tips for Team Collaboration / Mẹo Làm Việc Nhóm

### English:

1. Use pull requests for code review
2. Keep PRs small and focused
3. Merge main into your branch regularly
4. Communicate before working on same files
5. Use `.gitignore` properly
6. Write clear commit messages

### Tiếng Việt:

1. Sử dụng pull requests để review code
2. Giữ PRs nhỏ và tập trung
3. Thường xuyên merge main vào branch của bạn
4. Giao tiếp trước khi làm việc trên cùng files
5. Sử dụng `.gitignore` đúng cách
6. Viết commit messages rõ ràng
