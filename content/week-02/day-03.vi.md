+++
title = "Ngày 03 - 23/09/2026 (On-Site)"
weight = 3
+++

## BÁO CÁO TIẾN ĐỘ: HOÀN THIỆN KIẾN TRÚC ĐA PHÂN HỆ, TỐI ƯU ĐA NỀN TẢNG & ENGINE XUẤT BẢN RESUME

---

### 1. Định Hình Kiến Trúc Định Tuyến Đa Phân Hệ (Multi-page Architecture)

#### Quy Hoạch Monorepo & Phân Tách Endpoint Độc Lập
- **Tư duy kiến trúc:** Thay vì khởi tạo 2 repository rời rạc gây phân mảnh quy trình quản lý, tận dụng tối đa cơ chế App Router của Next.js để tích hợp 2 phân hệ độc lập trên cùng một nền tảng:
  - Phân hệ thương hiệu 3D (`/`): Đóng vai trò Landing Page trình diễn không gian phục vụ hoạt động pitching.
  - Phân hệ hồ sơ kỹ sư (`/portfolio`): Cung cấp góc nhìn toàn diện về năng lực chuyên môn, học vấn và các dự án thực tế.
- **Tối ưu hóa CI/CD:** Hợp nhất một pipeline duy nhất trên Vercel Edge Network, giảm tải 50% chi phí vận hành và cấu hình triển khai.

#### Phân Bổ Tài Nguyên Đồ Họa Theo Ngữ Cảnh (Conditional Resource Allocation)
- **Vấn đề phát hiện:** Khi người dùng điều hướng sang `/portfolio`, nếu tiếp tục duy trì tải mô hình kiến trúc `building.glb` cùng các texture nặng sẽ gây lãng phí bộ nhớ VRAM không cần thiết và làm nóng thiết bị.
- **Giải pháp xử lý:** Tái cấu trúc component `LaztarCanvas.tsx` với cơ chế kiểm soát cờ nạp điều kiện `loadModel={false}`:
  - Trang cá nhân kế thừa trọn vẹn lớp nền nghệ thuật (Molten Bronze Liquid Shader) và hạt bụi sáng (Forge Sparks) để đồng bộ nhận diện thương hiệu.
  - Hệ thống tự động giải phóng (unmount) mô hình 3D và toàn bộ buffer hình học nặng khi vào `/portfolio`, giải phóng ngay lập tức bộ nhớ và dồn 100% hiệu năng render mượt mà cho nội dung tài liệu.

#### Số Hóa Dữ Liệu Năng Lực Kỹ Sư Chuyên Nghiệp
- Chuyển đổi toàn bộ hồ sơ năng lực sang ngôn ngữ giao diện Skeuomorphism: định vị rõ nét vai trò Full-Stack Software Engineer & Korean BrSE (TOPIK 3, ĐH FPT, FPT Software Academy).
- Xây dựng case-study kỹ thuật có chiều sâu cho 3 dự án trọng điểm: **CosMate** (xử lý bất đồng bộ, tính toán vector bộ nhớ), **CineManage System** (kiến trúc nghiệp vụ, kiểm soát giao dịch), và **KoiCareHome**.

---

### 2. Tinh Chỉnh Trải Nghiệm Người Dùng & Xử Lý Typography Chuyên Sâu

#### Tối Ưu Phân Cấp Thông Tin & Điểm Nhìn Trọng Tâm
- Thay thế placeholder monogram ban đầu bằng thẻ chân dung kỹ sư thực tế, áp dụng viền kim loại vát sáng và hiệu ứng đổ bóng đa tầng đồng bộ với Design System.
- Loại bỏ các chỉ số trạng thái phân tán trên Header; quy hoạch duy nhất một huy hiệu trạng thái sẵn sàng tiếp nhận công việc (*Availability Status*) tại Hero Section kèm hiệu ứng `animate-pulse` tinh tế.
- Tích hợp thanh điều hướng nội bộ (*Anchor Smooth Scrolling*) với hiệu ứng kính mờ (`backdrop-blur`) tại trung tâm Navbar, cho phép người dùng cuộn mượt đến các vùng thông tin: Giới Thiệu, Kỹ Năng, Dự Án, Học Vấn.

#### Giải Quyết Triệt Để Lỗi Thụt Lún Đường Cơ Sở Số Liệu (Oldstyle Figures Resolution)
- **Hiện tượng lỗi:** Khi sử dụng font serif cổ điển (*Cormorant Garamond*), các con số bị áp dụng kiểu số cũ (Oldstyle figures) với cao độ không đồng đều (số 3, 4, 7, 9 bị thụt đáy xuống dưới đường cơ sở), gây cảm giác lệch hàng khi hiển thị các số liệu kỹ thuật quan trọng.
- **Giải pháp kỹ thuật:**
  - Quy chuẩn phân cấp font: Giới hạn font serif cho tên nhận diện thương hiệu cá nhân; áp dụng đồng bộ font sans-serif hiện đại (*Plus Jakarta Sans*) cho toàn bộ các thông số kỹ thuật.
  - Kích hoạt thuộc tính CSS OpenType cao cấp:
    ```css
    font-variant-numeric: lining-nums tabular-nums;
    ```
  - Kết quả: 100% các số liệu (80+ APIs, 3 Roles, 100% Dockerized) đạt độ thẳng hàng tuyệt đối trên cùng một đường cơ sở và có bề ngang đồng đều.

---

### 3. Xây Dựng Engine Xuất Bản & Tối Ưu In Ấn Độc Lập (@media print Engine)

#### Phân Phối Bản PDF Kỹ Sư Tiêu Chuẩn
- Thiết lập đường dẫn phân phối tài sản tĩnh chuẩn hóa tại `/public/Ta-Khac-Khoan-CV.pdf`, phục vụ nhu cầu tải về và xác thực hồ sơ năng lực chính thức chỉ với 1 click.

#### Tái Cấu Trúc Toàn Diện Layout In Ấn Qua CSS
- **Thách thức kỹ thuật:** Khi người dùng kích hoạt lệnh in (hoặc Save as PDF) từ trình duyệt, giao diện nền tối cùng WebGL Canvas bị loang lổ, vỡ layout và tràn thành 8 trang in không thể sử dụng.
- **Giải pháp thiết lập:** Cô lập hoàn toàn phạm vi in ấn bằng truy vấn media `@media print`:
  - Ẩn toàn bộ các thành phần động: WebGL Canvas, Header thanh điều hướng, nút bấm thao tác và lớp nền phát sáng.
  - Tái cấu trúc layout từ giao diện thẻ kính nổi sang phong cách tài liệu tối giản chuẩn quốc tế (**Minimalist Tech Resume**): nền trắng 100%, chữ đen tuyền tương phản cao (`#000000`), phân cấp đề mục rõ ràng với các đường ngăn cách mảnh.
  - Tinh chỉnh tỷ lệ co giãn (print-scaling) để toàn bộ thông tin học vấn, kinh nghiệm và dự án được dàn trang vừa vặn, chuẩn mực trong **đúng 1 trang A4**.

---

### 4. Thích Ứng Đa Nền Tảng Toàn Diện (Full-Responsive Optimization)

#### Tối Ưu Không Gian 3D Cho Khung Hình Dọc (Mobile Port)
- **Thuật toán Camera tương thích:** Trên màn hình di động có tỷ lệ khung hình dọc (Aspect Ratio < 1), thuật toán tự động tính toán lại trường nhìn (Camera FOV / Distance) và dời trọng tâm mô hình lên 40% phía trên khung hình, giải quyết triệt để lỗi mô hình bị che khuất bởi nội dung chữ.
- **Cắt giảm tải GPU di động:** Giảm 50% mật độ hạt sáng (*Forge Sparks*) và vô hiệu hóa con trỏ WebGL (*GlowCursor*) trên màn hình cảm ứng để ưu tiên tài nguyên cho thao tác vuốt cuộn màn hình và tiết kiệm pin.

#### Tái Cấu Trúc Bố Cục Trang Portfolio Trên Di Động
- Chuyển đổi linh hoạt hệ thống lưới: từ bố cục 4 cột kỹ năng trên Desktop sang dạng 1-2 cột trên Tablet/Mobile mà không làm vỡ cấu trúc thẻ.
- Áp dụng Typography co giãn linh hoạt (*Fluid Typography*) và chuẩn hóa kích thước các vùng tương tác chạm (Touch Targets) đạt tiêu chuẩn Accessibility tối thiểu **44x44px**.

---

### 5. Thiết Lập Quy Trình Vận Hành & CI/CD Lên Production

- **Kiểm soát phiên bản:** Quản lý mã nguồn chặt chẽ trên GitHub thông qua phân tách nhánh tính năng và các commit rõ ràng theo chuẩn Conventional Commits.
- **Tự động hóa Build & Deploy:** Liên kết trực tiếp repository với Vercel Edge Network, thiết lập pipeline tự động chạy kiểm tra kiểu dữ liệu (`tsc --noEmit`) và tối ưu hóa gói bundle (`npm run build`) trước khi phát hành.
- **Kết quả nghiệm thu:** Toàn bộ hệ thống chính thức hoạt động ổn định trên môi trường Production toàn cầu, hỗ trợ cơ chế tự động re-deploy liên tục mỗi khi cập nhật mã nguồn mới.