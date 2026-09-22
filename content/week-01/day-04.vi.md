+++
title = "Ngày 04 - 18/09/2026 (Remote)"
weight = 4
+++

## BÁO CÁO KỸ THUẬT: HIỆN THỰC HÓA PIPELINE ASSET 3D & GIẢI QUYẾT TRIỆT ĐỂ LỖI KẾT XUẤT WEBGL

---

### 1. Tinh Gọn Pipeline & Tối Ưu Hóa Dữ Liệu Mô Hình 3D Thực Chiến

#### Khảo sát Thực Nghiệm & Hợp Nhất Nguồn Asset
- Sau các thử nghiệm sơ bộ từ Ngày 03, nhận thấy việc nạp đồng thời mô hình phụ (`sea_keep_lonely_watcher.glb`) và duy trì các nút bấm chuyển đổi model làm phân mảnh câu chuyện kiến trúc, đồng thời làm tăng gấp đôi thời gian tải trang ban đầu.
- Quyết định loại bỏ toàn bộ các asset dư thừa, chuẩn hóa duy nhất một file mô hình kiến trúc chuẩn tại: `/public/models/building.glb`.

#### Cấu Hình Bộ Giải Nén DRACO Loader
- Xử lý lỗi không nạp được file GLB nén thông qua việc tích hợp bộ giải nén `DRACOLoader` từ thư viện `@react-three/drei`.
- Đưa logic giải mã hình học sang Web Worker chạy ngầm, giúp giải nén lưới đa giác (polygon mesh) dung lượng lớn mà không làm đơ giao diện chính của trình duyệt.

#### Giải Thuật Tự Động Căn Tâm & Scale Tỉ Lệ (Bounding Box Normalization)
- **Vấn đề:** File xuất khẩu từ phần mềm 3D thường bị lệch trục tọa độ gốc (Pivot Offset), dẫn đến việc camera xoay quanh tâm ảo và mô hình bị tràn ra ngoài góc nhìn của camera.
- **Giải pháp:** Ứng dụng giải thuật tính toán hộp giới hạn `Box3` của Three.js:
  - Tự động quét toàn bộ đỉnh lưới để xác định vector tâm thực tế của khối kiến trúc.
  - Tịnh tiến mô hình đưa tâm hình học về chính xác tọa độ gốc `(0, 0, 0)`.
  - Tự động tính toán tỉ lệ phóng đại (bounding sphere radius) tương thích với trường nhìn (FOV) của camera, giúp khối trụ sở luôn nằm trọn vẹn trong khung hình ở mọi kích thước màn hình.

---

### 2. Tái Cấu Trúc Không Gian & Xử Lý Logic Thị Giác Showroom

#### Khắc Phục Lỗi Bố Trí Không Gian Triển Lãm
- **Phát hiện lỗi:** Bản dựng ban đầu đặt hệ vách trưng bày và các khung ảnh lơ lửng ngoài trời phía sau tòa nhà, làm mất tính thực tế của công trình.
- **Tái thiết kế:** Bóc tách toàn bộ hệ vách triển lãm và 3 khung ảnh dự án (`/public/images/projects`), đưa vào đặt hoàn toàn **BÊN TRONG** căn phòng kính của tòa nhà, đảm bảo tính chân thực khi camera thực hiện cú lia xuyên kính ở Chặng 3.

---

### 3. Debug Chuyên Sâu Các Rào Cản Kỹ Thuật WebGL

#### Xử Lý Triệt Để Hiện Tượng Xung Đột Mặt Phẳng (Z-Fighting Flicker)
- **Bản chất kỹ thuật:** Hai bề mặt trần nhà showroom và hệ dầm chịu lực nằm trên các mặt phẳng song song có khoảng cách trục Z quá nhỏ. Khi camera di chuyển xa, độ chính xác của bộ đệm Z-Buffer giảm sút, khiến GPU không xác định được bề mặt nào nằm trước và gây hiện tượng chớp nháy liên tục.
- **Quy trình xử lý 3 bước:**
  1. Kích hoạt `logarithmicDepthBuffer: true` trên WebGL Canvas để phân bổ độ chính xác bộ đệm theo hàm logarit, mở rộng dải phân định độ sâu.
  2. Tinh chỉnh lại khoảng cách cắt của camera (`near = 0.1`, `far = 1000`) nhằm hạn chế suy hao phân giải độ sâu của GPU.
  3. Gán thuộc tính `polygonOffset: true` kết hợp `polygonOffsetFactor: -1` lên vật liệu trần showroom để ép GPU ưu tiên hiển thị bề mặt trần lên trên hệ kết cấu ngầm.

#### Khử Lỗi Xuyên Thấu Mặt Phẳng & Bề Mặt Khuất (Backface Culling)
- Khắc phục lỗi một số mảng tường và vách kính bị biến mất khi đổi góc máy bằng cách thiết lập chuẩn hóa `side: THREE.DoubleSide` cho toàn bộ các vật liệu kiến trúc đặc thù.
- Sử dụng helper `Bounds` và `Center` để đảm bảo hệ thống mesh luôn được bao bọc an toàn, loại bỏ hiện tượng camera cắt xuyên tường ngoài ý muốn.

#### Tối Ưu Hóa Chiều Sâu & Ánh Sáng Môi Trường
- Loại bỏ hoàn toàn bức tường đen chắn thô cứng ban đầu; thay thế bằng cấu trúc thẻ giao diện nổi (Floating HUD Cards) trong suốt, bảo toàn trọn vẹn không gian 3D.
- Cân bằng ánh sáng môi trường đa chiều bằng cách kết hợp `Environment HDR Preset` với hệ thống Directional Light và Ambient Light, giúp độ bóng bẩy của vật liệu kính và độ nhám của bê tông hiển thị chân thực dưới mọi góc nhìn.