+++
title = "Ngày 02 - 29/09/2026 (Remote)"
weight = 2
+++

## BÁO CÁO TIẾN ĐỘ SPRINT 0: THẨM ĐỊNH KIẾN TRÚC SỔ CÁI, AUDIT SƠ ĐỒ ERD & TỰ ĐỘNG HÓA MERMAID

---

### 1. Thẩm Định, Bảo Vệ & Chuẩn Hóa Kiến Trúc Sổ Cái (Domain E - Stock Ledger)

- **Bảo toàn trường gom nhóm giao dịch (`transaction_group_id` kiểu `UUID`):**
  - Kịp thời phát hiện và giải thích để thành viên nhóm không xóa trường này (do hiểu nhầm là khóa ngoại trỏ tới bảng transactions).
  - Làm rõ bản chất nghiệp vụ: Đây là mã gom nhóm (Grouping Identifier) dùng để kết nối các cặp bút toán đối ứng kép (Double-entry: 1 dòng OUT tại nguồn và 1 dòng IN tại đích trong các tác vụ Putaway, Transfer), giúp kiểm soát tổng biến động tồn toàn kho luôn bằng 0 và không bị lẫn lộn giữa các phiên luân chuyển.
- **Tối ưu hóa kiến trúc không tạo bảng thừa:**
  - Khẳng định không cần tạo thêm bảng trung gian `transactions` (tránh phát sinh thao tác `JOIN` làm giảm hiệu năng ghi sổ cao độ), mà tận dụng linh hoạt cặp `reference_types` + `reference_id` cho chứng từ gốc kết hợp cùng `transaction_group_id` cho phiên thực thi.
- **Làm rõ cơ chế Khóa ngoại tự tham chiếu (`reversal_of_id`):**
  - Bảo vệ nguyên tắc bất biến (Append-only) của sổ cái: khi có sai sót, áp dụng cơ chế "Bút toán đảo" (Reversal Entry) để bù trừ số liệu mà không vi phạm nguyên tắc cấm `UPDATE`/`DELETE`.
  - Thiết lập vết kiểm toán 2 chiều minh bạch và ngăn chặn hoàn toàn lỗi đảo bút toán 2 lần.
- **Chuẩn hóa chi tiết 8 Khóa ngoại (FK) của `stock_ledger`:**
  - Hệ thống hóa và giải nghĩa tường tận 8 mối quan hệ liên bảng (`warehouse_id`, `location_id`, `sku_id`, `lot_id`, `movement_type_id`, `reference_type_id`, `reversal_of_id`, `created_by`) gắn liền với các câu hỏi nghiệp vụ và truy vấn kiểm toán cốt lõi.

---

### 2. Rà Soát, Bắt Lỗi (QA/Audit) & Đồng Bộ Sơ Đồ ERD Trên Draw.io

- **Bắt lỗi quan hệ và chính tả trên thực thể `skus` và `lots`:**
  - Phát hiện và yêu cầu sửa lỗi vẽ sai ký hiệu đầu dây liên kết (từ quan hệ chuẩn 1-N bị chọn nhầm thành 1-1 trên Draw.io).
  - Bảo vệ quy ước đặt tên bảng số nhiều (`skus`) theo chuẩn nhóm và phân biệt rạch ròi với tên cột khóa ngoại số ít (`sku_id`).
- **Khắc phục tình trạng thiếu sót bảng và trường dữ liệu:**
  - Phát hiện sơ đồ tổng quan bị ẩn/sót mất bảng `lots` (gây đứt gãy luồng bốc dỡ theo hạn sử dụng FEFO của nông sản) và yêu cầu đưa lại vào sơ đồ.
  - Sửa lỗi đặt thừa chữ "s" ở tên cột khóa ngoại: chuyển đổi các cột `warehouses_id`, `reference_types_id`, `movement_types_id` về đúng chuẩn `<tên_bảng_số_ít>_id` (`warehouse_id`, `reference_type_id`, `movement_type_id`).
  - Yêu cầu bổ sung đầy đủ từ 6 trường ban đầu lên trọn vẹn 18/18 trường cho thực thể `stock_ledger` (bổ sung `sku_id`, `lot_id`, `qty_before`, `qty_after`, `reference_id`, `created_at`, `created_by`...).

---

### 3. Tự Động Hóa Chuyển Đổi Sang Mermaid & Tối Ưu Hiển Thị Sơ Đồ

- **Biên dịch và đồng bộ mã nguồn Mermaid ERD:**
  - Tự động hóa trích xuất toàn bộ cấu trúc 20 bảng, kiểu dữ liệu và 32 mối quan hệ liên kết từ file PlantUML/Data Dictionary sang mã Mermaid ERD chuẩn.
  - Giúp các thành viên trong nhóm có thể import trực tiếp vào Draw.io chỉ trong 1 thao tác thay vì phải ngồi vẽ hoặc gõ tay thủ công từng bảng biểu.
- **Tối ưu hóa layout chống chồng chéo trên Draw.io:**
  - Thiết lập chuyển đổi toàn bộ các đường liên kết (Edges) từ dạng cong uốn lượn sang dạng đường vuông góc (Orthogonal) kết hợp bo góc nhẹ (Rounded) để tăng tính chuyên nghiệp.
  - Cấu hình thuật toán bố cục tự động (kết hợp Horizontal Flow và Organic / Force-directed Layout với tham số Node Spacing: 100, Repulsive Power: 150), giải quyết dứt điểm tình trạng các khối bảng bị co cụm hoặc đè chéo dây lên nhau.
- **Đảm bảo tính nhất quán của bộ tài liệu bàn giao:**
  - Kiểm tra và xác nhận độ khớp chính xác 100% giữa sơ đồ hình ảnh ERD, file mã nguồn PlantUML (`ERD PlantUML_2.puml`) và Từ điển dữ liệu (`WMS_Data_Dictionary_v1_audit_updated_2.xlsx`), đảm bảo bộ tài liệu kỹ thuật hoàn toàn chuẩn chỉ và sẵn sàng nộp bài.

  ---

### 4. Tiến Độ & Vấn Đề (Status & Blockers)
- **Tiến độ:** Hoàn thiện và đồng bộ 100% dữ liệu kiến trúc giữa ERD, PlantUML và Data Dictionary; kết thúc trọn vẹn Sprint 0.
- **Vấn đề & Khắc phục:** Việc chỉnh sửa ERD thủ công trên Draw.io dễ sinh lỗi gõ phím; đã khắc phục triệt để bằng script chuyển đổi tự động sang Mermaid.

### 5. Kế Hoạch Ngày Tiếp Theo (Next Steps)
- Khởi tạo khung dự án NestJS Backend và cấu hình kết nối PostgreSQL với Prisma ORM.
- Chuyển thể 20 bảng cơ sở dữ liệu đã audit vào file `schema.prisma`.