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

---

## 📏 6. Chuẩn hóa phạm vi dữ liệu (Scaling & Normalization)

### 1. Bản chất & Lý do cần Scaling:
- **Vấn đề:** Các biến trong thực tế có đơn vị đo và thang đo rất chênh lệch (ví dụ: `Tuổi` từ 18 đến 65, nhưng `Thu nhập` từ 5.000.000 đến 100.000.000 VNĐ).
- **Tác động đến từng nhóm thuật toán:**
  - **Thuật toán dựa trên khoảng cách (KNN, SVM, K-Means):** Biến có độ lớn lớn hơn sẽ chi phối hoàn toàn phép tính khoảng cách (như khoảng cách Euclidean), khiến biến có giá trị nhỏ bị xem như không có ý nghĩa dù có thể chứa thông tin quan trọng.
  - **Thuật toán tối ưu Gradient Descent (Linear/Logistic Regression, Neural Networks):** Khi các biến không đồng đều thang đo, đường đồng mức của hàm mất mát có dạng elip dài và hẹp. Quá trình cập nhật Gradient sẽ dao động hình zic-zac, làm mô hình hội tụ rất chậm hoặc mất ổn định. Scaling đưa đường đồng mức về dạng gần tròn, giúp Gradient tiến thẳng về cực tiểu.
  - **Mô hình dạng cây (Decision Tree, Random Forest, XGBoost):** **Không bị ảnh hưởng** bởi scaling, do việc tìm điểm phân nhánh chỉ dựa trên thứ tự giá trị chứ không dựa trên độ lớn khoảng cách.

---

### 2. Các phương pháp phổ biến:

#### A. Min-Max Normalization (Đưa về đoạn cố định)
- **Công thức:** 
  $$x' = \frac{x - x_{min}}{x_{max} - x_{min}}$$
- **Đặc điểm:** Đưa toàn bộ giá trị về đoạn $[0, 1]$ (hoặc $[-1, 1]$).
- **Hạn chế:** Rất nhạy cảm với ngoại lai (outliers). Một giá trị cực đoan sẽ ép phần lớn các điểm dữ liệu còn lại co cụm sát về 0.

#### B. Standardization (Z-score Scaling)
- **Công thức:** 
  $$z = \frac{x - \mu}{\sigma}$$
  *(Trong đó $\mu$ là giá trị trung bình, $\sigma$ là độ lệch chuẩn).*
- **Đặc điểm:** Chuyển phân phối về dạng có trung bình $\mu = 0$ và độ lệch chuẩn $\sigma = 1$.
- **Ưu điểm:** Không bị giới hạn chặn trên/dưới cố định, ít bị bóp méo bởi ngoại lai hơn so với Min-Max.

#### C. Biến đổi Log (Log Transformation)
- **Công thức:** 
  $$x' = \log(x + 1)$$
  *(Dùng $+1$ để tránh lỗi khi $x = 0$).*
- **Ứng dụng:** Xử lý dữ liệu có phân phối lệch phải nghiêm trọng (phân phối đuôi dài - long-tail distribution) như giá nhà, lượt tương tác, doanh thu. Biến đổi Log giúp nén khoảng cách giữa các số cực lớn, đưa phân phối về gần dạng đối xứng hơn.

---

### 3. Bảng so sánh nhanh các phương pháp:

| Phương pháp | Khoảng giá trị đầu ra | Độ nhạy với Ngoại lai (Outliers) | Trường hợp nên áp dụng |
| :--- | :---: | :---: | :--- |
| **Min-Max Scaling** | $[0, 1]$ | Rất cao | Thuật toán cần giới hạn cố định (như xử lý ảnh pixel $0-255$). |
| **Standardization (Z-score)** | Không giới hạn cố định | Trung bình | Đa số các mô hình Gradient Descent, SVM, Logistic Regression. |
| **Log Transformation** | $[0, +\infty)$ | Rất thấp | Dữ liệu lệch đuôi dài (thu nhập, doanh số, số lượt view). |

---

### 4. Cạm bẫy sống còn: Rò rỉ dữ liệu (Data Leakage) khi Scaling
- **Sai lầm phổ biến:** Thực hiện `fit_transform` trên toàn bộ tập dữ liệu trước khi chia train/test:
  - Việc này khiến thông tin về giá trị lớn nhất, nhỏ nhất, hoặc độ lệch chuẩn của tập Test bị rò rỉ vào quá trình chuẩn hóa tập Train.
- **Quy tắc chuẩn mực:**
  1. Chia tập `train_test_split` trước.
  2. Chỉ tính toán tham số ($x_{min}, x_{max}, \mu, \sigma$) trên tập **Train** bằng `scaler.fit(X_train)`.
  3. Dùng nguyên bộ tham số đó để biến đổi (`transform`) cho cả tập Train, tập Validation và tập Test.

---

### 🎙️ 5. Kịch bản thuyết trình mẫu (~1.5 phút):
> *"Kính thưa thầy cô và các bạn, thao tác tiếp theo là **Scaling & Normalization** – tức đưa các biến về cùng một thang đo.  
> - Trong thực tế, các biến thường có đơn vị khác biệt hoàn toàn: ví dụ cột `Tuổi` dao động từ 20 đến 60, nhưng cột `Thu nhập` dao động từ hàng chục đến hàng trăm triệu.  
> - Với các mô hình tính khoảng cách như KNN hoặc SVM, biến `Thu nhập` sẽ chi phối toàn bộ kết quả, khiến biến `Tuổi` gần như bị vô hiệu hóa. Đồng thời, với mạng nơ-ron hoặc hồi quy, thang đo chênh lệch khiến Gradient Descent dao động zic-zac và hội tụ rất chậm. Riêng với các mô hình cây quyết định, bước này không bắt buộc vì phép chia nhánh chỉ phụ thuộc vào thứ tự tương đối.  
> - Có hai phương pháp thông dụng: **Min-Max Scaling** đưa dữ liệu về khoảng 0 đến 1, nhưng rất dễ bị méo nếu có giá trị ngoại lai; và **Standardization (Z-score)** đưa về trung bình bằng 0, độ lệch chuẩn bằng 1, ổn định hơn khi có ngoại lai. Với các biến phân phối lệch đuôi dài như doanh thu, ta dùng thêm **Log transform**.  
> - Một nguyên tắc sống còn khi làm việc: **Chỉ fit scaler trên tập Train**, sau đó mới áp dụng cùng tham số đó sang tập Test. Nếu tính min/max trên toàn bộ dữ liệu trước khi chia tập, chúng ta sẽ mắc lỗi rò rỉ dữ liệu (Data Leakage), làm kết quả đánh giá không còn chính xác."*

