## 🚀 Introduction

Autonomous mobile robots are a central topic in modern robotics, with applications ranging from service robotics to industrial automation and intelligent transportation systems. A key challenge in this field is enabling robots to operate reliably in real-world environments by combining perception, localization, mapping, and motion planning into a unified system.

This project presents the design and implementation of a complete ROS-based autonomous mobile robot, developed and validated on real hardware. The system integrates Simultaneous Localization and Mapping (SLAM), probabilistic localization, and autonomous navigation using the ROS navigation stack. It leverages data from multiple sensors—including LiDAR, ultrasonic, and Time-of-Flight (ToF) sensors—to improve environmental awareness and robustness.

Unlike purely simulation-based projects, this work focuses on end-to-end deployment, addressing practical challenges such as sensor noise, power management, communication reliability (I2C/serial), and real-time performance on embedded hardware (Jetson Nano + Arduino). The robot is capable of generating maps of unknown environments, localizing itself within those maps, and navigating autonomously to user-defined goals while avoiding obstacles.

In addition, the project explores multi-sensor integration strategies, costmap tuning, and navigation parameter optimization, providing insights into the trade-offs between accuracy, computational efficiency, and system stability.

Overall, this repository demonstrates a holistic approach to autonomous robotics, bridging the gap between theoretical concepts and real-world implementation—an essential step toward deploying intelligent robotic systems in practical applications.

## 🧠 System Overview

This project implements a complete autonomous mobile robot system based on the Robot Operating System (ROS), integrating perception, localization, mapping, and navigation into a unified framework. The system is designed to operate both in simulation and real-world environments, with a strong emphasis on practical deployment and hardware-software integration.

At a high level, the robot performs three main tasks:

**Environment Mapping (SLAM)**: Generating a map of an unknown environment using LiDAR data.
**Localization**: Estimating the robot’s position within the generated map using probabilistic methods.
**Autonomous Navigation**: Planning and executing collision-free paths toward user-defined goals.

The system architecture consists of two main layers:

🔹 **Hardware Layer**

The physical platform is built around an embedded computing unit and multiple sensors:

**Jetson Nano** as the main processing unit running ROS
**Arduino** for low-level motor control and sensor interfacing
**LiDAR sensor** (RPLidar A1) for environment perception
**Ultrasonic** and **ToF sensors** for short-range obstacle detection and redundancy
**Wheel encoders** + **IMU** for improved odometry estimation
Custom power management system for stable operation

🔹 **Software Layer**

The software stack is implemented using ROS (Noetic) and is composed of several interconnected modules:

**SLAM packages** (e.g., GMapping, Hector SLAM) for map generation
**AMCL** for probabilistic localization
**move_base** for autonomous navigation
**Costmaps** for obstacle representation and path safety
Custom ROS nodes for sensor integration and communication between Jetson and Arduino

To improve localization accuracy, the system combines **wheel encoder data with IMU measurements**, reducing drift and enhancing motion estimation, especially during rotations and uneven movements.

The system relies on standard ROS communication mechanisms such as **topics**, **services**, and **transform (tf) trees** to ensure synchronization between different modules.

Overall, this architecture enables the robot to perceive its environment, estimate its position, and navigate autonomously while handling real-world uncertainties such as sensor noise and hardware limitations.

## 🔧 Hardware Setup

This project is implemented on a custom-built autonomous mobile robot platform, combining embedded computing, low-level control, and multiple sensing modalities. The hardware design emphasizes reliability, modularity, and compatibility with real-time robotic applications.

🔹 **Embedded Computing Unit**

The main processing unit of the system is the **NVIDIA Jetson Nano**, which runs Ubuntu and ROS Noetic. It is responsible for executing high-level tasks such as SLAM, localization, navigation, and sensor data processing. The Jetson Nano provides sufficient computational power for real-time operation while maintaining a compact and energy-efficient form factor.

🔹 **Low-Level Control (Arduino)**

**An Arduino-based microcontroller** is used for low-level control tasks, including:

Motor control via PWM signals
Reading wheel encoder data
Interfacing with additional sensors

Communication between the Jetson Nano and Arduino is handled through **serial and I2C protocols**, enabling reliable data exchange between high-level and low-level components.

🔹 **Sensing System**

The robot utilizes multiple sensors to achieve robust perception:

**LiDAR (RPLidar A1)**:
The primary sensor for environment perception and SLAM. It provides 2D laser scan data used for mapping, obstacle detection, and localization.
**Time-of-Flight (ToF) Sensors (VL53 series):**
Used for short-range, high-precision distance measurement. These sensors improve detection accuracy in close-range scenarios where LiDAR performance may degrade.
**Ultrasonic Sensors:**
Added as a complementary sensing modality, particularly useful for detecting transparent or reflective surfaces (e.g., glass), where LiDAR measurements may be unreliable.
Wheel Encoders + IMU:
Wheel encoders provide odometry information, while the IMU enhances motion estimation by compensating for drift and improving orientation tracking. The combination of these sensors results in more accurate and stable localization.

🔹 **Motor Driver and Actuation**

The robot is equipped with DC motors controlled via a motor driver. PWM signals generated by the Arduino regulate motor speed and direction. A feedback control loop (PID controller) is implemented to ensure accurate velocity tracking based on encoder measurements.

🔹 **Power Management**

A dedicated power system is designed to supply stable voltage and current to all components, including the Jetson Nano, sensors, and motors. Special attention is given to power distribution, as insufficient current supply—particularly for sensors such as LiDAR—can lead to unstable behavior or system failures.

# 🧩 Software Architecture

The software system is developed using the Robot Operating System (ROS Noetic), providing a modular and scalable framework for integrating perception, localization, and control components. The architecture is designed around distributed ROS nodes that communicate through standardized interfaces, enabling flexibility and real-time performance.

🔹 **ROS Node Structure**

The system is composed of multiple ROS nodes, each responsible for a specific task within the pipeline. These nodes operate concurrently and exchange data through ROS communication mechanisms. The key components include:

Sensor Nodes:
Responsible for publishing raw data from LiDAR, ToF, ultrasonic sensors, IMU, and wheel encoders.
Odometry Node:
Processes encoder and IMU data to estimate the robot’s motion and publishes odometry information.
SLAM Node:
Generates a map of the environment using LiDAR data.
Localization Node (AMCL):
Estimates the robot’s pose within the generated map using probabilistic methods.
Navigation Node (move_base):
Handles path planning and motion control toward target goals.
Custom Interface Nodes:
Manage communication between the Jetson Nano and Arduino, including command transmission and sensor feedback.

🔹 **Communication Mechanisms**

The system relies on standard ROS communication tools:

Topics:
Used for continuous data streaming between nodes. Key topics include:
/cmd_vel for velocity commands
/odom for odometry data
/scan for LiDAR data
Services:
Used for synchronous operations such as resetting costmaps or updating parameters during runtime.
TF (Transform Tree):
Maintains the spatial relationship between coordinate frames such as map, odom, and base_link. This is essential for consistent localization and navigation.

🔹 **Coordinate Frames**

Accurate transformation between coordinate frames is critical for system performance. The main frames used in this project include:

map: Global reference frame representing the environment
odom: Local reference frame derived from odometry
base_link: Robot’s base frame

The transformation between these frames is continuously updated using odometry and localization outputs, enabling the robot to maintain an accurate estimate of its position.

🔹 **Sensor Fusion for Odometry**

To improve motion estimation, the system combines wheel encoder data with IMU measurements. Encoder data provides linear displacement information, while the IMU contributes orientation and angular velocity data. This fusion reduces accumulated drift and improves robustness, particularly during rotational movements and dynamic conditions.

🔹 **Embedded Communication**

Communication between the Jetson Nano and Arduino is implemented using serial and I2C protocols. The Arduino handles real-time control and sensor acquisition, while the Jetson processes high-level algorithms. Data exchange includes:

Sending velocity commands from ROS to the Arduino
Receiving encoder and sensor data from the Arduino
Converting low-level measurements into ROS-compatible messages
