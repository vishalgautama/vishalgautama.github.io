---
layout: page
title: Physics-based eVTOL Propulsion & Aerodynamic Models
description: MATLAB state-space propulsion models, motor torque-speed curves, battery discharge models, and coupled airframe aerodynamics for eVTOL control law development
importance: 3
category: industry
---

Developed a suite of physics-based plant models in MATLAB that replaced empirical estimates as the design authority for eVTOL development at VTOL Aviation India.

---

## Motor Model

The brushless DC motor is modeled using its electrical and mechanical dynamics:

$$
L \frac{di}{dt} = V_{\text{in}} - iR - k_e \omega_m
$$

$$
J_m \dot{\omega}_m = k_t i - \tau_{\text{load}} - b\omega_m
$$

where $$k_e$$ is the back-EMF constant, $$k_t$$ the torque constant, $$b$$ the viscous damping, and $$\tau_{\text{load}}$$ the propeller torque. At steady state this reduces to the torque-speed curve:

$$
\omega_m = \frac{V_{\text{in}} - iR}{k_e}, \quad \tau = k_t i
$$

Curves were parameterized against test stand data across operating voltage and temperature.

---

## Propeller (Blade-Element Momentum Theory)

Thrust and torque are computed by integrating elemental forces along the blade span:

$$
dT = \frac{1}{2} \rho c(r) \left( C_L \cos\phi - C_D \sin\phi \right) V_r^2 \, dr
$$

$$
dQ = \frac{1}{2} \rho c(r) \left( C_D \cos\phi + C_L \sin\phi \right) V_r^2 \, r \, dr
$$

where $$\phi$$ is the inflow angle, $$V_r$$ the resultant velocity at each blade element, and $$c(r)$$ the chord distribution. Integrated thrust and torque coefficients $$C_T(\lambda),\ C_Q(\lambda)$$ are tabulated as functions of advance ratio $$\lambda = V_\infty / (nD)$$.

---

## Battery Discharge Model

A modified Peukert model captures voltage sag under load:

$$
V_{\text{batt}}(t) = V_{\text{oc}}(\text{SoC}) - i(t) \cdot R_{\text{int}}(\text{SoC}, T)
$$

State of charge evolves as:

$$
\dot{\text{SoC}} = -\frac{i(t)}{C_{\text{rated}}}
$$

Internal resistance $$R_{\text{int}}$$ is mapped as a 2D lookup over SoC and temperature, fit to discharge curves measured on the test stand.

---

## Calibration Against Test Data

```python
import numpy as np
from scipy.optimize import curve_fit

def motor_model(V_omega, k_e, k_t, R, b):
    V_in, omega = V_omega
    i = (V_in - k_e * omega) / R
    torque = k_t * i - b * omega
    return torque

# Fit model to test stand measurements
popt, pcov = curve_fit(
    motor_model,
    (V_in_data, omega_data),
    torque_data,
    p0=[0.01, 0.01, 0.1, 1e-4]
)
k_e, k_t, R, b = popt
```

Prediction error after calibration was reduced to **within 3%** across the full operating envelope.

---

## Flight Envelope Analysis

Operating points were established at 50+ trim configurations varying payload, battery state, and altitude. Each trim point yielded linearized state-space matrices used for gain scheduling:

$$
\dot{x} = A(\xi) x + B(\xi) u, \quad \xi = [\text{payload},\ \text{SoC},\ h]
$$

Eigenvalue analysis at each operating point confirmed closed-loop stability before flight test at that configuration.

<!-- GIF placeholder -->
<!-- {% include figure.liquid path="assets/img/personal/envelope_analysis.gif" class="img-fluid rounded z-depth-1" alt="Flight envelope analysis" zoomable=true %} -->
