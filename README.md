# The Aerial Guardian

Drone-Based Multi-Object Tracking for Aerial Surveillance

A robust aerial surveillance system for pedestrian detection and multi-object tracking in drone footage using fine-tuned YOLO11s, adaptive small-object detection, ego-motion compensation, and appearance-aware tracking.

---

## Demonstration

Tracking output generated on the VisDrone benchmark:

*Insert GitHub video link here after uploading `uav0000086_00000_v_tracked.mp4`.*

---

## Project Overview

Drone-based surveillance introduces several challenges not commonly encountered in fixed-camera tracking systems:

* Small pedestrians occupying only a few pixels
* Camera translation, rotation, and scale changes
* Occlusions and dense crowds
* Real-time deployment constraints

The Aerial Guardian addresses these challenges through a specialized aerial detection and tracking pipeline.

## System Architecture

```text
Drone Frame
    │
    ▼
Adaptive SAHI
    │
    ▼
YOLO11s + P2 Head
    │
    ▼
ORB + RANSAC Ego-Motion Compensation
    │
    ▼
Kalman Prediction
    │
    ▼
Hungarian Association
    │
    ▼
Lost Track Recovery
    │
    ▼
OSNet ReID (Experimental)
    │
    ▼
Track Visualization
    │
    ▼
Tracked Video Output
```

## Key Features

### Detection

* Fine-tuned YOLO11s detector
* P2 detection head for small objects
* VisDrone-specific domain adaptation
* Adaptive SAHI for distant pedestrians

### Tracking

* Kalman motion prediction
* Hungarian assignment
* Lost-track recovery
* BoT-SORT-inspired association pipeline

### Camera Motion Compensation

* ORB feature extraction
* RANSAC homography estimation
* Rotation, scale, and perspective handling

### Appearance Modeling

* OSNet-based ReID encoder
* Appearance embedding generation
* Experimental identity preservation module

### Deployment

* YOLO11s exported to ONNX
* OSNet exported to ONNX
* Edge deployment ready
* Sub-300 MB model budget

## Training Configuration

| Parameter    | Value        |
| ------------ | ------------ |
| Base Model   | YOLO11s      |
| Dataset      | VisDrone DET |
| Target Class | Person       |
| Epochs       | 50           |
| Image Size   | 640          |
| Batch Size   | 16           |
| Framework    | Ultralytics  |

## Small Object Detection Strategy

Pedestrians in aerial imagery frequently occupy only 15–25 pixels.

To improve detection performance:

* Fine-tuned YOLO11s on VisDrone
* Added a stride-4 P2 detection head
* Integrated Adaptive SAHI

This improves recall while maintaining deployment efficiency.

## Identity Switch Mitigation

Identity switches were addressed using:

* ORB + RANSAC ego-motion compensation
* Kalman prediction
* Lost-track recovery
* Experimental OSNet appearance embeddings

These components improve tracking robustness under occlusions and drone motion.

## Failure Cases

Remaining challenges include:

* Dense crowds
* Long-term occlusions
* Motion blur
* Extremely small pedestrians

These cases are discussed in detail within the accompanying notebook.

## Deployment

### Exported Models

* YOLO11s → ONNX
* OSNet ReID → ONNX

### Deployment Targets

* NVIDIA Jetson Nano
* NVIDIA Xavier NX
* NVIDIA Jetson Orin NX
* ONNX Runtime
* TensorRT

### Model Budget

Target deployment constraint:

```text
< 300 MB
```

## Technology Stack

### Detection

* YOLO11s
* Ultralytics

### Tracking

* Kalman Filter
* Hungarian Assignment
* BoT-SORT-inspired tracking

### Computer Vision

* OpenCV
* ORB Features
* RANSAC Homography

### Re-Identification

* OSNet
* TorchReID

### Deployment

* ONNX
* TensorRT Ready

## Repository Structure

```text
.
├── notebooks/
│   └── Aerial_Guardian.ipynb
│
├── assets/
│   ├── demo.mp4
│   ├── architecture.png
│   ├── training_metrics.png
│   └── qualitative_results.png
│
├── exports/
│   ├── yolo11s.onnx
│   └── osnet_reid.onnx
│
├── weights/
│   └── best.pt
│
└── README.md
```

## Future Work

* Full appearance-aware BoT-SORT integration
* TensorRT optimization
* Multi-camera tracking
* Transformer-based MOT
* Real-time Jetson deployment

## Author

Sanat Walia

B.Tech Computer Science Engineering

Project developed for The Aerial Guardian Technical Assessment.
