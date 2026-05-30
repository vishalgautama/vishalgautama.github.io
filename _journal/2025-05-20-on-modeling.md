---
date: 2025-05-20 08:30:00-0400
---

Been thinking about the gap between simulation and hardware lately. The 6-DOF model matches flight data well on attitude but there is still something off in the translational response at high roll angles. My instinct is the propeller inflow model — BEM assumes uniform inflow and that breaks down when the disk is tilted. Worth revisiting the skewed wake correction.
