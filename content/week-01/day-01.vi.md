+++
title = "Ngày 01 - 15/09/2026 (onsite)"
weight = 1
+++

## Topics Learned

### Git

#### Các câu lệnh phổ biến

| Lệnh               | Mô Tả                                            |
| ------------------ | ------------------------------------------------ |
| git init           | Khởi tạo kho lưu trữ Git mới                     |
| git remote         | Quản lý kết nối kho lưu trữ từ xa                |
| git clone          | Sao chép kho lưu trữ từ xa về máy cục bộ         |
| git fetch          | Tải các thay đổi từ xa mà không hợp nhất         |
| git pull           | Tải và hợp nhất các thay đổi từ xa               |
| git status         | Hiển thị trạng thái hiện tại của kho lưu trữ     |
| git branch         | Liệt kê, tạo hoặc xóa các nhánh                  |
| git switch         | Chuyển sang nhánh khác                           |
| git checkout       | Chuyển nhánh hoặc khôi phục tệp thư mục làm việc |
| git add            | Chuẩn bị các thay đổi để commit                  |
| git commit         | Ghi lại các thay đổi vào kho lưu trữ             |
| git commit --amend | Sửa đổi commit cuối cùng                         |
| git push           | Tải các commit cục bộ lên từ xa                  |
| git reset          | Bỏ chuẩn bị hoặc đặt lại các commit              |
| git rebase         | Áp dụng lại các commit trên một nhánh khác       |
| git rebase -i      | Rebase tương tác để chỉnh sửa các commit         |
| git stash          | Lưu các thay đổi chưa commit tạm thời            |
| git stash pop      | Khôi phục các thay đổi đã lưu trữ                |
| git merge          | Kết hợp các thay đổi từ nhánh khác               |
| git cherry-pick    | Áp dụng các commit cụ thể từ nhánh khác          |

#### Xử Lý Xung Đột Git

| Tình Huống                                | Giải Pháp (Source Control)                                                                         |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------- |
| Giữ lại thay đổi từ cả hai nhánh          | Mở tệp trong trình soạn thảo, chỉnh sửa thủ công để bao gồm cả hai thay đổi, rồi nhập vào dấu ✓    |
| Giữ lại thay đổi từ nhánh hiện tại        | Di chuột qua dấu xung đột và nhập nút "Accept Current Change"                                      |
| Giữ lại thay đổi từ nhánh đến             | Di chuột qua dấu xung đột và nhập nút "Accept Incoming Change"                                     |
| Hủy hợp nhất và bắt đầu lại               | Nhập biểu tượng Source Control ở thanh bên, rồi nhập menu "..." và chọn "Abort Merge"              |
| Giải quyết xung đột trong trình soạn thảo | Xung đột được đánh dấu bằng màu sắc, chỉnh sửa thủ công hoặc sử dụng giao diện giải quyết xung đột |

---

## Công việc đã hoàn thành
- Thiết lập các công cụ giao tiếp và quản lý công việc của nhóm: Slack, Taiga.
- Clone repository mẫu của chương trình trainee, cài đặt Hugo Extended (`v0.166.0`), và khởi tạo trang ghi chép cá nhân.
- Cấu hình file `config.toml` (cập nhật `baseURL` và `author`) khớp với repository cá nhân `khackhoan123/LAZTAR-PEEP-2026-Ta-Khac-Khoan`.
- Triển khai (deploy) trang ghi chép tĩnh lên GitHub Pages thông qua quy trình tự động hóa GitHub Actions.
- Ôn tập và thực hành các lệnh Git cốt lõi, quy trình giải quyết xung đột mã nguồn (merge conflict).

## Khó khăn gặp phải & Hướng giải quyết
- **Vấn đề:** Không thể cài đặt Hugo qua công cụ quản lý gói của Windows (`winget install Hugo.Hugo.Extended`) do môi trường máy chưa nhận diện lệnh `winget` (`'winget' is not recognized`).
- **Giải quyết:** Chuyển sang phương án tự động hóa qua PowerShell: tải trực tiếp bản nén nhị phân chính thức (`hugo_extended_0.166.0_windows-amd64.zip`) từ GitHub Releases, giải nén vào `C:\Users\takha\bin`, và thêm thư mục này vào biến môi trường `PATH` của người dùng. Sau đó xác nhận cài đặt thành công với lệnh `hugo version`.

## Kế hoạch tiếp theo
- Hoàn thiện thực hành toàn bộ chu trình Git Flow trên repository nháp (tạo nhánh, tạo PR, review, merge và giả lập xử lý conflict).
- Chuẩn bị môi trường làm việc và tiếp tục các nội dung đào tạo cho Ngày 02.

## Hình ảnh thực hành

![Các thao tác Git cơ bản](/images/day01/git-basic.png)

![Thực hành Stash và xử lý Merge Conflict](/images/day01/git-conflict.png)

#### Xử lý xung đột (Merge Conflict) trên VS Code

- **Phát hiện xung đột:** Trình soạn thảo VS Code đánh dấu rõ vùng xung đột giữa nhánh `HEAD` và nhánh `feature/login` kèm các tùy chọn xử lý nhanh (`Accept Current Change`, `Accept Incoming Change`,...).
![Phát hiện xung đột trên VS Code](/images/day01/vscode-conflict.png)

- **Đã giải quyết xung đột:** Áp dụng lựa chọn nhận thay đổi từ nhánh merge (`Accept Incoming Change`), loại bỏ toàn bộ thẻ đánh dấu xung đột và hoàn thiện cú pháp.
![Xử lý xung đột hoàn tất trên VS Code](/images/day01/vscode-resolved.png)