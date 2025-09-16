# ROS2 ReBeL + Marlin Coordinator 
This repository contains the ROS 2 package rmc (ReBeL-Marlin Coordinator) used in the master thesis: 
**“Bridging ROS2 and Additive Manufacturing: A Real-Time Control Approach for Continuous Deposition Tasks”** 

## Features - 
- ROS 2 node for coordinating robot (CRI/TCP) and SKR/Marlin extruder 
- Ack-aware windowing scheme (N = [8,12]) at 50 Hz
- Preamble handling for PETG deposition
- In-flight monitoring via ROS 2 topics (/coordinator/heartbeat, /coordinator/inflight) #

# Run
```
bash
colcon build --symlink-install
source install/setup.bash
ros2 run rmc coordinator --ros-args --params-file src/rmc/params.yaml
```
License

MIT


