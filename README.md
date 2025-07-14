
# viEMO: Nâng cao nhận diện cảm xúc tiếng Việt bằng học đa phương thức

## Tổng quan
viEMO là dự án tập trung phát triển hệ thống nhận diện cảm xúc trong tiếng Việt dựa trên các kỹ thuật học đa phương thức. Dự án tận dụng dữ liệu từ văn bản, âm thanh và hình ảnh để nâng cao độ chính xác và độ tin cậy của hệ thống nhận diện cảm xúc.

## Quá trình xây dựng bộ dữ liệu

### 1. Dữ liệu văn bản
- **Nguồn**: Transcript (phụ đề hội thoại) của video YouTube.
- **Cách làm**: Tự động thu thập các câu thoại của nhân vật từ transcript, lấy thời gian bắt đầu của mỗi đoạn hội thoại. Sau đó, thủ công gán thời gian kết thúc cho từng đoạn.

### 2. Dữ liệu âm thanh
- **Nguồn**: Âm thanh từ video YouTube.
- **Cách làm**: Sử dụng thời gian bắt đầu và kết thúc của đoạn hội thoại để cắt trích đoạn âm thanh tương ứng.
- **Kiểm tra chất lượng**: Thủ công kiểm tra lại các đoạn âm thanh, loại bỏ các đoạn có nhiều người nói hoặc nhiều tạp âm.

### 3. Dữ liệu hình ảnh
- **Nguồn**: Khung hình từ video YouTube.
- **Cách làm**: Thủ công chụp ảnh khuôn mặt người nói trong từng đoạn hội thoại và lưu vào các thư mục riêng.

## Cấu trúc thư mục
- `Dataset/`
  - `Audio/`: Chứa các đoạn âm thanh đã cắt, phân loại theo video hoặc nhân vật.
  - `File csv/`: Chứa các file CSV lưu thông tin metadata, văn bản, thời gian, nhãn cảm xúc...
  - `Image/`: Chứa các thư mục ảnh khuôn mặt, phân loại theo video hoặc nhân vật.
- `Face_Emotion_Recognition-master/`: Mã nguồn và tài nguyên cho nhận diện cảm xúc khuôn mặt.
- `Speech_Emotion_Detection-master/`: Mã nguồn và tài nguyên cho nhận diện cảm xúc giọng nói.
- `Text_Emotion_Recognition-master/`: Mã nguồn và tài nguyên cho nhận diện cảm xúc văn bản.
- `Fusion_Method/`: Notebook cho các phương pháp kết hợp đa phương thức.

## Modalities và nguồn dữ liệu
Mỗi phương thức nhận diện cảm xúc sử dụng nguồn dữ liệu và mô hình khác nhau:

- **Nhận diện cảm xúc khuôn mặt**
  - Sử dụng mô hình `emotiondetector.h5` để phân loại cảm xúc từ ảnh khuôn mặt.
  - Dữ liệu đầu vào: Ảnh khuôn mặt trong `Dataset/Image/`.
  - Mã nguồn: `Face_Emotion_Recognition-master/`.

- **Nhận diện cảm xúc giọng nói**
  - Sử dụng bộ dữ liệu TESS Toronto để huấn luyện và đánh giá.
  - Dữ liệu đầu vào: Các đoạn âm thanh trong `Dataset/Audio/`.
  - Mã nguồn: `Speech_Emotion_Detection-master/`.

- **Nhận diện cảm xúc văn bản**
  - Sử dụng bộ dữ liệu UIT-VSMEC cho tiếng Việt.
  - Dữ liệu đầu vào: Các câu thoại và metadata trong `Dataset/File csv/`.
  - Mã nguồn: `Text_Emotion_Recognition-master/`.


## Kết hợp đa phương thức
Kết quả nhận diện từ ba phương thức (khuôn mặt, giọng nói, văn bản) sẽ được kết hợp bằng các thuật toán trong thư mục `Fusion_Method/`, cụ thể:

- **Product Fusion (Phép nhân xác suất):**
  - Xác suất dự đoán cảm xúc từ từng modality (khuôn mặt, giọng nói, văn bản) sẽ được nhân lại với nhau cho từng lớp cảm xúc, tạo ra một vector xác suất tổng hợp.
  - Nhãn cảm xúc cuối cùng là nhãn có xác suất tổng hợp cao nhất.
  - Phương pháp này giúp tận dụng đồng thời thông tin từ cả ba nguồn, tăng độ tin cậy khi các modalities đồng thuận.

- **Weighted Average Fusion (Trung bình có trọng số):**
  - Xác suất dự đoán từ từng modality được kết hợp theo công thức:
    - `fused_probs = audio_probs * 0.5 + image_probs * 0.35 + text_probs * 0.15`
  - Trọng số có thể điều chỉnh, ví dụ ưu tiên audio hơn các nguồn khác.
  - Kết quả cuối cùng là nhãn cảm xúc có xác suất tổng hợp lớn nhất.

Cả hai phương pháp đều là các kỹ thuật ensemble/fusion phổ biến, giúp tăng độ chính xác so với dùng riêng lẻ từng modality. Các thuật toán này được hiện thực bằng Python trong các notebook của thư mục `Fusion_Method`.

## Mục tiêu dự án
- Xây dựng các mô hình nhận diện cảm xúc tiếng Việt mạnh mẽ dựa trên dữ liệu văn bản, âm thanh, hình ảnh.
- Nghiên cứu và ứng dụng các kỹ thuật kết hợp đa phương thức để nâng cao hiệu quả nhận diện.
- Cung cấp bộ dữ liệu chất lượng cao, đã được kiểm duyệt thủ công cho cộng đồng nghiên cứu.

## Ghi chú
- Dữ liệu được thu thập từ các video YouTube công khai.
- Quá trình gán nhãn và kiểm duyệt được thực hiện thủ công để đảm bảo chất lượng bộ dữ liệu.

