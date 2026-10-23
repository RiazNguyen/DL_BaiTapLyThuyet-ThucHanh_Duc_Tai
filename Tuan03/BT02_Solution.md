# GIẢI BÀI TẬP 02 - PHÂN TÍCH KHÁM PHÁ VÀ TIỀN XỬ LÝ DỮ LIỆU
(Dựa trên tài liệu *Sridhar - Chapter 2 - Data Understanding*)

## Câu 1.
**Câu hỏi 1 (Data Overview):**
- **Số lượng mẫu:** 10
- **Số thuộc tính:** 6 (ID, Tuổi, Lương, Điểm, Giới tính, Mua hàng)
- **Kiểu dữ liệu:**
  + ID: Định danh (Categorical / Nominal)
  + Tuổi: Định lượng (Numerical / Continuous hoặc Discrete)
  + Lương: Định lượng (Numerical / Continuous)
  + Điểm: Định lượng (Numerical / Continuous)
  + Giới tính: Phân loại (Categorical / Nominal)
  + Mua hàng: Phân loại (Categorical / Nominal)

**Câu hỏi 2 (Descriptive Statistics):**
(Loại bỏ các giá trị thiếu `?` để tính toán)
- **Tuổi** (9 mẫu hợp lệ): Mean ≈ 32.11, Median = 30, Mode = Không có mode rõ ràng do tất cả các giá trị duy nhất đều xuất hiện 1 lần.
- **Lương** (9 mẫu hợp lệ): Mean ≈ 9722.22, Median = 8000, Mode = Không có mode rõ ràng.
- **Điểm** (9 mẫu hợp lệ): Mean ≈ 7.76, Median = 7.8, Mode = Không có mode rõ ràng.

**Câu hỏi 3 (Distribution Analysis):**
- **Histogram (Tuổi) và Boxplot (Lương):** Histogram tuổi sẽ có trục x là các khoảng tuổi, trục y là tần số. Boxplot lương sẽ có một box từ Q1 đến Q3, với đường gạch ở giữa là Median, râu kéo dài và các điểm ngoại lệ (outlier).
- **Nhận xét dạng phân phối:**
  + Tuổi: Mean (32.11) > Median (30) -> Phân phối lệch phải (Positive Skew).
  + Lương: Mean (9722.22) > Median (8000) -> Phân phối lệch phải (Positive Skew).
- **Outlier:** Lương có outlier rất rõ là `30000` (giá trị quá lớn) và `-2000` (giá trị bất thường, lương không thể âm). Tuổi có giá trị `50` khá cao nhưng chưa thực sự là outlier cực đoan.

**Câu hỏi 4 (Relationship Analysis):**
- **Scatter plot:** Trục hoành là Tuổi, trục tung là Lương.
- **Xu hướng:** Xu hướng tương quan **dương** (Positive). Tuổi càng cao, lương có xu hướng càng cao (ví dụ: tuổi 50 lương 30000, tuổi 40 lương 15000).
- **Phát hiện outlier:** Dữ liệu có (35, -2000) là outlier bất thường và (50, 30000) là giá trị cực trị.
- **Đề xuất tiền xử lý:** Xử lý điểm dữ liệu bị lỗi (-2000 có thể là do đánh máy sai, cần thay thế bằng NaN hoặc giá trị tuyệt đối). Điền khuyết cho các giá trị `?`.

**Câu hỏi 5 (EDA - Missing Data):**
- **Các giá trị thiếu:**
  + Tuổi: 1 missing (Mẫu ID=6) -> Tỷ lệ 10%
  + Lương: 1 missing (Mẫu ID=2) -> Tỷ lệ 10%
  + Điểm: 1 missing (Mẫu ID=9) -> Tỷ lệ 10%
- **Ảnh hưởng:** Tỷ lệ missing của từng cột thấp (10%), nhưng dataset rất nhỏ (10 dòng) nên ảnh hưởng chung đối với toàn bộ dữ liệu khá lớn.
- **Đề xuất:** Thay thế (Imputation) bằng Median cho Lương và Tuổi để tránh bị ảnh hưởng bởi outlier.

**Câu hỏi 6 (Missing Data - Processing):**
- **Vị trí và số lượng:** 1 vị trí mỗi cột (ID 6 cho Tuổi, ID 2 cho Lương, ID 9 cho Điểm).
- **Áp dụng:**
  + Phương pháp 1 (Mean): Điền Tuổi=32.11, Lương=9722.22, Điểm=7.76
  + Phương pháp 2 (Median): Điền Tuổi=30, Lương=8000, Điểm=7.8
- **So sánh:** Chọn **Median** vì thuộc tính Lương đang có chứa giá trị bất thường (outlier: 30000, -2000), khiến Mean bị kéo lệch (Mean = 9722 > Median = 8000). Median sẽ an toàn và đại diện tốt hơn cho trung tâm dữ liệu.

**Câu hỏi 7 (Data Quality):**
- **Giá trị không hợp lý:**
  + Lương `-2000` (sai miền giá trị, lương phải >= 0).
- **Cách xử lý:** Thay thế giá trị `-2000` thành giá trị khuyết (Missing) và áp dụng phương pháp điền khuyết bằng Median, hoặc lấy trị tuyệt đối (nếu giả định là lỗi nhập liệu gõ dư dấu trừ).

**Câu hỏi 8 (Outlier Detection):**
* Áp dụng phương pháp IQR cho Lương:
  - Q1 (25%) = 7000, Q3 (75%) = 8500 => IQR = 1500
  - Cận dưới = Q1 - 1.5*IQR = 7000 - 2250 = 4750
  - Cận trên = Q3 + 1.5*IQR = 8500 + 2250 = 10750
  - Các giá trị outlier là các giá trị nằm ngoài [4750, 10750] bao gồm: **-2000, 15000, 30000**.

**Câu hỏi 9 (Normalization):**
- **Z-score (Lương):** Z = (X - Mean) / Std. Với Mean ≈ 9722, Std ≈ 8268.
- **Nhận xét:** Dữ liệu sẽ được dời về trung bình (mean) là 0 và độ lệch chuẩn (std) là 1. Các giá trị bất thường sẽ có Z-score rất lớn (ví dụ 30000 sẽ có Z ≈ 2.45) hoặc nhỏ (ví dụ -2000 có Z ≈ -1.42). Sự thay đổi này giúp đưa dữ liệu về cùng thang chuẩn, dễ dàng đưa vào các thuật toán học máy mà không làm biến dạng hình dáng phân phối gốc.

**Câu hỏi 10 (Encoding):**
- Giới tính (One-hot): Nam -> `[1, 0]`, Nữ -> `[0, 1]`.
- Mua hàng (Binary): Yes -> `1`, No -> `0`.
- Kết quả (Ví dụ 3 dòng đầu):
  + ID 1: Nam, Yes -> Giới tính: `[1, 0]`, Mua hàng: `1`
  + ID 2: Nữ, No -> Giới tính: `[0, 1]`, Mua hàng: `0`
  + ID 3: Nam, Yes -> Giới tính: `[1, 0]`, Mua hàng: `1`
*(Làm tương tự cho các mẫu còn lại).*

**Câu hỏi 11 (Feature Selection):**
- Nên loại bỏ: `ID` vì chỉ là định danh duy nhất, không có ý nghĩa phân loại hay dự đoán.
- Nên giữ: `Tuổi`, `Lương`, `Giới tính`, `Điểm` vì đây là các thuộc tính mang thông tin nhân khẩu học và năng lực, có thể ảnh hưởng trực tiếp đến nhãn quyết định "Mua hàng".

**Câu hỏi 12 (Data Reduction):**
- Đề xuất: **Principal Component Analysis (PCA)**.
- Giải thích: PCA thường được áp dụng khi số lượng thuộc tính trong dataset rất lớn (hàng trăm cột) để giảm thiểu chiều dữ liệu mà không mất quá nhiều thông tin cốt lõi (Variance). Với dataset hiện tại chỉ có 5 thuộc tính, PCA không thực sự cần thiết, nhưng có thể sử dụng nếu cần đưa dữ liệu về không gian 2D/3D để dễ vẽ đồ thị phân tích.

---

## Câu 2.
**Tập dữ liệu Tuổi đã sắp xếp (N = 27):**
`13, 15, 16, 16, 19, 20, 20, 21, 22, 22, 25, 25, 25, 25, 30, 33, 33, 35, 35, 35, 35, 36, 40, 45, 46, 52, 70`

**a) Tính Mean, Median, Mode:**
- Mean (Trung bình): `(13 + 15 + ... + 70) / 27 ≈ 29.96`
- Median (Trung vị): `25` (phần tử thứ 14 ở giữa)
- Mode: Tập dữ liệu này là đa mode (Bimodal). Mode là `25` và `35` (đều xuất hiện nhiều nhất, 4 lần).

**b) Xác định tứ phân vị thứ nhất (Q1) và tứ phân vị thứ ba (Q3):**
- N = 27 (lẻ), trung vị ở vị trí 14 là 25.
- Q1 là trung vị của nửa đầu (13 phần tử đầu): `20` (phần tử thứ 7).
- Q3 là trung vị của nửa sau (13 phần tử cuối): `35` (phần tử thứ 21).

**c) Vẽ Boxplot và nhận xét phân bố:**
- **Thông số Boxplot (5-point summary):** Min = 13, Q1 = 20, Median = 25, Q3 = 35, Max = 70. IQR = 15.
- **Nhận xét:** Phân phối bị lệch phải (Positive skew). Điều này thể hiện qua việc khoảng cách từ Median đến Q3 (10 đơn vị) lớn hơn khoảng cách từ Q1 đến Median (5 đơn vị), và đuôi bên phải kéo dài ra đến 70. Giá trị 70 là ngoại lệ (Outlier) vì lớn hơn cận trên của Boxplot (Q3 + 1.5*IQR = 35 + 22.5 = 57.5).

**d) Rời rạc hóa dữ liệu với k = 4:**
1. **Phương pháp chia đều (Equal-width):**
   - Bề rộng khoảng (Width) = `(Max - Min) / 4 = (70 - 13) / 4 = 14.25`
   - Bin 1 `[13.0, 27.25)`: 13, 15, 16, 16, 19, 20, 20, 21, 22, 22, 25, 25, 25, 25 *(14 mẫu)*
   - Bin 2 `[27.25, 41.5)`: 30, 33, 33, 35, 35, 35, 35, 36, 40 *(9 mẫu)*
   - Bin 3 `[41.5, 55.75)`: 45, 46, 52 *(3 mẫu)*
   - Bin 4 `[55.75, 70.0]`: 70 *(1 mẫu)*

2. **Phương pháp tần số đều (Equal-frequency):**
   - Số lượng mẫu mỗi bin = `27 / 4 ≈ 6.75` (chia thành các nhóm khoảng 7 mẫu).
   - Bin 1: 13, 15, 16, 16, 19, 20, 20 *(7 mẫu)*
   - Bin 2: 21, 22, 22, 25, 25, 25, 25 *(7 mẫu)*
   - Bin 3: 30, 33, 33, 35, 35, 35, 35 *(7 mẫu)*
   - Bin 4: 36, 40, 45, 46, 52, 70 *(6 mẫu)*

---

## Câu 3.
*(Theo lý thuyết của Sridhar, các mức độ đo lường/Types of Data bao gồm: Nominal, Ordinal, Interval, Ratio)*

**Cho tập giá trị gốc `V(.) = {1, 2, 3, 4, 5}`**

Các biến đổi biểu diễn thang đo tương ứng:
1. **W1 = 100 * V** `(100, 200, 300, 400, 500)`: Tương ứng **Thang đo tỷ lệ (Ratio Scale)** vì phép biến đổi có dạng `y = ax (a > 0)`, bảo toàn tỷ lệ và gốc 0.
2. **W2 = V + 9** `(10, 11, 12, 13, 14)`: Tương ứng **Thang đo khoảng (Interval Scale)** vì phép biến đổi dạng `y = x + b`, bảo toàn khoảng cách nhưng điểm 0 (gốc) đã bị dịch chuyển.
3. **W3** `(8, 13, 45, 6, 7)`: Tương ứng **Thang đo định danh (Nominal Scale)** vì là phép ánh xạ ngẫu nhiên 1-1, không bảo toàn thứ tự hay khoảng cách nào.
4. **W4 = V^2** `(1, 4, 9, 16, 25)`: Tương ứng **Thang đo thứ bậc (Ordinal Scale)** vì đây là phép biến đổi đơn điệu tăng, bảo toàn được thứ tự nhưng khoảng cách giữa các phần tử không còn đồng đều.
5. **W5 = 2 * V - 12** `(-10, -8, -6, -4, -2)`: Tương ứng **Thang đo khoảng (Interval Scale)** vì phép biến đổi tuyến tính dạng `y = ax + b`, bảo toàn được tỷ lệ khoảng cách.
6. **W6 = 3 * V** `(3, 6, 9, 12, 15)`: Tương ứng **Thang đo tỷ lệ (Ratio Scale)** (dạng `y = ax (a > 0)`).

**Đại lượng nào biểu diễn cùng một thang đo với V(.)?**
Câu trả lời phụ thuộc vào bản chất gốc của dữ liệu `V(.)`:
- Nếu V(.) là *thang đo định danh*: Mọi phép biến đổi (W1 đến W6) đều biểu diễn cùng thang đo (vì tất cả đều là ánh xạ 1-1). Tuy nhiên `W3` là đại lượng duy nhất không thể xếp vào các thang đo cao hơn.
- Nếu V(.) là *thang đo thứ bậc*: Các đại lượng **W1, W2, W4, W5, W6** đều bảo toàn tính thứ tự nên biểu diễn cùng thang đo thứ bậc. `W4` là đặc trưng điển hình.
- Nếu V(.) là *thang đo khoảng*: Các đại lượng **W1, W2, W5, W6** bảo toàn tính tuyến tính nên biểu diễn cùng thang đo. Đặc trưng điển hình là `W2` và `W5`.
- Nếu V(.) là *thang đo tỷ lệ*: Chỉ có **W1** và **W6** bảo toàn tính tỷ lệ nên biểu diễn cùng một thang đo với V(.).
