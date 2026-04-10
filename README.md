# Autonomous Warehouse Navigation Robot

![ROS 2](https://img.shields.io/badge/ROS_2-Python-blue.svg)
![Build](https://img.shields.io/badge/build-ament__python-brightgreen.svg)
![Status](https://img.shields.io/badge/Status-In_Development-orange.svg)

An open-source Autonomous Mobile Robot (AMR) project designed for warehouse logistics. Built entirely on **ROS 2 (Python)**, this project integrates 2D SLAM for environment mapping, Navigation 2 (Nav2) for dynamic obstacle avoidance, and custom task execution logic to simulate industry-standard warehouse operations.

---

## System Architecture

This workspace is modularized into specific Python packages to separate physical descriptions, simulation, navigation, hardware interfacing, and high-level behavioral logic.

```text
warehouse_robot/
├── warehouse_robot_description/  # Visual (URDF/Xacro) and physical models
├── warehouse_robot_gazebo/       # 3D simulation worlds and launch files
├── warehouse_robot_navigation/   # slam_toolbox and Nav2 configurations
├── warehouse_robot_hardware/     # ESP32 serial bridge and firmware
└── warehouse_robot_core/         # Behavior trees and waypoint management