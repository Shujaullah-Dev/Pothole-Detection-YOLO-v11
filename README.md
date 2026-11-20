# Pothole Detection Using YOLOv11

A computer vision project for detecting and classifying road potholes by severity (**major**, **medium**, and **minor**) using the [Ultralytics YOLOv11](https://github.com/ultralytics/ultralytics) object detection architecture.

---

## Overview

This repository contains the complete training, validation, and inference pipeline implemented in [`CV_Project_.ipynb`](https://github.com/Shujaullah-Dev/Pothole-Detection-YOLO-v11/blob/main/CV_Project_.ipynb). The model is fine-tuned on a custom annotated pothole dataset to identify road surface damage in real time, making it suitable for automated road maintenance and smart infrastructure monitoring.

---

##  Dataset

The dataset used to train and evaluate this model is publicly available on Kaggle:

* **Dataset Link:** [Pothole YOLOv11 Dataset on Kaggle](https://www.kaggle.com/datasets/shujazahid/pothole-yolov11-dataset/code)
* **Dataset Split:**
  * **Training Set:** 1,512 images (1,509 labeled images + 3 background images)
  * **Validation Set:** 143 images (366 annotated instances)
    
---

##  Tech Stack & Environment

* **Framework:** Ultralytics (`v8.3.235`)
* **Model Architecture:** `yolo11n.pt` (181 layers, ~2.58M parameters, 6.3 GFLOPs)
* **Deep Learning Backend:** PyTorch (`2.9.0+cu126`)
* **Hardware:** NVIDIA Tesla T4 GPU (Google Colab)

---

##  Training Configuration

The model was trained using the following hyperparameters:

| Parameter | Value |
| :--- | :--- |
| **Base Weights** | `yolo11n.pt` |
| **Epochs** | 50 |
| **Image Size (`imgsz`)** | 640 × 640 |
| **Batch Size** | 8 |
| **Optimizer** | AdamW (`lr=0.001429`, `momentum=0.9`) |
| **Precision** | Automatic Mixed Precision (AMP) |
| **Close Mosaic** | Last 10 epochs |

---

##  Validation Results

After 50 epochs of training, the best checkpoint (`best.pt`) achieved the following performance on the validation set:

| Class | Images | Instances | Precision (P) | Recall (R) | mAP@50 | mAP@50-95 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **All Classes** | **143** | **366** | **0.494** | **0.551** | **0.501** | **0.322** |
| `major_pothole` | 79 | 131 | 0.657 | 0.716 | 0.721 | 0.491 |
| `medium_pothole` | 103 | 166 | 0.495 | 0.560 | 0.552 | 0.348 |
| `minor_pothole` | 45 | 69 | 0.330 | 0.377 | 0.230 | 0.126 |

* **Inference Speed:** ~4.9 ms per image on a Tesla T4 GPU.

---

##  Getting Started

### 1. Clone the Repository
```bash
git clone [https://github.com/Shujaullah-Dev/Pothole-Detection-YOLO-v11.git](https://github.com/Shujaullah-Dev/Pothole-Detection-YOLO-v11.git)
cd Pothole-Detection-YOLO-v11
