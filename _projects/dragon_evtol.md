---
layout: page
title: DRAGON eVTOL — Fixed-Wing to Multirotor Transition
description: PX4 firmware modification, gain-scheduled LQR/PID transition control laws, and first demonstration of stable fixed-wing-to-multirotor flight
importance: 2
category: research
---

Took GNC ownership of the DRAGON eVTOL platform at Virginia Tech's Nonlinear Systems Laboratory.

**What was built**

- Modified PX4 autopilot firmware in C++ to support the custom DRAGON airframe
- Designed gain-scheduled LQR and PID transition control laws with path-planning logic to manage the aerodynamic and inertial coupling during mode transition
- Demonstrated the platform's **first stable fixed-wing-to-multirotor transition** in flight

**Technical challenges**

Transitioning between fixed-wing and multirotor flight involves rapidly shifting control authority across control surfaces and rotors while the aerodynamic regime changes. The gain-scheduled approach allows continuous stability margin coverage across the transition corridor without discrete mode-switching artifacts.
