# Autonomous Warehouse Navigation Robot

![ROS 2](https://img.shields.io/badge/ROS_2-Python-blue.svg)
![Build](https://img.shields.io/badge/build-ament__python-brightgreen.svg)
![Status](https://img.shields.io/badge/Status-In_Development-orange.svg)
![Language](https://img.shields.io/badge/Language-Python-green.svg)
![License](https://img.shields.io/badge/License-Open_Source-blue.svg)

An open-source **Autonomous Mobile Robot (AMR)** project designed for warehouse logistics and material handling operations. Built entirely on **ROS 2 (Python)**, this project integrates 2D SLAM for environment mapping, Navigation 2 (Nav2) for autonomous path planning, and behavior trees for intelligent task execution.

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [Project Structure](#project-structure)
- [Technology Stack](#technology-stack)
- [Installation & Setup](#installation--setup)
- [Usage Guide](#usage-guide)
- [Module Documentation](#module-documentation)
- [Hardware Specifications](#hardware-specifications)
- [Software Components](#software-components)
- [Configuration Guide](#configuration-guide)
- [Testing & Validation](#testing--validation)
- [Contributing](#contributing)
- [License](#license)
- [References](#references)

---

## Overview

This project implements a complete autonomous navigation solution for warehouse robots. The system enables robots to:

- **Autonomously map environments** using SLAM (Simultaneous Localization and Mapping)
- **Navigate safely** through warehouses avoiding obstacles
- **Execute complex tasks** using behavior trees
- **Interface with hardware** through ESP32-based controllers
- **Simulate operations** in Gazebo before physical deployment

### Project Goals

1. Develop a modular, scalable ROS 2 framework for warehouse automation
2. Implement robust SLAM and navigation algorithms
3. Create a realistic simulation environment in Gazebo
4. Provide hardware abstraction for easy integration with different robot platforms
5. Enable multi-robot coordination through waypoint management

---

## Key Features

### 🤖 Autonomous Navigation
- **SLAM Integration**: Real-time environment mapping using slam_toolbox
- **Path Planning**: Dynamic obstacle avoidance with Nav2 stack
- **Localization**: Precise robot pose estimation in mapped environments
- **Waypoint Management**: Pre-programmed delivery routes and task scheduling

### 🎮 Simulation Environment
- **Gazebo 3D Simulation**: Realistic physics-based warehouse scenarios
- **Sensor Simulation**: Accurate modeling of LiDAR, odometry, and IMU sensors
- **Multiple Warehouse Layouts**: Configurable environments for different scenarios

### 🧠 Intelligent Behavior
- **Behavior Trees**: Hierarchical task planning and execution
- **Task Scheduling**: Efficient route optimization and delivery sequencing
- **Error Handling**: Graceful degradation and recovery mechanisms

### 🔧 Hardware Integration
- **ESP32 Serial Bridge**: Direct hardware communication
- **Motor Control**: PWM-based motor commands and feedback
- **Sensor Integration**: Support for LiDAR, encoders, and IMU sensors

### 📦 Modular Architecture
- **Separation of Concerns**: Distinct packages for description, simulation, navigation, hardware, and core logic
- **Reusability**: Components designed for easy extension and customization
- **Scalability**: Framework supports multiple robot instances

---

## System Architecture

```
warehouse_robot/
├── warehouse_robot_description/   # Robot model definition (URDF/Xacro)
├── warehouse_robot_gazebo/        # Simulation environment and launch files
├── warehouse_robot_navigation/    # SLAM and Nav2 configuration
├── warehouse_robot_hardware/      # ESP32 interface and firmware
└── warehouse_robot_core/          # Behavior trees and task management
```

### Architecture Diagram

```
┌─────────────────────────────────────────────────────┐
│         Behavior Tree & Task Manager (Core)         │
├─────────────────────────────────────────────────────┤
│              Navigation Stack (Nav2)                │
│  ┌────────────────────────────────────────────────┐ │
│  │ Path Planning │ Collision Avoidance │ Costmap │ │
│  └────────────────────────────────────────────────┘ │
├─────────────────────────────────────────────────────┤
│              SLAM & Localization                    │
│  ┌────────────────────────────────────────────────┐ │
│  │  slam_toolbox │ Odometry │ TF Transformation  │ │
│  └────────────────────────────────────────────────┘ │
├─────────────────────────────────────────────────────┤
│         Hardware Interface & Sensor Processing      │
│  ┌──────────────────┬──────────────────────────┐   │
│  │  Motor Control   │  LiDAR Driver            │   │
│  │  (ESP32 Serial)  │  IMU/Encoder Processing  │   │
│  └──────────────────┴──────────────────────────┘   │
└─────────────────────────────────────────────────────┘
           │
           ▼
    Physical Robot Hardware
```

---

## Project Structure

### Detailed Directory Layout

```
warehouse_robot/
│
├── warehouse_robot_description/
│   ├── urdf/
│   │   ├── robot.urdf
│   │   ├── robot.xacro
│   │   └── sensors.xacro
│   ├── meshes/
│   │   ├── base.stl
│   │   ├── wheel.stl
│   │   └── lidar_mount.stl
│   └── package.xml
│
├── warehouse_robot_gazebo/
│   ├── worlds/
│   │   ├── warehouse_small.world
│   │   ├── warehouse_large.world
│   │   └── obstacles.world
│   ├── launch/
│   │   ├── gazebo.launch.py
│   │   ├── spawner.launch.py
│   │   └── sim.launch.py
│   └── package.xml
│
├── warehouse_robot_navigation/
│   ├── config/
│   │   ├── nav2_params.yaml
│   │   ├── slam_toolbox_params.yaml
│   │   └── costmap_plugins.yaml
│   ├── maps/
│   │   ├── warehouse_map.yaml
│   │   └── warehouse_map.pgm
│   ├── launch/
│   │   ├── navigation.launch.py
│   │   ├── slam.launch.py
│   │   └── localization.launch.py
│   └── package.xml
│
├── warehouse_robot_hardware/
│   ├── src/
│   │   ├── motor_controller.py
│   │   ├── sensor_reader.py
│   │   └── esp32_bridge.py
│   ├── firmware/
│   │   └── esp32_firmware.ino
│   ├── launch/
│   │   └── hardware.launch.py
│   └── package.xml
│
├── warehouse_robot_core/
│   ├── src/
│   │   ├── behavior_tree.py
│   │   ├── task_manager.py
│   │   ├── waypoint_handler.py
│   │   └── mission_planner.py
│   ├── trees/
│   │   ├── delivery_task.xml
│   │   ├── navigation_task.xml
│   │   └── error_recovery.xml
│   ├── launch/
│   │   └── core.launch.py
│   └── package.xml
│
├── README.md
├── setup.py
└── .gitignore
```

---

## Technology Stack

### Core Framework
- **ROS 2** (Humble/Iron): Middleware for robot communication
- **Python 3.9+**: Primary programming language
- **ament_cmake**: Build system

### Navigation & SLAM
- **Navigation 2 (Nav2)**: Autonomous navigation framework
- **slam_toolbox**: SLAM implementation for 2D mapping
- **tf2**: Coordinate frame transformation library
- **geometry_msgs**: Standard geometric message types

### Simulation
- **Gazebo**: 3D physics-based simulator
- **gazebo_ros**: ROS 2 plugin for Gazebo integration
- **gazebo_plugins**: Sensor and actuator plugins

### Robotics Libraries
- **TensorFlow/PyTorch** (optional): For ML-based planning
- **OpenCV**: Image processing for sensor data
- **NumPy/SciPy**: Numerical computations

### Development Tools
- **colcon**: Build tool for ROS 2 workspaces
- **pytest**: Unit testing framework
- **pytest-cov**: Code coverage analysis

---

## Installation & Setup

### Prerequisites

- Ubuntu 20.04 LTS or higher
- ROS 2 (Humble or Iron distribution)
- Python 3.9+
- Git
- 4GB minimum RAM recommended

### Step 1: Install ROS 2

```bash
# Follow official ROS 2 installation guide
# https://docs.ros.org/en/humble/Installation.html
curl -sSL https://raw.githubusercontent.com/ros/rosdep/master/rosdep.sh | sudo bash /dev/stdin
```

### Step 2: Create Workspace

```bash
mkdir -p ~/warehouse_ws/src
cd ~/warehouse_ws/src
git clone https://github.com/PreetinderSinghBadesha/autonomous-warehouse-navigation-robot.git
cd ~/warehouse_ws
```

### Step 3: Install Dependencies

```bash
rosdep install --from-paths src --ignore-src -r -y
pip install -r requirements.txt
```

### Step 4: Build Workspace

```bash
cd ~/warehouse_ws
colcon build --symlink-install
source install/setup.bash
```

### Step 5: Verify Installation

```bash
ros2 pkg list | grep warehouse_robot
```

Expected output:
```
warehouse_robot_core
warehouse_robot_description
warehouse_robot_gazebo
warehouse_robot_hardware
warehouse_robot_navigation
```

---

## Usage Guide

### Simulation Mode

#### Launch Gazebo with Robot

```bash
cd ~/warehouse_ws
source install/setup.bash

# Start simulation
ros2 launch warehouse_robot_gazebo sim.launch.py

# In another terminal, start SLAM
ros2 launch warehouse_robot_navigation slam.launch.py

# In another terminal, start navigation
ros2 launch warehouse_robot_navigation navigation.launch.py
```

#### Teleop Control

```bash
# Manual control (for testing)
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

### Hardware Mode

#### Connect Hardware

```bash
# Identify ESP32 port
ls /dev/tty*

# Launch hardware interface
ros2 launch warehouse_robot_hardware hardware.launch.py port:=/dev/ttyUSB0
```

#### SLAM Mapping

```bash
# Create new map
ros2 launch warehouse_robot_navigation slam.launch.py map_name:=warehouse_map

# When mapping complete, save map
ros2 run nav2_map_server map_saver_cli -f ~/warehouse_ws/src/warehouse_robot_navigation/maps/warehouse_map
```

#### Autonomous Navigation

```bash
# Load pre-existing map
ros2 launch warehouse_robot_navigation localization.launch.py map:=warehouse_map.yaml

# Start navigation
ros2 launch warehouse_robot_navigation navigation.launch.py
```

#### Waypoint Mission Execution

```bash
# Send waypoint sequence
ros2 run warehouse_robot_core waypoint_handler --waypoints /path/to/waypoints.yaml

# Monitor mission status
ros2 topic echo /warehouse_robot_core/mission_status
```

---

## Module Documentation

### warehouse_robot_description

**Purpose**: Defines robot geometry, sensors, and physical properties

**Key Files**:
- `robot.urdf.xacro`: Main robot model with joints and links
- `sensors.xacro`: LiDAR, IMU, and encoder specifications
- `materials.xacro`: Visual properties and textures

**Topics Published**:
- `/tf`: Robot frame transforms

**Parameters**:
- `base_width`: Robot chassis width (m)
- `wheel_radius`: Wheel radius (m)
- `sensor_height`: Sensor mounting height (m)

---

### warehouse_robot_gazebo

**Purpose**: Provides simulation environment and physics

**Key Features**:
- Multiple warehouse world files
- Realistic sensor simulation
- Dynamic obstacle spawning

**Launch Files**:
- `sim.launch.py`: Complete simulation stack
- `spawner.launch.py`: Robot spawning in Gazebo

**World Files**:
- `warehouse_small.world`: Small warehouse (20m × 20m)
- `warehouse_large.world`: Large warehouse (50m × 50m)

---

### warehouse_robot_navigation

**Purpose**: SLAM and autonomous navigation

**Configuration Parameters** (nav2_params.yaml):
- **Planner**: DWB (Dynamic Window Approach)
- **Controller**: RPP (Regulated Pure Pursuit)
- **Local Costmap**: 2.5m × 2.5m rolling window
- **Global Costmap**: Full map representation

**Topics Subscribed**:
- `/cmd_vel`: Velocity commands
- `/scan`: LiDAR scan data

**Topics Published**:
- `/map`: Occupancy grid map
- `/amcl_pose`: Robot estimated pose
- `/plan`: Planned path

---

### warehouse_robot_hardware

**Purpose**: Hardware abstraction and control

**Supported Hardware**:
- **Controller**: ESP32 microcontroller
- **Motors**: 2× DC motors with encoders
- **Sensors**: 2D LiDAR, 9-DOF IMU
- **Communication**: Serial UART (115200 baud)

**Python Interface**:

```python
from warehouse_robot_hardware.esp32_bridge import ESP32Bridge

bridge = ESP32Bridge(port='/dev/ttyUSB0')
bridge.set_motor_speed(left_speed=0.5, right_speed=0.5)
encoder_data = bridge.read_encoders()
```

---

### warehouse_robot_core

**Purpose**: High-level task planning and execution

**Behavior Tree Structure**:
- Delivery Task: Navigate to location → Pick item → Return
- Error Recovery: Handle collisions and timeouts
- Idle Task: Wait for instructions

**Key Classes**:
- `BehaviorTreeExecutor`: Manages tree execution
- `TaskManager`: Schedules and prioritizes tasks
- `WaypointHandler`: Manages delivery route
- `MissionPlanner`: Plans overall mission

---

## Hardware Specifications

### Robot Platform

| Component | Specification |
|-----------|---------------|
| **Base Type** | Differential drive (2-wheel) |
| **Dimensions** | 300mm × 250mm × 150mm |
| **Weight** | 3.5 kg |
| **Max Speed** | 0.5 m/s |
| **Max Acceleration** | 0.2 m/s² |
| **Payload** | 2 kg |
| **Battery** | 12V LiPo (5Ah) |
| **Runtime** | ~2 hours continuous operation |

### Sensors

| Sensor | Model/Spec | Purpose |
|--------|-----------|---------|
| **LiDAR** | RPLiDAR A1 (360°, 25m range) | Obstacle detection, SLAM |
| **IMU** | MPU-9250 (9-DOF) | Heading estimation, stability |
| **Encoders** | Incremental (1024 PPR) | Odometry, wheel speed control |

### Actuators

| Actuator | Specification |
|----------|---------------|
| **Motors** | 2× DC motors (12V, 300 RPM, 1:20 gearbox) |
| **Motor Driver** | DRV8835 dual H-bridge |
| **PWM Frequency** | 20 kHz |

### Controller

| Component | Specification |
|-----------|---------------|
| **Microcontroller** | ESP32 (dual-core, 240MHz) |
| **Communication** | Serial UART (115200 baud) |
| **Firmware** | Arduino-compatible C++ |

---

## Software Components

### Node Architecture

```
┌──────────────────────────────────────────────────────────┐
│                    ROS 2 Humble/Iron                     │
├──────────────────────────────────────────────────────────┤
│
│  ┌─────────────────────┐   ┌─────────────────────┐
│  │  behavior_tree_     │   │   mission_          │
│  │  executor_node      │   │   planner_node      │
│  └─────────────────────┘   └─────────────────────┘
│         ▲                            │
│         │                            ▼
│  ┌─────────────────────────────────────────────┐
│  │    warehouse_robot_core package nodes      │
│  │  (waypoint_handler, task_manager)          │
│  └─────────────────────────────────────────────┘
│         ▲
│         │
│  ┌──────────────────────────────────────────────┐
│  │     warehouse_robot_navigation package      │
│  │  (nav2_bringup, slam_toolbox, amcl_node)   │
│  └──────────────────────────────────────────────┘
│         ▲
│         │
│  ┌──────────────────────────────────────────────┐
│  │    warehouse_robot_hardware package         │
│  │  (esp32_bridge_node, sensor_reader_node)   │
│  └──────────────────────────────────────────────┘
│
└──────────────────────────────────────────────────────────┘
```

### ROS 2 Topics

**Standard Topics**:

| Topic | Type | Direction | Frequency |
|-------|------|-----------|-----------|
| `/cmd_vel` | Twist | pub by Nav2 | 10 Hz |
| `/odom` | Odometry | pub by hardware | 50 Hz |
| `/scan` | LaserScan | pub by hardware | 10 Hz |
| `/imu/data` | Imu | pub by hardware | 30 Hz |
| `/map` | OccupancyGrid | pub by slam_toolbox | 1 Hz |
| `/amcl_pose` | PoseWithCovarianceStamped | pub by AMCL | 5 Hz |

**Custom Topics**:

| Topic | Type | Direction | Purpose |
|-------|------|-----------|---------|
| `/warehouse_robot_core/mission_status` | String | pub | Mission execution status |
| `/warehouse_robot_core/waypoints` | PoseArray | sub | Waypoint sequence |
| `/warehouse_robot_hardware/motor_cmd` | Float32MultiArray | sub | Motor PWM commands |

---

## Configuration Guide

### Navigation Configuration (nav2_params.yaml)

```yaml
planner_server:
  ros__parameters:
    use_sim_time: true
    expected_planner_frequency: 20.0
    planner_plugins: ["GridBased"]
    GridBased:
      plugin: nav2_navfn_planner/NavfnPlanner
      tolerance: 0.5
      use_astar: false
      allow_unknown: true

controller_server:
  ros__parameters:
    use_sim_time: true
    controller_frequency: 20.0
    min_x_velocity_threshold: 0.001
    min_y_velocity_threshold: 0.5
    min_theta_velocity_threshold: 0.001
    progress_checker_plugin: "progress_checker"
    default_tracker: "FollowPath"
    smoothing_frequency: 20.0
```

### SLAM Configuration (slam_toolbox_params.yaml)

```yaml
slam_toolbox:
  ros__parameters:
    use_sim_time: true
    solver_plugin: solver_plugins::CeresSolver
    ceres_linear_solver: SPARSE_NORMAL_CHOLESKY
    ceres_linear_solver_threads: 1
    ceres_preconditioner: SCHUR_JACOBI
    map_update_interval: 5.0
    scan_buffer_size: 10
```

### Gazebo Simulation Parameters

```yaml
gazebo:
  physics: ode
  real_time_update_rate: 1000.0
  max_step_size: 0.001
  gravity: [0, 0, -9.81]
```

---

## Testing & Validation

### Unit Tests

```bash
# Run all tests
colcon test --packages-select warehouse_robot_core

# Run specific test
colcon test --packages-select warehouse_robot_navigation
```

### Integration Tests

```bash
# Test SLAM in simulation
ros2 launch warehouse_robot_gazebo sim.launch.py
ros2 launch warehouse_robot_navigation slam.launch.py
# Teleop robot and verify map creation

# Test navigation stack
ros2 launch warehouse_robot_navigation navigation.launch.py
# Send 2D Nav Goal through RViz
```

### Performance Benchmarks

**Expected Performance Metrics**:
- SLAM mapping: 5-10 cm accuracy
- Navigation speed: 0.3-0.5 m/s
- Path planning latency: <100 ms
- CPU usage: ~40-60% on dual-core ESP32

### Validation Checklist

- [ ] Robot spawns correctly in Gazebo
- [ ] SLAM creates valid map
- [ ] Navigation stack plans paths
- [ ] Behavior tree executes tasks
- [ ] Hardware serial communication works
- [ ] Encoders provide accurate odometry
- [ ] Mission waypoints executed in sequence
- [ ] Error recovery handles obstacles

---

## Contributing

We welcome contributions! Please follow these guidelines:

### Development Workflow

1. Fork the repository
2. Create feature branch: `git checkout -b feature/your-feature`
3. Commit changes: `git commit -m "Add feature description"`
4. Push to branch: `git push origin feature/your-feature`
5. Submit Pull Request

### Code Standards

- Follow PEP 8 style guide
- Add docstrings to all functions/classes
- Include unit tests for new features
- Update README for significant changes

### Testing Before Submission

```bash
# Run linting
flake8 warehouse_robot_*

# Run type checking
mypy warehouse_robot_*

# Run tests
colcon test

# Check coverage
pytest --cov=warehouse_robot_*
```

---

## License

This project is licensed under the **MIT License** - see LICENSE file for details.

---

## References

### Official Documentation
- [ROS 2 Documentation](https://docs.ros.org/)
- [Navigation 2 Documentation](https://navigation.ros.org/)
- [slam_toolbox](https://github.com/StanleyLab/slam_toolbox)
- [Gazebo Simulation](http://gazebosim.org/)

### Related Papers & Resources
- SLAM Algorithms: [Probabilistic Robotics](http://www.probabilistic-robotics.org/)
- Path Planning: [Motion Planning Algorithms](https://cs.lth.se/~christoffer/papers/)
- Behavior Trees: [Behavior Trees in Robotics](https://arxiv.org/abs/1605.01661)

### Community Resources
- [ROS 2 Discourse](https://discourse.ros.org/)
- [ROS Answers](https://answers.ros.org/)
- [Stack Overflow - ros tag](https://stackoverflow.com/questions/tagged/ros)

---

## Contact & Support

**Project Maintainer**: Preetinder Singh Badesha  
**GitHub**: [@PreetinderSinghBadesha](https://github.com/PreetinderSinghBadesha)  
**Email**: [your-email@example.com]

---

**Last Updated**: 2026-05-04 18:26:20  
**Status**: In Development  
**Version**: 0.1.0