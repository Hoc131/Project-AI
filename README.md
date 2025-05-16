# Project-AI
TRAINYOLO
!pip install ultralytics

path: /content/dataset  # đường dẫn gốc
train: images/train
val: images/val

names:
  0: "Ca hu kho"
  1: "Canh cai"
  2: "Canh chua"
  3: "Com trang"
  4: "Dau hu sot ca"
  5: "Ga chien"
  6: "Rau muong xao"
  7: "Thit kho"
  8: "Thit kho trung"
  9: "Trung chien"

  TRAIN
from ultralytics import YOLO

# Load YOLOv8 model (n = nhỏ, s = vừa, m/l/x = lớn hơn)
model = YOLO("yolov8n.pt")

# Train
model.train(
    data="/content/dataset/data.yaml", 
    epochs=50,
    imgsz=640,
    batch=16,
    name="canteen_yolo",
    project="/content/runs",
    device=0  

Train CNN:
import cv2
import numpy as np
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Conv2D, MaxPooling2D, Flatten, Dense, Dropout, BatchNormalization
from tensorflow.keras.preprocessing.image import ImageDataGenerator
from tensorflow.keras.optimizers import Adam
from pathlib import Path

#Cấu hình đường dẫn cho Kaggle
base_dir = Path('/kaggle/input/traincnn/AI THINH_augmented')  # ← bạn cần upload dataset vào thư mục này hoặc zip & giải nén tại đây
model_path = '/kaggle/working/cantin_model.h5'

#Tạo ImageDataGenerator
datagen = ImageDataGenerator(
    rescale=1./255,
    rotation_range=10,
    width_shift_range=0.2,
    height_shift_range=0.2,
    shear_range=0.2,
    zoom_range=0.2,
    horizontal_flip=True,
    vertical_flip=True,
    brightness_range=[0.6, 1.4],
    fill_mode='nearest',
    validation_split=0.2
)

batch_size = 64

# Tải dữ liệu train/validation
train_data = datagen.flow_from_directory(
    base_dir,
    target_size=(128, 128),
    class_mode='categorical',
    subset='training',
    batch_size=batch_size
)

val_data = datagen.flow_from_directory(
    base_dir,
    target_size=(128, 128),
    class_mode='categorical',
    subset='validation',
    batch_size=batch_size
)

#  Khởi tạo model CNN
model = Sequential([
    Conv2D(128, (3, 3), activation='relu', input_shape=(128, 128, 3)),
    BatchNormalization(),
    MaxPooling2D((2, 2)),
    Dropout(0.3),

    Conv2D(256, (3, 3), activation='relu'),
    BatchNormalization(),
    MaxPooling2D((2, 2)),
    Dropout(0.3),

    Conv2D(512, (3, 3), activation='relu'),
    BatchNormalization(),
    MaxPooling2D((2, 2)),
    Dropout(0.4),

    Flatten(),
    Dense(512, activation='relu'),
    BatchNormalization(),
    Dropout(0.5),

    Dense(10, activation='softmax')
])

model.compile(optimizer=Adam(learning_rate=0.0003),
              loss='categorical_crossentropy',
              metrics=['accuracy'])

#  Huấn luyện mô hình
model.fit(train_data, validation_data=val_data, epochs=100, batch_size=batch_size)

#  Lưu mô hình
model.save(model_path)

from IPython.display import FileLink
FileLink(model_path)


CHẠY CODE DỰ ĐOÁN 
!pip install ultralytics tensorflow gradio opencv-python
!pip install gradio
CODE DỰ ĐOÁN
import gradio as gr
import cv2
import numpy as np
from ultralytics import YOLO
from tensorflow.keras.models import load_model
from tensorflow.keras.preprocessing.image import img_to_array
from PIL import Image

yolo_model = YOLO("/content/drive/MyDrive/yolov8n.pt")

# Keras .h5 để phân loại món ăn
classifier_model = load_model("/content/drive/MyDrive/cantin_model.h5")

# Danh sách nhãn tương ứng với mô hình classifier
class_labels = [
    "Ca hu kho", "Canh cai", "Canh chua", "Com trang", "Dau hu sot ca",
    "Ga chien", "Rau muong xao", "Thit kho", "Thit kho trung", "Trung chien"
]

# Bảng giá món ăn
food_prices = {
    "Ca hu kho": 10000,
    "Canh cai": 8000,
    "Canh chua": 8000,
    "Com trang": 5000,
    "Dau hu sot ca": 7000,
    "Ga chien": 12000,
    "Rau muong xao": 6000,
    "Thit kho": 12000,
    "Thit kho trung": 14000,
    "Trung chien": 7000
}

def preprocess_crop(crop_img):
    img = cv2.resize(crop_img, (128, 128))
    img = img.astype("float32") / 255.0
    img = np.expand_dims(img, axis=0)
    return img

def detect_and_classify(image):
    results = yolo_model(image)
    detections = results[0].boxes.data.cpu().numpy()

    predicted_classes = []
    total_price = 0

    for det in detections:
        x1, y1, x2, y2, score, class_id = det
        if score < 0.3:
            continue

        crop = image[int(y1):int(y2), int(x1):int(x2)]
        if crop.size == 0:
            continue

        # Tiền xử lý và phân loại bằng CNN
        input_img = preprocess_crop(crop)
        preds = classifier_model.predict(input_img)
        predicted_index = np.argmax(preds[0])
        predicted_label = class_labels[predicted_index]

        predicted_classes.append(predicted_label)
        total_price += food_prices.get(predicted_label, 0)

    if not predicted_classes:
        return image, "<span style='color:red'>Không phát hiện được món ăn!</span>", "0 đ"

    result_html = "".join([
        f"<li>{label}: {food_prices[label]:,} đ</li>" for label in predicted_classes
    ])
    return image, f"<ul>{result_html}</ul>", f"{total_price:,} đ"

# === Giao diện Gradio ===
header = """
<div style="text-align:center; padding: 20px;">
    <h1 style="color:#2E8B57">🍱 Nhận Diện Món Ăn & Tính Tiền</h1>
    <p>Tải ảnh mâm cơm lên để phát hiện và tính giá các món ăn</p>
</div>
"""

with gr.Blocks(theme=gr.themes.Soft()) as demo:
    gr.HTML(header)
    with gr.Row():
        with gr.Column():
            image_input = gr.Image(type="numpy", label="📷 Ảnh mâm cơm")
            btn = gr.Button("🔍 Nhận diện & tính tiền")
        with gr.Column():
            image_output = gr.Image(type="numpy", label="Ảnh kết quả")
            food_list = gr.HTML(label="🍽️ Danh sách món ăn")
            total_cost = gr.Textbox(label="💰 Tổng tiền")

    btn.click(fn=detect_and_classify, inputs=image_input, outputs=[image_output, food_list, total_cost])

# Chạy ứng dụng
demo.launch()


LINK TẢI YOLO: https://drive.google.com/file/d/1JEridz0WfOPmWWA6Q4PczaQd4_41YNHT/view?usp=drive_link
LINK TẢI CNN: https://drive.google.com/file/d/1rHIIPs2_TLxXr0mLFR3Dhl-eY-xRWUjc/view?usp=drive_link
