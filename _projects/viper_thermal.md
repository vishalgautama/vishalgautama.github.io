---
layout: page
title: Rolls-Royce Viper Thermal Failure Investigation
description: Aero-thermal failure analysis of the central bearing, identifying heat transfer breakdown at the bearing race as the primary failure driver
importance: 1
category: other
---

Led a thermal failure investigation of the Rolls-Royce Viper turbojet's central bearing as project lead during a research internship at the Indian Air Force Base Repair Depot, Kanpur.

---

## Background

The Rolls-Royce Viper is a single-spool axial turbojet used in the HAL HJT-16 Kiran trainer aircraft. The central bearing supports the compressor-turbine shaft and operates in a high-temperature, high-speed environment where heat management is critical. Recurring thermal failures at the bearing race prompted the investigation.

---

## Failure Mode Analysis

The failure was characterized by:
- Discoloration and micro-cracking at the inner bearing race
- Evidence of lubricant coking near the oil jet entry
- Material softening consistent with sustained overtemperature

The root cause was identified as a **breakdown in convective heat transfer** at the bearing race, driven by lubricant flow starvation at elevated shaft speeds.

---

## Thermal Analysis

The heat balance at the bearing was modeled as:

$$
Q_{\text{gen}} = Q_{\text{conv}} + Q_{\text{cond}}
$$

Frictional heat generation in the bearing:

$$
Q_{\text{gen}} = \mu \cdot F_n \cdot \omega \cdot r
$$

Convective removal by the oil film:

$$
Q_{\text{conv}} = h A_s (T_{\text{race}} - T_{\text{oil}})
$$

The convective coefficient $$h$$ drops sharply when the oil film thickness falls below a critical value, causing $$Q_{\text{conv}} < Q_{\text{gen}}$$ and driving the race temperature above the material's continuous operating limit.

---

## Gas Turbine Thermodynamic Context

The bearing operates in a region of the engine between compressor delivery and turbine entry. The local temperature $$T_{2.5}$$ was estimated using isentropic compression relations:

$$
T_{2.5} = T_1 \left(\frac{P_{2.5}}{P_1}\right)^{\frac{\gamma-1}{\gamma}}
$$

At design point conditions (OAT 35°C), $$T_{2.5}$$ was computed and used to establish the thermal boundary condition at the bearing housing.

---

## Corrective Thresholds

Operating limits were derived to prevent recurrence:

| Parameter | Limit |
|---|---|
| Maximum bearing race temperature | < 180°C continuous |
| Minimum oil flow rate at max N1 | > 0.8 L/min |
| Maximum shaft speed before enhanced cooling required | 95% N1 |

Pressure and temperature thresholds at the oil supply jet were established as the primary inspectable/monitorable parameters in maintenance procedures.

---

## Engines Studied

During the internship, overhaul and maintenance procedures were studied for:
- **Tumansky R-29** — afterburning turbojet (MiG-27)
- **Snecma M53** — augmented turbofan (Mirage 2000)
- **Rolls-Royce Viper 11/535** — turbojet (Kiran Mk.I/II)

Exposure to the overhaul cycle for these engines — from disassembly through dimensional inspection to reassembly — built the propulsion hardware intuition that directly informs the physics-based simulation models developed in later work.
