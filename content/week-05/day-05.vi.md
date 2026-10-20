+++
title = "Ngày 05 - 16/10/2026 (On-site)"
weight = 5
+++

## BÁO CÁO TIẾN ĐỘ: TỰ ĐỘNG HÓA MASTER DATA NÔNG SẢN, CHUẨN HÓA MÃ VẠCH EAN-13 & HỢP NHẤT PEEP1-UXUI

---

### 1. Các Công Việc Đã Hoàn Thành (Completed Tasks)

#### Xây Dựng Kịch Bản Tự Động Hóa Master Data Nông Sản Chuyên Sâu (`seed-catalog.ts`)
- **Hiện thực hóa catalog dữ liệu thực chiến (Commit `b908da3` - gần 700 dòng code):** Thay thế toàn bộ dữ liệu kiểm thử vô nghĩa (`TEST_SKU`) bằng bộ danh mục chuẩn hóa cho hệ thống quản lý nông sản tươi sống:
  - *7 Danh mục chuyên biệt:* Rau củ quả tươi (`VEG`), Trái cây đặc sản (`FRUIT`), Củ & Nấm tươi (`TUBER`), Rau ăn lá thủy canh (`LEAF`), Gia vị & Rau thơm (`HERB`), Nông sản khô & Ngũ cốc (`GRAIN`), Bao bì & Phụ liệu kho (`PACK`).
  - *8 Đơn vị tính (UOM) chuẩn hóa tỷ lệ quy đổi:* KG (scale 4 chữ số thập phân), Thùng carton (`BOX`), Túi (`BAG`), Bó (`BUNCH`), Khay (`TRAY`), Gói (`PACK`), Sọt nhựa (`BASKET`), Tấn (`TON`).
  - *5 Nhà cung cấp nông nghiệp uy tín:* Nông Trại Đà Lạt Fresh, HTX Nông Sản Hữu Cơ Mộc Châu, Cty Long An, HTX Thủy Canh Củ Chi, Nông Sản Xanh Miền Tây.
  - *15 SKUs nông sản thương phẩm đặc thù:*
    - Hàng theo dõi lô & hạn sử dụng: Cà chua Beef Đà Lạt (HSD 14 ngày, 1 Thùng = 10 KG), Xoài Cát Hòa Lộc, Cà rốt hữu cơ, Cam Sành, Tỏi cô đơn Lý Sơn (HSD 90 ngày), Gạo ST25 Ông Cua (HSD 180 ngày).
    - Hàng cân thực tế (**Catch-weight**): Bắp cải trắng Đà Lạt (1 Sọt = 25 KG thực tế), Dưa hấu không hạt Mặt Trời Đỏ (quản lý theo quả, trừ tồn theo cân ký thực).
    - Vật tư phụ liệu kho: Thùng carton lạnh 40x30x25cm (không quản lý lô/hạn).
- **Cấp phát mã vạch tiêu chuẩn quốc tế EAN-13:** Gán mã vạch chuẩn `893...` cho từng đơn vị quy đổi của từng SKU, đảm bảo máy quét barcode quang học và camera điện thoại nhận diện chính xác 100%.

#### Hợp Nhất Logic Nghiệp Vụ Chặn Ngày Lấy Hàng (`PEEP1` $\rightarrow$ `peep1-uxui`)
- **Tích hợp ràng buộc điều phối (Commit `249ec68`):** Merge toàn bộ các cập nhật logic từ nhánh `PEEP1` vào nhánh giao diện `peep1-uxui`, giải quyết xung đột tại các hook quản lý trạng thái form và kiểm tra logic đặt giữ hàng (Reservation).
- **Khóa chọn ngày trong quá khứ:** Bổ sung cơ chế validation chặn nhân viên kho chọn ngày nhặt hàng/xuất hàng nhỏ hơn ngày hiện tại, đồng thời kiểm soát trạng thái nút bấm thao tác xuất hàng tương ứng với quyền hạn người dùng.
- **Kiểm định tính toàn vẹn bộ test:** Thực thi bộ kiểm thử Frontend ngay sau merge, đảm bảo toàn bộ các ca kiểm tra giao diện đều giữ vững trạng thái Pass.

#### Gia Cố Kiến Trúc Toàn Vẹn Dữ Liệu & Cấp Phát Mã Chứng Từ Sạch
- **Cấp phát mã chứng từ chuẩn ISO qua Postgres Advisory Lock:** Loại bỏ cơ chế sequence ngẫu nhiên dễ sinh mã rác khi giao dịch thất bại; ứng dụng `pg_advisory_xact_lock` theo tiền tố nghiệp vụ và ngày giao dịch (`YYYYMMDD`), đảm bảo mã chứng từ chỉ tăng khi transaction thực sự commit thành công.
- **Chốt chặn chống xuất âm kho 3 tầng & Deferred Constraint Trigger:**
  - *Tầng 1 (Business Logic):* Kiểm soát chặt công thức `Available Stock = Physical Stock - Reserved Stock`.
  - *Tầng 2 (Concurrency Control):* Áp dụng Optimistic Concurrency Control (version) kết hợp khóa dòng `SELECT ... FOR UPDATE`.
  - *Tầng 3 (Database Constraint):* Cài đặt ràng buộc vật lý `CHECK (qty_on_hand >= 0)` cùng `Deferred Constraint Trigger` tự động đối chiếu số dư sổ cái và tồn kho cuối mỗi giao dịch.

---

### 2. Tiến Độ Hiện Tại & Vấn Đề Gặp Phải (Status & Blockers)

- **Tiến độ hiện tại:** Hoàn thành 100% bộ dữ liệu Master Data nông sản chuẩn chỉ; hợp nhất thành công toàn bộ logic nghiệp vụ vào giao diện; toàn bộ các luồng sẵn sàng cho việc nạp giao dịch thực tế.
- **Vấn đề gặp phải & Giải pháp:** Các mặt hàng cân thực tế (Catch-weight) có sự chênh lệch giữa đơn vị lưu kho và đơn vị cân xuất; đã xử lý triệt để thông qua bảng quy đổi UoM với trường hệ số tỷ lệ và công thức tính dung sai trọng lượng (`variancePercentage`).

---

### 3. Kế Hoạch Ngày Tiếp Theo (Next Steps - 19/10/2026 - On-site)

- Tối ưu hóa thời gian chờ giao dịch Prisma Transaction Timeout trên cơ sở dữ liệu Neon Cloud Serverless.
- Xây dựng và thực thi kịch bản tự động hóa 5 luồng giao dịch khép kín (`seed-demo-transactions.ts`) qua API thực tế.
- Thực hiện kiểm toán toàn diện hệ sinh thái kiểm thử tự động, xác minh mốc kỷ lục hơn 1.200+ tests vượt qua 100% trước thềm nghiệm thu.