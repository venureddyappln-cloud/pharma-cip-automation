# pharma-cip-automation
# GAMP 5-Compliant Automated Clean-in-Place (CIP) Control Package
### Siemens S7-1500 System Architecture | ISA-88 Batch Control Standard

This repository contains an advanced, industry-grade automation engineering package for a pharmaceutical **Clean-in-Place (CIP)** sanitization skid. The project bridges physical instrumentation processing, interlocking fail-safe mechanics, and strict regulatory validation frameworks required for GxP manufacturing facilities.

---

## 🛠️ Repository Architecture & Engineering Assets
The structure of this project mirrors a formal production-ready automation deployment package:

*   📂 **`plc/hardware-config/io_schedule.csv`**: Complete physical I/O allocation tracking sheet with S7 memory offsets, loop signal ranges (4–20 mA / 24 VDC), and GxP validation thresholds.
*   📂 **`plc/exports/CIP_Sequence.scl`**: Production-ready Siemens S7 Structured Control Language (SCL) core engine managing the step matrix, timers, and active safety loops.
*   📂 **`docs/FDS_CIP_System.md`**: Functional Design Specification outlining thermal invalidation constraints and dynamic quality-driven boundary transitions.

---

## 🏗️ System Operational Design & Logic Flow
The automation logic executes an automated, sequential sanitization cycle mapped strictly to the **ISA-88 Batch Control State Model**:

```text
[ STEP 0: IDLE ] 
       │
       ▼ (Operator presses Start Push Button & Safety Interlocks verify OK)
[ STEP 1: PRE-RINSE ] ────► Opens Water Valve (V_102) & Starts Pump (P_101) for 5 Mins
       │
       ▼ (Timer Expires)
[ STEP 2: CHEMICAL WASH ] ► Opens Acid Valve (V_101). Recirculates at Hot Temp (>= 80.0°C)
       │
       ▼ (Timer Expires & Temperature Validated)
[ STEP 3: PURITY FLUSH ] ─► Flushes lines with Purified Water until Conductivity < 1.5 µS/cm
       │
       ▼ (Dynamic Purity Threshold Achieved)
[ STEP 4: COMPLETE ] ────► Safely isolates all field actuators and resets loop to Idle
```

---

## 🔒 Hardwired Safety Interlocks & GxP Invalidation
Unlike standard IT or commercial software environments, this industrial package treats process safety and product integrity as primary software interlocks:

1.  **Vessel Overfill Protection (`LS_101`):** A physical tuning fork level switch acts as a hardwired input constraint. If a high level is reached, the system immediately cuts control signals to all pumps and valves, driving the plant into a safe, non-hazardous shutdown state.
2.  **Thermal Invalidation Guard (`TE_201`):** During the active Chemical Sanitization loop (`Step 2`), the logic continuously scans the thermal boundary. If fluid temperature drifts below **80.0°C** for more than 60 seconds, a validation failure flag is triggered. The system aborts the execution matrix and drops into an explicit lockout state (`Step 99`) to prevent batch cross-contamination.
3.  **Dynamic Quality Transition (`CE_202`):** Rather than blindly relying on a fixed rinse timer, the sequence utilizes a toroidal conductivity sensor. `Step 3` cannot transition until chemical residue is thoroughly evacuated and conductivity consistently scales below **1.5 µS/cm**.

---

## 🚀 Key Transferable Skills Demonstrated
*   **Industrial Programming Standards:** Advanced knowledge of IEC 61131-3 SCL/Structured Text design blocks.
*   **Regulatory Engineering Matrixing:** Mapping raw field instrument inputs straight to physical memory tables using deterministic, validation-ready data structures.
*   **GAMP 5 V-Model Lifecycle:** Proving that functional automation code is explicitly bound to operational requirements and lifecycle definitions.
