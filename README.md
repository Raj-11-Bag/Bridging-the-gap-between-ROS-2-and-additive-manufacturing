🚀 ROS 2-Based Real-Time Synchronization for Robotic Additive Manufacturing
📌 Overview

This repository presents a real-time synchronization framework for robotic additive manufacturing (AM), developed as part of a master’s thesis. The system enables continuous and stable material deposition by coordinating robot motion and extrusion using a ROS 2-based control architecture.

The key contribution is an acknowledgment-aware windowed streaming mechanism that ensures bounded latency and prevents extrusion starvation when interfacing with open-source firmware.

🎯 Motivation

Robotic additive manufacturing introduces flexibility in terms of:

Multi-axis deposition
Large-scale fabrication
Non-planar geometries

However, synchronization between robot motion and extrusion remains a major challenge, especially in open-source systems where:

Communication latency is variable
Controllers operate asynchronously
Buffering is not explicitly managed

This work addresses the lack of reliable real-time coordination between ROS 2 and extrusion firmware (Marlin).

🧠 Core Idea

Instead of naïvely streaming commands, this system implements a bounded, acknowledgment-driven command window:

Maintain a fixed range of in-flight commands
Use firmware acknowledgments (ok) as feedback
Dynamically regulate command transmission
🏗️ System Architecture
+---------------------+        TCP        +----------------------+
|     ROS 2 Node      | --------------->  |    Robot Controller  |
| (Coordinator Node)  |                  |     (igus ReBeL)     |
|                     |                  +----------------------+
|                     |
|                     | USB Serial
|                     v
|              +----------------------+
|              |   Marlin Firmware    |
|              |   (SKR Controller)   |
|              +----------------------+
|
+--------------------------------------------------------------+
⚙️ Hardware Setup
Component	Description
Robot	igus ReBeL (6-DOF robotic arm)
Extruder	Hemera XS
Controller	SKR Mini E3
Firmware	Marlin
Communication	TCP (robot), USB Serial (extruder)
💻 Software Stack
ROS 2 (Python-based node)
Custom coordinator node: /rebel_marlin_coordinator
rqt_graph (visualization)
CSV logging for data analysis
🔁 Control Loop

Frequency: 50 Hz (Δt = 20 ms)

Each cycle:

Compute robot motion (TCP trajectory)
Calculate extrusion increment
Send G-code command (G1 E...)
Receive acknowledgment (ok)
Update in-flight command count
Enforce window bounds
📐 Mathematical Model
Volumetric Flow
Q = w × h × v
Extrusion Rate
E_dot = Q / A_f
Discrete Extrusion
ΔE = E_dot × Δt

Where:

w = bead width
h = layer height
v = speed
A_f = filament cross-section
🔒 Synchronization Strategy
Window Constraints
N_min = 8
N_max = 12
Behavior
Condition	Action
inflight < N_min	Send commands (avoid starvation)
inflight > N_max	Pause sending (avoid overflow)
ack received	Decrease inflight
📊 Experimental Setup
Test Cases:
Straight-line deposition
Square plate
Continuous paths
Speeds:
30 mm/s
60 mm/s
📈 Results
Naïve Streaming
In-flight commands: >700
Starvation events: Present
Result: Discontinuous deposition
Windowed Streaming
In-flight commands: 8–12
Starvation events: None
Result: Continuous deposition
⏱️ Latency Analysis
Metric	Value
Median	~12 ms
p95	~25 ms
p99	~40 ms
📉 Performance Visualization
In-flight Commands
Naïve: Unbounded growth (>700)
Windowed: Stable (8–12)
Latency
Naïve: Up to ~3.5 s
Windowed: < 0.12 s
💰 Cost Analysis
Operational Cost
Component	Cost
Material	0.25–0.50 €/h
Energy	~0.07 €/h
Industrial Cost
Component	Cost
Labour	12.83 €/h
Depreciation	~1.04 €/h
Total	~16 €/h
⚠️ Challenges & Solutions
1. Synchronization mismatch

❗ Problem: Motion and extrusion unsynchronized
✅ Solution: Ack-aware windowing

2. Latency & jitter

❗ Problem: Communication delays
✅ Solution: Bounded in-flight queue

3. Starvation

❗ Problem: Empty planner buffer
✅ Solution: Maintain minimum window

4. Overflow

❗ Problem: Excess commands
✅ Solution: Enforce upper bound

5. Hardware instability

❗ Problem: Hotend silicone failure
✅ Solution: Identified and documented

🚧 Limitations
Open-loop extrusion (no feedback control)
Planar toolpaths only
No retraction implemented
Limited process optimization
🔮 Future Work
Integration with ros2_control
Closed-loop extrusion control
Multi-axis / curved printing
Vision-based monitoring
Process parameter optimization
🌟 Contributions
ROS 2-based robotic AM coordination
Acknowledgment-aware synchronization
Starvation-free continuous deposition
Open-source, reproducible framework
📂 Repository Structure
/src            → ROS 2 node implementation  
/data           → Experimental logs (CSV)  
/plots          → Latency & inflight graphs  
/images         → Print results & diagrams  
thesis.pdf      → Full thesis  
presentation.pdf→ Defense slides  
README.md       → Project documentation  
🎥 Demonstration

👉 [Insert Video Link Here]

🔗 References / Related Work
ROS 2 middleware
Marlin firmware
Robotic additive manufacturing literature
📬 Contact

Author: [Your Name]
Email: [Your Email]
Institution: [Your University]

🏁 Final Note

This project demonstrates that reliable real-time robotic additive manufacturing is achievable using low-cost, open-source systems when proper synchronization strategies are applied.
