# Lời thoại Thuyết trình Seminar
**Chủ đề:** Chapter 5: Feature Engineering (*Designing Machine Learning Systems* – Chip Huyen)  
**Tổng thời lượng dự kiến:** 22.0 phút (nằm trong khung chuẩn 20 – 25 phút, dư 3 phút cho phần Hỏi đáp)  

> Xem nội dung hiển thị trên từng slide tại: [SLIDE_CONTENT.md](SLIDE_CONTENT.md)

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

## Slide 1: Trang bìa *(0.5 phút)*
> "Kính chào thầy/cô và các bạn. Hôm nay, nhóm chúng em xin trình bày chuyên đề Seminar Chương 5: **Feature Engineering** (Kỹ thuật trích xuất và tạo đặc trưng), dựa trên cuốn sách *Designing Machine Learning Systems* của tác giả Chip Huyen. Bài trình bày của nhóm dự kiến kéo dài khoảng 22 phút."

---

## Slide 2: Tổng quan nội dung *(0.5 phút)*
> "Nội dung trình bày hôm nay được chia thành năm phần chính. Đầu tiên, chúng ta sẽ tìm hiểu vai trò của Feature Engineering. Tiếp theo là phần so sánh giữa đặc trưng tự học và đặc trưng thiết kế thủ công. Phần thứ ba đi sâu vào các thao tác xử lý đặc trưng thường gặp. Phần thứ tư tập trung vào một vấn đề rất phổ biến trong thực tế là rò rỉ dữ liệu (Data Leakage). Cuối cùng là các tiêu chí đánh giá một đặc trưng tốt và cách quản lý đặc trưng trong hệ thống sản xuất."

---

## Slide 3: Đặt vấn đề – Tầm quan trọng của Feature Engineering *(1.5 phút)*
> "Trước hết, chúng ta cần hiểu **Feature Engineering là gì**. Nói một cách trực diện, mỗi cột thông tin mô tả đối tượng trong bảng dữ liệu là một đặc trưng (feature). Feature Engineering là quá trình biến đổi dữ liệu thô ban đầu thành các đặc trưng có ý nghĩa, giúp mô hình học máy nắm bắt quy luật nhanh và chính xác hơn.
>
> Giáo sư Andrew Ng từng nhận xét: *'Học máy ứng dụng về bản chất chính là kỹ thuật tạo đặc trưng.'* Nguyên lý rất đơn giản: **Đầu vào là rác thì đầu ra cũng là rác**. Dù mô hình có phức tạp đến đâu nhưng thiếu đặc trưng phù hợp thì hiệu năng vẫn kém. Có hai lý do chính cho điều này: Thứ nhất, mô hình học máy chỉ xử lý được dữ liệu dạng số, trong khi dữ liệu thực tế chứa đầy văn bản, ngày tháng hay ô bị bỏ trống. Thứ hai, thuật toán không có trực giác đời thực.
>
> Ví dụ, khi dự đoán thời gian di chuyển của một chuyến xe công nghệ, nếu chỉ đưa tọa độ thô và chuỗi thời gian `'17:45'`, mô hình rất khó tự suy luận. Thay vào đó, kỹ sư tính ra `khoang_cach = 8.5 km`, gắn cờ `is_rush_hour = 1` và `is_rain = 1`. Việc đưa đúng ngữ cảnh thực tế như vậy giúp mô hình dự đoán chính xác hơn rất nhiều. Thực tế tại Facebook năm 2014 cũng cho thấy, trong bài toán dự đoán click quảng cáo, việc bổ sung đúng đặc trưng mang lại bước tiến lớn hơn hẳn so với việc mất nhiều tuần tinh chỉnh thuật toán."

---

## Slide 4: Learned Features vs. Engineered Features *(1.5 phút)*
> "Một câu hỏi thường gặp là: ngày nay Deep Learning đã tự học được đặc trưng, vậy kỹ thuật tạo đặc trưng thủ công có còn cần thiết không? Câu trả lời là **có**, vì hai phương pháp này phục vụ những loại dữ liệu khác nhau.
>
> **Learned Features** sử dụng mạng nơ-ron để tự trích xuất biểu diễn, rất hiệu quả trên dữ liệu phi cấu trúc như hình ảnh, âm thanh hay văn bản, nhưng đòi hỏi lượng dữ liệu lớn, chi phí tính toán cao và khó giải thích kết quả. Ngược lại, **Engineered Features** lại phát huy ưu thế trên dữ liệu dạng bảng – vốn là dạng dữ liệu phổ biến nhất trong tài chính, ngân hàng và thương mại điện tử. Phương pháp này cần ít dữ liệu hơn, mô hình nhẹ và giải thích được rõ ràng cho bộ phận nghiệp vụ.
>
> Do đó, trong các hệ thống sản xuất thực tế như YouTube hay Shopee, người ta kết hợp cả hai: dùng Learned Features để biểu diễn nội dung video hoặc sản phẩm dưới dạng vector, đồng thời dùng Engineered Features để nắm bắt hành vi người dùng theo thời gian thực, rồi đưa cả hai nhóm vào mô hình xếp hạng cuối cùng."

---

## Slide 5: Common Feature Engineering Operations – Tổng quan *(0.5 phút)*
> "Tiếp theo, chúng ta bước vào phần trọng tâm kỹ thuật: 6 thao tác xử lý đặc trưng phổ biến nhất theo tổng hợp của Chip Huyen, bao gồm: xử lý dữ liệu khuyết thiếu, chuẩn hóa phạm vi, rời rạc hóa, mã hóa biến phân loại, tổ hợp đặc trưng và nhúng vị trí. Chúng ta sẽ đi lần lượt vào bản chất và lưu ý thực tế của từng kỹ thuật."

---

## Slide 6: Xử lý dữ liệu khuyết thiếu *(1.5 phút)*
> "Thao tác đầu tiên là xử lý dữ liệu khuyết thiếu. Khi gặp các ô trống `NaN`, sai lầm phổ biến là vội vàng xóa dòng hoặc điền ngay giá trị trung bình. Trước khi xử lý, ta cần xác định cơ chế khiến dữ liệu bị thiếu.
>
> Có ba cơ chế: **MCAR** là thiếu ngẫu nhiên do sự cố kỹ thuật; **MAR** là thiếu có liên quan đến một cột thông tin khác đã biết; và quan trọng nhất là **MNAR** – thiếu do chính bản thân giá trị đó, ví dụ người có thu nhập rất cao thường từ chối khai báo lương trong khảo sát. Nếu xóa bỏ các dòng MNAR, ta sẽ làm mất hẳn nhóm đối tượng này. Còn nếu chỉ điền giá trị trung bình, ta sẽ làm sai lệch bản chất dữ liệu.
>
> Vì vậy, ngoài các cách điền khuyết như Median hay mô hình KNN, giải pháp rất hiệu quả trong thực tế là dùng **Missing Indicator**: tạo thêm một cột cờ nhị phân như `income_is_missing = 1`. Cột cờ này giúp mô hình nhận biết hành vi không khai báo như một đặc trưng riêng biệt, bảo toàn trọn vẹn thông tin hữu ích."

---

## Slide 7: Chuẩn hóa phạm vi dữ liệu *(1.5 phút)*
> "Thao tác thứ hai là **Scaling và Normalization**, tức đưa các biến về cùng một thang đo hợp lý. Trong dữ liệu thực tế, cột `Tuổi` chỉ từ 18 đến 65, nhưng cột `Thu nhập` lên tới hàng chục triệu. Với các thuật toán dựa trên khoảng cách như KNN hay SVM, biến thu nhập sẽ chi phối hoàn toàn phép tính khoảng cách. Còn với mạng nơ-ron hay hồi quy dùng Gradient Descent, sự chênh lệch thang đo khiến đường đồng mức của hàm mất mát bị dẹt, làm quá trình tối ưu dao động zic-zac và hội tụ chậm. Riêng các mô hình dạng cây như Random Forest hay XGBoost không cần bước này vì chúng chỉ chia nhánh theo thứ tự giá trị.
>
> Có ba kỹ thuật chính: **Min-Max Scaling** đưa dữ liệu về đoạn `[0, 1]`, nhưng dễ bị co cụm nếu có giá trị ngoại lai. **Standardization (Z-score)** đưa phân phối về trung bình 0 và độ lệch chuẩn 1, ít bị ảnh hưởng bởi ngoại lai hơn. Ngoài ra, với các biến có phân phối lệch đuôi dài như giá nhà hay doanh thu, ta dùng **Log Transformation** để nén khoảng cách các giá trị lớn.
>
> Một nguyên tắc bắt buộc khi thực hiện: chỉ tính các tham số như min, max, trung bình và độ lệch chuẩn trên tập **Train**, sau đó dùng chính bộ tham số đó để biến đổi tập Test. Tuyệt đối không chuẩn hóa trên toàn bộ tập dữ liệu trước khi chia tách."

---

## Slide 8: Rời rạc hóa & Mã hóa biến phân loại *(2.0 phút)*
> "Thao tác thứ ba là **Rời rạc hóa (Discretization hay Binning)**, tức chia một biến liên tục như `Tuổi` hay `Thu nhập` thành các nhóm rời rạc. Kỹ thuật này giúp các mô hình tuyến tính đơn giản nắm bắt được mối quan hệ phi tuyến và giảm tác động của nhiễu. Tuy nhiên, cần lưu ý việc chia khoảng có thể làm mất thông tin chi tiết ở ranh giới giữa hai nhóm.
>
> Thao tác thứ tư là **Mã hóa biến phân loại (Categorical Encoding)** để đưa chữ viết về dạng số:
> - **Ordinal Encoding** gán số nguyên theo thứ tự, chỉ nên dùng khi biến thực sự có thứ tự tự nhiên như *Thấp, Trung bình, Cao*.
> - **One-Hot Encoding** chuyển mỗi danh mục thành một cột nhị phân, phù hợp khi số danh mục ít. Nếu có hàng nghìn thành phố hay mã sản phẩm, One-Hot sẽ gây bùng nổ số chiều.
> - Để xử lý nhiều danh mục, ta có thể dùng **Target Encoding** – thay tên danh mục bằng trung bình nhãn mục tiêu tương ứng, nhưng phải kết hợp làm mượt (smoothing) và tính trong cross-validation để tránh quá khớp.
> - Cuối cùng, với dữ liệu có số lượng danh mục khổng lồ như ID người dùng hoặc xuất hiện danh mục mới khi vận hành thực tế, **Hashing Trick** dùng hàm băm để nén toàn bộ vào một kích thước vector cố định. Cách này tiết kiệm bộ nhớ và không lo danh mục lạ, dù phải chấp nhận xác suất nhỏ xảy ra xung đột băm."

---

## Slide 9: Feature Crossing & Positional Embeddings *(1.5 phút)*
> "Hai kỹ thuật còn lại trong nhóm thao tác phổ biến là **Feature Crossing** và **Positional Embeddings**.
>
> Với **Feature Crossing**, ta kết hợp hai hay nhiều biến rời rạc thành một biến tổ hợp mới, ví dụ kết hợp `Loại xe` và `Khung giờ`. Giá trị của một biến có thể phụ thuộc vào ngữ cảnh của biến kia: xe tải chạy vào giờ cao điểm sẽ có thời gian di chuyển khác hoàn toàn xe máy vào ban đêm. Tổ hợp này giúp các mô hình tuyến tính nắm bắt tương tác phi tuyến, nhưng cần thận trọng vì tổ hợp các biến nhiều danh mục sẽ làm tăng mạnh số chiều dữ liệu.
>
> Còn **Positional Embeddings** được dùng cho dữ liệu có thứ tự như văn bản hay chuỗi hành vi người dùng. Các mô hình hiện đại như Transformer xử lý các phần tử song song cùng lúc nên tự nó không biết từ nào đứng trước, từ nào đứng sau. Do đó, ta cộng thêm một vector mã hóa vị trí vào vector đặc trưng của từng phần tử, giúp mô hình nắm bắt chính xác cấu trúc tuần tự của chuỗi dữ liệu."

---

## Slide 10: Data Leakage – Khái niệm & Hệ quả *(1.5 phút)*
> "Chuyển sang phần thứ tư, chúng ta bàn về một vấn đề kỹ thuật thường gây thất bại cho các dự án Machine Learning khi đưa ra thực tế: **Data Leakage** (Rò rỉ dữ liệu).
>
> Data Leakage xảy ra khi thông tin từ tập kiểm thử hoặc từ tương lai vô tình lọt vào quá trình huấn luyện, trong khi tại thời điểm dự đoán thật, mô hình không hề có thông tin đó. Hệ quả là mô hình đạt điểm đánh giá nội bộ rất cao, có thể lên tới 98–99%, nhưng khi triển khai phục vụ người dùng thật thì hiệu năng sụt giảm nghiêm trọng.
>
> Hãy xét một ví dụ trong y tế: chúng ta cần xây dựng mô hình sàng lọc sớm để dự đoán bệnh nhân có mắc bệnh phổi hay không ngay lúc mới vào khám. Nếu trong dữ liệu bảng có cột `Chẩn_đoán_phòng_chụp_X_quang` hoặc `Mã_khoa_điều_trị`, mô hình sẽ đạt độ chính xác gần như tuyệt đối khi huấn luyện. Tuy nhiên, những thông tin này chỉ tồn tại **sau khi** bệnh nhân đã được bác sĩ kết luận bệnh. Khi áp dụng cho bệnh nhân mới đến quầy tiếp nhận, các cột này hoàn toàn trống, khiến mô hình thất bại trên thực tế."

---

## Slide 11: Các nguyên nhân gây Data Leakage phổ biến *(2.0 phút)*
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

## Slide 12: Cách phát hiện và phòng chống Data Leakage *(1.5 phút)*
> "Vậy làm thế nào để phòng chống và phát hiện Data Leakage?
>
> Về phòng chống, quy tắc đầu tiên là **luôn chia tập dữ liệu trước** mọi bước tiền xử lý. Trong mã nguồn, chúng ta nên đóng gói toàn bộ quy trình biến đổi và mô hình vào `Pipeline` (như `sklearn.pipeline.Pipeline`) để đảm bảo tính nhất quán và cô lập dữ liệu kiểm thử. Đồng thời, cần chọn cách chia phù hợp với cấu trúc bài toán: dùng **Temporal Split** cho chuỗi thời gian và **GroupKFold** khi dữ liệu gắn với từng cá nhân hay thực thể cụ thể. Ngoài ra, cần làm việc chặt chẽ với bộ phận thu thập dữ liệu để đảm bảo mọi đặc trưng đều sẵn sàng trước thời điểm dự đoán.
>
> Về phát hiện, dấu hiệu rõ ràng nhất là khi kết quả đánh giá đạt độ chính xác cao bất thường so với kỳ vọng thực tế, hoặc khi kiểm tra độ quan trọng đặc trưng thấy có một biến chiếm tỷ trọng áp đảo. Khi đó, ta nên thực hiện kiểm tra loại trừ từng biến đáng ngờ để xác định xem biến đó có đang vô tình mang thông tin tương lai hay không."

---

## Slide 13: Đánh giá đặc trưng – Feature Importance *(1.5 phút)*
> "Khi đã xây dựng được nhiều đặc trưng, không phải cứ giữ lại tất cả là tốt. Việc giữ các đặc trưng dư thừa hoặc nhiễu làm mô hình cồng kềnh, tăng độ trễ khi dự đoán và dễ gây quá khớp. Do đó, ta cần đánh giá **Feature Importance** – độ quan trọng của từng đặc trưng.
>
> Có ba phương pháp chính thường được sử dụng. Thứ nhất là độ quan trọng tích hợp sẵn trong mô hình cây, tuy nhanh nhưng thường bị thiên lệch về phía các biến có nhiều giá trị khác nhau. Thứ hai là **Permutation Importance**: ta xáo trộn ngẫu nhiên giá trị của một cột trên tập Validation; nếu hiệu năng mô hình giảm mạnh, chứng tỏ cột đó thực sự quan trọng. Cách này công bằng và áp dụng được cho mọi mô hình. Thứ ba là **SHAP**, dựa trên lý thuyết trò chơi, không chỉ cho biết biến nào quan trọng trên toàn tập dữ liệu mà còn giải thích được vì sao mô hình đưa ra dự đoán cho từng khách hàng cụ thể."

---

## Slide 14: Khả năng tổng quát hóa (Feature Generalization) *(1.5 phút)*
> "Bên cạnh độ quan trọng, Chip Huyen nhấn mạnh một tiêu chí nữa để đánh giá đặc trưng tốt là **khả năng tổng quát hóa (Feature Generalization)**. Một đặc trưng chỉ giúp tăng điểm trên tập huấn luyện nhưng nhanh chóng mất tác dụng ngoài thực tế không phải là đặc trưng tốt.
>
> Có hai khía cạnh cần kiểm tra. Thứ nhất là **độ bao phủ (Feature Coverage)**, tức đặc trưng đó xuất hiện trên bao nhiêu phần trăm dữ liệu. Nếu một đặc trưng chỉ có giá trị ở dưới 1% mẫu dữ liệu, nó rất dễ khiến mô hình học vẹt các trường hợp cá biệt. Thứ hai là **độ ổn định phân phối (Distribution Stability)**. Nếu phân phối của đặc trưng thay đổi quá nhanh theo từng tuần hoặc từng tháng – hiện tượng trôi dạt phân phối – mô hình sẽ nhanh chóng suy giảm độ chính xác nếu không được huấn luyện lại liên tục. Do đó, kỹ sư luôn phải cân bằng giữa việc mô tả chi tiết dữ liệu hiện tại và khả năng thích ứng lâu dài của đặc trưng."

---

## Slide 15: Mở rộng thực tế – Feature Store & MLOps *(1.0 phút)*
> "Để kết nối lý thuyết Chương 5 với kiến trúc hệ thống thực tế, nhóm xin giới thiệu ngắn gọn khái niệm **Feature Store**. Trong doanh nghiệp, một sự cố kỹ thuật rất hay gặp là **Training-Serving Skew**: đội ngũ khoa học dữ liệu viết công thức tính đặc trưng bằng Python lúc huấn luyện, nhưng đội ngũ kỹ sư hệ thống lại viết lại công thức đó bằng Java hoặc SQL lúc triển khai, dẫn đến sai số nhỏ làm mô hình dự đoán sai lệch.
>
> **Feature Store** – với các nền tảng phổ biến như Feast hay Hopsworks – ra đời để giải quyết vấn đề này. Nó đóng vai trò là kho lưu trữ và quản lý định nghĩa đặc trưng tập trung, đảm bảo đồng nhất giữa môi trường huấn luyện offline và môi trường phục vụ online độ trễ thấp, đồng thời cho phép các nhóm khác nhau tái sử dụng lại các đặc trưng đã được kiểm chứng."

---

## Slide 16: Tổng kết & Bài học rút ra *(1.0 phút)*
> "Để khép lại phần trình bày, nhóm xin đúc kết ba thông điệp cốt lõi từ Chương 5:
>
> Thứ nhất, **dữ liệu và hiểu biết nghiệp vụ là nền tảng**. Hãy chọn kỹ thuật xử lý missing, scaling hay mã hóa dựa trên cơ chế sinh ra dữ liệu và yêu cầu của từng họ thuật toán, đồng thời biết kết hợp cả đặc trưng thủ công và đặc trưng tự học khi cần thiết.
>
> Thứ hai, **kiểm soát chặt chẽ rò rỉ dữ liệu (Data Leakage)**. Luôn chia tập trước khi biến đổi, sử dụng Pipeline và chia tách đúng theo thời gian hoặc theo nhóm đối tượng để kết quả đánh giá phản ánh đúng thực tế.
>
> Thứ ba, **ưu tiên tính đơn giản và khả năng tổng quát hóa**. Hãy bắt đầu từ một tập đặc trưng đơn giản, sử dụng các công cụ như Permutation Importance hay SHAP để loại bỏ nhiễu, và đảm bảo đặc trưng giữ được sự ổn định khi đưa vào vận hành lâu dài."

---

## Slide 17: Tài liệu tham khảo & Video minh họa *(0.5 phút)*
> "Trên đây là danh mục tài liệu tham khảo chính thức và các bài giảng video trực quan mà nhóm đã tổng hợp để phục vụ quá trình nghiên cứu chuyên đề này, bao gồm sách gốc của Chip Huyen, các đầu sách chuyên sâu về Feature Engineering của O'Reilly và chuỗi video giải thích trực quan từ Stanford CS 329S và StatQuest."

---

## Slide 18: Cảm ơn & Thảo luận (Q&A) *(0.5 phút + Thời gian hỏi đáp)*
> "Phần trình bày của nhóm về Chương 5: Feature Engineering đến đây là kết thúc. Nhóm rất cảm ơn thầy/cô và các bạn đã chú ý lắng nghe. Sau đây, nhóm xin mời thầy/cô và các bạn đặt câu hỏi hoặc cùng thảo luận thêm về các nội dung vừa trình bày."

---

## Phụ lục: Gợi ý chuẩn bị câu trả lời cho phần Q&A

1. **Hỏi: Tại sao cây quyết định (XGBoost, Random Forest) không cần Scaling, còn Logistic Regression hay Neural Network lại cần?**
   - *Trả lời ngắn:* Cây quyết định tìm ngưỡng chia dựa trên thứ tự tương đối của giá trị (ví dụ: `Thu nhập > 20 triệu`), nên nhân hay chia cả cột cho một hằng số dương không làm thay đổi thứ tự này. Trong khi đó, các mô hình dùng Gradient Descent cập nhật trọng số tỷ lệ thuận với độ lớn giá trị đầu vào, nên thang đo không đồng đều sẽ làm quá trình tối ưu mất cân bằng và hội tụ chậm.
2. **Hỏi: Có nên dùng Target Encoding cho mọi biến phân loại nhiều danh mục không?**
   - *Trả lời ngắn:* Không nên dùng tùy tiện. Target Encoding dễ gây rò rỉ nhãn mục tiêu và overfitting nếu một danh mục chỉ xuất hiện vài lần. Khi dùng bắt buộc phải tính toán trên từng fold của Cross-Validation (out-of-fold) và áp dụng kỹ thuật làm mượt (smoothing) kéo giá trị về trung bình toàn cục. Nếu danh mục quá lớn và liên tục phát sinh giá trị mới lúc vận hành, Hashing Trick sẽ an toàn hơn.
3. **Hỏi: Làm sao phân biệt MAR và MNAR trong thực tế?**
   - *Trả lời ngắn:* Không có kiểm định thống kê thuần túy nào chứng minh chắc chắn hoàn toàn MNAR chỉ từ dữ liệu hiện có vì giá trị bị thiếu không quan sát được. Việc phân biệt chủ yếu dựa vào **kiến thức nghiệp vụ (domain knowledge)** và quy trình thu thập dữ liệu (ví dụ: hiểu tâm lý người thu nhập cao ngại khai báo, hoặc biết cảm biến hỏng ở dải nhiệt độ quá cao).

