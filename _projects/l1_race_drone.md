---
layout: page
title: L1 Adaptive Control — Race Drone Testbed
description: Custom race drone built as a high-bandwidth hardware testbed for validating L1 adaptive augmentation on a real agile platform
img: assets/projects/L1_Adaptive_fault_tolerant_controller_implementation.gif
importance: 4
category: research
---

Built a custom race drone from scratch as a hardware testbed for validating L1 adaptive control on a highly agile, lightweight platform with significant model uncertainty.

<div class="row mt-3">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/personal/parts_scattered_for_l1_adaptive_controller_race_drone.jpeg" class="img-fluid rounded z-depth-1" alt="Race drone components" zoomable=true %}
    <p class="text-center mt-1" style="font-size: 0.85rem;">Components selected and staged for assembly</p>
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/personal/parts_assembeled_for_l1_adaptive_controller_race_drone.jpeg" class="img-fluid rounded z-depth-1" alt="Race drone assembled" zoomable=true %}
    <p class="text-center mt-1" style="font-size: 0.85rem;">Platform assembled and ready for integration</p>
  </div>
</div>

---

## Motivation

Race-class quadrotors are an ideal testbed for adaptive control research — they operate at high bandwidth, have poorly characterized aerodynamic interactions between rotors at high speed, and experience significant gyroscopic and inertial cross-coupling during aggressive maneuvers. These are exactly the conditions where L1 adaptive augmentation provides the most benefit over a fixed-gain baseline.

---

## L1 Adaptive Architecture

The L1 controller augments a baseline LQR attitude controller. A state predictor runs in parallel at high rate:

$$
\dot{\hat{x}} = A_m \hat{x} + B\left(\hat{\sigma}(t) + u\right), \quad \hat{\sigma}(t) = \hat{\theta}^\top \phi(x)
$$

The adaptive law updates at the predictor rate:

$$
\dot{\hat{\theta}} = -\Gamma \text{Proj}(\hat{\theta},\ \tilde{x}^\top P B \phi(x))
$$

where $$\tilde{x} = \hat{x} - x$$ is the prediction error and $$\Gamma > 0$$ is the adaptation gain. A low-pass filter $$C(s)$$ on the adaptive signal provides the bandwidth separation that guarantees robustness:

$$
u_{\text{ad}}(s) = C(s)\hat{\sigma}(s), \quad \|C(s)\|_{\mathcal{L}_1} < \frac{1}{L}
$$

The $$\mathcal{L}_1$$ gain condition bounds the transient response and ensures the adaptive loop does not destabilize the plant.

---

## Platform Specifications

| Parameter | Value |
|---|---|
| Frame size | 5" race class |
| Flight controller | Custom (STM32-based) |
| Motor KV | High-KV for agile response |
| Control loop rate | 1 kHz inner loop |
| Adaptive predictor rate | 1 kHz |

<!-- GIF placeholder: add flight footage when available -->
<!-- {% include figure.liquid path="assets/img/personal/l1_race_drone_flight.gif" class="img-fluid rounded z-depth-1" alt="L1 race drone flight" zoomable=true %} -->
