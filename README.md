# stock: Dự đoán giá cổ phiếu



# Ý tưởng:

1. Lý do chọn mô hình LSTM (Long Short-Term Memory)
LSTM là một dạng RNN (Recurrent Neural Network) được thiết kế đặc biệt để lưu giữ thông tin lâu hơn so với các mô hình RNN thông thường, nhờ vào các thành phần như cell state và các cổng (gates) điều khiển việc giữ lại hay quên thông tin.
LSTM đặc biệt hiệu quả với chuỗi thời gian, bởi vì nó có thể lưu trữ và xử lý thông tin từ những thời điểm trước đó, nhờ đó phát hiện được các mẫu (patterns) dài hạn trong dữ liệu chuỗi, chẳng hạn như biến động giá cổ phiếu.
Khả năng của LSTM trong việc phát hiện xu hướng và dự đoán tương lai dựa trên dữ liệu lịch sử làm cho nó trở thành một lựa chọn phù hợp cho bài toán dự đoán giá cổ phiếu, nơi các yếu tố ngắn hạn và dài hạn đều quan trọng.
2. Cấu trúc dữ liệu sử dụng cho dự đoán
Dữ liệu bao gồm các đặc trưng chính liên quan đến giao dịch cổ phiếu:

Ticker: mã cổ phiếu.
Date/Time: thời gian giao dịch.
Open: giá mở cửa của cổ phiếu.
High và Low: mức giá cao nhất và thấp nhất trong thời gian giao dịch.
Close: giá đóng cửa.
Volume: khối lượng giao dịch.
Open Interest: số lượng hợp đồng mở (chưa thanh toán).
Dữ liệu này không cần làm sạch, vì đã được xử lý sẵn và loại bỏ các nhiễu không cần thiết.

3. Mục tiêu của mô hình
Dự đoán giá cổ phiếu tại thời điểm N khoảng thời gian sau (có thể là phút, giờ, hoặc ngày) dựa trên các giá trị hiện tại và trước đó trong dữ liệu.
4. Các chỉ báo kỹ thuật được thêm vào mô hình
Để cải thiện hiệu quả dự đoán, ta sử dụng thêm các chỉ báo kỹ thuật để cung cấp thêm thông tin cho mô hình:

EMA (Exponential Moving Average)
EMA là đường trung bình động lũy thừa, nhấn mạnh nhiều hơn vào các giá trị gần nhất so với MA (Moving Average) thông thường.
EMA7 (Đường trung bình động 7 đơn vị thời gian):

Ý nghĩa: EMA7 giúp phát hiện các xu hướng ngắn hạn của giá cổ phiếu, ví dụ, nếu giá nằm trên EMA7, đây là dấu hiệu của xu hướng tăng ngắn hạn và ngược lại. EMA7 cũng có thể dùng làm tín hiệu mua/bán.
EMA30 (Đường trung bình động 30 đơn vị thời gian):

Ý nghĩa: EMA30 nhạy cảm với các biến động gần đây, phù hợp cho việc phát hiện xu hướng trung hạn. Khi tích hợp vào LSTM, nó giúp mô hình hiểu các động lực giá ở mức trung hạn, hữu ích trong việc xác định xu hướng của cổ phiếu.
EMA99 (Đường trung bình động 99 đơn vị thời gian):

Ý nghĩa: EMA99 là một chỉ báo xu hướng dài hạn, loại bỏ các biến động ngắn hạn và tập trung vào xu hướng bền vững hơn. Việc sử dụng EMA99 trong mô hình giúp LSTM không chỉ tập trung vào các tín hiệu ngắn hạn mà còn nắm bắt các xu hướng dài hạn.

RSI (Relative Strength Index)

RSI đo lường mức độ mạnh/yếu của xu hướng hiện tại bằng cách đánh giá tốc độ và thay đổi giá cổ phiếu. Giá trị RSI dao động từ 0 đến 100:

Trên 70: báo hiệu trạng thái quá mua, có thể điều chỉnh giảm.

Dưới 30: báo hiệu trạng thái quá bán, có thể đảo chiều tăng giá.

Ứng dụng trong LSTM: RSI giúp mô hình phát hiện các tín hiệu đảo chiều, giúp LSTM hiểu rõ hơn về động lực giá và khả năng duy trì hoặc đảo chiều xu hướng hiện tại.

5. Tích hợp các chỉ báo vào LSTM
Các chỉ báo này giúp LSTM tăng khả năng dự đoán bằng cách:

Tăng cường thông tin về xu hướng: Các chỉ báo MA và EMA giúp mô hình hiểu rõ hơn về xu hướng ngắn hạn, trung hạn, và dài hạn, thay vì chỉ dựa vào giá cổ phiếu đơn thuần.

Loại bỏ nhiễu: EMA99, đặc biệt, giúp lọc bớt các biến động ngắn hạn, giúp mô hình tập trung vào các xu hướng bền vững hơn.

Tối ưu hóa dự đoán: Khi kết hợp các chỉ báo này, LSTM có thể học tốt hơn các mẫu phức tạp trong chuỗi thời gian và đưa ra dự đoán tốt hơn trong điều kiện có nhiều yếu tố ảnh hưởng đến biến động giá.

6. Sử dụng thêm mô hình Ensemble Learning - Random Forest

Ngoài LSTM, một mô hình Ensemble Learning như Random Forest cũng sẽ được thử nghiệm. Tuy nhiên, do thiếu khả năng lưu trữ thông tin dài hạn, Random Forest có thể không hiệu quả bằng LSTM khi dự đoán các mẫu phức tạp và dài hạn trong dữ liệu chuỗi thời gian.

8. Nhận xét
LSTM có khả năng dự đoán tốt hơn ở bước nhảy thời gian dự đoán (như 60 phút) so với Random Forest, do đặc tính lưu giữ thông tin dài hạn, giúp dự đoán giá cổ phiếu chính xác hơn trong các chuỗi thời gian phức tạp.
