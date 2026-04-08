
# 🚀 Real-Time ROS 2 Synchronization for Robotic Additive Manufacturing

A robust control framework enabling continuous, starvation-free material deposition by synchronizing robot motion and extrusion using ROS 2 and open-source firmware.

---

## 📌 Overview

This project presents a real-time synchronization framework for robotic additive manufacturing (AM), addressing a critical challenge in open-source systems:
reliable coordination between robot motion and extrusion under non-deterministic communication conditions.

A custom ROS 2 coordinator node is developed to regulate command transmission using an acknowledgment-aware windowing strategy, ensuring:

* Stable and continuous material deposition
* Bounded latency
* Elimination of extrusion starvation

---

## 🎯 Problem

In robotic AM, motion and extrusion are controlled by independent subsystems:

```bash
Robot     → trajectory execution (TCP motion)
Extruder  → firmware-based command execution (Marlin)
```

This leads to:

* ❌ Starvation → gaps in deposition
* ❌ Buffer overflow → delayed execution
* ❌ Loss of synchronization → poor print quality

Conventional streaming methods fail because they ignore execution feedback.

---

## 💡 Solution

This work introduces an acknowledgment-aware, bounded streaming mechanism:

* Maintain a controlled number of in-flight commands
* Use firmware acknowledgments (`ok`) as real-time feedback
* Dynamically regulate command flow

### 🔒 Window Control

```bash
N_min = 8    → Prevent starvation  
N_max = 12   → Prevent overflow  
```

This transforms an open-loop system into a feedback-driven synchronization process.

---

## 🏗️ System Architecture

```bash
          +----------------------------+
          |        ROS 2 Node          |
          |  (Coordinator Controller) |
          +------------+--------------+
                       |
             TCP       |        USB Serial
                       |
        +--------------v--------+     +----------------------+
        |   Robot Controller    |     |   Marlin Firmware    |
        |     (igus ReBeL)      |     |   (SKR Mini E3)      |
        +-----------------------+     +----------------------+
```

---

## ⚙️ Hardware

```bash
Robot: igus ReBeL (6-DOF)
Extruder: Hemera XS
Controller: SKR Mini E3
Firmware: Marlin
Interfaces: TCP (robot), USB serial (extruder)
```

---

## 💻 Software

```bash
ROS 2 (Python-based implementation)
Custom node: /rebel_marlin_coordinator
Data logging (CSV)
Visualization (rqt_graph)
```

---

## 🔁 Control Loop

```bash
Frequency: 50 Hz (Δt = 20 ms)
```

Each cycle executes:

* Robot motion update
* Extrusion computation
* G-code transmission (`G1 E...`)
* Acknowledgment reception (`ok`)
* In-flight queue update
* Window constraint enforcement

---

## 📐 Process Model

```bash
Q  = w × h × v
Ė = Q / A_f
ΔE = Ė × Δt
```

Where:

```bash
w   = bead width
h   = layer height
v   = speed
A_f = filament cross-section
```

---

## 📊 Experimental Validation

### Test Conditions

```bash
Straight-line deposition
Square geometry
Speeds: 30 mm/s and 60 mm/s
```

### 📉 Results Comparison

| Metric             | Naïve Streaming  | Windowed Control |
| ------------------ | ---------------- | ---------------- |
| In-flight commands | >700 (unbounded) | 8–12 (bounded)   |
| Starvation events  | Present          | None             |
| Deposition quality | Discontinuous    | Continuous       |

---

## ⏱️ Latency Performance

| Metric | Value  |
| ------ | ------ |
| Median | ~12 ms |
| p95    | ~25 ms |
| p99    | ~40 ms |

---

## 📈 Key Observations

* Without control → queue explosion + unstable extrusion
* With windowing → predictable, stable system behavior
* Continuous bead formation achieved in all test cases

---

## 💰 Cost Overview

### Operational Cost

```bash
Material: 0.25–0.50 €/h
Energy: ~0.07 €/h
```

### Industrial Estimate

```bash
Labour: 12.83 €/h
Depreciation: ~1.04 €/h
Total: ~16 €/h
```

---

## ⚠️ Challenges & Solutions

| Challenge                | Solution                 |
| ------------------------ | ------------------------ |
| Synchronization mismatch | Ack-aware windowing      |
| Communication latency    | Bounded command buffer   |
| Starvation               | Maintain minimum window  |
| Buffer overflow          | Enforce upper bound      |
| Hardware instability     | Diagnosed and documented |

---

## 🚧 Limitations

* Open-loop extrusion (no feedback sensors)
* Planar toolpaths only
* No retraction strategy
* Limited process tuning

---

## 🔮 Future Work

* Integration with ros2_control
* Closed-loop extrusion control
* Multi-axis / curved deposition
* Vision-based monitoring
* Process optimization

---

## 🌟 Contributions

* ROS 2-based robotic AM coordination
* Acknowledgment-aware synchronization strategy
* Starvation-free continuous deposition
* Fully reproducible open-source framework

---

## 📂 Repository Structure

```bash
/src             → ROS 2 implementation  
/data            → Experimental logs  
/plots           → Performance graphs  
/images          → System & print results  
thesis.pdf       → Full thesis  
presentation.pdf → Defense slides  
README.md        → Documentation  
```

---

## 🎥 Demonstration

[![Watch the video](https://img.youtube.com/vi/ORa4JMzHrFo/0.jpg)](https://www.youtube.com/shorts/ORa4JMzHrFo)


---

## 📬 Contact

```bash
Author: Raj
Email: rajbag4321@gmail.com
Location: Berlin, Germany
```

---

## 🏁 Final Statement

This work demonstrates that reliable robotic additive manufacturing can be achieved using low-cost, open-source systems when proper synchronization strategies are applied.

---
