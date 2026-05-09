# 🤖 Autonomous Line-Following Robot
**Ulster University — EEE416 Module | Year 2**

A two-wheeled differential drive robot that autonomously follows a black line on a white surface using five IR sensors, halts when an obstacle is detected, and streams live telemetry to a custom PC dashboard over Bluetooth Low Energy (BLE).

---

## ⚙️ Hardware
| Component | Part |
|---|---|
| Microcontroller | Arduino MKR WiFi 1010 |
| Motor Driver | TB6612FNG Dual H-Bridge |
| Line Sensors | 5-channel TCRT5000 IR Array |
| Obstacle Detection | HC-SR04 Ultrasonic Sensor |
| Comms | Bluetooth Low Energy (BLE) |

---

## 💻 Software
| Language | Purpose |
|---|---|
| C++ (Arduino) | Robot firmware — line following, obstacle detection, BLE comms |
| Python | PC dashboard — live telemetry, graphs, manual control |

---

## 🧠 How It Works

### Line Following
A five-sensor proportional algorithm reads the TCRT5000 IR array and steers accordingly:
- **Centre sensor only** → go straight
- **Inner off-centre sensor** → gentle curve correction
- **Outer sensor triggered** → sharp spin turn to recover
- **No sensors detect line** → stop and wait

### Obstacle Detection
The HC-SR04 ultrasonic sensor fires a pulse every loop iteration. If the measured distance falls below a configurable threshold (default 15 cm), all motor output is cut immediately. Manual reverse is still permitted so the robot can back away from the obstacle.

### Emergency Stop
A physical push button is wired to a hardware interrupt. When pressed, an ISR sets a flag that the main loop acts on immediately — stopping all motors and disabling the driver regardless of the current mode.

### BLE Communication
The robot advertises a custom GATT service with two characteristics:
- **Command** (write) — PC sends ASCII strings: `START`, `STOP`, `AUTO`, `FWD`, `SPEED=180`, `STOPDIST=20`, etc.
- **Telemetry** (notify) — Robot pushes a CSV string at 10Hz: sensor states, speed, distance, mode, PWM values

---

## 📊 PC Dashboard (Python)
Built with **Tkinter + Bleak** (async BLE library). Features:
- Live distance and speed graphs (rolling 8-second window)
- IR sensor indicators (green = line detected)
- Manual keyboard control (arrow keys / space / enter)
- Real-time statistics: acceleration, turning state, move time %, obstacle reaction time
- Configurable speed and stop distance sent to robot over BLE

---

## 🛠️ Tech Stack
`C++` `Arduino` `Python` `Tkinter` `Bleak` `BLE / GATT` `PWM Motor Control` `IR Sensors` `Ultrasonic Sensing` `Hardware Interrupts`

---
## Presentation Link
[EEE416_LineFollowingRobot_PowerPoint.pdf](https://github.com/user-attachments/files/27557085/EEE416_LineFollowingRobot_PowerPoint.pdf)

---
## Demo Video Link
https://ulster-my.sharepoint.com/:v:/r/personal/keenan-c36_ulster_ac_uk/Documents/EEE416%20Project.mov?csf=1&web=1&e=ynZmWD&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D

---
## 👤 Authors
**Jacob Dunlevy & Cillian Keenan** — MEng Mechatronics Engineering, Ulster University  
[LinkedIn](https://www.linkedin.com/in/jacob-dunlevy-24861a388)
