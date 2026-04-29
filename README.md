<div align="center">

# Line-Following Robot
### Autonomous Competition Robot with PID Control

[![Arduino](https://img.shields.io/badge/Arduino-C%2FC%2B%2B-00979D?style=flat-square&logo=arduino&logoColor=white)](https://arduino.cc)
[![Firmware](https://img.shields.io/badge/Firmware-3%20Variants-30363d?style=flat-square)](#firmware-variants)
[![Competition](https://img.shields.io/badge/Track%20Complete-%E2%9C%94%20Validated-01696f?style=flat-square)](#results)
[![License](https://img.shields.io/badge/License-MIT-30363d?style=flat-square)](LICENSE)

*Fully autonomous differential-drive robot with a hand-tuned PID control loop — built for robotics competitions.*

</div>

---

## The Engineering Problem

Building a line-following robot seems simple until you run it. A naive two-state controller ("turn left if the left sensor loses the line, turn right if the right does") oscillates violently and derails on curves. The real challenge is **smooth continuous correction** — which requires understanding control theory.

This project implements a full **PID (Proportional-Integral-Derivative) control loop** in C on an Arduino Uno, achieving stable line tracking at competitive speeds through iterative tuning. Three firmware variants were developed to explore different control strategies: pure line-following, obstacle avoidance, and Bluetooth remote control.

---

## How PID Control Works Here

The robot reads two infrared sensors (left and right) 200 times per second. Each reading generates an **error signal** that feeds into three control terms:

```
ERROR = LeftSensor_value − RightSensor_value

┌────────────────────────────────────────────────────┐
│  P (Proportional)  Kp × error                              │
│                    React now — big error → big correction  │
├────────────────────────────────────────────────────┤
│  I (Integral)      Ki × ∫error dt                          │
│                    Correct drift accumulated over time      │
├────────────────────────────────────────────────────┤
│  D (Derivative)    Kd × d(error)/dt                        │
│                    Predict overshoot; dampen oscillation    │
└────────────────────────────────────────────────────┘

Correction = (Kp × error) + (Ki × Σerror) + (Kd × Δerror)

Left  motor PWM = BASE_SPEED + Correction   (0–255)
Right motor PWM = BASE_SPEED − Correction   (0–255)
```

**Tuned constants** (competition configuration):
```cpp
float Kp = 30.0;   // Aggressive proportional — fast reaction
float Ki = 0.0;    // Integral disabled — track too short for drift to accumulate
float Kd = 10.0;   // Derivative active — damps oscillation on tight curves
int   BASE_SPEED = 150;  // ~59% PWM — fast but controllable
```

> **Why Ki = 0?** On a short competition track (≤3m), the robot doesn't accumulate meaningful steady-state error. Enabling Ki added integral windup and made corners worse. Real-world tuning beat textbook theory.

---

## Hardware

| Component | Model | Role |
|---|---|---|
| Microcontroller | Arduino Uno (ATmega328P) | Main control loop at 16MHz |
| Line sensors | IR sensor module ×2 | Left/right line detection |
| Motor driver | L298N dual H-bridge | PWM speed control for 2 motors |
| Drive motors | DC gear motors ×2 (6V, 150RPM) | Differential drive |
| Obstacle sensor | HC-SR04 ultrasonic | 2cm–40cm detection range |
| Bluetooth module | HC-05 (UART) | Smartphone remote control |
| Power | Li-Po 7.4V / 1000mAh | Stable voltage for motors + MCU |
| Chassis | 2-wheel differential + caster | Low center of gravity |

### Wiring

```
Arduino Uno
├─ D2  ──────────── IR Sensor LEFT  (signal)
├─ D3  ──────────── IR Sensor RIGHT (signal)
├─ D5  ──────────── L298N ENA  (PWM — left motor speed)
├─ D6  ──────────── L298N ENB  (PWM — right motor speed)
├─ D7  ──────────── L298N IN1  (left motor direction A)
├─ D8  ──────────── L298N IN2  (left motor direction B)
├─ D9  ──────────── L298N IN3  (right motor direction A)
├─ D10 ──────────── L298N IN4  (right motor direction B)
├─ D11 ──────────── HC-SR04 TRIG (obstacle mode)
├─ D12 ──────────── HC-SR04 ECHO (obstacle mode)
├─ RX  ──────────── HC-05 TX (bluetooth mode)
└─ TX  ──────────── HC-05 RX (bluetooth mode)
```

---

## Firmware Variants

Three complete, independently flashable firmware files are included:

| File | Mode | Key Feature |
|---|---|---|
| `seguidor_linha_pegapega.ino` | **Line Following (PID)** | Core competition firmware; fully tuned PID loop |
| `seguidor_linha_desvio.ino` | **Obstacle Avoidance** | Adds HC-SR04 interrupt: detects obstacle ≤ 15cm, stops, re-routes |
| `controle_bluetooth.ino` | **Bluetooth Remote** | HC-05 UART; smartphone sends F/B/L/R commands over serial |

---

## Getting Started

### Prerequisites
- [Arduino IDE](https://arduino.cc/en/software) 2.0+
- USB-A to USB-B cable
- Assembled robot (see hardware table)

### Flashing

```bash
# Clone the repository
git clone https://github.com/MarcosCF1/Rob-Seguidor-de-Linha.git

# Open Arduino IDE
# 1. File > Open > select the .ino variant you want
# 2. Tools > Board > Arduino Uno
# 3. Tools > Port > select your COM port
# 4. Sketch > Upload  (Ctrl+U)
```

### Sensor Calibration

Before first run, place the robot on the track (sensor straddling the line) and uncomment `#define CALIBRATE` in the sketch. The robot will sweep across the line and auto-detect threshold values. Re-comment and reflash before competition.

### Tuning PID Constants

1. Set `Ki = 0`, `Kd = 0` — tune `Kp` first until the robot tracks but oscillates
2. Increase `Kd` gradually until oscillation dampens
3. Only add `Ki` if the robot consistently drifts to one side on a long straight

---

## Results

- ✅ Completed the full competition track without derailment
- ⏱ Average lap time: **~8 seconds** on a 2m × 1m figure-8 track
- 🛎️ Obstacle avoidance: detected objects at ≤15cm, re-routed cleanly in all tests
- 📱 Bluetooth control: full directional response with <50ms latency over 5m range

---

## What This Project Teaches

This project is a good study of how **hardware constraints force better software**. The loop frequency is limited by the Arduino's 16MHz clock and analog read time, which means you can't just throw compute at poor algorithm design. Every optimization (integer math vs float, sensor polling vs interrupts) has a measurable effect on performance — a feedback loop that trains better systems thinking.

---

## Author

**Marcos Pires** — Full Stack Developer & AI Integration Engineer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-marcos--pires--dev-0A66C2?style=flat-square&logo=linkedin)](https://linkedin.com/in/marcos-pires-dev)
[![GitHub](https://img.shields.io/badge/GitHub-MarcosCF1-181717?style=flat-square&logo=github)](https://github.com/MarcosCF1)

---

<sub>MIT License — built for competition, documented for learning.</sub>
