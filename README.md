# Sensor Fusion Autonomous Lane Change System

Sensor Fusion autonomous lane change system implemented on a small-scale autonomous vehicle platform using ROS2.
The system performs lane detection, vehicle detection, and decision-making for overtaking using camera and LiDAR sensors.

## Demo

![lane change demo](https://github.com/user-attachments/assets/c14e20bb-3c93-4532-9c24-64a4baa22778)


The vehicle detects a slower vehicle ahead, performs a lane change to overtake, 
and safely returns to the original lane using the FSM-based decision logic.

<img width="490" height="364" alt="image" src="https://github.com/user-attachments/assets/0fa16f2a-ba4f-4755-acf9-a878e760b81b" />

YOLO-based Lane and Vehicle Detection

<img width="586" height="310" alt="차선 인식 예시" src="https://github.com/user-attachments/assets/dea590c9-a911-484d-a6c9-e5d0e9bbb99a" />

Example of 2nd lane recognition

## Overview

This project implements an autonomous lane change system based on a Finite State Machine (FSM).

The system detects lane markings and surrounding vehicles using camera and LiDAR, and makes lane-change decisions to safely overtake slower vehicles.  
The algorithm was implemented and tested on a small-scale electric vehicle platform equipped with onboard sensors and ROS2-based software modules.

### Key features:
- Lane detection
- Vehicle detection using YOLO
- FSM-based lane change decision
- Autonomous lane recovery

---

## System Architecture

Sensor data from the camera and LiDAR are processed through perception modules, and the FSM-based decision module determines the appropriate driving state.  
The control module then generates commands for the vehicle through UART communication.

```
Sensor (Camera / LiDAR)
        ↓
Perception (Lane Detection, YOLO Detection)
        ↓
Decision Making (FSM)
        ↓
Control
        ↓
Vehicle (UART Motor Control)
```
<img width="2408" height="1090" alt="image" src="https://github.com/user-attachments/assets/cbe2ebdd-10fa-42fc-b37f-bdef3696d955" />

---

## Hardware Plaform

The autonomous driving system is implemented on a small-scale electric ride-on vehicle platform.

### Vehicle Platform
- Ride-on electric vehicle body
- Drive motor
- Steering motor
- Wheels
- Battery

### Control Unit
- Arduino microcontroller
- Potentiometer (steering control)

### Sensors
- Camera: YOLO-based lane and vehicle detection. The image Y-coordinate is used to estimate the distance to the detected vehicle.
- LiDAR: Used for accurate distance measurement to the obstacle vehicle.

### Hardware Stack
- Platform: Small-scale Electric Vehicle
- Computing: Laptop
- Sensors: 2D LiDAR (Obstacle distance measurement) / Monocular Camera (YOLO-based detection)
- Actuators: Arduino-based DC/Steering Motor Control via UART

---

## Software Framework

- ROS2 node-based architecture
- Perception modules for lane and vehicle detection
- FSM-based decision-making module
- Vehicle control interface for steering and speed

### Software Stack
- OS: Ubuntu 22.04 / ROS2 Humble
- Perception: YOLOv8 (Lane & vehicle detection) / OpenCV (Lane segmentation)
- Communication: Custom Serial Protocol (ROS2 ↔ Arduino)

---

## Finite State Machine

The lane change decision is implemented using a five-state Finite State Machine (FSM).

The FSM consists of the following states:

1. **Normal Driving**  
   The vehicle follows the current lane during normal autonomous driving.

2. **Preparing to Change Lane**  
   When an obstacle is detected ahead, the system evaluates the adjacent lane and prepares for a lane change.

3. **Changing Lane**  
   The vehicle performs a lane change maneuver to avoid the obstacle.

4. **Overtaking**  
   The vehicle continues driving in the adjacent lane while passing the obstacle.

5. **Returning Lane**  
   After passing the obstacle, the vehicle safely returns to the original lane.

<img width="2924" height="1278" alt="image" src="https://github.com/user-attachments/assets/17ec6a25-aff9-4c29-9951-85d831fa2394" />

The vehicle changes lanes when an obstacle is detected and returns to the original lane after passing the obstacle.  
The FSM transitions between states based on lane detection, vehicle detection, and distance measurements.

---

## Experimental Scenarios

To evaluate the autonomous lane change system, four driving scenarios were designed.  

**Scenario 1 - Basic Overtaking**  
The vehicle ignores vehicles in the adjacent lane and continues driving.  
If a vehicle is detected ahead in the same lane, it changes lanes and then returns to the original lane.

**Scenario 2 - Consecutive Overtaking**  
The vehicle detects a vehicle ahead in the same lane and changes lanes to overtake.  
However, if another vehicle is detected ahead in the lane to return to, it overtakes the vehicle as well before returning.

**Scenario 3 - Lane Change with Rear Vehicle**  
The vehicle detects a vehicle ahead in the same lane.  
However, if another vehicle is detected approaching from behind in the target lane, it decelerates and waits.  
After the vehicle passes, it changes lanes and returns.

**Scenario 4 - Lane Change During Overtaking**
The vehicle detects a vehicle ahead in the same lane and changes lanes. 
While overtaking, it detects a vehicle ahead in the current lane, so it changes lanes again.  
Instead of initiating another overtaking maneuver, the system treats this lane change as a return to the original lane.

---

## Results

The system was tested in multiple driving scenarios and demonstrated reliable lane following and autonomous overtaking behavior.  
The experimental results confirmed that the proposed system can perform the following functions:

- Stable lane following
- Vehicle detection using camera and LiDAR
- Safe lane changes using FSM-based decision logic
- Autonomous return to the original lane

## Repository Structure
```
lane-change-fsm-system
├── code/src/                    # ROS2 기반 자율주행 시스템 소스 코드
│   ├── camera_perception_pkg/   # 이미지 데이터를 활용한 차선 및 차량 인식
│   ├── decision_making_pkg/     # FSM 기반 판단부
│   ├── interfaces_pkg/          # 노드 간 통신을 위한 커스텀 메시지
│   ├── launch_pkg/              # 다중 노드 실행 및 시스템 오케스트레이션을 위한 런치 파일
│   ├── lidar_perception_pkg/    # LiDAR 데이터를 활용한 전후방 장애물 유무 감지
│   └── serial_communication_pkg/ # PC와 차량 플랫폼 간의 UART 통신 인터페이스
|
├── docs/                        # 연구 관련 문서 및 학술 활동 자료
│   ├── ICCE_Conference_Paper/   # IEEE ICCE 국제 학회 채택 논문 및 포스터 자료
│   └── Industry_Project/        # 산학프로젝트 논문 및 우수 논문 발표 자료
|
├── media/                       # 프로젝트 시연 및 시각화 자료
│   ├── Scenarios/               # 주행 시나리오별 자율주행 시연 주행 영상
│   └── presentations/           # 산학 프로젝트 성과교류회 발표 영상
|
└── README.md                    # 프로젝트 개요, 시스템 아키텍처 등 안내 문서
```

## Acknowledgement

This project was developed as part of the Autonomous Driving Advanced Course at Sungkyunkwan University.

The experimental platform and basic framework were provided during the course. 
The perception pipeline using camera and LiDAR, the FSM-based lane-change decision 
algorithm, and the overall system integration were implemented in this project.
