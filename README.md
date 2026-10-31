# DriveGuard AI

**AI-based Driver Monitoring + ADAS Perception System — camera-only, retrofit-ready.**

DriveGuard AI fuses real-time road perception with driver-state monitoring into a single risk-assessment pipeline, alerting the driver before a lapse in attention becomes a collision — without relying on expensive LiDAR or radar hardware.

---

## Why this project

Advanced Driver Assistance Systems (ADAS) capable of monitoring both the road and the driver already exist — but almost exclusively in premium vehicles. Budget cars, two-wheelers, and commercial fleet vehicles that dominate transportation in emerging markets have no such protection, despite their drivers being among the most exposed to fatigue and distraction.

DriveGuard AI is a camera-only system built around lightweight, well-benchmarked computer vision models, designed to be deployable as a retrofit on any existing vehicle rather than a feature reserved for new, premium ones.

## How it works

The system is built around three interconnected layers:

| Layer | What it does | How |
|---|---|---|
| **1. Road Perception** | Detects vehicles, pedestrians, traffic signs, and lane boundaries | YOLOv8 object detection + lane detection |
| **2. Driver Monitoring** | Detects drowsiness and distraction from the driver's face | MediaPipe facial landmark tracking (eye-aspect-ratio, head pose) |
| **3. Risk Fusion** | Combines both signals into a single, continuously updated risk score and triggers alerts | Custom fusion logic + graded alert thresholds |

The risk score is sent to a physical alert unit — an Arduino Nano driving an OLED display and buzzer — so the alert is visible/audible in the vehicle itself, not just on a laptop screen.

## Architecture

```
┌─────────────────┐         ┌──────────────────┐
│  Road-facing cam  │     │  Driver-facing cam │
│   (USB webcam)    │     │   (laptop webcam)  │
└─────────┬─────────┘     └─────────┬──────────┘
          │                          │
          ▼                          ▼
 ┌──────────────────┐      ┌──────────────────────┐
 │  Road Perception │      │   Driver Monitoring  │
 │  (YOLOv8 + lanes)│      │  (MediaPipe + EAR)   │
 └─────────┬────────┘      └──────────┬───────────┘
           │                          │
           └───────────┬──────────────┘
                       ▼
              ┌ ────────────────────┐
              │    Risk Fusion      │
              │  (proximity + state)│
              └──────────┬──────────┘
                          │
             ┌────────────┴────────────┐
             ▼                         ▼
     ┌───────────────┐        ┌──────────────────┐
     │  Streamlit UI │        │  Arduino Nano    │
     │  (dashboard)  │        │  → OLED + Buzzer │
     └───────────────┘        └──────────────────┘
```

## Tech stack

**AI / ML**
- [YOLOv8](https://github.com/ultralytics/ultralytics) — object and lane detection
- [MediaPipe](https://github.com/google/mediapipe) — facial landmark tracking for drowsiness/distraction
- PyTorch — backing framework for YOLO training/fine-tuning
- MLflow — experiment tracking during model fine-tuning

**Backend**
- Python 3.10+
- OpenCV — video capture, classical lane detection, overlays
- NumPy — geometry and signal calculations

**Frontend / Dashboard**
- Streamlit — live camera feed, driver-state indicator, risk score display

**Hardware / Firmware**
- Arduino Nano (CH340) — receives risk signal over USB serial
- SSD1306 OLED display — visual alert
- Active buzzer module — audible alert
- Arduino C++ (via Arduino IDE)

**Datasets**
- [KITTI](http://www.cvlibs.net/datasets/kitti/) / [BDD100K](https://bdd-data.berkeley.edu/) — road-scene object and lane detection
- [NTHU-DDD](https://sites.google.com/view/drowsy-driver-detection-dataset) — driver drowsiness

## Hardware bill of materials

Built on a ~₹1,100 budget as a proof-of-concept retrofit kit:

| Component | Purpose |
|---|---|
| Arduino Nano R3 (CH340) | Drives OLED + buzzer, reads risk signal via serial |
| SSD1306 OLED display | On-device visual alert |
| Active buzzer module | On-device audible alert |
| USB webcam (480p) | Road-facing camera |
| Laptop webcam | Driver-facing camera |
| Prototype PCB + header strips | Permanent, removable-component circuit build |
| Dashboard mount (custom sunboard) | Positions the road-facing camera |

Full parts list and sourcing notes in [`/hardware`](./hardware).

## Project structure

```
driveguard-ai/
├── road_perception/     # YOLOv8 object detection + lane detection
├── driver_monitoring/   # MediaPipe drowsiness/distraction tracking
├── risk_fusion/         # Risk scoring and alert-threshold logic
├── dashboard/           # Streamlit UI
├── firmware/            # Arduino sketch (OLED + buzzer)
├── hardware/            # BOM, wiring diagrams, sourcing notes
├── data/                # Dataset subsets (gitignored)
└── README.md
```

## Roadmap

Built solo over an 11-week timeline:

- [x] **Week 1** — Environment setup, dataset acquisition, baseline YOLO test
- [ ] **Weeks 2–4** — Road perception layer (object + lane detection)
- [ ] **Weeks 5–7** — Driver monitoring layer (drowsiness + distraction)
- [ ] **Weeks 8–9** — Risk fusion layer
- [ ] **Week 10** — Dashboard + hardware integration
- [ ] **Week 11** — Polish, documentation, demo

## Getting started

```bash
git clone https://github.com/Alphaa1556/driveguard-ai.git
cd driveguard-ai

python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

pip install -r requirements.txt
```

See [`/firmware`](./firmware) for Arduino sketch upload instructions.

## Roots and future work

This project started as an extended abstract accepted for presentation at EDII Ahmedabad's 17th Biennial Conference on Entrepreneurship, framed around a retrofit "ADAS-as-a-Service" model for underserved vehicle segments.

The current build targets **cars** first, chosen for dataset and research maturity. Two-wheelers, auto-rickshaws, and commercial trucks are planned extensions once the core fusion architecture is validated.

## Author

**Yash Khanavkar**
BE Computer Science (AI & ML), MGM College of Engineering and Technology
[LinkedIn](https://linkedin.com/in/yashkhanavkar) · [GitHub](https://github.com/Alphaa1556)

---

*Built as a solo project for the Adhisthan tech community.*
