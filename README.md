## 🚀 Introduction

Autonomous mobile robots are a central topic in modern robotics, with applications ranging from service robotics to industrial automation and intelligent transportation systems. A key challenge in this field is enabling robots to operate reliably in real-world environments by combining perception, localization, mapping, and motion planning into a unified system.

This project presents the design and implementation of a complete ROS-based autonomous mobile robot, developed and validated on real hardware. The system integrates Simultaneous Localization and Mapping (SLAM), probabilistic localization, and autonomous navigation using the ROS navigation stack. It leverages data from multiple sensors—including LiDAR, ultrasonic, and Time-of-Flight (ToF) sensors—to improve environmental awareness and robustness.

Unlike purely simulation-based projects, this work focuses on end-to-end deployment, addressing practical challenges such as sensor noise, power management, communication reliability (I2C/serial), and real-time performance on embedded hardware (Jetson Nano + Arduino). The robot is capable of generating maps of unknown environments, localizing itself within those maps, and navigating autonomously to user-defined goals while avoiding obstacles.

In addition, the project explores multi-sensor integration strategies, costmap tuning, and navigation parameter optimization, providing insights into the trade-offs between accuracy, computational efficiency, and system stability.

Overall, this repository demonstrates a holistic approach to autonomous robotics, bridging the gap between theoretical concepts and real-world implementation—an essential step toward deploying intelligent robotic systems in practical applications.

## 🧠 System Overview

This project implements a complete autonomous mobile robot system based on the Robot Operating System (ROS), integrating perception, localization, mapping, and navigation into a unified framework. The system is designed to operate both in simulation and real-world environments, with a strong emphasis on practical deployment and hardware-software integration.

At a high level, the robot performs three main tasks:

Environment Mapping (SLAM): Generating a map of an unknown environment using LiDAR data.
Localization: Estimating the robot’s position within the generated map using probabilistic methods.
Autonomous Navigation: Planning and executing collision-free paths toward user-defined goals.

The system architecture consists of two main layers:

🔹 **Hardware Layer**

The physical platform is built around an embedded computing unit and multiple sensors:

Jetson Nano as the main processing unit running ROS
Arduino for low-level motor control and sensor interfacing
LiDAR sensor (RPLidar A1) for environment perception
Ultrasonic and ToF sensors for short-range obstacle detection and redundancy
Wheel encoders + IMU for improved odometry estimation
Custom power management system for stable operation
🔹 **Software Layer**

The software stack is implemented using ROS (Noetic) and is composed of several interconnected modules:

SLAM packages (e.g., GMapping, Hector SLAM) for map generation
AMCL for probabilistic localization
move_base for autonomous navigation
Costmaps for obstacle representation and path safety
Custom ROS nodes for sensor integration and communication between Jetson and Arduino

To improve localization accuracy, the system combines wheel encoder data with IMU measurements, reducing drift and enhancing motion estimation, especially during rotations and uneven movements.

The system relies on standard ROS communication mechanisms such as topics, services, and transform (tf) trees to ensure synchronization between different modules.

Overall, this architecture enables the robot to perceive its environment, estimate its position, and navigate autonomously while handling real-world uncertainties such as sensor noise and hardware limitations.
