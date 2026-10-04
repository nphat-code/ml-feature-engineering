# Ghi chú học tập - Chapter 5: Feature Engineering (Chip Huyen)

## 📌 Lời mở đầu (Tại sao đề tài này quan trọng?)
- **Mô hình tối tân không cứu được dữ liệu tồi:** Ngay cả những mô hình AI phức tạp nhất (State-of-the-art) vẫn hoạt động rất kém nếu không có một tập đặc trưng tốt.
- **Kinh nghiệm thực tế từ Facebook (2014):** Bài báo nổi tiếng về dự đoán click quảng cáo chỉ ra rằng: việc tìm ra đúng đặc trưng giúp tăng hiệu năng mô hình vượt trội hơn hẳn so với việc mất hàng tuần tinh chỉnh thuật toán.
- 👉 *Thực tế:* Phần lớn thời gian của Kỹ sư ML & Data Scientist trong doanh nghiệp là dành cho việc tìm tòi và xử lý đặc trưng.

---

## 📖 1. Feature Engineering là gì?
- **Feature (Đặc trưng):** Là các thông tin đầu vào dùng để mô tả đối tượng cần dự đoán (trong bảng dữ liệu, **mỗi cột chính là 1 feature** như: Tuổi, Giới tính, Giá tiền...).
- **Feature Engineering:** Là quá trình **xử lý, biến đổi và tạo ra các đặc trưng hữu ích** từ dữ liệu thô ban đầu để mô hình Học máy học được các quy luật tốt nhất.
- *Hình ảnh so sánh dễ nhớ:* Giống như **khâu sơ chế nguyên liệu nấu ăn** – dữ liệu thô mua về còn lẫn tạp chất, cần được rửa sạch, gọt vỏ thái miếng và nêm nếm gia vị kết hợp thì món ăn nấu ra mới ngon.

---


## ❓ 2. Tại sao lại cần Feature Engineering?
*Nguyên lý cốt lõi: "Garbage In, Garbage Out" – Động cơ xe đua dù xịn đến đâu nhưng nếu đổ xăng bẩn (dữ liệu thô chưa xử lý) thì xe không thể chạy.*

1. **Máy tính chỉ hiểu Số, không hiểu Đời thực:**
   - Dữ liệu thực tế đầy rẫy chữ viết (`"Nam/Nữ"`), ngày giờ (`2026-10-04 17:45`), ô bị trống (`NaN`), hoặc khoảng giá trị chênh lệch hàng triệu lần $\rightarrow$ Feature Engineering là bước bắt buộc để "dịch" dữ liệu thô sang các con số toán học mà thuật toán có thể tính toán được.
2. **Máy tính thiếu Trực giác của Con người (Domain Knowledge):**
   - Thuật toán ML chỉ là cỗ máy tính toán thống kê, nó **không tự hiểu ngữ cảnh đời thực**.
   - Con người dùng kiến thức chuyên ngành để chủ động "mớm" tín hiệu quan trọng cho mô hình thông qua các biến tạo mới.

---

## 💡 3. Ví dụ minh họa trực quan (Thấy ngay hiệu quả)

### 🚖 Ví dụ 1: Dự đoán thời gian chuyến xe Grab / Giao đồ ăn
- **Dữ liệu thô (Máy không hiểu ngữ cảnh):**
  - Tọa độ đón `(10.77, 106.70)`, Tọa độ trả `(10.82, 106.62)`.
  - Thời gian: `"2026-10-04 17:45:00"`.
  - Thời tiết: `"Mưa to"`.
- **Sau khi làm Feature Engineering (Cung cấp tín hiệu rõ ràng):**
  - Tính `khoang_cach_km = 8.5 km` (từ 2 cặp tọa độ $\rightarrow$ yếu tố quyết định thời gian đi).
  - Trích xuất `is_rush_hour = 1` (17h45 = giờ tan tầm) và `thu_trong_tuan = Friday` (chiều thứ 6 kẹt xe).
  - Mã hóa `is_rain = 1` (trời mưa tài xế chạy chậm hơn).
- **👉 Kết quả:** Biến các con số rời rạc thành bộ tín hiệu cực mạnh: *Đi 8.5 km + Giờ cao điểm chiều Thứ 6 + Trời mưa* $\rightarrow$ Mô hình dự đoán chuẩn xác thời gian chuyến đi sẽ kéo dài gấp đôi.

### 🏠 Ví dụ 2: Dự đoán Giá nhà
- **Dữ liệu thô:** `Chieu_dai = 20m`, `Chieu_rong = 5m`, `Nam_xay_dung = 2004`.
- **Sau khi làm Feature Engineering:**
  - `Dien_tich = Dai × Rong = 100 m²` (giá nhà tính theo m², biến diện tích tương quan trực tiếp với giá hơn là để 2 số đo riêng rẽ).
  - `Tuoi_nha = 2026 - 2004 = 22 nam` (phản ánh trực tiếp mức độ khấu hao công trình theo thời gian).

---

## ⚖️ 4. Learned Features vs. Engineered Features
*Câu hỏi mở đầu: "Deep Learning phát triển mạnh mẽ, liệu Feature Engineering có bị biến mất không?"*

### 1. Bản chất sự khác biệt:
- **Engineered Features (Đặc trưng thiết kế thủ công):**
  - Do con người vận dụng **kinh nghiệm & kiến thức nghiệp vụ (Domain Knowledge)** để chủ động tạo ra.
  - Thống trị trên **Dữ liệu dạng bảng (Tabular Data)** – loại dữ liệu phổ biến nhất trong doanh nghiệp (ngân hàng, tài chính, e-commerce).
- **Learned Features (Đặc trưng tự học):**
  - Mạng nơ-ron sâu tự động trích xuất các biểu diễn đặc trưng (Representation Learning) từ dữ liệu thô (ví dụ: CNN tự trích xuất đường nét, mắt mũi từ ảnh).
  - Vượt trội trên **Dữ liệu phi cấu trúc (Unstructured Data)** như Hình ảnh, Âm thanh, Video, Văn bản.

### 2. Bảng so sánh trực quan:

| Tiêu chí | 🛠️ Engineered Features (Thủ công) | 🤖 Learned Features (Tự học qua DL) |
| :--- | :--- | :--- |
| **Nguồn gốc** | Con người tự thiết kế dựa vào hiểu biết nghiệp vụ. | Mạng nơ-ron tự học trong quá trình huấn luyện. |
| **Dữ liệu thế mạnh** | **Dữ liệu dạng bảng (Tabular data)** (Excel, SQL). | **Dữ liệu phi cấu trúc** (Ảnh, Video, Âm thanh, Chữ). |
| **Tính giải thích** | **Rất cao (White-box):** Hiểu rõ tại sao mô hình ra quyết định. | **Thấp (Black-box):** Các ma trận vector/nhúng khó diễn giải. |
| **Tài nguyên & Dữ liệu** | Cần ít dữ liệu hơn, chạy nhẹ trên CPU/GPU cơ bản. | "Ngốn" dữ liệu lớn và bắt buộc GPU/tài nguyên mạnh. |
| **Hạn chế** | Tốn công sức người thiết kế, khó làm cho dữ liệu ảnh/âm thanh. | Khó giải thích, dễ overfit nếu lượng dữ liệu ít. |

### 3. Thực tế trong Production (Xu hướng kết hợp - Hybrid Approach):
- Trong các hệ thống lớn thực tế (TikTok, YouTube, Shopee), người ta **không loại trừ nhau mà kết hợp cả hai**:
  - Dùng **Learned Features** để hiểu nội dung (tạo vector Embeddings từ video/ảnh/text).
  - Dùng **Engineered Features** để nắm bắt hành vi nghiệp vụ (số lần click trong 1h qua, tỉ lệ xem hết video, địa điểm...).
  - 👉 Ghép cả hai nhóm đặc trưng này vào mô hình xếp hạng (Ranking Model) cuối cùng.


