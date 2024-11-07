# stock: Dự đoán giá cổ phiếu



# Ý tưởng:

## 1. Lý do chọn mô hình LSTM (Long Short-Term Memory)
LSTM là một biến thể của RNN (Recurrent Neural Network) được thiết kế đặc biệt để lưu giữ thông tin lâu hơn các RNN thông thường nhờ vào cấu trúc cell state và các cổng điều khiển việc lưu giữ hay quên thông tin. Điều này giúp LSTM hiệu quả trong việc xử lý chuỗi thời gian, khi có thể lưu trữ thông tin từ nhiều thời điểm trước, phát hiện các mẫu (patterns) dài hạn trong dữ liệu chuỗi như biến động giá cổ phiếu. Khả năng phát hiện xu hướng và dự đoán tương lai của LSTM dựa trên dữ liệu lịch sử khiến nó rất phù hợp cho bài toán dự đoán giá cổ phiếu, nơi cả yếu tố ngắn hạn và dài hạn đều quan trọng.

## 2. Cấu trúc dữ liệu sử dụng cho dự đoán
Dữ liệu bao gồm các đặc trưng chính liên quan đến giao dịch cổ phiếu:

- **Ticker**: mã cổ phiếu
- **Date/Time**: thời gian giao dịch
- **Open**: giá mở cửa của cổ phiếu
- **High** và **Low**: mức giá cao nhất và thấp nhất trong thời gian giao dịch
- **Close**: giá đóng cửa
- **Volume**: khối lượng giao dịch
- **Open Interest**: số lượng hợp đồng mở (chưa thanh toán)

Dữ liệu này đã được xử lý sẵn và không cần làm sạch thêm vì đã loại bỏ các nhiễu không cần thiết.

## 3. Mục tiêu của mô hình
Mô hình dự đoán giá cổ phiếu tại thời điểm **N khoảng thời gian sau** (có thể là phút, giờ, hoặc ngày) dựa trên các giá trị hiện tại và trước đó trong dữ liệu.

## 4. Các chỉ báo kỹ thuật được thêm vào mô hình
Để cải thiện hiệu quả dự đoán, một số chỉ báo kỹ thuật sẽ được sử dụng:

### EMA (Exponential Moving Average)
EMA là đường trung bình động lũy thừa, nhấn mạnh vào các giá trị gần nhất hơn so với MA thông thường:

- **EMA7** (7 đơn vị thời gian): Giúp phát hiện xu hướng ngắn hạn. Nếu giá nằm trên EMA7, đây là dấu hiệu của xu hướng tăng ngắn hạn và ngược lại, hỗ trợ việc xác định tín hiệu mua/bán.
- **EMA30** (30 đơn vị thời gian): Phát hiện xu hướng trung hạn, nhạy cảm với biến động gần đây, giúp mô hình hiểu động lực giá trung hạn của cổ phiếu.
- **EMA99** (99 đơn vị thời gian): EMA dài hạn, loại bỏ nhiễu ngắn hạn, giúp mô hình tập trung vào xu hướng bền vững, hữu ích để phát hiện các xu hướng dài hạn.

### RSI (Relative Strength Index)
RSI đo lường độ mạnh/yếu của xu hướng hiện tại bằng cách đánh giá tốc độ và thay đổi giá, với giá trị từ 0 đến 100:
- **Trên 70**: báo hiệu trạng thái quá mua (có thể điều chỉnh giảm).
- **Dưới 30**: báo hiệu trạng thái quá bán (có thể đảo chiều tăng giá).

Khi tích hợp vào LSTM, RSI giúp mô hình phát hiện các tín hiệu đảo chiều và hiểu rõ hơn về khả năng duy trì hoặc thay đổi xu hướng giá.

## 5. Tích hợp các chỉ báo vào LSTM
Việc kết hợp các chỉ báo này giúp LSTM tăng khả năng dự đoán nhờ:
- **Tăng cường thông tin về xu hướng**: Các chỉ báo MA và EMA giúp mô hình hiểu rõ hơn các xu hướng ngắn, trung, và dài hạn, thay vì chỉ dựa trên giá cổ phiếu đơn thuần.
- **Loại bỏ nhiễu**: EMA99 đặc biệt giúp lọc bớt các biến động ngắn hạn, giúp mô hình tập trung vào các xu hướng ổn định và bền vững hơn.
- **Tối ưu hóa dự đoán**: Với các chỉ báo này, LSTM có thể học được các mẫu phức tạp trong chuỗi thời gian, giúp dự đoán tốt hơn khi có nhiều yếu tố ảnh hưởng đến biến động giá.

## 6. Thử nghiệm mô hình Ensemble Learning - Random Forest
Ngoài LSTM, mô hình Ensemble Learning như **Random Forest** cũng sẽ được thử nghiệm. Tuy nhiên, do thiếu khả năng lưu trữ thông tin dài hạn, Random Forest có thể không hiệu quả bằng LSTM trong việc dự đoán các mẫu phức tạp và dài hạn trong dữ liệu chuỗi thời gian.

## 7. Nhận xét
LSTM có khả năng dự đoán tốt hơn tại bước nhảy thời gian dự đoán (như 60 phút) so với Random Forest nhờ khả năng lưu trữ thông tin dài hạn, giúp dự đoán chính xác hơn các biến động phức tạp trong chuỗi thời gian.





# Kết quả

Mô tả kết quả dự đoán của cổ phiếu PNJ, các cổ phiếu khác vui lòng xem trong file **.ipynb**

## Dự đoán trên LSTM của giá cổ phiếu PNJ
![image](https://github.com/user-attachments/assets/fd640191-357a-47fc-8a86-ca9ab80eb107)



**Phân Tích Biểu Đồ:**

Đường Xanh (Actual): Đây là giá cổ phiếu thực tế của PNJ từ giữa năm 2020 đến cuối năm 2020. Đường này phản ánh giá cổ phiếu thật và là cơ sở để đánh giá độ chính xác của mô hình dự đoán.

Đường Đỏ (Predicted): Đây là kết quả dự đoán từ mô hình LSTM (hoặc mô hình khác) của chúng tôi. Đường này cho thấy khả năng của mô hình trong việc dự đoán xu hướng giá cổ phiếu và được so sánh với giá thực tế.

**Độ Chính Xác của Mô Hình:**

Đường dự đoán (màu đỏ) bám sát đường thực tế (màu xanh), cho thấy mô hình dự đoán tốt xu hướng chung. Các sai lệch nhỏ là bình thường, vì thị trường có nhiều yếu tố khó dự đoán chính xác tuyệt đối.

Mô hình nhận diện tốt các biến động tăng giảm, đặc biệt trong giai đoạn tháng 10 đến tháng 12, khi giá cổ phiếu PNJ tăng mạnh. Kết quả cho thấy mô hình có khả năng dự đoán khá ổn định các biến động ngắn hạn.

**Kết Luận:**

Mô hình LSTM đạt độ chính xác cao trong việc dự đoán giá cổ phiếu PNJ trong khoảng thời gian này. Các biến động ngắn hạn mạnh hoặc đột ngột có thể chưa được dự đoán chính xác hoàn toàn, nhưng điều này có thể cải thiện với dữ liệu bổ sung hoặc điều chỉnh mô hình.












## Dự đoán trên RF của cổ phiếu PNJ
![image](https://github.com/user-attachments/assets/b1146758-8599-4323-98bd-85f29a09db65)

Đường Màu Xanh (Actual): Đây là đường biểu thị giá cổ phiếu PNJ thực tế từ giữa năm 2020 đến cuối năm 2020. Đường này cho thấy dữ liệu chuẩn để đánh giá độ chính xác của mô hình dự đoán.

Đường Màu Đỏ (Predicted): Đây là kết quả dự đoán giá cổ phiếu của mô hình Random Forest. Đường dự đoán này được so sánh trực tiếp với giá thực tế để đánh giá hiệu suất mô hình.

**Độ Phù Hợp Giữa Dự Đoán và Thực Tế**: Đường dự đoán (màu đỏ) bám sát đường thực tế (màu xanh) khá tốt, cho thấy mô hình có khả năng dự đoán xu hướng chung của giá cổ phiếu PNJ. Tuy có một số điểm mà đường dự đoán khác biệt so với thực tế, nhưng nhìn chung, các biến động chính của thị trường đã được mô hình dự đoán đúng.

**Biến Động Giá và Xu Hướng**: Mô hình đã nắm bắt được các xu hướng tăng giảm, đặc biệt là vào giai đoạn từ tháng 10 đến tháng 12, khi giá cổ phiếu tăng rõ rệt. Kết quả cho thấy mô hình dự đoán khá ổn định các biến động ngắn hạn của giá cổ phiếu.

**Kết Luận:** Biểu đồ này cho thấy mô hình Random Forest có hiệu quả trong việc dự đoán giá cổ phiếu PNJ trong giai đoạn trên. Mặc dù có một số sai lệch nhỏ, mô hình vẫn đảm bảo được khả năng dự đoán xu hướng tổng quan của thị trường.





# Hướng phát triển tương lai

Để tối ưu hóa và mở rộng khả năng của mô hình dự đoán giá cổ phiếu với LSTM, các hướng phát triển tiềm năng có thể đi như sau:

## 1. Nâng cao dữ liệu đầu vào
   - **Bổ sung các yếu tố kinh tế vĩ mô**: Thêm các biến số như lãi suất, lạm phát, GDP và các chỉ số kinh tế toàn cầu để giúp mô hình hiểu thêm về bối cảnh chung và dự đoán chính xác hơn.
   - **Dữ liệu mạng xã hội và tin tức**: Sử dụng dữ liệu tin tức tài chính hoặc phân tích tình cảm từ các mạng xã hội như Twitter để nắm bắt tâm lý thị trường, đặc biệt là khi có các sự kiện bất ngờ.

## 2. Cải thiện cấu trúc LSTM
   - **Sử dụng Bidirectional LSTM**: Cho phép mô hình học cả thông tin trong quá khứ và tương lai tiềm năng, tăng độ chính xác của dự đoán.
   - **Stacked LSTM**: Sử dụng LSTM nhiều lớp để mô hình học được các đặc điểm phức tạp hơn trong dữ liệu chuỗi thời gian, đặc biệt là các mối quan hệ phi tuyến tính.
   - **Attention Mechanism**: Thêm cơ chế Attention giúp mô hình tập trung vào các điểm dữ liệu quan trọng hơn, đặc biệt là khi có các sự kiện thị trường quan trọng.

## 3. Tích hợp các chỉ báo kỹ thuật nâng cao
   - **Moving Average Convergence Divergence (MACD)**: Bổ sung MACD để phát hiện điểm giao nhau giữa hai đường trung bình, từ đó xác định xu hướng đảo chiều.
   - **Bollinger Bands**: Dải Bollinger giúp mô hình hiểu rõ hơn về mức độ biến động của thị trường, đặc biệt là khi giá cổ phiếu có xu hướng quay lại mức trung bình.

## 4. Thử nghiệm thêm các mô hình khác
   - **Transformers**: Sử dụng Transformers, một mô hình tiên tiến với cơ chế Attention có khả năng nắm bắt mối quan hệ xa trong chuỗi dữ liệu, có thể cải thiện độ chính xác dự đoán so với LSTM.
   - **Hybrid Models**: Kết hợp giữa LSTM và các mô hình khác (như ARIMA hoặc Prophet) để tận dụng ưu điểm của cả hai trong xử lý chuỗi thời gian và các yếu tố mùa vụ.

## 5. Tối ưu hóa và điều chỉnh mô hình
   - **Hyperparameter Tuning**: Sử dụng Grid Search hoặc Bayesian Optimization để tối ưu hóa các siêu tham số (hyperparameters) cho LSTM, giúp tăng cường độ chính xác của mô hình.
   - **Feature Engineering tự động**: Sử dụng các kỹ thuật học sâu như AutoML để tìm ra các biến số mới hoặc tối ưu hóa cách xử lý dữ liệu đầu vào.

## 6. Mở rộng và áp dụng trong thực tế
   - **Ứng dụng mô hình vào các sản phẩm giao dịch tự động**: Tích hợp mô hình vào hệ thống giao dịch tự động để đưa ra các quyết định mua/bán cổ phiếu dựa trên dự đoán của mô hình.
   - **Kiểm tra và đánh giá qua dữ liệu thực tế**: Thực hiện kiểm tra trong thời gian thực (backtesting) để đánh giá hiệu quả của mô hình trong các tình huống thực tế và liên tục tinh chỉnh dựa trên phản hồi từ thị trường.




