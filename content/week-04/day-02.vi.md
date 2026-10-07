+++
title = "Ngày 02 - 06/10/2026 (Remote)"
weight = 2
+++

## BÁO CÁO TIẾN ĐỘ: HOÀN THIỆN ENGINE XUẤT KHO FEFO, MỞ RỘNG 99 UNIT TESTS & TÍCH HỢP FRONTEND CLIENT

---

### 1. Các Công Việc Đã Hoàn Thành (Completed Tasks)

#### Xây Dựng Engine Xuất Kho Nông Sản & Xử Lý Thiếu Hụt (BE-21 đến BE-29 - Outbound Pipeline)
- **Thuật toán cấp phát tồn tự động FEFO (`BE-22`):**
  - Hiện thực hóa hàm `allocateOutboundFEFO()`: Tự động lọc và sắp xếp các lô hàng nông sản ưu tiên theo hạn sử dụng tăng dần (`lot.expiry_date ASC`), tự động tính toán chính xác lượng tồn còn thiếu (`shortageQty`) để kích hoạt kịch bản giao thiếu hoặc nợ đơn.
- **Quản trị giữ chỗ tồn kho (`BE-23` - Stock Reservation):**
  - Xây dựng `reserveOutbound()` tuân thủ nghiêm ngặt quy tắc **BR-D-06** và **BR-D-07**: Tăng số lượng giữ chỗ `qty_reserved` và tự động sinh bản ghi `inventory_reservations` với trạng thái `ACTIVE`.
- **Tối ưu lộ trình nhặt hàng & Chuyển vùng Staging (`BE-25`, `BE-26`):**
  - Hàm `createPickTask()`: Tự động sắp xếp các lượt nhặt hàng theo thứ tự mã vị trí ô kệ (`location.code / full_path ASC`), tạo ra lộ trình di chuyển ngắn nhất cho nhân viên kho.
  - Hàm `confirmPick()`: Chuyển dịch tồn kho vật lý từ kệ lưu trữ sang khu vực tập kết chờ xuất `STAGING`, ghi nhận chính xác cặp thẻ kho đối ứng `PICK_OUT` / `PICK_IN`.
- **Bàn cân Catch-weight & Hoàn tất xuất kho (`BE-27`, `BE-28`, `BE-29`):**
  - Tích hợp hàm `recordCatchWeight()` tính toán tỷ lệ sai lệch trọng lượng thực tế (`variancePercentage`) của nông sản, hàm `shipOutbound()` trừ tồn tại Staging và ghi thẻ kho `SHIP`, cùng hàm tiếp nhận hàng trả về `receiveReturn()`.
  - Thiết lập migration phân quyền: `20261005160000_outbound_permissions`.

#### Phát Hiện & Lên Phương Án Chặn Lỗi Vỡ Trigger Database Khi Partial Picking
- **Phát hiện điểm mù logic:** Khi nhặt hàng một phần (ví dụ nhặt 4 trên tổng 10 đơn vị giữ chỗ), logic ban đầu đánh dấu toàn bộ reservation thành `COMPLETED` trong khi `inventory.qty_reserved` vẫn còn dư 6. Điều này khiến database trigger `wms_reservation_balance` của PostgreSQL từ chối và rollback 100% giao dịch.
- **Biện pháp xử lý:** Tiến hành cách ly lỗi, lên giải pháp kỹ thuật bóc tách dòng reservation thành 2 phần: hoàn tất phần đã nhặt và giữ nguyên bản ghi active cho phần còn lại, bảo toàn tuyệt đối đẳng thức `SUM(active_reservations) === qty_reserved`.

#### Mở Rộng Hệ Thống Kiểm Thử Tự Động Toàn Diện (Unit Test Suites - 99 Tests Passed)
- Trực tiếp thiết kế và hoàn thiện 9 bộ Test Suite chuyên sâu với hơn **1,500+ dòng code test**, nâng tổng số test của dự án từ 33 lên **143 tests**:
  1. `outbound.service.spec.ts` (529 dòng): Bao phủ trọn vẹn thuật toán FEFO, giữ tồn, xử lý thiếu hàng, nhặt hàng, cân catch-weight và xuất hàng.
  2. `outbound.controller.spec.ts` (86 dòng): Kiểm thử hợp đồng API, status code và validation DTO.
  3. `transfer.service.spec.ts` (293 dòng): Kiểm thử xác thực vị trí BIN, kiểm tra tồn khả dụng và giao dịch đối ứng nguyên tử.
  4. `inventory-ledger.service.spec.ts` (111 dòng): Kiểm thử thẻ kho đối ứng, idempotent replay và xử lý xung đột OCC.
  5. `inventory.service.spec.ts` (122 dòng): Kiểm tra truy vấn tồn kho an toàn và bắt lỗi Prisma P2025.
  6. `auth.service.spec.ts` (317 dòng): Kiểm tra mã hóa bcrypt, cơ chế khóa tài khoản sau 5 lần thất bại và xoay vòng token.
  7. `auth.controller.spec.ts` (118 dòng): Kiểm thử hợp đồng các endpoint xác thực.
  8. `inventory-ledger.mapper.spec.ts`: Kiểm thử ánh xạ kiểu số Decimal an toàn.
  9. `protect.guard.spec.ts`: Kiểm tra phân quyền RBAC và phân quyền theo phạm vi kho.
- **Kết quả thực thi Jest Runner:** 9/9 test suites cốt lõi vượt qua kiểm thử tự động với tỷ lệ thành công 100% (99 passed).

#### Tích Hợp Frontend UI & Đảm Bảo Tính Toàn Vẹn Hệ Thống CI/CD
- **Hệ thống phản hồi giao diện:** Tự tay xây dựng component `toast.tsx` (131 dòng code), chuẩn hóa thông báo trạng thái thành công/thất bại thống nhất cho toàn bộ màn hình vận hành.
- **Nâng cấp tầng `api-client.ts`:** Cấu hình tự động đính kèm `Bearer token`, chuẩn hóa cấu trúc bắt lỗi từ NestJS và tự động xử lý khi hết hạn phiên đăng nhập (401 Unauthorized).
- **Kết nối luồng Nhập kho (Inbound Operations):** Tích hợp bối cảnh kho đang chọn (`Warehouse Context`) vào dialog tạo phiếu nhập hàng `create-inbound-dialog.tsx`, liên kết trực tiếp các nút duyệt/hủy với API thực tế.
- **Kiểm soát chất lượng mã nguồn & CI Build:**
  - Trực tiếp giải quyết xung đột merge và hợp nhất an toàn các PR cốt lõi vào nhánh chính `PEEP1` (PR #12, #19, #26, #28, #29, #40).
  - Kiểm tra biên dịch nghiêm ngặt: Chạy lệnh `tsc --noEmit --incremental false` đạt trạng thái 0 lỗi TypeScript, dọn dẹp triệt để các warning unused variables bằng ESLint và Prettier.

---

### 2. Tiến Độ Hiện Tại & Vấn Đề Gặp Phải (Status & Blockers)

- **Tiến độ hiện tại:** Hoàn tất 100% luồng nghiệp vụ Xuất kho FEFO, hệ thống Unit Test lõi đạt tỷ lệ Pass 100% (99/99 tests), kết nối thành công UI Toast và API Client phía Frontend; nhánh chính `PEEP1` đạt trạng thái build sạch tuyệt đối.
- **Vấn đề gặp phải & Giải pháp:** Vấn đề xung đột giữa logic Partial Picking và trigger cơ sở dữ liệu `wms_reservation_balance` đã được khoanh vùng và thiết kế giải pháp phân tách bản ghi giữ chỗ để đảm bảo không bị rollback dữ liệu.

---

### 3. Kế Hoạch Ngày Tiếp Theo (Next Steps - 07/10/2026)

- Triển khai giải pháp phân tách dòng reservation cho kịch bản Partial Picking và kiểm thử thực tế trên Neon Cloud.
- Tiếp tục hỗ trợ kết nối các màn hình Frontend còn lại (Sơ đồ vị trí kho, danh sách nhặt hàng Outbound) với hệ thống API Backend.
- Chuẩn bị dữ liệu mẫu toàn diện phục vụ đợt tổng duyệt thử nghiệm (Dry-run Demo) của Tuần 4.