# Digiboost_AI_Industrial_and_Business_Automation Worshop
__________________________________________________________

# 🎓 AI Industrial Automation Workshop – Day 1

## Computer Vision, YOLO Object Detection & Industrial Automation

This repository contains the complete practical material for **Day 1** of the AI Industrial Automation Workshop. Participants will learn YOLO, Ultralytics, OpenCV, and industrial automation systems.

---

# 📌 Prerequisites

- Basic Python knowledge  
- Windows / Linux system  
- Internet connection  
- Anaconda / Miniconda installed (recommended)  
- Webcam or IP camera (optional)  

---

# 1️⃣ Miniconda Installation & Setup

## Download Miniconda
https://docs.conda.io/en/latest/miniconda.html

## Install Steps
1. Download installer
2. Run setup
3. Select default options
4. Finish installation

---

## Open Anaconda Prompt (Windows)

Start Menu → Search:
```
Anaconda Prompt
```

You should see:
```
(base) C:\Users\YourName>
```

---

## Create Environment

```bash
conda create -n myenv python
```

Activate:

```bash
conda activate myenv
```

---

## Install Dependencies

```bash
pip install ultralytics
pip install opencv-python
pip install playsound==1.2.2
```

---

## Verify Installation

```bash
python -c "from ultralytics import YOLO; print('YOLO Ready')"
```

---

# 2️⃣ YOLO (You Only Look Once)

YOLO is a real-time object detection system.

### Features:
- Fast inference
- Single pass detection
- High accuracy
- Industrial applications

### Applications:
- CCTV systems
- Robotics
- Drones
- Quality inspection
- Automation

---

# 3️⃣ Network Camera Discovery

## Linux / Ubuntu

```bash
sudo apt install nmap
nmap -sn 192.168.1.0/24
```

## Windows

Use Nmap software:
https://nmap.org/download.html

---

# 4️⃣ IP Camera Stream (OpenCV)

```python
import cv2

ip = "192.168.1.49"
url = f"rtsp://admin:123456@{ip}:554/unicast/c1/s0/live"

cap = cv2.VideoCapture(url)

while True:
    ret, frame = cap.read()
    if not ret:
        continue

    cv2.imshow("Camera", frame)

    if cv2.waitKey(1) == 27:
        break

cap.release()
cv2.destroyAllWindows()
```

---

# 5️⃣ YOLO Detection on Camera

```python
import cv2
from ultralytics import YOLO

model = YOLO("yolo11n.pt")

cap = cv2.VideoCapture(url)

while True:
    ret, frame = cap.read()
    if not ret:
        continue

    results = model(frame)
    frame = results[0].plot()

    cv2.imshow("YOLO", frame)

    if cv2.waitKey(1) == 27:
        break

cap.release()
cv2.destroyAllWindows()
```

---

# 6️⃣ Person Detection Alarm System

```python
import cv2
from ultralytics import YOLO
import winsound
import time

model = YOLO("yolo11n.pt")

last = 0

def alarm():
    global last
    if time.time() - last > 3:
        winsound.Beep(1000, 500)
        print("Person Detected!")
        last = time.time()

cap = cv2.VideoCapture(url)

while True:
    ret, frame = cap.read()
    if not ret:
        continue

    results = model(frame)[0]

    for box in results.boxes:
        cls = int(box.cls[0])
        if cls == 0:
            alarm()

    cv2.imshow("Security", results.plot())

    if cv2.waitKey(1) == 27:
        break

cap.release()
cv2.destroyAllWindows()
```

---

# 7️⃣ ROI Restricted Area Detection

- Define polygon region
- Detect person inside zone
- Trigger alarm

Used in:
- Banks
- Military zones
- Warehouses

---

# 8️⃣ Pretrained Models

Sources:
- Ultralytics (yolo11n.pt)
- Roboflow
- HuggingFace
- GitHub

---

# 9️⃣ Faulty Dough Detection

```python
from ultralytics import YOLO

model = YOLO("pastry_camera.pt")

model.predict(
    source="video.mp4",
    show=True,
    save=True
)
```

---

# 🔟 Dataset Structure

```
dataset/
 ├── images/
 │   ├── train/
 │   └── val/
 ├── labels/
 │   ├── train/
 │   └── val/
 └── data.yaml
```

---

# 1️⃣1️⃣ data.yaml

```yaml
path: ../dataset
train: images/train
val: images/val

nc: 2
names: [CocaCola, Pepsi]
```

---

# 1️⃣2️⃣ Training Command

```bash
yolo task=detect mode=train model=yolo11n.pt data=data.yaml epochs=100 imgsz=640
```

---

# 1️⃣3️⃣ Conveyor Belt Quality Control System

Detect wrong bottle on wrong belt and trigger alarm.

```python
# (Full system code simplified for README)
# Uses YOLO + ROI + alarm system logic
```

---

# 🚀 Deployment

- Jetson Nano
- Jetson TX2
- Raspberry Pi
- Industrial PCs

---

# 🎯 Learning Outcomes

- YOLO object detection
- CCTV AI system
- Industrial automation
- Dataset training
- Real-time inference
- Edge deployment

---

# 🔜 Next

Day 2: Business Automation using n8n
