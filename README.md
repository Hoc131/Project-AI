# 🍱 AI Canteen – Hệ thống nhận diện món ăn bằng Trí tuệ nhân tạo

## 📌 Tổng quan dự án

AI Canteen là một hệ thống ứng dụng trí tuệ nhân tạo (AI) để tự động nhận diện món ăn tại căn tin sinh viên thông qua hình ảnh. Mục tiêu của dự án là:
- Rút ngắn thời gian phục vụ.
- Giảm sai sót trong việc ghi nhận món ăn.
- Hướng đến triển khai thực tế trong các căn tin, nhà ăn, hoặc các khu vực phục vụ ăn uống đông người.

Hệ thống sử dụng kết hợp:
- **CNN** để phân loại ảnh đơn món ăn (phù hợp với khay chỉ có 1 món).
- **YOLOv8** để nhận diện nhiều món ăn cùng lúc trong ảnh.
- Giao diện demo đơn giản sử dụng **Gradio**, cho phép người dùng tải ảnh và xem kết quả nhận diện.

Hướng dẫn sử dụng:
B1: Tải 2 đường link google drive ( model train YOLO, model train CNN )
B2: Vào google collab để chạy code dự đoán
B3: Tải những thư viện cần thiết ( trong file code dự đoán) để chạy code dự đoán
B4: Dán code dự đoán vào google collab và bắt đầu chạy 
B5: Khi code chạy xong sẽ hiển thị giao diện gradio, nhấn vào tải ảnh lên để lấy ảnh thức ăn để dự đoán.
Model được làm với giao diện upload ảnh đã chụp và dự đoán dựa trên những gì đã train.




