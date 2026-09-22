+++
title = "Ngày 01 - 21/09/2026 (Remote)"
weight = 1
+++

## BÁO CÁO TIẾN ĐỘ: TỐI ƯU HIỆU NĂNG 60 FPS & THIẾT LẬP GIAO DIỆN XÚC GIÁC SKEUOMORPHISM

---

### 1. Tối Ưu Hiệu Năng Đồ Họa Thực Chiến (Khóa Cứng 60 FPS)

#### Giải Phóng Luồng Render Chính (Main Thread Optimization)
- **Phát hiện nút thắt cổ chai:** Logic lắng nghe sự kiện di chuột để tính toán ma trận xoay góc nhìn camera (Mouse Parallax) chạy liên tục bên trong hook `useFrame` gây xung đột với chuyển động nội suy của GSAP ScrollTrigger, dẫn đến hiện tượng giật khung hình khi người dùng di chuột nhanh.
- **Biện pháp xử lý:** Tắt bỏ hoàn toàn phép tính xoay ma trận này trong vòng lặp kết xuất, giữ camera chuyển động mượt mà thuần túy theo tọa độ cuộn chuột, giải phóng đáng kể chu kỳ xử lý của CPU và GPU.

#### Chuẩn Hóa Bộ Tham Số WebGL Renderer
- **Kiểm soát mật độ điểm ảnh (DPR):** Khóa trần hiển thị `dpr={[1, 1.25]}`. Cấu hình này ngăn trình duyệt cố render ở độ phân giải 2K/4K nguyên bản trên các màn hình Retina, chống tràn bộ nhớ VRAM và giữ máy luôn mát khi duyệt web.
- **Loại bỏ tính toán bóng đổ nặng:** Tắt cơ chế bóng đổ thời gian thực (`shadows={false}`), triệt tiêu các pass render ma trận bóng phức tạp không cần thiết đối với một landing page trình diễn.
- **Kích hoạt phần cứng tối đa:** Cấu hình Canvas khởi tạo với các cờ tối ưu hóa: `powerPreference: "high-performance"`, `depth: true`, `antialias: false`, giúp duy trì ổn định tốc độ khung hình 60 FPS trên đa dạng cấu hình thiết bị.

---

### 2. Thiết Lập Design System & Kiến Trúc Giao Diện Xúc Giác (Modern Skeuomorphism)

#### Bảng Token Màu Sắc & Hiệu Ứng Nền Đặc Trưng
- **Tone màu nhận diện:** Nền Đen Tuyền (`#000000`), điểm nhấn Vàng Kim (`#D4AF37`) và Hổ Phách Đồng (`#C88A35`).
- **Shader nền nghệ thuật:** Giữ nguyên 100% Background Shader dạng sóng kim loại lỏng (*Molten Bronze Shader*) chạy ngầm dưới nền kết hợp hiệu ứng hạt bụi sáng bay bổng (*Forge Sparks*), tạo chiều sâu thị giác sang trọng cho tổng thể công trình.

#### Chuẩn Hóa Hệ Thống Thẻ Kính Glassmorphism & Nút Bấm Xúc Giác
- **Thẻ kính nổi (Glassmorphic Cards):** Đóng gói toàn bộ tiêu đề, mô tả và badge thông tin vào trong các thẻ kính vát mép bo cong `rounded-2xl` (`bg-[#161412]/85`, `backdrop-blur-xl`, đổ bóng chìm đa tầng), giải quyết dứt điểm tình trạng văn bản bị chìm vào nền mô hình 3D.
- **Nút bấm xúc giác (Tactile CTA):** Tạo hình nút bấm dạng viên thuốc nổi khối (`rounded-full`) với hiệu ứng viền ánh kim phản chiếu vật lý, mang lại cảm giác tương tác cơ khí chân thực.

#### Nâng Cấp Typography & Khắc Phục Lỗi Font Tiếng Việt
- **Xử lý lỗi hiển thị:** Loại bỏ các lỗi vỡ chân chữ và giãn khoảng cách bất thường ở các ký tự tiếng Việt có dấu (`ế`, `ứ`, `ộ`, `ẫ`).
- **Tích hợp bộ font tối ưu qua Next.js Font:**
  - *Cormorant Garamond / Playfair Display:* Sử dụng cho hệ thống tiêu đề chính, định hình phong cách tạp chí kiến trúc đẳng cấp.
  - *Plus Jakarta Sans:* Sử dụng cho các đoạn văn bản mô tả nội dung, mang lại sự mạch lạc và tối ưu trải nghiệm đọc trên màn hình kỹ thuật số.

#### Tinh Chỉnh Micro-Badge & Đồng Bộ Validation Form
- Thay thế khung nhãn to thô cứng (`[ 01 / SHOWCASE KIẾN TRÚC ]`) bằng dạng Micro-Badge viên thuốc siêu nhỏ kèm chấm sáng màu hổ phách tinh tế.
- Xây dựng quy chuẩn kiểm tra hợp lệ dữ liệu (Validation) chặt chẽ cho trường Số điện thoại và Email với thông báo lỗi trực quan trước khi chuyển sang trạng thái gửi thành công.

---

### 3. Tích Hợp Hiệu Ứng Tương Tác WebGL Cao Cấp (OGL Glow Cursor)

#### Nâng Cấp Con Trỏ Chuột Phát Quang
- Loại bỏ hoàn toàn vòng tròn CSS/SVG trắng đơn điệu bám chuột ban đầu.
- Triển khai linh kiện vệt sáng phát quang (Glow Cursor) viết bằng engine WebGL OGL siêu nhẹ, hoạt động độc lập không làm sụt giảm tốc độ khung hình của scene 3D chính.
- Phối màu phát quang đồng bộ theo dải sáng nhận diện: Hổ Phách (`#C88A35`) và Kem Sáng (`#FFF6ED`).

#### Tinh Chỉnh Thuật Toán Fragment Shader
- Tái cấu trúc công thức tính toán độ suy giảm quang học (`taper/life`) trong Fragment Shader, đảm bảo điểm sáng to và rực nhất (*hotspot*) luôn tập trung ở đầu con trỏ chuột, sau đó vuốt đuôi mỏng dần và tan biến mượt mà theo gia tốc di chuyển tay người dùng.

---

### 4. Đánh Giá Hiện Trạng & Định Hướng Triển Khai Tiếp Theo

#### Kết Quả Đạt Được
- Hoàn thiện toàn bộ khung giao diện, chuyển động camera 3D 4 chặng, hiệu ứng vệt sáng WebGL, hệ thống thẻ thông tin Glassmorphism và showroom trưng bày 3 dự án tiêu biểu.
- Ứng dụng chạy mượt mà, khóa cứng 60 FPS, hiển thị tiếng Việt hoàn chỉnh và thể hiện chuẩn xác tinh thần nhận diện của Laztar Construction.

#### Định Hướng Tiếp Theo
- Đấu nối endpoint API chính thức để tiếp nhận và lưu trữ dữ liệu từ Form gửi yêu cầu dự toán vào hệ thống quản lý (CRM / Database).
- Thay thế các ảnh mockup dự án tạm thời bằng bộ ảnh render/chụp thực tế chất lượng cao từ ban thiết kế.