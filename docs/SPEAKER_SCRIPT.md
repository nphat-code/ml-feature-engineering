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
> "Chào thầy/cô và các bạn. Hôm nay mình sẽ trình bày phần seminar về Chương 5: **Feature Engineering** trong cuốn *Designing Machine Learning Systems* của tác giả Chip Huyen. Bài chia sẻ của mình sẽ kéo dài khoảng 22 phút, sau đó mình rất mong nhận được câu hỏi và trao đổi từ mọi người."

---

## Slide 2: Tổng quan nội dung *(0.5 phút)*
> "Để mọi người dễ theo dõi, bài trình bày hôm nay gồm 5 phần chính: đầu tiên là lý do vì sao Feature Engineering lại quan trọng; thứ hai là so sánh giữa đặc trưng do mô hình tự học và đặc trưng do con người thiết kế; phần ba đi qua 6 thao tác xử lý đặc trưng hay dùng nhất; phần bốn nói về một lỗi rất dễ gặp ngoài thực tế là Data Leakage; và cuối cùng là cách đánh giá một đặc trưng tốt cũng như cách quản lý đặc trưng khi đưa mô hình vào vận hành."

---

## Slide 3: Đặt vấn đề – Tầm quan trọng của Feature Engineering *(1.5 phút)*
> "Đầu tiên, nói ngắn gọn thì **Feature Engineering là gì**? Trong một bảng dữ liệu, mỗi cột thông tin mô tả đối tượng chính là một feature, hay còn gọi là đặc trưng. Feature Engineering là quá trình biến đổi dữ liệu thô ban đầu thành các đặc trưng có ý nghĩa, sao cho mô hình học máy dễ học được quy luật nhất.
>
> Andrew Ng từng có một câu nói rất nổi tiếng: *'Học máy ứng dụng về bản chất chính là làm feature engineering.'* Điều này bắt nguồn từ nguyên lý **Garbage In, Garbage Out** – đầu vào không tốt thì mô hình phức tạp mấy cũng không cho ra kết quả tốt được. Có hai lý do chính: một là mô hình học máy chỉ tính toán được trên số, trong khi dữ liệu thực tế toàn chữ viết, ngày giờ hoặc bị bỏ trống; hai là thuật toán không có sẵn kiến thức đời thực như con người.
>
> Chẳng hạn với bài toán dự đoán thời gian chạy của một chuyến xe công nghệ. Nếu mình chỉ đưa tọa độ thô và chuỗi `'17:45:00'`, mô hình sẽ rất khó tự suy luận. Nhưng nếu mình tính sẵn `khoang_cach = 8.5 km`, gắn thêm cờ `is_rush_hour = 1` và `is_rain = 1`, mô hình sẽ hiểu ngay ngữ cảnh và dự đoán chính xác hơn nhiều. Thực tế ở Facebook năm 2014 cũng vậy, khi dự đoán click quảng cáo, việc tìm ra đúng đặc trưng giúp cải thiện kết quả nhiều hơn hẳn so với việc ngồi tinh chỉnh thuật toán."

---

## Slide 4: Learned Features vs. Engineered Features *(1.5 phút)*
> "Nhiều bạn thường thắc mắc: bây giờ Deep Learning có thể tự học đặc trưng rồi, vậy mình có cần tự làm Feature Engineering nữa không? Câu trả lời là vẫn rất cần, vì hai cách này phát huy thế mạnh trên những loại dữ liệu khác nhau.
>
> **Learned Features** – tức đặc trưng do mạng nơ-ron tự học – làm rất tốt với dữ liệu phi cấu trúc như ảnh, âm thanh hay văn bản. Nhưng bù lại, nó cần rất nhiều dữ liệu, tốn chi phí GPU và khó giải thích vì sao mô hình ra quyết định. Trong khi đó, **Engineered Features** do con người thiết kế lại rất hiệu quả trên dữ liệu dạng bảng – thứ chiếm phần lớn trong ngân hàng, bán lẻ hay tài chính. Cách này cần ít dữ liệu hơn, mô hình nhẹ và dễ giải thích với bên nghiệp vụ.
>
> Vì vậy, các hệ thống lớn như YouTube hay Shopee thường kết hợp cả hai: dùng Learned Features để hiểu nội dung video hoặc sản phẩm qua vector embedding, còn Engineered Features để mô tả hành vi người dùng như số lượt click trong một giờ qua, rồi gộp chung vào mô hình xếp hạng cuối cùng."

---

## Slide 5: Common Feature Engineering Operations – Tổng quan *(0.5 phút)*
> "Bây giờ mình sẽ đi vào phần kỹ thuật trọng tâm của chương này, đó là 6 thao tác xử lý đặc trưng phổ biến nhất mà Chip Huyen tổng hợp: xử lý dữ liệu bị thiếu, chuẩn hóa thang đo, rời rạc hóa, mã hóa biến phân loại, tổ hợp đặc trưng và nhúng vị trí. Mình sẽ đi lần lượt từng kỹ thuật xem bản chất và điểm cần lưu ý là gì."

---

## Slide 6: Xử lý dữ liệu khuyết thiếu *(1.5 phút)*
> "Thao tác đầu tiên là xử lý dữ liệu khuyết thiếu. Khi thấy bảng dữ liệu có nhiều ô trống `NaN`, phản xạ quen thuộc của nhiều người là xóa luôn dòng đó hoặc điền số trung bình vào. Nhưng trước khi làm vậy, mình nên hỏi: tại sao dữ liệu lại bị thiếu?
>
> Có 3 cơ chế chính: **MCAR** là thiếu ngẫu nhiên hoàn toàn, ví dụ rớt mạng làm mất vài dòng log; **MAR** là thiếu có liên quan đến một cột khác đã biết, chẳng hạn phụ nữ ít điền cân nặng hơn nam giới trong khảo sát; và đáng chú ý nhất là **MNAR** – thiếu vì chính giá trị đó, ví dụ người thu nhập rất cao thường ngại khai báo mức lương. Nếu mình xóa bừa các dòng MNAR, mình sẽ làm mất hẳn nhóm người thu nhập cao khỏi dữ liệu; còn nếu điền trung bình vào thì lại làm sai lệch thông tin.
>
> Do đó, bên cạnh việc điền Median hay dùng mô hình KNN để dự đoán giá trị thiếu, có một mẹo rất hữu ích là tạo thêm cột cờ **Missing Indicator**, ví dụ `income_is_missing = 1`. Nhờ cột cờ này, việc người dùng không chịu khai báo cũng trở thành một thông tin để mô hình học."

---

## Slide 7: Chuẩn hóa phạm vi dữ liệu *(1.5 phút)*
> "Thao tác thứ hai là **Scaling và Normalization**, hiểu đơn giản là đưa các cột số về cùng một thang đo hợp lý. Trong thực tế, cột `Tuổi` chỉ quanh quẩn từ 18 đến 65, nhưng cột `Thu nhập` lại lên tới hàng chục, hàng trăm triệu. Với các thuật toán tính khoảng cách như KNN hay SVM, biến `Thu nhập` sẽ lấn át hoàn toàn biến `Tuổi`. Còn với mạng nơ-ron hoặc hồi quy dùng Gradient Descent, thang đo lệch nhau làm hàm mất mát bị dẹt, khiến đường đi tối ưu bị zic-zac và hội tụ chậm. Riêng các mô hình dạng cây như Random Forest hay XGBoost thì không bị ảnh hưởng bởi bước này, vì chúng chỉ chia nhánh dựa trên thứ tự lớn nhỏ.
>
> Thường mình sẽ có 3 cách chính: **Min-Max Scaling** ép giá trị về đoạn `[0, 1]`, nhưng rất nhạy với giá trị ngoại lai; **Standardization (Z-score)** đưa dữ liệu về trung bình 0 và độ lệch chuẩn 1, nên ổn định hơn khi có ngoại lai; còn với dữ liệu lệch đuôi dài như doanh thu hay lượt xem, mình dùng thêm **Log Transformation** để nén các con số quá lớn lại.
>
> Một điểm mọi người nhất định phải nhớ ở đây là: chỉ tính min, max, trung bình và độ lệch chuẩn trên tập **Train**, rồi mới dùng bộ thông số đó áp sang tập Test. Nếu chuẩn hóa trên cả tập dữ liệu rồi mới chia thì sẽ bị rò rỉ dữ liệu ngay."

---

## Slide 8: Rời rạc hóa & Mã hóa biến phân loại *(2.0 phút)*
> "Thao tác thứ ba là **Rời rạc hóa (Discretization hay Binning)**, tức chia một biến liên tục như `Tuổi` thành các nhóm rời rạc. Cách này giúp các mô hình tuyến tính học được những mối quan hệ phi tuyến và bớt bị ảnh hưởng bởi nhiễu. Tuy nhiên, chia khoảng cũng có thể làm mất đi độ chi tiết ở phần ranh giới giữa hai nhóm.
>
> Tiếp theo là thao tác thứ tư: **Mã hóa biến phân loại (Categorical Encoding)** để đổi các nhãn chữ sang dạng số cho mô hình đọc được:
> - **Ordinal Encoding** gán số `0, 1, 2...`, nhưng chỉ nên dùng khi biến có thứ tự thật sự như *Thấp, Trung bình, Cao*.
> - **One-Hot Encoding** biến mỗi danh mục thành một cột `0/1`, rất gọn khi ít danh mục, nhưng nếu có vài nghìn tên sản phẩm thì số cột sẽ phình ra rất lớn.
> - Để xử lý nhiều danh mục, mình có thể dùng **Target Encoding** – thay tên danh mục bằng trung bình nhãn mục tiêu của nhóm đó, nhưng nhớ phải làm mượt (smoothing) và tính trong cross-validation để tránh overfitting.
> - Còn nếu gặp dữ liệu cực lớn như hàng triệu ID người dùng, hoặc lúc chạy thật xuất hiện danh mục mới chưa từng gặp, **Hashing Trick** dùng hàm băm nén toàn bộ vào một mảng kích thước cố định. Cách này tiết kiệm bộ nhớ và xử lý được danh mục lạ, chỉ đánh đổi một rủi ro nhỏ là xung đột băm."

---

## Slide 9: Feature Crossing & Positional Embeddings *(1.5 phút)*
> "Hai kỹ thuật còn lại trong nhóm này là **Feature Crossing** và **Positional Embeddings**.
>
> **Feature Crossing** nghĩa là ghép hai hay nhiều đặc trưng lại với nhau để tạo đặc trưng mới, ví dụ ghép `Loại xe` với `Khung giờ`. Bởi vì cùng một khung giờ cao điểm, xe máy và xe tải sẽ có tốc độ di chuyển rất khác nhau. Việc ghép như vậy giúp mô hình tuyến tính nắm bắt được sự tương tác giữa các biến, tuy nhiên nếu ghép hai cột có quá nhiều danh mục thì số chiều dữ liệu sẽ tăng rất nhanh.
>
> Còn **Positional Embeddings** thì dành cho dữ liệu có thứ tự, như câu văn hoặc chuỗi click chuột của người dùng. Các mô hình như Transformer xử lý song song tất cả các từ cùng một lúc nên tự nó không biết từ nào đứng trước, từ nào đứng sau. Vì thế, người ta cộng thêm một vector biểu diễn vị trí vào từng phần tử đầu vào để mô hình giữ được thông tin thứ tự của chuỗi."

---

## Slide 10: Data Leakage – Khái niệm & Hệ quả *(1.5 phút)*
> "Sang phần thứ tư, mình muốn nhấn mạnh một vấn đề rất hay làm các dự án Machine Learning gặp sự cố khi đưa ra thực tế, đó là **Data Leakage** hay rò rỉ dữ liệu.
>
> Hiểu đơn giản, Data Leakage xảy ra khi thông tin từ tập kiểm thử hoặc thông tin của tương lai vô tình lọt vào lúc huấn luyện, nhưng tại thời điểm dự đoán thật ngoài đời thì mô hình không hề có thông tin đó. Kết quả là lúc thử nghiệm nội bộ, mô hình đạt độ chính xác rất cao, tới 98–99%, nhưng khi đem ra dùng thật thì dự đoán sai hàng loạt.
>
> Mình lấy một ví dụ dễ hình dung trong y tế: giả sử bạn xây dựng mô hình dự đoán bệnh nhân có mắc bệnh nặng hay không ngay khi họ vừa tới bệnh viện. Nếu trong bảng dữ liệu lại có cột `Mã_khoa_điều_trị` hoặc `Ngày_nhập_viện`, mô hình sẽ học rất nhanh và đạt điểm gần như tuyệt đối. Nhưng thực tế, bệnh nhân phải được bác sĩ khám và chẩn đoán xong thì mới có mã khoa điều trị. Khi bệnh nhân mới bước vào quầy tiếp nhận, cột đó chưa hề tồn tại, nên mô hình coi như vô tác dụng."

---

## Slide 11: Các nguyên nhân gây Data Leakage phổ biến *(2.0 phút)*
> "Chip Huyen liệt kê 5 nguyên nhân gây rò rỉ dữ liệu hay gặp nhất trong thực tế:
>
> Thứ nhất là **tiền xử lý trước khi chia tập**, như lúc nãy mình có nhắc – tính trung bình hay chuẩn hóa trên toàn bộ dữ liệu rồi mới chia Train/Test.
>
> Thứ hai là **rò rỉ theo thời gian**. Với dữ liệu chuỗi thời gian như doanh số hay giá chứng khoán, nếu chia ngẫu nhiên thì mô hình sẽ vô tình dùng thông tin của ngày mai để đoán cho ngày hôm qua.
>
> Thứ ba là **rò rỉ theo nhóm**. Giả sử mỗi bệnh nhân có 10 ảnh chụp X-quang, nếu chia ngẫu nhiên thì ảnh của cùng một người có thể nằm ở cả Train và Test. Lúc đó mô hình dễ học thuộc đặc điểm riêng của người đó thay vì học dấu hiệu bệnh.
>
> Thứ tư là **dữ liệu bị trùng lặp hoặc gần trùng lặp** nằm rải rác ở cả hai tập Train và Test mà không được lọc ra trước.
>
> Và thứ năm là **biến đại diện cho nhãn (Target Proxy)** – những cột chỉ được sinh ra sau khi kết quả đã ngã ngũ, giống như ví dụ mã khoa điều trị ở slide trước."

---

## Slide 12: Cách phát hiện và phòng chống Data Leakage *(1.5 phút)*
> "Vậy làm sao để phát hiện và tránh được Data Leakage?
>
> Về cách phòng tránh, nguyên tắc quan trọng nhất là **luôn chia tập dữ liệu trước** mọi bước xử lý. Khi viết code, mình nên gói toàn bộ quy trình biến đổi và mô hình vào một `Pipeline` (như `sklearn.pipeline.Pipeline`) để đảm bảo bước `fit` chỉ chạy trên phần dữ liệu huấn luyện. Bên cạnh đó, phải chia tập đúng với cấu trúc dữ liệu: dùng **Temporal Split** cho dữ liệu thời gian, và dùng **GroupKFold** nếu dữ liệu gom theo từng người dùng hay bệnh nhân. Cuối cùng là kiểm tra kỹ xem ở thời điểm dự đoán thật thì từng cột dữ liệu đã có sẵn hay chưa.
>
> Còn để phát hiện sớm, bạn cứ để ý hai dấu hiệu: một là kết quả đánh giá trên tập Validation cao một cách bất thường so với thực tế; hai là khi kiểm tra độ quan trọng đặc trưng thấy có một cột chiếm ảnh hưởng áp đảo. Gặp trường hợp này, mình nên thử bỏ cột đó ra để kiểm tra lại xem nó có đang vô tình mang thông tin tương lai hay không."

---

## Slide 13: Đánh giá đặc trưng – Feature Importance *(1.5 phút)*
> "Khi đã tạo ra nhiều đặc trưng rồi, không phải cứ giữ hết lại là tốt. Dữ liệu thừa vừa làm mô hình nặng, vừa tăng độ trễ lúc dự đoán lại dễ gây quá khớp. Vì vậy mình cần đo **Feature Importance** để biết đặc trưng nào thật sự có ích.
>
> Có 3 cách đo hay dùng: thứ nhất là dùng chỉ số có sẵn trong mô hình cây, ưu điểm là nhanh nhưng hay bị thiên lệch về các cột có nhiều giá trị khác nhau. Thứ hai là **Permutation Importance**: mình thử xáo trộn ngẫu nhiên giá trị của một cột trên tập Validation rồi xem độ chính xác tụt bao nhiêu; tụt càng nhiều chứng tỏ cột đó càng quan trọng. Cách này khá công bằng và dùng được cho mọi mô hình. Thứ ba là **SHAP**, dựa trên lý thuyết trò chơi, giúp mình nhìn rõ đóng góp của từng đặc trưng không chỉ trên toàn tập dữ liệu mà còn giải thích được cho từng dự đoán cụ thể."

---

## Slide 14: Khả năng tổng quát hóa (Feature Generalization) *(1.5 phút)*
> "Ngoài độ quan trọng, Chip Huyen còn nhắc đến một yếu tố khác để xem đặc trưng có bền vững không, đó là **khả năng tổng quát hóa (Feature Generalization)**. Một đặc trưng chỉ giúp tăng điểm trên tập dữ liệu hiện tại nhưng vài tuần sau lại mất tác dụng thì không ổn định.
>
> Có hai tiêu chí cần kiểm tra ở đây. Đầu tiên là **Feature Coverage (Độ bao phủ)**, tức đặc trưng đó xuất hiện trên bao nhiêu phần trăm dữ liệu. Nếu một đặc trưng chỉ có giá trị ở vài trường hợp hiếm gặp (dưới 1%), mô hình rất dễ học vẹt. Thứ hai là **Distribution Stability (Độ ổn định phân phối)**. Nếu phân phối của đặc trưng thay đổi quá nhanh theo thời gian – tức bị drift dữ liệu – mô hình sẽ xuống cấp nhanh và phải huấn luyện lại liên tục. Do đó, người làm mô hình luôn phải cân bằng giữa việc mô tả chi tiết dữ liệu hiện tại và giữ cho đặc trưng ổn định lâu dài."

---

## Slide 15: Mở rộng thực tế – Feature Store & MLOps *(1.0 phút)*
> "Trước khi kết thúc, mình mở rộng thêm một chút sang góc độ triển khai hệ thống thực tế với khái niệm **Feature Store**. Trong các công ty công nghệ, có một lỗi rất hay xảy ra gọi là **Training-Serving Skew**: người làm mô hình viết công thức tính đặc trưng bằng Python lúc huấn luyện, nhưng kỹ sư backend lại viết lại công thức đó bằng Java hoặc SQL lúc chạy thật, dẫn đến chênh lệch nhỏ làm mô hình dự đoán sai.
>
> Để giải quyết chuyện này, người ta dùng **Feature Store** như Feast hay Hopsworks. Hiểu đơn giản, nó là một kho tập trung lưu giữ định nghĩa đặc trưng, đảm bảo công thức tính giữa lúc huấn luyện offline và lúc phục vụ online hoàn toàn giống nhau, đồng thời giúp các nhóm khác trong công ty có thể dùng lại đặc trưng đã làm sẵn."

---

## Slide 16: Tổng kết & Bài học rút ra *(1.0 phút)*
> "Tóm lại, qua Chương 5 của Chip Huyen, có ba ý chính mình muốn mọi người nhớ lại sau buổi hôm nay:
>
> Thứ nhất, **hiểu dữ liệu và ngữ cảnh nghiệp vụ là gốc**: chọn cách xử lý missing, scaling hay encoding dựa trên đặc điểm dữ liệu và họ mô hình, đồng thời kết hợp linh hoạt giữa đặc trưng thủ công và đặc trưng tự học.
>
> Thứ hai, **cẩn trọng với Data Leakage**: luôn chia tập trước khi xử lý, dùng `Pipeline` và chọn cách chia đúng theo thời gian hoặc theo nhóm để không bị ảo tưởng bởi kết quả đánh giá offline.
>
> Thứ ba, **bắt đầu đơn giản và kiểm tra khả năng tổng quát hóa**: dùng Permutation Importance hay SHAP để giữ lại những đặc trưng thật sự có giá trị và ổn định theo thời gian."

---

## Slide 17: Tài liệu tham khảo & Video minh họa *(0.5 phút)*
> "Đây là các tài liệu và video mình đã tham khảo để chuẩn bị cho bài hôm nay, gồm sách gốc của Chip Huyen, hai đầu sách chuyên sâu về Feature Engineering của O'Reilly và CRC Press, cùng một số video giải thích trực quan từ Stanford CS 329S và kênh StatQuest."

---

## Slide 18: Cảm ơn & Thảo luận (Q&A) *(0.5 phút + Thời gian hỏi đáp)*
> "Phần trình bày của mình về Chương 5: Feature Engineering đến đây là hết. Cảm ơn thầy/cô và các bạn đã lắng nghe. Nếu thầy/cô hay bạn nào có câu hỏi hoặc góc nhìn khác về các kỹ thuật vừa rồi, mình cùng trao đổi thêm nhé."

---

## Phụ lục: Gợi ý chuẩn bị câu trả lời cho phần Q&A

1. **Hỏi: Tại sao cây quyết định (XGBoost, Random Forest) không cần Scaling, còn Logistic Regression hay Neural Network lại cần?**
   - *Trả lời ngắn:* Cây quyết định chỉ tìm ngưỡng chia dựa trên thứ tự lớn nhỏ (ví dụ: `Thu nhập > 20 triệu`), nên mình có nhân hay chia cả cột cho một số dương thì thứ tự đó vẫn giữ nguyên. Còn với Logistic Regression hay mạng nơ-ron dùng Gradient Descent, bước cập nhật trọng số tỷ lệ thuận với độ lớn của đầu vào, nên thang đo lệch nhau sẽ làm mô hình hội tụ chậm và mất cân bằng.
2. **Hỏi: Có nên dùng Target Encoding cho mọi biến phân loại nhiều danh mục không?**
   - *Trả lời ngắn:* Không nên dùng tùy tiện vì rất dễ gây rò rỉ nhãn mục tiêu và overfitting nếu có những danh mục xuất hiện quá ít lần. Khi dùng, bắt buộc phải tính trên từng fold của Cross-Validation (out-of-fold) và làm mượt (smoothing) kéo giá trị về trung bình chung. Nếu danh mục quá nhiều và liên tục phát sinh giá trị mới lúc chạy thật, dùng Hashing Trick sẽ an toàn hơn.
3. **Hỏi: Làm sao phân biệt MAR và MNAR trong thực tế?**
   - *Trả lời ngắn:* Thực tế không có công thức thống kê nào chứng minh chắc chắn 100% một cột là MNAR chỉ dựa vào dữ liệu hiện có, vì phần giá trị bị thiếu mình đâu có nhìn thấy được. Việc phân biệt chủ yếu dựa vào **kiến thức nghiệp vụ** và cách hệ thống thu thập dữ liệu (ví dụ: biết tâm lý người thu nhập cao ngại điền lương, hoặc biết cảm biến hay mất tín hiệu khi nhiệt độ vượt ngưỡng).
