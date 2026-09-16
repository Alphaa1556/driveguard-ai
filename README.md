DriveGuard AI

AI-based Driver Monitoring + ADAS Perception System — camera-only, retrofit-ready.

Status: 🚧 In progress — core perception pipeline under active development.

Overview

DriveGuard AI is a solo project combining road perception and driver monitoring into a single retrofit-friendly system that can run on consumer-grade hardware, without requiring OEM-level sensor integration. The goal is to detect risky driving conditions in real time — both what's happening on the road and how the driver is responding to it — and fuse the two into a single risk signal.

The project was presented to and approved by the Adhisthan tech community panel, and has been selected for the Eureka! (E-Cell IIT Bombay) startup competition, where it is being developed with a B2B2C licensing model for fleet operators.

Key Components
Road Perception (YOLOv8): Real-time detection of vehicles, pedestrians, lane boundaries, and obstacles from a single dashboard-mounted camera.
Driver Monitoring (MediaPipe): Facial landmark and eye/head-pose tracking to detect drowsiness, distraction, and gaze-off-road events.
Risk Fusion Layer: Combines road-perception and driver-monitoring signals into a single, actionable risk score, rather than treating them as independent alerts.
Scope

The initial version is scoped to passenger cars (multi-vehicle support was considered but deferred based on panel feedback, to prioritize a shippable, well-tested core first).

Hardware

Built around low-cost, off-the-shelf hardware components (camera module, processing unit, mounting hardware) to keep the system affordable and easy to retrofit into existing vehicles.

Roadmap
 Repo structure and initial planning
 Hardware procurement
 Road perception pipeline (YOLOv8 integration)
 Driver monitoring pipeline (MediaPipe integration)
 Risk fusion logic
 End-to-end testing on live camera feed
 Pitch deck refinement for Eureka!
Author

Yash Khanavkar (@Alphaa1556)
