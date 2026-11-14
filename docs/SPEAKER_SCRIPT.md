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
> "Chào thầy/cô và các bạn. Hôm nay mình xin chia sẻ về Chương 5: **Feature Engineering** trong cuốn *Designing Machine Learning Systems* của tác giả Chip Huyen. Bài trình bày sẽ tập trung trực tiếp vào các khái niệm và kỹ thuật cốt lõi nhất của chương."

---

## Slide 2: Mục tiêu & Nội dung chính *(0.5 phút)*
> "Để bắt đầu, nội dung bài trình bày hôm nay sẽ đi qua năm phần trọng tâm. Trước tiên là bản chất và tầm quan trọng của Feature Engineering, tiếp theo là so sánh giữa đặc trưng tự học và đặc trưng do con người thiết kế. Sau đó, chúng ta sẽ điểm qua các kỹ thuật xử lý đặc trưng thường dùng nhất trong thực tế, phân tích vấn đề rò rỉ dữ liệu (Data Leakage) cùng cách phòng ngừa, và cuối cùng là cách đánh giá để nhận biết một đặc trưng tốt."

---

## Slide 3: Đặt vấn đề – Tầm quan trọng của Feature Engineering *(1.5 phút)*
> "Trước hết, **Feature Engineering là gì**? Hiểu một cách đơn giản, đây là quá trình biến đổi dữ liệu thô ban đầu thành các đặc trưng có ý nghĩa để mô hình học máy có thể học quy luật hiệu quả nhất.
>
> Bước này quyết định rất lớn đến chất lượng mô hình vì nguyên lý cốt lõi: đầu vào là rác thì đầu ra cũng là rác. Điều này xuất phát từ hai lý do thực tế. Thứ nhất, mô hình học máy chỉ tính toán được trên dữ liệu dạng số, trong khi dữ liệu đời thực lại chứa nhiều văn bản, ngày giờ, ô trống hoặc các cột có thang đo chênh lệch nhau. Thứ hai, mô hình không tự hiểu được bối cảnh nghiệp vụ nếu chúng ta không diễn giải giúp nó.
>
> Chẳng hạn trong bài toán dự đoán thời gian chuyến xe công nghệ, nếu chỉ đưa vào tọa độ thô và mốc giờ `'17:45'`, mô hình sẽ rất khó học. Nhưng nếu chúng ta tính sẵn `khoảng cách = 8.5 km`, đồng thời gắn cờ `giờ cao điểm = 1` và `trời mưa = 1`, mô hình sẽ nắm bắt quy luật nhanh và dự đoán chính xác hơn nhiều so với việc tự tìm quy luật từ dữ liệu thô."

---

## Slide 4: Learned Features vs. Engineered Features *(1.5 phút)*
> "Một câu hỏi thường gặp là hiện nay Deep Learning đã có thể tự học đặc trưng, vậy việc tự thiết kế đặc trưng thủ công có còn cần thiết nữa không? Câu trả lời là rất cần, bởi mỗi cách tiếp cận phục vụ một loại dữ liệu khác nhau.
>
> Với **Learned Features**, mạng nơ-ron tự trích xuất đặc trưng nên rất hiệu quả trên dữ liệu phi cấu trúc như ảnh, video hay văn bản; tuy nhiên cách này đòi hỏi lượng dữ liệu lớn, chi phí GPU cao và kết quả là dạng hộp đen khó giải thích. Ngược lại, **Engineered Features** do con người thiết kế dựa trên hiểu biết nghiệp vụ lại là lựa chọn phù hợp cho dữ liệu dạng bảng — loại dữ liệu phổ biến trong tài chính, ngân hàng và bán lẻ — vì nó nhẹ, cần ít dữ liệu hơn và giải thích được rõ ràng.
>
> Trong thực tế, các hệ thống lớn thường kết hợp cả hai hướng này. Chẳng hạn tại YouTube, để quyết định video nào được ưu tiên hiển thị trên trang chủ, hệ thống vừa dùng vector nhúng tự học để hiểu nội dung video, vừa dùng các đặc trưng thủ công như lịch sử click gần đây của người dùng để sắp xếp thứ tự."

---

## Slide 5: Tổng quan các kỹ thuật xử lý đặc trưng *(0.5 phút)*
> "Về mặt kỹ thuật, Chương 5 đúc kết sáu nhóm phương pháp xử lý đặc trưng cơ bản và phổ biến nhất, gồm: xử lý dữ liệu khuyết thiếu, chuẩn hóa thang đo, rời rạc hóa, mã hóa biến phân loại, tổ hợp đặc trưng và nhúng vị trí. Bây giờ chúng ta sẽ lần lượt đi qua bản chất và điểm cần lưu ý của từng kỹ thuật."

---

## Slide 6: Xử lý dữ liệu khuyết thiếu *(1.5 phút)*
> "Kỹ thuật đầu tiên là xử lý dữ liệu khuyết thiếu. Khi thấy dữ liệu có các ô `NaN`, chúng ta không nên vội xóa dòng hay điền tùy tiện giá trị trung bình, mà cần xác định trước lý do dữ liệu bị thiếu. Thông thường có ba cơ chế: **MCAR** là thiếu ngẫu nhiên hoàn toàn do lỗi kỹ thuật như rớt mạng mất log; **MAR** là thiếu phụ thuộc vào một cột khác đã biết, chẳng hạn phụ nữ ít khai báo cân nặng hơn nam; và **MNAR** là thiếu do chính giá trị của cột đó, ví dụ người có thu nhập rất cao thường từ chối khai báo lương.
>
> Với trường hợp MNAR, nếu xóa dòng thì mô hình sẽ mất hẳn nhóm đối tượng đặc biệt này, còn nếu chỉ điền giá trị trung bình thì lại làm sai lệch thông tin. Vì vậy, bên cạnh việc điền bằng Median hoặc mô hình như KNN, một giải pháp rất hiệu quả là tạo thêm cột cờ **Missing Indicator**, ví dụ `income_is_missing = 1`, để biến chính trạng thái không khai báo thành một tín hiệu hữu ích cho mô hình."

---

## Slide 7: Chuẩn hóa phạm vi dữ liệu (Scaling) *(1.5 phút)*
> "Bước tiếp theo là chuẩn hóa phạm vi dữ liệu, hay Scaling, nhằm đưa các biến số về cùng một khoảng giá trị hợp lý. Trong cùng một bảng dữ liệu, cột `Tuổi` thường chỉ nằm từ 18 đến 60, trong khi `Thu nhập` có thể lên tới hàng chục triệu.
>
> Với các thuật toán dựa trên khoảng cách như KNN, SVM hay các mô hình tối ưu bằng Gradient Descent như mạng nơ-ron, bước chuẩn hóa là bắt buộc để thuật toán không bị biến có giá trị lớn chi phối và giúp quá trình hội tụ diễn ra nhanh hơn. Ngược lại, các mô hình dạng cây như Random Forest hay XGBoost lại không cần bước này vì chúng chỉ phân nhánh dựa trên thứ tự lớn nhỏ.
>
> Ba cách chuẩn hóa thường dùng nhất gồm **Min-Max Scaling** để đưa dữ liệu về đoạn `[0, 1]` nhưng khá nhạy với ngoại lai; **Standardization (Z-score)** để đưa về trung bình 0 và độ lệch chuẩn 1 giúp ổn định hơn; và **Log Transformation** để nén khoảng cách cho dữ liệu có phân phối lệch đuôi dài như doanh thu. Dù chọn cách nào, nguyên tắc bắt buộc là chỉ `fit` bộ chuẩn hóa trên tập Train, rồi dùng chính tham số đó để biến đổi sang tập Test nhằm tránh rò rỉ dữ liệu."

---

## Slide 8: Rời rạc hóa & Mã hóa biến phân loại *(2.0 phút)*
> "Kỹ thuật thứ ba là **rời rạc hóa (Binning)**, tức chia một biến liên tục thành từng khoảng nhỏ để giúp các mô hình tuyến tính học được quan hệ phi tuyến và giảm bớt tác động của giá trị ngoại lai.
>
> Song song đó, để mô hình hiểu được các biến dạng chữ, ta cần **mã hóa biến phân loại (Encoding)**. Nếu biến thực sự có thứ tự tự nhiên như *Thấp, Trung bình, Cao*, ta dùng **Ordinal Encoding** để gán các số `0, 1, 2`. Nếu biến không có thứ tự và số lượng danh mục ít, **One-Hot Encoding** sẽ tạo các cột nhị phân `0/1` rất an toàn, nhưng sẽ gây bùng nổ số chiều nếu có hàng nghìn mã sản phẩm.
>
> Khi số lượng danh mục lớn, hai lựa chọn phổ biến hơn là Target Encoding và Hashing Trick. **Target Encoding** thay từng danh mục bằng giá trị trung bình của nhãn mục tiêu, ví dụ thay mã quận `'Q1'` bằng tỷ lệ khách hàng rời bỏ trung bình tại quận đó. Tuy nhiên, nếu một danh mục chỉ xuất hiện một vài lần thì trung bình này rất dễ gây quá khớp, nên ta cần kết hợp thêm kỹ thuật làm mượt (smoothing) với trung bình chung của toàn tập và chỉ tính trong Cross-Validation. Trong khi đó, **Hashing Trick** dùng hàm băm để nén toàn bộ danh mục vào một vector có kích thước cố định; cách này vừa tiết kiệm bộ nhớ vừa xử lý được các danh mục mới phát sinh lúc vận hành, dù phải chấp nhận khả năng xảy ra va chạm băm khi hai danh mục khác nhau cùng rơi vào một vị trí."

---

## Slide 9: Feature Crossing & Positional Embeddings *(1.5 phút)*
> "Hai kỹ thuật tạo đặc trưng tiếp theo là Feature Crossing và Positional Embeddings. **Feature Crossing** là việc ghép hai hoặc nhiều biến lại thành một đặc trưng mới, chẳng hạn kết hợp `Loại xe` với `Khung giờ`. Cách làm này giúp các mô hình tuyến tính nắm bắt được tương tác phức tạp giữa các thuộc tính mà chưa cần chuyển sang mô hình phi tuyến nặng nề; tuy nhiên cần tránh ghép những cột có quá nhiều danh mục để không làm số chiều tăng vọt.
>
> Còn **Positional Embeddings** được dùng cho dữ liệu dạng chuỗi như văn bản hoặc chuỗi hành vi người dùng. Vì các kiến trúc như Transformer xử lý các phần tử song song nên tự thân mô hình không phân biệt được từ nào đứng trước, từ nào đứng sau. Do đó, ta cộng thêm một vector mã hóa vị trí vào đặc trưng đầu vào để mô hình nắm bắt được thứ tự tuần tự của chuỗi."

---

## Slide 10: Data Leakage – Khái niệm & Tác hại *(1.5 phút)*
> "Sau khi nắm các kỹ thuật tạo và biến đổi đặc trưng, chúng ta cần đặc biệt chú ý đến vấn đề kỹ thuật nghiêm trọng nhất khi triển khai học máy, đó là **Data Leakage**, tức rò rỉ dữ liệu.
>
> Data Leakage xảy ra khi thông tin từ tương lai hoặc từ chính nhãn mục tiêu vô tình lọt vào dữ liệu huấn luyện, trong khi lúc triển khai thực tế thì mô hình không hề có thông tin đó. Hệ quả là kết quả đánh giá lúc thử nghiệm rất cao, có thể lên tới 98–99%, nhưng khi đưa vào vận hành thật thì hiệu năng mô hình suy giảm nghiêm trọng.
>
> Lấy ví dụ trong y tế với bài toán dự đoán bệnh nhân có chuyển nặng hay không ngay lúc mới vào viện. Nếu trong bảng dữ liệu huấn luyện có cột `Mã_khoa_điều_trị` hoặc `Ngày_nhập_viện`, mô hình có thể đạt độ chính xác gần như tuyệt đối. Nhưng thực tế, bệnh nhân phải khám xong mới được phân khoa điều trị; tại thời điểm vừa đến quầy tiếp nhận, thông tin này hoàn toàn chưa tồn tại, khiến mô hình trở nên vô dụng ngoài thực tế."

---

## Slide 11: 5 Nguyên nhân gây Data Leakage phổ biến *(1.5 phút)*
> "Vậy Data Leakage thường xuất phát từ đâu? Trong thực tế có năm nguyên nhân phổ biến nhất. Thứ nhất là tiền xử lý trước khi chia tập, chẳng hạn tính min, max hay mean trên toàn bộ bảng dữ liệu rồi mới chia Train/Test, làm thông tin phân phối của tập Test lọt vào bước chuẩn hóa. Thứ hai là rò rỉ theo thời gian, xảy ra khi ta dùng chia ngẫu nhiên (Random Split) cho dữ liệu chuỗi thời gian, khiến mô hình dùng dữ liệu giao dịch ở tương lai để dự đoán hành vi trong quá khứ.
>
> Thứ ba là rò rỉ theo nhóm, khi dữ liệu của cùng một thực thể — chẳng hạn nhiều ảnh X-quang của cùng một bệnh nhân — xuất hiện ở cả Train và Test, làm mô hình ghi nhớ quá mức đặc điểm cá nhân thay vì học quy luật bệnh lý. Thứ tư là dữ liệu trùng lặp không được lọc sạch khiến cùng một bản ghi nằm ở cả hai tập. Và thứ năm là sự xuất hiện của biến đại diện cho nhãn (Target Proxy), tức cột đặc trưng phản ánh kết quả sau khi sự kiện mục tiêu đã xảy ra, như cột `số_tiền_được_duyệt` trong bài toán dự đoán hồ sơ có được duyệt vay hay không."

---

## Slide 12: Phòng chống & Phát hiện Data Leakage *(1.5 phút)*
> "Để kiểm soát vấn đề này, chúng ta cần chú ý cả hai khía cạnh là phòng ngừa và nhận diện. Về phòng ngừa, nguyên tắc đầu tiên là luôn chia tập Train và Test trước khi chạm vào dữ liệu, đồng thời đóng gói toàn bộ quy trình xử lý bằng `Pipeline` của Scikit-learn để đảm bảo mọi bước như scaling hay imputation chỉ `fit` trên phần Train trong từng vòng Cross-Validation. Với dữ liệu thời gian ta phải dùng **Temporal Split**, với dữ liệu theo nhóm đối tượng ta dùng **GroupKFold**, và luôn kiểm tra xem tại thời điểm dự đoán thật thì các cột đầu vào đã tồn tại hay chưa.
>
> Còn để phát hiện sớm khi đã lỡ xảy ra rò rỉ, ta cần cảnh giác nếu thấy độ chính xác cao bất thường so với thực tế bài toán, hoặc khi kiểm tra thấy một cột có độ quan trọng cao vượt trội bất thường so với phần còn lại thì phải rà soát lại nguồn gốc của cột đó ngay."

---

## Slide 13: Đánh giá đặc trưng – Feature Importance *(1.5 phút)*
> "Không phải cứ tạo càng nhiều đặc trưng thì mô hình càng tốt, vì các biến thừa sẽ làm mô hình nặng, chậm và dễ quá khớp. Do đó, ta cần đánh giá **Feature Importance** để chọn lọc lại.
>
> Cách nhanh nhất là dùng **Built-in Tree Importance** dựa trên mức giảm tạp chất trong cây quyết định, nhưng phương pháp này dễ thiên lệch về những cột có nhiều giá trị phân biệt. Khách quan hơn là **Permutation Importance**, hoạt động bằng cách xáo trộn ngẫu nhiên từng cột trên tập Validation rồi đo mức sụt giảm hiệu năng; cột nào khi xáo trộn mà làm hiệu năng tụt mạnh thì cột đó thực sự quan trọng, và cách này dùng được cho mọi loại mô hình. Cuối cùng, **SHAP** dựa trên lý thuyết trò chơi giúp đo chính xác đóng góp của từng đặc trưng không chỉ trên toàn tập dữ liệu mà còn cho từng mẫu dự đoán riêng lẻ; chẳng hạn với riêng hồ sơ vay A, SHAP có thể chỉ rõ `thu nhập` đóng góp tăng điểm uy tín bao nhiêu và `lịch sử trễ hạn` kéo giảm bao nhiêu."

---

## Slide 14: Tính tổng quát hóa (Feature Generalization) *(1.5 phút)*
> "Bên cạnh độ quan trọng trên dữ liệu hiện có, một đặc trưng tốt bắt buộc phải có khả năng tổng quát hóa khi gặp dữ liệu mới trong tương lai. Điều này thể hiện qua hai tiêu chí chính là **Feature Coverage** — tức tỷ lệ mẫu có giá trị cho đặc trưng đó, tránh dùng những đặc trưng chỉ xuất hiện ở dưới 1% trường hợp cá biệt vì mô hình rất dễ bị quá khớp — và **Distribution Stability**, đòi hỏi phân phối của đặc trưng không được biến động quá nhanh theo thời gian để hạn chế tình trạng Data Drift. Nói cách khác, ta luôn cần cân bằng giữa việc mô tả chi tiết dữ liệu hiện tại và giữ cho đặc trưng ổn định lâu dài."

---

## Slide 15: Tổng kết – 3 Bài học cốt lõi *(1.0 phút)*
> "Đúc kết lại toàn bộ Chương 5, có ba thông điệp cốt lõi cần ghi nhớ. Thứ nhất, hiểu bản chất dữ liệu là nền tảng để chọn đúng kỹ thuật xử lý từ missing, scaling đến encoding, cũng như biết cách kết hợp hợp lý giữa đặc trưng thủ công và đặc trưng tự học. Thứ hai, cần kiểm soát chặt chẽ Data Leakage ngay từ đầu bằng cách chia tập trước khi tiền xử lý, dùng Pipeline và chọn đúng chiến lược chia theo thời gian hoặc nhóm thực thể. Và thứ ba, hãy ưu tiên sự đơn giản và bền vững bằng cách dùng Permutation Importance hoặc SHAP để loại bỏ biến nhiễu, giữ lại những đặc trưng có độ bao phủ và tính ổn định cao."

---

## Slide 16: Tài liệu tham khảo *(0.5 phút)*
> "Trên đây là những nội dung cốt lõi của Chương 5. Các tài liệu chính mình sử dụng để tham khảo gồm giáo trình của Chip Huyen, sách Feature Engineering của O'Reilly, cùng các bài giảng trực quan từ Stanford và StatQuest."

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
