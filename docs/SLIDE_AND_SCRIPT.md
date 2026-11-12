# Nội dung Slide & Lời thoại Thuyết trình Seminar
**Chủ đề:** Chapter 5: Feature Engineering  
**Tài liệu chính:** *Designing Machine Learning Systems: An Iterative Process for Production-Ready Applications* – Chip Huyen (O'Reilly Media, 2022)  
**Tổng số slide:** 18 slides  
**Tổng thời lượng dự kiến:** 21 – 22 phút (nằm trong khung chuẩn 20 – 25 phút, dư 3 – 4 phút cho phần Hỏi đáp)

---

## Bảng phân bổ thời lượng tổng quát

| Phần | Slides | Thời lượng dự kiến |
| :--- | :---: | :---: |
| Mở đầu & Đặt vấn đề | Slide 1 – 3 | 2.5 phút |
| Phần 1: Learned Features vs. Engineered Features | Slide 4 | 1.5 phút |
| Phần 2: Common Feature Engineering Operations | Slide 5 – 9 | 7.0 phút |
| Phần 3: Data Leakage (Rò rỉ dữ liệu) | Slide 10 – 12 | 5.0 phút |
| Phần 4: Engineering Good Features & Production | Slide 13 – 15 | 4.0 phút |
| Tổng kết, Tài liệu tham khảo & Q&A | Slide 16 – 18 | 2.0 phút (+ Q&A) |
| **Tổng cộng** | **18 slides** | **22.0 phút** |

---

## Slide 1: Trang bìa
- **Thời lượng:** 0.5 phút

### Nội dung trên Slide
- **Tiêu đề chính:** Chapter 5: Feature Engineering
- **Phụ đề:** *Designing Machine Learning Systems* – Chip Huyen (O'Reilly, 2022)
- **Thông tin môn học:** Seminar Môn Học Máy (Machine Learning)
- **Giảng viên hướng dẫn:** `[Tên giảng viên]`
- **Nhóm thực hiện:** `[Danh sách thành viên nhóm]`

### Gợi ý bố cục & Trực quan
- Bố cục tối giản, nền sáng hoặc tối tương phản cao, kèm một biểu tượng minh họa luồng chuyển đổi dữ liệu thô thành vector đặc trưng.

### Lời thoại thuyết trình
> "Kính chào thầy/cô và các bạn. Hôm nay, nhóm chúng em xin trình bày chuyên đề Seminar Chương 5: **Feature Engineering** (Kỹ thuật trích xuất và tạo đặc trưng), dựa trên cuốn sách *Designing Machine Learning Systems* của tác giả Chip Huyen. Bài trình bày của nhóm dự kiến kéo dài khoảng 22 phút."

---

## Slide 2: Tổng quan nội dung (Agenda)
- **Thời lượng:** 0.5 phút

### Nội dung trên Slide
1. **Đặt vấn đề:** Tại sao Feature Engineering quyết định hiệu năng mô hình?
2. **Learned Features vs. Engineered Features:** So sánh và cách kết hợp thực tế.
3. **Common Feature Engineering Operations:** Các kỹ thuật xử lý đặc trưng phổ biến.
4. **Data Leakage (Rò rỉ dữ liệu):** Nguyên nhân, nhận diện và phòng chống.
5. **Engineering Good Features & Production:** Đánh giá, tổng quát hóa và Feature Store.

### Gợi ý bố cục & Trực quan
- Sử dụng sơ đồ lộ trình 5 bước (timeline/numbered cards) để người nghe dễ theo dõi cấu trúc bài nói.

### Lời thoại thuyết trình
> "Nội dung trình bày hôm nay được chia thành năm phần chính. Đầu tiên, chúng ta sẽ tìm hiểu vai trò của Feature Engineering. Tiếp theo là phần so sánh giữa đặc trưng tự học và đặc trưng thiết kế thủ công. Phần thứ ba đi sâu vào các thao tác xử lý đặc trưng thường gặp. Phần thứ tư tập trung vào một vấn đề rất phổ biến trong thực tế là rò rỉ dữ liệu (Data Leakage). Cuối cùng là các tiêu chí đánh giá một đặc trưng tốt và cách quản lý đặc trưng trong hệ thống sản xuất."

---

## Slide 3: Đặt vấn đề – Tầm quan trọng của Feature Engineering
- **Thời lượng:** 1.5 phút

### Nội dung trên Slide
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

### Gợi ý bố cục & Trực quan
- Chia 2 cột: Cột trái nêu nguyên lý và 2 lý do cốt lõi; Cột phải minh họa sơ đồ chuyển đổi từ bảng dữ liệu thô sang bảng đặc trưng số cho ví dụ chuyến xe.

### Lời thoại thuyết trình
> "Trước hết, chúng ta cần hiểu **Feature Engineering là gì**. Nói một cách trực diện, mỗi cột thông tin mô tả đối tượng trong bảng dữ liệu là một đặc trưng (feature). Feature Engineering là quá trình biến đổi dữ liệu thô ban đầu thành các đặc trưng có ý nghĩa, giúp mô hình học máy nắm bắt quy luật nhanh và chính xác hơn.
>
> Giáo sư Andrew Ng từng nhận xét: *'Học máy ứng dụng về bản chất chính là kỹ thuật tạo đặc trưng.'* Nguyên lý rất đơn giản: **Đầu vào là rác thì đầu ra cũng là rác**. Dù mô hình có phức tạp đến đâu nhưng thiếu đặc trưng phù hợp thì hiệu năng vẫn kém. Có hai lý do chính cho điều này: Thứ nhất, mô hình học máy chỉ xử lý được dữ liệu dạng số, trong khi dữ liệu thực tế chứa đầy văn bản, ngày tháng hay ô bị bỏ trống. Thứ hai, thuật toán không có trực giác đời thực.
>
> Ví dụ, khi dự đoán thời gian di chuyển của một chuyến xe công nghệ, nếu chỉ đưa tọa độ thô và chuỗi thời gian `'17:45'`, mô hình rất khó tự suy luận. Thay vào đó, kỹ sư tính ra `khoang_cach = 8.5 km`, gắn cờ `is_rush_hour = 1` và `is_rain = 1`. Việc đưa đúng ngữ cảnh thực tế như vậy giúp mô hình dự đoán chính xác hơn rất nhiều. Thực tế tại Facebook năm 2014 cũng cho thấy, trong bài toán dự đoán click quảng cáo, việc bổ sung đúng đặc trưng mang lại bước tiến lớn hơn hẳn so với việc mất nhiều tuần tinh chỉnh thuật toán."

---

## Slide 4: Learned Features vs. Engineered Features
- **Thời lượng:** 1.5 phút

### Nội dung trên Slide
- **Câu hỏi đặt ra:** Khi Deep Learning có thể tự học đặc trưng, Feature Engineering thủ công có còn cần thiết?

| Tiêu chí | Engineered Features (Thủ công) | Learned Features (Mô hình tự học) |
| :--- | :--- | :--- |
| **Nguồn gốc** | Con người thiết kế từ kiến thức nghiệp vụ | Mạng nơ-ron tự trích xuất trong quá trình huấn luyện |
| **Dữ liệu phù hợp** | Dữ liệu dạng bảng (Tabular Data: Excel, SQL) | Dữ liệu phi cấu trúc (Hình ảnh, Âm thanh, Văn bản) |
| **Khả năng giải thích** | Cao (White-box), rõ nguyên nhân dự đoán | Thấp (Black-box), biểu diễn dạng vector |
| **Yêu cầu tài nguyên** | Ít dữ liệu hơn, chi phí tính toán thấp | Cần lượng dữ liệu rất lớn và phần cứng GPU |

- **Thực tế trong Production:** Không thay thế nhau mà **kết hợp cả hai**.
  - *Ví dụ (Hệ thống gợi ý YouTube/Shopee):* Dùng **Learned Features** (vector embedding của video/sản phẩm) kết hợp với **Engineered Features** (số lần click trong 1 giờ qua, vị trí, thời gian).

### Gợi ý bố cục & Trực quan
- Đặt bảng so sánh ở trung tâm; phía dưới là sơ đồ khối minh họa luồng kết hợp cả 2 loại đặc trưng đi vào mô hình xếp hạng (Ranking Model).

### Lời thoại thuyết trình
> "Một câu hỏi thường gặp là: ngày nay Deep Learning đã tự học được đặc trưng, vậy kỹ thuật tạo đặc trưng thủ công có còn cần thiết không? Câu trả lời là **có**, vì hai phương pháp này phục vụ những loại dữ liệu khác nhau.
>
> **Learned Features** sử dụng mạng nơ-ron để tự trích xuất biểu diễn, rất hiệu quả trên dữ liệu phi cấu trúc như hình ảnh, âm thanh hay văn bản, nhưng đòi hỏi lượng dữ liệu lớn, chi phí tính toán cao và khó giải thích kết quả. Ngược lại, **Engineered Features** lại phát huy ưu thế trên dữ liệu dạng bảng – vốn là dạng dữ liệu phổ biến nhất trong tài chính, ngân hàng và thương mại điện tử. Phương pháp này cần ít dữ liệu hơn, mô hình nhẹ và giải thích được rõ ràng cho bộ phận nghiệp vụ.
>
> Do đó, trong các hệ thống sản xuất thực tế như YouTube hay Shopee, người ta kết hợp cả hai: dùng Learned Features để biểu diễn nội dung video hoặc sản phẩm dưới dạng vector, đồng thời dùng Engineered Features để nắm bắt hành vi người dùng theo thời gian thực, rồi đưa cả hai nhóm vào mô hình xếp hạng cuối cùng."

---

## Slide 5: Common Feature Engineering Operations – Tổng quan
- **Thời lượng:** 0.5 phút

### Nội dung trên Slide
6 nhóm thao tác xử lý đặc trưng phổ biến trong Chapter 5:
1. **Handling Missing Values:** Xử lý dữ liệu khuyết thiếu.
2. **Scaling & Normalization:** Chuẩn hóa phạm vi và phân phối dữ liệu.
3. **Discretization (Binning):** Rời rạc hóa biến liên tục.
4. **Encoding Categorical Features:** Mã hóa biến phân loại sang dạng số.
5. **Feature Crossing:** Tổ hợp chéo các đặc trưng.
6. **Positional Embeddings:** Nhúng thông tin vị trí cho dữ liệu tuần tự.

### Gợi ý bố cục & Trực quan
- Sơ đồ cây hoặc 6 thẻ (grid 2x3) tóm tắt tên kỹ thuật kèm mục tiêu 1 dòng ngắn cho mỗi kỹ thuật.

### Lời thoại thuyết trình
> "Tiếp theo, chúng ta bước vào phần trọng tâm kỹ thuật: 6 thao tác xử lý đặc trưng phổ biến nhất theo tổng hợp của Chip Huyen, bao gồm: xử lý dữ liệu khuyết thiếu, chuẩn hóa phạm vi, rời rạc hóa, mã hóa biến phân loại, tổ hợp đặc trưng và nhúng vị trí. Chúng ta sẽ đi lần lượt vào bản chất và lưu ý thực tế của từng kỹ thuật."

---

## Slide 6: Xử lý dữ liệu khuyết thiếu (Handling Missing Values)
- **Thời lượng:** 1.5 phút

### Nội dung trên Slide
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

### Gợi ý bố cục & Trực quan
- Bên trái: Bảng tóm tắt 3 cơ chế MCAR, MAR, MNAR. Bên phải: Bảng dữ liệu mẫu minh họa việc điền Median kết hợp thêm cột `income_is_missing`.

### Lời thoại thuyết trình
> "Thao tác đầu tiên là xử lý dữ liệu khuyết thiếu. Khi gặp các ô trống `NaN`, sai lầm phổ biến là vội vàng xóa dòng hoặc điền ngay giá trị trung bình. Trước khi xử lý, ta cần xác định cơ chế khiến dữ liệu bị thiếu.
>
> Có ba cơ chế: **MCAR** là thiếu ngẫu nhiên do sự cố kỹ thuật; **MAR** là thiếu có liên quan đến một cột thông tin khác đã biết; và quan trọng nhất là **MNAR** – thiếu do chính bản thân giá trị đó, ví dụ người có thu nhập rất cao thường từ chối khai báo lương trong khảo sát. Nếu xóa bỏ các dòng MNAR, ta sẽ làm mất hẳn nhóm đối tượng này. Còn nếu chỉ điền giá trị trung bình, ta sẽ làm sai lệch bản chất dữ liệu.
>
> Vì vậy, ngoài các cách điền khuyết như Median hay mô hình KNN, giải pháp rất hiệu quả trong thực tế là dùng **Missing Indicator**: tạo thêm một cột cờ nhị phân như `income_is_missing = 1`. Cột cờ này giúp mô hình nhận biết hành vi không khai báo như một đặc trưng riêng biệt, bảo toàn trọn vẹn thông tin hữu ích."

---

## Slide 7: Chuẩn hóa phạm vi dữ liệu (Scaling & Normalization)
- **Thời lượng:** 1.5 phút

### Nội dung trên Slide
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

### Gợi ý bố cục & Trực quan
- Minh họa đồ thị đường đồng mức (contour plot) trước và sau Scaling cho Gradient Descent; kèm bảng so sánh 3 phương pháp.

### Lời thoại thuyết trình
> "Thao tác thứ hai là **Scaling và Normalization**, tức đưa các biến về cùng một thang đo hợp lý. Trong dữ liệu thực tế, cột `Tuổi` chỉ từ 18 đến 65, nhưng cột `Thu nhập` lên tới hàng chục triệu. Với các thuật toán dựa trên khoảng cách như KNN hay SVM, biến thu nhập sẽ chi phối hoàn toàn phép tính khoảng cách. Còn với mạng nơ-ron hay hồi quy dùng Gradient Descent, sự chênh lệch thang đo khiến đường đồng mức của hàm mất mát bị dẹt, làm quá trình tối ưu dao động zic-zac và hội tụ chậm. Riêng các mô hình dạng cây như Random Forest hay XGBoost không cần bước này vì chúng chỉ chia nhánh theo thứ tự giá trị.
>
> Có ba kỹ thuật chính: **Min-Max Scaling** đưa dữ liệu về đoạn `[0, 1]`, nhưng dễ bị co cụm nếu có giá trị ngoại lai. **Standardization (Z-score)** đưa phân phối về trung bình 0 và độ lệch chuẩn 1, ít bị ảnh hưởng bởi ngoại lai hơn. Ngoài ra, với các biến có phân phối lệch đuôi dài như giá nhà hay doanh thu, ta dùng **Log Transformation** để nén khoảng cách các giá trị lớn.
>
> Một nguyên tắc bắt buộc khi thực hiện: chỉ tính các tham số như min, max, trung bình và độ lệch chuẩn trên tập **Train**, sau đó dùng chính bộ tham số đó để biến đổi tập Test. Tuyệt đối không chuẩn hóa trên toàn bộ tập dữ liệu trước khi chia tách."

---

## Slide 8: Rời rạc hóa (Discretization) & Mã hóa biến phân loại (Encoding)
- **Thời lượng:** 2.0 phút

### Nội dung trên Slide
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

### Gợi ý bố cục & Trực quan
- Nửa trên minh họa biểu đồ biến đổi `Tuổi` liên tục thành 4 nhóm (bins). Nửa dưới trình bày bảng so sánh 4 phương pháp Encoding.

### Lời thoại thuyết trình
> "Thao tác thứ ba là **Rời rạc hóa (Discretization hay Binning)**, tức chia một biến liên tục như `Tuổi` hay `Thu nhập` thành các nhóm rời rạc. Kỹ thuật này giúp các mô hình tuyến tính đơn giản nắm bắt được mối quan hệ phi tuyến và giảm tác động của nhiễu. Tuy nhiên, cần lưu ý việc chia khoảng có thể làm mất thông tin chi tiết ở ranh giới giữa hai nhóm.
>
> Thao tác thứ tư là **Mã hóa biến phân loại (Categorical Encoding)** để đưa chữ viết về dạng số:
> - **Ordinal Encoding** gán số nguyên theo thứ tự, chỉ nên dùng khi biến thực sự có thứ tự tự nhiên như *Thấp, Trung bình, Cao*.
> - **One-Hot Encoding** chuyển mỗi danh mục thành một cột nhị phân, phù hợp khi số danh mục ít. Nếu có hàng nghìn thành phố hay mã sản phẩm, One-Hot sẽ gây bùng nổ số chiều.
> - Để xử lý nhiều danh mục, ta có thể dùng **Target Encoding** – thay tên danh mục bằng trung bình nhãn mục tiêu tương ứng, nhưng phải kết hợp làm mượt (smoothing) và tính trong cross-validation để tránh quá khớp.
> - Cuối cùng, với dữ liệu có số lượng danh mục khổng lồ như ID người dùng hoặc xuất hiện danh mục mới khi vận hành thực tế, **Hashing Trick** dùng hàm băm để nén toàn bộ vào một kích thước vector cố định. Cách này tiết kiệm bộ nhớ và không lo danh mục lạ, dù phải chấp nhận xác suất nhỏ xảy ra xung đột băm."

---

## Slide 9: Feature Crossing & Positional Embeddings
- **Thời lượng:** 1.5 phút

### Nội dung trên Slide
- **1. Feature Crossing (Tổ hợp chéo đặc trưng):**
  - Tạo đặc trưng mới bằng cách kết hợp 2 hay nhiều đặc trưng gốc (ví dụ: `Loai_xe` $\times$ `Khung_gio`, hoặc `Vi_do` $\times$ `Kinh_do`).
  - *Tác dụng:* Giúp mô hình tuyến tính học được sự tương tác phức tạp giữa các biến.
  - *Lưu ý:* Nếu hai biến đều có nhiều danh mục ($M \times N$), số chiều sẽ tăng rất nhanh $\rightarrow$ cần chọn lọc hoặc dùng kết hợp với Hashing/Regularization.
- **2. Positional Embeddings (Nhúng vị trí):**
  - *Bối cảnh:* Dữ liệu chuỗi (văn bản NLP, lịch sử click, chuỗi thời gian) phụ thuộc vào thứ tự xuất hiện.
  - *Vấn đề:* Kiến trúc xử lý song song như Transformer không tự phân biệt được thứ tự phần tử.
  - *Giải pháp:* Cộng thêm vector biểu diễn vị trí (dạng hàm Sin/Cos cố định hoặc vector học được) vào vector đặc trưng đầu vào để mô hình hiểu vị trí tương đối và tuyệt đối.

### Gợi ý bố cục & Trực quan
- Nửa trái: Ma trận giao giữa `Loại xe` và `Khung giờ` minh họa Feature Crossing. Nửa phải: Sơ đồ cộng `Token Embedding + Positional Embedding` trong chuỗi đầu vào.

### Lời thoại thuyết trình
> "Hai kỹ thuật còn lại trong nhóm thao tác phổ biến là **Feature Crossing** và **Positional Embeddings**.
>
> Với **Feature Crossing**, ta kết hợp hai hay nhiều biến rời rạc thành một biến tổ hợp mới, ví dụ kết hợp `Loại xe` và `Khung giờ`. Giá trị của một biến có thể phụ thuộc vào ngữ cảnh của biến kia: xe tải chạy vào giờ cao điểm sẽ có thời gian di chuyển khác hoàn toàn xe máy vào ban đêm. Tổ hợp này giúp các mô hình tuyến tính nắm bắt tương tác phi tuyến, nhưng cần thận trọng vì tổ hợp các biến nhiều danh mục sẽ làm tăng mạnh số chiều dữ liệu.
>
> Còn **Positional Embeddings** được dùng cho dữ liệu có thứ tự như văn bản hay chuỗi hành vi người dùng. Các mô hình hiện đại như Transformer xử lý các phần tử song song cùng lúc nên tự nó không biết từ nào đứng trước, từ nào đứng sau. Do đó, ta cộng thêm một vector mã hóa vị trí vào vector đặc trưng của từng phần tử, giúp mô hình nắm bắt chính xác cấu trúc tuần tự của chuỗi dữ liệu."

---

## Slide 10: Data Leakage – Vấn đề nghiêm trọng trong triển khai ML
- **Thời lượng:** 1.5 phút

### Nội dung trên Slide
- **Khái niệm:** **Data Leakage (Rò rỉ dữ liệu)** xảy ra khi thông tin từ nhãn mục tiêu hoặc từ tương lai vô tình xuất hiện trong dữ liệu đầu vào lúc huấn luyện, nhưng không có sẵn tại thời điểm dự đoán thực tế.
- **Hệ quả thực tế:**
  - Trên tập kiểm thử nội bộ (Offline): Độ chính xác rất cao (ví dụ 98–99%).
  - Khi đưa lên vận hành thật (Production): Mô hình sai lệch nghiêm trọng và không sử dụng được.
- **Ví dụ minh họa (Y tế):**
  - *Mục tiêu:* Dự đoán bệnh nhân có mắc bệnh nặng hay không khi mới tiếp nhận.
  - *Đặc trưng bị rò rỉ:* Cột `Ma_khoa_dieu_tri` hoặc `Ngay_nhap_vien`.
  - *Nguyên nhân:* Dữ liệu này chỉ được ghi nhận **sau khi** bác sĩ đã chẩn đoán bệnh xong.

### Gợi ý bố cục & Trực quan
- Sơ đồ trục thời gian (Timeline): Đánh dấu mốc **"Thời điểm dự đoán (Prediction Time)"**. Chỉ những dữ liệu sinh ra *trước* mốc này mới hợp lệ; dữ liệu *sau* mốc này là rò rỉ.

### Lời thoại thuyết trình
> "Chuyển sang phần thứ tư, chúng ta bàn về một vấn đề kỹ thuật thường gây thất bại cho các dự án Machine Learning khi đưa ra thực tế: **Data Leakage** (Rò rỉ dữ liệu).
>
> Data Leakage xảy ra khi thông tin từ tập kiểm thử hoặc từ tương lai vô tình lọt vào quá trình huấn luyện, trong khi tại thời điểm dự đoán thật, mô hình không hề có thông tin đó. Hệ quả là mô hình đạt điểm đánh giá nội bộ rất cao, có thể lên tới 98–99%, nhưng khi triển khai phục vụ người dùng thật thì hiệu năng sụt giảm nghiêm trọng.
>
> Hãy xét một ví dụ trong y tế: chúng ta cần xây dựng mô hình sàng lọc sớm để dự đoán bệnh nhân có mắc bệnh phổi hay không ngay lúc mới vào khám. Nếu trong dữ liệu bảng có cột `Chẩn_đoán_phòng_chụp_X_quang` hoặc `Mã_khoa_điều_trị`, mô hình sẽ đạt độ chính xác gần như tuyệt đối khi huấn luyện. Tuy nhiên, những thông tin này chỉ tồn tại **sau khi** bệnh nhân đã được bác sĩ kết luận bệnh. Khi áp dụng cho bệnh nhân mới đến quầy tiếp nhận, các cột này hoàn toàn trống, khiến mô hình thất bại trên thực tế."

---

## Slide 11: Các nguyên nhân gây Data Leakage phổ biến
- **Thời lượng:** 2.0 phút

### Nội dung trên Slide
5 nguyên nhân thường gặp theo Chip Huyen:
1. **Tiền xử lý trước khi chia tập (Splitting after Preprocessing):** `fit` Scaler, Imputer hoặc chọn đặc trưng trên toàn bộ dataset rồi mới chia Train/Test.
2. **Rò rỉ theo thời gian (Time Leakage):** Dùng chia ngẫu nhiên (Random Split) cho dữ liệu chuỗi thời gian (vd: giá chứng khoán, doanh số), khiến mô hình dùng dữ liệu ngày mai để dự đoán ngày hôm qua.
3. **Rò rỉ theo nhóm (Group Leakage):** Các mẫu của cùng một thực thể (cùng một người dùng, cùng một bệnh nhân) xuất hiện ở cả tập Train và tập Test.
4. **Dữ liệu trùng lặp (Duplicates / Near-duplicates):** Bản ghi bị lặp lại nằm rải rác ở cả hai tập.
5. **Biến đại diện cho nhãn (Target Proxy / Leaked Features from Generation):** Đặc trưng được tạo ra hoặc cập nhật từ chính kết quả của biến mục tiêu.

### Gợi ý bố cục & Trực quan
- Trình bày 5 khối (cards) kèm một dòng minh họa trực quan cho từng lỗi (đặc biệt minh họa Random Split vs. Time-based Split).

### Lời thoại thuyết trình
> "Theo tổng hợp của Chip Huyen, có 5 nguyên nhân chính dẫn đến Data Leakage mà kỹ sư cần đặc biệt lưu ý:
>
> Thứ nhất là **tiền xử lý trước khi chia tập dữ liệu**. Nếu bạn chuẩn hóa dữ liệu hoặc điền giá trị trung bình trên toàn bộ bảng dữ liệu trước khi gọi `train_test_split`, thông tin thống kê của tập Test đã gián tiếp tham gia vào tập Train.
>
> Thứ hai là **rò rỉ theo thời gian**. Với dữ liệu chuỗi thời gian như doanh số bán hàng hay giá cổ phiếu, việc chia ngẫu nhiên làm dữ liệu tương lai rơi vào tập Train để dự đoán các mốc thời gian trong quá khứ.
>
> Thứ ba là **rò rỉ theo nhóm**. Ví dụ mỗi bệnh nhân có 10 ảnh chụp X-quang; nếu chia ngẫu nhiên, một bệnh nhân có thể có ảnh ở cả Train và Test. Mô hình có thể học thuộc đặc điểm riêng của bệnh nhân đó thay vì học dấu hiệu bệnh lý.
>
> Thứ tư là **dữ liệu trùng lặp hoặc gần trùng lặp** không được lọc bỏ trước khi chia tập, khiến tập Test không còn đánh giá được khả năng tổng quát hóa.
>
> Thứ năm là **biến phụ thuộc vào nhãn (Target Proxy)** – các biến chỉ được sinh ra sau khi sự kiện mục tiêu đã ngã ngũ, như ví dụ mã khoa điều trị ở slide trước."

---

## Slide 12: Cách phát hiện và phòng chống Data Leakage
- **Thời lượng:** 1.5 phút

### Nội dung trên Slide
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

### Gợi ý bố cục & Trực quan
- Hai cột song song: Bên trái là "Phòng chống (4 quy tắc kỹ thuật)", bên phải là "Phát hiện (3 dấu hiệu cảnh báo)", kèm đoạn code minh họa ngắn 3 dòng của `Pipeline`.

### Lời thoại thuyết trình
> "Vậy làm thế nào để phòng chống và phát hiện Data Leakage?
>
> Về phòng chống, quy tắc đầu tiên là **luôn chia tập dữ liệu trước** mọi bước tiền xử lý. Trong mã nguồn, chúng ta nên đóng gói toàn bộ quy trình biến đổi và mô hình vào `Pipeline` (như `sklearn.pipeline.Pipeline`) để đảm bảo tính nhất quán và cô lập dữ liệu kiểm thử. Đồng thời, cần chọn cách chia phù hợp với cấu trúc bài toán: dùng **Temporal Split** cho chuỗi thời gian và **GroupKFold** khi dữ liệu gắn với từng cá nhân hay thực thể cụ thể. Ngoài ra, cần làm việc chặt chẽ với bộ phận thu thập dữ liệu để đảm bảo mọi đặc trưng đều sẵn sàng trước thời điểm dự đoán.
>
> Về phát hiện, dấu hiệu rõ ràng nhất là khi kết quả đánh giá đạt độ chính xác cao bất thường so với kỳ vọng thực tế, hoặc khi kiểm tra độ quan trọng đặc trưng thấy có một biến chiếm tỷ trọng áp đảo. Khi đó, ta nên thực hiện kiểm tra loại trừ từng biến đáng ngờ để xác định xem biến đó có đang vô tình mang thông tin tương lai hay không."

---

## Slide 13: Đánh giá đặc trưng – Feature Importance
- **Thời lượng:** 1.5 phút

### Nội dung trên Slide
- **Tại sao cần đo Feature Importance?**
  - Loại bỏ đặc trưng nhiễu hoặc dư thừa.
  - Giảm kích thước mô hình, giảm độ trễ khi dự đoán (inference latency) và tiết kiệm chi phí lưu trữ.
  - Tăng khả năng giải thích mô hình cho các bên liên quan.
- **Các phương pháp phổ biến:**
  1. **Built-in Tree Importance:** Tính toán nhanh dựa trên mức độ giảm tạp chất (Gini/Entropy), nhưng dễ thiên lệch về các biến có nhiều giá trị phân biệt (high cardinality).
  2. **Permutation Importance:** Xáo trộn ngẫu nhiên giá trị một cột trên tập Validation và đo mức giảm hiệu năng. Khách quan, áp dụng được cho mọi loại mô hình.
  3. **SHAP (SHapley Additive exPlanations):** Dựa trên lý thuyết trò chơi, định lượng đóng góp biên của từng đặc trưng ở cả cấp độ toàn cục (toàn bộ dữ liệu) và cục bộ (từng mẫu dự đoán cụ thể).

### Gợi ý bố cục & Trực quan
- Minh họa một biểu đồ thanh ngang (Bar chart / SHAP summary plot) xếp hạng mức độ ảnh hưởng của 5 đặc trưng mẫu.

### Lời thoại thuyết trình
> "Khi đã xây dựng được nhiều đặc trưng, không phải cứ giữ lại tất cả là tốt. Việc giữ các đặc trưng dư thừa hoặc nhiễu làm mô hình cồng kềnh, tăng độ trễ khi dự đoán và dễ gây quá khớp. Do đó, ta cần đánh giá **Feature Importance** – độ quan trọng của từng đặc trưng.
>
> Có ba phương pháp chính thường được sử dụng. Thứ nhất là độ quan trọng tích hợp sẵn trong mô hình cây, tuy nhanh nhưng thường bị thiên lệch về phía các biến có nhiều giá trị khác nhau. Thứ hai là **Permutation Importance**: ta xáo trộn ngẫu nhiên giá trị của một cột trên tập Validation; nếu hiệu năng mô hình giảm mạnh, chứng tỏ cột đó thực sự quan trọng. Cách này công bằng và áp dụng được cho mọi mô hình. Thứ ba là **SHAP**, dựa trên lý thuyết trò chơi, không chỉ cho biết biến nào quan trọng trên toàn tập dữ liệu mà còn giải thích được vì sao mô hình đưa ra dự đoán cho từng khách hàng cụ thể."

---

## Slide 14: Khả năng tổng quát hóa (Feature Generalization)
- **Thời lượng:** 1.5 phút

### Nội dung trên Slide
- **Mục tiêu:** Đặc trưng tốt không chỉ hoạt động tốt trên tập huấn luyện mà phải ổn định trên dữ liệu mới trong tương lai.
- **Hai tiêu chí cốt lõi theo Chip Huyen:**
  1. **Feature Coverage (Độ bao phủ):**
     - Tỷ lệ phần trăm mẫu dữ liệu có giá trị hợp lệ cho đặc trưng đó.
     - Đặc trưng có độ bao phủ quá thấp (chỉ xuất hiện ở vài trường hợp cá biệt) dễ gây overfitting và ít đóng góp chung.
  2. **Distribution Stability (Độ ổn định phân phối):**
     - Phân phối giá trị của đặc trưng cần ổn định theo thời gian.
     - Tránh các đặc trưng bị trôi dạt phân phối (distribution drift) quá nhanh khi hành vi người dùng hoặc bối cảnh thay đổi.
- **Sự đánh đổi (Trade-off):** Cần cân bằng giữa **độ đặc hiệu** (khớp sát dữ liệu hiện tại) và **tính tổng quát** (bền vững theo thời gian).

### Gợi ý bố cục & Trực quan
- Hai biểu đồ minh họa nhỏ: Biểu đồ 1 so sánh đặc trưng độ bao phủ cao vs. độ bao phủ thấp; Biểu đồ 2 minh họa sự dịch chuyển phân phối (Distribution Drift) giữa thời điểm $t_1$ và $t_2$.

### Lời thoại thuyết trình
> "Bên cạnh độ quan trọng, Chip Huyen nhấn mạnh một tiêu chí nữa để đánh giá đặc trưng tốt là **khả năng tổng quát hóa (Feature Generalization)**. Một đặc trưng chỉ giúp tăng điểm trên tập huấn luyện nhưng nhanh chóng mất tác dụng ngoài thực tế không phải là đặc trưng tốt.
>
> Có hai khía cạnh cần kiểm tra. Thứ nhất là **độ bao phủ (Feature Coverage)**, tức đặc trưng đó xuất hiện trên bao nhiêu phần trăm dữ liệu. Nếu một đặc trưng chỉ có giá trị ở dưới 1% mẫu dữ liệu, nó rất dễ khiến mô hình học vẹt các trường hợp cá biệt. Thứ hai là **độ ổn định phân phối (Distribution Stability)**. Nếu phân phối của đặc trưng thay đổi quá nhanh theo từng tuần hoặc từng tháng – hiện tượng trôi dạt phân phối – mô hình sẽ nhanh chóng suy giảm độ chính xác nếu không được huấn luyện lại liên tục. Do đó, kỹ sư luôn phải cân bằng giữa việc mô tả chi tiết dữ liệu hiện tại và khả năng thích ứng lâu dài của đặc trưng."

---

## Slide 15: Mở rộng thực tế – Feature Store & MLOps
- **Thời lượng:** 1.0 phút

### Nội dung trên Slide
- **Thách thức trong Production: Training-Serving Skew**
  - Logic tính đặc trưng khi huấn luyện (Offline: Python/Pandas) khác với khi phục vụ trực tuyến (Online: Java/Go/SQL).
  - Hệ quả: Sai lệch dữ liệu đầu vào giữa lúc học và lúc dự đoán thật.
- **Giải pháp: Feature Store (Kho lưu trữ đặc trưng)**
  - Công cụ tiêu biểu: **Feast, Hopsworks, Tecton**.
  - **Lợi ích chính:**
    1. Đồng nhất định nghĩa đặc trưng giữa **Offline Store** (training) và **Online Store** (inference độ trễ thấp).
    2. Tái sử dụng đặc trưng giữa nhiều mô hình và nhiều nhóm kỹ sư khác nhau.
    3. Hỗ trợ theo dõi phiên bản (versioning) và kiểm soát chất lượng đặc trưng.

### Gợi ý bố cục & Trực quan
- Sơ đồ kiến trúc đơn giản: `Nguồn dữ liệu thô` $\rightarrow$ `Feature Engineering Pipeline` $\rightarrow$ `Feature Store (Offline & Online)` $\rightarrow$ `Training & Serving`.

### Lời thoại thuyết trình
> "Để kết nối lý thuyết Chương 5 với kiến trúc hệ thống thực tế, nhóm xin giới thiệu ngắn gọn khái niệm **Feature Store**. Trong doanh nghiệp, một sự cố kỹ thuật rất hay gặp là **Training-Serving Skew**: đội ngũ khoa học dữ liệu viết công thức tính đặc trưng bằng Python lúc huấn luyện, nhưng đội ngũ kỹ sư hệ thống lại viết lại công thức đó bằng Java hoặc SQL lúc triển khai, dẫn đến sai số nhỏ làm mô hình dự đoán sai lệch.
>
> **Feature Store** – với các nền tảng phổ biến như Feast hay Hopsworks – ra đời để giải quyết vấn đề này. Nó đóng vai trò là kho lưu trữ và quản lý định nghĩa đặc trưng tập trung, đảm bảo đồng nhất giữa môi trường huấn luyện offline và môi trường phục vụ online độ trễ thấp, đồng thời cho phép các nhóm khác nhau tái sử dụng lại các đặc trưng đã được kiểm chứng."

---

## Slide 16: Tổng kết & Bài học rút ra
- **Thời lượng:** 1.0 phút

### Nội dung trên Slide
Ba nguyên tắc cốt lõi từ Chapter 5:
1. **Dữ liệu và Ngữ cảnh là nền tảng:** Kết hợp hợp lý giữa *Engineered Features* và *Learned Features*; chọn kỹ thuật xử lý (Missing, Scaling, Encoding) phù hợp với bản chất dữ liệu và thuật toán.
2. **Kiểm soát chặt chẽ Data Leakage:** Chia tập trước khi biến đổi, dùng `Pipeline`, áp dụng đúng *Temporal Split* hoặc *GroupKFold*, và kiểm tra tính khả dụng theo thời gian thực.
3. **Đơn giản, Đo lường và Tổng quát hóa:** Bắt đầu từ các đặc trưng đơn giản, đánh giá đóng góp bằng *Permutation Importance* hoặc *SHAP*, và ưu tiên các đặc trưng có độ bao phủ và độ ổn định cao.

### Gợi ý bố cục & Trực quan
- 3 thẻ tóm tắt lớn (3 Key Takeaways) rõ ràng, chữ to, dễ đọc từ xa.

### Lời thoại thuyết trình
> "Để khép lại phần trình bày, nhóm xin đúc kết ba thông điệp cốt lõi từ Chương 5:
>
> Thứ nhất, **dữ liệu và hiểu biết nghiệp vụ là nền tảng**. Hãy chọn kỹ thuật xử lý missing, scaling hay mã hóa dựa trên cơ chế sinh ra dữ liệu và yêu cầu của từng họ thuật toán, đồng thời biết kết hợp cả đặc trưng thủ công và đặc trưng tự học khi cần thiết.
>
> Thứ hai, **kiểm soát chặt chẽ rò rỉ dữ liệu (Data Leakage)**. Luôn chia tập trước khi biến đổi, sử dụng Pipeline và chia tách đúng theo thời gian hoặc theo nhóm đối tượng để kết quả đánh giá phản ánh đúng thực tế.
>
> Thứ ba, **ưu tiên tính đơn giản và khả năng tổng quát hóa**. Hãy bắt đầu từ một tập đặc trưng đơn giản, sử dụng các công cụ như Permutation Importance hay SHAP để loại bỏ nhiễu, và đảm bảo đặc trưng giữ được sự ổn định khi đưa vào vận hành lâu dài."

---

## Slide 17: Tài liệu tham khảo & Video minh họa
- **Thời lượng:** 0.5 phút

### Nội dung trên Slide
- **Sách & Tài liệu chính quy:**
  1. Chip Huyen (2022), *Designing Machine Learning Systems*, O'Reilly Media (Chapter 5, pp. 140–170).
  2. Alice Zheng & Amanda Casari (2018), *Feature Engineering for Machine Learning*, O'Reilly Media.
  3. Max Kuhn & Kjell Johnson (2019), *Feature Engineering and Selection*, CRC Press.
- **Khóa học & Video tham khảo:**
  1. **Stanford CS 329S (Chip Huyen):** *Feature Engineering & Data Pipeline in ML Systems*.
  2. **StatQuest with Josh Starmer:** *Data Leakage in Machine Learning*; *One-Hot Encoding and Categorical Data*.
  3. **Made With ML (Goku Mohandas):** *Feature Engineering & Feature Store in Production*.
  4. **Google Cloud Tech:** *Best Practices for ML: Feature Engineering at Scale*.

### Gợi ý bố cục & Trực quan
- Chia 2 cột gọn gàng: Sách tham khảo bên trái, Khóa học/Video bên phải (có thể kèm mã QR dẫn tới repository GitHub chứa ghi chú chi tiết của nhóm).

### Lời thoại thuyết trình
> "Trên đây là danh mục tài liệu tham khảo chính thức và các bài giảng video trực quan mà nhóm đã tổng hợp để phục vụ quá trình nghiên cứu chuyên đề này, bao gồm sách gốc của Chip Huyen, các đầu sách chuyên sâu về Feature Engineering của O'Reilly và chuỗi video giải thích trực quan từ Stanford CS 329S và StatQuest."

---

## Slide 18: Cảm ơn & Thảo luận (Q&A)
- **Thời lượng:** 0.5 phút (+ Thời gian hỏi đáp)

### Nội dung trên Slide
- **Lời cảm ơn:** Cảm ơn thầy/cô và các bạn đã lắng nghe!
- **Gợi ý câu hỏi thảo luận mở:**
  1. Khi nào nên ưu tiên *Target Encoding* thay cho *Hashing Trick* (và ngược lại)?
  2. Trong bài toán gợi ý sản phẩm theo thời gian thực, làm thế nào để tránh *Data Leakage* khi tạo đặc trưng hành vi người dùng?

### Gợi ý bố cục & Trực quan
- Thiết kế thoáng, nổi bật dòng chữ "Q&A – Trao đổi và Thảo luận" kèm thông tin liên hệ hoặc link repository tài liệu.

### Lời thoại thuyết trình
> "Phần trình bày của nhóm về Chương 5: Feature Engineering đến đây là kết thúc. Nhóm rất cảm ơn thầy/cô và các bạn đã chú ý lắng nghe. Sau đây, nhóm xin mời thầy/cô và các bạn đặt câu hỏi hoặc cùng thảo luận thêm về các nội dung vừa trình bày."

---

## Phụ lục: Gợi ý chuẩn bị câu trả lời cho phần Q&A (Dành cho người thuyết trình)

1. **Hỏi: Tại sao cây quyết định (XGBoost, Random Forest) không cần Scaling, còn Logistic Regression hay Neural Network lại cần?**
   - *Trả lời ngắn:* Cây quyết định tìm ngưỡng chia dựa trên thứ tự tương đối của giá trị (ví dụ: `Thu nhập > 20 triệu`), nên nhân hay chia cả cột cho một hằng số dương không làm thay đổi thứ tự này. Trong khi đó, các mô hình dùng Gradient Descent cập nhật trọng số tỷ lệ thuận với độ lớn giá trị đầu vào, nên thang đo không đồng đều sẽ làm quá trình tối ưu mất cân bằng và hội tụ chậm.
2. **Hỏi: Có nên dùng Target Encoding cho mọi biến phân loại nhiều danh mục không?**
   - *Trả lời ngắn:* Không nên dùng tùy tiện. Target Encoding dễ gây rò rỉ nhãn mục tiêu và overfitting nếu một danh mục chỉ xuất hiện vài lần. Khi dùng bắt buộc phải tính toán trên từng fold của Cross-Validation (out-of-fold) và áp dụng kỹ thuật làm mượt (smoothing) kéo giá trị về trung bình toàn cục. Nếu danh mục quá lớn và liên tục phát sinh giá trị mới lúc vận hành, Hashing Trick sẽ an toàn hơn.
3. **Hỏi: Làm sao phân biệt MAR và MNAR trong thực tế?**
   - *Trả lời ngắn:* Không có kiểm định thống kê thuần túy nào chứng minh chắc chắn hoàn toàn MNAR chỉ từ dữ liệu hiện có vì giá trị bị thiếu không quan sát được. Việc phân biệt chủ yếu dựa vào **kiến thức nghiệp vụ (domain knowledge)** và quy trình thu thập dữ liệu (ví dụ: hiểu tâm lý người thu nhập cao ngại khai báo, hoặc biết cảm biến hỏng ở dải nhiệt độ quá cao).
