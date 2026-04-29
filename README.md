# Line-Following Robot

> Autonomous robot with PID control algorithm — built for competitive robotics. Three firmware variants: line-following, obstacle avoidance, and Bluetooth remote control.

[![C++](https://img.shields.io/badge/C%2B%2B-Arduino-00979D?style=flat-square&logo=arduino&logoColor=white)](https://arduino.cc)
[![Hardware](https://img.shields.io/badge/Hardware-Arduino%20Uno-00979D?style=flat-square)](https://arduino.cc)
[![License](https://img.shields.io/badge/License-MIT-01696f?style=flat-square)](LICENSE)

---

## Overview

A fully autonomous robot that follows a black line on a white surface using infrared sensors and a **PID (Proportional-Integral-Derivative) control loop** for smooth, accurate navigation. Developed for robotics competitions, with additional modes for obstacle avoidance and Bluetooth remote control.

---

## Hardware

| Component | Quantity | Notes |
|---|---|---|
| Arduino Uno | 1 | Main microcontroller |
| IR Sensor Array | 2 | Left + Right line detection |
| L298N Motor Driver | 1 | Dual H-bridge for DC motors |
| DC Gear Motors | 2 | 6V, 150 RPM |
| HC-SR04 Ultrasonic Sensor | 1 | Obstacle detection mode |
| HC-05 Bluetooth Module | 1 | Remote control mode |
| Li-Po Battery | 1 | 7.4V, 1000mAh |
| Robot Chassis | 1 | Two-wheel differential drive |

---

## Wiring Diagram

```
Arduino Uno
├── D2  ──────────── IR Sensor LEFT (OUT)
├── D3  ──────────── IR Sensor RIGHT (OUT)
├── D5  ──────────── L298N ENA (PWM — Left Motor Speed)
├── D6  ──────────── L298N ENB (PWM — Right Motor Speed)
├── D7  ──────────── L298N IN1
├── D8  ──────────── L298N IN2
├── D9  ──────────── L298N IN3
├── D10 ──────────── L298N IN4
├── D11 ──────────── HC-SR04 TRIG (obstacle mode)
├── D12 ──────────── HC-SR04 ECHO (obstacle mode)
├── RX  ──────────── HC-05 TX (bluetooth mode)
└── TX  ──────────── HC-05 RX (bluetooth mode)
```

---

## PID Control — How It Works

The robot uses a **PID loop** to calculate corrective motor speed adjustments in real time:

```
Error = LeftSensor - RightSensor

P (Proportional) — instant correction proportional to current error
I (Integral)     — corrects accumulated drift over time
D (Derivative)   — predicts future error from rate of change

Correction = (Kp × Error) + (Ki × ∫Error dt) + (Kd × d(Error)/dt)

LeftMotorSpeed  = BaseSpeed + Correction
RightMotorSpeed = BaseSpeed - Correction
```

Tuned constants: `Kp = 30`, `Ki = 0.0`, `Kd = 10` (optimized for indoor competition tracks).

---

## Firmware Variants

| File | Mode | Description |
|---|---|---|
| `Código Seguidor de linha (Pega pega)` | Line Following | Core PID algorithm for track navigation |
| `Código seguidor de linha (Desvia Obstáculo)` | Obstacle Avoidance | Ultrasonic sensor + line following hybrid |
| `Código controle bluetooth` | Bluetooth Remote | HC-05 module for manual smartphone control |

---

## Getting Started

### Prerequisites

- [Arduino IDE](https://arduino.cc/en/software) 2.0+
- USB-A to USB-B cable

### Upload Firmware

```bash
# 1. Clone the repository
git clone https://github.com/MarcosCF1/Rob-Seguidor-de-Linha.git

# 2. Open Arduino IDE
# 3. File > Open > choose the firmware variant (.ino file)
# 4. Select board: Tools > Board > Arduino Uno
# 5. Select port: Tools > Port > (your COM port)
# 6. Upload: Sketch > Upload (Ctrl+U)
```

### Calibration

Before the first run, place the robot on the track and run the calibration loop (uncomment `#define CALIBRATE` in the sketch). The robot will sweep both sensors across the line to determine threshold values automatically.

---

## Results

- Successfully completed full competition track without derailment
- Average lap time: **~8 seconds** on 2m×1m figure-8 track
- Obstacle avoidance: detected objects at ≤15cm, re-routed cleanly

---

## Author

**Marcos Pires** — Full Stack Developer & Embedded Enthusiast

[![LinkedIn](https://img.shields.io/badge/LinkedIn-marcos--pires--dev-0A66C2?style=flat-square&logo=linkedin)](https://linkedin.com/in/marcos-pires-dev)
[![GitHub](https://img.shields.io/badge/GitHub-MarcosCF1-181717?style=flat-square&logo=github)](https://github.com/MarcosCF1)

---

## License

MIT License — see [LICENSE](LICENSE) for details.
