# **Version 0.1 — Test Report and Analysis**

## **1. Objective**

Evaluate the behavior of the parallel MT3608 boost converter architecture in an unloaded condition and identify whether all regulators operate simultaneously or if one device dominates the regulation loop.

---

## **2. Test Setup**

* **Input voltage:** 3.7 V
* **Output load:** No load (open circuit)
* **Expected output:** ~5 V
* **Measurement tools:**

  * Oscilloscope (probe on SW pin of each MT3608)
* **Configuration:**

  * Three MT3608 units connected in parallel
  * Shared FB node and identical resistor network
  * Shared output capacitor bank

---

## **3. Test Procedures and Observations**

### **3.1 Initial Power-Up**

* A voltage of **3.7 V** was applied to the input of the system.
* With no load, the output voltage measured **4.92 V**.

### **3.2 Oscilloscope Analysis (SW Pin)**

* The oscilloscope probe was placed on the **SW pin of each MT3608**.
* **Only one IC** showed the normal switching waveform expected from a functioning MT3608.
* The other two devices remained inactive (standby), indicating that only a **single MT3608 was regulating the output**, while the others were not contributing.

### **3.3 Feedback Trace Verification**

* The MT3608 ICs were swapped in their positions to verify whether a PCB feedback-trace issue was preventing regulation.
* After swapping, **the same IC continued to regulate**, regardless of its physical position.
* This eliminates the possibility of a feedback routing or soldering issue on the PCB.

### **3.4 Removal of the Regulating IC**

* The functioning IC was removed from the circuit to observe the behavior of the remaining two devices.
* After removal:

  * A different MT3608 began regulating normally.
  * The third MT3608 continued in standby, unchanged.
  * The new output voltage became **4.83 V**.

---

## **4. Interpretation**

The behavior strongly suggests **mismatch in the internal feedback reference (FB offset)** between individual MT3608 units.
Even within the same manufacturing lot, boost converters can have small variations in the internal 0.6 V reference used for regulation.

Because of this:

* The MT3608 that naturally regulates to the **highest output voltage** becomes the dominant controller.
* It maintains VOUT on its own.
* The other ICs detect that the output voltage is already above their regulation threshold and therefore enter a **standby / PFM state**, never turning on.
* This prevents proper current sharing and makes true parallel operation impossible under the current architecture.

---

## **5. Conclusion**

* The prototype assumed that all MT3608 devices would have identical regulation points; however, real-world variations in **feedback reference voltage** cause each IC to regulate at slightly different output voltages.
* **Only the MT3608 with the highest internal reference (highest regulated VOUT)** becomes active, while the others remain idle.
* This confirms that the current parallel architecture is **not viable** for ensuring simultaneous operation of multiple MT3608 regulators.
* A new design approach is required — one that:

  * Does **not** rely on identical reference voltages,
  * Provides **true current sharing**,
  * Or uses a **single master regulator** followed by distribution elements.

---

## **6. Recommended Next Steps**

To enable proper **parallel operation of multiple MT3608 regulators**, the design must account for the natural variation in the internal 0.6 V feedback reference of each device. Since each MT3608 regulates to a slightly different voltage, only the unit with the highest reference becomes active while the others remain in standby.
A new approach must ensure that all regulators can share current simultaneously.

Recommended next steps:

* Implement a **current-sharing method** between the MT3608 outputs to prevent one IC from dominating the regulation loop.
* Add small **ballast resistors** on each MT3608 output to equalize voltage differences and encourage parallel contribution.
* Isolate each feedback network in a way that minimizes interaction while still maintaining a common output target.
* Evaluate the effect of slightly adjusting the resistor divider of each MT3608 to compensate for reference mismatch.
* Test dynamic behavior under load to understand how the converters transition between PWM and PFM modes when operating together.
* Characterize the feedback reference variation of each MT3608 to define equalization strategies.

This approach retains the goal of **making the MT3608 chips operate in parallel**, avoiding standby dominance and ensuring stable current sharing.
