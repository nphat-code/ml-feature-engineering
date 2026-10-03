# KẾ HOẠCH & TỔNG HỢP NỘI DUNG SEMINAR
**Chủ đề:** Chapter 5: Feature Engineering  
**Môn học:** Machine Learning  
**Tài liệu chính:** *Designing Machine Learning Systems: An Iterative Process for Production-Ready Applications* - Chip Huyen (O'Reilly, 2022)  
**Thời gian bắt đầu báo cáo:** Tuần 8  
**Thời lượng thuyết trình:** 20 - 25 phút  
**Sản phẩm cần nộp:** Slide PPTX + Tài liệu tham khảo (References) + Link Video liên quan  

---

## I. YÊU CẦU SEMINAR (TỪ GIẢNG VIÊN)

1. **Thời lượng:** 20 – 25 phút (khoảng 15 – 20 slides).
2. **Hình thức:** Báo cáo thuyết trình bằng slide `.pptx`.
3. **Tiêu chí nội dung:**
   - Trình bày đầy đủ, chính xác các ý chính trong tài liệu chỉ định.
   - Nhấn mạnh và chia sẻ những nội dung quan trọng, các điểm nhóm thấy hay, có tính ứng dụng thực tế cao.
   - Bổ sung tài liệu tham khảo chất lượng cao và video minh họa (YouTube, Tech Talk, Course).

---

## II. TÓM TẮT CHI TIẾT CHAPTER 5: FEATURE ENGINEERING (TRANG 140 – 170)

### 1. Learned Features vs. Engineered Features
- **Engineered Features (Đặc trưng thiết kế thủ công):**
  - Dựa trên kiến thức chuyên ngành (domain knowledge).
  - Tốn nhiều thời gian và công sức, nhưng có tính diễn giải cao (interpretability) và đặc biệt hiệu quả với dữ liệu dạng bảng (tabular data).
- **Learned Features (Đặc trưng tự học qua Deep Learning / Representation Learning):**
  - Mô hình tự động trích xuất các biểu diễn đặc trưng (như CNN cho hình ảnh, Transformer/Embeddings cho ngôn ngữ).
  - Giảm bớt công sức thiết kế thủ công, tuy nhiên cần lượng dữ liệu lớn và thường là "hộp đen" (black-box).
- **Thực tế trong Production:**
  - Cả hai phương pháp bổ trợ lẫn nhau; hệ thống thực tế thường kết hợp cả đặc trưng thủ công (đặc thù nghiệp vụ) và đặc trưng tự học (embeddings).

---

### 2. Common Feature Engineering Operations (Các thao tác phổ biến)

#### A. Handling Missing Values (Xử lý dữ liệu khuyết thiếu)
- **3 Cơ chế thiếu dữ liệu:**
  1. *MCAR (Missing Completely at Random):* Thiếu hoàn toàn ngẫu nhiên, không phụ thuộc vào giá trị của bất kỳ biến nào.
  2. *MAR (Missing at Random):* Dữ liệu thiếu phụ thuộc vào một biến quan sát được khác (ví dụ: nam giới ít khai báo chỉ số cân nặng hơn nữ giới).
  3. *MNAR (Missing Not at Random):* Lý do thiếu phụ thuộc chính vào giá trị bị thiếu (ví dụ: người có thu nhập rất cao thường từ chối khai báo thu nhập).
- **Các phương pháp xử lý:**
  - *Xóa (Deletion):* Xóa dòng (Listwise) hoặc xóa cột; chỉ áp dụng khi tỷ lệ thiếu rất nhỏ hoặc MCAR.
  - *Điền khuyết (Imputation):*
    - Thống kê đơn giản: Mean, Median (dữ liệu lệch), Mode (biến phân loại).
    - Nâng cao: KNN Imputer, MICE (Iterative Imputer), Model-based imputation.
  - *Thêm cờ (Missing Indicator):* Tạo thêm một cột nhị phân `is_missing` để mô hình học được ý nghĩa của việc dữ liệu bị khuyết.

#### B. Scaling & Normalization (Chuẩn hóa phạm vi)
- Giúp các thuật toán nhạy cảm với khoảng cách (KNN, SVM, K-Means) và Gradient Descent hội tụ nhanh, ổn định.
- **Kỹ thuật chính:**
  - *Min-Max Normalization:* Đưa giá trị về đoạn $[0, 1]$. Nhược điểm: Rất nhạy cảm với ngoại lai (outliers).
  - *Standardization (Z-score Scaling):* Đưa về phân phối chuẩn có $\mu = 0, \sigma = 1$. Ít bị ảnh hưởng bởi outliers hơn.
- *Lưu ý sống còn:* Phải fit scaler trên tập **Train**, sau đó áp dụng (transform) sang tập Validation/Test để tránh rò rỉ dữ liệu.

#### C. Discretization (Rời rạc hóa / Binning)
- Chia một biến liên tục thành nhiều khoảng giá trị (bins) rời rạc.
- Giúp mô hình tuyến tính học được quan hệ phi tuyến, giảm tác động của outliers và nhiễu.
- Phương pháp: Equal-width (chia đều khoảng), Equal-frequency (chia theo phân vị/quantile).

#### D. Encoding Categorical Features (Mã hóa biến định loại)
- **One-Hot Encoding:** Phù hợp với biến có ít danh mục (low cardinality). Nhược điểm: Tạo ra ma trận thưa và bùng nổ số chiều.
- **Label / Ordinal Encoding:** Gán mỗi danh mục một số nguyên; chỉ phù hợp khi có thứ tự tự nhiên (ví dụ: Thấp, Trung bình, Cao).
- **Target Encoding:** Thay danh mục bằng trung bình nhãn mục tiêu tương ứng (cần kỹ thuật làm mượt - smoothing để tránh overfitting).
- **Hashing Trick (Feature Hashing):** Dùng hàm băm ánh xạ danh mục vào không gian cố định; giải quyết bài toán danh mục có số lượng cực lớn hoặc xuất hiện danh mục mới lúc deploy (unseen categories).

#### E. Feature Crossing (Tổ hợp chéo đặc trưng)
- Tạo biến mới từ việc kết hợp 2 hoặc nhiều biến ban đầu (ví dụ: `Loại_xe` $\times$ `Khung_giờ`).
- Cung cấp cho các mô hình tuyến tính khả năng nắm bắt tương tác phi tuyến tính phức tạp giữa các thuộc tính.

#### F. Positional Embeddings (Nhúng vị trí)
- Dùng cho dữ liệu có tính thứ tự (chuỗi thời gian, văn bản NLP).
- Hỗ trợ mô hình hiểu được vị trí tương đối và tuyệt đối của phần tử trong chuỗi.

---

### 3. Data Leakage (Rò rỉ dữ liệu - Điểm then chốt thực tế)

#### A. Khái niệm & Mức độ nguy hiểm
- Dữ liệu tập test (hoặc thông tin từ tương lai) vô tình xuất hiện trong quá trình huấn luyện.
- Hậu quả: Điểm đánh giá offline (train/val accuracy) rất cao, nhưng khi đưa lên production phục vụ người dùng thật thì mô hình hoàn toàn thất bại.

#### B. Các nguyên nhân thường gặp
1. **Tiền xử lý trước khi chia tập dữ liệu:** Fit Scaler, điền thiếu (mean/median), chọn feature trên toàn bộ dataset rồi mới train_test_split.
2. **Rò rỉ thời gian (Time-leakage):** Với dữ liệu chuỗi thời gian (time-series), chia tập ngẫu nhiên (random split) làm mô hình dùng dữ liệu tương lai để dự đoán quá khứ.
3. **Rò rỉ theo nhóm / thực thể (Group leakage):** Dữ liệu của cùng một người dùng/bệnh nhân xuất hiện rải rác ở cả train và test.
4. **Duplicate data / Gần trùng lặp:** Dữ liệu bị trùng xuất hiện ở cả 2 tập.
5. **Biến phụ thuộc vào nhãn (Target proxy):** Biến được thu thập sau khi sự kiện mục tiêu đã xảy ra (ví dụ: cột `Ngày_nhập_viện` dùng để dự đoán `Có_bị_bệnh_hay_không`).

#### C. Cách phát hiện & phòng chống
- Luôn chia tập dữ liệu (Split) trước khi thực hiện bất kỳ bước biến đổi nào.
- Sử dụng **Temporal Split** (chia theo mốc thời gian) cho bài toán thời gian.
- Sử dụng **GroupKFold** khi các mẫu có quan hệ nhóm.
- Nghi ngờ ngay khi một feature có correlation hoặc feature importance cao đột biến.
- Đóng gói toàn bộ luồng xử lý bằng `Pipeline` (ví dụ `sklearn.pipeline.Pipeline`).

---

### 4. Engineering Good Features (Thế nào là một đặc trưng tốt?)

1. **Feature Importance (Độ quan trọng của đặc trưng):**
   - Giúp loại bỏ đặc trưng nhiễu, giảm kích thước mô hình, tăng tốc độ dự đoán (inference latency).
   - Công cụ đánh giá: Permutation Importance, SHAP (SHapley Additive exPlanations), Tree-based Feature Importance.
2. **Feature Generalization (Khả năng tổng quát hóa):**
   - Đảm bảo feature bao phủ (coverage) được phần lớn dữ liệu, không chỉ xuất hiện trong vài trường hợp cá biệt.
   - Giá trị của feature phải ổn định theo thời gian, tránh bị trôi dạt phân phối (distribution drift) quá nhanh.

---

## III. DÀN Ý SLIDE THUYẾT TRÌNH (18 SLIDES / 20 - 25 PHÚT)

| Slide | Tiêu đề slide | Nội dung chính |
| :---: | :--- | :--- |
| **1** | Trang bìa | Tên đề tài: Chapter 5 - Feature Engineering, Giảng viên, Thành viên nhóm |
| **2** | Tổng quan nội dung | 4 phần chính: Learned vs Engineered, Common Operations, Data Leakage, Good Features |
| **3** | Đặt vấn đề: Tầm quan trọng của Feature Engineering | Câu nói nổi tiếng của Andrew Ng: "Applied machine learning is basically feature engineering." |
| **4** | Learned Features vs Engineered Features | So sánh ưu/nhược điểm, khi nào dùng phương pháp nào |
| **5** | Common Operations: Tổng quan | Sơ đồ phân nhánh các kỹ thuật xử lý |
| **6** | Xử lý Missing Values | 3 loại MCAR, MAR, MNAR; kỹ thuật Imputation & Missing Indicator |
| **7** | Chuẩn hóa dữ liệu (Scaling) | Min-Max vs Standardization; cạm bẫy fit scaler sai cách |
| **8** | Rời rạc hóa (Discretization) & Mã hóa (Encoding) | Binning; One-hot vs Ordinal vs Target Encoding; Hashing Trick |
| **9** | Feature Crossing & Positional Embeddings | Tạo đặc trưng kết hợp; ứng dụng trong Transformer & Time Series |
| **10** | **Data Leakage: Hiểm họa tiềm ẩn** | Khái niệm & tại sao mô hình 99% accuracy lại sập trên Production |
| **11** | Các nguyên nhân gây Data Leakage phổ biến | Split sai thứ tự, Time leakage, Group leakage, Target proxy |
| **12** | Giải pháp phòng ngừa Data Leakage | Sklearn Pipeline, Temporal split, GroupKFold, Data sanity check |
| **13** | Đánh giá đặc trưng: Feature Importance | Permutation Importance, SHAP values - Giải thích mô hình |
| **14** | Khả năng tổng quát hóa (Feature Generalization) | Coverage, Stability, tránh Overfitting do đặc trưng quá chi tiết |
| **15** | Mở rộng thực tế: Feature Store & MLOps | Giới thiệu ngắn về Feature Store (Feast, Hopsworks) giải quyết Training-Serving Skew |
| **16** | Tổng kết & Bài học rút ra | 3 takeaways chính: Đúng bản chất dữ liệu, Tránh Leakage, Đơn giản trước phức tạp sau |
| **17** | References & Video Links | Danh mục tài liệu sách, bài báo và link YouTube hay |
| **18** | Q&A | Lời cảm ơn và câu hỏi thảo luận |

---

## IV. TÀI LIỆU THAM KHẢO & VIDEO (ĐỂ ĐƯA VÀO BÁO CÁO)

### 1. Sách & Tài liệu chính quy
1. **Chip Huyen (2022)**, *Designing Machine Learning Systems*, O'Reilly Media. (Chapter 5: Feature Engineering, pp. 140–170).
2. **Alice Zheng & Amanda Casari (2018)**, *Feature Engineering for Machine Learning: Principles and Techniques for Data Scientists*, O'Reilly Media.
3. **Max Kuhn & Kjell Johnson (2019)**, *Feature Engineering and Selection: A Practical Approach for Predictive Models*, CRC Press.

### 2. Video & Khóa học chọn lọc (Link đề xuất)
1. **Stanford CS 329S (Chip Huyen)**: Bài giảng về *Feature Engineering & Data Pipeline in ML Systems*.
2. **StatQuest with Josh Starmer**:
   - *Data Leakage in Machine Learning* (Giải thích trực quan sinh động nguyên nhân rò rỉ dữ liệu).
   - *One-Hot Encoding and Categorical Data*.
3. **Made With ML (Goku Mohandas)**:
   - Bài giảng thực tế về *Feature Engineering & Feature Store in Production*.
4. **Google Cloud Tech**:
   - *Best Practices for ML: Feature Engineering at Scale*.
