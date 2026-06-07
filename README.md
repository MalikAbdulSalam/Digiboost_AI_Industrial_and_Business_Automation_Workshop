# Digiboost_AI_Industrial_and_Business_Automation Worshop
__________________________________________________________

# 🎓 AI Industrial Automation Workshop – Day 1

## Computer Vision, YOLO Object Detection & Industrial Automation

This repository contains the complete practical material for **Day 1** of the **AI Industrial Automation Workshop**. Participants will learn how to build real-world AI systems using **YOLO**, **Ultralytics**, **OpenCV**, and industrial automation concepts.

---

## 📚 Workshop Contents

1. YOLO (You Only Look Once)
2. Ultralytics Installation
3. AI Security Surveillance System
4. Working with Pretrained Models
5. Faulty Dough Detection
6. Training Custom YOLO Models
7. Conveyor Belt Quality Control System

---

# 1️⃣ YOLO (You Only Look Once)

YOLO is a state-of-the-art real-time object detection algorithm.

Unlike traditional computer vision approaches that scan an image multiple times, YOLO processes the entire image in a single pass and predicts:

- Object locations
- Object classes
- Confidence scores

## ✅ Advantages of YOLO

- Extremely fast
- Real-time performance
- High accuracy
- Excellent for edge devices
- Suitable for industrial automation

### Applications

- CCTV Surveillance
- Autonomous Vehicles
- Industrial Quality Control
- Robotics
- Smart Cities
- Drone Vision Systems

---

# 2️⃣ Ultralytics

Ultralytics provides an easy-to-use implementation of YOLO.

## Installation

### Step 1: Create Conda Environment

```bash
conda create -n myenv python
conda activate myenv
```

### Step 2: Install Ultralytics

```bash
pip install ultralytics
```

### Step 3: Verify Installation

```bash
python -c "from ultralytics import YOLO; print('YOLO Installed Successfully')"
```

### Additional Packages

```bash
pip install opencv-python
pip install playsound==1.2.2
```

---

# 3️⃣ Practical Project 1: AI Security Surveillance System

## A. Discover Devices on Network

### Linux

```bash
sudo apt install nmap
nmap -sn 192.168.1.0/24
```

### Windows

Install Nmap and run:

```bash
nmap -sn 192.168.1.0/24
```

---

## B. View RTSP Camera Stream

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

## C. YOLO Detection on Camera Feed

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

    annotated_frame = results[0].plot()

    cv2.imshow("YOLO Feed", annotated_frame)

    if cv2.waitKey(1) == 27:
        break

cap.release()
cv2.destroyAllWindows()
```

---

## D. Person Detection Alarm System

### Features

- Detects humans
- Triggers alarm
- Useful for restricted areas
- Smart CCTV surveillance

### Workflow

```text
Camera
   ↓
YOLO Detection
   ↓
Person Found?
   ↓
Yes
   ↓
Alarm Triggered
```

---

## E. Restricted Area Monitoring (ROI)

### Region of Interest (ROI)

Detect a person only when they enter a specified zone.

### Applications

- Military Areas
- Factories
- Warehouses
- Data Centers
- Restricted Rooms

### Concept

```text
+-----------------------+
|                       |
|     Restricted Zone   |
|                       |
+-----------------------+
```

When a detected person enters the ROI:

```text
Trigger Alarm
```

---

# 4️⃣ Pretrained Models

A pretrained model is already trained by someone else.

Instead of training from scratch, we can directly use the model.

## Sources

### GitHub

Search for:

```text
YOLO trained model
```

### Roboflow Universe

Thousands of public datasets and models.

### Hugging Face

Large collection of AI models.

### Ultralytics Official Models

```text
yolo11n.pt
yolo11s.pt
yolo11m.pt
yolo11l.pt
yolo11x.pt
```

---

# 5️⃣ Practical Project 2: Faulty Dough Detection

## Objective

Detect defective dough pieces on a production line.

### Load Model

```python
from ultralytics import YOLO

model = YOLO("Trained_Models/pastry_camera.pt")
```

### Run Inference

```python
model.predict(
    source="inputs/having_faulty_doughs.mp4",
    show=True,
    save=True
)
```

### Alarm on Defective Dough

```python
from playsound import playsound

if class_name == "Defected":
    playsound("faulty_dough_detected_please_remove.mp3")
```

---

# 6️⃣ Training Our Own YOLO Model

## Step 1: Dataset Collection

Collect images of:

- Products
- Defects
- Objects of Interest

Capture under:

- Different lighting conditions
- Different angles
- Different backgrounds

---

## Step 2: Annotation

### Recommended Tools

- Roboflow
- LabelImg

Draw bounding boxes and export in YOLO format.

---

## Step 3: Dataset Structure

```text
dataset/
│
├── images/
│   ├── train/
│   └── val/
│
├── labels/
│   ├── train/
│   └── val/
│
└── data.yaml
```

---

## Step 4: Create data.yaml

```yaml
path: ../dataset

train: images/train
val: images/val

nc: 2

names:
  - CocaCola
  - Pepsi
```

---

## Step 5: Train Model

```bash
yolo task=detect mode=train model=yolo11n.pt data=data.yaml epochs=100 imgsz=640
```

### Parameter Explanation

| Parameter | Description |
|------------|-------------|
| task | Object Detection |
| mode | Training Mode |
| model | Pretrained Weights |
| data | Dataset Configuration |
| epochs | Training Iterations |
| imgsz | Input Image Size |

---

# 7️⃣ Conveyor Belt Quality Control System

## Problem Statement

Wrong products may appear on the wrong conveyor belt.

Examples:

- Coca-Cola bottle on Pepsi line
- Pepsi bottle on Coca-Cola line

The system should:

1. Detect bottles
2. Identify bottle class
3. Determine conveyor belt location
4. Trigger alarm
5. Activate removal mechanism

---

## Workflow

```text
Camera
   ↓
YOLO Detection
   ↓
Identify Bottle
   ↓
Check Conveyor Belt
   ↓
Correct?
 ├── Yes → Continue
 └── No → Alarm + Remove
```

---

## Industrial Integration

### PLC Integration

Common protocols:

- Modbus
- Ethernet/IP
- OPC-UA

### Microcontroller Integration

- Arduino
- ESP32
- Raspberry Pi

### Relay Control

Used to control:

- Pneumatic Pushers
- Conveyor Motors
- Solenoid Valves

Example:

```python
ser.write(b'REMOVE\n')
```

---

# 🚀 Deployment Targets

After training, models can be deployed on:

- NVIDIA Jetson Nano
- NVIDIA Jetson TX2
- NVIDIA Jetson Xavier
- Raspberry Pi
- Desktop PCs
- Industrial Edge Computers

---

# 🎯 Learning Outcomes

By the end of Day 1, participants will be able to:

- Build AI CCTV systems
- Detect people using YOLO
- Create intrusion detection systems
- Use pretrained models
- Train custom YOLO models
- Build industrial inspection systems
- Create conveyor belt quality control solutions
- Integrate AI with automation hardware

---

# 📝 Recommended Practice

1. Collect your own dataset
2. Annotate using Roboflow
3. Train a custom YOLO model
4. Deploy on Jetson or Raspberry Pi
5. Integrate with PLC or Arduino
6. Build a complete industrial automation solution

---

# 📖 Resources

- Ultralytics YOLO
- Roboflow
- OpenCV
- Hugging Face
- GitHub Models

---

# 🔜 Next Session

## Day 2: Business Automation using n8n

Topics include:

- What is Automation?
- Why Automation?
- Triggers
- Workflows
- Conditions
- Memory
- AI Agents
- WhatsApp Automation
- CRM Automation
- Business Process Automation

---

# 👨‍🏫 Instructor

**AI Industrial Automation Workshop**

Computer Vision • YOLO • Industrial AI • Robotics • Automation

### 🎓 Happy Learning!
