# Lời thoại Thuyết trình Seminar
**Chủ đề:** Chapter 5: Feature Engineering (*Designing Machine Learning Systems* – Chip Huyen)  
**Thời lượng dự kiến:** ~20.0 phút thuyết trình + 5.0 phút hỏi đáp  

> Xem nội dung hiển thị tương ứng trên từng slide tại: [SLIDE_CONTENT.md](SLIDE_CONTENT.md)

---

## Bảng phân bổ thời lượng tổng quát

| Phần | Slides | Thời lượng dự kiến |
| :--- | :---: | :---: |
| Mở đầu & Đặt vấn đề | Slide 1 – 3 | 2.5 phút |
| Phần 1: Learned Features vs. Engineered Features | Slide 4 | 1.5 phút |
| Phần 2: Các kỹ thuật Feature Engineering cốt lõi | Slide 5 – 9 | 7.0 phút |
| Phần 3: Data Leakage (Rò rỉ dữ liệu) | Slide 10 – 12 | 5.0 phút |
| Phần 4: Engineering Good Features | Slide 13 – 14 | 3.0 phút |
| Tổng kết, Tài liệu tham khảo & Q&A | Slide 15 – 17 | 1.5 phút (+ Q&A) |
| **Tổng cộng** | **17 slides** | **~20.0 phút** |

---

## Slide 1: Giới thiệu đề tài *(0.5 phút)*
> "Chào thầy/cô và các bạn. Hôm nay mình xin chia sẻ về Chương 5: **Feature Engineering** trong cuốn *Designing Machine Learning Systems* của tác giả Chip Huyen. Bài trình bày của mình sẽ tập trung trực tiếp vào các khái niệm và kỹ thuật cốt lõi nhất của chương trong khoảng 20 phút."

---

## Slide 2: Mục tiêu & Nội dung chính *(0.5 phút)*
> "Bài hôm nay gồm 5 nội dung trọng tâm:
> 1. Bản chất và tầm quan trọng của Feature Engineering.
> 2. So sánh đặc trưng tự học và đặc trưng do con người thiết kế.
> 3. Các kỹ thuật xử lý đặc trưng hay dùng nhất trong thực tế.
> 4. Vấn đề rò rỉ dữ liệu (Data Leakage): nguyên nhân, cách phát hiện và phòng ngừa.
> 5. Cách đánh giá và nhận biết một đặc trưng tốt."

---

## Slide 3: Đặt vấn đề – Tầm quan trọng của Feature Engineering *(1.5 phút)*
> "Trước hết, **Feature Engineering là gì**? Đơn giản là quá trình biến đổi dữ liệu thô ban đầu thành các đặc trưng có ý nghĩa để mô hình học máy dễ học nhất.
>
> Tại sao bước này lại quyết định thành bại của mô hình? Vì nguyên lý cốt lõi: **Đầu vào là rác thì đầu ra cũng là rác**. Có hai lý do:
> 1. Mô hình chỉ tính toán được trên số, trong khi dữ liệu đời thực chứa đầy chữ, ngày giờ, ô trống hoặc thang đo lệch nhau.
> 2. Mô hình không tự hiểu nghiệp vụ thực tế.
>
> Chẳng hạn khi dự đoán thời gian chuyến xe công nghệ: nếu chỉ đưa tọa độ thô và giờ `'17:45'`, mô hình rất khó học. Nhưng nếu mình tính ra `khoảng cách = 8.5 km`, gắn cờ `giờ cao điểm = 1` và `trời mưa = 1`, mô hình sẽ nắm bắt quy luật nhanh và dự đoán chính xác hơn rất nhiều so với việc để mô hình tự tìm quy luật từ dữ liệu thô."

---

## Slide 4: Learned Features vs. Engineered Features *(1.5 phút)*
> "Hiện nay Deep Learning có thể tự học đặc trưng, vậy việc tự thiết kế đặc trưng thủ công có còn cần thiết? Câu trả lời là **rất cần**, vì mỗi loại phục vụ một dạng dữ liệu riêng:
>
> - **Learned Features** do mạng nơ-ron tự trích xuất, rất hiệu quả trên dữ liệu phi cấu trúc như ảnh, video, văn bản; nhưng cần lượng dữ liệu lớn, tốn GPU và là dạng hộp đen khó giải thích.
> - **Engineered Features** do con người thiết kế dựa trên hiểu biết nghiệp vụ, lại là lựa chọn hàng đầu cho dữ liệu dạng bảng – loại dữ liệu phổ biến nhất trong tài chính, ngân hàng và bán lẻ. Nó nhẹ, cần ít dữ liệu hơn và giải thích được rõ ràng.
>
> Trong thực tế, các hệ thống lớn thường kết hợp cả hai. Chẳng hạn như YouTube, để quyết định video nào được hiện lên đầu trang chủ cho người dùng xem: họ vừa dùng vector nhúng tự học để hiểu nội dung video, vừa dùng các đặc trưng thủ công như lịch sử click gần đây để sắp xếp thứ tự hiển thị."

---

## Slide 5: Tổng quan các kỹ thuật xử lý đặc trưng *(0.5 phút)*
> "Về mặt kỹ thuật, Chip Huyen đúc kết 6 phương pháp xử lý đặc trưng phổ biến nhất: xử lý dữ liệu khuyết thiếu, chuẩn hóa thang đo, rời rạc hóa, mã hóa biến phân loại, tổ hợp đặc trưng và nhúng vị trí. Bây giờ mình sẽ đi lần lượt qua bản chất và điểm cần lưu ý của từng phương pháp."

---

## Slide 6: Xử lý dữ liệu khuyết thiếu *(1.5 phút)*
> "Đầu tiên là xử lý dữ liệu khuyết thiếu. Khi thấy dữ liệu bị khuyết ô `NaN`, đừng vội xóa dòng hay điền tùy tiện giá trị trung bình. Ta cần xác định lý do dữ liệu bị thiếu:
> - **MCAR:** Thiếu ngẫu nhiên hoàn toàn do lỗi kỹ thuật (như rớt mạng mất log).
> - **MAR:** Thiếu phụ thuộc vào một cột khác đã biết (như phụ nữ ít điền cân nặng hơn nam).
> - **MNAR:** Thiếu vì chính giá trị đó (như người thu nhập rất cao từ chối khai báo lương).
>
> Với MNAR, nếu xóa dòng đi, ta sẽ làm mất hẳn nhóm đối tượng đặc biệt này. Nếu chỉ điền số trung bình thì làm sai lệch thông tin. Vì vậy, ngoài việc điền Median hoặc KNN, một kỹ thuật rất hiệu quả là tạo thêm cột cờ **Missing Indicator**, ví dụ `income_is_missing = 1`. Cột cờ này biến chính trạng thái không khai báo thành một tín hiệu hữu ích cho mô hình học."

---

## Slide 7: Chuẩn hóa phạm vi dữ liệu (Scaling) *(1.5 phút)*
> "Bước tiếp theo là Chuẩn hóa thang đo (Scaling), tức đưa các biến số về cùng một khoảng giá trị hợp lý. Trong một bảng, `Tuổi` từ 18–60 nhưng `Thu nhập` lên tới hàng chục triệu.
>
> - Các mô hình tính khoảng cách như KNN, SVM, hay mạng nơ-ron dùng Gradient Descent **bắt buộc phải chuẩn hóa** để thuật toán không bị biến số lớn chi phối và hội tụ nhanh hơn.
> - Ngược lại, các mô hình dạng cây như Random Forest hay XGBoost **không cần bước này**, vì chúng chỉ phân nhánh theo thứ tự lớn nhỏ.
>
> Ba cách hay dùng: **Min-Max Scaling** ép về `[0, 1]`, nhưng nhạy với ngoại lai; **Standardization (Z-score)** đưa về trung bình 0, độ lệch chuẩn 1, ổn định hơn; và **Log Transformation** để nén dữ liệu phân phối lệch đuôi dài như doanh thu.
>
> Nguyên tắc bắt buộc: **Chỉ fit scaler trên tập Train**, sau đó áp nguyên bộ tham số đó sang tập Test để tránh rò rỉ dữ liệu."

---

## Slide 8: Rời rạc hóa & Mã hóa biến phân loại *(2.0 phút)*
> "Kỹ thuật thứ ba là **Rời rạc hóa (Binning)**, tức chia biến liên tục thành từng khoảng nhỏ, giúp các mô hình tuyến tính học được quan hệ phi tuyến và giảm bớt ảnh hưởng của ngoại lai.
>
> Tiếp theo là **Mã hóa biến phân loại (Encoding)** để chuyển dạng chữ thành số cho mô hình học:
> - **Ordinal Encoding:** Gán số thứ tự $0, 1, 2...$, chỉ dùng khi biến thực sự có thứ tự tự nhiên (*Thấp, Trung bình, Cao*).
> - **One-Hot Encoding:** Tạo cột nhị phân `0/1`, tốt khi có ít danh mục. Nếu có hàng nghìn mã sản phẩm, One-Hot sẽ làm bùng nổ số chiều.
> - **Target Encoding:** Thay danh mục bằng trung bình của nhãn mục tiêu, rất mạnh cho dữ liệu dạng bảng nhiều danh mục, nhưng cần kỹ thuật làm mượt (smoothing) để tránh quá khớp.
> - **Hashing Trick:** Dùng hàm băm nén toàn bộ danh mục vào một vector kích thước cố định. Cách này tiết kiệm bộ nhớ và xử lý được cả những danh mục mới xuất hiện lúc chạy thực tế."

---

## Slide 9: Feature Crossing & Positional Embeddings *(1.5 phút)*
> "Hai kỹ thuật tiếp theo là:
>
> **Feature Crossing** là ghép hai hoặc nhiều biến lại thành biến mới, ví dụ ghép `Loại xe` với `Khung giờ`. Cách này giúp mô hình tuyến tính nắm bắt được tương tác phức tạp giữa các thuộc tính mà không cần chuyển sang mô hình phi tuyến nặng nề. Nhưng lưu ý: tránh ghép các cột có quá nhiều giá trị vì sẽ làm số chiều tăng vọt.
>
> Còn **Positional Embeddings** dành cho dữ liệu dạng chuỗi như chữ viết hay chuỗi hành vi người dùng. Các mô hình như Transformer xử lý song song nên không tự biết từ nào đứng trước, từ nào đứng sau. Ta cộng thêm một vector mã hóa vị trí vào đặc trưng đầu vào để mô hình nắm bắt được thứ tự tuần tự của chuỗi."

---

## Slide 10: Data Leakage – Khái niệm & Tác hại *(1.5 phút)*
> "Tiếp theo là vấn đề kỹ thuật nghiêm trọng nhất trong triển khai học máy: **Data Leakage** (Rò rỉ dữ liệu).
>
> Data Leakage xảy ra khi thông tin từ tương lai hoặc từ nhãn mục tiêu vô tình lọt vào dữ liệu huấn luyện, trong khi lúc triển khai thực tế thì mô hình không hề có thông tin đó. Hậu quả là điểm đánh giá lúc thử nghiệm rất cao, 98–99%, nhưng khi chạy thực tế thì hiệu năng mô hình suy giảm nghiêm trọng.
>
> Ví dụ trong y tế: dự đoán bệnh nhân có chuyển nặng hay không lúc mới vào viện. Nếu trong dữ liệu có cột `Mã_khoa_điều_trị` hoặc `Ngày_nhập_viện`, mô hình sẽ đạt điểm gần như 100%. Nhưng thực tế, bệnh nhân phải khám xong thì mới có mã khoa điều trị. Lúc bệnh nhân mới đến quầy tiếp nhận, cột này hoàn toàn chưa có, khiến mô hình vô dụng."

---

## Slide 11: 5 Nguyên nhân gây Data Leakage phổ biến *(1.5 phút)*
> "5 nguyên nhân gây rò rỉ dữ liệu phổ biến nhất gồm:
> 1. **Tiền xử lý trước khi chia tập:** Tính min, max, mean trên cả bảng dữ liệu rồi mới chia Train/Test.
> 2. **Rò rỉ theo thời gian:** Dùng chia ngẫu nhiên (Random Split) cho dữ liệu chuỗi thời gian, khiến mô hình lấy dữ liệu ngày mai để đoán ngày hôm qua.
> 3. **Rò rỉ theo nhóm:** Dữ liệu của cùng một người dùng hay bệnh nhân xuất hiện ở cả Train và Test, làm mô hình ghi nhớ quá mức đặc điểm cá nhân.
> 4. **Dữ liệu trùng lặp:** Bản ghi trùng lặp nằm ở cả hai tập do không lọc sạch trước.
> 5. **Biến đại diện cho nhãn (Target Proxy):** Cột đặc trưng phản ánh kết quả sau khi sự kiện mục tiêu đã xảy ra."

---

## Slide 12: Phòng chống & Phát hiện Data Leakage *(1.5 phút)*
> "Để phòng chống và nhận diện Data Leakage:
>
> - **Phòng ngừa:**
>   - Luôn chia tập Train/Test **trước tiên**, không chạm vào dữ liệu trước khi chia.
>   - Đóng gói toàn bộ bước xử lý bằng `Pipeline` của Scikit-learn để đảm bảo chỉ fit trên phần Train.
>   - Dùng **Temporal Split** cho bài toán thời gian và **GroupKFold** cho dữ liệu theo nhóm đối tượng.
>   - Kiểm tra kỹ xem tại thời điểm dự đoán thật, cột đó đã tồn tại hay chưa.
> - **Dấu hiệu nhận diện:**
>   - Khi thấy độ chính xác cao bất thường so với thực tế.
>   - Hoặc khi thấy một cột có độ quan trọng (Feature Importance) cao vượt trội bất thường, cần nghi ngờ và kiểm tra lại nguồn gốc của cột đó ngay."

---

## Slide 13: Đánh giá đặc trưng – Feature Importance *(1.5 phút)*
> "Không phải cứ tạo thật nhiều đặc trưng là tốt. Đặc trưng thừa làm mô hình chậm, nặng và dễ quá khớp. Vì vậy ta cần đo **Feature Importance**:
>
> 1. **Built-in Tree Importance:** Đo mức giảm tạp chất trong cây quyết định, tính rất nhanh nhưng hay thiên lệch về cột có nhiều giá trị.
> 2. **Permutation Importance:** Xáo trộn ngẫu nhiên từng cột trên tập Validation rồi đo mức sụt giảm hiệu năng. Cột nào xáo trộn mà làm hiệu năng tụt mạnh thì cột đó thực sự quan trọng. Cách này khách quan và dùng cho mọi mô hình.
> 3. **SHAP:** Dựa trên lý thuyết trò chơi, đo chính xác mức độ đóng góp của từng đặc trưng trên cả toàn tập dữ liệu lẫn từng mẫu dự đoán cụ thể."

---

## Slide 14: Tính tổng quát hóa (Feature Generalization) *(1.5 phút)*
> "Bên cạnh độ quan trọng, một đặc trưng tốt bắt buộc phải có **khả năng tổng quát hóa** ngoài thực tế:
>
> - **Feature Coverage (Độ bao phủ):** Đặc trưng xuất hiện trên bao nhiêu phần trăm mẫu dữ liệu. Nếu chỉ xuất hiện ở dưới 1% trường hợp cá biệt, mô hình rất dễ bị quá khớp (overfitting).
> - **Distribution Stability (Độ ổn định phân phối):** Phân phối của đặc trưng không được biến động quá nhanh theo thời gian (tránh Data Drift).
>
> Bài học rút ra là: luôn cân bằng giữa việc mô tả thật chi tiết dữ liệu hiện tại và giữ cho đặc trưng ổn định lâu dài trên dữ liệu tương lai."

---

## Slide 15: Tổng kết – 3 Bài học cốt lõi *(1.0 phút)*
> "Đúc kết lại toàn bộ Chương 5, mình muốn nhấn mạnh 3 thông điệp chính:
>
> 1. **Hiểu dữ liệu là nền tảng:** Chọn kỹ thuật xử lý (missing, scaling, encoding) dựa trên cơ chế sinh dữ liệu và mô hình sử dụng; biết kết hợp cả đặc trưng thủ công và tự học.
> 2. **Kiểm soát chặt chẽ Data Leakage:** Luôn chia tập trước khi tiền xử lý, dùng Pipeline và chia tập đúng theo thời gian hoặc nhóm thực thể.
> 3. **Đơn giản và bền vững:** Dùng Permutation Importance hoặc SHAP để loại bỏ đặc trưng nhiễu, ưu tiên các đặc trưng có độ bao phủ và tính ổn định cao."

---

## Slide 16: Tài liệu tham khảo *(0.5 phút)*
> "Đây là các tài liệu chính mình sử dụng để nghiên cứu đề tài này, gồm giáo trình của Chip Huyen, sách Feature Engineering của O'Reilly, cùng các bài giảng trực quan từ Stanford và StatQuest."

---

## Slide 17: Cảm ơn & Q&A *(Hỏi đáp thảo luận)*
> "Phần trình bày của mình về Chương 5: Feature Engineering đến đây là kết thúc. Cảm ơn thầy/cô và các bạn đã chú ý theo dõi. Rất mong nhận được câu hỏi và đóng góp ý kiến từ mọi người."

---

## Phụ lục: Gợi ý trả lời nhanh cho phần Q&A

1. **Hỏi: Tại sao mô hình cây (XGBoost, Random Forest) không cần Scaling?**
   - *Trả lời:* Mô hình cây chỉ tìm ngưỡng phân nhánh dựa trên thứ tự lớn nhỏ (ví dụ: `Thu nhập > 20 triệu`). Nhân hay chia cả cột cho một số dương không làm thay đổi thứ tự này. Ngược lại, Gradient Descent cập nhật trọng số phụ thuộc trực tiếp vào độ lớn đầu vào, nên thang đo lệch nhau sẽ làm mô hình tối ưu mất cân bằng và hội tụ chậm.
2. **Hỏi: Khi nào nên dùng Target Encoding thay cho Hashing Trick?**
   - *Trả lời:* Dùng **Target Encoding** khi biến có nhiều danh mục nhưng tập danh mục đã tương đối cố định và cần giữ mối liên hệ chặt chẽ với nhãn mục tiêu (nhớ phải tính trong Cross-Validation để tránh rò rỉ). Dùng **Hashing Trick** khi số lượng danh mục quá khổng lồ (hàng triệu ID) hoặc lúc chạy thực tế liên tục phát sinh các giá trị mới chưa từng thấy trong tập huấn luyện.
3. **Hỏi: Làm sao biết dữ liệu bị thiếu là MNAR trong thực tế?**
   - *Trả lời:* Dữ liệu hiện có không thể tự chứng minh 100% MNAR vì phần giá trị thiếu không quan sát được. Nhận biết MNAR chủ yếu dựa vào **kiến thức nghiệp vụ và quy trình thu thập dữ liệu** (ví dụ: tâm lý người thu nhập cao ngại khai báo, hoặc cảm biến tự ngắt khi nhiệt độ vượt ngưỡng cho phép).
