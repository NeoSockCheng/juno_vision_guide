# Juno Vision Guide

<div align="center">

![ROS](https://img.shields.io/badge/ROS-Noetic-22314E?style=for-the-badge&logo=ros&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![YOLOv8](https://img.shields.io/badge/YOLOv8-Object_Detection-00FFFF?style=for-the-badge&logo=yolo&logoColor=black)
![OpenCV](https://img.shields.io/badge/OpenCV-Computer_Vision-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![Google AI](https://img.shields.io/badge/Gemini_2.5-AI_NLP-4285F4?style=for-the-badge&logo=google&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-18.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)

</div>

> **AI-Powered Voice-Controlled Object Detection & Distance Estimation System**  
> A sophisticated ROS-based vision assistant demonstrating distributed system design, real-time computer vision, and multi-AI service integration.

---

## 🎯 Project Overview

An intelligent robotics application that combines **Google Gemini AI**, **YOLOv8 object detection**, and **Depth Pro API** to enable natural language object finding with accurate distance estimation. Users simply speak commands like *"Find my phone"* and the system detects, locates, and announces the distance to the object.

### 📹 Demo

> **"Find my phone"** → System detects → **"The cell phone is approximately 1.5 meters away"**

*Note: Add demo.gif here showcasing the system in action*

### 📊 Project Stats

- **5 ROS Nodes** working in distributed architecture
- **80+ Object Classes** supported via YOLO
- **7 ROS Topics** for inter-node communication  
- **3 AI APIs** integrated (Gemini, Depth Pro, Speech Recognition)
- **30+ FPS** real-time object detection
- **<2s** average response time (excluding depth API)

### Core Technologies

| Category | Stack |
|----------|-------|
| **Framework** | ROS Noetic, Python 3.10+ |
| **Computer Vision** | YOLOv8, OpenCV |
| **AI/NLP** | Google Gemini 2.5 Flash |
| **Depth Estimation** | Depth Pro API (Hugging Face) |
| **Voice I/O** | Google Speech Recognition, Google TTS |

---

## 🏗️ System Architecture

**Distributed ROS Architecture** with 5 interconnected nodes:

```mermaid
graph LR
    A[🎤 Speech Recognition<br/>Google Speech API]
    B[🧠 AI Processing<br/>Gemini 2.5 Flash]
    C[👁️ Object Detection<br/>YOLOv8]
    D[📏 Depth Estimation<br/>Depth Pro API]
    E[🔊 Text-to-Speech<br/>Google TTS]
    
    A -->|Voice Command| B
    B -->|Target Object| C
    C -->|Image + BBox| D
    D -->|Distance Result| E
    E -.->|Audio Feedback| A
    
    style A fill:#4285F4,stroke:#333,stroke-width:2px,color:#fff
    style B fill:#34A853,stroke:#333,stroke-width:2px,color:#fff
    style C fill:#FBBC04,stroke:#333,stroke-width:2px,color:#000
    style D fill:#EA4335,stroke:#333,stroke-width:2px,color:#fff
    style E fill:#9334E6,stroke:#333,stroke-width:2px,color:#fff
```

**ROS Topics Pipeline**:
```mermaid
graph TD
    SR[Speech Recognition Node] -->|item_finder_input| SP[Speech Processing Node]
    SP -->|item_finder_object| OD[Object Detection Node]
    SP -->|item_finder_response| TTS[TTS Node]
    OD -->|detected_object_bbox| DE[Depth Estimation Node]
    OD -->|detected_object_image/compressed| DE
    DE -->|item_finder_response| TTS
    DE -->|depth_status| SP
    SP -->|item_finder_sr_termination| SR
    
    style SR fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    style SP fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style OD fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style DE fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    style TTS fill:#e8f5e9,stroke:#388e3c,stroke-width:2px
```

**Key Design Patterns**:
- **Pub/Sub Messaging**: 7 ROS topics for asynchronous communication
- **State Synchronization**: Coordinated workflow management across nodes
- **Error Recovery**: 20-second timeouts with user re-prompting
- **API Integration**: RESTful calls to cloud-based AI services

---

## 🔄 User Workflow

```mermaid
sequenceDiagram
    participant U as 👤 User
    participant SR as 🎤 Speech Recognition
    participant AI as 🧠 Gemini AI
    participant YL as 👁️ YOLOv8
    participant DP as 📏 Depth Pro
    participant TTS as 🔊 TTS
    
    U->>SR: "Find my phone"
    SR->>AI: Voice transcription
    AI->>AI: Extract "cell phone"
    AI->>YL: Target: "cell phone"
    AI->>TTS: "Finding object: cell phone"
    TTS->>U: Audio announcement
    
    loop Real-time Detection (20s timeout)
        YL->>YL: Process camera frames
    end
    
    YL->>DP: Image + Bounding Box
    YL->>TTS: "Estimating distance..."
    TTS->>U: Audio update
    
    DP->>DP: GPU depth calculation
    DP->>TTS: Distance: 1.5m
    TTS->>U: "Cell phone is 1.5 meters away"
    
    TTS->>U: "Tell me what to find..."
    U->>SR: [Next query...]
```

---

## ⚡ Key Features

- ✅ **Voice-Controlled Object Finding** - Natural language queries (*"Where is my laptop?"*)
- ✅ **Real-Time Vision Processing** - YOLOv8 detection at 30+ FPS
- ✅ **Accurate Distance Estimation** - GPU-powered depth calculation
- ✅ **80+ Object Classes** - Comprehensive YOLO object support
- ✅ **Hands-Free Operation** - Complete audio interaction loop
- ✅ **Smart NLP** - Synonym mapping (iPhone→phone, MacBook→laptop)

---

## 🚀 Quick Start

### Prerequisites
- Ubuntu 18.04 + ROS Noetic
- Python 3.10+ (Anaconda)
- USB Camera + Microphone

### Installation

```bash
# Clone repository
cd ~/catkin_ws/src/
git clone https://github.com/NeoSockCheng/juno-vision-guide.git
cd juno-vision-guide

# Setup environment
conda env create -f environment.yml
conda activate juno_vision_guide

# Configure API keys
# Add your keys to scripts/.env:
# GEMINI_API_KEY=your-key-here
# DEPTH_PRO_API_KEY=your-key-here

# Build workspace
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```

### Run System

```bash
# Terminal 1: Start ROS core
roscore

# Terminal 2: Launch all nodes
roslaunch juno_vision_guide juno_vision_guide.launch
```

**Usage**: Wait for *"Tell me what you want to find..."* → Speak command → System responds with distance

---

## 📊 Technical Highlights

### 1. **Distributed ROS Nodes**
Five independent Python nodes communicating via topics:
- `google_sr.py` - Speech-to-text with ambient noise adjustment
- `speech_input.py` - Gemini AI object extraction + validation
- `object_detection.py` - YOLOv8 inference with bounding boxes
- `object_depth_estimation.py` - API-based depth calculation
- `google_tts.py` - Text-to-speech feedback

### 2. **AI-Powered NLP**
```python
# Gemini prompt engineering for object extraction
prompt = f"""From: '{user_input}', extract the main object.
Valid objects: {yolo_classes}. Handle synonyms (iphone→phone).
Reply with object name or 'No object extracted'."""
```

### 3. **Computer Vision Pipeline**
- **YOLO Inference**: 70% confidence threshold
- **Bounding Box Export**: JSON with coordinates
- **Image Compression**: JPEG encoding for API transmission
- **Timeout Handling**: 20-second detection window

### 4. **State Management**
Global flags synchronize asynchronous workflows:
```python
depth_busy = False  # Blocks speech input during processing
object_captured = False  # Prevents duplicate detections
```

---

## 🔧 Configuration

| Parameter | Default | Location |
|-----------|---------|----------|
| Camera Device | `1` | `google_sr.py` |
| Microphone Index | `1` | `google_sr.py` |
| Confidence Threshold | `0.7` | `object_detection.py` |
| Detection Timeout | `20s` | `object_detection.py` |
| Depth API Endpoint | Hugging Face | `object_depth_estimation.py` |

---

## 📁 Project Structure

```
juno_vision_guide/
├── scripts/                    # ROS nodes (Python)
│   ├── google_sr.py           # Speech recognition
│   ├── speech_input.py        # Gemini NLP processing
│   ├── object_detection.py    # YOLOv8 detection
│   ├── object_depth_estimation.py  # Depth API integration
│   └── google_tts.py          # Text-to-speech
├── launch/
│   └── juno_vision_guide.launch  # Multi-node orchestration
├── environment.yml            # Conda dependencies
├── yolov8n.pt                # YOLO model weights (6.5MB)
└── yolo_object_list.json     # 80 object class mappings
```

---

## 🎓 Skills Demonstrated

<table>
<tr>
<td width="50%">

### 🤖 Robotics & Systems
- **ROS Architecture** - Multi-node orchestration
- **Pub/Sub Messaging** - Asynchronous communication
- **Distributed Systems** - 5-node coordination
- **State Management** - Global flags & synchronization
- **Launch Files** - System bootstrapping

### 💻 Computer Vision
- **YOLOv8** - Real-time object detection
- **OpenCV** - Image processing & manipulation
- **Bounding Box** - Detection data extraction
- **Image Compression** - JPEG encoding/decoding

### 🧠 AI/ML Integration
- **Google Gemini API** - NLP & prompt engineering
- **LLM Integration** - Contextual object extraction
- **Synonym Mapping** - Intelligent name resolution

</td>
<td width="50%">

### ☁️ Cloud & APIs
- **RESTful APIs** - HTTP POST/GET requests
- **Authentication** - API key management
- **Base64 Encoding** - Image transmission
- **Error Handling** - API timeout/retry logic

### 🎙️ Audio Processing
- **Speech Recognition** - Google Speech API
- **TTS Synthesis** - Voice feedback generation
- **Noise Adjustment** - Ambient audio filtering

### 🐍 Python Development
- **Threading** - Asynchronous workflows
- **Error Recovery** - Graceful degradation
- **JSON Processing** - Data serialization
- **Environment Variables** - Configuration management

</td>
</tr>
</table>

---

## 📜 License

MIT License - Open source for educational and commercial use

---

## 🔗 Links

- **YOLO**: [Ultralytics YOLOv8](https://github.com/ultralytics/ultralytics)
- **Gemini API**: [Google AI Studio](https://aistudio.google.com/)
- **Depth Pro**: [Hugging Face Space](https://huggingface.co/spaces/yzh70/depth-pro)

---

*Built with ❤️ for intelligent robotics applications*