+++
title = "Ngày 05 - 25/09/2026 (On-site)"
weight = 5
+++

## BÁO CÁO TIẾN ĐỘ SPRINT 0: RÀ SOÁT TỪ ĐIỂN DỮ LIỆU & ĐỊNH NGHĨA 42 QUY TẮC NGHIỆP VỤ KHO

---

### 1. Rà Soát & Chuẩn Hóa Từ Điển Dữ Liệu (Data Dictionary)

- **Kiểm tra chéo cấu trúc bảng:**
  - Rà soát toàn bộ 20 bảng dữ liệu thuộc 5 domain trong tài liệu từ điển dữ liệu của nhóm.
  - Bổ sung đồng bộ bộ 4 trường Audit tiêu chuẩn (`created_at`, `created_by`, `updated_at`, `updated_by`) cho 19 bảng Master Data và bảng giao dịch.
  - Tách biệt bảng `stock_ledger` chỉ giữ đúng 2 trường audit phục vụ cơ chế Append-only.
- **Đồng bộ hóa khóa ngoại quản lý hạn dùng (FEFO):**
  - Chuyển đổi định dạng tham chiếu lô hàng sang `lot_id` (`BIGINT` FK trỏ tới bảng `lots`) thay vì lưu chuỗi text tự do, tạo tiền đề cho thuật toán tự động phân bổ hàng cận hạn xuất trước.
- **Cơ chế Snapshot số dư tức thời:**
  - Bổ sung 2 cột `qty_before` và `qty_after` vào từng dòng ledger tại từng vị trí ô kệ, phục vụ công tác đối soát số học tức thì: `qty_after = qty_before + qty_change`.

---

### 2. Thiết Lập & Chuẩn Hóa Bộ Quy Tắc Nghiệp Vụ (Business Rules)

- **Thẩm định bộ 42 quy tắc kho:** Phối hợp cùng các thành viên chốt danh sách 42 Business Rules bao quát toàn bộ 5 phân hệ trong hệ thống.
- **Chi tiết 9 quy tắc cốt lõi cho Domain E (Stock Ledger):**
  - *Cổng giao dịch duy nhất:* Mọi logic tác động đến tồn kho bắt buộc phải ủy quyền duy nhất qua `InventoryLedgerService`.
  - *Tính nguyên tử hóa (Atomicity):* Thao tác ghi dòng sổ cái và cập nhật số dư tồn kho tại bảng `inventory` bắt buộc phải nằm trong cùng một Database Transaction.
  - *Tách biệt Giữ chỗ (Reservation) và Tồn vật lý:* Nghiệp vụ giữ chỗ đơn hàng chỉ làm giảm tồn khả dụng (`qty_available`) và tăng tồn giữ chỗ (`qty_reserved`), không làm thay đổi tồn vật lý nên không được ghi vào `stock_ledger`.
  - *Bất biến đối soát:* Tổng biến động lũy kế $\sum (qty\_change)$ tại một ô kệ luôn luôn bằng với số lượng tồn vật lý `inventory.qty_on_hand`.
- **Cơ chế Idempotency Key chống trùng lặp:**
  - Thiết kế trường `entry_key` trên bảng sổ cái nhằm ngăn ngừa triệt để lỗi ghi nhận trùng giao dịch khi người dùng click thao tác nhiều lần, mạng chập chờn retry API hoặc khi import số dư đầu kỳ.

  ---

### 3. Tiến Độ & Vấn Đề (Status & Blockers)
- **Tiến độ:** Chuẩn hóa xong Từ điển dữ liệu và 42 Business Rules; chốt giải pháp Idempotency và Snapshot số dư.
- **Vấn đề & Khắc phục:** Thiếu các trường audit đồng bộ ở các bảng master; đã rà soát bổ sung đủ bộ 4 cột tiêu chuẩn.

### 4. Kế Hoạch Ngày Tiếp Theo (Next Steps)
- Thẩm định 5 sơ đồ luồng vận hành chính (BF-01 đến BF-05).
- Chạy thử nghiệm trên giấy (Paper Testing) 8 kịch bản nghiệp vụ kho phức tạp.