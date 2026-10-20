+++
title = "Ngày 04 - 15/10/2026 (Remote)"
weight = 4
+++

## BÁO CÁO TIẾN ĐỘ: TỐI ƯU DATABASE WORKER, THIẾT LẬP DYNAMIC CORS VERCEL & CẤU HÌNH CLOUD DEPLOY RENDER

---

### 1. Các Công Việc Đã Hoàn Thành (Completed Tasks)

#### Tối Ưu Hóa Database Worker & Khắc Phục Lỗi Nuốt Ngoại Lệ
- **Nâng cấp cơ chế bắt lỗi (`business-audit.service.ts` - Commit `04248e7`):** Khắc phục tình trạng worker chạy ngầm ghi log kiểm toán nuốt lỗi im lặng khi gặp sự cố mạng; bổ sung ghi log chi tiết cấu trúc lỗi (`err?.message || err`), hỗ trợ truy vết nhanh trạng thái kết nối tới PostgreSQL trên Cloud.
- **Chuẩn hóa phụ thuộc dự án:** Đồng bộ toàn diện tệp khóa phụ thuộc `package-lock.json`, xử lý dứt điểm các cảnh báo peer dependencies giữa NestJS và Prisma Client.

#### Cấu Hình Server Production & Thiết Kế Bộ Lọc Dynamic CORS Cho Vercel
- **Cấu hình lắng nghe mạng toàn cục (`main.ts` - Commit `e4b4e36`):**
  - Chuyển đổi địa chỉ lắng nghe máy chủ từ `127.0.0.1` sang tất cả giao diện mạng `0.0.0.0`, cho phép dịch vụ container hóa trên PaaS nhận request từ bên ngoài.
  - Cấu hình cổng động linh hoạt theo biến môi trường Render: `Number(process.env.PORT || 3069)`.
- **Cơ chế cấp phép CORS động (`allowed-origins.ts`):**
  - Thiết lập regex và bộ phân tích URL tự động cấp phép toàn bộ các tên miền con triển khai của Frontend trên Vercel (khớp pattern `https://*.vercel.app`), giúp các bản build preview và production của Vercel gọi API trơn tru mà không cần khai báo cứng từng domain.
  - Cho phép mặc định các môi trường phát triển cục bộ (`localhost:3000`, `3069`, `3100`).
  - Bỏ qua kiểm tra CORS đối với các request nội bộ không gửi `Origin` header (Server-to-Server, Health Check, Postman/cURL).
- **Kiểm thử tự động cho CORS (`allowed-origins.spec.ts`):** Viết bổ sung các ca kiểm thử đơn vị hồi quy, khẳng định bộ lọc CORS từ chối các origin giả mạo và chấp nhận hợp lệ 100% các request từ Vercel.

#### Khởi Tạo Cấu Hình Infrastructure as Code (`render.yaml`)
- **Thiết lập Blueprint triển khai tự động (Commit `b16429b`, `3c855a6`):**
  - Khởi tạo tệp `render.yaml` chuẩn hóa: Khai báo dịch vụ `freshlink-wms-be`, môi trường `node`, hạ tầng đặt tại khu vực `singapore` nhằm tối ưu độ trễ mạng thấp nhất về Việt Nam.
  - Tự động hóa chu trình phát hành: lệnh build `npm install && npm run build` và lệnh khởi chạy `npm run start:prod`.
  - Khai báo danh mục biến môi trường bắt buộc: `DATABASE_URL`, `ACCESS_TOKEN_SECRET`, `REFRESH_TOKEN_SECRET`, `CLOUDINARY_*`, `SEED_ADMIN_PASSWORD`.
  - Cập nhật tài liệu mẫu `warehouse_be/.env.example` phục vụ chuẩn hóa môi trường cho toàn đội ngũ.

---

### 2. Tiến Độ Hiện Tại & Vấn Đề Gặp Phải (Status & Blockers)

- **Tiến độ hiện tại:** Hoàn thành 100% công tác chuẩn bị hạ tầng triển khai Cloud; Backend kết nối thông suốt với Frontend trên Vercel qua cơ chế Dynamic CORS; cấu hình Render Blueprint sẵn sàng kích hoạt tự động.
- **Vấn đề gặp phải & Giải pháp:** Frontend trên Vercel sinh domain preview ngẫu nhiên theo từng commit dẫn đến lỗi CORS liên tục; đã xử lý triệt để bằng regex whitelist an toàn cho toàn bộ namespace `*.vercel.app` trên giao thức HTTPS.

---

### 3. Kế Hoạch Ngày Tiếp Theo (Next Steps - 16/10/2026 - On-site)

- Xây dựng kịch bản Master Data Automation nông sản thực tế (`seed-catalog.ts`): 15 SKUs, 8 UOMs quy đổi, 7 danh mục chuyên biệt và 5 nhà cung cấp.
- Cấp phát mã vạch tiêu chuẩn quốc tế EAN-13 cho từng đơn vị bao bì phục vụ máy quét barcode.
- Merge các ràng buộc nghiệp vụ ngày nhặt hàng từ nhánh `PEEP1` vào giao diện `peep1-uxui`.