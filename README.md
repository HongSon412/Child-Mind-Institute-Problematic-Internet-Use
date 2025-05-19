# Dự Đoán Sử Dụng Internet Có Vấn Đề ở Trẻ Em Bằng Dữ Liệu Thể Chất và Thể Dục

Dự án này nhằm phát triển một mô hình học máy để dự đoán việc sử dụng internet có vấn đề ở trẻ em và thanh thiếu niên bằng cách sử dụng dữ liệu thể chất và thể dục. Bằng cách tận dụng các phép đo thể chất dễ dàng thu thập, mô hình này xác định các dấu hiệu sớm của việc sử dụng internet có vấn đề, cho phép can thiệp kịp thời để thúc đẩy các thói quen kỹ thuật số lành mạnh hơn.

## Bối Cảnh

Việc sử dụng internet có vấn đề ở trẻ em và thanh thiếu niên ngày càng phổ biến trong thời đại kỹ thuật số, thường liên quan đến các thách thức sức khỏe tâm thần như trầm cảm và lo âu. Các phương pháp đánh giá truyền thống phức tạp, đòi hỏi chuyên môn chuyên nghiệp, gây ra các rào cản về khả năng tiếp cận, văn hóa và ngôn ngữ cho nhiều gia đình. Ngược lại, các phép đo thể chất và thể dục—như tư thế, chế độ ăn uống và mức độ hoạt động—dễ dàng tiếp cận và có thể dùng làm chỉ số để phát hiện việc sử dụng công nghệ quá mức. Dự án này khám phá các chỉ số này để cung cấp một giải pháp có thể mở rộng nhằm xác định việc sử dụng internet có vấn đề, đặc biệt là ở những nơi thiếu nguồn lực.

## Bộ Dữ Liệu

Bộ dữ liệu được lấy từ cuộc thi của Viện Tâm Trí Trẻ Em về việc sử dụng internet có vấn đề. Nó bao gồm:

- **Đặc Điểm Thể Chất**: BMI, chiều cao, cân nặng, v.v.
- **Khía Cạnh Hành Vi**: Mô hình sử dụng internet.
- **Dữ Liệu Thể Dục**: Thời gian chịu đựng, nhịp tim, v.v.
- **Dữ Liệu Chuỗi Thời Gian**: Dữ liệu actigraphy được xử lý thành các đặc trưng thống kê.

Dữ liệu được chia thành tập huấn luyện và tập kiểm tra, với dữ liệu chuỗi thời gian bổ sung được hợp nhất để tăng cường khả năng dự đoán.

## Phương Pháp

Quy trình học máy trong dự án được thực hiện như sau:

- **Kỹ Thuật Đặc Trưng**:
  - Lựa chọn các đặc trưng thể chất, hành vi và thể dục liên quan.
  - Mã hóa các biến phân loại (ví dụ: mùa) thành giá trị số.
  - Tổng hợp các thống kê chuỗi thời gian (trung bình, độ lệch chuẩn) từ dữ liệu actigraphy.
  - Tạo các đặc trưng phái sinh (ví dụ: `BMI_Age`, `Internet_Hours_Age`).

- **Tiền Xử Lý Dữ Liệu**:
  - Xử lý giá trị thiếu bằng cách nội suy KNN.
  - Chuẩn hóa và điều chỉnh tỷ lệ cho tương thích với mô hình.

- **Huấn Luyện Mô Hình**:
  - Sử dụng nhiều mô hình hồi quy: LightGBM, XGBoost, CatBoost, RandomForest và GradientBoosting.
  - Áp dụng kỹ thuật ensemble, bao gồm VotingRegressor, để kết hợp các dự đoán.
  - Điều chỉnh siêu tham số để đạt hiệu suất tối ưu.

- **Đánh Giá**:
  - Xác thực chéo Stratified K-Fold (5 fold).
  - Sử dụng điểm Quadratic Weighted Kappa (QWK) làm thước đo đánh giá.
  - Tối ưu hóa ngưỡng để tối đa hóa QWK.

## Kết Quả

Các mô hình được đánh giá bằng xác thực chéo 5 fold, với hiệu suất được đo bằng điểm QWK:

| Mô Hình                | QWK Xác Thực Trung Bình | QWK Tối Ưu Hóa |
|-----------------------|-------------------------|----------------|
| LightGBM             | 0.4078                 | 0.445          |
| CatBoost             | 0.3602                 | 0.457          |
| XGBoost              | 0.3913                 | 0.434          |
| VotingRegressor      | 0.3893                 | 0.449          |
| Ensemble (Tất Cả Mô Hình) | 0.3785            | 0.448          |

Bản nộp cuối cùng sử dụng biểu quyết đa số từ các dự đoán của mô hình để xác định điểm `sii` (Mức Độ Nghiêm Trọng của Vấn Đề Internet).

## Cách Sử Dụng

Để chạy mã nguồn và tái tạo kết quả, làm theo các bước sau:

1. **Các Phụ Thuộc**:
   - Cài đặt các thư viện Python cần thiết: `numpy`, `pandas`, `scikit-learn`, `lightgbm`, `xgboost`, `catboost`, `torch`, `polars`, `matplotlib`, `seaborn`.
   - Khuyến nghị sử dụng GPU để huấn luyện nhanh hơn (được cấu hình trong tham số mô hình).

2. **Dữ Liệu**:
   - Tải bộ dữ liệu từ cuộc thi của Viện Tâm Trí Trẻ Em trên Kaggle.
   - Đặt các tệp vào các thư mục thích hợp:
     - `train.csv`, `test.csv`, `sample_submission.csv` trong thư mục gốc.
     - Dữ liệu chuỗi thời gian trong `series_train.parquet` và `series_test.parquet`.

3. **Chạy Notebook**:
   - Mở tệp `lb0-494-with-tabnet-ds102.ipynb` trong Jupyter Notebook hoặc môi trường tương thích.
   - Thực thi các ô theo thứ tự để:
     - Tải và tiền xử lý dữ liệu.
     - Huấn luyện các mô hình.
     - Tạo dự đoán.

4. **Kết Quả**:
   - Tệp `submission.csv` sẽ được tạo với các giá trị `sii` dự đoán cho tập kiểm tra.

## Kết Luận

Dự án này chứng minh tiềm năng của việc sử dụng dữ liệu thể chất và thể dục làm chỉ số để dự đoán việc sử dụng internet có vấn đề ở trẻ em. Ensemble của các mô hình học máy đạt được hiệu suất dự đoán hợp lý, với điểm QWK tối ưu hóa khoảng 0.45. Các cải tiến trong tương lai có thể bao gồm:

- Kỹ thuật đặc trưng nâng cao.
- Tích hợp các nguồn dữ liệu bổ sung.
- Khám phá các phương pháp học sâu.

Chúng tôi hoan nghênh các đóng góp cho kho lưu trữ này để tiếp tục nâng cao mô hình và thúc đẩy các thói quen kỹ thuật số lành mạnh hơn ở trẻ em!
