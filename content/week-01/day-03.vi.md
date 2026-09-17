+++
title = "Ngày 03 - 17/09/2026 (On-site)"
weight = 3
+++

## BÁO CÁO TIẾN ĐỘ R&D & PHÁT TRIỂN LANDING PAGE 3D (LAZTAR CONSTRUCTION)

### 1. Nghiên cứu giải pháp & Xác lập Kiến trúc Trải nghiệm (Creative Strategy & System Architecture)

* **Phân tích và lựa chọn hướng tiếp cận đồ họa cho Web:** Thực hiện đánh giá thực nghiệm chuyên sâu giữa hai phương án: *Pre-rendered Image Sequence (Canvas Stream)* theo tiêu chuẩn trình diễn web của Apple và *Real-time WebGL Engine*.
* **Định hình công nghệ chủ đạo:** Triển khai trên nền tảng Next.js (App Router) kết hợp hệ sinh thái đồ họa Three.js, React Three Fiber (R3F), GSAP ScrollTrigger và tích hợp Lenis Smooth Scroll nhằm chuẩn hóa vật lý cuộn chuột mượt mà.
* **Thiết lập kịch bản chuyển động (Scroll-driven 3D Storytelling) 4 chặng:**
  * **Chặng 1 (Brand Hook):** Khẳng định ngôn ngữ kiến trúc và vị thế thương hiệu thông qua góc phối cảnh bao quát.
  * **Chặng 2 (Infrastructure & Landscape):** Điều hướng camera cận cảnh hạ tầng cảnh quan xanh cùng các giải pháp kỹ thuật ngoại thất.
  * **Chặng 3 (Capability Gallery):** Dẫn dắt tầm nhìn vào không gian trưng bày chiều sâu, làm nổi bật hồ sơ năng lực và các công trình tiêu biểu.
  * **Chặng 4 (Conversion Trigger):** Biến đổi trạng thái góc máy, kích hoạt Form dự toán và tiếp nhận liên hệ trực tiếp.

### 2. Khai thác & Xử lý Tài nguyên 3D Thực nghiệm (3D Asset Pipeline & Prototyping)

* **Khảo sát nguồn dữ liệu 3D:** Thử nghiệm các kênh cấp phép mô hình tiêu chuẩn bao gồm Sketchfab, Spline, Meshy AI và các kho dữ liệu kiến trúc mở.
* **Chuẩn hóa quy trình chuyển đổi định dạng:** Nghiên cứu quy trình chuyển đổi tệp thiết kế kỹ thuật từ `.skp` (SketchUp) sang `.glb` / `.gltf`, đảm bảo tối ưu hóa lưới đa giác (polygon mesh) và nén dung lượng truyền tải trên web.
* **Tích hợp thành công 2 bản phối cảnh thử nghiệm đa góc nhìn:**
  * Phương án kiến trúc hình khối tự tạo (Trụ sở hiện đại - Procedural Mesh).
  * Phương án công trình phức hợp thực tế (Sea Keep Landmark).

### 3. Hiện thực hóa Giao diện & Tối ưu Trải nghiệm Người dùng (UI/UX Implementation)

* **Xây dựng hệ thống UI Glassmorphism:** Thiết lập tone màu nhận diện chủ đạo Vàng Gold - Đen Titanium; hoàn thiện thanh điều hướng động (Global Navigation), thẻ tóm tắt năng lực và modal trạng thái tiếp nhận tư vấn.
* **Giải quyết triệt để các rào cản kỹ thuật WebGL:**
  * **Khử lỗi hiển thị bề mặt & Clipping:** Khắc phục triệt để lỗi xuyên thấu mặt phẳng và lỗi hiển thị mặt khuất (Backface Culling) thông qua kỹ thuật chuẩn hóa `DoubleSide`, tự động cân chỉnh tỉ lệ mô hình bằng `Bounds` và `Center`.
  * **Tối ưu chiều sâu không gian:** Thay thế các khối chặn cứng bằng hệ thống thẻ giao diện nổi trong suốt (Floating HUD Cards), duy trì trọn vẹn không gian 3D tương tác.
  * **Cân bằng ánh sáng môi trường đa chiều:** Kết hợp `Environment HDR Preset` với hệ thống Directional/Ambient Light, giúp chất liệu kính phản chiếu và bề mặt bê tông hiển thị khối chân thực.
  * **Kiểm soát hiệu năng phần cứng:** Tinh chỉnh chỉ số DPR (Device Pixel Ratio) và lược bỏ tính toán bóng đổ phức tạp để duy trì tốc độ khung hình (FPS) ổn định trên đa dạng thiết bị.

### 4. Kế hoạch tiếp theo (Next Steps)

* Tiếp nhận dữ liệu bản vẽ và mô hình 3D chính thức từ bộ phận thiết kế để thay thế mô hình mockup.
* Tinh chỉnh kịch bản lia camera cinematic bám sát thông điệp kinh doanh và thương hiệu của công ty.
* Hoàn thiện kết nối API gửi dữ liệu biểu mẫu dự toán về hệ thống quản lý dữ liệu khách hàng (CRM).