## 🚀 Introduction

Autonomous mobile robots are a central topic in modern robotics, with applications ranging from service robotics to industrial automation and intelligent transportation systems. A key challenge in this field is enabling robots to operate reliably in real-world environments by combining perception, localization, mapping, and motion planning into a unified system.

This project presents the design and implementation of a complete ROS-based autonomous mobile robot, developed and validated on real hardware. The system integrates Simultaneous Localization and Mapping (SLAM), probabilistic localization, and autonomous navigation using the ROS navigation stack. It leverages data from multiple sensors—including LiDAR, ultrasonic, and Time-of-Flight (ToF) sensors—to improve environmental awareness and robustness.

Unlike purely simulation-based projects, this work focuses on end-to-end deployment, addressing practical challenges such as sensor noise, power management, communication reliability (I2C/serial), and real-time performance on embedded hardware (Jetson Nano + Arduino). The robot is capable of generating maps of unknown environments, localizing itself within those maps, and navigating autonomously to user-defined goals while avoiding obstacles.

In addition, the project explores multi-sensor integration strategies, costmap tuning, and navigation parameter optimization, providing insights into the trade-offs between accuracy, computational efficiency, and system stability.

Overall, this repository demonstrates a holistic approach to autonomous robotics, bridging the gap between theoretical concepts and real-world implementation—an essential step toward deploying intelligent robotic systems in practical applications.
