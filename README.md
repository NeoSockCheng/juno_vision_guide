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

**Key Design Patterns**: Pub/Sub messaging (7 ROS topics) • State synchronization • 20s timeout with error recovery • RESTful API integration

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
Ubuntu 18.04 • ROS Noetic • Python 3.10+ • USB Camera + Microphone

### Installation & Run

```bash
# Clone & setup
cd ~/catkin_ws/src/
git clone https://github.com/NeoSockCheng/juno-vision-guide.git
conda env create -f environment.yml && conda activate juno_vision_guide

# Add API keys to scripts/.env: GEMINI_API_KEY, DEPTH_PRO_API_KEY

# Build & launch
cd ~/catkin_ws && catkin_make && source devel/setup.bash
roscore  # Terminal 1

roslaunch juno_vision_guide juno_vision_guide.launch  # Terminal 2
```

---

## 📊 Technical Implementation

**Distributed Architecture**: 5 Python ROS nodes (`google_sr.py`, `speech_input.py`, `object_detection.py`, `object_depth_estimation.py`, `google_tts.py`)

**AI/NLP**: Gemini 2.5 Flash with prompt engineering for object extraction + synonym mapping (iPhone→phone)

**Computer Vision**: YOLOv8 @ 70% confidence • JSON bounding boxes • JPEG compression • 20s timeout

**State Management**: Global flags (`depth_busy`, `object_captured`) for asynchronous workflow synchronization

**Configuration**: Camera/Mic device index: 1 • Detection timeout: 20s • Depth API: Hugging Face

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