---
layout: page
title: 6-DOF eVTOL Simulation & Adaptive Control
description: Nonlinear simulation with MRAC, L1, LPV, Geometric, LQR, and H-infinity controllers validated through Lyapunov analysis and Monte Carlo studies
importance: 1
category: research
---

Architected a full 6-DOF nonlinear simulation in MATLAB/Simulink for an eVTOL platform, coupling rigid body dynamics, SO(3) kinematics, quaternion representations, blade-element momentum theory, and aerodynamic models.

**Control Architectures Implemented**

- Model Reference Adaptive Control (MRAC)
- L1 Adaptive Control
- Linear Parameter-Varying (LPV) control
- Geometric control on SO(3)
- LQR and H-infinity designs
- Fault-tolerant control allocation under actuator failures, informed by system identification data

**Validation**

- Lyapunov stability analysis across the full operating envelope
- Monte Carlo robustness studies under parametric uncertainty
- Flight testing on the physical platform to close the simulation-to-hardware loop
