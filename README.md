# Cloud Accident Detection System Using CNN and YOLOv8

A real-time accident detection system that combines **Convolutional Neural Networks (CNNs)** and **YOLOv8 vehicle tracking** to detect vehicle crashes from video feeds with cloud alerts and emergency notifications.

---

## Overview

This project automatically detects vehicle crashes from video sources.
It uses:

* **YOLOv8** for vehicle detection and ID-based tracking
* A custom-trained **CNN classifier** to distinguish accident vs non-accident vehicle crops
* Multi-frame crash confirmation logic for reliability
* Cloud API integration for event logging and crash image uploads
* WhatsApp, SMS, and Call alerts for emergency response

---

## Project Structure

```
Cloud Accident Detection/
├── dataset/                      
│   ├── 0/                         # Non-accident images
│   ├── 1/                         # Accident images
│   ├── train_split.csv            # Training metadata
│   ├── test_split.csv             # Testing metadata
│   └── crash_cnn_model.h5         # Trained CNN model
│
├── src/
│   ├── data_prep.ipynb            # Dataset creation and preprocessing
│   ├── data_train.ipynb           # CNN model training and evaluation
│   └── video_detection.ipynb      # Crash detection with YOLO + CNN + Cloud alerts
│
└── README.md
```

---

## Requirements

Install dependencies with:

```bash
pip install tensorflow keras opencv-python ultralytics scikit-learn matplotlib pandas numpy pillow requests pywhatkit twilio
```

Optional (for notebook use):

```bash
pip install jupyter
```

---

## Workflow

### 1. Dataset Preparation — `data_prep.ipynb`

* Extracts images from videos using YOLOv8 frame-by-frame.
* Saves cropped vehicle images into:

  * `0/` → non-accident
  * `1/` → accident
* Generates:

  * `train_split.csv`
  * `test_split.csv`
* Includes dataset preview and validation steps.

### 2. Model Training — `data_train.ipynb`

* Loads and preprocesses training and testing images.
* Builds a CNN architecture:

  * Conv → MaxPool → Conv → MaxPool → Conv → MaxPool
  * Flatten → Dense → Dropout → Softmax
* Trains for 10 epochs using Adam optimizer.
* Plots accuracy and loss curves.
* Saves model as `crash_cnn_model.h5`.

### 3. Crash Detection on Videos — `video_detection.ipynb`

* Loads YOLOv8 for vehicle detection and tracking.
* Loads trained CNN classifier for accident prediction.
* Identifies each vehicle with a unique ID.
* Tracks movement across frames to detect sudden stops.
* Applies confidence smoothing and multi-frame validation.
* Cloud Features:

  * Sends crash event JSON to cloud API
  * Uploads crash image to cloud storage
* Alert Features:

  * WhatsApp alert via PyWhatKit
  * SMS alert via Twilio
  * Voice call via Twilio

Crash visualization labels:

* SAFE
* INITIALIZING
* MOVING AWAY
* CRASH CONFIRMED

---

## Dataset Description

The dataset is created from real accident and non-accident videos.

### How the dataset was built

* Videos were processed frame-by-frame.
* YOLOv8 detected vehicles in each frame.
* Detected vehicles were cropped and saved.
* The crops were manually categorized:

  * `0` → non-accident
  * `1` → accident
* These images were used to train the CNN classifier.

### Dataset Files

* `0/` folder: non-crash images
* `1/` folder: crash images
* `train_split.csv` and `test_split.csv` store image paths and labels
* `crash_cnn_model.h5`: final trained model

---

## How to Run

### 1. Clone the Repository

```bash
git clone https://github.com/KAIDManiac/Cloud-Accident-Detection.git
cd Cloud-Accident-Detection
```

### 2. Run dataset preparation

```bash
jupyter notebook src/data_prep.ipynb
```

### 3. Train the CNN

```bash
jupyter notebook src/data_train.ipynb
```

### 4. Run crash detection on a video

```bash
jupyter notebook src/video_detection.ipynb
```

Edit the following in the notebook:

```
MODEL_PATH = "path/to/crash_cnn_model.h5"
VIDEO_PATH = "path/to/video.mp4"
```

---

## Key Features

* Real-time accident detection from video feeds
* CNN + YOLOv8 hybrid model for high accuracy
* Multi-frame crash confirmation to reduce false positives
* Crash image extraction and upload to cloud storage
* Cloud API integration for logging crash events
* Automated WhatsApp, SMS, and call alerts
* Support for multi-vehicle crash detection

---

## Future Improvements

* Deploy complete pipeline on cloud environment
* Add support for real-time CCTV camera feeds
* Build web dashboard for monitoring crashes
* Improve CNN with transfer learning
* Add more diverse crash scenarios to the dataset

---

## License

This project is open-source under the MIT License.

---

## Author

**Syed Abubaker Ahmed (Kaid)**

AI Engineer | Computer Vision Enthusiast
