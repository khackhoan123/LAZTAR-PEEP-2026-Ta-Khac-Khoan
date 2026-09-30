+++
title = "Ngày 04 - 24/09/2026 (Remote)"
weight = 4
+++

## BÁO CÁO TIẾN ĐỘ SPRINT 0: THỐNG NHẤT QUY ƯỚC NHÓM & THIẾT KẾ KIẾN TRÚC SỔ CÁI BẤT BIẾN (DOMAIN E)

---

### 1. Thống Nhất Quy Ước Kỹ Thuật Chung & Phân Bổ Nhóm

- **Quy chuẩn đặt tên (Naming Conventions):**
  - Thống nhất toàn bộ tên bảng theo chuẩn `snake_case` số nhiều (`users`, `warehouses`, `locations`... ngoại trừ bảng đặc thù `stock_ledger`).
  - Chuẩn hóa cấu trúc khóa: Khóa chính đặt là `id`, khóa ngoại tuân thủ nghiêm ngặt định dạng `<tên_bảng_số_ít>_id`.
- **Chuẩn hóa kiểu dữ liệu hệ thống:**
  - Khóa chính sử dụng `BIGINT` tự tăng để đảm bảo khả năng mở rộng lâu dài.
  - Số lượng hàng hóa tồn kho thống nhất dùng kiểu `DECIMAL(18,4)` để xử lý triệt để số lượng lẻ và đặc thù cân thực tế (Catch weight) của nông sản.
  - Trường thời gian chuẩn hóa dùng `TIMESTAMPTZ` (UTC) đồng bộ trên mọi bảng.
- **Quy định bảo toàn dữ liệu (Soft Delete):** Nghiêm cấm xóa cứng (`DELETE`) đối với các bảng Master Data đã phát sinh dữ liệu giao dịch; quản lý vòng đời bản ghi thông qua cờ `is_active` hoặc cột `status`.
- **Phân bổ phụ thuộc giữa các Domain:** Chia 5 domain cho các thành viên trong nhóm, xác định rõ lộ trình ưu tiên: Domain B (Master Data) và Domain C (Locations/Partners) phải hoàn thiện cấu trúc trước để Domain D (Inventory) và Domain E (Stock Ledger) liên kết khóa ngoại.

---

### 2. Thiết Kế Kiến Trúc Lõi Sổ Cái Bất Biến (Domain E - Stock Ledger)

- **Nguyên tắc Append-Only (Chỉ thêm mới):**
  - Thiết kế thực thể trung tâm `stock_ledger` lưu trữ toàn bộ biến động dịch chuyển vật lý của hàng hóa trong kho.
  - Bảng này chỉ cho phép thao tác `INSERT`, cấm tuyệt đối `UPDATE` và `DELETE` ở cả tầng ứng dụng (Backend logic) và ràng buộc cơ sở dữ liệu.
- **Cơ chế Bút toán đảo (Reversal Entry):**
  - Khi phát sinh lỗi nghiệp vụ (nhập sai SKU, sai số lượng), hệ thống không sửa dòng dữ liệu cũ mà ghi nhận một dòng đối ứng ngược dấu thông qua khóa tự tham chiếu `reversal_of_id`.
- **Thiết kế Audit Trail đặc thù:**
  - Khác với các bảng Master Data thông thường, bảng `stock_ledger` chỉ sở hữu duy nhất 2 trường theo dõi: `created_at` và `created_by` (FK trỏ về `users.id`), loại bỏ hoàn toàn `updated_at` và `updated_by` để bảo toàn tính bất biến của lịch sử kho.
- **Quản lý biến động kép (Double-entry Movement):**
  - Bổ sung trường `transaction_group_id` (kiểu `UUID`): Khi thực hiện luồng di chuyển nội bộ (Putaway, Chuyển ô vị trí, Picking), hệ thống sinh một cặp bút toán (1 dòng giảm tại vị trí nguồn, 1 dòng tăng tại vị trí đích) có cùng một nhóm ID, đảm bảo tổng chênh lệch tồn toàn kho luôn bằng 0.
- **Bảng danh mục chuẩn hóa:**
  - Thiết lập bảng `movement_types` (`RECEIPT`, `PUTAWAY_OUT`, `PUTAWAY_IN`, `PICK`, `SHIP`, `TRANSFER_OUT`, `TRANSFER_IN`, `ADJUST_IN`, `ADJUST_OUT`, `REVERSAL`) và `reference_types` bằng liên kết khóa ngoại để kiểm soát toàn vẹn dữ liệu.

  ---

### 3. Tiến Độ & Vấn Đề (Status & Blockers)
- **Tiến độ:** Hoàn thành thống nhất quy ước kỹ thuật chung và thiết kế kiến trúc lõi Domain E đúng hạn.
- **Vấn đề & Khắc phục:** Xuất hiện nguy cơ lệch chuẩn kiểu dữ liệu giữa các domain; đã giải quyết bằng bảng quy ước chung (BIGINT, DECIMAL(18,4), TIMESTAMPTZ).

### 4. Kế Hoạch Ngày Tiếp Theo (Next Steps)
- Rà soát chéo Từ điển dữ liệu (Data Dictionary) của 20 bảng biểu.
- Thẩm định và hoàn thiện bộ 42 Business Rules toàn hệ thống.