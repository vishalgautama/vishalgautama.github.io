---
layout: page
title: 500 lb Heavy-lift Multirotor UAV
description: End-to-end technical ownership from motor and frame sizing through avionics integration, GNC commissioning, and first autonomous flight
img: assets/img/personal/heavy_lift_multirotor.jpeg
importance: 1
category: industry
---

Led the complete development of a 500 lb payload-class heavy-lift multirotor UAV at VTOL Aviation India, from initial sizing through autonomous flight operations.

{% include figure.liquid path="assets/img/personal/heavy_lift_multirotor.jpeg" class="img-fluid rounded z-depth-1" alt="500 lb heavy-lift multirotor UAV" zoomable=true %}

---

## Sizing & Design

Motor and propeller selection was driven by a hover thrust-to-weight margin requirement. For an $$N$$-rotor system:

$$
T_{\text{total}} = N \cdot T_{\text{motor}} \geq \text{TWR} \cdot mg
$$

Each motor operating point was placed at ~60% throttle in hover, leaving sufficient headroom for control authority. Battery capacity was sized against the required endurance:

$$
E = P_{\text{hover}} \cdot t_{\text{flight}} \cdot \eta^{-1}, \quad C_{\text{battery}} \geq \frac{E}{V_{\text{nominal}}}
$$

Drivetrain and ESC selections were validated against motor torque-speed curves from the propulsion test stand before procurement.

---

## Control Law Design

The inner-loop attitude controller uses a cascaded PID structure. The outer position loop generates attitude commands fed into the inner loop:

$$
\ddot{p}_{\text{des}} = K_p e_p + K_d \dot{e}_p + K_i \int e_p \, dt
$$

$$
\phi_{\text{des}},\ \theta_{\text{des}} = f(\ddot{p}_{\text{des}},\ \psi_{\text{des}})
$$

Gain scheduling was applied across the flight envelope (payload variation, battery state) with operating points established via trim analysis at each configuration.

---

## Verification & Test

All control laws were verified through HITL testing before first flight. Stability margins were evaluated via Bode analysis on the linearized closed-loop:

$$
\text{GM} = -20 \log_{10} |L(j\omega_{pc})| \geq 6 \text{ dB}, \quad \text{PM} = 180° + \angle L(j\omega_{gc}) \geq 45°
$$

where $$L(s) = C(s)G(s)$$ is the open-loop transfer function.

<!-- GIF placeholder: replace path when flight animation is ready -->
<!-- {% include figure.liquid path="assets/img/personal/heavy_lift_flight.gif" class="img-fluid rounded z-depth-1" alt="Heavy-lift UAV autonomous flight" zoomable=true %} -->

*Autonomous flight footage — coming soon*

---

## Control Allocation

For an $$N$$-rotor system, the control allocation maps desired wrench $$[T,\ \tau_x,\ \tau_y,\ \tau_z]^\top$$ to individual rotor thrusts:

$$
\begin{bmatrix} T \\ \tau_x \\ \tau_y \\ \tau_z \end{bmatrix} = B \begin{bmatrix} T_1 \\ \vdots \\ T_N \end{bmatrix}, \quad \mathbf{T} = B^+ \begin{bmatrix} T \\ \tau_x \\ \tau_y \\ \tau_z \end{bmatrix}
$$

where $$B^+$$ is the pseudoinverse of the allocation matrix. Rotor commands are then saturated and converted to PWM.

---

## Results

| Metric | Value |
|---|---|
| Gross takeoff weight | ~500 lb |
| Hover endurance | Validated per design spec |
| V&V test cases authored | 100+ |
| Stability margins (min) | GM > 6 dB, PM > 45° |
| Autonomous waypoint accuracy | < 1 m |
