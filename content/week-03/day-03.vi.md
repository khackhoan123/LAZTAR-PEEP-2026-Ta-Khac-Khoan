+++
title = "Ngày 03 - 30/09/2026 (On-site)"
weight = 3
+++

## BÁO CÁO TIẾN ĐỘ: KHỞI TẠO MÔI TRƯỜNG, MIGRATE 20 BẢNG LÊN NEON, KÍCH HOẠT SWAGGER & THẨM ĐỊNH UI/UX FIGMA

---

### 1. Các Công Việc Đã Hoàn Thành (Completed Tasks)

#### Khởi Tạo & Chuẩn Hóa Môi Trường Phát Triển Monorepo
- **Thiết lập nhánh chuẩn làm việc:** Clone mã nguồn dự án về máy trạm on-site, thiết lập và đồng bộ nhánh làm việc chuẩn `PEEP1` cho toàn bộ dự án Mini-WMS.
- **Cài đặt & đồng bộ dependencies:** Cài đặt thành công toàn bộ thư viện phụ thuộc cho cả hai phân hệ độc lập:
  - Phân hệ máy chủ `warehouse_be` chạy trên nền tảng NestJS.
  - Phân hệ giao diện `warehouse_fe` xây dựng trên nền tảng Next.js (App Router).
- **Cấu hình kết nối cơ sở dữ liệu phân tán:** Cấu hình chuẩn xác tệp biến môi trường `.env`, thiết lập chuỗi kết nối SSL an toàn trỏ trực tiếp đến máy chủ PostgreSQL dùng chung trên nền tảng điện toán đám mây Neon Cloud (`PEEP1`).

#### Hiện Thực Hóa Schema & Đồng Bộ Cơ Sở Dữ Liệu Qua Prisma ORM
- **Chạy Migration 20 bảng cơ sở dữ liệu:**
  - Hoàn tất dịch chuyển toàn bộ bản thiết kế schema đã audit ở Sprint 0 thành mã khai báo `schema.prisma`.
  - Thực thi lệnh migration thành công (`prisma migrate deploy`), khởi tạo đồng bộ trọn vẹn 20 bảng nghiệp vụ chuẩn trên cơ sở dữ liệu Neon Cloud mà không phát sinh lỗi xung đột khóa ngoại.
- **Tích hợp Git an toàn & Không xung đột:** Kiểm duyệt và merge mã nguồn từ nhánh tính năng (feature branch) vào nhánh chính `PEEP1` thông qua Pull Request chuẩn mực, đảm bảo lịch sử commit sạch và nhất quán.
- **Khởi tạo dữ liệu mẫu (Database Seeding):** Xây dựng và thực thi tập lệnh seed dữ liệu ban đầu phục vụ phát triển, bao gồm tài khoản quản trị hệ thống (Admin), cấu trúc ô kệ kho mẫu và danh mục sản phẩm nông sản thử nghiệm.
- **Xác thực toàn vẹn qua Prisma Studio:** Sinh mã Prisma Client (`npx prisma generate`) và kiểm tra trực quan cấu trúc toàn bộ 20 bảng cùng quan hệ dữ liệu mẫu trên công cụ Prisma Studio.

#### Triển Khai & Kiểm Thử Máy Chủ NestJS Backend
- **Khởi động dịch vụ:** Kích hoạt thành công server backend ở chế độ phát triển (Development Mode) tại cổng `3069` với trạng thái 0 cảnh báo và 0 lỗi biên dịch.
- **Kích hoạt tài liệu API Swagger UI:** Cấu hình và kích hoạt thành công giao diện tương tác Swagger UI tại endpoint `/api-docs`, sẵn sàng cung cấp hợp đồng giao tiếp API (API Contract) phục vụ quá trình tích hợp giữa Backend và Frontend.

#### Thẩm Định & Chuẩn Hóa Thiết Kế UI/UX (Frontend System)
- **Hoàn thiện thiết kế Figma cho các luồng cốt lõi:** Đồng hành rà soát các bản vẽ giao diện người dùng trên Figma bao gồm: Luồng Nhận hàng thực địa (Inbound Receiving), Danh mục quản lý hàng hóa (SKU Catalog), và Sơ đồ trực quan hóa ô kệ kho (Warehouse Map Layout).
- **Thống nhất tư duy Mobile-First:** Rà soát và chuẩn hóa luồng trải nghiệm người dùng, ưu tiên tối ưu hóa thao tác một tay và kích thước điểm chạm (Touch Targets) trên thiết bị di động cho các màn hình công nhân thao tác trực tiếp tại kho.

---

### 2. Tiến Độ Hiện Tại & Vấn Đề Gặp Phải (Status & Blockers)

- **Tiến độ hiện tại:** Hoàn thành 100% mục tiêu khởi tạo môi trường, hạ tầng cơ sở dữ liệu đám mây, kích hoạt máy chủ backend và chốt xong khung thiết kế UI/UX trên Figma; dự án sẵn sàng bước vào giai đoạn code song song cả BE và FE.
- **Vấn đề gặp phải & Giải pháp:** Quá trình kết nối từ máy trạm đến Neon Cloud ban đầu gặp độ trễ mạng nhẹ khi thực thi các script migration phức tạp; đã xử lý triệt để bằng cách tối ưu hóa cấu hình Connection Pooling và timeout hợp lý trong Prisma schema.

---

### 3. Kế Hoạch Ngày Tiếp Theo (Next Steps - 01/10/2026)

- **Lập Google Sheet phân rã chức năng (BE & FE):** Xây dựng bảng theo dõi chi tiết bám sát chu trình kho nông sản (Master Data $\rightarrow$ Inbound $\rightarrow$ Putaway $\rightarrow$ Allocation & Outbound); gán rõ độ ưu tiên (P0, P1, P2) cùng deadline cụ thể để làm chủ tiến độ.
- **Bàn giao task & Kích hoạt code Frontend:** Chuyển giao các luồng giao diện Figma đã chốt cho đội ngũ Frontend bắt đầu dựng layout, component base và ghép nối các API Master Data đầu tiên (SKU, Kho, Vị trí) từ Backend.
- **Thiết lập hạ tầng lưu trữ đám mây Cloudinary:** Khởi tạo tài khoản và trích xuất bộ khóa định danh (`CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`), tích hợp sẵn sàng vào biến môi trường để phục vụ lưu trữ ảnh đại diện, chứng từ nhận hàng và hóa đơn số.
- **Cập nhật tài liệu Từ điển dữ liệu (Data Dictionary):** Đối soát lại các trường bổ sung thực tế của bảng `users` (như `full_name`, `password_hash`) sau đợt migrate trên Neon để đồng bộ tài liệu khớp 100% với cơ sở dữ liệu thực tế.