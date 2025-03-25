# YOLOv8 Pothole Detection

## Overview
This project utilizes YOLOv8 to detect potholes in images and videos. It involves dataset preparation, model training, evaluation, and inference on test data.

## Prerequisites
Ensure you have the following installed:
- Python 3.10+
- PyTorch 2.3.1+cu121
- NVIDIA GPU with CUDA 12.2 (recommended)

## Installation
1. Clone the repository (if applicable).
2. Install required dependencies:
   ```bash
   pip install ultralytics==8.0.0 roboflow
   ```
3. Check if Ultralytics YOLOv8 is correctly installed:
   ```python
   import ultralytics
   ultralytics.checks()
   ```

## Dataset Preparation
1. Create a dataset directory:
   ```bash
   mkdir -p /content/datasets
   ```
2. Download the dataset from Roboflow:
   ```python
   from roboflow import Roboflow
   rf = Roboflow(api_key="YOUR_API_KEY")
   project = rf.workspace("YOUR_NAME").project("yolov8pothole")
   version = project.version(1)
   dataset = version.download("yolov5")
   ```

## Training the YOLOv8 Model
Train the model using the following command:
```bash
cd /content
yolo task=detect mode=train model=yolov8m.pt data=/content/Yolov8Pothole-1/data.yaml epochs=70 imgsz=640
```

## Model Evaluation
1. Confusion Matrix:
   ```python
   from IPython.display import Image
   Image(filename='/content/runs/detect/train2/confusion_matrix.png', width=900)
   ```
2. Training and Validation Loss:
   ```python
   Image(filename='/content/runs/detect/train2/results.png', width=600)
   ```
3. Validation Batch Predictions:
   ```python
   Image(filename='/content/runs/detect/train2/val_batch0_pred.jpg', width=600)
   ```
4. Validate with best weights:
   ```bash
   yolo task=detect mode=val model=/content/runs/detect/train2/weights/best.pt data=/content/datasets/Yolov8Pothole-1/data.yaml
   ```

## Inference on Test Data
```bash
yolo task=detect mode=predict model=/content/runs/detect/train2/weights/best.pt conf=0.25 source=/content/datasets/Yolov8Pothole-1/test/images
```

## Testing on a Demo Video
```bash
cp "/content/drive/MyDrive/Pothole Detect/demo2.mp4" .
yolo task=detect mode=predict model=/content/runs/detect/train2/weights/best.pt conf=0.25 source='/content/demo2.mp4'
```

## Display the Demo Video
[Watch the demo video](https://www.linkedin.com/feed/update/urn:li:activity:7230758159672369152/)

```python
from IPython.display import HTML
from base64 import b64encode
import os

# Input video path
save_path = '/content/runs/detect/predict3/demo2.mp4'
compressed_path = "/content/result_compressed2.mp4"

os.system(f"ffmpeg -i {save_path} -vcodec libx264 {compressed_path}")

# Show video
mp4 = open(compressed_path,'rb').read()
data_url = "data:video/mp4;base64," + b64encode(mp4).decode()
HTML(f"""
<video width=600 controls>
  <source src='{data_url}' type='video/mp4'>
</video>
""")
```

## Troubleshooting
- Ensure all paths exist before executing commands.
- Check that your dataset is downloaded correctly.
- Ensure your GPU is available and properly set up with CUDA.

---
This README provides step-by-step instructions for setting up, training, and evaluating a pothole detection model using YOLOv8. Let me know if you need further refinements!

