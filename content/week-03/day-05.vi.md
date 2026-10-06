+++
title = "Ngày 05 - 02/10/2026 (On-site)"
weight = 5
+++

## BÁO CÁO TIẾN ĐỘ: TỐI ƯU HÓA DECORATOR PHÂN QUYỀN, TINH CHỈNH KIẾN TRÚC WMS & THIẾT KẾ KỊCH BẢN SEED DATA 20 BẢNG

---

### 1. Các Công Việc Đã Hoàn Thành (Completed Tasks)

#### Khắc Phục Lỗi Biên Dịch & Chuẩn Hóa Decorator Phân Quyền (BE-36 & BE-37)
- **Xử lý triệt để lỗi biên dịch TypeScript `TS2345` (BE-36):**
  - Tái cấu trúc và nới lỏng kiểu dữ liệu Generic trong custom decorator `@RequirePermission`, sửa dứt điểm lỗi ép kiểu không tương thích giữa metadata phân quyền và execution context của NestJS.
  - Triển khai gắn decorator phân quyền chuẩn hóa trên toàn bộ các Controller thuộc phân hệ Master Data: `categories.controller.ts`, `skus.controller.ts`, `suppliers.controller.ts`, `warehouses.controller.ts`.
- **Chuẩn hóa tập trung mã quyền `MODULE.ACTION` (BE-37):**
  - Chuyển đổi toàn bộ chuỗi quyền phân tán sang bộ hằng số tập trung (Permissions Constants), chuẩn hóa định dạng in hoa phân tách bằng dấu chấm (ví dụ: `CATEGORY.READ`, `SKU.CREATE`, `SUPPLIER.UPDATE`, `WAREHOUSE.READ`, `WAREHOUSE.CREATE`).
  - Đảm bảo định dạng mã quyền khớp 100% với danh mục quyền đã định nghĩa trong bảng `permissions` và cấu trúc bảng `roles` của cơ sở dữ liệu Neon.

#### Rà Soát Kiến Trúc Hệ Thống & Bổ Sung Module Kỹ Thuật Cốt Lõi
- **Làm rõ bản chất kiến trúc hạch toán kho (Eliminating Virtual Tasks):**
  - Trực tiếp giải đáp phản biện kỹ thuật cho đội ngũ Backend: Cơ sở dữ liệu 20 bảng không có bảng `tasks` hay cột `task_id`; các khái niệm "pick task", "putaway task" thuần túy là thuật ngữ nghiệp vụ, mọi biến động kho bắt buộc phải hạch toán trực tiếp qua `stock_ledger` kết hợp chứng từ tham chiếu.
- **Rà soát & Bổ sung 8 Module Kỹ thuật Cốt lõi (Tránh trượt kiến trúc):**
  - *Backend:* Bổ sung dịch vụ lõi thẻ kho `InventoryLedgerService` (BE-04b), API Quản trị User & Gán vai trò theo kho (BE-07b), dịch vụ quy đổi đơn vị `UomConversionService` (BE-09b), API tra cứu nhanh Barcode & Bin (BE-10b), và bóc tách riêng API quản lý liên kết SKU - Nhà cung cấp `sku_suppliers` (BE-12b) tách biệt với hồ sơ nhà cung cấp chung (BE-12).
  - *Frontend:* Bổ sung giao diện Quản trị User/Role (FE-03b), Quản lý Danh mục & Đơn vị tính (FE-05b), và Giao diện liên kết SKU - Nhà cung cấp (FE-07b).
- **Thống nhất chiến lược Mobile-First:**
  - Khẳng định và bảo vệ kiến trúc tách biệt các luồng thao tác hiện trường trên thiết bị di động (Mobile Track) cho công nhân kho thay vì gộp chung vào Web desktop, đảm bảo tối ưu hóa trải nghiệm quét mã và thao tác một tay tại kho bãi.
- **Thiết lập chuẩn bảo mật User:** Đóng hoàn toàn endpoint đăng ký tài khoản tự do (`/register`), chuyển 100% sang luồng cấp phát nội bộ do Admin thực hiện với quyền `USER.CREATE`.

#### Thiết Kế Kịch Bản Nạp Dữ Liệu Mẫu Chi Tiết Cho 20 Bảng (BE-38 - In Progress)
- **Tương thích cơ chế kiểm toán tự động (Audit Trail Compatibility):** Thiết lập tài khoản định danh hệ thống (`SYSTEM` với `ID: 1000`) nhằm cung cấp biến ngữ cảnh phiên `app.actor_user_id`, đáp ứng trọn vẹn các database trigger kiểm toán tự động trên PostgreSQL.
- **Xây dựng cấu trúc cây vị trí kho 4 tầng chuẩn mực:** Lập dữ liệu mẫu phân tầng không gian kho vật lý hoàn chỉnh: Vùng kho (`ZONE`) $\rightarrow$ Dãy kệ (`AISLE`) $\rightarrow$ Tầng kệ (`RACK`) $\rightarrow$ Ô vị trí chi tiết (`BIN`).
- **Bảo đảm cân bằng số học tuyệt đối:** Thiết kế các dòng dữ liệu mẫu đảm bảo tính toán khớp 100% giữa các bảng:
  - $\sum (stock\_ledger.qty\_change) = inventory.qty\_on_hand$
  - $\sum (inventory\_reservations.qty\_reserved) = inventory.qty\_reserved$
- **Cài cắm kịch bản kiểm thử FEFO:** Thiết lập 2 lô nông sản khác nhau (`lot_id`) của cùng một mã SKU với hạn sử dụng chênh lệch để chuẩn bị sẵn dữ liệu thử nghiệm thuật toán xuất hàng ưu tiên hạn cận date (FEFO).

---

### 2. Tiến Độ Hiện Tại & Vấn Đề Gặp Phải (Status & Blockers)

- **Tiến độ hiện tại:** Hoàn thành xuất sắc 2 task BE-36 và BE-37; bóc tách và chuẩn hóa toàn bộ các module kỹ thuật cốt lõi; bản thiết kế kịch bản Seed Data 20 bảng (BE-38) đã hoàn thiện 90% về mặt logic nghiệp vụ và toán học; sẵn sàng chuyển thể thành code.
- **Vấn đề gặp phải & Giải pháp:** Các database trigger trên Neon Cloud từ chối các câu lệnh chèn dữ liệu nếu thiếu biến phiên `app.actor_user_id`; đã xử lý bằng giải pháp khởi tạo actor ngầm `SYSTEM` (ID: 1000) trong transaction trước khi chạy seed.

---

### 3. Kế Hoạch Ngày Tiếp Theo (Next Steps)

- Chuyển thể toàn bộ kịch bản dữ liệu mẫu 20 bảng vào tệp thực thi `prisma/seed.ts` và chạy nạp chính thức lên cơ sở dữ liệu Neon Cloud.
- Tiến hành rà soát và review Pull Request cho cụm tính năng Nhận hàng & Cất hàng (Inbound / Putaway - từ BE-12 đến BE-18), đặc biệt kiểm tra tính nguyên tử của giao dịch kho và cặp thẻ kho đối ứng.