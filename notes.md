# Ghi chú học tập - Chapter 5: Feature Engineering (Chip Huyen)

## 📌 Lời mở đầu (Tại sao đề tài này quan trọng?)
- **Mô hình dù tốt cũng không cứu được dữ liệu kém:** Ngay cả các mô hình tiên tiến nhất vẫn hoạt động kém nếu thiếu tập đặc trưng phù hợp.
- **Bài học thực tế từ Facebook (2014):** Trong bài toán dự đoán click quảng cáo, việc tìm ra đúng đặc trưng mang lại hiệu quả vượt trội hơn hẳn so với việc mất nhiều tuần tinh chỉnh thuật toán.
- 👉 *Thực tế:* Trong các dự án Machine Learning, kỹ sư thường dành phần lớn thời gian để xử lý và tạo đặc trưng, thay vì chỉ huấn luyện mô hình.

---

## 📖 1. Feature Engineering là gì?
- **Feature (Đặc trưng):** Là thông tin đầu vào dùng để mô tả đối tượng dự đoán (trong bảng dữ liệu, **mỗi cột là 1 feature**: Tuổi, Giới tính, Giá tiền...).
- **Feature Engineering (Kỹ thuật tạo đặc trưng):** Là quá trình **biến đổi dữ liệu thô thành các đặc trưng hữu ích** để mô hình học máy dễ học quy luật nhất.

---

## ❓ 2. Tại sao cần Feature Engineering?
*Nguyên lý: "Garbage In, Garbage Out" – Đầu vào là rác thì đầu ra cũng là rác.*

1. **Mô hình học máy chỉ hiểu dữ liệu dạng số:**
   - Dữ liệu thực tế có nhiều dạng: văn bản (`"Nam/Nữ"`), ngày giờ, ô trống (`NaN`), số quá lớn hoặc quá nhỏ...
   - Cần Feature Engineering để chuyển tất cả về dạng số chuẩn hóa mà mô hình tính toán được.
2. **Mô hình không tự hiểu được ngữ cảnh thực tế:**
   - Thuật toán chỉ tính toán trên số liệu thuần túy, không có trực giác hay kiến thức đời thực.
   - Con người cần đưa hiểu biết nghiệp vụ vào để tạo ra các đặc trưng có nghĩa, giúp mô hình nắm bắt đúng bản chất bài toán.

---

## 💡 3. Ví dụ minh họa thực tế

### 🚖 Ví dụ 1: Dự đoán thời gian chuyến xe công nghệ
- **Dữ liệu thô (Mô hình khó học trực tiếp):**
  - Tọa độ đón `(10.77, 106.70)`, tọa độ trả `(10.82, 106.62)`.
  - Giờ đi: `"2026-10-04 17:45:00"`.
  - Thời tiết: `"Mưa to"`.
- **Sau khi tạo đặc trưng (Rõ nghĩa cho mô hình):**
  - `khoang_cach_km = 8.5 km` (tính từ tọa độ đón - trả).
  - `is_rush_hour = 1` (17h45 là giờ cao điểm).
  - `is_rain = 1` (trời mưa thì tốc độ xe giảm).
- **👉 Kết quả:** Thay vì để mô hình tự học từ tọa độ và chuỗi thời gian rời rạc, ta cung cấp trực tiếp: *Khoảng cách 8.5 km + Giờ cao điểm + Trời mưa* $\rightarrow$ Mô hình dự đoán thời gian chính xác hơn nhiều.

### 🏠 Ví dụ 2: Dự đoán Giá nhà
- **Dữ liệu thô:** `Chieu_dai = 20m`, `Chieu_rong = 5m`, `Nam_xay_dung = 2004`.
- **Sau khi tạo đặc trưng:**
  - `Dien_tich = 20 × 5 = 100 m²` (giá nhà gắn liền với diện tích, tốt hơn để 2 kích thước riêng biệt).
  - `Tuoi_nha = 2026 - 2004 = 22 nam` (nhà càng cũ thì giá trị càng giảm).

---

## ⚖️ 4. Learned Features vs. Engineered Features
*Câu hỏi: "Deep Learning tự học được đặc trưng, vậy Feature Engineering có còn cần thiết không?"*

### 1. Phân biệt nhanh:
- **Engineered Features (Tự tạo thủ công):**
  - Do con người tạo ra dựa trên kiến thức thực tế (Domain Knowledge).
  - Rất hiệu quả trên **dữ liệu dạng bảng (Tabular Data)** – loại dữ liệu phổ biến nhất trong ngân hàng, bán lẻ, tài chính.
- **Learned Features (Mô hình tự học):**
  - Mạng nơ-ron tự trích xuất đặc trưng từ dữ liệu thô (ví dụ: CNN tự nhận diện cạnh, mắt mũi trong ảnh).
  - Rất mạnh trên **dữ liệu phi cấu trúc** như Hình ảnh, Âm thanh, Video, Văn bản.

### 2. Bảng so sánh tóm tắt:

| Tiêu chí | 🛠️ Engineered Features (Thủ công) | 🤖 Learned Features (Mô hình tự học) |
| :--- | :--- | :--- |
| **Nguồn gốc** | Do con người thiết kế. | Mạng nơ-ron tự học trong quá trình huấn luyện. |
| **Dữ liệu phù hợp** | Dữ liệu dạng bảng (Excel, SQL). | Dữ liệu phi cấu trúc (Ảnh, Video, Văn bản). |
| **Khả năng giải thích** | **Dễ giải thích (White-box):** Hiểu rõ vì sao mô hình ra kết quả. | **Khó giải thích (Black-box):** Biểu diễn dạng vector khó diễn giải. |
| **Dữ liệu & Tài nguyên** | Cần ít dữ liệu hơn, huấn luyện nhẹ nhàng. | Cần lượng dữ liệu lớn và tài nguyên tính toán cao (GPU). |
| **Nhược điểm** | Tốn công sức con người, khó làm cho ảnh/chữ. | Khó giải thích, dễ quá khớp (overfit) khi ít dữ liệu. |

### 3. Thực tế trong sản xuất (Kết hợp cả hai):
- Trong thực tế, các hệ thống lớn (YouTube, TikTok, Shopee) luôn kết hợp cả hai:
  - Dùng **Learned Features** để hiểu nội dung (vector nhúng của video, sản phẩm).
  - Dùng **Engineered Features** để nắm bắt hành vi (số lần click trong 1 giờ qua, địa điểm, thời gian...).
  - Kết hợp cả hai nhóm đưa vào mô hình xếp hạng cuối cùng.

### 🎙️ 4. Kịch bản thuyết trình mẫu (~1.5 phút):
1. **Mở đầu:** *"Nhiều người nghĩ Deep Learning phát triển thì không cần tạo đặc trưng thủ công nữa vì mô hình tự học được. Điều này có đúng không?"*
2. **Bản chất:**
   - *Learned Features* rất mạnh với ảnh, chữ, âm thanh, nhưng cần dữ liệu khổng lồ, tính toán tốn kém và là hộp đen khó giải thích.
   - *Engineered Features* vẫn là lựa chọn hàng đầu cho dữ liệu dạng bảng – loại dữ liệu chiếm phần lớn trong doanh nghiệp. Mô hình nhẹ, chạy nhanh, cần ít dữ liệu và giải thích được rõ ràng.
3. **Thực tế:** *"Vì vậy, các hệ thống thực tế không chọn 1 trong 2 mà kết hợp cả hai. Tiếp theo, chúng ta sẽ đi vào các kỹ thuật xử lý đặc trưng cụ thể..."*

---

## 🧩 5. Xử lý Dữ liệu khuyết thiếu (Handling Missing Values)

### 1. 3 Cơ chế gây thiếu dữ liệu:
Cần hiểu vì sao dữ liệu bị thiếu trước khi quyết định cách xử lý:

| Cơ chế | Ý nghĩa đơn giản | Ví dụ thực tế |
| :--- | :--- | :--- |
| **MCAR** *(Missing Completely at Random)* | **Thiếu ngẫu nhiên 100%:** Việc thiếu không liên quan gì đến dữ liệu khác. | Đứt mạng ngẫu nhiên làm mất vài dòng nhật ký giao dịch. |
| **MAR** *(Missing at Random)* | **Thiếu phụ thuộc biến khác:** Việc thiếu có liên quan đến một cột thông tin khác đã biết. | Khảo sát sức khỏe: phụ nữ thường ít khi điền cân nặng hơn nam giới (phụ thuộc biến *Giới tính*). |
| **MNAR** *(Missing Not at Random)* | **Thiếu phụ thuộc chính giá trị đó:** Bản thân giá trị đó khiến nó không được ghi nhận. | Người có thu nhập quá cao thường từ chối khai báo mức lương trong khảo sát. |

### 2. Các cách xử lý trong thực tế:
- **Xóa dòng/cột (Deletion):**
  - Chỉ dùng khi số lượng thiếu rất ít ($< 1 - 2\%$) và là dạng ngẫu nhiên (MCAR).
  - Tránh xóa khi dữ liệu dạng MNAR vì sẽ làm mất hẳn nhóm đối tượng đặc biệt.
- **Điền khuyết (Imputation):**
  - Cơ bản: Điền Mean (trung bình) hoặc Median (trung vị - tốt khi có số liệu ngoại lai).
  - Nâng cao: Dùng mô hình dự đoán giá trị thiếu (KNN, MICE).
- **Thêm cột cờ đánh dấu (Missing Indicator):**
  - Tạo cột nhị phân đánh dấu ô đó từng bị trống (ví dụ: `income_is_missing = 1`).
  - *Lợi ích:* Biến thông tin "người dùng không chịu khai báo" thành một đặc trưng riêng cho mô hình học.

### 🎙️ 3. Kịch bản thuyết trình mẫu (~1.5 phút):
> *"Thưa thầy cô và các bạn, khi thấy dữ liệu bị trống, sai lầm phổ biến là vội vàng xóa dòng hoặc chỉ điền số trung bình. Trong thực tế, ta cần hiểu lý do vì sao dữ liệu bị thiếu:  
> - Dữ liệu có thể rơi mất ngẫu nhiên do lỗi hệ thống (MCAR), nhưng cũng có thể bị khuyết có lý do (MNAR) – ví dụ người có thu nhập rất cao thường không chịu khai báo. Nếu chỉ điền số trung bình vào, ta sẽ vô tình làm mất thông tin về nhóm người này.  
> - Vì vậy, giải pháp hiệu quả là dùng **Missing Indicator**: tạo thêm cột `income_is_missing = 1`. Cột cờ này giúp mô hình nhận biết được hành vi không khai báo, giữ trọn vẹn thông tin hữu ích cho bài toán."*
