# Seminar Machine Learning - Chapter 5: Feature Engineering

Báo cáo chuyên đề Seminar môn Học Máy (Machine Learning).

- **Chủ đề:** Chapter 5: Feature Engineering
- **Tài liệu tham khảo chính:** *Designing Machine Learning Systems: An Iterative Process for Production-Ready Applications* - Chip Huyen (O'Reilly Media, 2022)
- **Thời lượng thuyết trình:** 20 - 25 phút (Bắt đầu từ tuần 8)

---

## 📂 Cấu trúc Repository

```text
├── .vscode/
│   └── settings.json             # Cấu hình tự động mở file Markdown ở dạng Preview
├── docs/                         # Tài liệu / Slide thuyết trình (PPTX, PDF)
├── src/                          # Code demo (nếu có)
├── SEMINAR_NOTE_CHAPTER_5.md     # Bản ghi chú chi tiết lý thuyết, dàn ý slide và references
├── .gitignore                    # Bộ lọc file không cần thiết khi commit
└── README.md                     # Giới thiệu tổng quan dự án
```

---

## 🎯 Nội dung trọng tâm báo cáo

1. **Learned Features vs. Engineered Features:** So sánh đặc trưng tự học và thiết kế thủ công, ngữ cảnh áp dụng thực tế.
2. **Common Feature Engineering Operations:**
   - Xử lý dữ liệu khuyết thiếu (Missing values: MCAR, MAR, MNAR, Imputation).
   - Chuẩn hóa phạm vi (Scaling: Min-Max, Standardization).
   - Rời rạc hóa (Discretization / Binning).
   - Mã hóa biến phân loại (Categorical Encoding: One-Hot, Ordinal, Target, Hashing Trick).
   - Tổ hợp đặc trưng (Feature Crossing) & Nhúng vị trí (Positional Embeddings).
3. **Data Leakage (Rò rỉ dữ liệu):**
   - Các nguyên nhân thường gặp trong thực tế (Split sau transform, time leakage, group leakage, target proxy).
   - Cách phát hiện và các biện pháp phòng chống (Pipeline, Temporal split, GroupKFold).
4. **Engineering Good Features:**
   - Đánh giá độ quan trọng (Feature Importance: SHAP, Permutation).
   - Khả năng tổng quát hóa (Feature Generalization) và ổn định trong Production.

---

## 📖 Chi tiết kế hoạch & Dàn ý Slide

Xem chi tiết toàn bộ lý thuyết, kịch bản 18 slides và danh mục tài liệu/video tham khảo tại:  
👉 [SEMINAR_NOTE_CHAPTER_5.md](SEMINAR_NOTE_CHAPTER_5.md)
