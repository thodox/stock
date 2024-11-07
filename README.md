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





## Kết quả

## Dự đoán trên LSTM
![image](https://github.com/user-attachments/assets/fd640191-357a-47fc-8a86-ca9ab80eb107)





