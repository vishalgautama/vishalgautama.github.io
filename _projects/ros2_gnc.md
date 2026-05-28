---
layout: page
title: ROS 2 GNC Middleware with EKF Sensor Fusion
description: Low-latency C++/Python ROS 2 middleware for time-synchronized multi-sensor acquisition with Extended Kalman Filter state estimation
importance: 3
category: research
---

Designed and implemented a low-latency ROS 2 middleware stack for onboard GNC on a UAV platform.

**System design**

- Time-synchronized acquisition across GPS and IMU sensors in C++ and Python
- Extended Kalman Filter (EKF) for sensor fusion, delivering accurate pose and velocity estimates for closed-loop guidance, navigation, and control
- Architecture prioritized deterministic latency to meet the real-time requirements of the control loop

**Outcome**

The EKF-fused state estimates are fed directly into the onboard control laws, enabling stable autonomous flight with reliable localization under GPS and IMU noise.
