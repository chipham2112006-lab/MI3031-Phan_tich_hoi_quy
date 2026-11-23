# MI3031 – Chương 4: Hồi quy tuyến tính đơn (Nhóm 4)

Bài tập phân tích **hồi quy tuyến tính đơn biến** (mục 4.1 trong slide Chương 4, học phần MI3031), trình bày từ **lý thuyết → tính toán → kiểm định → dự đoán** trên bộ dữ liệu *Student Performance*.

- **Biến phụ thuộc (Y):** `Performance Index` – chỉ số kết quả học tập
- **Biến độc lập (x):** `Previous Scores` – điểm các bài kiểm tra trước đó (được chọn vì có hệ số tương quan cao nhất với Y, r ≈ 0,92)

## Cấu trúc thư mục

```
.
├── MI3031_Nhom4.ipynb       # Notebook chính (đã chạy sẵn, có đầy đủ kết quả và biểu đồ)
├── Student_Performance.csv  # Bộ dữ liệu (10 000 dòng, 6 cột)
├── Sltk.pdf                 # Slide Chương 4 (nếu cần đối chiếu)
├── requirements.txt         # Thư viện cần cài
├── .gitignore
└── README.md
```

> Notebook tự tìm file dữ liệu theo các tên `Student_Performance.csv` hoặc `StudentsPerformance.csv` (cùng thư mục hoặc thư mục `data/`), nên đổi tên file không gây lỗi.

## Nội dung notebook (bám sát slide mục 4.1)

| Phần | Nội dung | Mục slide |
|---|---|---|
| 1 | Chuẩn bị môi trường, đọc dữ liệu | – |
| 2 | Khám phá dữ liệu (EDA), chọn biến độc lập | – |
| 3 | Mô hình hồi quy tuyến tính đơn biến, các giả thiết | 4.1.1 |
| 4 | Ước lượng bình phương nhỏ nhất (tính tay + đối chiếu `statsmodels`, `scikit-learn`) | 4.1.2 |
| 5 | Tính chất ước lượng: kỳ vọng, phương sai, ước lượng σ², sai số chuẩn; mô phỏng tính không chệch | 4.1.3 |
| 6 | Kiểm định t cho β₀, β₁; kiểm định F; bảng ANOVA | 4.1.4 |
| 7 | Khoảng tin cậy cho β₀, β₁; KTC cho trung bình; khoảng dự đoán | 4.1.5 |
| 8 | Phân tích phần dư; R²; hệ số tương quan; ý nghĩa hệ số chặn | 4.1.6 |
| 9 | Hồi quy trên các phép biến đổi (ln Y, √Y, 1/x, ln x) | 4.1.7 |
| 10 | Dự đoán: hold-out 80/20, hàm dự đoán kèm khoảng tin cậy/dự đoán | ứng dụng |
| 11 | Tổng kết, kết luận, hạn chế | – |

## Cách chạy

**Yêu cầu:** Python 3.10 trở lên.

```bash
# 1. (khuyến nghị) tạo môi trường ảo
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 2. cài thư viện
pip install -r requirements.txt

# 3. mở notebook
jupyter lab MI3031_Nhom4.ipynb
```

Sau đó chọn **Run → Run All Cells**. Kết quả có thể tái lập vì đã cố định `RANDOM_STATE = 42`.

## Kết quả chính

| Chỉ tiêu | Giá trị |
|---|---|
| Đường hồi quy | `Performance Index = -15,1818 + 1,0138 × Previous Scores` |
| KTC 95% cho β₁ | [1,0051 ; 1,0226] |
| t (β₁ = 0) / F | 227,06 / 51 555,11 (p ≈ 0) |
| R² | 0,8376 |
| σ̂ (sai số chuẩn của phần dư) | 7,74 |
| RMSE trên tập kiểm tra | 7,68 (gần bằng tập huấn luyện → không quá khớp) |

**Nhận xét ngắn:**
- Previous Scores ảnh hưởng tuyến tính, thuận chiều và có ý nghĩa thống kê rất cao đến Performance Index; mỗi điểm tăng ứng với khoảng +1,01 điểm chỉ số kết quả.
- Phần dư không cong, không có dạng phễu, phương sai đồng nhất (Breusch–Pagan p ≈ 0,17).
- Tính chuẩn của sai số chỉ thỏa mãn xấp xỉ (phần dư nhẹ đuôi). Nhờ n lớn nên suy diễn về hệ số vẫn đáng tin, nhưng khoảng dự đoán hơi rộng thừa (phủ ≈ 98,8% thay vì 95%).
- Các phép biến đổi không cải thiện mô hình, nên giữ mô hình tuyến tính gốc.
- Hệ số chặn không có ý nghĩa thực tế vì x = 0 nằm ngoài miền dữ liệu (40 – 99).

## Dữ liệu

| Biến | Ý nghĩa |
|---|---|
| `Hours Studied` | Số giờ học |
| `Previous Scores` | Điểm các bài kiểm tra trước đó |
| `Extracurricular Activities` | Có tham gia ngoại khóa (Yes/No) |
| `Sleep Hours` | Số giờ ngủ trung bình mỗi ngày |
| `Sample Question Papers Practiced` | Số đề mẫu đã luyện |
| `Performance Index` | Chỉ số kết quả học tập (biến phụ thuộc) |

Dữ liệu không có giá trị thiếu; 127 dòng trùng lặp được giữ nguyên vì các biến nhận giá trị trong miền nhỏ nên trùng lặp là tự nhiên.

## Phạm vi

Chỉ thực hiện **hồi quy tuyến tính đơn** (hết mục 4.1). Tương quan (4.2) và hồi quy bội nằm ngoài phạm vi bài này.

## Thành viên nhóm 4

| Họ tên | MSSV | Phân công |
|---|---|---|
| | | |
