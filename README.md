# 🚀 Autonomous Mobile Robot with ROS (SLAM & Navigation)

## 🧠 Overview
<p align="justify">
This project presents the design and implementation of an autonomous mobile robot using ROS (Robot Operating System). The system integrates SLAM, localization, navigation, and multi-sensor fusion to enable autonomous movement in real-world environments.

The robot is built using a combination of embedded systems (Jetson Nano + Arduino), LIDAR-based perception, and additional sensors such as IMU and ultrasonic modules.
</p>

The project includes:
- SLAM-based mapping (GMapping, Hector SLAM)
- Localization using AMCL
- Path planning using ROS Navigation Stack
- Sensor fusion (Encoder + IMU + LIDAR)
- Real-world deployment and testing
- Simulation in Gazebo and visualization in RViz

![Gazebo](Images/Gazebo.png)

![Real Robot](Images/Real_robot.png)

## 🔧 System Architecture

The system consists of multiple hardware and software components working together:

### 🔹 Hardware
- Jetson Nano (main processing unit)
- Arduino (low-level control & sensor interface)
- RPLidar A1 (environment perception)
- Wheel encoders (odometry)
- IMU sensor (orientation estimation)
- Ultrasonic / ToF sensors (obstacle detection)

### 🔹 Software
- ROS Noetic (Ubuntu 20.04)
- SLAM algorithms (GMapping, Hector SLAM)
- Navigation stack (move_base)
- Sensor drivers and custom ROS nodes

The robot uses a distributed architecture where:
- Arduino handles low-level sensor acquisition
- Jetson Nano processes data and runs ROS
- Communication is done via Serial / I2C

## 🧩 Features

- Autonomous navigation in unknown environments
- Real-time mapping using SLAM
- Accurate localization using AMCL
- Dynamic obstacle avoidance
- Multi-sensor fusion for improved accuracy
- Simulation support (Gazebo)
- Visualization (RViz)

## Results

The robot was tested in both simulation and real-world environments.

Key achievements:
- Successful map generation using LIDAR
- Stable localization using AMCL
- Smooth navigation using DWA local planner
- Integration of additional sensors into costmap

<table align="left">
  <tr>
    <td><img src="Images/Gazebo_Simulation.gif" width="400"/></td>
    <td><img src="Images/Disinfection_Robot.gif" width="135"/></td>
    <td><img src="Images/Avoid_Obstacles.gif" width="120"/></td>
  </tr>
</table>
<br clear="left"/>


# 🗺️ Environment Map
<p align="justify">
The map of the laboratory environment was generated using the implemented SLAM framework. By teleoperating the robot throughout the workspace, a consistent representation of the environment—including walls and obstacles—was constructed.
This map serves as the foundation for localization and autonomous navigation. The integration of sensor data (including odometry and IMU) improves the consistency and reliability of the generated map.
</p>

<p align="center">
  <img src="Images/Map.png" width="500"/>
</p>

# 🧭 Sensor Selection and Evaluation

<p align="justify">
In this section, multiple distance sensors were evaluated to determine the most suitable option for robotic navigation and obstacle detection. The considered sensors include VL53L1X, VL53L0X, Mtof171000C0, and SRF08. A comparative analysis was conducted based on key characteristics such as field of view (FOV), measurement range, communication protocol, cost, and ease of integration. Additionally, a practical evaluation was performed by measuring sensor outputs at known distances ranging from 10 cm to 60 cm. The measured values were recorded and compared against ground-truth distances to assess accuracy and consistency.
</p>

<p align="center">
  <img src="Images/Distance_Sensor_Specifications_and_Comparison.png" width="500"/>
  <img src="Images/Distance_Measurement_Accuracy_Evaluation.png" width="500"/>
</p>

# 📌 Final Sensor Configuration Decision

Based on accuracy, stability, and integration capability, the VL53L1X sensor was selected as the primary ranging sensor due to its superior accuracy and programmable I2C configuration. To enhance robustness, an additional ultrasonic sensor (SRF08) was incorporated for redundancy in cases where laser-based measurements may fail (e.g., transparent surfaces such as glass).

<p align="center">
  <img src="Images/Setup_sensors.png"/>
</p>
