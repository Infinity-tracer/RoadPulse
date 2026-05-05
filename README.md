# RoadPulse - The Living Safety Network for Indian Roads

**Team ID:** SS20 | **Semester:** 6th | **Course:** Interdisciplinary Project

A platform that transforms smartphones and dashcams into nodes of a self-learning road safety network using edge computer vision (YOLOv8), real-time streaming (Kafka), and multi-agent AI systems.

---

## Project Structure

```
imobilthon/
├── RoadDamageDetection-main/    # Streamlit MVP (WORKING)
├── video-privacy-blur/           # Privacy blur module (WORKING)
├── Multivideo_Objectdetection_MLOPS_Project/  # Kafka pipeline reference
└── 1001_0/                       # Video dataset (444 dashcam videos)
```

---

## Working Components

| Component | Status | Description |
|-----------|--------|-------------|
| Road Damage Detection (Streamlit) | WORKING | YOLOv8 detecting potholes, cracks from webcam/images/videos |
| Privacy Blur Tool | WORKING | Blurs faces and license plates in videos |
| Kafka MLOps Pipeline | REFERENCE | Docker-based video streaming architecture |

---

## Quick Start - Streamlit MVP

### Prerequisites

- Python 3.10+
- pip
- Webcam (optional, for realtime detection)

### Setup

```bash
# 1. Navigate to the detection app
cd RoadDamageDetection-main

# 2. Create virtual environment (recommended)
python -m venv venv

# Windows
venv\Scripts\activate

# Linux/Mac
source venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt
```

### Run the App

```bash
streamlit run Home.py
```

**Opens at:** http://localhost:8501

### Features Available

| Page | What it does | How to test |
|------|--------------|-------------|
| **Realtime Detection** | Live webcam detection | Connect webcam, click Start |
| **Image Detection** | Upload image for analysis | Upload any road image |
| **Video Detection** | Process video files | Upload MP4/AVI file |

### Detection Classes

The model detects 4 types of road damage:
1. **Longitudinal Crack** - Cracks parallel to road direction
2. **Transverse Crack** - Cracks perpendicular to road direction  
3. **Alligator Crack** - Interconnected crack pattern
4. **Potholes** - Holes in road surface

---

## Privacy Blur Module

Blurs faces and license plates before cloud upload (GDPR/privacy compliance).

### Setup

```bash
cd video-privacy-blur
pip install -e .
```

### Usage

```bash
# Basic usage
python -m privacy_blur.cli --input <video.mp4> --output <output.mp4>

# With preview window
python -m privacy_blur.cli --input sample.mp4 --output blurred.mp4 --show

# Blur only plates (no faces)
python -m privacy_blur.cli --input video.mp4 --output out.mp4 --no-blur-faces

# Use pixelate instead of gaussian blur
python -m privacy_blur.cli --input video.mp4 --output out.mp4 --method pixelate
```

### Options

| Flag | Description |
|------|-------------|
| `--input` | Input video path or camera index (0) |
| `--output` | Output video path |
| `--show` | Preview window while processing |
| `--blur-faces` / `--no-blur-faces` | Toggle face blurring |
| `--blur-plates` / `--no-blur-plates` | Toggle license plate blurring |
| `--method gaussian/pixelate` | Blur method |
| `--conf` | Detection confidence threshold (default: 0.35) |

---

## Kafka Pipeline (Reference Architecture)

Requires Docker Desktop.

```bash
cd Multivideo_Objectdetection_MLOPS_Project

# Start Kafka
docker network create kafka-network
docker compose -f kafka/docker-compose.yml up -d

# Start app
docker compose up -d

# View UI: http://localhost:5000
# TorchServe: http://localhost:8081/models
```

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Detection Model | YOLOv8s (Ultralytics) |
| Frontend | Streamlit |
| Realtime Video | streamlit-webrtc, OpenCV |
| Privacy | YOLO face/plate detection + Gaussian blur |
| Streaming | Apache Kafka (reference) |
| Inference Server | TorchServe (reference) |

---

## Model Details

- **Model:** `YOLOv8_Small_RDD.pt` (89MB)
- **Location:** `RoadDamageDetection-main/models/`
- **Input Size:** 640x640
- **Classes:** 4 (cracks + potholes)

---

## Dataset

- **Location:** `1001_0/1001/`
- **Count:** 444 dashcam videos
- **Size:** ~5GB
- **Source:** Indian road footage

---

## Team

| Name | Program | USN |
|------|---------|-----|
| Sriram Velumuri | AIML | 1RV23AI117 |
| Nitya Sharma | AIML | 1RV23AI071 |
| Bhoomi Reddy Venkata Narasimha Reddy | ME | 1RV23ME032 |
| Bulusu Vyaghri Ramachandra Vivek | ME | 1RV23ME036 |

**Guide:** Dr. Harsha, Assistant Professor, ECE

---

## Troubleshooting

### Streamlit won't start
```bash
# Check Python version
python --version  # Should be 3.10+

# Reinstall dependencies
pip install -r requirements.txt --force-reinstall
```

### Webcam not detected
- Check browser permissions for camera access
- Try a different browser (Chrome recommended)
- Ensure no other app is using the webcam

### CUDA/GPU errors
The app works on CPU by default. For GPU:
```bash
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118
```

### Model download fails
Model is included in `models/` folder. If missing:
```bash
# Download manually
curl -L -o models/YOLOv8_Small_RDD.pt https://github.com/oracl4/RoadDamageDetection/raw/main/models/YOLOv8_Small_RDD.pt
```

---

## What's Next (Roadmap)

- [ ] LangGraph multi-agent system (ValidatorAgent, DispatcherAgent)
- [ ] Apache Flink real-time ETL pipeline
- [ ] Mobile app (React Native)
- [ ] IMU sensor integration for pothole depth estimation
- [ ] Municipal API integration
- [ ] Firebase push notifications

---

## License

Academic project - RV College of Engineering, Bengaluru
