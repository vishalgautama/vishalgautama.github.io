---
layout: page
title: 6-DOF eVTOL Simulation & Adaptive Control
description: Nonlinear simulation with MRAC, L1, LPV, Geometric, LQR, and H-infinity controllers validated through Lyapunov analysis and Monte Carlo studies
importance: 1
category: research
---

Architected a full 6-DOF nonlinear simulation in MATLAB/Simulink for an eVTOL platform, coupling rigid body dynamics, SO(3) kinematics, quaternion representations, blade-element momentum theory, and aerodynamic models.

---

## Equations of Motion

The translational and rotational dynamics are expressed in the body frame as:

$$
m(\dot{v} + \omega \times v) = F_{\text{aero}} + F_{\text{prop}} + R^\top g
$$

$$
J\dot{\omega} + \omega \times J\omega = \tau_{\text{prop}} + \tau_{\text{aero}}
$$

where $$v \in \mathbb{R}^3$$ is the body-frame velocity, $$\omega \in \mathbb{R}^3$$ the angular rate, $$J$$ the inertia tensor, and $$R \in SO(3)$$ the rotation matrix from body to inertial frame.

Attitude is tracked using unit quaternions $$q = [q_0,\ \mathbf{q}]^\top$$, with kinematics:

$$
\dot{q} = \frac{1}{2} \begin{bmatrix} -\mathbf{q}^\top \\ q_0 I + [\mathbf{q}]_\times \end{bmatrix} \omega
$$

This avoids the gimbal singularities inherent to Euler angle representations and enables smooth large-angle maneuvers.

---

## Control Architectures

### LQR Baseline

The linearized system $$\dot{x} = Ax + Bu$$ is stabilized by minimizing:

$$
J = \int_0^\infty \left( x^\top Q x + u^\top R u \right) dt
$$

The optimal gain $$K = R^{-1}B^\top P$$ is obtained by solving the algebraic Riccati equation $$A^\top P + PA - PBR^{-1}B^\top P + Q = 0$$.

### Model Reference Adaptive Control (MRAC)

The plant is written as $$\dot{x} = A_m x + B(u + \Delta(x))$$, where $$\Delta(x)$$ captures matched uncertainty. The reference model tracks the ideal closed-loop $$\dot{x}_m = A_m x_m + B_m r$$. Adaptive gains are updated by:

$$
\dot{\hat{\theta}} = -\Gamma e^\top P B \phi(x)
$$

where $$e = x - x_m$$, $$P$$ satisfies the Lyapunov equation $$A_m^\top P + PA_m = -Q$$, and $$\Gamma > 0$$ is the adaptation rate.

### L1 Adaptive Control

The L1 architecture separates estimation from control. A state predictor runs in parallel:

$$
\dot{\hat{x}} = A_m \hat{x} + B(\hat{\theta}(t)\phi(x) + u)
$$

with fast adaptation rate $$\Gamma \gg 1$$. A low-pass filter $$C(s)$$ on the adaptive signal ensures robustness by limiting bandwidth of the adaptive loop, decoupling adaptation speed from closed-loop robustness.

### Geometric Control on SO(3)

Attitude errors are computed directly on the manifold to avoid unwinding and singularities:

$$
e_R = \frac{1}{2}(R_d^\top R - R^\top R_d)^\vee, \quad e_\omega = \omega - R^\top R_d \omega_d
$$

The control torque is:

$$
\tau = -k_R e_R - k_\omega e_\omega + \omega \times J\omega - J(\hat{\omega} R^\top R_d \omega_d - R^\top R_d \dot{\omega}_d)
$$

---

## Simulation

<!-- GIF placeholder: replace path when animation is ready -->
<!-- {% include figure.liquid path="assets/img/personal/6dof_sim.gif" class="img-fluid rounded z-depth-1" alt="6-DOF simulation animation" zoomable=true %} -->

*6-DOF simulation animation — coming soon*

---

## Validation

**Lyapunov stability** — candidate function $$V = e_R^\top e_R + e_\omega^\top J e_\omega$$ shown to be negative definite under the geometric controller, proving almost-global asymptotic stability.

**Monte Carlo robustness** — 500-run sweep over $$\pm 20\%$$ inertia uncertainty, $$\pm 15\%$$ thrust coefficient variation, and sensor noise; all runs converged within specified tracking tolerance.

**Flight testing** — simulation predictions validated against logged flight data; attitude RMSE within 2° and position RMSE within 0.3 m under nominal conditions.

---

## MATLAB Implementation Snippet

```matlab
% Quaternion kinematics
q_dot = 0.5 * quat_product(q, [0; omega]);

% Newton-Euler translational dynamics
v_dot = (1/m) * (F_prop + F_aero) - cross(omega, v) + R' * g_vec;

% Rotational dynamics
omega_dot = J \ (tau_prop + tau_aero - cross(omega, J * omega));
```
