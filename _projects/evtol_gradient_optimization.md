---
layout: page
title: Gradient Descent eVTOL Design Optimization
description: Custom gradient descent with penalty method and BEMT-based propulsion model to minimize takeoff weight over wing area, rotor count, diameter, pitch, and cruise velocity
importance: 2
category: research
related_publications: false
---

<a href="/assets/projects/Gradient_descent_EVTOL_Optimization.pdf" target="_blank" class="btn btn-sm z-depth-1" role="button" style="background-color: var(--global-theme-color); color: white; margin-bottom: 1.5rem;">
  <i class="fa-regular fa-file-pdf"></i> &nbsp; View Full Report
</a>

Developed a custom gradient descent optimizer with penalty-based constraint enforcement to minimize the total takeoff weight of an eVTOL aircraft. Thrust and power coefficients are computed in the loop using a **Blade Element Momentum Theory (BEMT)** propulsion model, coupling aerodynamics directly into the optimization.

---

## Design Variables

The optimizer searches over five design variables:

| Variable | Description | Bounds |
|---|---|---|
| $$S$$ | Wing area [m²] | [6, 20] |
| $$N$$ | Number of rotors | [4, 12] |
| $$D$$ | Rotor diameter [m] | [0.3, 0.7] |
| $$\theta$$ | Blade collective pitch [deg] | [5, 20] |
| $$V$$ | Cruise velocity [m/s] | [25, 60] |

---

## Objective Function

Minimize total aircraft takeoff weight, accounting for structural mass, rotor/motor mass, and battery mass:

$$
W_{\text{TO}} = g \left( m_{\text{payload}} + m_{\text{structure}} + m_{\text{rotors}} + m_{\text{battery}} \right)
$$

where:

$$
m_{\text{structure}} = m_0 + c_s S^\alpha, \quad m_{\text{rotors}} = N(c_1 D^\beta \theta^\gamma + m_{\text{motor}})
$$

Battery mass is derived from the total mission energy requirement (hover + cruise + 20% reserve):

$$
m_{\text{battery}} = \frac{E_{\text{hover}} + E_{\text{cruise}}}{E_b} \cdot (1 + f_{\text{reserve}})
$$

---

## BEMT Propulsion Model

Thrust and power coefficients are computed at each optimizer iteration by integrating blade element forces along the rotor span:

$$
dT = B \left( dL \cos\phi - dD \sin\phi \right) dr, \quad dQ = B \cdot r \left( dL \sin\phi + dD \cos\phi \right) dr
$$

where $$\phi = \arctan(v_i / \Omega r)$$ is the local inflow angle and lift/drag coefficients are interpolated from airfoil tables. Non-dimensionalized:

$$
C_T = \frac{T}{\rho n^2 D^4}, \quad C_P = \frac{2\pi n Q}{\rho n^3 D^5}
$$

The rotor speed $$n$$ is iterated to satisfy hover thrust: $$n = \sqrt{W / (N C_T \rho D^4)}$$.

---

## Constraint Set

Six inequality constraints enforced via penalty method ($$f_{\text{pen}} = f + R \sum \max(0, c_i)^2$$):

1. Hover thrust sufficient to support takeoff weight
2. Battery energy capacity meets mission requirement
3. Rotor layout fits within wing span
4. Rotor tip speed $$< 0.8 \times$$ speed of sound (Mach limit)
5. Cruise speed maintains 30% stall margin
6. Wing loading $$\leq 600 \text{ N/m}^2$$

---

## Optimizer

Gradient computed via forward finite difference ($$h = 10^{-6}$$), bounds enforced by projection:

```matlab
x_new = x - alpha * grad;
x_new = max(min(x_new, ub), lb);
```

Convergence checked on the L2 norm of the design update: $$\|x_{\text{new}} - x\| < 10^{-4}$$.

---

## Results

**Optimized design (200 iterations):**

| Parameter | Value |
|---|---|
| Wing area $$S$$ | 20.00 m² |
| Number of rotors $$N$$ | 6 |
| Rotor diameter $$D$$ | 0.70 m |
| Blade pitch $$\theta$$ | 5.00° |
| Cruise velocity $$V$$ | 60.00 m/s |
| **Total takeoff weight** | **770.76 kg** |
| Thrust coefficient $$C_T$$ | 0.0849 |
| Power coefficient $$C_P$$ | 0.0560 |

Design space exploration via 2D contour slices (wing area vs. rotor count, wing area vs. diameter, diameter vs. cruise velocity) confirmed the optimizer located the correct region of the feasible set.
