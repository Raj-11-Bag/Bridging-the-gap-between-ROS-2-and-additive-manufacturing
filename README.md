# ROS2 ReBeL + Marlin Coordinator 
This repository contains the ROS 2 package rmc (ReBeL-Marlin Coordinator) used in the master thesis: 
**“Bridging ROS2 and Additive Manufacturing: A Real-Time Control Approach for Continuous Deposition Tasks”** 


**Thesis Project:** Bridging the Gap Between ROS2 and Additive Manufacturing — A Real-Time Control Approach for Continuous Deposition Tasks  
**Supervisor:** Prof. Ajinkya Keskar  
**Institution:** Srh Berlin University  
**Grade:** 1.0 

### 🧩 Project Summary
This project presents a real-time control system integrating an **igus ReBeL 6-DOF robotic arm** with a **Marlin-controlled extrusion unit**, coordinated through **ROS2 and TCP/Serial communication**.  
The system enables **continuous, synchronized material deposition** for robotic additive manufacturing using **PLA/PETG** materials.

### ⚙️ Key Features
- Real-time ROS2–Marlin synchronization  
- TCP-based CRI control for igus ReBeL  
- Serial G-code streaming to SKR Mini E3 controller  
- Modular Python-based control interface  
- Dynamic extrusion control via multi-threaded architecture
- ROS 2 node for coordinating robot (CRI/TCP) and SKR/Marlin extruder 
- Ack-aware windowing scheme (N = [8,12]) at 50 Hz
- Preamble handling for PETG deposition
- In-flight monitoring via ROS 2 topics (/coordinator/heartbeat, /coordinator/inflight) #

### 🔗 Communication Interfaces
- **Robot Control:** TCP/IP (CRI protocol, igus ReBeL API)  
- **Extruder Control:** Serial (Marlin G-code, SKR Mini E3 V3.0)  

### 🧠 Technologies
`Python` · `ROS2` · `Marlin` · `igus ReBeL CRI` · `Serial Communication` · `TCP Sockets`  

### 📹 Demo Video
👉 [LinkedIn Post](#) | [Video Demo](#)

---

# Run
```
bash
colcon build --symlink-install
source install/setup.bash
ros2 run rmc coordinator --ros-args --params-file src/rmc/params.yaml
```
License

MIT


