# Week 4 Task 2 - Custom Vehicle Detection using YOLOv8

## Objective

The objective of this task was to create a custom labeled dataset and train a YOLOv8 object detection model for detecting vehicles in road traffic images.

---

## Workflow

### 1. Video Collection

A real-world traffic surveillance video was selected for dataset creation.

### 2. Frame Extraction

Frames were extracted from the video using FFmpeg.

Example command:

```bash
ffmpeg -i traffic_video.mp4 -vf fps=10 images/frame_%04d.jpg
```

### 3. Dataset Preparation

The extracted frames were divided into:

* Training dataset
* Validation dataset
* Test dataset

Dataset structure:

```text
dataset_project/

├── images/
│   ├── train/
│   ├── val/
│   └── test/
│
├── labels/
│   ├── train/
│   └── val/
```

### 4. Image Annotation

Images were annotated using Label Studio in YOLO format.

Vehicle categories were grouped into:

| Class ID | Class Name    |
| -------- | ------------- |
| 0        | light_vehicle |
| 1        | heavy_vehicle |

### 5. YOLO Configuration

A `data.yaml` file was created for training.

Example configuration:

```yaml
path: C:/summer_internship/dataset_project

train: images/train
val: images/val

nc: 2

names:
  0: light_vehicle
  1: heavy_vehicle
```

### 6. Model Training

YOLOv8 model was trained using Ultralytics.

Training command:

```bash
yolo detect train data=data.yaml model=yolov8s.pt epochs=50
```

### 7. Prediction

Predictions were generated using the trained model.

Prediction command:

```bash
yolo detect predict model=runs/detect/train9/weights/best.pt source=images/test
```

---

## Tools Used

* Python
* YOLOv8 (Ultralytics)
* Label Studio
* FFmpeg
* GitHub

---

## Training Results

| Metric    | Value |
| --------- | ----- |
| Precision | 0.804 |
| Recall    | 0.896 |
| mAP50     | 0.885 |
| mAP50-95  | 0.399 |

---

## Important Output Files

```text
runs/detect/train9/

├── weights/
│   ├── best.pt
│   └── last.pt
│
├── results.png
└── confusion_matrix.png
```

---

## Conclusion

A complete custom object detection pipeline was successfully developed using YOLOv8. The project involved dataset preparation, image annotation, model training, and prediction generation using a custom traffic dataset.
