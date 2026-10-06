+++
title = "Ngày 04 - 01/10/2026 (Remote)"
weight = 4
+++

## BÁO CÁO TIẾN ĐỘ: HOẠCH ĐỊNH MA TRẬN PHÂN RÃ WBS, THIẾT LẬP HẠ TẦNG CLOUDINARY & REVIEW CODE KIẾN TRÚC RBAC

---

### 1. Các Công Việc Đã Hoàn Thành (Completed Tasks)

#### Hoạch Định Ma Trận Phân Rã Chức Năng (WBS) & Kích Hoạt Frontend
- **Xây dựng bảng phân rã công việc (Work Breakdown Structure):** 
  - Khởi tạo hệ thống ma trận theo dõi tiến độ chi tiết bám sát chu trình vận hành kho nông sản: Master Data $\rightarrow$ Inbound $\rightarrow$ Putaway $\rightarrow$ Allocation & Outbound.
  - Thiết lập phân cấp độ ưu tiên (P0, P1, P2), thời hạn bàn giao cụ thể, và cơ chế tự chủ nhận việc theo domain cho từng thành viên (BE/FE) để tránh chồng chéo phạm vi.
- **Bàn giao giao diện & Kích hoạt nhánh Frontend:**
  - Chuyển giao toàn bộ thông số kỹ thuật và thiết kế Figma đã thẩm định sang đội ngũ Frontend.
  - Kích hoạt phân nhánh tính năng đầu tiên cho Frontend: dựng bộ layout nền tảng, hệ thống component nguyên tử (atomic components), và chuẩn bị sẵn sàng các interface để ghép nối với cụm API Master Data (SKU, Kho, Vị trí ô kệ).

#### Thiết Lập Hạ Tầng Lưu Trữ Đám Mây & Chuẩn Hóa Tài Liệu Dữ Liệu
- **Tích hợp dịch vụ lưu trữ media đám mây (Cloudinary):**
  - Khởi tạo tài khoản dự án trên Cloudinary phục vụ nhu cầu lưu trữ chứng từ nhận hàng, hóa đơn nông sản và ảnh nhận diện người dùng.
  - Trích xuất và cấu hình an toàn bộ 3 khóa định danh (`CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`) vào tệp biến môi trường `.env` của hệ thống.
- **Cập nhật Từ điển dữ liệu (Data Dictionary Synchronization):**
  - Đối soát cấu trúc bảng `users` thực tế vừa migrate trên cơ sở dữ liệu Neon Cloud so với tài liệu thiết kế ban đầu.
  - Bổ sung và chuẩn hóa đầy đủ các trường mới (`full_name`, `password_hash`, metadata phân quyền) vào tài liệu Data Dictionary nhằm bảo đảm tính đồng nhất 100% giữa tài liệu kiến trúc và cơ sở dữ liệu thực tế.

#### Ban Hành Quy Chuẩn Kỹ Thuật Dự Án (Engineering Guidelines)
- **Đóng đinh chuẩn giao tiếp API (API Contract):**
  - Cấu trúc dữ liệu JSON Request/Response bắt buộc chuyển hóa 100% sang định dạng `camelCase` để đồng bộ mượt mà giữa NestJS và Next.js.
  - Phân định rạch ròi cơ chế phân quyền hạt nhân theo chuẩn `MODULE.ACTION` thay vì role-based tĩnh.
  - Thiết lập mốc giới hạn đóng băng mã nguồn (Code Freeze vào 14/10/2026) nhằm đảm bảo thời gian kiểm thử tích hợp toàn diện.

#### Quản Trị Repository & Kiểm Soát An Ninh Mã Nguồn Trên GitHub
- **Bảo vệ toàn vẹn nhánh chính `PEEP1`:** Phát hiện và lập tức đóng (Close) Pull Request trái phép `#9` từ nhóm khác, loại trừ hoàn toàn nguy cơ ghi đè mã nguồn và làm sai lệch cấu trúc thư mục dự án.
- **Quy chuẩn hóa quy trình lọc PR:** Ban hành cú pháp truy vấn chuẩn trên repository dùng chung (`is:pr is:open base:PEEP1`) để đội ngũ tập trung theo dõi đúng các pull request nội bộ.
- **Thiết lập kỷ luật Git:** Ban hành quy tắc xóa nhánh tính năng (Delete branch) ngay sau khi merge thành công vào `PEEP1` nhằm giữ cây lịch sử Git luôn tinh gọn, tránh rác nhánh.

#### Review Code Chuyên Sâu Pull Request #12 (Feat/PEEP1-ID01-AUTH-RBAC)
- **Rà soát kiến trúc & Bắt lỗi bảo mật:**
  - Kiểm tra toàn diện các file: `auth.service.ts`, `protect.guard.ts`, `user.controller.ts` và hệ thống DTOs.
  - Phát hiện và chặn đứng việc hard-code quyền hạn dạng `@RequireRoles('ADMIN')`, yêu cầu tác giả chuyển dịch dứt điểm sang cơ chế kiểm tra theo Permission nguyên tử `@RequirePermission`.
  - Yêu cầu cấu hình chuẩn hóa DTO sang `camelCase` và bọc logic xử lý người dùng trong Database Transaction để bảo đảm tính nguyên tử (Atomicity).
- **Nghiệm thu & Merge:** Kiểm tra commit chỉnh sửa mới nhất, xác nhận code đã chuyển đổi sang `@RequirePermission('USER.MANAGE')` ở cấp độ Class, hỗ trợ linh hoạt DTO và bảo đảm transaction an toàn; chính thức phê duyệt merge PR `#12` vào nhánh chính `PEEP1`.

---

### 2. Tiến Độ Hiện Tại & Vấn Đề Gặp Phải (Status & Blockers)

- **Tiến độ hiện tại:** Hoàn thành 100% các mục tiêu kế hoạch: kích hoạt Frontend, thiết lập hạ tầng Cloudinary, chuẩn hóa Data Dictionary bảng `users`, bảo vệ an toàn nhánh `PEEP1` và nghiệm thu thành công module Auth/RBAC.
- **Vấn đề gặp phải & Giải pháp:**
  - *Vấn đề:* Phát hiện sự bất đồng bộ nghiêm trọng trong cách định nghĩa quyền giữa các thành viên: một nhánh code tự ý triển khai theo format `<resource>:<action>` (kiểu `user:read`), trong khi nhánh khác lại dùng `MODULE.ACTION` (kiểu `USER.READ`), dẫn đến nguy cơ gãy toàn bộ hệ thống Guard và xung đột trực tiếp với dữ liệu bảng `permissions` trên Neon Database.
  - *Giải pháp:* Quyết định khóa cứng quy chuẩn thống nhất toàn dự án là **`MODULE.ACTION`**; đồng thời lên kế hoạch trực tiếp rà soát lại toàn bộ mã nguồn của các controller để refactor và đồng bộ dứt điểm.

---

### 3. Kế Hoạch Ngày Tiếp Theo (Next Steps - 02/10/2026 - On-site)

- Trực tiếp rà soát lại toàn bộ mã nguồn Backend, khắc phục triệt để lỗi biên dịch `TS2345` trên custom decorator `@RequirePermission` (BE-36).
- Viết lại và quy chuẩn hóa đồng bộ 100% các chuỗi phân quyền trên toàn bộ Controller của Master Data về định dạng chuẩn `MODULE.ACTION` (BE-37).
- Bổ sung các module lõi còn thiếu và hoàn thiện bản thiết kế kịch bản nạp dữ liệu mẫu (Seed Data Plan) chi tiết cho trọn vẹn 20 bảng cơ sở dữ liệu trên Neon (BE-38).