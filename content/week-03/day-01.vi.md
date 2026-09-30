+++
title = "Ngày 01 - 28/09/2026 (Remote)"
weight = 1
+++

## BÁO CÁO TIẾN ĐỘ SPRINT 0: THẨM ĐỊNH SƠ ĐỒ LUỒNG VẬN HÀNH & KIỂM THỬ TRÊN GIẤY 8 KỊCH BẢN KHO

---

### 1. Thẩm Định Độ Phủ Của Sơ Đồ Luồng Nghiệp Vụ (Business Flow Diagrams)

Rà soát và hoàn thiện 5 sơ đồ luồng vận hành chính của hệ thống kho:
- **BF-01 (Inbound):** Quy trình tiếp nhận hàng tại cửa kho (`RECEIVING`), quy đổi đơn vị đóng gói về đơn vị đo lường cơ sở (Base UoM - kg), và điều hướng phân bổ hàng lên vị trí ô kệ (`BIN`).
- **BF-02 (Outbound):** Tiếp nhận đơn hàng, áp dụng thuật toán phân bổ lô cận hạn (FEFO), xử lý các nhánh ngoại lệ khi thiếu tồn, đưa hàng ra khu vực tập kết (`STAGING`), cân thực tế (*Catch weight*) và hoàn tất xuất kho.
- **BF-03 (Transfer):** Kiểm tra tồn khả dụng tại vị trí nguồn, trừ tồn nguồn, cộng tồn đích và ghi nhận cặp bút toán đối ứng nội bộ.
- **BF-04 (Cycle Count & Adjustment):** Quy trình kiểm kê định kỳ, phát hiện chênh lệch thực tế, tạo phiếu điều chỉnh và áp dụng cơ chế phê duyệt độc lập từ cấp Quản lý trước khi cập nhật sổ cái.
- **BF-05 (Opening Stock):** Quy trình kiểm tra trùng lặp mã đợt import dữ liệu tồn kho đầu kỳ.

---

### 2. Thực Nghiệm Kiểm Thử Thiết Kế Trên Giấy (Paper Testing)

Tiến hành chạy thử nghiệm dòng dữ liệu trên giấy đối với 8 kịch bản kho thực tế để xác nhận thiết kế cơ sở dữ liệu không gặp điểm nghẽn logic:
1. *Quy đổi đơn vị tính:* Nhập 10 thùng (1 thùng = 12 hộp) $\rightarrow$ Hệ thống quy đổi chuẩn xác thành +120 EA tại vị trí `RECEIVING`.
2. *Chuyển vị trí (Putaway):* Di chuyển 120 EA lên kệ $\rightarrow$ Phát sinh đúng 1 cặp dòng ledger đối ứng (-120 và +120) có chung `transaction_group_id`.
3. *Giữ chỗ đơn hàng:* Khách đặt 30 EA $\rightarrow$ Giữ chỗ tăng `qty_reserved`, giảm `qty_available`, bảng `stock_ledger` không phát sinh bản ghi.
4. *Soạn và xuất hàng:* Trừ tồn vật lý và ghi nhận đúng mã loại dịch chuyển `PICK` và `SHIP`.
5. *Hao hụt kiểm kê:* Phát hiện thiếu 2 EA $\rightarrow$ Phiếu kiểm kê ở trạng thái chờ duyệt, chỉ ghi nhận `ADJUST_OUT` sau khi Quản lý kho xác nhận.
6. *Sửa sai nghiệp vụ:* Nhập nhầm mã hàng $\rightarrow$ Hệ thống tự động tạo bút toán đảo `REVERSAL` để hoàn tồn và tạo bút toán mới, bảo toàn lịch sử.
7. *Phân quyền theo kho:* Kiểm tra cơ chế ràng buộc truy cập chéo giữa các kho khác nhau dựa trên `warehouse_id`.
8. *Kiểm soát tranh chấp (Concurrency):* Xử lý trường hợp 2 giao dịch đồng thời nhặt hàng tại ô cuối cùng an toàn thông qua cơ chế khóa dòng dữ liệu.

---

### 3. Đóng Gói Bộ Hồ Sơ Bàn Giao Sprint 0

- Tổng hợp và cấu trúc lại toàn bộ tài liệu thiết kế của nhóm trên kho lưu trữ chung theo đúng 5 danh mục:
  - `00_Plan`: Kế hoạch và quy ước nhóm.
  - `01_Business_Flow`: Hệ thống sơ đồ luồng vận hành End-to-End.
  - `02_ERD`: Bản vẽ thiết kế cơ sở dữ liệu v1.
  - `03_Data_Dictionary`: Từ điển dữ liệu chi tiết cho 20 bảng.
  - `04_Business_Rules`: Danh mục 42 quy tắc nghiệp vụ hệ thống.
- Chốt thỏa thuận làm việc nội bộ nhóm (Working Agreement) để chuẩn bị bước vào giai đoạn hiện thực hóa mã nguồn ở Sprint tiếp theo.

---

### 4. Tiến Độ & Vấn Đề (Status & Blockers)
- **Tiến độ:** Hoàn tất 100% hồ sơ thiết kế Sprint 0; 8 kịch bản kiểm thử trên giấy đều thông suốt, không phát sinh xung đột logic.
- **Vấn đề & Khắc phục:** Không có trở ngại kỹ thuật tồn đọng.

### 5. Kế Hoạch Ngày Tiếp Theo (Next Steps)
- Rà soát chéo các mối quan hệ thực thể trên sơ đồ ERD trực quan.
- Tự động hóa trích xuất mã nguồn sang Mermaid ERD để đồng bộ tài liệu bàn giao.