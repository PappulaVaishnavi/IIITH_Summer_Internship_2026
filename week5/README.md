# YOLO Object Detection Pipeline (Labeling → Training → Inference → Video Generation)

## Overview
This project demonstrates an end-to-end computer vision workflow using a pre-trained YOLO model. It includes dataset annotation, preprocessing, model training, evaluation, and generating outputs such as annotated images and videos.

The dataset is created using Label Studio and used to train a custom object detection model.

---

## Pipeline Workflow

### 1. Dataset Annotation (Label Studio)
- Tool used: Label Studio
- Access via browser: https://localhost:8080/
- Bounding boxes are created for custom object classes
- Recommended: keep 1–2 classes for learning
- Export annotations in YOLO format

---

### 2. Dataset Structure
dataset/
├── train/
│   ├── images/
│   ├── labels/
├── val/
│   ├── images/
│   ├── labels/
├── test/
│   ├── images/

---

### 3. Dataset Configuration

data.yaml
train: dataset/train/images
val: dataset/val/images

nc: 2
names: ["class1", "class2"]

---

train.txt & val.txt
- Contains image paths for training and validation sets

---

### 4. Image Preprocessing (Resize using FFmpeg)

ffmpeg -i input.jpg -vf scale=384:-1 output.jpg

Benefits:
- Maintains aspect ratio
- Keeps bounding boxes valid
- Ensures consistent training data

---

### 5. Model Training (YOLO Fine-Tuning)

yolo detect train data=data.yaml model=yolov5s.pt epochs=100 imgsz=640

Training Notes:
- Loss decreases initially
- Validation loss may increase later (overfitting)
- Best model is saved before overfitting

---

### 6. Model Output

runs/detect/train/weights/best.pt

---

### 7. Inference (Object Detection)

yolo detect predict model=best.pt source=dataset/test/images save=True

Output:
- Bounding boxes
- Class labels
- Confidence scores

---

### 8. Convert Frames to Video (FFmpeg)

ffmpeg -framerate 1 -i frame_%03d.jpg -c:v libx264 -pix_fmt yuv420p output.mp4

---

## Project Structure

YOLO-Object-Detection-Pipeline/
│
├── dataset/
│   ├── train/
│   ├── val/
│   ├── test/
│
├── runs/
│   ├── detect/
│   │   ├── predict/
│   │   ├── train/
│   │       ├── weights/
│
├── scripts/
├── utils/
├── outputs/
├── data.yaml
├── train.txt
├── val.txt
├── README.md
├── requirements.txt

---

## Tools & Technologies
- Python
- YOLO (Ultralytics / YOLOv5)
- Label Studio
- FFmpeg
- OpenCV
- NumPy

---

## Results
- Custom object detection model trained successfully
- Predictions generated on test dataset
- Bounding boxes visualized on images
- Frames converted into video output

---

## Future Improvements
- Add more object classes
- Improve dataset diversity
- Real-time webcam detection
- Deploy using Streamlit or Flask
- Edge deployment (Jetson / mobile devices)

---

## Notes
- Keep number of classes small for learning
- Ensure images and labels match correctly
- Maintain YOLO normalized format
- Validate dataset before training

---

## Conclusion
This project demonstrates a complete YOLO-based object detection pipeline from dataset creation to model inference. It can be extended to real-world applications like surveillance, automation, and smart detection systems.