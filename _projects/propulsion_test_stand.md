---
layout: page
title: 1 kHz Multi-channel Propulsion Test Stand
description: NI DAQ-based dynamometer capturing thrust, torque, RPM, phase current, and winding temperature at fidelity exceeding commercial dynamometers at the required scale
img: assets/img/personal/propulsion_test_stand.jpg
importance: 2
category: industry
---

Built a high-fidelity propulsion test stand from scratch at VTOL Aviation India to support physics-based model development for eVTOL systems.

**Hardware**

- NI DAQ hardware with 1 kHz multi-channel data acquisition
- Simultaneous measurement of: thrust, torque, RPM, phase current, winding temperature
- Measurement fidelity exceeds commercial dynamometers available at the required scale

**Software**

- Python and C++ post-processing pipelines to systematically calibrate MATLAB propulsion models against test stand data
- Reduced model prediction error to **within 3%** across the full operating envelope

**Impact**

The calibrated models replaced empirical estimates as the design authority for eVTOL control law development, enabling physics-based gain scheduling and accurate simulation-to-flight correlation.

---

<div class="row mt-3">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/personal/propulsion_test_stand.jpg" class="img-fluid rounded z-depth-1" alt="Large propulsion test stand" zoomable=true %}
    <p class="text-center mt-1" style="font-size: 0.85rem;">Large propulsion test setup</p>
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/personal/small_test_stand_high_fedility.jpg" class="img-fluid rounded z-depth-1" alt="Small high-fidelity test stand" zoomable=true %}
    <p class="text-center mt-1" style="font-size: 0.85rem;">Small high-fidelity propulsion test setup</p>
  </div>
</div>

<div class="row mt-3">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/personal/engine_test_bench.PNG" class="img-fluid rounded z-depth-1" alt="Engine test bench" zoomable=true %}
    <p class="text-center mt-1" style="font-size: 0.85rem;">Engine test bench setup</p>
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/personal/coaxial_setup.png" class="img-fluid rounded z-depth-1" alt="Coaxial propulsion setup" zoomable=true %}
    <p class="text-center mt-1" style="font-size: 0.85rem;">Coaxial propulsion setup</p>
  </div>
</div>

<div class="row mt-3">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/personal/dynamic_load_test_bench.png" class="img-fluid rounded z-depth-1" alt="Dynamic load test bench" zoomable=true %}
    <p class="text-center mt-1" style="font-size: 0.85rem;">Dynamic load test bench</p>
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/personal/drop_test_bench.png" class="img-fluid rounded z-depth-1" alt="Drop test bench" zoomable=true %}
    <p class="text-center mt-1" style="font-size: 0.85rem;">Drop test bench</p>
  </div>
</div>
