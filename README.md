# Road Pothole Detection & Severity Classification with YOLOv11

This project uses computer vision to automatically detect potholes on roads and classify how severe they are (**major**, **medium**, or **minor**). Powered by the [Ultralytics YOLOv11](https://github.com/ultralytics/ultralytics) object detection model, it is built to spot road damage in real time—helping municipalities, road maintenance teams, and smart-city systems prioritize repairs before small cracks turn into dangerous hazards.

---

## What's Inside This Repository

Everything needed to train, evaluate, and run predictions with the model is contained in a single, end-to-end Jupyter Notebook: [`CV_Project_.ipynb`](https://github.com/Shujaullah-Dev/Pothole-Detection-YOLO-v11/blob/main/CV_Project_.ipynb). By fine-tuning a lightweight YOLOv11 model on custom-labeled road images, this pipeline balances high detection speed with practical accuracy so it can run smoothly on live video feeds or dashcam footage.

---

## The Dataset

To teach the model the difference between minor surface wear and deep, hazardous potholes, it was trained and tested on a custom dataset hosted publicly on Kaggle:

* **Download the Data:** [Pothole YOLOv11 Dataset on Kaggle](https://www.kaggle.com/datasets/shujazahid/pothole-yolov11-dataset/code)
* **How the Data is Split:**
  * **Training Set:** 1,512 images total (1,509 images containing labeled potholes, plus 3 clean "background" road images to help prevent false alarms).
  * **Validation Set:** 143 unseen images containing 366 labeled potholes used to test how well the model generalizes.

---

## Tools & Hardware Used

Because real-world road monitoring often happens on edge devices or standard hardware, this project uses the **Nano** variant of YOLOv11 (`yolo11n.pt`), which is designed to be fast and lightweight without sacrificing too much accuracy.

* **Core Framework:** Ultralytics (`v8.3.235`)
* **Model Architecture:** `yolo11n.pt` (181 layers, ~2.58 million parameters, 6.3 GFLOPs)
* **Deep Learning Engine:** PyTorch (`2.9.0+cu126`)
* **Training Hardware:** NVIDIA Tesla T4 GPU via Google Colab

---

## How the Model Was Trained

The model was trained from pre-trained base weights for 50 full passes (epochs) over the dataset. Key settings include:

| Setting | Value | Why It Matters |
| :--- | :--- | :--- |
| **Starting Weights** | `yolo11n.pt` | Uses transfer learning for faster, more accurate training |
| **Epochs** | 50 | Full passes through the training dataset |
| **Image Resolution** | 640 × 640 px | Standard input size balancing detail and speed |
| **Batch Size** | 8 | Number of images processed before updating model weights |
| **Optimizer** | AdamW (`lr=0.001429`, `momentum=0.9`) | Smoothly adjusts learning rates to avoid overfitting |
| **Mixed Precision (AMP)** | Enabled | Speeds up GPU training while using less memory |
| **Close Mosaic** | Last 10 epochs | Turns off mosaic data augmentation near the end so the model fine-tunes on normal, uncropped road views |

---

## Model Performance & Results

After 50 epochs, the best-performing model checkpoint (`best.pt`) was tested against the validation set. As expected, larger and deeper potholes are much easier for the model to spot reliably than small surface defects:

| Severity Class | Validation Images | Pothole Instances | Precision | Recall | mAP@50 | mAP@50-95 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Overall (All Classes)** | **143** | **366** | **49.4%** | **55.1%** | **50.1%** | **32.2%** |
| `major_pothole` | 79 | 131 | 65.7% | 71.6% | 72.1% | 49.1% |
| `medium_pothole` | 103 | 166 | 49.5% | 56.0% | 55.2% | 34.8% |
| `minor_pothole` | 45 | 69 | 33.0% | 37.7% | 23.0% | 12.6% |

* **Real-Time Speed:** Processes each image in roughly **4.9 milliseconds** on a Tesla T4 GPU (~200+ frames per second), making it more than fast enough for real-time video analysis.

---

## Getting Started

### 1. Clone the Repository
Download the project files to your local machine using Git:

```bash
git clone [https://github.com/Shujaullah-Dev/Pothole-Detection-YOLO-v11.git](https://github.com/Shujaullah-Dev/Pothole-Detection-YOLO-v11.git)
cd Pothole-Detection-YOLO-v11
