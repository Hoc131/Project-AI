# 🧠 Ứng dụng Nhận diện Món ăn Canteen

## Tổng quan về dự án

Ứng dụng này sử dụng mô hình học sâu (YOLO để phát hiện đối tượng và một mô hình CNN Keras tùy chỉnh để phân loại món ăn) nhằm mục đích tự động nhận diện các món ăn trong ảnh hoặc từ webcam. Sau khi nhận diện, ứng dụng sẽ hiển thị tên các món ăn được phát hiện và ước tính tổng chi phí dựa trên giá được định nghĩa trước. Ngoài ra, ứng dụng còn có chức năng ghi lại lịch sử các món ăn đã nhận diện và thống kê các món ăn phổ biến trong ngày.

## Hướng dẫn cài đặt

Để chạy ứng dụng này, bạn cần cài đặt Python và các thư viện sau:

1.  **Cài đặt Python:** Nếu bạn chưa cài đặt Python, hãy tải và cài đặt phiên bản mới nhất từ [python.org](https://www.python.org/).

2.  **Cài đặt các thư viện cần thiết:** Mở terminal hoặc command prompt và chạy lệnh sau:

    "pip install tkinter pillow opencv-python ultralytics numpy tensorflow keras"


3.  **Tải mô hình YOLO:** Mô hình YOLOv8n sẽ được tự động tải xuống khi bạn chạy ứng dụng lần đầu tiên.

4.  **Mô hình CNN Keras:** Đảm bảo bạn đã có file mô hình CNN Keras đã huấn luyện (`CNNAI.h5`) trong cùng thư mục với script Python.

## Hướng dẫn sử dụng

1.  **Chạy ứng dụng:** Mở terminal hoặc command prompt, điều hướng đến thư mục chứa file Python của ứng dụng và chạy lệnh:
   
    python your_script_name.py
    
    (Thay "your_script_name.py" bằng tên file Python ).

3.  **Sử dụng các chức năng:**
    **Chọn ảnh từ máy:** Nhấn nút "📂 Chọn ảnh từ máy" để chọn một ảnh từ máy tính của bạn. Ứng dụng sẽ hiển thị ảnh, nhận diện các món ăn và hiển thị kết quả ở khung bên phải.
    **Bật Webcam:** Nhấn nút "▶️ Bật Webcam" để kích hoạt webcam. Hình ảnh từ webcam sẽ hiển thị ở khung giữa.
    **Chụp ảnh:** Khi webcam đang hoạt động, nhấn nút "📸 Chụp ảnh" để chụp ảnh hiện tại từ webcam và tiến hành nhận diện.
    **Tắt Webcam:** Nhấn nút "⏹️ Tắt Webcam" để dừng việc sử dụng webcam.
    **Thống kê hôm nay:** Nhấn nút "📊 Thống kê hôm nay" để xem danh sách các món ăn đã được nhận diện trong ngày hôm nay và món ăn nào phổ biến nhất.

## Các phần phụ thuộc

- 'tkinter': Để xây dựng giao diện người dùng đồ họa.
- 'pillow' (PIL): Để làm việc với hình ảnh.
- 'opencv-python' ('cv2'): Để xử lý hình ảnh và tương tác với webcam.
- 'ultralytics' ('YOLO'): Để sử dụng mô hình YOLO cho việc phát hiện đối tượng.
- 'numpy': Để thực hiện các phép toán số học.
- 'tensorflow' ('keras'): Để tải và sử dụng mô hình CNN Keras.

## Chất lượng chương trình

**Cấu trúc tốt:** Mã nguồn được chia thành các hàm rõ ràng, mỗi hàm thực hiện một chức năng cụ thể (ví dụ: 'detect_and_classify', 'show_result', 'start_webcam'). Giao diện người dùng được xây dựng bằng `tkinter` với các frame để quản lý bố cục.
 **Chú thích:** Các phần quan trọng của mã đã được chú thích để giải thích logic và mục đích của chúng.
 **Thông lệ tốt nhất:**
  -Sử dụng các thư viện chuyên dụng cho từng tác vụ (ví dụ: YOLO cho phát hiện, Keras cho phân loại).
  -Xử lý lỗi cơ bản (ví dụ: kiểm tra việc mở webcam).
  -Giao diện người dùng thân thiện với các nút bấm và hiển thị kết quả rõ ràng.
  -Ghi log các hoạt động nhận diện.
