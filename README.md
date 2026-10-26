# BÁO CÁO PHÂN TÍCH DỮ LIỆU NHÀ HÀNG (TIPS DATASET)

> **Học phần:** Khoa học Dữ liệu trong Kinh tế & Kinh doanh  
> **Ngôn ngữ & Thư viện:** Python 3 | Pandas  
> **Phương pháp luận:** Quy trình chuẩn CRISP-DM (Cross-Industry Standard Process for Data Mining)  
> **Bộ dữ liệu:** `tips.csv`

---

## MỤC LỤC
1. [Giới thiệu & Mục tiêu nghiệp vụ (Business Understanding)](#1-giới-thiệu--mục-tiêu-nghiệp-vụ)
2. [Thiết lập môi trường & Nạp dữ liệu](#2-thiết-lập-môi-trường--nạp-dữ-liệu)
3. [Câu 1: Khai phá dữ liệu cơ bản (Data Understanding)](#3-câu-1-khai-phá-dữ-liệu-cơ-bản)
4. [Câu 2: Phân tích hóa đơn theo thời gian ăn & số lượng người](#4-câu-2-phân-tích-hóa-đơn-theo-thời-gian-ăn--số-lượng-người)
5. [Câu 3: Phân tích so sánh khách hút thuốc và không hút thuốc](#5-câu-3-phân-tích-so-sánh-khách-hút-thuốc-và-không-hút-thuốc)
6. [Câu 4: Đánh giá khách hàng vào các ngày cuối tuần](#6-câu-4-đánh-giá-khách-hàng-vào-các-ngày-cuối-tuần)
7. [Kết luận & Đề xuất giải pháp kinh doanh](#7-kết-luận--đề-xuất-giải-pháp-kinh-doanh)
8. [Cấu trúc repository & Hướng dẫn thực thi](#8-cấu-trúc-repository--hướng-dẫn-thực-thi)

---

## 1. Giới thiệu & Mục tiêu nghiệp vụ

Dự án phân tích dữ liệu kinh doanh được triển khai dựa trên **quy trình chuẩn CRISP-DM**. Thay vì chỉ lập trình đơn thuần, dự án tập trung vào tư duy phân tích: đặt câu hỏi kinh doanh, thấu hiểu dữ liệu, phát hiện quy luật và diễn giải kết quả nhằm hỗ trợ nhà quản lý tối ưu hóa vận hành nhà hàng.

### Mục tiêu phân tích cụ thể:
* **Khám phá tổng quan:** Đánh giá quy mô, tính toàn vẹn và các phân phối thống kê cốt lõi của dữ liệu.
* **Tối ưu hóa công suất phục vụ:** Đo lường sự khác biệt về hóa đơn giữa các bữa ăn (Trưa/Tối) và quy mô bàn ăn để sắp xếp nhân sự, bàn ghế.
* **Phân khúc hành vi:** So sánh nhóm khách hút thuốc và không hút thuốc về mức chi tiêu và thói quen trả tiền boa.
* **Chiến lược cuối tuần:** Xác định phân khúc khách hàng chủ lực (Nam/Nữ) và khung giờ vàng vào Thứ Bảy & Chủ Nhật để thiết kế chiến dịch marketing phù hợp.

---

## 2. Thiết lập môi trường & Nạp dữ liệu

Theo quy chuẩn của thư viện Pandas, dữ liệu dạng bảng được đọc từ tệp CSV trực tuyến về cấu trúc **DataFrame**:

```python
import pandas as pd

# Đường dẫn nạp bộ dữ liệu
url = "https://raw.githubusercontent.com/mwaskom/seaborn-data/master/tips.csv"
df = pd.read_csv(url)

# Kiểm tra trực quan 5 dòng đầu tiên
df.head()
```

---

## 3. Câu 1: Khai phá dữ liệu cơ bản

### 3.1. Mã nguồn Python thực hiện

```python
# 1. Kiểm tra kích thước bảng dữ liệu (thuộc tính .shape trả về số dòng, số cột)
print("Kích thước bảng dữ liệu (dòng, cột):", df.shape)

# 2. Xem cấu trúc dữ liệu, kiểu dữ liệu từng cột và số lượng bản ghi hợp lệ
df.info()

# 3. Kiểm tra số lượng ô bị khuyết thiếu (NaN/Null) trên từng trường
print("\nSố lượng giá trị khuyết thiếu:")
print(df.isna().sum())

# 4. Thống kê mô tả các đại lượng thống kê cốt lõi cho các biến định lượng
df.describe()
```

### 3.2. Bảng kết quả thống kê mô tả (`df.describe()`)

| Đại lượng | `total_bill` (Tổng hóa đơn) | `tip` (Tiền boa) | `size` (Số người/bàn) |
| :--- | :---: | :---: | :---: |
| **count** (Số lượng) | 244.00 | 244.00 | 244.00 |
| **mean** (Trung bình) | $19.79 | $3.00 | 2.57 |
| **std** (Độ lệch chuẩn) | $8.90 | $1.38 | 0.95 |
| **min** (Nhỏ nhất) | $3.07 | $1.00 | 1.00 |
| **25%** (Tứ phân vị 1) | $13.35 | $2.00 | 2.00 |
| **50%** (Trung vị) | $17.80 | $2.90 | 2.00 |
| **75%** (Tứ phân vị 3) | $24.13 | $3.56 | 3.00 |
| **max** (Lớn nhất) | $50.81 | $10.00 | 6.00 |

### 3.3. Nhận xét phân tích
* **Quy mô & Cấu trúc:** Tập dữ liệu gồm **244 quan sát (dòng)** và **7 thuộc tính (cột)**. Trong đó gồm 3 biến định lượng (`total_bill`, `tip`, `size`) và 4 biến định tính phân loại (`sex`, `smoker`, `day`, `time`).
* **Độ sạch của dữ liệu:** Bảng dữ liệu hoàn toàn toàn vẹn, **không có giá trị khuyết thiếu nào (`isna().sum() = 0` trên toàn bộ các cột)**, sẵn sàng cho các phân tích tiếp theo mà không cần xử lý điền khuyết hay loại bỏ dòng lỗi.
* **Phân phối giá trị hóa đơn:** Giá trị trung bình (`mean = $19.79`) lớn hơn trung vị (`median = $17.80`), kết hợp giá trị cực đại lên tới `$50.81` cho thấy phân phối chi tiêu có xu hướng **lệch phải**. Phần lớn các bàn ăn chi tiêu trong khoảng `$13.35 - $24.13`, và có một số ít bàn chi tiêu cao vượt trội kéo trung bình lên.
* **Quy mô phục vụ:** Bàn **2 người** là quy mô phổ biến nhất (chiếm hơn 60% tổng số lượt khách của nhà hàng).

---

## 4. Câu 2: Phân tích hóa đơn theo thời gian ăn & số lượng người

### 4.1. Mã nguồn Python thực hiện

```python
# 1. Thống kê hóa đơn theo bữa ăn (time)
df_time = df.groupby('time')['total_bill'].agg(['count', 'mean', 'median', 'std'])
print("--- Hóa đơn theo bữa ăn ---")
print(df_time)

# 2. Thống kê hóa đơn theo số lượng người (size)
df_size = df.groupby('size')['total_bill'].agg(['count', 'mean', 'median'])
print("\n--- Hóa đơn theo số lượng người ---")
print(df_size)

# 3. Phân tích kết hợp đa chiều bằng groupby và unstack
df_time_size = df.groupby(['time', 'size'])['total_bill'].mean().unstack()
print("\n--- Hóa đơn trung bình theo Bữa ăn x Quy mô bàn ---")
print(df_time_size)
```

### 4.2. Bảng kết quả tổng hợp

#### a) Theo thời gian ăn (`time`):
| Bữa ăn (`time`) | Số lượng đơn (`count`) | Hóa đơn trung bình (`mean`) | Hóa đơn trung vị (`median`) | Độ lệch chuẩn (`std`) |
| :--- | :---: | :---: | :---: | :---: |
| **Lunch** (Trưa) | 68 | $17.17 | $15.97 | $7.71 |
| **Dinner** (Tối) | 176 | $20.80 | $18.39 | $9.14 |

#### b) Ma trận hóa đơn trung bình (`time` x `size`):
| Bữa ăn / Quy mô | 1 người | 2 người | 3 người | 4 người | 5 người | 6 người |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Lunch** | $9.33 | $14.89 | $18.52 | $29.95 | $41.19 | $30.38 |
| **Dinner** | $5.16 | $17.23 | $24.00 | $28.40 | $27.29 | $48.17 |

### 4.3. Nhận xét phân tích
* **Khung giờ ăn uống:** Dữ liệu thực tế chỉ ghi nhận 2 bữa: **Trưa (`Lunch`)** và **Tối (`Dinner`)** (không có dữ liệu bữa sáng).
* **Chênh lệch giữa các bữa ăn:**
  * **Buổi tối là thời điểm tạo doanh thu chủ lực:** Số lượng bàn ăn buổi tối (**176 đơn**) gấp 2.6 lần buổi trưa (**68 đơn**). Mức chi tiêu trung bình buổi tối đạt **$20.80**, cao hơn 21% so với bữa trưa (**$17.17**). Điều này phản ánh tâm lý khách hàng buổi tối thường thoải mái thời gian hơn, gọi thêm món khai vị, tráng miệng và đồ uống có cồn.
* **Tác động của quy mô bàn (`size`):**
  * Hóa đơn **tỷ lệ thuận rõ nét với số người**: bàn 1 người chi trung bình ~$7.24, bàn 2 người ~$16.45, bàn 4 người ~$28.61, và bàn 6 người lên đến ~$34.83.
  * Bàn 2 người là phân khúc đông nhất (156 đơn), tiếp theo là bàn 3 và 4 người.

---

## 5. Câu 3: Phân tích so sánh khách hút thuốc và không hút thuốc

### 5.1. Mã nguồn Python thực hiện

```python
# Tạo cột tính tỷ lệ phần trăm tiền tip trên tổng hóa đơn để so sánh chuẩn xác
df['tip_pct'] = (df['tip'] / df['total_bill']) * 100

# So sánh toàn diện 2 nhóm smoker
so_sanh_smoker = df.groupby('smoker')[['total_bill', 'tip', 'tip_pct']].agg({
    'total_bill': ['count', 'mean', 'std'],
    'tip': ['mean', 'std'],
    'tip_pct': ['mean', 'median']
})
print(so_sanh_smoker)
```

### 5.2. Bảng kết quả so sánh

| Nhóm khách (`smoker`) | Số lượng (`count`) | Hóa đơn TB (`total_bill mean`) | Độ lệch chuẩn HĐ (`std`) | Tiền tip TB (`tip mean`) | Tỷ lệ tip TB (`tip_pct mean`) | Tỷ lệ tip trung vị (`tip_pct median`) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **No** (Không hút thuốc) | 151 | $19.19 | $8.26 | $2.99 | 15.93% | 15.56% |
| **Yes** (Có hút thuốc) | 93 | $20.76 | $9.83 | $3.01 | 16.32% | 15.38% |

### 5.3. Nhận xét phân tích
* **Về giá trị hóa đơn:** Nhóm khách **có hút thuốc chi tiêu trung bình cao hơn** ($20.76 so với $19.19). Tuy nhiên, độ lệch chuẩn của nhóm này cao hơn đáng kể ($9.83 so với $8.26), cho thấy mức chi của nhóm hút thuốc có độ phân tán lớn: có khách chi rất ít nhưng cũng có khách gọi những bữa tiệc giá trị rất cao.
* **Về tiền tip và tỷ lệ boa:**
  * Tiền tip trung bình tuyệt đối giữa 2 nhóm gần như **tương đương nhau** (~$3.00).
  * Tỷ lệ phần trăm boa (`tip_pct`) của cả hai nhóm đều xoay quanh mức chuẩn ngành là **15.5% – 16.3%**. Điều này chứng minh thói quen boa tiền gắn liền với giá trị hóa đơn hơn là sở thích hút thuốc.

---

## 6. Câu 4: Đánh giá khách hàng vào các ngày cuối tuần

### 6.1. Mã nguồn Python thực hiện

```python
# Bước 1: Áp dụng quy tắc lọc nhiều điều kiện trong Pandas (dùng toán tử '|' và bọc trong ngoặc đơn)
mask_cuoi_tuan = (df['day'] == 'Sat') | (df['day'] == 'Sun')
df_cuoi_tuan = df[mask_cuoi_tuan]

# Bước 2: Kiểm tra tần suất các bữa ăn vào ngày cuối tuần
print("--- Phân bố bữa ăn vào cuối tuần ---")
print(df_cuoi_tuan['time'].value_counts())

# Bước 3: So sánh chi tiêu giữa Nam và Nữ vào ngày cuối tuần
chi_tieu_cuoi_tuan = df_cuoi_tuan.groupby('sex')['total_bill'].agg(
    So_luong_don='count',
    Chi_tieu_trung_binh='mean',
    Tong_doanh_thu='sum'
)
print("\n--- Tổng hợp chi tiêu theo giới tính vào cuối tuần ---")
print(chi_tieu_cuoi_tuan)
```

### 6.2. Bảng kết quả tổng hợp cuối tuần

#### a) Phân bố bữa ăn cuối tuần:
* **Dinner (Bữa tối):** 163 đơn (87 đơn vào Thứ Bảy, 76 đơn vào Chủ Nhật).
* **Lunch (Bữa trưa):** 0 đơn (hoàn toàn không có khách vào buổi trưa).

#### b) So sánh chi tiêu theo giới tính vào cuối tuần:
| Giới tính (`sex`) | Số lượng đơn (`count`) | Chi tiêu trung bình (`mean`) | Tổng doanh thu (`sum`) | Tỷ trọng doanh thu |
| :--- | :---: | :---: | :---: | :---: |
| **Female** (Nữ) | 46 đơn | $19.76 | $908.75 | 26.68% |
| **Male** (Nam) | 117 đơn | $21.34 | $2,496.81 | 73.32% |
| **Tổng cộng** | **163 đơn** | **$20.89** | **$3,405.56** | **100.0%** |

### 6.3. Nhận xét phân tích
* **Thói quen thời gian:** **100% khách hàng cuối tuần đi ăn vào buổi tối (`Dinner`)**. Quán ăn đóng cửa hoặc chỉ phục vụ thực đơn tối vào Thứ Bảy và Chủ Nhật.
* **Chi tiêu theo giới tính:**
  * **Khách hàng Nam giới là đối tượng chi trả áp đảo toàn diện:**
    * Chiếm **117 trên tổng số 163 đơn** (~71.8% số lượng đơn).
    * Đóng góp **$2,496.81** trên tổng số $3,405.56 doanh thu cuối tuần (~73.3% tổng doanh thu).
    * Mức chi trung bình mỗi đơn của Nam giới đạt **$21.34**, cao hơn khách hàng Nữ (**$19.76**).

---

## 7. Kết luận & Đề xuất giải pháp kinh doanh

Áp dụng mô hình **Kể chuyện bằng dữ liệu (Bối cảnh - Phát hiện - Hành động)** từ học phần:

1. **Bố trí nhân sự và vận hành:**
   * Dồn trọng tâm ca làm việc và nhân viên phục vụ vào **buổi tối**, đặc biệt là tối Thứ Bảy và Chủ Nhật.
   * Ưu tiên sắp xếp sơ đồ bàn ăn dành cho **bàn 2 người và bàn 4 người** vì đây là các nhóm khách chiếm tỷ trọng doanh thu cao nhất.
2. **Chiến lược tiếp thị & Thực đơn:**
   * Thiết kế các gói combo món ăn, đồ uống hoặc set tiệc gia đình/nhóm bạn vào cuối tuần nhắm trực tiếp vào đối tượng **khách hàng Nam giới**.
   * Với nhóm khách hút thuốc có mức chi tiêu cao nhưng biến động lớn, quán có thể bố trí khu vực ngồi ngoài trời (outdoor) tiện nghi để vừa gia tăng trải nghiệm khách hàng, vừa tối đa hóa doanh thu từ nhóm khách này.

---

## 8. Cấu trúc repository & Hướng dẫn thực thi

### 8.1. Cấu trúc thư mục repository
```text
tips-data-analysis/
├── README.md               # Báo cáo phân tích chi tiết (Markdown)
└── tips_analysis.py        # Mã nguồn Python xử lý toàn bộ phân tích
```

### 8.2. Hướng dẫn chạy mã nguồn
Chạy trực tiếp tệp phân tích bằng dòng lệnh:
```bash
python tips_analysis.py
```
