# 📚 TADA - Bestseller Book Analysis & Prediction

> Phân tích dữ liệu & xây dựng mô hình dự đoán sách bán chạy cho sàn thương mại điện tử chuyên sách **Tada**, phục vụ đội ngũ Category Management trong việc ra quyết định nhập hàng, định giá và khuyến mãi.

**Capstone Project — Rikkei Education x HUST**

🔗 **Notebook phân tích (Google Colab):** [Xem tại đây](https://colab.research.google.com/drive/1M7ysFg4LAk1qRjVio_PYgwZKUqt22UVn?usp=drive_link)
🔗 **Toàn bộ dự án (dữ liệu, slide, dashboard, script) — Google Drive:** [Xem tại đây](https://drive.google.com/drive/folders/15UfTf_umlDd_zm4SvnGeey7LIv1agjaw?usp=drive_link)

---

## 📖 Tổng quan

Tada là sàn thương mại điện tử chuyên sâu về ngành Sách tại Việt Nam. Với hàng nghìn đầu sách trên sàn, đội ngũ Category Management cần một cơ sở dữ liệu vững chắc để trả lời câu hỏi: **thể loại nào, nhà xuất bản nào, mức giá nào thực sự tạo ra sách bán chạy?**

Dự án khai thác dữ liệu sản phẩm, doanh số và bình luận khách hàng thực tế trên sàn để:
- Phân tích các yếu tố ảnh hưởng đến khả năng bán chạy của một cuốn sách
- Xây dựng mô hình Machine Learning dự đoán tiềm năng bán chạy trước khi nhập hàng
- Dựng dashboard Power BI theo dõi hiệu suất kinh doanh liên tục
- Đề xuất hành động cụ thể cho Category Management, Marketing và Ban lãnh đạo

## 🎯 Mục tiêu phân tích

1. Xác định thể loại / nhà xuất bản có khả năng bán chạy cao nhất
2. Tìm các yếu tố (giá bán, mức giảm giá, rating, số lượng đánh giá...) thực sự ảnh hưởng đến doanh số
3. Xây dựng mô hình dự đoán sách bán chạy dựa trên đặc trưng biết trước khi nhập hàng
4. Chuyển hoá phát hiện thành khuyến nghị hành động cụ thể cho từng bộ phận

## 🗂️ Dữ liệu

| Nguồn | Nội dung |
|---|---|
| `book_data.csv` | Thông tin sản phẩm & doanh số (giá, số lượng bán, thể loại, NXB, rating...) |
| `prepared_data_book.csv` | Dữ liệu đã xử lý sẵn (giá quy đổi, mức giảm giá) |
| `comments.csv` | 141.000+ lượt bình luận & đánh giá của khách hàng |
| `book_id.csv` | Danh sách ID sản phẩm tham chiếu |

Sau làm sạch: **1.777 đầu sách**, trải trên **355 thể loại** và **42 nhà xuất bản**.

**Định nghĩa "sách bán chạy":** top 20% đầu sách có số lượng bán cao nhất toàn sàn (`is_bestseller = 1`), dùng nhất quán xuyên suốt toàn bộ phân tích và mô hình.

## 🔬 Phương pháp

1. **Làm sạch & hợp nhất dữ liệu** — loại trùng lặp, tính lại mức giảm giá, tổng hợp đặc trưng hành vi khách hàng từ dữ liệu bình luận
2. **Phân tích khám phá (EDA)** — mức độ tập trung doanh số (Pareto), hiệu suất theo thể loại/NXB, ảnh hưởng của giá bán/giảm giá/rating
3. **Xây dựng mô hình dự đoán** — Random Forest với K-Fold Target Encoding, RandomizedSearchCV để tinh chỉnh siêu tham số, đánh giá bằng 5-fold Cross-Validation
4. **Dựng dashboard vận hành** — Power BI, 4 trang theo dõi liên tục

## 📊 Phát hiện chính

- **Nguyên lý Pareto:** ~20% đầu sách chiếm **82% tổng số lượng bán** và **81,7% tổng doanh thu** — doanh số cực kỳ tập trung vào nhóm sách "hit"
- **Thể loại hiệu suất cao:** Sách tài chính - tiền tệ, kỹ năng làm việc, tư duy - kỹ năng sống có tỷ lệ bán chạy vượt trội (30–42%) so với mặt bằng chung
- **Nhà xuất bản:** quy mô doanh số lớn không đồng nghĩa ổn định — một NXB có thể dẫn đầu doanh số chỉ nhờ 1–2 cuốn "hit" đơn lẻ
- **Giá & khuyến mãi:** vùng giá 50.000–120.000đ và mức giảm giá 30–40% tối ưu hoá khả năng bán chạy
- **Rating:** điểm đánh giá tuyệt đối 5.0 **không** đồng nghĩa bán chạy nhất — hiệu ứng mẫu nhỏ (sách ít review dễ đạt 5.0) khiến nhóm 4.8–4.9 (nhiều review hơn) mới thực sự đáng tin cậy

## 🤖 Mô hình dự đoán

| Mô hình | Mục đích | AUC | F1 (lớp bán chạy) |
|---|---|---|---|
| **Mô hình A — Random Forest** | Dự đoán trước khi nhập hàng (giá, giảm giá, rating, thể loại, NXB) | 0.81 | 0.55 |
| **Mô hình B — Giám sát sớm** | Phát hiện sách đang "lên hit" dựa trên tín hiệu review/bình luận sau khi lên kệ | 0.98 | 0.85 |

Yếu tố ảnh hưởng lớn nhất đến khả năng bán chạy (Mô hình A): **Nhà xuất bản > Rating > Mức giảm giá > Giá bán > Thể loại > Số trang**.

## 📈 Dashboard Power BI

4 trang tương tác: **Overview** (tổng quan kinh doanh & Pareto) · **Thể loại và NXB** (hiệu suất theo danh mục) · **Doanh thu** (phân tích theo giá bán/NXB) · **Đánh giá và review của khách hàng**.

## 🛠️ Công nghệ sử dụng

- **Python** - pandas, scikit-learn, matplotlib (làm sạch dữ liệu, EDA, Machine Learning)
- **Power BI** - DAX, Power Query (dashboard vận hành)
- **Google Colab** - môi trường phân tích chính

## 📁 Cấu trúc dự án (Google Drive)

Toàn bộ dữ liệu gốc, notebook, file Power BI (`.pbix`), slide thuyết trình (`.pptx`), script thuyết trình và tài liệu chuẩn bị phản biện được lưu trong thư mục Drive ở link phía trên.

## 👤 Tác giả

**Nguyễn Việt Anh**
— Rikkei Education x HUST, 2026
