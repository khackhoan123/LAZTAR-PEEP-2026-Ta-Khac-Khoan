+++
title = "Ngày 01 - 05/10/2026 (Remote)"
weight = 1
+++

## BÁO CÁO TIẾN ĐỘ: THI CÔNG SỔ CÁI BẤT BIẾN, MODULE TRANSFER VÀ XỬ LÝ LỖI OCC & DATABASE CONSTRAINTS

---

### 1. Các Công Việc Đã Hoàn Thành (Completed Tasks)

#### Xây Dựng & Chuẩn Hóa Lõi Sổ Cái Tồn Kho Đối Ứng (BE-04b - Core Inventory Ledger)
- **Cơ chế ghi sổ đơn kênh duy nhất (Single Source of Truth):**
  - Xây dựng hoàn chỉnh `InventoryLedgerService` với hàm trọng tâm `recordMovement()`, đảm bảo toàn bộ biến động tăng/giảm tồn kho vật lý (`qty_on_hand`) và tồn khả dụng (`qty_available`) đều phải ủy quyền qua một cổng hạch toán duy nhất.
  - Tích hợp cơ chế Idempotent Replay qua trường định danh `entryKey`, ngăn chặn triệt để rủi ro ghi trùng giao dịch khi có sự cố retry từ client.
- **Hiện thực hóa quy tắc bút toán kép (BR-D-09 - Double-entry Movement):**
  - Triển khai các phương thức `recordDualMovement()` và `recordDualMovementInTransaction()`: bắt buộc mỗi biến động luân chuyển nội bộ phải sinh đủ 1 cặp bút toán đối ứng (`OUT` tại nguồn và `IN` tại đích) có cùng mã `transaction_group_id` trong cùng một transaction nguyên tử, bảo toàn tổng lượng biến động ròng toàn kho luôn bằng 0.

#### Phát Triển Phân Hệ Điều Chuyển Kho Nội Bộ (BE-19, BE-20 - Internal Transfer)
- **Kiểm soát ràng buộc nghiệp vụ điều chuyển:**
  - Hiện thực hóa `createTransferRequest()` (`BE-19`): Tự động quy đổi đơn vị tính (UoM conversion) về Base UoM, kiểm tra và khóa tồn khả dụng `qty_available` tại vị trí nguồn trước khi khởi tạo yêu cầu.
  - Hiện thực hóa `confirmTransfer()` (`BE-20`): Áp dụng cơ chế kiểm tra sớm (Fail-Fast) bắt buộc vị trí đích phải thuộc phân loại `BIN`, thực thi giao dịch đối ứng xuất kho nguồn và nhập kho đích, đồng thời hạch toán chính xác cặp thẻ kho `TRANSFER_OUT` / `TRANSFER_IN`.
  - Soạn thảo và áp dụng migration phân quyền: `20261005140000_transfer_permissions`.

#### Gia Cố Bảo Mật Hệ Thống Xác Thực & Phân Quyền Động (Auth, RBAC & Audit)
- **Bảo vệ Refresh Token:** Khắc phục lỗ hổng lưu trữ token thuần; tạo migration `20261005150000_user_refresh_token_hash`, mã hóa token bằng `bcrypt`, tích hợp cơ chế xoay vòng token (Token Rotation) và thu hồi tức thì phiên đăng nhập qua `token_version`.
- **Cơ chế chống brute-force:** Kích hoạt tính năng tự động khóa tài khoản khi đăng nhập thất bại 5 lần liên tiếp (`failed_login_count`, `locked_until`).
- **Tự động hóa bối cảnh kiểm toán:** Truyền tự động ngữ cảnh người thực thi (`app.actor_user_id`) qua helper `withActor()` để kích hoạt các database trigger kiểm toán nội bộ.

#### Xử Lý Triệt Để Hàng Loạt Lỗi Chí Mạng (Critical Showstopper Bugs)
- **Khắc phục lỗi xung đột đồng thời OCC & Prisma P2025:** Thiết lập hàm `updateInventoryOptimistically()`, bắt mã lỗi `P2025 (Record not found)` khi kiểm tra version và chuyển hóa thành `ConflictException (409)` có ngữ nghĩa để worker/client retry an toàn, loại trừ lỗi Lost Update.
- **Xử lý đứt gãy truy vấn khi SKU không có Lô (`lotId = null`):** Tái cấu trúc hàm query nội bộ, phân nhánh linh hoạt giữa `findFirst` (khi `lotId === null`) và `findUnique` (khi có `lotId`) để giải quyết triệt để lỗi xung đột compound unique selector trên Prisma Client.
- **Xóa bỏ lỗi 403 tập thể do lệch chuẩn mã quyền:** Đồng bộ toàn bộ chuỗi quyền phân tán sang chuẩn `MODULE.ACTION`, chuẩn hóa regex kiểm tra trong `protect.guard.ts` và gán decorator `@RequirePermission()` trên các controller.

---

### 2. Tiến Độ Hiện Tại & Vấn Đề Gặp Phải (Status & Blockers)

- **Tiến độ hiện tại:** Hoàn thành 100% nền móng sổ cái `InventoryLedgerService`, phân hệ Transfer và hệ thống Auth/RBAC bảo mật; gỡ bỏ toàn bộ các lỗi đứt gãy schema cơ sở dữ liệu.
- **Vấn đề gặp phải & Giải pháp:** Quá trình kiểm tra đồng thời nhiều giao dịch nhập/xuất kho dễ phát sinh lỗi P2025 do cơ chế Optimistic Concurrency Control; đã giải quyết bằng interceptor bắt lỗi P2025 chuyển thành mã HTTP 409 chuẩn để điều phối luồng retry an toàn.

---

### 3. Kế Hoạch Ngày Tiếp Theo (Next Steps - 06/10/2026)

- Triển khai phân hệ Xuất kho toàn diện (Outbound Orders, FEFO Allocation, Picking, Catch-weight và Dispatch).
- Viết và mở rộng toàn diện bộ Unit Test tự động cho các service nghiệp vụ trọng yếu (Inventory, Ledger, Transfer, Auth, Outbound).
- Xây dựng component UI Feedback (Toast) và nâng cấp tầng API Client kết nối dữ liệu phía Frontend.