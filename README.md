# MT3608 Paralelization Experiment

## Overview
This project aims to **develop and validate** a DC-DC boost module composed of **three MT3608 converters in parallel**, designed to **step up voltage to 5 V** with a **target output current of up to 6 A**.

The goal is to evaluate whether multiple MT3608 converters can operate reliably in parallel while maintaining voltage stability, current sharing, and thermal safety.

---

## Objectives
- Design a parallel configuration using **three MT3608 step-up converters**.
- Achieve an **output voltage of 5 V** and up to **6 A total output current**.
- Evaluate **load balancing** and **thermal performance** between converters.
- Analyze the **limitations and failure modes** of MT3608 parallelization.

---

## Current Status
> The project is in an experimental stage.  
> There is **no current validation** that the configuration can achieve the specified current or stability requirements.

Prototype testing and measurement phases are ongoing. Future updates will include:
- Schematics and PCB layout
- Test data (efficiency, ripple, current sharing)
- Thermal analysis

---

## Technical Information
- **IC:** MT3608 (AEROSEMI)
- **Input voltage:** 2 V – 5 V
- **Target output:** 5 V / 6 A (shared)
- **Switching frequency:** 1.2 MHz
- **Topology:** Parallel step-up (boost) converters
- **Package:** SOT-23-6L

---

## License
This project is licensed under the  
**Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)**.

See the [LICENSE](./LICENSE) file for details.

---

## Author
**Matheus Renó Torres**  
Hardware Research & Development  
2025  
