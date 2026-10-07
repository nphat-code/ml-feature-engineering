# Ghi chú học tập - Chapter 5: Feature Engineering (Chip Huyen)

## 📌 Lời mở đầu (Tại sao đề tài này quan trọng?)
- **Mô hình phức tạp không bù đắp được dữ liệu kém chất lượng:** Ngay cả các mô hình tiên tiến (State-of-the-art) vẫn cho hiệu năng thấp nếu không được cung cấp tập đặc trưng phù hợp.
- **Thực nghiệm từ nghiên cứu của Facebook (2014):** Nghiên cứu về dự đoán tỷ lệ nhấp chuột (CTR) cho thấy việc tìm ra đặc trưng phù hợp mang lại cải thiện hiệu năng rõ rệt hơn nhiều so với việc chỉ tập trung tinh chỉnh thuật toán học máy.
- 👉 *Thực tế:* Trong quy trình phát triển hệ thống ML, phần lớn thời gian được dành cho việc thu thập, phân tích và xử lý đặc trưng.

---

## 📖 1. Feature Engineering là gì?
- **Feature (Đặc trưng):** Là các thông tin đầu vào dùng để mô tả đối tượng cần dự đoán (trong bảng dữ liệu, **mỗi cột chính là 1 feature** như: Tuổi, Giới tính, Giá tiền...).
- **Feature Engineering:** Là quá trình **xử lý, biến đổi và tạo ra các đặc trưng hữu ích** từ dữ liệu thô ban đầu để mô hình Học máy học được các quy luật tốt nhất.

---

## ❓ 2. Tại sao lại cần Feature Engineering?
*Nguyên lý cốt lõi: "Garbage In, Garbage Out" – Chất lượng đầu ra của mô hình phụ thuộc trực tiếp vào chất lượng và cách biểu diễn của dữ liệu đầu vào.*

1. **Mô hình học máy chỉ hiểu dữ liệu dạng số:**
   - Dữ liệu thô thường chứa nhiều định dạng khác nhau: văn bản (`"Nam/Nữ"`), chuỗi thời gian (`2026-10-04 17:45`), giá trị bị khuyết (`NaN`), hoặc khoảng giá trị có sự chênh lệch lớn $\rightarrow$ Feature Engineering là bước chuẩn hóa và chuyển đổi dữ liệu về dạng biểu diễn toán học phù hợp cho các giải thuật tính toán.
2. **Bổ sung tri thức miền nghiệp vụ (Domain Knowledge):**
   - Thuật toán ML chỉ thực hiện các phép tính thống kê mà không tự hiểu được ngữ cảnh thực tế của bài toán.
   - Kỹ sư sử dụng hiểu biết nghiệp vụ để chủ động tạo ra các đặc trưng có ý nghĩa, giúp mô hình nắm bắt được các mối quan hệ quan trọng ẩn trong dữ liệu.

---

## 💡 3. Ví dụ minh họa thực tế

### 🚖 Ví dụ 1: Dự đoán thời gian di chuyển chuyến xe
- **Dữ liệu thô:**
  - Tọa độ đón `(10.77, 106.70)`, Tọa độ trả `(10.82, 106.62)`.
  - Thời gian khởi hành: `"2026-10-04 17:45:00"`.
  - Thời tiết: `"Mưa to"`.
- **Sau khi thực hiện Feature Engineering:**
  - Tính `khoang_cach_km = 8.5 km` (tính từ tọa độ điểm đón và điểm trả).
  - Trích xuất `is_rush_hour = 1` (17h45 nằm trong khung giờ cao điểm).
  - Mã hóa `is_rain = 1` (điều kiện thời tiết bất lợi ảnh hưởng đến tốc độ di chuyển).
- **👉 Kết quả:** Chuyển đổi dữ liệu rời rạc thành các thuộc tính mang tính đại diện cao (*Khoảng cách 8.5 km + Giờ cao điểm + Mưa*), giúp mô hình ước lượng thời gian di chuyển sát với thực tế hơn.

### 🏠 Ví dụ 2: Dự đoán Giá bất động sản
- **Dữ liệu thô:** `Chieu_dai = 20m`, `Chieu_rong = 5m`, `Nam_xay_dung = 2004`.
- **Sau khi thực hiện Feature Engineering:**
  - `Dien_tich = Dai × Rong = 100 m²` (diện tích có tương quan tuyến tính rõ rệt hơn với giá trị bất động sản so với từng kích thước riêng lẻ).
  - `Tuoi_nha = 2026 - 2004 = 22 nam` (phản ánh mức độ hao mòn công trình theo thời gian).

---

## ⚖️ 4. Learned Features vs. Engineered Features
*Câu hỏi thảo luận: "Khi Deep Learning phát triển mạnh mẽ, liệu Feature Engineering có còn cần thiết không?"*

### 1. Bản chất sự khác biệt:
- **Engineered Features (Đặc trưng thiết kế thủ công):**
  - Do con người vận dụng **kinh nghiệm & kiến thức nghiệp vụ (Domain Knowledge)** để chủ động tạo ra.
  - Phù hợp và hiệu quả cao trên **Dữ liệu dạng bảng (Tabular Data)** – dạng dữ liệu phổ biến nhất trong các bài toán kinh doanh (tài chính, ngân hàng, thương mại điện tử).
- **Learned Features (Đặc trưng tự học):**
  - Mạng nơ-ron sâu tự động trích xuất các biểu diễn đặc trưng (Representation Learning) từ dữ liệu thô (ví dụ: mô hình CNN tự trích xuất đường nét, góc cạnh từ ảnh).
  - Phù hợp trên **Dữ liệu phi cấu trúc (Unstructured Data)** như Hình ảnh, Âm thanh, Video, Văn bản.

### 2. Bảng so sánh trực quan:

| Tiêu chí | 🛠️ Engineered Features (Thủ công) | 🤖 Learned Features (Tự học qua DL) |
| :--- | :--- | :--- |
| **Nguồn gốc** | Con người chủ động thiết kế dựa vào hiểu biết nghiệp vụ. | Mạng nơ-ron tự học trong quá trình huấn luyện. |
| **Dữ liệu thế mạnh** | **Dữ liệu dạng bảng (Tabular data)** (CSDL quan hệ, bảng tính). | **Dữ liệu phi cấu trúc** (Ảnh, Video, Âm thanh, Văn bản). |
| **Tính giải thích** | **Cao (White-box):** Dễ dàng diễn giải cơ chế đưa ra quyết định. | **Thấp (Black-box):** Biểu diễn dạng vector/embedding khó diễn giải trực tiếp. |
| **Tài nguyên & Dữ liệu** | Hoạt động tốt với tập dữ liệu vừa/nhỏ, chi phí tính toán thấp. | Yêu cầu tập dữ liệu lớn và tài nguyên tính toán cao (GPU/TPU). |
| **Hạn chế** | Tốn công sức xây dựng đặc trưng, khó áp dụng cho dữ liệu phi cấu trúc phức tạp. | Khó giải thích nguyên nhân dự đoán, dễ quá khớp (overfit) khi ít dữ liệu. |

### 3. Thực tế triển khai (Phương pháp kết hợp - Hybrid Approach):
- Trong các hệ thống sản xuất quy mô lớn (hệ thống gợi ý, xếp hạng tìm kiếm), hai hướng tiếp cận này **thường được kết hợp cùng nhau**:
  - Dùng **Learned Features** để mã hóa nội dung (vector nhúng của văn bản, hình ảnh).
  - Dùng **Engineered Features** để thể hiện các đặc trưng hành vi và nghiệp vụ (tần suất tương tác gần đây, địa điểm, thời điểm...).
  - 👉 Kết hợp cả hai nhóm đặc trưng vào mô hình xếp hạng (Ranking Model) cuối cùng.

### 🎙️ 4. Kịch bản thuyết trình gợi ý (~1.5 - 2 phút):
1. **Mở đầu:** *"Với sự phát triển mạnh mẽ của Deep Learning, có ý kiến cho rằng các kỹ thuật Feature Engineering truyền thống không còn cần thiết vì mô hình có thể tự học đặc trưng..."*
2. **Đối chiếu hai phương pháp:**
   - *Learned Features:* Rất phù hợp cho **dữ liệu phi cấu trúc** (hình ảnh, âm thanh, văn bản), tuy nhiên thường khó diễn giải và đòi hỏi lượng dữ liệu lớn cùng chi phí tính toán cao.
   - *Engineered Features:* Đặc biệt hiệu quả trên **dữ liệu dạng bảng (Tabular data)** - dạng dữ liệu chiếm phần lớn trong các bài toán ứng dụng thực tế. Phương pháp này giúp mô hình dễ giải thích, huấn luyện nhanh và ít phụ thuộc vào lượng dữ liệu khổng lồ.
3. **Kết luận và chuyển tiếp:** *"Trong thực tế, các hệ thống lớn thường áp dụng mô hình lai (Hybrid Approach) kết hợp cả hai nhóm đặc trưng. Để nắm rõ các kỹ thuật phổ biến được áp dụng trong xây dựng đặc trưng, chúng ta cùng chuyển sang phần tiếp theo..."*

---

## 🧩 5. Xử lý Dữ liệu khuyết thiếu (Handling Missing Values)
*Thời lượng đề xuất: 1 Slide (~1.5 đến 2 phút)*

### 1. 3 Cơ chế khuyết thiếu dữ liệu
Xác định đúng cơ chế thiếu giúp lựa chọn phương pháp xử lý thích hợp:

| Loại khuyết thiếu | Bản chất | Ví dụ thực tế |
| :--- | :--- | :--- |
| **MCAR** *(Missing Completely at Random)* | **Thiếu hoàn toàn ngẫu nhiên:** Xác suất thiếu không phụ thuộc vào bất kỳ biến số nào. | Lỗi đường truyền mạng ngẫu nhiên làm mất một số dòng bản ghi nhật ký hệ thống. |
| **MAR** *(Missing at Random)* | **Thiếu ngẫu nhiên có điều kiện:** Xác suất thiếu phụ thuộc vào một biến quan sát được khác trong tập dữ liệu. | Tỷ lệ điền thông tin cân nặng có sự chênh lệch theo biến *Giới tính*. |
| **MNAR** *(Missing Not at Random)* | **Thiếu không ngẫu nhiên:** Xác suất thiếu phụ thuộc trực tiếp vào chính giá trị của biến đó. | Đối tượng có mức thu nhập đặc biệt cao hoặc rất thấp thường từ chối khai báo thu nhập trong khảo sát. |

### 2. Các phương pháp xử lý phổ biến:
- **Xóa dòng/cột (Deletion):** Phù hợp khi tỷ lệ thiếu rất nhỏ ($< 1 - 2\%$) và dữ liệu thuộc dạng MCAR. Không nên áp dụng cho MNAR vì có thể làm biến dạng phân phối thực tế của mẫu.
- **Điền khuyết (Imputation):**
  - Phương pháp cơ bản: Điền Mean (trung bình) hoặc Median (trung vị - hạn chế ảnh hưởng của giá trị ngoại lai).
  - Phương pháp nâng cao: KNN Imputer, Iterative Imputer (MICE).
- **Kỹ thuật bổ trợ: Cờ báo dữ liệu khuyết thiếu (Missing Indicator):**
  - Tạo thêm cột nhị phân đánh dấu trạng thái bị khuyết (ví dụ: `income_is_missing = 1`).
  - *Ý nghĩa:* Lưu giữ thông tin về sự vắng mặt của dữ liệu như một đặc trưng riêng biệt, hỗ trợ mô hình nhận diện được các mẫu dữ liệu đặc thù.

### 🎙️ 3. Kịch bản thuyết trình gợi ý (~1.5 phút):
> *"Thưa thầy cô và các bạn, khi xử lý dữ liệu khuyết thiếu, tiếp cận theo cách máy móc như xóa dòng hoặc chỉ điền giá trị trung bình (Mean) có thể dẫn đến sai lệch phân phối. Trong thực tế:  
> - Cần phân biệt rõ cơ chế khuyết thiếu: dữ liệu có thể thiếu hoàn toàn ngẫu nhiên (MCAR), hoặc thiếu có hệ thống gắn liền với giá trị của chính biến đó (MNAR) – ví dụ nhóm thu nhập đặc thù thường không khai báo. Nếu chỉ điền giá trị trung bình vào nhóm này, ta sẽ làm biến dạng phân phối thực tế.  
> - Do đó, bên cạnh việc điền khuyết, một giải pháp hiệu quả là kết hợp kỹ thuật **Missing Indicator**: tạo thêm một đặc trưng nhị phân `income_is_missing = 1`. Đặc trưng này cung cấp thêm thông tin cho mô hình rằng dữ liệu từng bị khuyết, từ đó bảo toàn được tín hiệu phân loại quan trọng."*
