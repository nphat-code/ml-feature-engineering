# Nội dung Slide Thuyết trình Seminar
**Chủ đề:** Chapter 5: Feature Engineering  
**Tài liệu chính:** *Designing Machine Learning Systems: An Iterative Process for Production-Ready Applications* – Chip Huyen (O'Reilly Media, 2022)  
**Tổng số slide:** 18 slides  

> Xem lời thoại thuyết trình tương ứng từng slide tại: [SPEAKER_SCRIPT.md](SPEAKER_SCRIPT.md)

---

## Slide 1: Trang bìa
- **Tiêu đề chính:** Chapter 5: Feature Engineering
- **Phụ đề:** *Designing Machine Learning Systems* – Chip Huyen (O'Reilly, 2022)
- **Thông tin môn học:** Seminar Môn Học Máy (Machine Learning)
- **Giảng viên hướng dẫn:** `[Tên giảng viên]`
- **Nhóm thực hiện:** `[Danh sách thành viên nhóm]`
- **Gợi ý bố cục & trực quan:** Bố cục tối giản, nền tương phản cao, kèm biểu tượng minh họa luồng chuyển đổi dữ liệu thô thành vector đặc trưng.

---

## Slide 2: Tổng quan nội dung (Agenda)
1. **Đặt vấn đề:** Tại sao Feature Engineering quyết định hiệu năng mô hình?
2. **Learned Features vs. Engineered Features:** So sánh và cách kết hợp thực tế.
3. **Common Feature Engineering Operations:** Các kỹ thuật xử lý đặc trưng phổ biến.
4. **Data Leakage (Rò rỉ dữ liệu):** Nguyên nhân, nhận diện và phòng chống.
5. **Engineering Good Features & Production:** Đánh giá, tổng quát hóa và Feature Store.
- **Gợi ý bố cục & trực quan:** Sơ đồ lộ trình 5 bước (timeline hoặc 5 thẻ đánh số) để người nghe dễ theo dõi cấu trúc bài nói.

---

## Slide 3: Đặt vấn đề – Tầm quan trọng của Feature Engineering
- **Định nghĩa:** Feature Engineering là quá trình biến đổi dữ liệu thô thành các đặc trưng hữu ích để mô hình học máy học được quy luật một cách hiệu quả nhất.
- **Trích dẫn:** *"Applied machine learning is basically feature engineering."* — Andrew Ng
- **Nguyên lý cốt lõi:** *"Garbage In, Garbage Out"* – Đầu vào là rác thì đầu ra cũng là rác.
- **Hai lý do cần Feature Engineering:**
  1. **Mô hình chỉ hiểu dữ liệu dạng số:** Cần chuẩn hóa văn bản, ngày giờ, ô trống (`NaN`) và thang đo.
  2. **Mô hình không tự hiểu ngữ cảnh nghiệp vụ:** Con người cần đưa kiến thức thực tế vào đặc trưng.
- **Ví dụ minh họa (Dự đoán thời gian chuyến xe):**
  - *Dữ liệu thô:* Tọa độ đón/trả, thời điểm `"17:45:00"`, thời tiết `"Mưa to"`.
  - *Đặc trưng tạo mới:* `khoang_cach_km = 8.5`, `is_rush_hour = 1`, `is_rain = 1`.
- **Thực tế (Facebook, 2014):** Trong bài toán dự đoán click quảng cáo, tìm ra đúng đặc trưng mang lại hiệu quả cải thiện vượt trội so với việc chỉ tinh chỉnh thuật toán.
- **Gợi ý bố cục & trực quan:** Chia 2 cột: Cột trái nêu nguyên lý và 2 lý do cốt lõi; Cột phải minh họa sơ đồ chuyển đổi từ bảng dữ liệu thô sang bảng đặc trưng số cho ví dụ chuyến xe.

---

## Slide 4: Learned Features vs. Engineered Features
- **Câu hỏi đặt ra:** Khi Deep Learning có thể tự học đặc trưng, Feature Engineering thủ công có còn cần thiết?

| Tiêu chí | Engineered Features (Thủ công) | Learned Features (Mô hình tự học) |
| :--- | :--- | :--- |
| **Nguồn gốc** | Con người thiết kế từ kiến thức nghiệp vụ | Mạng nơ-ron tự trích xuất trong quá trình huấn luyện |
| **Dữ liệu phù hợp** | Dữ liệu dạng bảng (Tabular Data: Excel, SQL) | Dữ liệu phi cấu trúc (Hình ảnh, Âm thanh, Văn bản) |
| **Khả năng giải thích** | Cao (White-box), rõ nguyên nhân dự đoán | Thấp (Black-box), biểu diễn dạng vector |
| **Yêu cầu tài nguyên** | Ít dữ liệu hơn, chi phí tính toán thấp | Cần lượng dữ liệu rất lớn và phần cứng GPU |

- **Thực tế trong Production:** Không thay thế nhau mà **kết hợp cả hai**.
  - *Ví dụ (Hệ thống gợi ý YouTube/Shopee):* Dùng **Learned Features** (vector embedding của video/sản phẩm) kết hợp với **Engineered Features** (số lần click trong 1 giờ qua, vị trí, thời gian).
- **Gợi ý bố cục & trực quan:** Đặt bảng so sánh ở trung tâm; phía dưới là sơ đồ khối minh họa luồng kết hợp cả 2 loại đặc trưng đi vào mô hình xếp hạng (Ranking Model).

---

## Slide 5: Common Feature Engineering Operations – Tổng quan
6 nhóm thao tác xử lý đặc trưng phổ biến trong Chapter 5:
1. **Handling Missing Values:** Xử lý dữ liệu khuyết thiếu.
2. **Scaling & Normalization:** Chuẩn hóa phạm vi và phân phối dữ liệu.
3. **Discretization (Binning):** Rời rạc hóa biến liên tục.
4. **Encoding Categorical Features:** Mã hóa biến phân loại sang dạng số.
5. **Feature Crossing:** Tổ hợp chéo các đặc trưng.
6. **Positional Embeddings:** Nhúng thông tin vị trí cho dữ liệu tuần tự.
- **Gợi ý bố cục & trực quan:** Sơ đồ cây hoặc 6 thẻ (grid 2x3) tóm tắt tên kỹ thuật kèm mục tiêu 1 dòng ngắn cho mỗi kỹ thuật.

---

## Slide 6: Xử lý dữ liệu khuyết thiếu (Handling Missing Values)
- **Bước 1: Hiểu 3 cơ chế gây thiếu dữ liệu**
  - **MCAR (Missing Completely at Random):** Thiếu hoàn toàn ngẫu nhiên (vd: lỗi đường truyền làm mất vài dòng log).
  - **MAR (Missing at Random):** Việc thiếu phụ thuộc vào một biến khác đã quan sát (vd: tỷ lệ bỏ trống cân nặng khác nhau theo giới tính).
  - **MNAR (Missing Not at Random):** Lý do thiếu phụ thuộc chính vào giá trị bị thiếu (vd: người có thu nhập rất cao từ chối khai báo mức lương).
- **Bước 2: Lựa chọn phương pháp xử lý**
  - **Deletion (Xóa dòng/cột):** Chỉ áp dụng khi tỷ lệ thiếu rất nhỏ (< 1–2%) và thuộc dạng MCAR. Tránh dùng với MNAR vì gây sai lệch mẫu.
  - **Imputation (Điền khuyết):**
    - Thống kê đơn giản: Mean, Median (ưu tiên khi có ngoại lai), Mode (biến phân loại).
    - Dựa trên mô hình: KNN Imputer, MICE (Iterative Imputer).
  - **Missing Indicator (Cột cờ đánh dấu):** Tạo thêm biến nhị phân `is_missing = 1` để giữ lại thông tin về hành vi không khai báo.
- **Gợi ý bố cục & trực quan:** Bên trái: Bảng tóm tắt 3 cơ chế MCAR, MAR, MNAR. Bên phải: Bảng dữ liệu mẫu minh họa việc điền Median kết hợp thêm cột `income_is_missing`.

---

## Slide 7: Chuẩn hóa phạm vi dữ liệu (Scaling & Normalization)
- **Vấn đề:** Chênh lệch thang đo giữa các biến (vd: `Tuoi` 18–65 vs. `Thu_nhap` 5–100 triệu).
- **Tác động theo nhóm thuật toán:**
  - *Thuật toán khoảng cách (KNN, SVM, K-Means):* Biến giá trị lớn chi phối khoảng cách Euclidean.
  - *Gradient Descent (Regression, Neural Nets):* Hàm mất mát bị kéo dẹt dạng elip, cập nhật dao động zic-zac, hội tụ chậm.
  - *Mô hình cây (Decision Tree, Random Forest, XGBoost):* **Không bị ảnh hưởng** vì chỉ xét thứ tự giá trị.
- **Ba phương pháp phổ biến:**
  - **Min-Max Scaling:** $x' = \frac{x - x_{min}}{x_{max} - x_{min}} \in [0, 1]$. Nhạy cảm cao với ngoại lai (outliers).
  - **Standardization (Z-score):** $z = \frac{x - \mu}{\sigma}$ ($\mu = 0, \sigma = 1$). Ổn định hơn trước ngoại lai.
  - **Log Transformation:** $x' = \log(x + 1)$. Xử lý phân phối lệch phải đuôi dài (doanh thu, lượt xem).
- **Nguyên tắc bắt buộc:** Chỉ `fit` trên tập **Train**, sau đó `transform` sang tập Validation/Test để tránh rò rỉ dữ liệu.
- **Gợi ý bố cục & trực quan:** Minh họa đồ thị đường đồng mức (contour plot) trước và sau Scaling cho Gradient Descent; kèm bảng so sánh 3 phương pháp.

---

## Slide 8: Rời rạc hóa (Discretization) & Mã hóa biến phân loại (Encoding)
- **1. Discretization (Binning):** Chia biến liên tục thành các khoảng rời rạc.
  - *Cách chia:* Equal-width (khoảng đều), Equal-frequency (theo phân vị/quantile), hoặc theo nghiệp vụ (nhóm tuổi).
  - *Lợi ích:* Giúp mô hình tuyến tính học được quan hệ phi tuyến, giảm nhiễu và ngoại lai. *Lưu ý:* Có thể mất thông tin chi tiết ở vùng biên khoảng.
- **2. Encoding Categorical Features:**

| Phương pháp | Cơ chế | Phù hợp khi | Nhược điểm / Lưu ý |
| :--- | :--- | :--- | :--- |
| **Ordinal** | Gán số nguyên $0, 1, 2...$ | Có thứ tự tự nhiên (Thấp < TB < Cao) | Gây ngộ nhận thứ tự nếu biến không có thứ tự |
| **One-Hot** | Vector nhị phân $[0, 1, 0]$ | Ít danh mục (low cardinality) | Ma trận thưa, bùng nổ số chiều khi nhiều danh mục |
| **Target** | Thay bằng trung bình nhãn mục tiêu | Nhiều danh mục, mô hình dạng bảng | Dễ overfitting/leakage $\rightarrow$ cần làm mượt (smoothing) & CV |
| **Hashing Trick** | Dùng hàm băm ánh xạ vào mảng cố định | Số danh mục rất lớn, có danh mục mới khi deploy | Có thể xảy ra xung đột băm (hash collision), khó giải thích |

- **Gợi ý bố cục & trực quan:** Nửa trên minh họa biểu đồ biến đổi `Tuổi` liên tục thành 4 nhóm (bins). Nửa dưới trình bày bảng so sánh 4 phương pháp Encoding.

---

## Slide 9: Feature Crossing & Positional Embeddings
- **1. Feature Crossing (Tổ hợp chéo đặc trưng):**
  - Tạo đặc trưng mới bằng cách kết hợp 2 hay nhiều đặc trưng gốc (ví dụ: `Loai_xe` $\times$ `Khung_gio`, hoặc `Vi_do` $\times$ `Kinh_do`).
  - *Tác dụng:* Giúp mô hình tuyến tính học được sự tương tác phức tạp giữa các biến.
  - *Lưu ý:* Nếu hai biến đều có nhiều danh mục ($M \times N$), số chiều sẽ tăng rất nhanh $\rightarrow$ cần chọn lọc hoặc dùng kết hợp với Hashing/Regularization.
- **2. Positional Embeddings (Nhúng vị trí):**
  - *Bối cảnh:* Dữ liệu chuỗi (văn bản NLP, lịch sử click, chuỗi thời gian) phụ thuộc vào thứ tự xuất hiện.
  - *Vấn đề:* Kiến trúc xử lý song song như Transformer không tự phân biệt được thứ tự phần tử.
  - *Giải pháp:* Cộng thêm vector biểu diễn vị trí (dạng hàm Sin/Cos cố định hoặc vector học được) vào vector đặc trưng đầu vào để mô hình hiểu vị trí tương đối và tuyệt đối.
- **Gợi ý bố cục & trực quan:** Nửa trái: Ma trận giao giữa `Loại xe` và `Khung giờ` minh họa Feature Crossing. Nửa phải: Sơ đồ cộng `Token Embedding + Positional Embedding` trong chuỗi đầu vào.

---

## Slide 10: Data Leakage – Vấn đề nghiêm trọng trong triển khai ML
- **Khái niệm:** **Data Leakage (Rò rỉ dữ liệu)** xảy ra khi thông tin từ nhãn mục tiêu hoặc từ tương lai vô tình xuất hiện trong dữ liệu đầu vào lúc huấn luyện, nhưng không có sẵn tại thời điểm dự đoán thực tế.
- **Hệ quả thực tế:**
  - Trên tập kiểm thử nội bộ (Offline): Độ chính xác rất cao (ví dụ 98–99%).
  - Khi đưa lên vận hành thật (Production): Mô hình sai lệch nghiêm trọng và không sử dụng được.
- **Ví dụ minh họa (Y tế):**
  - *Mục tiêu:* Dự đoán bệnh nhân có mắc bệnh nặng hay không khi mới tiếp nhận.
  - *Đặc trưng bị rò rỉ:* Cột `Ma_khoa_dieu_tri` hoặc `Ngay_nhap_vien`.
  - *Nguyên nhân:* Dữ liệu này chỉ được ghi nhận **sau khi** bác sĩ đã chẩn đoán bệnh xong.
- **Gợi ý bố cục & trực quan:** Sơ đồ trục thời gian (Timeline): Đánh dấu mốc **"Thời điểm dự đoán (Prediction Time)"**. Chỉ những dữ liệu sinh ra *trước* mốc này mới hợp lệ; dữ liệu *sau* mốc này là rò rỉ.

---

## Slide 11: Các nguyên nhân gây Data Leakage phổ biến
5 nguyên nhân thường gặp theo Chip Huyen:
1. **Tiền xử lý trước khi chia tập (Splitting after Preprocessing):** `fit` Scaler, Imputer hoặc chọn đặc trưng trên toàn bộ dataset rồi mới chia Train/Test.
2. **Rò rỉ theo thời gian (Time Leakage):** Dùng chia ngẫu nhiên (Random Split) cho dữ liệu chuỗi thời gian (vd: giá chứng khoán, doanh số), khiến mô hình dùng dữ liệu ngày mai để dự đoán ngày hôm qua.
3. **Rò rỉ theo nhóm (Group Leakage):** Các mẫu của cùng một thực thể (cùng một người dùng, cùng một bệnh nhân) xuất hiện ở cả tập Train và tập Test.
4. **Dữ liệu trùng lặp (Duplicates / Near-duplicates):** Bản ghi bị lặp lại nằm rải rác ở cả hai tập.
5. **Biến đại diện cho nhãn (Target Proxy / Leaked Features from Generation):** Đặc trưng được tạo ra hoặc cập nhật từ chính kết quả của biến mục tiêu.
- **Gợi ý bố cục & trực quan:** Trình bày 5 khối (cards) kèm một dòng minh họa trực quan cho từng lỗi (đặc biệt minh họa Random Split vs. Time-based Split).

---

## Slide 12: Cách phát hiện và phòng chống Data Leakage
- **Biện pháp phòng chống (Prevention):**
  1. **Chia tập dữ liệu trước (Split First):** Luôn tách Train/Validation/Test trước khi thực hiện bất kỳ phép biến đổi thống kê nào.
  2. **Đóng gói bằng `Pipeline`:** Sử dụng `sklearn.pipeline.Pipeline` để đảm bảo mỗi fold trong Cross-Validation chỉ `fit` trên phần dữ liệu huấn luyện.
  3. **Chọn chiến lược chia tập đúng bản chất:**
     - Dữ liệu thời gian $\rightarrow$ **Temporal Split** (huấn luyện trên quá khứ, kiểm thử trên tương lai).
     - Dữ liệu theo nhóm/người dùng $\rightarrow$ **GroupKFold** (đảm bảo cùng một đối tượng chỉ nằm ở 1 tập).
  4. **Kiểm tra mốc thời gian (Point-in-Time Correctness):** Xác minh nguồn thu thập của mọi cột trước khi đưa vào huấn luyện.
- **Cách phát hiện (Detection):**
  - Kiểm tra Feature Importance hoặc độ tương quan cao bất thường của một biến với nhãn.
  - Nghi ngờ và kiểm tra lại nếu kết quả trên tập Validation cao bất hợp lý so với bài toán thực tế.
  - Thử loại bỏ từng biến đáng ngờ (Ablation Study) để xem mức độ sụt giảm hiệu năng.
- **Gợi ý bố cục & trực quan:** Hai cột song song: Bên trái là "Phòng chống (4 quy tắc kỹ thuật)", bên phải là "Phát hiện (3 dấu hiệu cảnh báo)", kèm đoạn code minh họa ngắn 3 dòng của `Pipeline`.

---

## Slide 13: Đánh giá đặc trưng – Feature Importance
- **Tại sao cần đo Feature Importance?**
  - Loại bỏ đặc trưng nhiễu hoặc dư thừa.
  - Giảm kích thước mô hình, giảm độ trễ khi dự đoán (inference latency) và tiết kiệm chi phí lưu trữ.
  - Tăng khả năng giải thích mô hình cho các bên liên quan.
- **Các phương pháp phổ biến:**
  1. **Built-in Tree Importance:** Tính toán nhanh dựa trên mức độ giảm tạp chất (Gini/Entropy), nhưng dễ thiên lệch về các biến có nhiều giá trị phân biệt (high cardinality).
  2. **Permutation Importance:** Xáo trộn ngẫu nhiên giá trị một cột trên tập Validation và đo mức giảm hiệu năng. Khách quan, áp dụng được cho mọi loại mô hình.
  3. **SHAP (SHapley Additive exPlanations):** Dựa trên lý thuyết trò chơi, định lượng đóng góp biên của từng đặc trưng ở cả cấp độ toàn cục (toàn bộ dữ liệu) và cục bộ (từng mẫu dự đoán cụ thể).
- **Gợi ý bố cục & trực quan:** Minh họa một biểu đồ thanh ngang (Bar chart / SHAP summary plot) xếp hạng mức độ ảnh hưởng của 5 đặc trưng mẫu.

---

## Slide 14: Khả năng tổng quát hóa (Feature Generalization)
- **Mục tiêu:** Đặc trưng tốt không chỉ hoạt động tốt trên tập huấn luyện mà phải ổn định trên dữ liệu mới trong tương lai.
- **Hai tiêu chí cốt lõi theo Chip Huyen:**
  1. **Feature Coverage (Độ bao phủ):**
     - Tỷ lệ phần trăm mẫu dữ liệu có giá trị hợp lệ cho đặc trưng đó.
     - Đặc trưng có độ bao phủ quá thấp (chỉ xuất hiện ở vài trường hợp cá biệt) dễ gây overfitting và ít đóng góp chung.
  2. **Distribution Stability (Độ ổn định phân phối):**
     - Phân phối giá trị của đặc trưng cần ổn định theo thời gian.
     - Tránh các đặc trưng bị trôi dạt phân phối (distribution drift) quá nhanh khi hành vi người dùng hoặc bối cảnh thay đổi.
- **Sự đánh đổi (Trade-off):** Cần cân bằng giữa **độ đặc hiệu** (khớp sát dữ liệu hiện tại) và **tính tổng quát** (bền vững theo thời gian).
- **Gợi ý bố cục & trực quan:** Hai biểu đồ minh họa nhỏ: Biểu đồ 1 so sánh đặc trưng độ bao phủ cao vs. độ bao phủ thấp; Biểu đồ 2 minh họa sự dịch chuyển phân phối (Distribution Drift) giữa thời điểm $t_1$ và $t_2$.

---

## Slide 15: Mở rộng thực tế – Feature Store & MLOps
- **Thách thức trong Production: Training-Serving Skew**
  - Logic tính đặc trưng khi huấn luyện (Offline: Python/Pandas) khác với khi phục vụ trực tuyến (Online: Java/Go/SQL).
  - Hệ quả: Sai lệch dữ liệu đầu vào giữa lúc học và lúc dự đoán thật.
- **Giải pháp: Feature Store (Kho lưu trữ đặc trưng)**
  - Công cụ tiêu biểu: **Feast, Hopsworks, Tecton**.
  - **Lợi ích chính:**
    1. Đồng nhất định nghĩa đặc trưng giữa **Offline Store** (training) và **Online Store** (inference độ trễ thấp).
    2. Tái sử dụng đặc trưng giữa nhiều mô hình và nhiều nhóm kỹ sư khác nhau.
    3. Hỗ trợ theo dõi phiên bản (versioning) và kiểm soát chất lượng đặc trưng.
- **Gợi ý bố cục & trực quan:** Sơ đồ kiến trúc đơn giản: `Nguồn dữ liệu thô` $\rightarrow$ `Feature Engineering Pipeline` $\rightarrow$ `Feature Store (Offline & Online)` $\rightarrow$ `Training & Serving`.

---

## Slide 16: Tổng kết & Bài học rút ra
Ba nguyên tắc cốt lõi từ Chapter 5:
1. **Dữ liệu và Ngữ cảnh là nền tảng:** Kết hợp hợp lý giữa *Engineered Features* và *Learned Features*; chọn kỹ thuật xử lý (Missing, Scaling, Encoding) phù hợp với bản chất dữ liệu và thuật toán.
2. **Kiểm soát chặt chẽ Data Leakage:** Chia tập trước khi biến đổi, dùng `Pipeline`, áp dụng đúng *Temporal Split* hoặc *GroupKFold*, và kiểm tra tính khả dụng theo thời gian thực.
3. **Đơn giản, Đo lường và Tổng quát hóa:** Bắt đầu từ các đặc trưng đơn giản, đánh giá đóng góp bằng *Permutation Importance* hoặc *SHAP*, và ưu tiên các đặc trưng có độ bao phủ và độ ổn định cao.
- **Gợi ý bố cục & trực quan:** 3 thẻ tóm tắt lớn (3 Key Takeaways) rõ ràng, chữ to, dễ đọc từ xa.

---

## Slide 17: Tài liệu tham khảo & Video minh họa
- **Sách & Tài liệu chính quy:**
  1. Chip Huyen (2022), *Designing Machine Learning Systems*, O'Reilly Media (Chapter 5, pp. 140–170).
  2. Alice Zheng & Amanda Casari (2018), *Feature Engineering for Machine Learning*, O'Reilly Media.
  3. Max Kuhn & Kjell Johnson (2019), *Feature Engineering and Selection*, CRC Press.
- **Khóa học & Video tham khảo:**
  1. **Stanford CS 329S (Chip Huyen):** *Feature Engineering & Data Pipeline in ML Systems*.
  2. **StatQuest with Josh Starmer:** *Data Leakage in Machine Learning*; *One-Hot Encoding and Categorical Data*.
  3. **Made With ML (Goku Mohandas):** *Feature Engineering & Feature Store in Production*.
  4. **Google Cloud Tech:** *Best Practices for ML: Feature Engineering at Scale*.
- **Gợi ý bố cục & trực quan:** Chia 2 cột gọn gàng: Sách tham khảo bên trái, Khóa học/Video bên phải (có thể kèm mã QR dẫn tới repository GitHub chứa ghi chú chi tiết của nhóm).

---

## Slide 18: Cảm ơn & Thảo luận (Q&A)
- **Lời cảm ơn:** Cảm ơn thầy/cô và các bạn đã lắng nghe!
- **Gợi ý câu hỏi thảo luận mở:**
  1. Khi nào nên ưu tiên *Target Encoding* thay cho *Hashing Trick* (và ngược lại)?
  2. Trong bài toán gợi ý sản phẩm theo thời gian thực, làm thế nào để tránh *Data Leakage* khi tạo đặc trưng hành vi người dùng?
- **Gợi ý bố cục & trực quan:** Thiết kế thoáng, nổi bật dòng chữ "Q&A – Trao đổi và Thảo luận" kèm thông tin liên hệ hoặc link repository tài liệu.
