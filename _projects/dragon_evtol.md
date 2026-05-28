---
layout: page
title: DRAGON eVTOL — Fixed-Wing to Multirotor Transition
description: PX4 firmware modification, gain-scheduled LQR/PID transition control laws, and first demonstration of stable fixed-wing-to-multirotor flight
importance: 2
category: research
---

Took GNC ownership of the DRAGON eVTOL platform at Virginia Tech's Nonlinear Systems Laboratory, culminating in the platform's **first stable fixed-wing-to-multirotor transition** in flight.

---

## The Transition Problem

During conversion from fixed-wing to multirotor mode, both the aerodynamic regime and the effective control authority change simultaneously. At high forward speed, control surfaces dominate; at low speed, rotors take over. The transition corridor sits between these two extremes, where neither set of actuators is fully effective.

The goal is to maintain stability and trackability across the full corridor without discrete switching transients.

<!-- GIF placeholder: replace path when transition animation is ready -->
<!-- {% include figure.liquid path="assets/img/personal/dragon_transition.gif" class="img-fluid rounded z-depth-1" alt="DRAGON eVTOL transition animation" zoomable=true %} -->

*Transition flight animation — coming soon*

---

## Gain-Scheduled LQR

The airframe is linearized at a series of trim points indexed by airspeed $$V \in [V_{\min},\ V_{\max}]$$:

$$
\dot{x} = A(V)x + B(V)u
$$

At each trim point, an LQR gain $$K(V)$$ is computed by solving:

$$
A(V)^\top P + PA(V) - PB(V)R^{-1}B(V)^\top P + Q = 0, \quad K(V) = R^{-1}B(V)^\top P
$$

Gains are interpolated in real time as a function of measured airspeed, providing continuous stability coverage across the transition corridor.

$$
K_{\text{sched}}(V) = (1 - \alpha(V)) K_{\text{mc}} + \alpha(V) K_{\text{fw}}, \quad \alpha(V) = \frac{V - V_{\min}}{V_{\max} - V_{\min}}
$$

---

## PX4 Firmware Modifications

Standard PX4 supports fixed-wing and multirotor separately with a discrete mode switch. DRAGON required a custom **blended control allocator** that simultaneously commands rotors and control surfaces during transition, with authority split scheduled by airspeed.

Key firmware changes (C++):
```cpp
// Blended control allocation during transition
float alpha = get_transition_factor(airspeed_m_s);

// Blend rotor and surface commands
actuator_out.rotor   = alpha * mc_out.rotor + (1.0f - alpha) * fw_out.rotor_trim;
actuator_out.aileron = (1.0f - alpha) * fw_out.aileron + alpha * mc_out.aileron_trim;
actuator_out.elevator = fw_out.elevator;
```

---

## Path Planning Through the Transition Corridor

The transition is executed along a planned trajectory that trades altitude for airspeed in a controlled dive, ensuring the vehicle reaches fixed-wing trim speed before full surface authority is engaged. The path is parameterized as:

$$
\begin{bmatrix} \dot{x} \\ \dot{z} \end{bmatrix} = \begin{bmatrix} V\cos\gamma \\ -V\sin\gamma \end{bmatrix}
$$

where $$\gamma$$ is the flight path angle, held constant during the speed transition phase.

---

## Results

| Metric | Value |
|---|---|
| Transition airspeed range | 8 – 18 m/s |
| Altitude loss during transition | < 4 m |
| Attitude deviation (max) | < 8° |
| Time to complete transition | ~4 s |

The platform successfully completed the transition corridor and recovered to stable multirotor hover, marking the first successful demonstration of this maneuver on the DRAGON airframe.
