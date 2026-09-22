+++
title = "Ngày 02 - 22/09/2026 (Remote)"
weight = 2
+++

## BÁO CÁO KICKOFF & HOẠCH ĐỊNH KIẾN TRÚC HỆ THỐNG DỰ ÁN MINI-WMS (FRESHLINK PRODUCE)

---

### 1. Nắm Rõ Mục Tiêu & Chuẩn Hóa Tech Stack Dự Án

#### Bản chất và Bối cảnh Đề tài
- **Mục tiêu dự án:** Tham gia phát triển hệ thống quản lý kho nông sản thực tế (**FreshLink Produce**) kéo dài trong 5 tuần theo mô hình thi đua giữa 2 team nội bộ để đánh giá, xếp hạng năng lực chuyên môn (Backend, Frontend, BA, QA) và xét duyệt nhân sự cho các dự án thương mại chính thức.
- **Khóa cứng Bộ Công nghệ Bắt buộc (Production Tech Stack):**
  - **Backend:** NestJS (TypeScript) với kiến trúc module hóa hướng dịch vụ.
  - **Cơ sở dữ liệu & ORM:** PostgreSQL 16 kết hợp cùng Prisma ORM để quản lý migration và schema type-safe.
  - **Frontend:** Next.js (React + TypeScript) với tư duy **Mobile-first** phục vụ trực tiếp thao tác thực địa của công nhân kho.
  - **Hạ tầng & Tự động hóa:** Docker Compose phục vụ môi trường local và GitHub Actions cho quy trình CI kiểm tra mã nguồn tự động.

---

### 2. Phân Tích Nghiệp Vụ Kho Đặc Thù & Kiến Trúc Hệ Thống Cốt Lõi

#### Thiết Lập Hai Nguyên Tắc Kỹ Thuật Bất Biến
- **Cơ chế Sổ Cái (Ledger-based / Append-only):** Thiết lập nguyên tắc kế toán kho nghiêm ngặt—tuyệt đối không chạy lệnh `UPDATE` hoặc `DELETE` trực tiếp lên số lượng tồn kho. Mọi biến động tăng/giảm vật lý đều phải ghi nhận thành một bản ghi mới trong bảng `stock_ledger` (tương tự lịch sử giao dịch ngân hàng). Khi phát hiện sai lệch số liệu, bắt buộc phải tạo bút toán đảo để bù trừ nhằm đảm bảo khả năng truy vết kiểm toán (Audit Trail) minh bạch 100%.
- **Kiến trúc "Một Cửa Duy Nhất" (Single Gatekeeper):** Toàn bộ các luồng nghiệp vụ từ chiều Nhập (Inbound) cho đến chiều Xuất (Outbound) đều không được phép tự ý sửa tồn, mà bắt buộc phải ủy quyền qua một service trung tâm duy nhất là `InventoryLedgerService`.

#### Làm Rõ 8 Quy Tắc Nghiệp Vụ Kho Nông Sản
1. **Tách biệt Trạng thái Tồn kho:** Phân biệt rõ ràng giữa Tồn vật lý thực tế (*On-hand*) và Tồn khả dụng có thể bán/xuất (*Available = On-hand - Committed*).
2. **Quy đổi Đơn vị Tính Chuẩn (Base UoM):** Tự động quy đổi các đơn vị bao bì nhập/xuất (thùng, sọt, túi) về đơn vị đo lường cơ sở thống nhất là `kg` trong sổ cái.
3. **Thuật toán Xuất Hàng FEFO (*First Expired, First Out*):** Do đặc thù nông sản dễ hỏng hóc theo thời gian, hệ thống ưu tiên bốc dỡ các lô hàng có hạn sử dụng gần nhất thay vì dùng chuẩn FIFO thông thường.
4. **Bàn Cân Thực Tế (*Catch Weight*):** Hỗ trợ việc cân trọng lượng thực tế tại trạm đóng gói để trừ kho chính xác và tính tiền dựa trên số cân thực thay vì trọng lượng lý thuyết danh nghĩa.
5. **Xử lý Ba Nhánh Thiếu Tồn Khi Pick Hàng:** Khi kho không đủ hàng đáp ứng đơn, hệ thống hỗ trợ 3 kịch bản: Giao thiếu theo số lượng thực tế, đổi sang mã hàng tương đương, hoặc tự động tạo đơn nợ giao bù (Backorder).
6. **Kiểm Soát Thất Thoát Bằng Quy Trình Phê Duyệt:** Mọi phiếu kiểm kê, điều chỉnh tồn kho (Stock Adjustment) bắt buộc phải qua trạng thái chờ Quản lý kho (Manager) duyệt mới được phép ghi sổ cái.
7. **Khóa Idempotency Nhập Đầu Kỳ:** Áp dụng Idempotency Key để ngăn chặn việc nhân bản hoặc import trùng lặp dữ liệu tồn kho đầu kỳ từ các file bảng tính.
8. **Kiểm Soát Tranh Chấp Đồng Thời (Concurrency Lock):** Áp dụng kỹ thuật Pessimistic Locking (`SELECT ... FOR UPDATE`) trên PostgreSQL để triệt tiêu lỗi Race Condition khi nhiều nhân viên cùng xuất kho một mặt hàng tại cùng một thời điểm.

#### Định Hướng Mô Hình Phân Quyền Bảo Mật (Lightweight RBAC)
- Thống nhất áp dụng mô hình phân quyền dựa trên quyền hạn chi tiết (**Permission-based Access Control**) thay vì gắn cứng theo tên Role.
- Sử dụng Custom Decorator và Guard trong NestJS để kiểm tra quyền hạn thao tác cụ thể, tuân thủ chặt chẽ nguyên tắc đặc quyền tối thiểu (*Least Privilege*) và nguyên tắc Đóng/Mở (*Open/Closed*).

---

### 3. Tổ Chức Đội Ngũ & Phân Chia Trách Nhiệm (Team 5 Thành Viên)

#### Vai Trò Trưởng Nhóm & Code Owner
- Trực tiếp đảm nhận vai trò **Team Leader**, chịu trách nhiệm điều phối tiến độ sprint, quy chuẩn Git, kiểm duyệt Pull Request và trực tiếp làm chủ mã nguồn (**Code Owner**) của module lõi `InventoryLedgerService`.
- **Ranh giới Leader vs BA:** Phân định rõ Leader quản lý con người, tiến độ và giải pháp kỹ thuật; BA làm chủ sơ đồ luồng nghiệp vụ, use-case và ma trận quy tắc kho.

#### Cơ Cấu Phân Chia Nhiệm Vụ Thành Viên:
- **Leader / Backend 1:** Thiết kế Prisma Schema, module lõi sổ cái tồn kho, hệ thống Auth/RBAC, Master Data, trục Nhập hàng (Inbound) và chức năng Điều chỉnh tồn.
- **Backend 2:** Phụ trách trục Xuất hàng (Outbound), thuật toán xếp đơn FEFO, xử lý các nhánh thiếu hàng, tích hợp bàn cân catch-weight và quy trình giao/trả hàng.
- **Frontend 1 (Mobile-first):** Xây dựng giao diện công nhân kho thực địa: quét mã vạch, nhận hàng tại cửa kho, danh sách nhặt hàng (pick-list) và màn hình đóng gói.
- **Frontend 2 (Desktop):** Xây dựng trang Admin quản trị Master Data, báo cáo chi tiết biến động sổ cái và màn hình duyệt phiếu điều chỉnh cho cấp Quản lý.
- **BA / QA:** Thiết kế tài liệu sơ đồ luồng End-to-End, xây dựng bộ kịch bản kiểm thử (Test Cases) bao quát các ca biên rủi ro cao (âm kho, cân thiếu, hàng cận date).

---

### 4. Mục Tiêu Gate-Check Tuần 1

- Thiết lập cơ chế họp Daily Standup 15 phút mỗi ngày và thống nhất kênh trao đổi kỹ thuật nội bộ.
- Dựng xong khung Repository, Docker Compose môi trường dev, cấu hình pipeline GitHub Actions CI cơ bản.
- Hoàn thiện tài liệu sơ đồ luồng nghiệp vụ kho để thuyết trình vượt qua cổng đánh giá đầu vào của mentor trước khi chính thức code ở Tuần 2.