---
layout: page
title: ROS 2 GNC Middleware with EKF Sensor Fusion
description: Low-latency C++/Python ROS 2 middleware for time-synchronized multi-sensor acquisition with Extended Kalman Filter state estimation
importance: 3
category: research
---

Designed and implemented a low-latency ROS 2 middleware stack for onboard GNC on a UAV platform, delivering real-time state estimates to the control loop via Extended Kalman Filter sensor fusion.

---

## System Architecture

```
┌─────────────┐    ┌─────────────┐
│   GPS Node  │    │   IMU Node  │
│  (10–20 Hz) │    │  (200 Hz)   │
└──────┬──────┘    └──────┬──────┘
       │                  │
       └──────┬───────────┘
              │  time-synchronized
              ▼
       ┌─────────────┐
       │  EKF Node   │  ← fused state estimate
       │  (200 Hz)   │     [p, v, q, ω, b_a, b_g]
       └──────┬──────┘
              │
              ▼
       ┌─────────────┐
       │ Control Node│
       └─────────────┘
```

Time synchronization uses ROS 2 message filters with an approximate time policy, aligning sensor stamps before they enter the filter.

---

## Extended Kalman Filter

The state vector is:

$$
x = \begin{bmatrix} p^\top & v^\top & q^\top & b_a^\top & b_g^\top \end{bmatrix}^\top \in \mathbb{R}^{16}
$$

where $$p$$ is position, $$v$$ velocity, $$q$$ unit quaternion attitude, $$b_a$$ accelerometer bias, and $$b_g$$ gyroscope bias.

**Prediction step** (IMU propagation at 200 Hz):

$$
\hat{x}_{k|k-1} = f(\hat{x}_{k-1}, u_k), \quad P_{k|k-1} = F_k P_{k-1} F_k^\top + Q
$$

where $$F_k = \partial f / \partial x \big|_{\hat{x}_{k-1}}$$ is the Jacobian of the process model and $$Q$$ is the process noise covariance.

**Update step** (GPS correction at 10–20 Hz):

$$
K_k = P_{k|k-1} H_k^\top \left( H_k P_{k|k-1} H_k^\top + R \right)^{-1}
$$

$$
\hat{x}_k = \hat{x}_{k|k-1} + K_k \left( z_k - h(\hat{x}_{k|k-1}) \right)
$$

$$
P_k = (I - K_k H_k) P_{k|k-1}
$$

Attitude updates are applied as a quaternion correction on the manifold rather than an additive correction in $$\mathbb{R}^4$$, preserving the unit-norm constraint.

---

## Implementation

**EKF prediction node (C++):**
```cpp
void ImuCallback(const sensor_msgs::msg::Imu::SharedPtr msg) {
  // Extract IMU measurements
  Eigen::Vector3d accel(msg->linear_acceleration.x,
                        msg->linear_acceleration.y,
                        msg->linear_acceleration.z);
  Eigen::Vector3d gyro(msg->angular_velocity.x,
                       msg->angular_velocity.y,
                       msg->angular_velocity.z);

  // Remove estimated bias
  accel -= state_.b_a;
  gyro  -= state_.b_g;

  // Propagate state
  state_.p += dt_ * state_.v;
  state_.v += dt_ * (state_.R * accel + gravity_);
  state_.q  = quat_integrate(state_.q, gyro, dt_);

  // Propagate covariance
  P_ = F_ * P_ * F_.transpose() + Q_;
}
```

**GPS update (C++):**
```cpp
void GpsCallback(const sensor_msgs::msg::NavSatFix::SharedPtr msg) {
  Eigen::Vector3d z = lla_to_ned(msg->latitude,
                                  msg->longitude,
                                  msg->altitude);
  Eigen::Vector3d innov = z - state_.p;

  Eigen::Matrix3d S = H_ * P_ * H_.transpose() + R_gps_;
  Eigen::MatrixXd K = P_ * H_.transpose() * S.inverse();

  state_.p += K.topRows(3) * innov;
  state_.v += K.middleRows(3, 3) * innov;
  P_ = (I_ - K * H_) * P_;
}
```

---

## Performance

| Metric | Value |
|---|---|
| End-to-end latency (IMU → control) | < 5 ms |
| Position RMSE (hover, GPS denied) | < 0.4 m over 60 s |
| Attitude RMSE | < 1.2° |
| Filter update rate | 200 Hz |
