# Nội dung Slide Thuyết trình Seminar
**Chủ đề:** Chapter 5: Feature Engineering  
**Tài liệu chính:** *Designing Machine Learning Systems* – Chip Huyen (O'Reilly Media, 2022)  
**Tổng số slide:** 17 slides  
**Thời lượng dự kiến:** ~20 phút (thuyết trình) + 5 phút (hỏi đáp)

> Xem lời thoại tương ứng từng slide tại: [SPEAKER_SCRIPT.md](SPEAKER_SCRIPT.md)

---

## Slide 1: Giới thiệu đề tài
- **Đề tài:** **Chapter 5: Feature Engineering**
- **Tài liệu gốc:** *Designing Machine Learning Systems* – Chip Huyen (O'Reilly Media, 2022)
- **Gợi ý trực quan:** Bố cục tối giản tập trung vào tên đề tài, kèm sơ đồ dòng chảy dữ liệu: `Raw Data` $\rightarrow$ `Feature Engineering` $\rightarrow$ `Model`.

---

## Slide 2: Mục tiêu & Nội dung chính
1. **Bản chất Feature Engineering:** Vì sao quyết định hiệu năng mô hình?
2. **Learned Features vs. Engineered Features:** So sánh và ngữ cảnh sử dụng.
3. **Các kỹ thuật Feature Engineering cốt lõi:**
   - Xử lý dữ liệu khuyết thiếu (Missing values).
   - Chuẩn hóa phạm vi (Scaling & Normalization).
   - Rời rạc hóa (Binning) & Mã hóa danh mục (Categorical Encoding).
   - Tổ hợp đặc trưng (Feature Crossing) & Nhúng vị trí (Positional Embeddings).
4. **Data Leakage (Rò rỉ dữ liệu):** Nguyên nhân, cách phát hiện và phòng ngừa.
5. **Engineering Good Features:** Đánh giá độ quan trọng và khả năng tổng quát hóa.
- **Gợi ý trực quan:** Sơ đồ 5 khối nội dung nối tiếp rõ ràng.

---

## Slide 3: Đặt vấn đề – Tầm quan trọng của Feature Engineering
- **Định nghĩa:** Quá trình biến đổi dữ liệu thô thành các đặc trưng giúp mô hình học quy luật hiệu quả nhất.
- **Nguyên lý cốt lõi:** *"Garbage In, Garbage Out"* – Dữ liệu đầu vào kém thì mô hình không thể cho kết quả tốt.
- **Hai lý do bắt buộc phải làm Feature Engineering:**
  1. *Mô hình chỉ hiểu dữ liệu số:* Dữ liệu thực tế chứa văn bản, ngày giờ, ô trống (`NaN`), thang đo chênh lệch.
  2. *Mô hình không tự hiểu nghiệp vụ:* Cần đưa kiến thức thực tế vào đặc trưng.
- **Ví dụ (Dự đoán thời gian chuyến xe):**
  - *Dữ liệu thô:* Tọa độ `(10.77, 106.70)`, giờ `"17:45"`, thời tiết `"Mưa to"`.
  - *Đặc trưng tạo mới:* `khoang_cach = 8.5 km`, `is_rush_hour = 1`, `is_rain = 1`.
  - $\rightarrow$ Mô hình nắm bắt quy luật nhanh và dự đoán chính xác hơn nhiều so với dữ liệu thô.

---

## Slide 4: Learned Features vs. Engineered Features
- **Vấn đề:** Deep Learning tự học được đặc trưng, vậy có cần tạo đặc trưng thủ công?

| Tiêu chí | Engineered Features (Thủ công) | Learned Features (Mô hình tự học) |
| :--- | :--- | :--- |
| **Nguồn gốc** | Con người thiết kế từ kiến thức nghiệp vụ | Mạng nơ-ron tự trích xuất |
| **Dữ liệu phù hợp** | Dữ liệu dạng bảng (Tabular: Excel, SQL) | Dữ liệu phi cấu trúc (Ảnh, Âm thanh, Text) |
| **Khả năng giải thích** | Rõ ràng (White-box), minh bạch | Phức tạp (Black-box), vector ẩn |
| **Tài nguyên & Dữ liệu** | Cần ít dữ liệu, chi phí tính toán thấp | Cần lượng dữ liệu lớn và GPU |

- **Kết luận thực tế:** Kết hợp cả hai (ví dụ: YouTube dùng vector nhúng để hiểu nội dung video, kết hợp với các đặc trưng thủ công như số lượt click gần đây để sắp xếp thứ tự video hiển thị trên trang chủ).

---

## Slide 5: Tổng quan các kỹ thuật xử lý đặc trưng
Chapter 5 đúc kết 6 kỹ thuật xử lý cơ bản nhất:
1. **Handling Missing Values:** Nhận diện cơ chế và xử lý dữ liệu bị khuyết.
2. **Scaling & Normalization:** Đưa các biến số về cùng thang đo.
3. **Discretization (Binning):** Chuyển biến liên tục thành các khoảng rời rạc.
4. **Categorical Encoding:** Mã hóa biến chữ/danh mục sang số.
5. **Feature Crossing:** Tổ hợp chéo các biến để nắm bắt tương tác.
6. **Positional Embeddings:** Nhúng thông tin vị trí cho dữ liệu dạng chuỗi.
- **Gợi ý trực quan:** Lộ trình 6 bước theo dạng thẻ (grid 3x2).

---

## Slide 6: Xử lý dữ liệu khuyết thiếu (Missing Values)
- **1. Phân biệt 3 cơ chế thiếu:**
  - **MCAR (Thiếu hoàn toàn ngẫu nhiên):** Do lỗi khách quan, không phụ thuộc biến nào (vd: đứt mạng rớt vài dòng log).
  - **MAR (Thiếu ngẫu nhiên theo biến khác):** Phụ thuộc biến đã biết (vd: phụ nữ ít khai cân nặng hơn nam).
  - **MNAR (Thiếu không ngẫu nhiên):** Phụ thuộc chính giá trị đó (vd: người thu nhập rất cao từ chối khai mức lương).
- **2. Lựa chọn cách xử lý:**
  - *Xóa (Deletion):* Chỉ dùng khi tỷ lệ thiếu rất thấp (< 1–2%) và là MCAR. Không xóa khi MNAR vì làm mất nhóm dữ liệu đặc trưng.
  - *Điền khuyết (Imputation):* Mean, Median (tốt khi có ngoại lai), hoặc mô hình dự đoán (KNN).
  - *Cột cờ Missing Indicator:* Thêm cột `income_is_missing = 1` để giữ lại thông tin về hành vi không khai báo.

---

## Slide 7: Chuẩn hóa phạm vi dữ liệu (Scaling)
- **Lý do cần Scaling:** Tránh trường hợp biến số lớn (Thu nhập: hàng chục triệu) lấn át biến số nhỏ (Tuổi: 18–60).
- **Tác động theo thuật toán:**
  - *KNN, SVM, Gradient Descent:* **Bắt buộc**, giúp thuật toán tính khoảng cách chính xác và hội tụ nhanh.
  - *Mô hình cây (Decision Tree, XGBoost):* **Không cần**, vì chỉ chia nhánh dựa trên thứ tự giá trị.
- **3 phương pháp chính:**
  1. **Min-Max Scaling:** Ép về đoạn $[0, 1]$. Rất nhạy cảm với ngoại lai (outliers).
  2. **Standardization (Z-score):** Đưa về trung bình $\mu = 0$, độ lệch chuẩn $\sigma = 1$. Ổn định trước ngoại lai.
  3. **Log Transformation:** $x' = \log(x + 1)$, nén khoảng cách cho dữ liệu lệch đuôi dài (doanh thu, view).
- **Nguyên tắc sống còn:** Chỉ `fit` trên tập **Train**, dùng tham số đó để `transform` cho tập Test.

---

## Slide 8: Rời rạc hóa & Mã hóa biến phân loại
- **1. Discretization (Binning):**
  - Chia biến liên tục thành các khoảng (Equal-width, Quantile, hoặc theo ngưỡng nghiệp vụ).
  - Giúp mô hình tuyến tính học được quan hệ phi tuyến và giảm bớt ảnh hưởng của nhiễu.
- **2. Categorical Encoding (Mã hóa danh mục):**
  - **Ordinal Encoding:** Gán số $0, 1, 2...$ — chỉ dùng khi có thứ tự thật (*Thấp, Trung bình, Cao*).
  - **One-Hot Encoding:** Tạo cột nhị phân $[0, 1]$ — phù hợp khi số danh mục ít; tránh dùng khi có hàng nghìn danh mục.
  - **Target Encoding:** Thay danh mục bằng trung bình nhãn mục tiêu — hiệu quả cho bảng nhiều danh mục, cần kỹ thuật làm mượt (smoothing) để tránh quá khớp.
  - **Hashing Trick:** Dùng hàm băm ánh xạ danh mục vào vector cố định — tối ưu bộ nhớ và xử lý được danh mục mới lúc vận hành.

---

## Slide 9: Feature Crossing & Positional Embeddings
- **1. Feature Crossing (Tổ hợp chéo đặc trưng):**
  - Ghép 2 hoặc nhiều biến thành đặc trưng mới (vd: `Loại_xe` $\times$ `Khung_giờ`).
  - Giúp các mô hình tuyến tính nắm bắt tương tác phi tuyến phức tạp.
  - *Lưu ý:* Tránh tổ hợp các biến có quá nhiều danh mục vì gây bùng nổ số chiều.
- **2. Positional Embeddings (Nhúng vị trí):**
  - Áp dụng cho dữ liệu tuần tự (văn bản, chuỗi hành vi người dùng).
  - Mô hình như Transformer xử lý song song nên không tự nhận biết thứ tự từ.
  - Giải pháp: Cộng thêm vector mã hóa vị trí vào từng vector đặc trưng đầu vào.

---

## Slide 10: Data Leakage – Khái niệm & Tác hại
- **Định nghĩa:** Thông tin từ tập kiểm thử hoặc từ tương lai vô tình lọt vào quá trình huấn luyện, nhưng không có sẵn tại thời điểm dự đoán thật.
- **Hệ quả thực tế:**
  - Điểm đánh giá offline (Validation): Rất cao (98–99%).
  - Khi triển khai thực tế (Production): Sai lệch nghiêm trọng, mô hình hỏng.
- **Ví dụ thực tế (Y tế):**
  - *Mục tiêu:* Dự đoán bệnh nhân có chuyển nặng hay không khi mới vào viện.
  - *Đặc trưng bị rò rỉ:* `Mã_khoa_điều_trị` hoặc `Ngày_nhập_viện`.
  - *Bản chất:* Những thông tin này chỉ sinh ra **sau khi** bác sĩ đã khám và có kết luận bệnh.

---

## Slide 11: 5 Nguyên nhân gây Data Leakage phổ biến
1. **Tiền xử lý trước khi chia tập dữ liệu:** Chuẩn hóa hoặc điền khuyết trên toàn bộ dữ liệu trước khi gọi `train_test_split`.
2. **Rò rỉ theo thời gian (Time Leakage):** Dùng chia ngẫu nhiên (Random Split) cho dữ liệu chuỗi thời gian, lấy dữ liệu tương lai để dự đoán quá khứ.
3. **Rò rỉ theo nhóm (Group Leakage):** Dữ liệu của cùng một đối tượng (cùng người dùng, cùng bệnh nhân) nằm ở cả Train và Test.
4. **Dữ liệu trùng lặp (Duplicates):** Bản ghi trùng lặp xuất hiện ở cả hai tập do không lọc sạch từ đầu.
5. **Biến đại diện cho nhãn (Target Proxy):** Đặc trưng phản ánh kết quả của nhãn mục tiêu sau khi sự kiện đã xảy ra.

---

## Slide 12: Phòng chống & Phát hiện Data Leakage
- **Quy tắc phòng chống:**
  - **Luôn chia tập dữ liệu trước:** Mọi phép biến đổi thống kê chỉ được học trên tập Train.
  - **Sử dụng Pipeline:** Dùng `sklearn.pipeline.Pipeline` để đóng gói biến đổi và mô hình.
  - **Chia tập đúng bản chất:** Dùng **Temporal Split** cho dữ liệu thời gian; dùng **GroupKFold** cho dữ liệu theo nhóm đối tượng.
  - **Kiểm tra tính sẵn có:** Xác minh mọi đặc trưng đều đã tồn tại tại thời điểm dự đoán.
- **Dấu hiệu nhận biết:**
  - Điểm đánh giá cao một cách bất thường so với thực tế bài toán.
  - Có một đặc trưng chiếm độ quan trọng (Feature Importance) áp đảo các biến còn lại.

---

## Slide 13: Đánh giá đặc trưng – Feature Importance
- **Mục tiêu:** Loại bỏ đặc trưng thừa/nhiễu, giảm kích thước mô hình, tăng tốc độ dự đoán (giảm latency).
- **3 phương pháp thông dụng:**
  1. **Built-in Tree Importance:** Nhanh, nhưng dễ thiên vị các biến có nhiều giá trị phân biệt.
  2. **Permutation Importance:** Xáo trộn ngẫu nhiên từng cột trên tập Validation để đo mức sụt giảm hiệu năng. Khách quan và dùng được cho mọi mô hình.
  3. **SHAP (SHapley Additive exPlanations):** Dựa trên lý thuyết trò chơi, đo lường chính xác đóng góp của từng đặc trưng cho cả toàn bộ tập dữ liệu lẫn từng mẫu dự đoán riêng lẻ.

---

## Slide 14: Tính tổng quát hóa (Feature Generalization)
- **Yêu cầu:** Đặc trưng tốt phải giữ được hiệu quả ổn định trên dữ liệu mới trong tương lai.
- **Hai tiêu chí cốt lõi:**
  1. **Feature Coverage (Độ bao phủ):**
     - Tỷ lệ phần trăm mẫu có giá trị cho đặc trưng đó.
     - Đặc trưng chỉ xuất hiện ở dưới 1% mẫu dữ liệu dễ làm mô hình học vẹt (overfitting).
  2. **Distribution Stability (Độ ổn định phân phối):**
     - Phân phối giá trị của đặc trưng không được thay đổi quá nhanh theo thời gian (tránh Data Drift).
- **Nguyên tắc:** Cân bằng giữa độ chi tiết cho bài toán hiện tại và độ bền vững theo thời gian.

---

## Slide 15: Tổng kết – 3 Bài học cốt lõi
1. **Hiểu bản chất dữ liệu là nền tảng:** Chọn kỹ thuật xử lý (Missing, Scaling, Encoding) dựa trên cơ chế sinh dữ liệu và mô hình sử dụng; kết hợp cả đặc trưng thủ công và đặc trưng tự học.
2. **Kiểm soát chặt chẽ Data Leakage:** Luôn chia tập trước khi tiền xử lý, dùng Pipeline và chia theo thời gian hoặc nhóm thực thể.
3. **Ưu tiên sự đơn giản và bền vững:** Đo lường độ quan trọng bằng Permutation Importance/SHAP để loại bỏ biến nhiễu, ưu tiên đặc trưng có độ bao phủ và tính ổn định cao.

---

## Slide 16: Tài liệu tham khảo
- **Sách giáo trình:**
  1. Chip Huyen (2022), *Designing Machine Learning Systems*, O'Reilly Media (Chapter 5: Feature Engineering).
  2. Alice Zheng & Amanda Casari (2018), *Feature Engineering for Machine Learning*, O'Reilly Media.
  3. Max Kuhn & Kjell Johnson (2019), *Feature Engineering and Selection*, CRC Press.
- **Bài giảng & Video trực quan:**
  1. Stanford CS 329S (Chip Huyen) – *Feature Engineering & Data Pipeline*.
  2. StatQuest with Josh Starmer – *Data Leakage in Machine Learning*.

---

## Slide 17: Cảm ơn & Q&A
- **Lời cảm ơn:** Cảm ơn thầy/cô và các bạn đã theo dõi!
- **Câu hỏi thảo luận:**
  1. *Khi nào nên dùng Target Encoding thay vì Hashing Trick (và ngược lại)?*
  2. *Trong dữ liệu chuỗi thời gian, vì sao Random Split hầu như luôn dẫn tới Data Leakage?*
